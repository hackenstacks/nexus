# 🌀 NeXuS Medusa Proxy - Multi-Network Mesh Architecture

**Philosophy:** "Together Everyone Achieves More"
**Mission:** Censorship-resistant anonymous network that self-heals when blocked

## Overview

NeXuS Medusa Proxy is a revolutionary multi-network anonymity system that provides **automagical self-healing routing** across five different anonymity networks. When one network is blocked or censored, traffic automatically fails over to another network - without user intervention.

## Core Networks

### 1. **Tor** (Priority: 10 - Highest)
- **Purpose:** Primary anonymity network, fastest and most established
- **Instances:** 5+ (configurable via `TORS` env var)
- **Ports:** SOCKS 1080, HTTP 8888 (via Privoxy)
- **Features:**
  - Load balanced via HAProxy
  - Automatic circuit rotation
  - Exit node selection
  - Bridge support for censorship circumvention

### 2. **Snowflake** (Priority: 15)
- **Purpose:** Tor pluggable transport helping censored users
- **Instances:** 1+ (configurable via `SNOWFLAKES`)
- **Default:** Runs on every NeXuS node (helping others)
- **Features:**
  - WebRTC-based proxy
  - Helps users in censored regions
  - NAT traversal capabilities
  - **Embodies "Together Everyone Achieves More"**

### 3. **I2P** (Priority: 20)
- **Purpose:** Anonymous P2P network, excellent for .i2p domains
- **Instances:** 2+ (configurable via `I2PS`)
- **Ports:** SOCKS 11000+, HTTP 12000+, Console 17000+
- **Features:**
  - Hidden services (.i2p domains)
  - Participating router (helps I2P network)
  - Outproxy capability (clearnet via Tor)
  - Optimized for P2P protocols (BitTorrent, IRC)

### 4. **Yggdrasil** (Priority: 30)
- **Purpose:** Encrypted IPv6 mesh network
- **Instances:** 1+ (configurable via `YGGDRASILS`)
- **Ports:** Admin 9001+, Peering 6000+
- **Features:**
  - End-to-end encrypted IPv6
  - Decentralized mesh routing
  - No central authority
  - Peer-to-peer connectivity

### 5. **Reticulum** (Priority: 40)
- **Purpose:** Cryptographic mesh networking stack
- **Instances:** 1+ (configurable via `RETICULUMS`)
- **Ports:** Management 4965+, UDP 4965+, TCP 4242+
- **Features:**
  - Transport-agnostic (UDP, TCP, LoRa, etc.)
  - E2E encryption by default
  - Self-organizing topology
  - Works on any network medium

## Automagical Self-Healing Routing

### How It Works

The **SmartRouter** continuously monitors all networks and automatically routes traffic based on:

1. **Network Health** - Is the network working right now?
2. **Protocol Preferences** - Which network is best for this protocol?
3. **Performance** - Which network has lowest latency?
4. **Reliability** - Which network has fewest recent failures?

### Failover Example

```
User requests: torrent.example.com via BitTorrent

SmartRouter decision:
1. Check routing rules: BitTorrent → prefers I2P
2. Check I2P health: ✅ Working (latency: 50ms)
3. Route via I2P

Later, I2P gets blocked...

SmartRouter decision:
1. Check I2P health: 🚨 3 consecutive failures
2. Mark I2P as blocked
3. Failover to next preference: Tor
4. Route via Tor automatically
5. Continue monitoring I2P, retry after 60s
```

### Routing Rules

Defined in `routing-rules.yaml`:

```yaml
# Domain-based routing
"*.i2p": [i2p, tor]           # I2P domains prefer I2P network
"*.onion": [tor, snowflake]   # Tor hidden services require Tor

# Protocol-based routing
irc: [i2p, tor]               # IRC prefers I2P
bittorrent: [i2p, tor]        # BitTorrent prefers I2P
matrix: [i2p, tor, yggdrasil] # Matrix can use multiple networks

# Application-specific routing
deltachat: [tor, i2p]         # Email-based messaging
onionshare: [tor, snowflake]  # Anonymous file sharing (Tor only)
retroshare: [i2p, tor]        # F2F network
briar: [tor, snowflake]       # P2P messaging

# Default for everything else
default: [tor, snowflake, i2p, yggdrasil, reticulum]
```

## Application Support

### Messaging Apps

#### **DeltaChat**
- Email-based decentralized messaging
- Routes via: Tor → I2P (failover)
- Ports: 143, 993 (IMAP), 587, 465 (SMTP)

#### **Briar**
- P2P encrypted messaging
- Routes via: Tor → Snowflake (failover)
- Uses Tor hidden services
- Also supports Bluetooth/WiFi Direct mesh

#### **Matrix**
- Federated messaging protocol
- Routes via: I2P → Tor → Yggdrasil (failover chain)
- Ports: 8448 (federation), 8008 (client)

### File Sharing

#### **OnionShare**
- Anonymous file sharing
- Routes via: Tor only (hidden services)
- No failover (requires Tor hidden services)

