# 🧪 Testing NeXuS Medusa Proxy Router

## Quick Start - Get It Working Now

### Prerequisites Check

Before starting, verify you have the required software:

```bash
# Check for Tor
which tor
tor --version

# Check for Privoxy
which privoxy
privoxy --version

# Check for HAProxy
which haproxy
haproxy -v

# Optional (for full multi-network support):
which i2pd          # I2P daemon
which yggdrasil     # Yggdrasil mesh
which snowflake-proxy  # Snowflake
which rnsd          # Reticulum (optional)
```

**Minimum requirement:** Tor, Privoxy, HAProxy (original medusa-proxy functionality)

---

## Test 1: Basic Tor Routing (Original Functionality)

This tests that the base system works:

```bash
cd /home/user/git/medusa-proxy

# Start with original launcher (Tor only)
python3 start.py
```

**Expected output:**
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
Starting haproxy (port 1080).
Starting privoxy (port 8888).
```

**Test the proxy:**
```bash
# In another terminal
curl --socks5 localhost:1080 http://check.torproject.org/api/ip
# Should return Tor exit node IP

curl --proxy localhost:8888 http://check.torproject.org/api/ip
# Should return Tor exit node IP via HTTP proxy
```

**Success criteria:** ✅ Different IP than your real IP, indicates Tor exit node

---

## Test 2: NeXuS Multi-Network Launcher

This tests the full NeXuS stack with SmartRouter:

```bash
cd /home/user/git/medusa-proxy

# Launch all NeXuS networks
python3 nexus-start.py
```

**Expected output:**
```
============================================================
🌀 NeXuS Medusa Proxy - Multi-Network Edition
Version: 0.2.2
Philosophy: Together Everyone Achieves More
============================================================

📦 Software Versions:
- Privoxy: 3.0.34
- HAProxy: 2.8.11
- Tor: 0.4.8.13
- Snowflake: ...
- I2P: ...
- Yggdrasil: ...

🧠 Initializing Smart Router (automagical self-healing)...

🚀 Launching Network Services:

🌨️  Starting 1 Snowflake proxy(ies) (helping others)...
🌨️  Snowflake proxy helping 10 censored users simultaneously

🔒 Starting 2 I2P instance(s)...
[I2P instances starting...]

🌐 Starting 1 Yggdrasil mesh instance(s)...
[Yggdrasil starting...]

📡 Starting 1 Reticulum instance(s)...
[Reticulum starting...]

🧅 Starting 5 Tor instance(s) with HAProxy + Privoxy...
[Tor instances starting...]

🔄 Starting automagical health monitoring (every 30s)...

============================================================
✅ All NeXuS networks operational!
🌀 Smart router will automatically failover if networks are blocked
📊 Proxy endpoints:
   - HTTP:  localhost:8888 (Privoxy)
   - SOCKS: localhost:1080 (HAProxy)
   - I2P:   localhost:11000+ (I2P SOCKS)
============================================================
```

**What to watch for:**
- Each network should start successfully
- If a network fails (missing software), it will log a warning and continue
- Health monitoring starts automatically

---

## Test 3: SmartRouter Health Monitoring

After 30 seconds, you should see health checks:

```
🔍 Testing Tor proxies...
✅ Tor network healthy (latency: 250ms)
✅ I2P network healthy (latency: 800ms)
✅ Yggdrasil network healthy (latency: 120ms)

📊 Network Health Status:
   ✅ TOR: healthy (latency: 250ms, score: 260)
   ✅ SNOWFLAKE: healthy (latency: 300ms, score: 315)
   ✅ I2P: healthy (latency: 800ms, score: 820)
   ✅ YGGDRASIL: healthy (latency: 120ms, score: 150)
```

**Success criteria:**
- ✅ Networks showing as "healthy"
- ✅ Latency values displayed
- ✅ Scores calculated

---

## Test 4: Protocol-Specific Routing

Test that different protocols route to different networks:

**Create a test script:**

```bash
cat > /tmp/test-routing.py << 'EOF'
#!/usr/bin/env python3
import sys
sys.path.insert(0, '/home/user/git/medusa-proxy')

from proxy import SmartRouter

# Initialize router
router = SmartRouter()

# Simulate some networks as working
router.networks['tor'].mark_success(250)
router.networks['i2p'].mark_success(800)

# Test routing decisions
print("🧪 Testing SmartRouter routing decisions:\n")

tests = [
    ("example.i2p", "*.i2p"),
    ("example.onion", "*.onion"),
    ("irc.libera.chat", "irc"),
    ("torrent.example.com", "bittorrent"),
    ("matrix.org", "matrix"),
    ("google.com", "default"),
]

for destination, protocol in tests:
    network = router.route(destination, protocol)
    print(f"  {destination:25} ({protocol:12}) → {network}")

print("\n📊 Network Health:")
for name, health in router.get_network_status().items():
    status = "✅" if health['working'] else "🚨"
    print(f"  {status} {name:12} latency: {health['latency_ms']:6.0f}ms  score: {health['score']:.0f}")
EOF

chmod +x /tmp/test-routing.py
python3 /tmp/test-routing.py
```

**Expected output:**
```
🧪 Testing SmartRouter routing decisions:

  example.i2p               (*.i2p      ) → i2p
  example.onion             (*.onion    ) → tor
  irc.libera.chat           (irc        ) → i2p
  torrent.example.com       (bittorrent ) → i2p
  matrix.org                (matrix     ) → i2p
  google.com                (default    ) → tor

📊 Network Health:
  ✅ tor          latency:    250ms  score: 260
  ✅ i2p          latency:    800ms  score: 820
