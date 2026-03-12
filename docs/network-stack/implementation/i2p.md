# I2P Network Support - Implementation Documentation

## Overview

Complete I2P (Invisible Internet Project) integration for Medusa Proxy, following the existing Service pattern used for Tor and Snowflake. This enables NeXuS nodes to participate in the I2P network for anonymous communications and helps strengthen the I2P network through routing participation.

## Implementation Files

### 1. `proxy/i2p.py` (Main Implementation)

**Class: I2P(Service)**

Extends the Service base class to provide I2P daemon management with the following features:

- **Multi-instance support**: Run multiple I2P routers on different ports (14000+)
- **Network participation**: Nodes help strengthen I2P by routing traffic
- **Dual proxy modes**: SOCKS5 for .i2p domains + HTTP proxy for web access
- **Health monitoring**: Checks process status and network integration
- **Configurable bandwidth**: Control how much bandwidth to share

**Key Methods:**

- `__init__()` - Initialize I2P instance with configuration
- `start()` - Start i2pd daemon
- `working` - Health check (process + network integration status)
- `get_proxy_config()` - Return proxy URLs for applications
- `_get_status()` - Query I2P console API for status

**Port Allocation:**

- SOCKS proxy: 14000 + instance_id
- HTTP proxy: 14100 + instance_id
- Console API: 17000 + instance_id

### 2. `templates/i2pd.conf` (Configuration Template)

Jinja2 template for i2pd daemon configuration supporting:

**Core Features:**
- SOCKS5 proxy for .i2p domain access
- HTTP proxy with optional Tor outproxy capability
- Web console for monitoring (local access only)
- Configurable bandwidth sharing
- Optional floodfill mode (helps network with routing)

**Network Transports:**
- NTCP2 (TCP-based, reliable)
- SSU2 (UDP-based, NAT traversal)

**Security:**
- Local-only bindings (127.0.0.1)
- Reseed verification enabled
- No authentication required for local console

**Performance:**
- AES-NI hardware acceleration
- Elgamal precomputation
- Configurable tunnel limits
- IPv6 support

### 3. `proxy/__init__.py` (Module Exports)

Updated to export I2P class alongside Tor, Haproxy, Privoxy, Snowflake, and Yggdrasil.

### 4. `test-i2p.py` (Test Script)

Standalone test script for I2P integration:
- Creates multiple I2P instances
- Monitors network integration status
- Demonstrates proper usage
- Includes documentation and examples

## Environment Variables

Configure I2P behavior through environment variables:

### `I2PS` (default: 2)
Number of I2P router instances to run
```bash
export I2PS=3  # Run 3 I2P routers
```

### `I2P_BANDWIDTH` (default: 0 = unlimited)
Bandwidth limit in KB/s
```bash
export I2P_BANDWIDTH=512  # Limit to 512 KB/s
```

### `I2P_SHARE_RATIO` (default: 80)
Percentage of bandwidth to share with network (1-100)
```bash
export I2P_SHARE_RATIO=50  # Share 50% of bandwidth
```

### `I2P_FLOODFILL` (default: false)
Enable floodfill mode to help network with routing
```bash
export I2P_FLOODFILL=true  # Only on nodes with good uptime/bandwidth
```

## Usage Examples

### Basic Usage - Single Instance

```python
from proxy import I2P

# Create I2P instance with defaults
i2p = I2P()

# Get proxy configuration
config = i2p.get_proxy_config()
# Returns: {
#   "socks": "socks5://127.0.0.1:14000",
#   "http": "http://127.0.0.1:14100"
# }

# Check if working
if i2p.working:
    print("I2P integrated into network")
```

### Multiple Instances with Custom Config

```python
from proxy import I2P

# Instance 1: Limited bandwidth
i2p1 = I2P(bandwidth=256, share_ratio=50)

# Instance 2: Unlimited, floodfill mode
i2p2 = I2P(bandwidth=0, share_ratio=80, floodfill="true")

# Monitor both
for i2p in [i2p1, i2p2]:
    if i2p.working:
        print(f"I2P {i2p.id} ready: SOCKS={i2p.port} HTTP={i2p.http_port}")
```

### Integration with Medusa Proxy

```python
# In start.py or similar orchestration script
import os
from proxy import I2P, log

# Get configuration
I2PS = int(os.environ.get("I2PS", 2))

# Create I2P instances
i2p_routers = [I2P() for i in range(I2PS)]

# Monitor and restart if needed
for i2p in i2p_routers:
    if not i2p.working:
        log.warning(f"I2P {i2p.id} not working, restarting")
        i2p.restart()
```

## Testing

### Run Test Script

```bash
# Test with default (2 instances)
./test-i2p.py

# Test with 3 instances
./test-i2p.py 3

# Test with environment variables
export I2PS=2
export I2P_BANDWIDTH=512
export I2P_SHARE_RATIO=75
./test-i2p.py
```

### Expected Behavior