#### **RetroShare**
- Friend-to-friend network
- Routes via: I2P → Tor (failover)
- Ports: 7812-7815

#### **BitTorrent**
- P2P file sharing
- Routes via: I2P → Tor (failover)
- Ports: 6881-6889, 51413
- Prefers I2P to avoid exit node issues

### Communication

#### **IRC**
- Internet Relay Chat
- Routes via: I2P → Tor (failover)
- Ports: 6667, 6697, 7000
- Better anonymity via I2P

## Architecture Diagram

```
                        NeXuS Node
    ┌───────────────────────────────────────────────────┐
    │                                                   │
    │  ┌──────────────────────────────────────────┐    │
    │  │     Smart Router (Automagical Brain)     │    │
    │  │  - Health monitoring every 30s           │    │
    │  │  - Automatic failover                    │    │
    │  │  - Protocol-specific routing             │    │
    │  └──────────────────────────────────────────┘    │
    │                      │                            │
    │         ┌────────────┼────────────┐               │
    │         │            │            │               │
    │    ┌────▼───┐   ┌───▼────┐  ┌───▼────┐           │
    │    │  Tor   │   │  I2P   │  │Yggdrasil          │
    │    │ (×5)   │   │  (×2)  │  │  (×1)  │           │
    │    │Port    │   │Port    │  │IPv6    │           │
    │    │1080    │   │11000   │  │Mesh    │           │
    │    └────┬───┘   └───┬────┘  └───┬────┘           │
    │         │           │           │                 │
    │    ┌────▼───┐   ┌───▼────┐  ┌───▼────┐           │
    │    │Snowflake   │Reticulum  │HAProxy │           │
    │    │Helping │   │Crypto  │  │Load    │           │
    │    │Others  │   │Mesh    │  │Balance │           │
    │    └────────┘   └────────┘  └───┬────┘           │
    │                                  │                │
    │                             ┌────▼────┐           │
    │                             │Privoxy  │           │
    │                             │HTTP:8888│           │
    │                             └────┬────┘           │
    │                                  │                │
    └──────────────────────────────────┼────────────────┘
                                       │
                                    Client
                              (Browser, Apps, etc.)
```

## Health Monitoring

### Continuous Checks (Every 30s)

For each network, the SmartRouter checks:
- ✅ Process is running
- ✅ Proxy endpoint responds
- ✅ Can reach test endpoints
- ✅ Latency is acceptable

### Failure Handling

- **1 failure:** Log warning, keep using network
- **2 failures:** Increase score (prefer other networks)
- **3 failures:** Mark as blocked, failover to next network
- **After 60s:** Retry blocked network (auto-healing)

### Health Metrics

Each network tracks:
- **is_working:** Boolean status
- **latency_ms:** Response time
- **failure_count:** Consecutive failures
- **success_count:** Total successful checks
- **score:** Routing priority (lower = better)

## Configuration

### Environment Variables

```bash
# Network instances
export TORS=5              # Number of Tor instances
export SNOWFLAKES=1        # Number of Snowflake proxies (helping others!)
export I2PS=2              # Number of I2P instances
export YGGDRASILS=1        # Number of Yggdrasil instances
export RETICULUMS=1        # Number of Reticulum instances

# Monitoring
export HEALTH_CHECK_INTERVAL=30  # Seconds between health checks
export PROXY_CHECK_INTERVAL=15m  # Interval for Tor proxy checks

# I2P configuration
export I2P_BANDWIDTH=unlimited   # Bandwidth limit
export I2P_SHARE_RATIO=80        # % of bandwidth to share with network

# Yggdrasil configuration
export YGGDRASIL_PEERS="tcp://peer1:port,tcp://peer2:port"

# Tor configuration
export TOR_EXIT_NODES="{us},{ca},{de}"  # Preferred exit countries
export TOR_BRIDGES="obfs4 ..."           # Bridge configuration
```

### Launch NeXuS Node

```bash
# Start all networks with smart routing
./nexus-start.py

# Or with Docker
docker run -p 8888:8888 -p 1080:1080 \
    -e TORS=5 -e I2PS=2 -e SNOWFLAKES=1 \
    nexus-medusa-proxy
```

## Use Cases

### 1. Bypassing Censorship
- **Scenario:** Country blocks Tor
- **NeXuS Response:** Auto-failover to Snowflake bridges → I2P → Yggdrasil
- **Result:** Uninterrupted access

### 2. P2P Applications
- **Scenario:** Running BitTorrent anonymously
- **NeXuS Response:** Routes via I2P (better for P2P than Tor exit nodes)
- **Result:** Better performance, fewer issues

### 3. Hidden Services
- **Scenario:** Accessing .i2p and .onion sites
- **NeXuS Response:** .i2p → I2P network, .onion → Tor network
- **Result:** Optimal routing for each network type

### 4. Network Resilience
- **Scenario:** ISP throttles Tor traffic
- **NeXuS Response:** Detects high latency, fails over to I2P
- **Result:** Maintains performance automatically

## Security Considerations

### Threat Model

