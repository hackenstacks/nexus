# NeXuS Startup Checklist
**Purpose:** Verify EXACT state before making ANY changes

## 1. Read First (in order)
- [ ] STATE.md - current architecture
- [ ] Git status - what's uncommitted
- [ ] Last session report - what was done

## 2. Verify Services
```bash
cd /home/user/git/NeXuS-NetWork-Stack
podman ps --filter name=nexus- --format "{{.Names}}\t{{.Status}}"
```
**Expected:** nexus-tor, nexus-i2p, nexus-yggdrasil, nexus-reticulum, nexus-dns

## 3. Test Routing
```bash
# Tor working?
curl --socks5 localhost:1080 http://httpbin.org/ip

# Medusa Privoxy working?
curl -x http://localhost:8888 http://httpbin.org/ip

# I2P console up?
curl -s http://localhost:7070/ | grep "Network status"
```

## 4. Check Git
```bash
git status --short
git log --oneline -5
```

## 5. Identify What's Broken
List SPECIFIC issues:
- Service X won't start because...
- Port Y not responding because...
- Config Z missing/wrong because...

## 6. Before ANY Changes
```bash
# Backup current state
cp nexus-compose.yaml ~/backups/nexus-compose.yaml.$(date +%Y%m%d-%H%M%S)
cp STATE.md ~/backups/STATE.md.$(date +%Y%m%d-%H%M%S)
```

## 7. After Changes
Update STATE.md:
- What changed
- Why
- Test commands to verify
- Commit-ready? YES/NO
