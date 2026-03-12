# Reticulum Network - Quick Start Guide

## What is Reticulum?

Reticulum is a cryptographic networking stack for building resilient, decentralized mesh networks. Unlike Tor (onion routing) or I2P (garlic routing), Reticulum provides a complete networking layer with:

- **End-to-end encryption** by default
- **Self-organizing mesh topology** - nodes automatically discover and connect
- **Location-independent addressing** - cryptographic identities, not IP addresses
- **Transport node capability** - every node helps route packets for others

## NeXuS Implementation

### Files
```
proxy/reticulum.py         - Service class (4.9KB)
templates/reticulum.conf   - Configuration template (1.3KB)
```

### Key Features
- ✅ Follows Service class pattern (like Tor, I2P, Yggdrasil)
- ✅ Transport node mode enabled (helps the network)
- ✅ Multi-instance support via RETICULUMS env var
- ✅ Comprehensive health checking
- ✅ Auto-discovery via UDP broadcast + multicast
- ✅ TCP server for incoming connections

### Port Allocation
| Component | Port Range | Purpose |
|-----------|------------|---------|
| Management | 4965+ | Service identification |
| UDP Interface | 4965+ | Local network discovery |
| TCP Interface | 4242+ | Incoming connections |

## Usage

### Single Instance
```python
from proxy import Reticulum

# Start Reticulum transport node
rtn = Reticulum()

# Check health
if rtn.working:
    print(f"Reticulum routing at PID {rtn.pid}")

# Stop gracefully
rtn.stop()
```

### Multiple Instances (High Availability)
```python
# Create 3 Reticulum instances
instances = [Reticulum() for _ in range(3)]

# Health check
for rtn in instances:
    print(f"Instance {rtn.id}: {'✓' if rtn.working else '✗'}")
```

### Environment Variables
```bash
export RETICULUMS=3  # Run 3 instances
```

## Network Topology

```
[NeXuS Node A]  <--UDP--> [NeXuS Node B]
      |                         |
    TCP                       TCP
      |                         |
      v                         v
[Remote Node C] <--Mesh--> [Remote Node D]
```

Each node:
1. **Listens** on UDP for local discovery
2. **Listens** on TCP for remote connections
3. **Routes** packets for other nodes (transport mode)
4. **Auto-discovers** nearby nodes via AutoInterface

## Health Monitoring

The `working` property checks:
- ✅ Process is running (PID exists)
- ✅ Config directory created
- ✅ Log shows successful initialization

```python
if rtn.working:
    # Logs: 🌐 Reticulum instance 0 healthy (PID 12345) - routing for the network
    #       UDP: 4965, TCP: 4242, Config: /var/lib/reticulum/instance-0
```

## Configuration

Auto-generated from template with:
```ini
enable_transport = Yes       # Help route for others
share_instance = Yes         # Share with local programs
respond_to_probes = Yes      # Allow connectivity testing

[interfaces]
  AutoInterface              # Auto-discover local nodes
  UDPInterface              # Broadcast discovery
  TCPServerInterface        # Accept connections
```

## NeXuS Philosophy

**"Together Everyone Achieves More"**

Every Reticulum node is a **transport node**, meaning:
- It routes packets for others (not just for itself)
- It strengthens the mesh simply by existing
- It helps build resilience for everyone

This embodies NeXuS core values:
- **Decentralization** - No central servers
- **Cooperation** - Every node helps others
- **Resilience** - Mesh heals automatically
- **Privacy** - End-to-end encryption by default

## Integration Status

✅ **COMPLETE** - Ready for orchestration in start.py

Next steps:
1. Add Reticulum instances to start.py
2. Configure RETICULUMS environment variable
3. Monitor health via working property
4. Deploy as part of NeXuS Medusa Proxy stack

---

*"Sane • Simple • Secure • Stealthy • Beautiful"*
*NeXuS - Where Everyone Has A Voice, And Every Voice Protected*
