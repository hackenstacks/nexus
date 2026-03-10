# NeXuS Security Stack

**Sane • Simple • Secure** — Multi-layer intrusion detection and prevention.

## Architecture Overview

```
Internet
   │
   ▼
[UFW firewall] — blocks + logs [UFW BLOCK] prefix
   │
   ▼
[klogd] — reads kernel ring buffer → syslogd → /var/log/messages
   │
   ├──► [PSAD] — port scan detection, danger levels 1-5, auto-block via iptables
   │       └──► [fwsnort] — Snort IDS rules translated to iptables (29,600+ signatures)
   │
   └──► [fail2ban] — brute-force detection, bans repeat offenders
```

---

## PSAD — Port Scan Attack Detector

PSAD reads iptables/UFW logs and classifies port scans by danger level (1–5).

### Prerequisites — Logging Chain

PSAD only sees what reaches syslog. The full chain must be intact:

1. **UFW must log with the right prefix** (`/etc/ufw/before.rules`):
   ```
   -A ufw-before-input -j LOG --log-prefix "[UFW BLOCK] " --log-level 4
   -A ufw-before-forward -j LOG --log-prefix "[UFW BLOCK] " --log-level 4
   ```

2. **klogd must be running** (reads kernel ring buffer → syslogd):
   ```bash
   doas rc-update add klogd boot
   doas rc-service klogd start
   ```

3. **psad.conf must point at the right daemon and prefix** (`/etc/psad/psad.conf`):
   ```
   SYSLOG_DAEMON           syslogd;
   IPT_SYSLOG_FILE         /var/log/messages;
   IPT_PREFIX              [UFW BLOCK];
   ```

### Danger Levels

| Level | Meaning | Action |
|-------|---------|--------|
| 1 | Low-risk probe | Log only |
| 2 | Moderate scan | Log only |
| 3 | Active scan (>50 ports) | Email alert |
| 4 | Aggressive scan | Block + email |
| 5 | Full port sweep | Block + email + psadwatchd escalation |

### Daily Operations

```bash
# Check current status
doas psad --Status

# Show blocked IPs
doas psad --Status | grep "Danger level [3-5]"

# Flush all blocks
doas psad --flush-blocked-ip all

# Force signature reload after fwsnort update
doas psad -H

# Update signatures + reload
doas psad --sig-update && doas psad -H

# Live log watching
tail -f /var/log/psad/status.out
```

### Config Reference (`/etc/psad/psad.conf`)

| Key | Value | Notes |
|-----|-------|-------|
| `SYSLOG_DAEMON` | `syslogd` | NOT metalog (even if metalog.conf exists) |
| `IPT_SYSLOG_FILE` | `/var/log/messages` | Where busybox syslogd writes |
| `IPT_PREFIX` | `[UFW BLOCK]` | Must match UFW log prefix exactly |
| `ENABLE_AUTO_IDS` | `Y` | Enable automatic blocking |
| `AUTO_IDS_DANGER_LEVEL` | `3` | Block at level 3+ |
| `EMAIL_ALERT_DANGER_LEVEL` | `3` | Email at level 3+ |

---

## fwsnort — Snort Rules → iptables

fwsnort translates Snort IDS signatures into iptables rules, giving PSAD application-layer awareness.

### nftables Compatibility Fix

Alpine's `iptables-nft` (v1.8.11) rejects the old `!port` negation syntax. fwsnort generates ~8 rules with this pattern that must be stripped before applying.

**Fix script** (`~/scripts/nexus-fix-fwsnort-nftables.sh`):
```bash
#!/bin/sh
# Strip incompatible !port rules and apply the rest
sed -i '/dports.*,![0-9]/d; /sports.*,![0-9]/d' /var/lib/fwsnort/fwsnort_iptcmds.sh
IPTABLES=iptables sh /var/lib/fwsnort/fwsnort_iptcmds.sh
doas psad -H
echo "fwsnort rules applied"
```