1. **Startup**: Each instance creates config, starts i2pd daemon
2. **Integration**: Takes ~2 minutes to integrate into I2P network
3. **Status checks**: Monitor tunnel participation and connectivity
4. **Logging**: Clear status messages with tunnel counts and PIDs

## I2P Network Participation Philosophy

Following NeXuS philosophy of "Together Everyone Achieves More":

- **Every node helps**: By default, nodes participate in I2P routing
- **Configurable contribution**: Bandwidth limits respect resource constraints
- **Privacy first**: No unsafe logging, local-only console access
- **Network strengthening**: More participants = stronger anonymity for all

## Key I2P Features

### 1. Anonymous Communications
- Access .i2p hidden services (I2P sites)
- End-to-end encrypted messaging
- Distributed, decentralized architecture

### 2. Garlic Routing
- Multiple messages bundled together ("garlic")
- Stronger anonymity than simple onion routing
- Unidirectional tunnels (separate inbound/outbound)

### 3. Network Database
- Distributed hash table for routing info
- Floodfill routers maintain full netDB
- Regular routers query floodfill peers

### 4. Transport Flexibility
- NTCP2: TCP-based, works through firewalls
- SSU2: UDP-based, NAT traversal via STUN
- Automatic transport selection

## Integration Points

### With Tor (Outproxy)
Configure HTTP proxy to route clearnet through Tor:
```ini
[httpproxy]
outproxy = http://127.0.0.1:4444  # Privoxy/Tor
```

### With Applications
Applications can use I2P via:
- **SOCKS proxy**: Point app to 127.0.0.1:14000
- **HTTP proxy**: Browser proxy to 127.0.0.1:14100
- **SAM bridge**: Enable in config for SAM-compatible apps
- **I2CP**: Enable for Java I2P apps

### With Medusa Routing
I2P instances can be added to HAProxy rotation:
- Multiple I2P routers = load balancing
- Combine with Tor for multi-network routing
- Yggdrasil overlay for encrypted mesh routing

## Performance Characteristics

### Startup Time
- Initial reseed: ~30-60 seconds
- Network integration: ~2-5 minutes
- Full optimization: ~10-15 minutes

### Bandwidth Usage
- Minimum: ~5 KB/s (very light participation)
- Recommended: 50+ KB/s for good participation
- Floodfill: 100+ KB/s sustained

### Resource Usage
- RAM: ~64-128 MB per instance
- CPU: Low (<5% on modern hardware)
- Disk: ~50 MB for RouterInfo database

## Security Considerations

1. **Local-only bindings**: All services bound to 127.0.0.1
2. **No authentication needed**: Safe since console is localhost-only
3. **Reseed verification**: Cryptographically verify bootstrap data
4. **No unsafe logging**: Client IPs never logged
5. **Separate data directories**: Each instance isolated

## Troubleshooting

### Instance won't start
```bash
# Check logs
tail -f /var/log/i2p/i2p-14000.log

# Verify directories exist
ls -la /var/lib/i2p/14000/
ls -la /var/run/i2p/

# Check if port is available
netstat -an | grep 14000
```

### Network integration slow
- Normal for first run (building RouterInfo)
- Check bandwidth settings (too low = slow integration)
- Verify internet connectivity
- Check firewall (allow UDP for SSU2)

### Proxy not responding
- Wait 2-5 minutes after startup
- Check `working` status
- Verify tunnels are established (console API)
- Restart if needed: `i2p.restart()`

## Future Enhancements

- [ ] Parse HTML console for detailed tunnel statistics
- [ ] Enable i2pcontrol API for better monitoring
- [ ] SAM bridge integration for applications
- [ ] Automatic floodfill eligibility detection
- [ ] I2P-to-Tor routing optimization
- [ ] Bandwidth scheduling (time-based limits)
- [ ] Integration with NeXuS mesh routing

## References

- [I2P Official Website](https://geti2p.net/)
- [i2pd Documentation](https://i2pd.readthedocs.io/)
- [I2P Specification](https://geti2p.net/spec)
- [SAM Bridge API](https://geti2p.net/en/docs/api/samv3)

## NeXuS Integration Status

✅ **COMPLETE** - I2P implementation ready for integration into Medusa Proxy

**Files Created:**
- `proxy/i2p.py` - Core implementation (171 lines)
- `templates/i2pd.conf` - Configuration template (98 lines)
- `test-i2p.py` - Test script with documentation (65 lines)
- `I2P_IMPLEMENTATION.md` - This documentation

**Next Steps:**
1. Install i2pd package in Docker image
2. Test multi-instance deployment
3. Integrate with HAProxy load balancing
4. Update main start.py for I2P orchestration
5. Document in main README.md

---

**Implementation Date**: 2026-02-13
**NeXuS Philosophy**: Sane • Simple • Secure • Stealthy • Beautiful
**Status**: Ready for testing and deployment

🌐 Together Everyone Achieves More 🌐
