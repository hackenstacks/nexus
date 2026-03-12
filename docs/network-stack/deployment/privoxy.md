# NeXuS Privoxy Standalone Architecture

## Why Separate Privoxy?

The original setup had Privoxy built into the `nexus-tor` container, which caused issues:
- ❌ Hard to debug (is Tor broken or Privoxy broken?)
- ❌ Couldn't restart Privoxy without restarting Tor
- ❌ Status monitoring was confusing
- ❌ Resource limits affected both services together

**Now Privoxy runs as a separate container!**

## Architecture

```
┌─────────────────────────────────────────────────┐
│  Your Browser (localhost:8118)                  │
└─────────────────┬───────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────┐
│  nexus-privoxy (Standalone Privoxy)             │
│  • Ad blocking (104,983 rules)                  │
│  • Privacy filters                              │
│  • Cookie management                            │
│  • Port: 8118                                   │
└─────────────────┬───────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────┐
│  nexus-tor (Tor + HAProxy)                      │
│  • 2 Tor instances (load balanced)              │
│  • HAProxy SOCKS: 1080                          │
│  • Built-in HTTP: 8888 (backup)                 │
└─────────────────────────────────────────────────┘
                  │
                  ▼
            Tor Network
```

## Ports

| Port | Service | Use This |
|------|---------|----------|
| **8118** | **Privoxy Standalone** | **✅ RECOMMENDED** |
| 8888 | Built-in Tor HTTP | Backup only |
| 1080 | Tor SOCKS | Direct SOCKS access |
| 2090 | HAProxy Stats | Monitoring |

## Setup

**1. Build the standalone Privoxy:**
```bash
cd /home/user/git/NeXuS-NetWork-Stack
podman-compose -f nexus-compose.yaml build privoxy
```

**2. Start everything:**
```bash
./nexus-shutdown.sh
./nexus-launch.sh
```

**3. Verify Privoxy is separate:**
```bash
podman ps --filter "name=nexus-privoxy"
# Should show: nexus-privoxy container running
```

**4. Test it:**
```bash
# Test standalone Privoxy
curl --proxy http://localhost:8118 https://check.torproject.org/api/ip

# Test built-in (backup)
curl --proxy http://localhost:8888 https://check.torproject.org/api/ip
```

## Managing Standalone Privoxy

**Restart just Privoxy (without affecting Tor):**
```bash
./nexus-restart-service.sh privoxy
```

**Check Privoxy logs:**
```bash
podman logs nexus-privoxy
podman logs -f nexus-privoxy  # Follow
```

**Stop just Privoxy:**
```bash
podman stop nexus-privoxy
```

**Start just Privoxy:**
```bash
podman start nexus-privoxy
```

## Browser Configuration

**Firefox:**
```
Settings → Network Settings → Manual proxy configuration
  HTTP Proxy: localhost
  Port: 8118
  ✓ Also use this proxy for HTTPS
  ✓ Proxy DNS when using SOCKS v5
```

**Chrome/Chromium:**
```bash
chromium --proxy-server="http://localhost:8118"
```

**System-wide (all apps):**
```bash
export http_proxy=http://localhost:8118
export https_proxy=http://localhost:8118
```

## Troubleshooting

**Privoxy won't start:**
```bash
# Check if port 8118 is already in use
sudo netstat -tlnp | grep 8118

# Rebuild Privoxy container
podman-compose -f nexus-compose.yaml build --no-cache privoxy
podman-compose -f nexus-compose.yaml up -d privoxy
```

**Privoxy works but Tor doesn't:**
```bash
# Restart just Tor (Privoxy stays up)
./nexus-restart-service.sh tor
```

**Switch back to built-in:**
```bash
# Just use port 8888 instead of 8118
# Change browser proxy to: localhost:8888
```

## Benefits

✅ **Independent Management** - Restart Privoxy without affecting Tor
✅ **Easier Debugging** - Know exactly which service has issues
✅ **Resource Control** - Separate CPU/memory limits
✅ **Cleaner Monitoring** - Status script shows each service clearly
✅ **Backup Option** - Built-in 8888 still works if standalone fails

## Resource Usage

Standalone Privoxy is lightweight:
- **CPU:** 0.25 cores max
- **RAM:** 128MB max
- **Startup:** ~2 seconds

Much faster than restarting the whole nexus-tor container!

🌀 **Together Everyone Achieves More**
