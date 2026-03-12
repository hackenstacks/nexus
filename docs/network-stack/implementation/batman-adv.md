# 🦇 B.A.T.M.A.N. Advanced Setup for NeXuS

## What is B.A.T.M.A.N.?

**Better Approach To Mobile Adhoc Networking**
- Layer 2 mesh protocol (like a smart switch)
- Auto-routing, self-healing network
- Works over WiFi, Ethernet, VPN tunnels
- Complements Yggdrasil (overlay) and Reticulum (crypto mesh)

## NeXuS Mesh Trinity

```
B.A.T.M.A.N. → Layer 2 mesh (WiFi/Ethernet physical networks)
    ↓
Yggdrasil   → Layer 3 overlay mesh (200::/7 IPv6 global)
    ↓
Reticulum   → Cryptographic mesh (packet radio, LoRa, etc)
```

## Prerequisites

**Host system needs:**
```bash
# Load batman-adv kernel module on HOST (not in container)
doas modprobe batman-adv

# Check if loaded
lsmod | grep batman

# Make it load on boot
echo "batman-adv" | doas tee -a /etc/modules-load.d/batman.conf
```

## Build & Start

```bash
# 1. Build B.A.T.M.A.N. container
podman build -f Dockerfile.batman -t localhost/nexus-batman:latest .

# 2. Apply IPv6 rules (allow B.A.T.M.A.N. link-local)
doas ./nexus-ipv6-yggdrasil-only.sh

# 3. Start NeXuS with B.A.T.M.A.N.
./nexus-shutdown.sh
./nexus-launch-gentle.sh
```

## Configuration

**Default settings** (in batman-setup.sh):
- Routing algorithm: BATMAN_V (fastest, most efficient)
- Gateway mode: client (use mesh for internet)
- Hop penalty: 30 (lower = prefer this node)
- Originator interval: 5000ms (announce every 5 seconds)
- Bridge loop avoidance: enabled
- Distributed ARP table: enabled
- Multicast optimization: enabled

**To change:**
```bash
# Edit batman-setup.sh and adjust batctl commands
nano batman-setup.sh
```

## Add Network Interfaces to Mesh

B.A.T.M.A.N. needs physical interfaces to mesh over:

```bash
# Get into batman container
podman exec -it nexus-batman sh

# Add WiFi interface to mesh
batctl if add wlan0

# Add Ethernet interface
batctl if add eth0

# Add VPN tunnel
batctl if add tun0

# Check added interfaces
batctl if
```

## Monitor Mesh Network

```bash
# Check neighbors (other mesh nodes)
podman exec nexus-batman batctl n

# Check originators (all nodes in mesh)
podman exec nexus-batman batctl o

# Show gateway servers
podman exec nexus-batman batctl gwl

# Show translation table (who's where)
podman exec nexus-batman batctl tg

# Live kernel log
podman exec nexus-batman batctl log
```

## Use Cases

**Local mesh WiFi:**
```bash
# Create mesh between laptops/phones over WiFi
# Each device runs NeXuS + B.A.T.M.A.N.
# Auto-routing, no internet needed
```

**Community mesh network:**
```bash
# Connect neighborhood over rooftop WiFi links
# B.A.T.M.A.N. handles routing
# Yggdrasil provides global addressing
# Everyone shares internet connection
```

**Disaster recovery:**
```bash
# When internet is down:
# 1. B.A.T.M.A.N. creates local mesh
# 2. Reticulum adds LoRa/packet radio
# 3. Yggdrasil provides addressing
# = Complete off-grid network!
```

## IPv6 Configuration

**Allowed ranges:**
- `fe80::/10` - B.A.T.M.A.N. link-local (mesh coordination)
- `200::/7` - Yggdrasil global addresses (overlay)
- All other IPv6 BLOCKED (prevents leaks)

**Check IPv6 rules:**
```bash
doas ip6tables -L -v -n
```

## Troubleshooting

**No neighbors showing:**
- Check if batman-adv module loaded: `lsmod | grep batman`
- Check if interfaces added: `batctl if`
- Check WiFi is in ad-hoc/mesh mode: `iwconfig`

**Container won't start:**
- Ensure `privileged: true` in compose file
- Check kernel module on HOST: `doas modprobe batman-adv`
- Check container logs: `podman logs nexus-batman`

**Mesh not routing:**
- Check gateway mode: `batctl gw_mode`
- Check routing algorithm: `batctl ra`
- Increase log level: `batctl loglevel all`

## Advanced: WiFi Mesh Setup

Create ad-hoc WiFi network for B.A.T.M.A.N.:

```bash
# On HOST (not in container):

# Set WiFi to ad-hoc mode
doas ip link set wlan0 down
doas iw wlan0 set type ibss
doas ip link set wlan0 up

# Join ad-hoc network
doas iw wlan0 ibss join NeXuS-Mesh 2412

# Add to batman mesh
podman exec nexus-batman batctl if add wlan0

# Check neighbors
podman exec nexus-batman batctl n
```

## Integration with Other Mesh Networks

**With Yggdrasil:**
```bash
# B.A.T.M.A.N. creates physical layer
# Yggdrasil runs on top for global routing
# Both can coexist on bat0 interface
```

**With Reticulum:**
```bash
# Reticulum can use bat0 as an interface
# Add to reticulum config:
# [[interfaces]]
#   type = AutoInterface
#   interface_name = bat0
```

## Resources

- B.A.T.M.A.N. Advanced: https://www.open-mesh.org/
- Documentation: https://www.open-mesh.org/projects/batman-adv/wiki
- Mailing list: https://lists.open-mesh.org/

🦇 **Together Everyone Achieves More** - Mesh Edition! 🌀