NeXuS Medusa Proxy protects against:
- ✅ Network censorship (multi-network failover)
- ✅ Traffic analysis (encrypted multi-hop routing)
- ✅ ISP monitoring (anonymity networks)
- ✅ Exit node attacks (multiple networks, no single point)
- ✅ Correlation attacks (different networks use different routing)

### Not Protected Against

- ❌ Endpoint compromise (malware on your device)
- ❌ Browser fingerprinting (use Tor Browser for this)
- ❌ Social engineering attacks
- ❌ Physical access to device

### Best Practices

1. **Use Tor Browser** for web browsing (not just proxy)
2. **Different networks for different activities** (don't mix identities)
3. **Keep software updated** (security patches)
4. **Monitor logs** for unusual activity
5. **Contribute bandwidth** (run Snowflake, help others)

## Performance

### Typical Latency

- **Tor:** 200-500ms (3 hops)
- **Snowflake:** 300-600ms (bridges + 3 hops)
- **I2P:** 500-1500ms (garlic routing)
- **Yggdrasil:** 50-200ms (direct mesh, fewer hops)
- **Reticulum:** Varies by transport (UDP fast, LoRa slow)

### Bandwidth

Each network participates in routing for others:
- **Snowflake:** Helps censored users (low bandwidth)
- **I2P:** Shares 80% of bandwidth by default
- **Yggdrasil:** Routes for mesh (minimal overhead)
- **Reticulum:** Forwards packets (configurable)

## NeXuS Philosophy

### "Together Everyone Achieves More"

Every NeXuS node:
- ✅ Helps censored users (Snowflake proxy)
- ✅ Strengthens I2P network (participating router)
- ✅ Extends Yggdrasil mesh (peering node)
- ✅ Routes Reticulum packets (transport node)

**You benefit from the network. The network benefits from you.**

### "Sane • Simple • Secure"

- **Sane:** Reasonable defaults, no complex configuration
- **Simple:** One script runs everything
- **Secure:** Multiple layers of anonymity
- **Stealthy:** Encrypted at every layer
- **Beautiful:** Clean architecture, documented code

## Troubleshooting

### No networks working

```bash
# Check if services started
ps aux | grep -E "(tor|i2pd|yggdrasil|rnsd|snowflake)"

# Check logs
tail -f /var/log/tor/*.log
tail -f /var/log/i2p/*.log
tail -f /var/log/yggdrasil/*.log
```

### Slow performance

```bash
# Check network health
# Health status displayed every check interval
# Look for high latency or failures

# Try specific network directly
curl --socks5 localhost:1080 http://check.torproject.org  # Tor
curl --socks5 localhost:11000 http://example.com          # I2P
```

### I2P not working

```bash
# Check I2P console
curl http://localhost:17000/  # I2P console API

# Verify SOCKS proxy
netstat -tlnp | grep 11000
```

### Routing not failing over

```bash
# Check health monitoring is running
# Should see health checks in logs every 30s

# Verify routing rules
cat routing-rules.yaml
```

## Development

### Project Structure

```
medusa-proxy/
├── proxy/
│   ├── tor.py              # Tor service
│   ├── snowflake.py        # Snowflake proxy
│   ├── i2p.py              # I2P service
│   ├── yggdrasil.py        # Yggdrasil mesh
│   ├── reticulum.py        # Reticulum network
│   ├── smart_router.py     # Automagical routing brain
│   ├── haproxy.py          # Load balancer
│   ├── privoxy.py          # HTTP proxy
│   └── service.py          # Base service class
├── templates/
│   ├── tor.cfg             # Tor configuration template
│   ├── i2pd.conf           # I2P configuration template
│   ├── yggdrasil.conf      # Yggdrasil configuration
│   ├── reticulum.conf      # Reticulum configuration
│   ├── haproxy.cfg         # HAProxy configuration
│   └── privoxy.cfg         # Privoxy configuration
├── nexus-start.py          # Main launcher (NeXuS edition)
├── start.py                # Original launcher (Tor only)
├── routing-rules.yaml      # Routing configuration
└── README.md               # Documentation
```

### Adding a New Network

1. Create `proxy/newnetwork.py` extending `Service` class
2. Create `templates/newnetwork.conf` for configuration
3. Add to `proxy/__init__.py` exports
4. Register with SmartRouter in `nexus-start.py`
5. Add routing rules to `routing-rules.yaml`
6. Update this documentation

## Contributing

NeXuS is about freedom and collaboration. Contributions welcome!

Areas for improvement:
- Additional anonymity networks
- Better health checking algorithms
- Performance optimizations
- Protocol-specific routing rules
- Application integration guides

## License

Same as upstream medusa-proxy (check original README)

## Credits

- **Original Medusa Proxy:** datawookie/medusa-proxy
- **NeXuS Multi-Network Edition:** NeXuS Project
- **Tor Project:** The Tor network
- **I2P Project:** The Invisible Internet Project
- **Yggdrasil Project:** Yggdrasil Network
- **Reticulum Project:** Reticulum Network Stack

---

🌀 **Together Everyone Achieves More** 🌀

*Building a censorship-resistant future, one node at a time.*
