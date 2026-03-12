# 🐉 NeXuS Beast Slayer Control Guide

## 🎮 The Three Commands You Need

```bash
cd /home/user/git/medusa-proxy

./nexus-launch.sh           # Start all networks
./nexus-status.py --live     # Live monitor (30 sec refresh)
./nexus-doctor.sh            # 🩺 Auto-fix network issues
```

## 🩺 NeXuS Doctor - Automagical Repair

**Having network issues? Run the doctor!**

```bash
./nexus-doctor.sh
```

**What it does:**
- ✅ Checks all services automatically
- ✅ Restarts stopped containers
- ✅ Fixes crash loops
- ✅ Detects port conflicts
- ✅ Tests connectivity
- ✅ Cleans up zombies
- ✅ Shows what it fixed

**Run it whenever:**
- Services aren't working
- After system reboot
- Something feels wrong
- Just to check health

## 📊 Status Monitor Features

### Single Check
```bash
./nexus-status.py
```
Shows:
- ✅ Network status (all services)
- 🌐 Exit IPs (Tor routing confirmed)
- 📜 **Recent logs** from each container
- 🔄 Routing configuration

### Live Monitoring (30 second auto-refresh)
```bash
./nexus-status.py --live
```
- Auto-updates every 30 seconds
- Shows live logs
- Press Ctrl+C to exit

## 🌐 Networks Running

| Network | Ports | Status |
|---------|-------|--------|
| 🔥 Privoxy | 8118 | Smart router + ad-blocking |
| 🧅 Tor | 1080, 8888 | Anonymity network |
| 👁️ I2P | 4444, 4447, 7070 | Anonymous P2P |
| 🌐 Yggdrasil | 9001, 6000 | Encrypted mesh |
| 📡 Reticulum | 4242, 4965 | Mesh networking |
| 🔍 DNS | 5353 | Split DNS resolver |

## 🛠️ Individual Container Control

### View Logs
```bash
podman logs -f nexus-privoxy      # Privoxy logs
podman logs -f nexus-i2p           # I2P logs
podman logs -f nexus-yggdrasil     # Yggdrasil logs
podman logs -f nexus-reticulum     # Reticulum logs
podman logs --tail 50 nexus-dns    # Last 50 DNS log lines
```

### Restart a Service
```bash
podman restart nexus-privoxy
podman restart nexus-i2p
# etc...
```

### Stop Everything
```bash
podman-compose -f nexus-compose.yaml down
```

## 🎯 Using NeXuS Routing

Set your browser/system proxy to:
```
HTTP Proxy: localhost:8118
SOCKS Proxy: (leave blank or use localhost:1080 for direct Tor)
```

### What Gets Routed Where:
- `*.i2p` domains → I2P network
- Everything else → Tor network
- 104,983 ads/trackers → BLOCKED

## 🔥 Quick Troubleshooting

### Privoxy not working?
```bash
podman logs nexus-privoxy          # Check logs
podman restart nexus-privoxy       # Restart it
```

### Want to see everything?
```bash
./nexus-status.py --live           # Live monitor mode
```

### Reticulum not connecting?
```bash
podman logs nexus-reticulum        # Check what it's doing
```

## 🌀 Together Everyone Achieves More

Your node helps strengthen these mesh networks just by running them!