### Update Workflow

```bash
# Full update (downloads new rules, regenerates, applies, reloads PSAD)
doas sh ~/scripts/nexus-update-fwsnort.sh

# Manual steps:
doas fwsnort --update-rules   # download latest Snort community rules
doas fwsnort                  # translate to iptables
doas sh ~/scripts/nexus-fix-fwsnort-nftables.sh  # strip bad rules + apply
doas psad --sig-update && doas psad -H            # reload PSAD
```

### Verify Rules Loaded

```bash
# Count fwsnort rules in iptables
doas iptables -L | grep -c FWSNORT

# Should show ~29,600+ rules
```

---

## fail2ban — Brute Force Protection

fail2ban monitors auth logs and bans IPs that exceed threshold.

### Alpine-Specific Config

Alpine uses `busybox syslogd` writing to `/var/log/messages` — NOT `/var/log/auth.log` or `/var/log/sshd_log`.

**`/etc/fail2ban/paths-overrides.local`:**
```ini
[DEFAULT]
sshd_log = /var/log/messages
syslog_authpriv = /var/log/messages
```

**`/etc/fail2ban/jail.d/alpine-ssh.conf`:**
```ini
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/messages
maxretry = 5
bantime = 3600
findtime = 600

[sshd-ddos]
enabled = true
port = ssh
filter = sshd-ddos
logpath = /var/log/messages
maxretry = 5
bantime = 3600
```

### Operations

```bash
# Check status (requires doas — socket is root-only)
doas fail2ban-client status
doas fail2ban-client status sshd

# Unban an IP
doas fail2ban-client set sshd unbanip 1.2.3.4

# Test a filter against log
doas fail2ban-regex /var/log/messages /etc/fail2ban/filter.d/sshd.conf

# Restart after config changes
doas rc-service fail2ban restart
```

---

## AppArmor (Pending Activation)

AppArmor is installed but NOT active — the kernel LSM list is missing it.

**Current kernel LSMs:** `lockdown,capability,landlock,yama`

**To activate**, add to `/boot/extlinux.conf` APPEND line:
```
lsm=landlock,yama,apparmor
```

Then reboot. Verify with:
```bash
cat /sys/kernel/security/lsm
aa-status
```

**Planned profiles** (complain mode first, then enforce):
- `tor` — `/usr/bin/tor`
- `privoxy` — `/usr/sbin/privoxy`
- `psad` — `/usr/sbin/psad`
- `nginx` — `/usr/sbin/nginx`

---

## Port Knocking (Queued)

`knockd` — SSH access via sequence knock, zero open ports visible.

**Planned sequence:** Three UDP ports → triggers temporary iptables rule → SSH accessible for 10s.

Status: **Not yet configured.** Install: `doas apk add knockd`

---

## Fix Scripts Reference

| Script | Purpose | Run As |
|--------|---------|--------|
| `~/scripts/nexus-fix-psad-logging.sh` | Fix full UFW→klogd→syslogd→PSAD chain | `doas sh` |
| `~/scripts/nexus-fix-fwsnort-nftables.sh` | Strip bad `!port` rules + apply fwsnort | `doas sh` |
| `~/scripts/nexus-update-fwsnort.sh` | Full fwsnort update + PSAD reload | `doas sh` |
| `~/scripts/nexus-fix-fail2ban.sh` | Fix fail2ban logpath for Alpine | `doas sh` |

---

## Security Checklist

```
[ ] klogd running:        rc-service klogd status
[ ] UFW log prefix set:   grep "UFW BLOCK" /etc/ufw/before.rules
[ ] PSAD scanning:        psad --Status | grep "Pkts"
[ ] fwsnort rules loaded: iptables -L | grep -c FWSNORT
[ ] fail2ban active:      doas fail2ban-client status
[ ] AppArmor:             PENDING (extlinux.conf change required)
[ ] Port knocking:        PENDING (knockd not installed)
```
