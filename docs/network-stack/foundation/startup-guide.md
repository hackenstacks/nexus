# 🌀 NeXuS Network Stack - Complete Startup Guide

## Step-by-Step Startup Instructions

### Option 1: Docker/Podman Container Stack (Recommended)

**Prerequisites:**
- Docker or Podman installed
- Root/sudo access for network routing

**Step 1: Start the NeXuS Stack**
```bash
cd /home/user/git/NeXuS-NetWork-Stack
./nexus-launch.sh
```

This will:
1. Start Medusa Tor proxy (foundation layer with 5 Tor instances)
2. Wait for Tor circuits to establish
3. Start all other services:
   - I2P network (SOCKS proxy on 4447, HTTP on 4444)
   - Yggdrasil mesh network
   - Reticulum mesh network
   - Unbound DNS resolver
4. All services bootstrap over Tor (no clearnet calls)

**Step 2: Monitor Network Status**
```bash
# Single check
./nexus-status.py

# Live monitoring (auto-refresh every 30 seconds)
./nexus-status.py --live
```

**Step 3: Route All Traffic Through NeXuS**
```bash
# Enable full traffic routing (requires root)
sudo ./nexus-route-all-traffic.sh start

# Check routing status
sudo ./nexus-route-all-traffic.sh status

# Disable routing (restore normal)
sudo ./nexus-route-all-traffic.sh stop
```

---

### Option 2: Python Direct Launcher (No Docker)

**Prerequisites:**
- Python 3.8+
- All network tools installed (tor, i2pd, yggdrasil, etc.)

**Step 1: Install Dependencies**
```bash
pip3 install -r requirements.txt
```

**Step 2: Launch NeXuS**
```bash
./nexus-start.py
```

This launches:
- 5 Tor instances with HAProxy load balancing
- Snowflake proxies (helps censored users)
- I2P network instances
- Yggdrasil mesh
- Reticulum mesh
- Smart Router (automagical self-healing)
- Privoxy (HTTP proxy frontend on port 8888)

**Step 3: Monitor (in another terminal)**
```bash
./nexus-status.py --live
```

---

## Service Endpoints

| Service | Protocol | Port | Description |
|---------|----------|------|-------------|
| **Privoxy** | HTTP Proxy | 8888 | Smart routing + ad-blocking frontend |
| **HAProxy** | SOCKS5 | 1080 | Tor load balancer |
| **I2P SOCKS** | SOCKS5 | 4447 | I2P network proxy |
| **I2P HTTP** | HTTP Proxy | 4444 | I2P network HTTP proxy |
| **I2P Console** | Web UI | 7070 | I2P admin interface |
| **Yggdrasil** | Admin API | 9001 | Yggdrasil mesh admin |
| **Reticulum** | TCP | 4242 | Reticulum mesh network |
| **Unbound DNS** | DNS | 5353 | Private DNS resolver |
| **HAProxy Stats** | Web UI | 2090 | HAProxy statistics (admin:nexus) |

---

## Browser Configuration

### Firefox
1. Settings → Network Settings → Manual proxy configuration
2. HTTP Proxy: `localhost` Port: `8888`
3. Check "Use this proxy server for all protocols"
4. Check "Proxy DNS when using SOCKS v5"

### Chrome/Chromium
```bash
chromium --proxy-server="http://localhost:8888"
```

### System-wide (100% traffic routing)
```bash
sudo ./nexus-route-all-traffic.sh start
```

---

## Verification

**Test Tor anonymity:**
```bash
curl --proxy http://localhost:8888 https://check.torproject.org/api/ip
```

**Test I2P access:**
```bash
curl --proxy socks5h://localhost:4447 http://stats.i2p/
```

**Check your exit IP:**
```bash
curl --proxy http://localhost:8888 https://httpbin.org/ip
```

**Monitor real-time:**
```bash
./nexus-status.py --live
```

---

## Troubleshooting

### Tor not starting
```bash
# Check logs
podman logs nexus-tor

# Restart just Tor
podman restart nexus-tor
```

### I2P not connecting
```bash
# I2P takes 3-5 minutes to bootstrap
# Check console: http://localhost:7070

# View logs
podman logs nexus-i2p
```

### No internet after routing
```bash
# Disable routing to restore normal internet
sudo ./nexus-route-all-traffic.sh stop

# Check if services are running
./nexus-status.py
```

### Container won't start
```bash
# Check if port is already in use
sudo netstat -tulpn | grep -E '(8888|1080|4447)'

# Force restart all
podman-compose -f nexus-compose.yaml down
./nexus-launch.sh
```

---

## Stopping NeXuS

### Docker/Podman Stack
```bash
# Stop all services
podman-compose -f nexus-compose.yaml down

# Remove volumes (fresh start)
podman-compose -f nexus-compose.yaml down -v
```

### Python Launcher
```bash
# Press Ctrl+C in the nexus-start.py terminal
# Smart router will gracefully shutdown all networks
```

### Restore Normal Routing
```bash
sudo ./nexus-route-all-traffic.sh stop
```

---

## Philosophy: "Together Everyone Achieves More"

Every NeXuS node is identical - one script runs them all. The smart router automatically:
- Detects network failures
- Routes around censorship
- Heals broken connections
- Balances load across networks

🌀 **Sane. Simple. Secure.**