```

**Success criteria:**
- ✅ I2P domains route to I2P
- ✅ Onion domains route to Tor
- ✅ IRC routes to I2P (per config)
- ✅ Default traffic routes to Tor

---

## Test 5: Automatic Failover (Self-Healing)

Simulate network failure to test failover:

```python
#!/usr/bin/env python3
import sys
sys.path.insert(0, '/home/user/git/medusa-proxy')

from proxy import SmartRouter

router = SmartRouter()

# Setup: Tor and I2P both working
router.networks['tor'].mark_success(250)
router.networks['i2p'].mark_success(800)

print("🧪 Testing Automatic Failover:\n")
print("Initial state: Tor and I2P both healthy")

# Route default traffic
network = router.get_best_network("default")
print(f"  Default traffic routes to: {network}")

print("\n🚨 Simulating Tor network failure...")
# Simulate 3 failures (marks Tor as blocked)
for i in range(3):
    router.networks['tor'].mark_failure()

print(f"  Tor failure count: {router.networks['tor'].failure_count}")
print(f"  Tor working: {router.networks['tor'].is_working}")

# Route again - should failover to I2P
network = router.get_best_network("default")
print(f"\n✅ Automatic failover: Default traffic now routes to: {network}")

print("\n📊 Fallback chain:")
chain = router.get_fallback_chain("default")
print(f"  Available networks: {chain}")
```

**Expected output:**
```
🧪 Testing Automatic Failover:

Initial state: Tor and I2P both healthy
  Default traffic routes to: tor

🚨 Simulating Tor network failure...
  Tor failure count: 3
  Tor working: False

✅ Automatic failover: Default traffic now routes to: i2p

📊 Fallback chain:
  Available networks: ['i2p', 'yggdrasil']
```

**Success criteria:**
- ✅ Tor used initially (highest priority)
- ✅ After 3 failures, Tor marked as not working
- ✅ Traffic automatically fails over to I2P
- ✅ Fallback chain excludes blocked Tor

---

## Test 6: Real-World Usage

Test with actual applications:

### Using HTTP Proxy (Browsers, etc.)

```bash
# Set environment variables
export http_proxy=http://localhost:8888
export https_proxy=http://localhost:8888

# Test with curl
curl http://check.torproject.org/api/ip
# Should show Tor exit IP

# Test with Firefox
firefox --new-instance --profile /tmp/ff-test &
# In Firefox: Preferences → Network → Manual Proxy
# HTTP Proxy: localhost, Port: 8888
```

### Using SOCKS Proxy (Applications)

```bash
# Test with curl
curl --socks5 localhost:1080 http://check.torproject.org/api/ip

# Test with SSH over SOCKS
ssh -o ProxyCommand='nc -x localhost:1080 %h %p' user@example.com

# Test with torrent client (transmission)
# Settings → Network → Proxy
# Type: SOCKS5
# Proxy: localhost:1080
```

### I2P Direct Access

```bash
# I2P SOCKS proxy (direct I2P access)
curl --socks5 localhost:11000 http://example.i2p

# I2P HTTP proxy
curl --proxy localhost:12000 http://example.i2p
```

---

## Troubleshooting

### Nothing starts

```bash
# Check if ports are already in use
netstat -tlnp | grep -E "(1080|8888|10000)"

# Check logs
ls -la /var/log/tor/
ls -la /var/log/i2p/
ls -la /var/log/yggdrasil/

# Check permissions
ls -la /var/lib/tor/
ls -la /var/run/tor/
```

### Networks fail health checks

```bash
# Test Tor manually
curl --socks5 localhost:10000 http://check.torproject.org/api/ip

# Test I2P console
curl http://localhost:17000/

# Check if processes are running
ps aux | grep -E "(tor|i2pd|yggdrasil|rnsd)"
```

### SmartRouter not routing correctly

```bash
# Verify routing rules loaded
cat routing-rules.yaml

# Check Python can import modules
python3 -c "from proxy import SmartRouter; print('OK')"

# Run with more verbose logging
# (Edit proxy/log.py to set DEBUG level)
```

---

## Minimal Test (If Missing Dependencies)

If you only have Tor, Privoxy, and HAProxy:

```bash
# Just test basic Tor routing
python3 start.py

# Test it works
curl --socks5 localhost:1080 http://check.torproject.org/api/ip
```

The SmartRouter will work with whatever networks are available - it gracefully handles missing services.

---

## Success Checklist

✅ **Basic Functionality:**
- [ ] Tor instances start
- [ ] HAProxy load balances Tor
- [ ] Privoxy HTTP proxy works
- [ ] Can reach internet via proxy
- [ ] IP changes (confirms Tor working)

✅ **Multi-Network:**
- [ ] Multiple networks start (Tor, I2P, Yggdrasil, etc.)
- [ ] SmartRouter initializes
- [ ] Health monitoring runs every 30s
- [ ] Networks show healthy status

✅ **Smart Routing:**
- [ ] .i2p domains route to I2P
- [ ] .onion domains route to Tor
- [ ] Default traffic routes to Tor
- [ ] Failover works when network fails

✅ **Self-Healing:**
- [ ] Failed networks detected (3 failures)
- [ ] Traffic automatically reroutes
- [ ] Blocked networks retry after 60s
- [ ] System stays operational

---

## Next Steps After Testing

Once you verify the router works:

1. **Update Dockerfile** - Add all dependencies for production
2. **Deploy to actual NeXuS nodes** - Test in real censored environments
3. **Implement Smart Scrolls** - Multi-network web hosting
4. **Add IPFS** - Distributed content layer
5. **B.A.T.M.A.N. mesh** - Local network resilience

---

🌀 **Together Everyone Achieves More** 🌀

*Test thoroughly. Deploy confidently. Stay free.*
