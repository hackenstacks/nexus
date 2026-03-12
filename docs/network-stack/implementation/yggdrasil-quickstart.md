# Yggdrasil Quick Start Guide

## What You Need

1. **Yggdrasil daemon installed** on your system
2. **NeXuS Medusa Proxy** with Yggdrasil support

## Basic Usage

### Start Single Yggdrasil Instance

```bash
# Default configuration (multicast discovery only)
python3 start.py
```

### Connect to Public Peers

```bash
# Set peer addresses
export YGGDRASIL_PEERS="tcp://54.37.137.221:11129,tcp://45.147.198.155:6010"

# Start proxy
python3 start.py
```

### Multiple Instances

```bash
# Run 3 Yggdrasil instances for redundancy
export YGGDRASILS=3
export YGGDRASIL_PEERS="tcp://peer1.com:6000,tls://peer2.com:6001,tcp://peer3.com:6002"

python3 start.py
```

## Configuration Files

After starting, you'll find:

- **Config**: `/etc/yggdrasil/yggdrasil-0.conf` (and -1.conf, -2.conf, etc.)
- **Logs**: `/var/log/yggdrasil/yggdrasil-0.log`
- **Data**: `/var/lib/yggdrasil/0/`

## Health Check

```python
from proxy import Yggdrasil

ygg = Yggdrasil()

if ygg.working:
    print("✅ Connected to Yggdrasil mesh!")
else:
    print("❌ Connection issues")
```

## Finding Your IPv6 Address

Check the health output or query the admin API:

```bash
curl -s http://localhost:9001 -d '{"request":"getSelf"}' | jq '.response.address'
```

Output: `"200:1234:5678:abcd:ef01:2345:6789:abcd"`

## Public Peer Lists

Get peers from official repository:
- https://github.com/yggdrasil-network/public-peers

**Example peers:**
```
tcp://54.37.137.221:11129
tcp://45.147.198.155:6010
tls://102.223.180.74:993
tcp://188.225.9.167:18226
```

## Port Usage

| Instance | Admin API | Peering Listen |
|----------|-----------|----------------|
| 0        | 9001      | 6000           |
| 1        | 9002      | 6001           |
| 2        | 9003      | 6002           |

## Testing Connectivity

### Ping Another Yggdrasil Node

```bash
ping6 200:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx
```

### Check Peer Count

```bash
curl -s http://localhost:9001 -d '{"request":"getPeers"}' | jq '.response.peers | length'
```

### View Routing Table

```bash
curl -s http://localhost:9001 -d '{"request":"getSwitchPeers"}' | jq .
```

## Troubleshooting

### No Peers Connecting

1. Check your peer addresses are correct
2. Verify firewall allows outbound connections
3. Try different peers from public list

### Admin API Not Responding

- Daemon may still be starting (wait 10 seconds)
- Check logs: `tail -f /var/log/yggdrasil/yggdrasil-0.log`

### Process Not Running

```bash
# Check if yggdrasil executable exists
which yggdrasil

# Install if missing (Alpine)
apk add yggdrasil
```

## Advanced: Custom Peers

Edit the config template to use your own peer infrastructure:

```bash
# Add to templates/yggdrasil.conf
Peers: [
  "tcp://your-peer-1.example.com:6000",
  "tls://your-peer-2.example.com:6001"
]
```

## Integration Example

```python
from proxy import Yggdrasil, Tor
import os

# Start Yggdrasil mesh networking
ygg_count = int(os.environ.get("YGGDRASILS", "1"))
yggdrasils = [Yggdrasil() for _ in range(ygg_count)]

# Start Tor instances
tor_count = int(os.environ.get("TORS", "10"))
tors = [Tor() for _ in range(tor_count)]

# Health check everything
print("\n🔗 Yggdrasil Mesh Status:")
for ygg in yggdrasils:
    ygg.working

print("\n🧅 Tor Circuit Status:")
for tor in tors:
    tor.working
```

## NeXuS Philosophy

**Together Everyone Achieves More** 

Every Yggdrasil node participates in routing, strengthening the mesh network. The more nodes that join, the more resilient and faster the network becomes.

Your node helps others while they help you. That's the NeXuS way! 🔗
