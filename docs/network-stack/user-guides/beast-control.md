# 🐉 NeXuS Hydra Beast Control

## 🔥 FIRE IT UP

```bash
cd /home/user/git/NeXuS-NetWork-Stack
./nexus-launch.sh
```

Wait 90 seconds for Tor circuits, then check status:
```bash
./nexus-status.py
```

**What starts:**
- 🧅 **Tor** (2 instances, HAProxy load-balanced) - Port 1080 (SOCKS), 8888 (HTTP)
- 🌐 **I2P** (takes 3-5 min to bootstrap) - Port 4444 (HTTP), 4447 (SOCKS)
- 🕸️ **Yggdrasil** mesh network - Port 9001
- 📡 **Reticulum** mesh network - Port 4242
- 🔒 **Unbound DNS** - Port 5353

## 🌑 SHUT IT DOWN (100% DARK)

```bash
./nexus-shutdown.sh
```

Want to remove ALL data (full reset):
```bash
./nexus-shutdown.sh --volumes
```

## 🧪 TEST THE BEAST

**After 90 seconds (Tor ready):**
```bash
# Test Tor routing
curl --proxy http://localhost:8888 https://check.torproject.org/api/ip
# Should show: {"IsTor":true,"IP":"..."}
```

**After 3-5 minutes (I2P ready):**
```bash
# Monitor I2P bootstrap
./test-i2p-bootstrap.sh

# Or test manually
curl --proxy http://localhost:8888 http://legwork.i2p
```

## 🎯 ROUTING RULES (NOW WORKING!)

The beast now has its brain connected:

- `.i2p` domains → **I2P network** (port 4447)
- `.onion` domains → **Tor network** (port 1080)
- Everything else → **Tor network** (default)

**Privoxy Smart Routing Active:**
- Config: `/etc/privoxy/user.action` (inside nexus-tor container)
- Rules: `medusa-routing.action`
- Engine: Built-in actionsfile directive

## 🐕 THE GREMLINS SLAIN (Day 178)

1. ✅ **Privoxy Template** - Added actionsfile directive
2. ✅ **Mount Path** - Fixed .new → active routing file
3. ✅ **I2P Network Lock** - Changed 127.0.0.1 → 0.0.0.0

**Local Build:**
- Image: `localhost/medusa-proxy:nexus-fixed`
- Source: `/home/user/git/medusa-proxy`
- Modified: `templates/privoxy.cfg`

## 📊 MONITOR THE BEAST

```bash
# Real-time status
./nexus-status.py

# Container status
podman ps

# HAProxy stats (load balancing)
firefox http://localhost:2090
# Login: admin / nexus

# I2P console
firefox http://localhost:7070
```

## 🔧 TROUBLESHOOTING

**Containers stuck in "starting":**
- Normal! Tor takes 90 seconds, I2P takes 3-5 minutes
- Check: `podman logs nexus-tor`

**I2P not working:**
- Wait 3-5 minutes for bootstrap
- Run: `./test-i2p-bootstrap.sh`
- Check: `curl http://localhost:7070`

**Routing not working:**
- Verify actionsfile loaded: `podman exec nexus-tor cat /etc/privoxy/config-0`
- Should see: `actionsfile /etc/privoxy/user.action`

**Need to rebuild after changes:**
```bash
cd /home/user/git/medusa-proxy
podman build -t localhost/medusa-proxy:nexus-fixed .
cd /home/user/git/NeXuS-NetWork-Stack
./nexus-shutdown.sh
./nexus-launch.sh
```

## 🎸 VICTORY MUSIC

Day 178 - Medusa riding Hydra - Dogs finally barking - Beast fully operational

**Together Everyone Achieves More** 🌀
