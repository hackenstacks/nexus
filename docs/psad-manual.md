# PSAD Manual — NeXuS Edition

**Port Scan Attack Detector**
*Sane • Simple • Secure*

---

## What PSAD Does

PSAD monitors firewall log entries in real-time and detects port scans, OS fingerprinting attempts, and known attack signatures. When a host exceeds a danger threshold it can automatically block them via iptables.

**The detection chain on this system:**

```
Network traffic
      ↓
  iptables (UFW managed)
      ↓  [dropped packets logged with "[UFW BLOCK]" prefix]
  klogd  ←── reads /proc/kmsg (kernel ring buffer)
      ↓
  syslogd (busybox) ←── receives kernel messages from klogd
      ↓
  /var/log/messages
      ↓
  PSAD  ←── watches /var/log/messages for [UFW BLOCK] entries
      ↓
  Detection / Auto-block / Logging
```

> **Critical:** All four links in the chain must be working. See [Troubleshooting](#troubleshooting).

---

## This System's Configuration

| Setting | Value |
|---------|-------|
| Config file | `/etc/psad/psad.conf` |
| Log source | `/var/log/messages` |
| Log prefix | `[UFW BLOCK]` |
| Syslog daemon | `syslogd` (busybox) |
| Auto-block | Enabled at Danger Level 5 |
| Signatures | `/etc/psad/signatures` |
| Per-IP data | `/var/log/psad/<ip>/` |
| Status file | `/var/log/psad/status.out` |

---

## Danger Levels

PSAD assigns a danger level (1–5) based on packet count from a single source:

| Level | Packets | Unique Hosts | Auto-block timeout |
|-------|---------|--------------|--------------------|
| DL1   | 5       | 10           | 1 hour             |
| DL2   | 15      | 20           | 1 hour             |
| DL3   | 150     | 50           | 1 hour             |
| DL4   | 1,500   | 100          | 1 hour             |
| DL5   | 10,000  | 500          | **Permanent**      |

Auto-block fires at **DL5** (`AUTO_IDS_DANGER_LEVEL 5`). Lower if you want earlier blocking.

---

## Daily Operations

### Check status
```sh
doas psad --Status
# or read the file directly:
doas cat /var/log/psad/status.out
```

### Check who PSAD is tracking right now
```sh
doas cat /var/log/psad/top_attackers
doas cat /var/log/psad/top_ports
doas cat /var/log/psad/top_sigs
```

### See live firewall log entries PSAD reads
```sh
grep '\[UFW BLOCK\]' /var/log/messages | tail -20
```

### Reload PSAD after config change
```sh
doas psad -H
```

### Reload signatures only
```sh
doas psad --sig-update && doas psad -H
```

### Restart service
```sh
doas rc-service psad restart
```

### Flush auto-blocked IPs
```sh
doas psad --Flush-auto-blocked-ips
```

### Unblock a specific IP
```sh
doas psad --Unblock-ip 192.168.1.x
```

### Force analysis of current logs
```sh
doas psad --analyze-msgs-file /var/log/messages
```

---

## Interpreting Status Output

```
[+] Src IP:     192.168.1.127           ← attacker IP
    Danger:     5 (auto-blocked)         ← danger level
    Packets:    12,453                   ← total packets seen
    TCP ports:  22,80,443,8080...        ← ports probed
    Signatures: SID2023452 (Mirai)...   ← fwsnort signature matches
    First:      Mar 9 21:54             ← scan start time
    Last:       Mar 9 21:55             ← last packet seen
```

Signature matches (`SID:xxxxx`) come from fwsnort-translated Snort rules. No signature match just means it's a raw scan, not a known exploit pattern — still detected and blocked.

---

## Configuration Reference

Key settings in `/etc/psad/psad.conf`:

```
# What log file to watch
IPT_SYSLOG_FILE     /var/log/messages;

# Prefix that identifies firewall log entries
IPT_PREFIX          [UFW BLOCK];

# Syslog daemon in use
SYSLOG_DAEMON       syslogd;

# Auto-block settings
ENABLE_AUTO_IDS         Y;
AUTO_IDS_DANGER_LEVEL   5;    # block at this level

# Danger level thresholds (packets from one IP)
DANGER_LEVEL1       5;
DANGER_LEVEL2       15;
DANGER_LEVEL3       150;
DANGER_LEVEL4       1500;
DANGER_LEVEL5       10000;

# How long scans are remembered
SCAN_TIMEOUT        3600;    # 1 hour

# Alerting (email disabled, file-based only)
ALERTING_METHODS    noemail;
```

### To enable email alerts
```
ALERTING_METHODS        email;
EMAIL_ADDRESSES         you@domain.com;
EMAIL_ALERT_DANGER_LEVEL 4;
```

### To lower auto-block threshold (block earlier)
```
AUTO_IDS_DANGER_LEVEL   3;
```

### To whitelist an IP or network
```
IGNORE_PORTS        NONE;
# Add to /etc/psad/auto_dl:
1.2.3.4/32    0;    # always danger level 0 = never block
192.168.1.0/24  0;
```

---

## Scripts

### Fix the UFW → klogd → syslogd → PSAD logging chain
**[nexus-fix-psad-logging.sh](https://github.com/hackenstacks/nexus/blob/main/scripts/nexus-fix-psad-logging.sh)**

Run once if PSAD stops seeing traffic. Fixes:
- klogd not running (kernel logs never reach syslogd)
- before.rules LOG rules missing `[UFW BLOCK]` prefix
- psad.conf pointing to wrong syslog daemon

```sh
doas sh ~/scripts/nexus-fix-psad-logging.sh
```

---

## Troubleshooting

### PSAD sees no packets

Check each link in the chain:

```sh
# 1. Is klogd running?
ps aux | grep klogd
# Fix: doas rc-service klogd start && doas rc-update add klogd boot

# 2. Are [UFW BLOCK] entries reaching messages?
grep '\[UFW BLOCK\]' /var/log/messages | tail -5
# If empty: UFW LOG rules missing prefix — run nexus-fix-psad-logging.sh

# 3. Is PSAD running?
rc-service psad status

# 4. Trigger a test (from another machine or localhost):
nmap -sS 127.0.0.1
# Then check: grep 'UFW BLOCK' /var/log/messages | tail -5
```

### PSAD detects scans but doesn't auto-block

```sh
# Check danger level of the attacker:
doas cat /var/log/psad/top_attackers

# Auto-block only fires at DL5 (10,000 packets) by default.
# Lower threshold in psad.conf:
AUTO_IDS_DANGER_LEVEL   3;
doas psad -H
```

### psadwatchd not running warning

Not a problem — `psadwatchd` is a watchdog process that restarts PSAD if it crashes. It is not installed as a separate service on Alpine. PSAD functions normally without it.

### PSAD running but /var/log/messages is empty

busybox `syslogd` requires `klogd` to forward kernel messages. They are separate processes:
- `syslogd` — handles userspace logs via `/dev/log` socket
- `klogd` — reads `/proc/kmsg` and forwards kernel messages to syslogd

Both must run. Check: `ps aux | grep -E 'syslogd|klogd'`

### Auto-blocked IP persists after reboot

Auto-block rules are written to iptables which don't persist across reboots by default on Alpine. The blocked IP file is at `/var/log/psad/auto_blocked_iptables` — PSAD re-applies these on startup.

---

## FAQ

**Q: Why does PSAD use so much RAM (~150MB)?**
A: PSAD is a Perl daemon. The Perl interpreter + runtime + signature database loads entirely into memory. This is normal. It cannot be reduced without replacing PSAD.

**Q: What's the difference between PSAD and fail2ban?**
A: fail2ban reacts to failed login attempts in application logs. PSAD detects network-layer scan patterns from firewall logs. They complement each other — fail2ban handles brute force, PSAD handles reconnaissance.

**Q: Will PSAD block legitimate scanners like Shodan/Censys?**
A: Yes, if they probe enough ports to hit DL5. That is intentional — NeXuS does not want to be indexed. If you need to whitelist a known scanner, add it to `/etc/psad/auto_dl`.

**Q: Can PSAD detect UDP scans?**
A: Yes. The `/var/log/messages` entry from the test scan showed UDP packets being logged and PSAD tracks them separately from TCP.

**Q: Does PSAD work with IPv6?**
A: Yes — `ENABLE_IPV6_DETECTION Y` is set. IPv6 attackers appear in `/var/log/psad/` under their IPv6 address. Confirmed working: two IPv6 addresses were detected during the initial test.

**Q: How do I see what fwsnort signatures PSAD matched?**
A: `doas cat /var/log/psad/top_sigs` — lists Snort SIDs matched with hit counts.

**Q: How often does PSAD check the log?**
A: Every 5 seconds (`CHECK_INTERVAL 5`).

**Q: How long does PSAD remember a scan?**
A: 1 hour (`SCAN_TIMEOUT 3600`). After that, the packet counter resets and the danger level drops. DL5 auto-blocks are permanent until manually flushed or PSAD is restarted without the block file.

---

## Key Files

```
/etc/psad/psad.conf          — main configuration
/etc/psad/signatures         — Snort-compatible detection signatures
/etc/psad/auto_dl            — per-IP danger level overrides (whitelists)
/var/log/psad/               — all PSAD data
/var/log/psad/status.out     — last status output (root readable only)
/var/log/psad/top_attackers  — current top threats
/var/log/psad/top_ports      — most scanned ports
/var/log/psad/top_sigs       — matched Snort signatures
/var/log/psad/<ip>/          — per-attacker data directory
/var/log/messages            — log source PSAD reads
```

---

## Service Commands

```sh
doas rc-service psad start
doas rc-service psad stop
doas rc-service psad restart
doas rc-update add psad default     # enable on boot
doas rc-update del psad default     # disable on boot
```

---

*NeXuS — Sane • Simple • Secure*
*Last updated: 2026-03-09*
