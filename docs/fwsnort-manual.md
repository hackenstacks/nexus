# fwsnort Manual — NeXuS Edition

**Firewall Snort — Translates Snort Rules into iptables Rules**
*Sane • Simple • Secure*

---

## What fwsnort Does

fwsnort translates Snort intrusion detection rules into iptables rules using the `iptables string match` module. Where PSAD detects *that* a scan is happening, fwsnort detects *what kind of attack* is happening — matching payload signatures of known malware, exploits, trojans, and C2 beacons.

**The detection chain:**

```
Network packet arrives
      ↓
  fwsnort iptables rules (FWSNORT_INPUT/OUTPUT/FORWARD chains)
      ↓  [payload matches Snort signature]
  LOG with prefix "[NNNN] SIDxxxxxxx ESTAB"
      ↓
  /var/log/messages  (via klogd → syslogd)
      ↓
  PSAD reads the ESTAB-prefixed entries
      ↓
  Signature match recorded, auto-block if danger level reached
```

> fwsnort does **detection only** — it logs matches. PSAD does the blocking based on fwsnort's log output.

---

## This System's Configuration

| Item | Detail |
|------|--------|
| Binary | `/usr/sbin/fwsnort` |
| Generated rules (shell) | `/var/lib/fwsnort/fwsnort_iptcmds.sh` (48MB, ~29,672 rules) |
| Generated rules (restore) | `/var/lib/fwsnort/fwsnort.save` (29MB) |
| Rule loader | `/var/lib/fwsnort/fwsnort.sh` |
| Log output | `/var/log/messages` (via syslogd) |
| Log prefix format | `[RULE#] SIDxxxxxxx ESTAB` |
| Snort rules source | Emerging Threats (ET) — auto-updated |
| Daily update job | `/etc/periodic/daily/fwsnort` |
| Update script | `/usr/bin/update-fwsnort` (replaced by NeXuS wrapper) |

---

## How fwsnort + PSAD Work Together

```
fwsnort role:   Signature matching (WHAT is attacking)
PSAD role:      Scan detection + blocking (WHO is attacking)

fwsnort logs:   [3486] SID2023511 ESTAB  ← Snort rule fired
PSAD reads:     ESTAB prefix → records signature match for that IP
PSAD blocks:    IP reaches DL5 → permanent iptables block
```

The `ESTAB` suffix in fwsnort's log prefix is what PSAD uses (`AUTO_BLOCK_REGEX ESTAB` in psad.conf) to identify fwsnort signature matches vs plain scan detections.

---

## The iptables-nft Compatibility Issue

### Problem
fwsnort generates some rules with `!` inside multiport port lists:
```
-m multiport ! --dports 25,!445,!1500
```
This is invalid syntax on `iptables v1.8.11 (nf_tables)` — the nftables backend. It causes `iptables-restore` to fail at that line, rejecting the **entire 29,672-rule ruleset**.

### Scale
Only **8 rules** out of 29,672 have this syntax. They come from Snort rules with complex port negations that fwsnort cannot correctly translate.

