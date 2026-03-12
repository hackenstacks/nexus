# 🚀 NeXuS Medusa Proxy - QUICK START

**Get it working in 5 minutes**

## Step 1: Check Prerequisites

```bash
cd /home/user/git/medusa-proxy

# Minimum requirements (must have):
which tor      # ✅ REQUIRED
which privoxy  # ✅ REQUIRED
which haproxy  # ✅ REQUIRED
which python3  # ✅ REQUIRED

# Optional (nice to have):
which i2pd          # I2P network
which yggdrasil     # Mesh network
which snowflake-proxy  # Help censored users
```

**If missing any REQUIRED packages, install them first!**

---

## Step 2: Test Basic Tor Routing (Original Functionality)

This tests that the foundation works:

```bash
# Start the original Tor-only version
python3 start.py
```

**You should see:**
```
========================================
Medusa Proxy: 0.2.2

- Privoxy: 3.0.34
- HAProxy: 2.8.11
- Tor: 0.4.8.13
========================================
Starting tor (port 10000).
Starting tor (port 10001).
...
```

**In another terminal, test it:**
```bash
curl --socks5 localhost:1080 http://check.torproject.org/api/ip
```

**Success:** You get a different IP than yours (Tor exit node IP)
**Failure:** Connection refused or timeout = something wrong with setup

---

## Step 3: Test NeXuS Multi-Network (If Step 2 Worked)

Only try this if basic Tor routing works!

```bash
# Stop start.py (Ctrl+C)

# Launch full NeXuS stack
python3 nexus-start.py
```

**You should see:**
```
🌀 NeXuS Medusa Proxy - Multi-Network Edition
Version: 0.2.2
Philosophy: Together Everyone Achieves More
...
✅ All NeXuS networks operational!
```

**Watch for:**
- ✅ Networks that start successfully
- ⚠️ Warnings for missing networks (OK if optional)
- 🚨 Errors (need to fix)

---

## Step 4: Verify It's Working

**Test Tor proxy:**
```bash
curl --socks5 localhost:1080 http://check.torproject.org/api/ip
```

**Test HTTP proxy:**
```bash
curl --proxy localhost:8888 http://check.torproject.org/api/ip
```

**Both should return different IPs (Tor exit nodes)**

---

## Step 5: Check Health Monitoring

After 30 seconds, you should see health checks in the output:

```
📊 Network Health Status:
   ✅ TOR: healthy (latency: 250ms, score: 260)
   ✅ I2P: healthy (latency: 800ms, score: 820)
   ⚠️ YGGDRASIL: not available (install yggdrasil)
```

---

## Troubleshooting

### "Connection refused" when testing

**Problem:** Services didn't start

**Fix:**
```bash
# Check if Tor is running
ps aux | grep tor

# Check ports
netstat -tlnp | grep -E "(1080|8888|10000)"

# Check logs
ls -la /var/log/tor/
tail /var/log/tor/*.log
```

### "Permission denied" errors

**Problem:** Need privileges to write to /var/

**Fix:**
```bash
# Run with sudo
sudo python3 start.py

# Or fix permissions
sudo chown -R $USER:$USER /var/lib/tor /var/log/tor /var/run/tor
```

### Import errors (Python modules)

**Problem:** Missing Python dependencies

**Fix:**
```bash
pip3 install requests jinja2 pyyaml
```

---

## What's Working?

After you verify it works, you have:

✅ **Tor routing** - Multiple Tor instances load balanced
✅ **HTTP proxy** - localhost:8888 via Privoxy
✅ **SOCKS proxy** - localhost:1080 via HAProxy
✅ **Auto circuit rotation** - Fresh Tor exit nodes

**If multi-network works, you also have:**

✅ **Smart router** - Automagical routing decisions
✅ **Health monitoring** - Continuous network checks
✅ **Multi-network support** - Tor, I2P, Yggdrasil, Reticulum, Snowflake
✅ **Self-healing** - Automatic failover when blocked

---

## Next Steps (After It Works)

1. **Read full docs:** `cat TEST_ROUTER.md`
2. **Configure routing:** `vim routing-rules.yaml`
3. **Install more networks:** I2P, Yggdrasil (see nexus-install.sh)
4. **Deploy production:** Docker, systemd service
5. **Help others:** Share with NeXuS community

---

🌀 **Together Everyone Achieves More** 🌀

*Get it working. Then make it better.*