### Fix
Strip the 8 bad lines before loading. The affected signatures are removed (they can't be correctly translated anyway) and the remaining 29,664 rules load cleanly.

**[nexus-fix-fwsnort-nftables.sh](https://github.com/hackenstacks/nexus/blob/main/scripts/nexus-fix-fwsnort-nftables.sh)** — strips bad lines, applies rules, signals PSAD.

```sh
doas sh ~/scripts/nexus-fix-fwsnort-nftables.sh
```

---

## Daily Operations

### Apply fwsnort rules after reboot
fwsnort rules are not persistent across reboots. Re-apply with:
```sh
doas sh ~/scripts/nexus-fix-fwsnort-nftables.sh
```

### Update rules + signatures (full refresh)
```sh
doas sh ~/scripts/nexus-update-fwsnort.sh
```
This: updates Snort rules from upstream → regenerates iptables rules → fixes nft syntax → applies → reloads PSAD.

### Check if fwsnort rules are loaded
```sh
doas iptables -L | grep -c FWSNORT
# Should return a large number (hundreds of chains/rules)

doas iptables -L | grep FWSNORT | head -10
# Should show: FWSNORT_INPUT, FWSNORT_OUTPUT, FWSNORT_FORWARD chains
```

### Check for signature matches in logs
```sh
grep 'ESTAB' /var/log/messages | tail -20
# Shows fwsnort signature hits with SID numbers

grep 'SID2023452' /var/log/messages | tail -5
# Check for specific Snort SID
```

### See what signatures PSAD has matched
```sh
doas cat /var/log/psad/top_sigs
```

### Manual rule regeneration (without updating upstream)
```sh
doas /usr/sbin/fwsnort
doas sh ~/scripts/nexus-fix-fwsnort-nftables.sh
```

---

## Update Workflow

The daily cron job at `/etc/periodic/daily/fwsnort` calls `/usr/bin/update-fwsnort`.

**Replace the system update script with the NeXuS wrapper:**
```sh
doas cp ~/scripts/nexus-update-fwsnort.sh /usr/bin/update-fwsnort
```

The NeXuS wrapper adds the iptables-nft fix step that the original script is missing.

**Full update sequence (what nexus-update-fwsnort.sh does):**
```
1. fwsnort --update-rules     → fetch latest Snort/ET rules
2. fwsnort                    → translate rules to iptables commands
3. nexus-fix-fwsnort-nftables → strip 8 bad nft-incompatible lines
4. fwsnort_iptcmds.sh         → apply ~29,664 rules to iptables
5. psad --sig-update          → update PSAD signatures
6. psad -H                    → reload PSAD
```

> **Warning:** Applying fwsnort rules takes 5–10 minutes on this hardware. 29,000+ individual iptables calls. This is normal.

---

## Scripts

### nexus-fix-fwsnort-nftables.sh
**[nexus-fix-fwsnort-nftables.sh](https://github.com/hackenstacks/nexus/blob/main/scripts/nexus-fix-fwsnort-nftables.sh)**

Strips the 8 iptables-nft incompatible lines from `fwsnort_iptcmds.sh` and applies all remaining rules. Run after every reboot or fwsnort regeneration.

```sh
doas sh ~/scripts/nexus-fix-fwsnort-nftables.sh
```

### nexus-update-fwsnort.sh
**[nexus-update-fwsnort.sh](https://github.com/hackenstacks/nexus/blob/main/scripts/nexus-update-fwsnort.sh)**

Full update: upstream rules → regenerate → fix → apply → reload PSAD. Replace `/usr/bin/update-fwsnort` with this.

```sh
doas sh ~/scripts/nexus-update-fwsnort.sh
# Or install permanently:
doas cp ~/scripts/nexus-update-fwsnort.sh /usr/bin/update-fwsnort
```

---

## Understanding fwsnort Log Entries

Example log entry:
```
Mar 9 22:15:33 system kern.warn kernel: [UFW BLOCK] IN=wlan0 SRC=1.2.3.4 DST=192.168.1.137
  [3486] SID2023511 ESTAB ...
```

| Field | Meaning |
|-------|---------|
| `[3486]` | fwsnort rule number |
| `SID2023511` | Snort rule ID — look up at `rules.emergingthreats.net` |
| `ESTAB` | Connection was established (vs SYN-only scan) |
| `SRC=` | Attacker IP |
| `DST=` | Your machine |

PSAD picks up the `ESTAB` prefix and records the SID against the source IP.

---

## Snort Rule Categories in Use

fwsnort pulls **Emerging Threats (ET)** rules covering:

| Category | Examples |
|----------|---------|
| Trojans | Mirai, Gh0st RAT, Emotet, BlackCarat, PlugX |
| Exploit attempts | Redis SSH key injection, MSSQL exploits |
| C2 beacons | APT28/Sednit, Orchard botnet, SideCopy APT |
| Policy violations | IRC P2P, eDonkey, non-standard port usage |
| Scanning | nmap OS detection, service version probes |

---

## Troubleshooting

### fwsnort rules fail to load (iptables-restore error)

```
iptables-restore: invalid port/service '!445' specified
Error occurred at line: 19885
```

This is the known nft compatibility issue. Run:
```sh
doas sh ~/scripts/nexus-fix-fwsnort-nftables.sh
```

### fwsnort chains not in iptables after reboot

fwsnort rules don't persist. Add to `/etc/local.d/fwsnort.start`:
```sh
#!/bin/sh
sh /home/user/scripts/nexus-fix-fwsnort-nftables.sh
```
```sh
chmod +x /etc/local.d/fwsnort.start
rc-update add local default
```

### No ESTAB signature matches appearing

```sh
# 1. Confirm fwsnort chains are loaded:
doas iptables -L | grep FWSNORT | head -5

# 2. Confirm PSAD is watching for ESTAB prefix:
grep AUTO_BLOCK_REGEX /etc/psad/psad.conf
# Should show: AUTO_BLOCK_REGEX    ESTAB;

# 3. Generate test traffic to a monitored port and watch:
watch -n1 'grep ESTAB /var/log/messages | tail -5'
```

### fwsnort rule application is very slow

This is expected. 29,000+ individual `iptables` calls at ~10–20ms each = 5–10 minutes on older hardware. Watch progress:
```sh
watch -n2 'doas iptables -L | grep -c FWSNORT'
```

### update-fwsnort completes but rules don't persist

The original `/usr/bin/update-fwsnort` applies rules via `fwsnort.sh` which uses `iptables-restore`. This fails silently on nftables due to the syntax issue. Replace it:
```sh
doas cp ~/scripts/nexus-update-fwsnort.sh /usr/bin/update-fwsnort
```

---

## FAQ

**Q: Does fwsnort slow down network traffic?**
A: Yes, slightly. Each packet that reaches a FWSNORT chain is checked against string patterns. The `string` match module does pattern matching in kernel space — efficient but not zero-cost. On this hardware (Core i3) the impact is negligible for normal traffic volumes.

**Q: What happens if fwsnort rules aren't loaded?**
A: PSAD still detects scans (via UFW BLOCK entries) but won't match any Snort signatures. You lose the `top_sigs` data and deeper attack characterization. Basic port scan detection and auto-blocking still works.

**Q: Why is fwsnort_iptcmds.sh 48MB?**
A: Each Snort rule translates to 2–4 iptables commands (INPUT, OUTPUT, FORWARD variants). ~15,000 Snort rules × 3 commands × ~1KB per command ≈ 45MB. This is normal.

**Q: How often should I update fwsnort rules?**
A: Daily. The cron job at `/etc/periodic/daily/fwsnort` handles this automatically once `/usr/bin/update-fwsnort` is replaced with the NeXuS wrapper.

**Q: Can fwsnort block traffic by itself?**
A: No — fwsnort only logs matches. Blocking is done by PSAD reading those log entries. The two tools are designed to work together.

**Q: What are the FWSNORT_INPUT_ESTAB vs FWSNORT_INPUT chains?**
A: `FWSNORT_INPUT_ESTAB` matches on established connections (full payload available). `FWSNORT_INPUT` matches on all packets including SYN. ESTAB rules catch more sophisticated attacks that require session context.

**Q: Where can I look up a Snort SID?**
A: Search for `sid:XXXXXXX` at `https://rules.emergingthreats.net` or `https://www.snort.org/rule_docs/`.

**Q: Will updating rules break anything?**
A: The update clears and reloads all rules. During the 5–10 minute load window, fwsnort signature detection is offline. PSAD's basic scan detection (UFW BLOCK entries) continues uninterrupted during the update.

---

## Key Files

```
/usr/sbin/fwsnort                      — fwsnort binary
/usr/bin/update-fwsnort                — daily update script (replace with NeXuS wrapper)
/etc/periodic/daily/fwsnort            — cron entry (calls update-fwsnort daily)
/var/lib/fwsnort/fwsnort_iptcmds.sh   — 48MB shell script, individual iptables calls
/var/lib/fwsnort/fwsnort.sh            — compact rule loader (uses iptables-restore)
/var/lib/fwsnort/fwsnort.save          — iptables-restore format backup
/var/lib/fwsnort/archive/              — previous rule versions
/var/log/fwsnort/fwsnort.log           — fwsnort run log (root only)
/var/log/messages                      — where signature matches appear (ESTAB prefix)
```

---

*NeXuS — Sane • Simple • Secure*
*Last updated: 2026-03-09*
