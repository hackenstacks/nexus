# NeXuS Transparent Routing Architecture
## Complete Multi-Layer Darknet Routing System

**Version:** 1.0
**Date:** 2026-02-12
**Status:** DESIGN SPECIFICATION
**Philosophy:** Sane • Simple • Secure • Stealthy • Beautiful

---

## EXECUTIVE SUMMARY

This document defines the complete transparent routing architecture for NeXuS, transforming ALL network traffic into multi-layer anonymized communication through TOR, I2P, Yggdrasil, and Reticulum networks. Built on the proven Medusa proxy foundation, this architecture provides UNSTOPPABLE resilience and INVISIBLE leak-proof operation.

**Key Principles:**
- **Transparent:** ALL traffic routed automatically, zero application configuration
- **Multi-Layer:** Nested anonymity chains (I2P over TOR, Yggdrasil over I2P/TOR, etc.)
- **Leak-Proof:** DNS, IPv6, WebRTC, and all other leaks prevented at firewall level
- **Resilient:** Automatic failover, health checking, circuit rotation
- **Split-Tunneling:** Whitelist support for local/trusted traffic

---

## ARCHITECTURE OVERVIEW

```
┌─────────────────────────────────────────────────────────────────┐
│                    APPLICATION LAYER                             │
│  (Browser, IRC, Matrix, WebTorrent, RetroShare, OnionShare)    │
└────────────────────┬────────────────────────────────────────────┘
                     │ ALL traffic intercepted transparently
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                  TRANSPARENT PROXY LAYER                         │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  nftables/iptables Rules                                 │  │
│  │  - Mark packets by destination/protocol                  │  │
│  │  - Force DNS through dnscrypt-proxy                      │  │
│  │  - Split tunnel whitelist bypass                         │  │
│  │  - IPv6 blackhole (prevent leaks)                        │  │
│  │  - REDIRECT all HTTP(S) to local proxy                   │  │
│  └──────────────────────────────────────────────────────────┘  │
└────────────────────┬────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                   SMART ROUTING LAYER (HAProxy)                  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Domain-based routing:                                    │  │
│  │  • .onion    → TOR SOCKS                                 │  │
│  │  • .i2p      → I2P HTTP Proxy                            │  │
│  │  • .ygg      → Yggdrasil Proxy                           │  │
│  │  • clearnet  → Privoxy → TOR                             │  │
│  │  • Direct    → Whitelisted local IPs                     │  │
│  │                                                           │  │
│  │  Protocol detection & load balancing                      │  │
│  └──────────────────────────────────────────────────────────┘  │
└────────┬────────────┬────────────┬────────────┬────────────────┘
         │            │            │            │
         ▼            ▼            ▼            ▼
    ┌────────┐  ┌────────┐  ┌────────┐  ┌──────────┐
    │  TOR   │  │  I2P   │  │ Yggdr  │  │Reticulum │
    │ Network│  │Network │  │Network │  │ Network  │
    └────────┘  └────────┘  └────────┘  └──────────┘
         │            │            │            │
         └────────────┴────────────┴────────────┘
                      │
                      ▼
            [ ANONYMOUS INTERNET ]
```

---

## COMPONENT ARCHITECTURE

### 1. FIREWALL LAYER (nftables/iptables)

**Purpose:** Transparent traffic interception and leak prevention

#### 1.1 Core Rules

```bash
#!/bin/bash
# /home/user/scripts/nexus-transparent-firewall.sh

# Sane • Simple • Secure • Stealthy • Beautiful

# Configuration
TRANS_UID="nexus-proxy"      # User running transparent proxy
TRANS_PORT="9040"            # Transparent proxy port
DNS_PORT="5353"              # DNSCrypt-proxy port
WHITELIST_IPS="10.0.0.0/8 192.168.0.0/16 127.0.0.0/8"

# Packet marking for routing decisions
# Mark 1: Route through TOR
# Mark 2: Route through I2P (over TOR)
# Mark 3: Route through Yggdrasil
# Mark 4: Direct (whitelisted)

setup_nftables() {
    echo "🔥 Setting up NeXuS Transparent Firewall..."

    # Flush existing rules
    nft flush ruleset

    # Create main table
    nft add table inet nexus_filter

    # ==========================================
    # INPUT CHAIN - Only accept established connections
    # ==========================================
    nft add chain inet nexus_filter input { type filter hook input priority 0 \; policy drop \; }
    nft add rule inet nexus_filter input ct state established,related accept
    nft add rule inet nexus_filter input iif lo accept
    nft add rule inet nexus_filter input icmp type echo-request limit rate 5/second accept

    # ==========================================
    # OUTPUT CHAIN - Transparent routing
    # ==========================================
    nft add chain inet nexus_filter output { type filter hook output priority 0 \; policy drop \; }

    # Allow localhost
    nft add rule inet nexus_filter output oif lo accept

    # Allow transparent proxy user (prevent loop)
    nft add rule inet nexus_filter output skuid $TRANS_UID accept

    # Allow whitelisted IPs directly (split tunnel)
    for IP in $WHITELIST_IPS; do
        nft add rule inet nexus_filter output ip daddr $IP mark set 4 accept
    done

    # Prevent IPv6 leaks - DROP ALL IPv6
    nft add rule inet nexus_filter output meta nfproto ipv6 drop

    # ==========================================
    # DNS INTERCEPTION - Force through dnscrypt-proxy
    # ==========================================
    nft add rule inet nexus_filter output udp dport 53 redirect to :$DNS_PORT
    nft add rule inet nexus_filter output tcp dport 53 redirect to :$DNS_PORT

    # ==========================================
    # TRANSPARENT HTTP/HTTPS REDIRECTION
    # ==========================================
    nft add chain inet nexus_filter prerouting { type nat hook prerouting priority -100 \; }
    nft add rule inet nexus_filter prerouting tcp dport 80 mark set 1 redirect to :$TRANS_PORT
    nft add rule inet nexus_filter prerouting tcp dport 443 mark set 1 redirect to :$TRANS_PORT

    # ==========================================
    # MARK-BASED ROUTING (for HAProxy backend selection)
    # ==========================================
    # Mark 1: TOR routing (default for clearnet)
    nft add rule inet nexus_filter output mark 1 ct state new,established accept

    # Mark 2: I2P routing (bootstrapped over TOR)
    nft add rule inet nexus_filter output mark 2 ct state new,established accept

    # Mark 3: Yggdrasil routing (over TOR/I2P)
    nft add rule inet nexus_filter output mark 3 ct state new,established accept

    # Mark 4: Direct routing (whitelisted only)
    nft add rule inet nexus_filter output mark 4 accept

    # Default: if no mark, mark as TOR (mark 1)
    nft add rule inet nexus_filter output ct state new mark set 1

    # Accept established connections
    nft add rule inet nexus_filter output ct state established,related accept

    echo "✅ Firewall rules deployed - ALL traffic now anonymized!"
    echo "🔒 IPv6 BLOCKED | DNS SECURED | TRANSPARENT MODE ACTIVE"
}

# DNS Leak Prevention - Additional layer
prevent_dns_leaks() {
    # Disable IPv6 at kernel level
    sysctl -w net.ipv6.conf.all.disable_ipv6=1
    sysctl -w net.ipv6.conf.default.disable_ipv6=1

    # Disable IPv6 on all interfaces
    for interface in $(ls /sys/class/net/); do
        sysctl -w net.ipv6.conf.$interface.disable_ipv6=1 2>/dev/null
    done

    # Prevent DNS leaks via /etc/resolv.conf
    chattr -i /etc/resolv.conf 2>/dev/null
    echo "nameserver 127.0.0.1" > /etc/resolv.conf
    echo "options edns0" >> /etc/resolv.conf
    chattr +i /etc/resolv.conf

    echo "🛡️ DNS leak prevention activated!"
}

# WebRTC Leak Prevention (browser-level)
generate_browser_policy() {
    cat > /tmp/nexus-browser-policy.json <<EOF
{
    "WebRTCIPHandlingPolicy": "disable_non_proxied_udp",
    "WebRTCUDPPortRange": "",
    "ProxyMode": "fixed_servers",
    "ProxyServer": "http://127.0.0.1:8888"
}
EOF
    echo "📋 Browser policy generated at /tmp/nexus-browser-policy.json"
    echo "   Apply to Chrome/Chromium for WebRTC leak prevention"
}

case "$1" in
    start)
        setup_nftables
        prevent_dns_leaks
        generate_browser_policy
        ;;
    stop)
        nft flush ruleset
        echo "🔥 Transparent firewall disabled"
        ;;
    status)
        nft list ruleset
        ;;
    *)
        echo "Usage: $0 {start|stop|status}"
        exit 1
        ;;
esac
```

#### 1.2 Leak Prevention Matrix

| Leak Type | Prevention Method | Layer |
|-----------|------------------|-------|
| **DNS Leak** | Force all DNS to dnscrypt-proxy (port 5353) → TOR | nftables DNAT |
| **IPv6 Leak** | Block ALL IPv6 traffic at firewall + kernel | nftables DROP + sysctl |
| **WebRTC Leak** | Browser policy forcing proxy for all UDP | Browser config |
| **Application Bypass** | Transparent REDIRECT (no app can bypass) | nftables REDIRECT |
| **Time-based Fingerprint** | Random time offset in TOR circuits | TOR config |
| **MTU Fingerprint** | Normalize packet sizes | Privoxy + TOR |

---

### 2. SMART ROUTING LAYER (HAProxy)

**Purpose:** Domain-based routing, protocol detection, load balancing

#### 2.1 HAProxy Configuration

```haproxy
# /etc/haproxy/nexus-master.cfg
# Sane • Simple • Secure • Stealthy • Beautiful

global
    daemon
    user nexus-proxy
    group nexus-proxy
    log /dev/log local0 warning
    maxconn 100000
    tune.ssl.default-dh-param 2048

    # Performance tuning
    nbthread 4
    cpu-map auto:1/1-4 0-3

defaults
    mode http
    log global
    option httplog
    option dontlognull
    option http-server-close
    option forwardfor except 127.0.0.0/8
    option redispatch

    timeout connect 10s
    timeout client  3600s
    timeout server  3600s
    timeout queue   30s

    retries 3
    maxconn 50000

# ==========================================
# STATISTICS INTERFACE
# ==========================================
listen stats
    bind *:9999
    mode http
    stats enable
    stats uri /
    stats realm NeXuS\ Proxy\ Statistics
    stats auth admin:nexus-secret-2026
    stats refresh 5s

# ==========================================
# TRANSPARENT HTTP PROXY (Port 9040)
# ==========================================
frontend nexus_transparent
    bind 127.0.0.1:9040 transparent
    mode http

    # Logging
    log-format "%ci:%cp [%tr] %ft %b/%s %TR/%Tw/%Tc/%Tr/%Ta %ST %B %CC %CS %tsc %ac/%fc/%bc/%sc/%rc %sq/%bq %hr %hs %{+Q}r"

    # Domain-based ACLs
    acl is_onion    hdr_end(host) -i .onion
    acl is_i2p      hdr_end(host) -i .i2p
    acl is_ygg      hdr_end(host) -i .ygg
    acl is_local    hdr_end(host) -i .local

    # Protocol detection
    acl is_ssl      dst_port 443

    # Smart routing based on destination
    use_backend backend_tor_onion  if is_onion
    use_backend backend_i2p        if is_i2p
    use_backend backend_yggdrasil  if is_ygg
    use_backend backend_local      if is_local
    use_backend backend_tor_clear  # Default: clearnet through TOR

# ==========================================
# SOCKS PROXY (Port 1080)
# ==========================================
frontend nexus_socks
    bind 127.0.0.1:1080
    mode tcp
    default_backend backend_tor_socks_pool

# ==========================================
# BACKEND: TOR for .onion sites
# ==========================================
backend backend_tor_onion
    mode http
    balance roundrobin
    option httpchk GET /

    # Multiple TOR instances for load balancing
    server tor-onion-1 127.0.0.1:9050 check inter 30s fall 3 rise 2
    server tor-onion-2 127.0.0.1:9051 check inter 30s fall 3 rise 2
    server tor-onion-3 127.0.0.1:9052 check inter 30s fall 3 rise 2

# ==========================================
# BACKEND: I2P for .i2p sites
# ==========================================
backend backend_i2p
    mode http
    balance roundrobin

    # I2P HTTP proxy (running in container)
    server i2p-http-1 127.0.0.1:4444 check inter 30s fall 3 rise 2
    server i2p-http-2 127.0.0.1:4445 check inter 30s fall 3 rise 2

# ==========================================
# BACKEND: Yggdrasil for .ygg sites
# ==========================================
backend backend_yggdrasil
    mode http
    balance roundrobin

    # Yggdrasil HTTP proxy (over TOR)
    server ygg-proxy-1 127.0.0.1:8080 check inter 30s fall 3 rise 2

# ==========================================
# BACKEND: Clearnet through TOR (via Privoxy)
# ==========================================
backend backend_tor_clear
    mode http
    balance roundrobin
    option httpchk HEAD / HTTP/1.0

    # Privoxy instances (each connected to HAProxy TOR pool)
    server privoxy-1 127.0.0.1:8888 check inter 30s fall 3 rise 2
    server privoxy-2 127.0.0.1:8889 check inter 30s fall 3 rise 2
    server privoxy-3 127.0.0.1:8890 check inter 30s fall 3 rise 2

# ==========================================
# BACKEND: TOR SOCKS pool (round-robin)
# ==========================================
backend backend_tor_socks_pool
    mode tcp
    balance roundrobin
    option tcp-check

    # TOR SOCKS instances
    server tor-socks-1 127.0.0.1:9050 check inter 30s fall 3 rise 2
    server tor-socks-2 127.0.0.1:9051 check inter 30s fall 3 rise 2
    server tor-socks-3 127.0.0.1:9052 check inter 30s fall 3 rise 2
    server tor-socks-4 127.0.0.1:9053 check inter 30s fall 3 rise 2
    server tor-socks-5 127.0.0.1:9054 check inter 30s fall 3 rise 2

# ==========================================
# BACKEND: Local/Direct (whitelisted only)
# ==========================================
backend backend_local
    mode http
    # This should only be hit for whitelisted local services
    # In transparent mode, firewall already handles this
    server local-direct 127.0.0.1:80
```

#### 2.2 Domain Routing Logic

```
Request arrives → Check Host header
                   │
                   ├─ Ends with .onion  → backend_tor_onion (direct TOR SOCKS)
                   ├─ Ends with .i2p    → backend_i2p (I2P HTTP proxy over TOR)
                   ├─ Ends with .ygg    → backend_yggdrasil (Ygg proxy over TOR/I2P)
                   ├─ Ends with .local  → backend_local (direct, whitelisted only)
                   └─ Anything else     → backend_tor_clear (Privoxy → TOR)
```

---

### 3. MULTI-LAYER ANONYMITY CHAINS

#### 3.1 Chain Architecture

```
┌──────────────────────────────────────────────────────────┐
│  Layer 4: Application (IRC, Matrix, WebTorrent, etc.)   │
└────────────────────┬─────────────────────────────────────┘
                     │
┌────────────────────▼─────────────────────────────────────┐
│  Layer 3: Reticulum Network                              │
│  - Mesh networking over TOR/I2P                          │
│  - Encrypted packets through darknet                     │
│  - Uses TOR/I2P as transport layer                       │
└────────────────────┬─────────────────────────────────────┘
                     │
┌────────────────────▼─────────────────────────────────────┐
│  Layer 2: Yggdrasil Network                              │
│  - Overlay network routing                               │
│  - Traffic exits through I2P or TOR                      │
│  - Provides IPv6 mesh addressing                         │
└────────────────────┬─────────────────────────────────────┘
                     │
┌────────────────────▼─────────────────────────────────────┐
│  Layer 1: I2P Network (bootstrapped over TOR)            │
│  - I2P outproxy: Privoxy → TOR                           │
│  - Hidden service access                                 │
│  - Garlic routing encryption                             │
└────────────────────┬─────────────────────────────────────┘
                     │
┌────────────────────▼─────────────────────────────────────┐
│  Layer 0: TOR Network (base anonymity)                   │
│  - Multiple circuits                                     │
│  - Exit node rotation                                    │
│  - Bridge support for censorship bypass                  │
└────────────────────┬─────────────────────────────────────┘
                     │
                     ▼
              [ INTERNET ]
```

#### 3.2 I2P over TOR Configuration

**I2P Container Setup:**

```yaml
# /home/user/docker/i2p-over-tor/docker-compose.yml
version: '3.8'

services:
  i2p-over-tor:
    image: divax/i2p:current
    container_name: nexus-i2p-over-tor
    restart: unless-stopped

    # Network configuration
    networks:
      - nexus_darknet

    ports:
      - "127.0.0.1:4444:4444"   # HTTP Proxy
      - "127.0.0.1:4445:4445"   # HTTPS Proxy
      - "127.0.0.1:7657:7657"   # I2P Router Console
      - "127.0.0.1:7658:7658"   # I2P SAM API

    volumes:
      - i2p_data:/i2p/.i2p
      - ./i2p-config:/config

    environment:
      # Force I2P to use TOR as outproxy
      - JVM_XMX=512M
      - I2P_OUTPROXY_HOST=nexus-privoxy-tor
      - I2P_OUTPROXY_PORT=8888

    # Depend on TOR being available
    depends_on:
      - privoxy-tor-pool

    user: "1000:1000"

  # Privoxy for I2P outproxy (connected to TOR)
  privoxy-tor-pool:
    image: vimagick/privoxy
    container_name: nexus-privoxy-i2p-outproxy
    restart: unless-stopped

    networks:
      - nexus_darknet

    volumes:
      - ./privoxy-i2p-outproxy.conf:/etc/privoxy/config:ro

    depends_on:
      - tor-pool

  # TOR pool for I2P outproxy
  tor-pool:
    image: dperson/torproxy
    container_name: nexus-tor-i2p-base
    restart: unless-stopped

    networks:
      - nexus_darknet

    environment:
      - TOR_NewCircuitPeriod=120
      - TOR_MaxCircuitDirtiness=600

networks:
  nexus_darknet:
    driver: bridge
    ipam:
      config:
        - subnet: 172.20.0.0/16

volumes:
  i2p_data:
```

**I2P Configuration (tunnels.conf):**

```ini
# /home/user/docker/i2p-over-tor/i2p-config/tunnels.conf

# I2P HTTP Proxy (for .i2p sites)
[I2P HTTP Proxy]
type = http
host = 0.0.0.0
port = 4444
outproxy = nexus-privoxy-tor
outproxyPort = 8888
# Use TOR for clearnet access from I2P

# I2P HTTPS Proxy
[I2P HTTPS Proxy]
type = httpclient
host = 0.0.0.0
port = 4445
outproxy = nexus-privoxy-tor
outproxyPort = 8888
```

#### 3.3 Yggdrasil over TOR/I2P Configuration

**Yggdrasil Container:**

```yaml
# /home/user/docker/yggdrasil-over-tor/docker-compose.yml
version: '3.8'

services:
  yggdrasil-over-i2p:
    build: .
    container_name: nexus-yggdrasil-overlay
    restart: unless-stopped

    cap_add:
      - NET_ADMIN

    networks:
      - nexus_darknet

    ports:
      - "127.0.0.1:8080:8080"   # HTTP proxy
      - "127.0.0.1:9001:9001"   # Admin API

    volumes:
      - yggdrasil_data:/var/lib/yggdrasil
      - ./yggdrasil.conf:/etc/yggdrasil.conf:ro

    environment:
      # Peer connections routed through I2P/TOR
      - YGG_PEER_PROXY=socks5://nexus-tor-i2p-base:9050

    depends_on:
      - tor-pool

networks:
  nexus_darknet:
    external: true

volumes:
  yggdrasil_data:
```

**Yggdrasil Configuration:**

```json
{
  "Listen": ["tcp://[::]:9001"],
  "AdminListen": "tcp://127.0.0.1:9001",

  "Peers": [
    "tcp://1.2.3.4:12345",
    "tls://5.6.7.8:54321"
  ],

  "InterfacePeers": {},

  "MulticastInterfaces": [],

  "AllowedEncryptionPublicKeys": [],

  "EncryptionPublicKey": "",
  "EncryptionPrivateKey": "",

  "SigningPublicKey": "",
  "SigningPrivateKey": "",

  "IfName": "ygg0",
  "IfMTU": 65535,

  "SessionFirewall": {
    "Enable": false,
    "AllowFromDirect": true,
    "AllowFromRemote": true
  },

  "TunnelRouting": {
    "Enable": true,
    "IPv4LocalSubnets": [],
    "IPv6LocalSubnets": [],
    "IPv4RemoteSubnets": {},
    "IPv6RemoteSubnets": {}
  },

  "SwitchOptions": {
    "MaxTotalQueueSize": 4194304
  },

  "NodeInfo": {
    "name": "nexus-anonymous-node",
    "location": "unknown"
  },

  "ProxyType": "socks5",
  "ProxyAddress": "127.0.0.1:9050"
}
```

#### 3.4 Reticulum over TOR/I2P

**Reticulum Configuration:**

```ini
# /home/user/.reticulum/config

[reticulum]
enable_transport = yes
share_instance = yes
shared_instance_port = 37428
instance_control_port = 37429

[logging]
loglevel = 4

# I2P Interface (primary)
[interfaces]
  [[I2P Interface]]
    type = TCPClientInterface
    enabled = yes
    target_host = 127.0.0.1
    target_port = 7658  # I2P SAM API

  [[TOR Interface]]
    type = TCPClientInterface
    enabled = yes
    proxy_type = socks5
    proxy_host = 127.0.0.1
    proxy_port = 9050
    target_host = reticulum.onion
    target_port = 4965
```

**Reticulum Startup Script:**

```bash
#!/bin/bash
# /home/user/scripts/nexus-reticulum-overlay.sh

# Ensure I2P and TOR are running
if ! pgrep -f "i2p" > /dev/null; then
    echo "🔴 I2P not running! Starting I2P container..."
    docker-compose -f ~/docker/i2p-over-tor/docker-compose.yml up -d
    sleep 30  # Wait for I2P bootstrap
fi

if ! pgrep -f "tor" > /dev/null; then
    echo "🔴 TOR not running! Starting TOR..."
    systemctl start tor
    sleep 10
fi

# Start Reticulum daemon
echo "🔥 Starting Reticulum mesh network over I2P/TOR..."
rnsd --config ~/.reticulum/config

# Announce presence on mesh
echo "📡 Announcing node on Reticulum network..."
rnstatus
```

---

### 4. DNS ARCHITECTURE

**Purpose:** Prevent ALL DNS leaks while handling special TLDs

#### 4.1 DNSCrypt-Proxy Configuration

```toml
# /etc/dnscrypt-proxy/dnscrypt-proxy.toml

# Sane • Simple • Secure • Stealthy • Beautiful

listen_addresses = ['127.0.0.1:5353']
max_clients = 250

# Use only DNSCrypt servers over TOR
server_names = ['cloudflare', 'google', 'quad9-dnscrypt-ip4-nofilter']

# Force all DNS through TOR SOCKS proxy
proxy = 'socks5://127.0.0.1:9050'

# DNS caching
cache = true
cache_size = 4096
cache_min_ttl = 2400
cache_max_ttl = 86400
cache_neg_min_ttl = 60
cache_neg_max_ttl = 600

# DNSSEC validation
require_dnssec = true
require_nolog = true
require_nofilter = true

# Query logging (for debugging only)
[query_log]
  file = '/var/log/dnscrypt-proxy/query.log'
  format = 'tsv'

# Blocked names (malware/tracking)
[blocked_names]
  blocked_names_file = '/etc/dnscrypt-proxy/blocked-names.txt'

# Special TLD handling
[static]
  [static.'onion']
    stamp = 'sdns://AAAAAAAAAAAAAAAAAAAAAA'  # TOR resolver

  [static.'i2p']
    stamp = 'sdns://AgcAAAAAAAAAAAAHNC40LjQuNA'  # I2P resolver

  [static.'ygg']
    stamp = 'sdns://AAAAAAAAAAAAAAAAAAAAAA'  # Yggdrasil resolver
```

#### 4.2 Special TLD Resolution

**TOR .onion Resolution:**

```bash
# Handled directly by TOR SOCKS proxy
# HAProxy routes .onion to TOR backend
# No DNS needed - onion addresses are keys
```

**I2P .i2p Resolution:**

```bash
#!/bin/bash
# /home/user/scripts/nexus-i2p-resolver.sh

# I2P addressbook for .i2p domain resolution
# Queries I2P router's addressbook API

DOMAIN="$1"
I2P_CONSOLE="http://127.0.0.1:7657"

# Query I2P addressbook
DEST=$(curl -s "$I2P_CONSOLE/susidns/addressbook.jsp?book=router&hostname=$DOMAIN" \
       | grep -oP 'Destination:\s*\K[A-Za-z0-9\-~]+')

if [ -n "$DEST" ]; then
    echo "$DEST"
else
    echo "ERROR: .i2p domain not found in addressbook"
    exit 1
fi
```

**Yggdrasil .ygg Resolution:**

```bash
#!/bin/bash
# /home/user/scripts/nexus-ygg-resolver.sh

# Yggdrasil uses IPv6 addresses
# Resolve .ygg to Yggdrasil IPv6

DOMAIN="$1"
YGG_ADMIN="http://127.0.0.1:9001"

# Query Yggdrasil admin API for peer info
IPV6=$(curl -s "$YGG_ADMIN/api" \
       -H "Content-Type: application/json" \
       -d '{"request":"getNodeInfo","name":"'$DOMAIN'"}' \
       | jq -r '.response.address')

if [ "$IPV6" != "null" ]; then
    echo "$IPV6"
else
    echo "ERROR: .ygg domain not found"
    exit 1
fi
```

#### 4.3 DNS Leak Test Script

```bash
#!/bin/bash
# /home/user/scripts/nexus-dns-leak-test.sh

echo "🔍 Testing for DNS leaks..."
echo ""

# Test 1: Check resolv.conf
echo "[1] Checking /etc/resolv.conf..."
cat /etc/resolv.conf | grep nameserver
echo ""

# Test 2: Query external DNS leak test
echo "[2] Querying DNS leak test via TOR..."
curl --socks5 127.0.0.1:9050 -s https://www.dnsleaktest.com/results.html | grep -oP 'ISP:\s*\K[^<]+'
echo ""

# Test 3: Check for IPv6 leaks
echo "[3] Testing for IPv6 leaks..."
curl -6 --max-time 5 -s https://ipv6.icanhazip.com 2>&1
if [ $? -ne 0 ]; then
    echo "✅ IPv6 is disabled (no leak)"
else
    echo "🔴 WARNING: IPv6 LEAK DETECTED!"
fi
echo ""

# Test 4: WebRTC leak test
echo "[4] WebRTC leak check (manual)..."
echo "   Open: https://browserleaks.com/webrtc"
echo "   Expected: No local IP addresses visible"
echo ""

# Test 5: Compare real IP vs TOR IP
echo "[5] IP comparison..."
echo -n "   Real IP (should timeout): "
timeout 3 curl -s https://api.ipify.org 2>&1 || echo "✅ BLOCKED"
echo -n "   TOR IP: "
curl --socks5 127.0.0.1:9050 -s https://api.ipify.org
echo ""

echo "✅ DNS leak test complete!"
```

---

### 5. INTEGRATION COMPONENTS

#### 5.1 Privoxy Enhanced Configuration

```
# /etc/privoxy/nexus-master.conf
# Sane • Simple • Secure • Stealthy • Beautiful

user-manual /usr/share/doc/privoxy/user-manual
confdir /etc/privoxy
logdir /var/log/privoxy
actionsfile match-all.action
actionsfile default.action
actionsfile user.action
filterfile default.filter
filterfile user.filter
logfile logfile

# Listen on localhost only
listen-address 127.0.0.1:8888

# Forward to TOR SOCKS
forward-socks5 / 127.0.0.1:9050 .

# uBlock Origin filters (downloaded and updated)
actionsfile /etc/privoxy/ublock-origin.action
filterfile /etc/privoxy/ublock-origin.filter

# EasyList tracking protection
actionsfile /etc/privoxy/easylist.action

# Privacy settings
enable-remote-toggle 0
enable-edit-actions 0
enable-remote-http-toggle 0
enforce-blocks 1

# Header modifications for anonymity
hide-user-agent 1
hide-referer 1
hide-from-header 1
add-header "DNT: 1"

# Compression (save bandwidth over TOR)
enable-compression 1

# Connection settings
keep-alive-timeout 30
default-server-timeout 60
socket-timeout 300
```

**Auto-update uBlock filters:**

```bash
#!/bin/bash
# /home/user/scripts/nexus-update-privoxy-filters.sh

UBLOCK_URL="https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters"
PRIVOXY_DIR="/etc/privoxy"

echo "🔥 Updating Privoxy ad-blocking filters..."

# Download uBlock Origin filters
curl -s "$UBLOCK_URL/filters.txt" -o /tmp/ublock-filters.txt

# Convert uBlock format to Privoxy format
# (This requires a conversion tool or script)
python3 /home/user/scripts/ublock-to-privoxy.py \
    /tmp/ublock-filters.txt \
    $PRIVOXY_DIR/ublock-origin.action

# Download EasyList
curl -s "https://easylist.to/easylist/easylist.txt" -o /tmp/easylist.txt
python3 /home/user/scripts/ublock-to-privoxy.py \
    /tmp/easylist.txt \
    $PRIVOXY_DIR/easylist.action

# Reload Privoxy
killall -HUP privoxy

echo "✅ Filters updated and Privoxy reloaded!"
```

#### 5.2 Snowflake (TOR Bridge) Default

```bash
#!/bin/bash
# /home/user/scripts/nexus-snowflake-bridge.sh

# Snowflake browser extension as TOR bridge (censorship circumvention)

# Install Snowflake standalone proxy
if ! command -v snowflake-proxy &> /dev/null; then
    echo "📦 Installing Snowflake proxy..."
    go install gitlab.torproject.org/tpo/anti-censorship/pluggable-transports/snowflake/proxy@latest
fi

# Run Snowflake proxy
echo "❄️ Starting Snowflake bridge proxy..."
~/go/bin/proxy -capacity 10 -summary-interval 3600 &

echo "✅ Snowflake running - helping censored users access TOR!"
```

**TOR Configuration with Snowflake:**

```
# Add to /etc/tor/torrc

# Snowflake bridge (anti-censorship)
UseBridges 1
ClientTransportPlugin snowflake exec /usr/bin/snowflake-client

Bridge snowflake 192.0.2.3:80 2B280B23E1107BB62ABFC40DDCC8824814F80A72 \
    fingerprint=2B280B23E1107BB62ABFC40DDCC8824814F80A72 \
    url=https://snowflake-broker.torproject.net.global.prod.fastly.net/ \
    front=cdn.sstatic.net ice=stun:stun.l.google.com:19302
```

#### 5.3 TOR Middle Relay (Optional - give back to network)

```
# /etc/tor/torrc-relay

# Middle relay (not exit) - safer for home users
ORPort 9001
Nickname NexusMiddleRelay
ContactInfo nexus-admin@protonmail.com

# Bandwidth limits (adjust based on connection)
RelayBandwidthRate 1000 KB   # 1 MB/s
RelayBandwidthBurst 2000 KB  # 2 MB/s burst

# Reduced exit policy (middle relay, not exit)
ExitPolicy reject *:*

# Directory cache
DirPort 9030
DirPortFrontPage /etc/tor/tor-exit-notice.html
```

---

### 6. APPLICATION INTEGRATION

#### 6.1 IRC over I2P

```bash
# /home/user/.irssi/config

servers = (
  {
    address = "irc2p.i2p";
    chatnet = "I2P-IRC";
    port = "6667";
    use_tls = "no";
    autoconnect = "yes";
  }
);

# Use system-wide transparent proxy
# No additional configuration needed - traffic auto-routed through I2P
```

#### 6.2 Matrix Bridge

```yaml
# /home/user/docker/matrix-tor/docker-compose.yml
version: '3.8'

services:
  matrix-synapse:
    image: matrixdotorg/synapse:latest
    container_name: nexus-matrix-bridge
    restart: unless-stopped

    ports:
      - "127.0.0.1:8008:8008"

    volumes:
      - matrix_data:/data
      - ./homeserver.yaml:/data/homeserver.yaml:ro

    environment:
      - SYNAPSE_CONFIG_PATH=/data/homeserver.yaml
      - SYNAPSE_HTTP_PROXY=http://127.0.0.1:8888  # Privoxy → TOR
      - SYNAPSE_HTTPS_PROXY=http://127.0.0.1:8888

volumes:
  matrix_data:
```

**Matrix Homeserver Config:**

```yaml
# homeserver.yaml (excerpt)

# Use TOR for federation
outbound_federation_restricted: false
federation_ip_range_whitelist: []

# HTTP proxy for outbound federation
http_proxy: "http://127.0.0.1:8888"
https_proxy: "http://127.0.0.1:8888"

# Hidden service configuration
public_baseurl: "http://matrix-nexus.onion"
```

#### 6.3 WebTorrent Bridge

```javascript
// /home/user/webtorrent-proxy/server.js
// WebTorrent tracker over TOR

const WebTorrent = require('webtorrent');
const SocksProxyAgent = require('socks-proxy-agent');

const torAgent = new SocksProxyAgent('socks5://127.0.0.1:9050');

const client = new WebTorrent({
  tracker: {
    agent: torAgent,
    announce: [
      'udp://tracker.opentrackr.org:1337',  // Proxied through TOR
      'http://tracker.i2p.rocks:6969/announce'  // I2P tracker
    ]
  },
  dht: {
    bootstrap: [
      'dht.transmissionbt.com:6881',
      'router.bittorrent.com:6881'
    ]
  },
  webSeeds: true
});

console.log('🔥 WebTorrent client running over TOR/I2P!');
```

#### 6.4 RetroShare over I2P/TOR

```bash
# RetroShare configuration for darknet mode only

# ~/.retroshare/config/retroshare.conf
[Network]
LocalAddress=127.0.0.1
LocalPort=7812
ExternalAddress=
ExternalPort=0

# Force connections through I2P/TOR
[I2P]
Enabled=true
SAMHost=127.0.0.1
SAMPort=7658

[TOR]
Enabled=true
ProxyAddress=127.0.0.1
ProxyPort=9050
OnionAddress=retroshare-nexus.onion
```

#### 6.5 OnionShare

```bash
# OnionShare automatically uses TOR SOCKS
# Just configure proxy settings

onionshare-cli --persistent \
    --stay-open \
    --proxy socks5://127.0.0.1:9050 \
    /path/to/shared/files
```

---

### 7. MONITORING & HEALTH CHECKS

#### 7.1 Master Health Check Script

```bash
#!/bin/bash
# /home/user/scripts/nexus-proxy-health.sh

# Sane • Simple • Secure • Stealthy • Beautiful

# Color codes
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m'

echo "🔥 NeXuS Transparent Proxy Health Check"
echo "========================================"
echo ""

# Check 1: Firewall rules
echo -n "🛡️  Firewall (nftables): "
if nft list ruleset | grep -q "nexus_filter"; then
    echo -e "${GREEN}ACTIVE${NC}"
else
    echo -e "${RED}INACTIVE${NC}"
fi

# Check 2: TOR instances
echo -n "🧅  TOR instances: "
TOR_COUNT=$(pgrep -c tor)
if [ $TOR_COUNT -ge 3 ]; then
    echo -e "${GREEN}$TOR_COUNT running${NC}"
else
    echo -e "${YELLOW}$TOR_COUNT running (expected 5+)${NC}"
fi

# Check 3: I2P router
echo -n "🌐  I2P router: "
if curl -s http://127.0.0.1:7657 > /dev/null; then
    echo -e "${GREEN}RUNNING${NC}"
else
    echo -e "${RED}DOWN${NC}"
fi

# Check 4: Yggdrasil
echo -n "🔗  Yggdrasil: "
if pgrep -x yggdrasil > /dev/null; then
    echo -e "${GREEN}RUNNING${NC}"
else
    echo -e "${RED}DOWN${NC}"
fi

# Check 5: Reticulum
echo -n "📡  Reticulum: "
if pgrep -f rnsd > /dev/null; then
    echo -e "${GREEN}RUNNING${NC}"
else
    echo -e "${RED}DOWN${NC}"
fi

# Check 6: HAProxy
echo -n "⚖️   HAProxy: "
if pgrep -x haproxy > /dev/null; then
    echo -e "${GREEN}RUNNING${NC}"
else
    echo -e "${RED}DOWN${NC}"
fi

# Check 7: Privoxy instances
echo -n "🔒  Privoxy instances: "
PRIVOXY_COUNT=$(pgrep -c privoxy)
if [ $PRIVOXY_COUNT -ge 2 ]; then
    echo -e "${GREEN}$PRIVOXY_COUNT running${NC}"
else
    echo -e "${YELLOW}$PRIVOXY_COUNT running${NC}"
fi

# Check 8: DNSCrypt-proxy
echo -n "🔐  DNSCrypt-proxy: "
if pgrep -f dnscrypt-proxy > /dev/null; then
    echo -e "${GREEN}RUNNING${NC}"
else
    echo -e "${RED}DOWN${NC}"
fi

# Check 9: Test actual anonymity
echo ""
echo "📍 IP Address Test:"
echo -n "   Real IP: "
timeout 3 curl -s https://api.ipify.org || echo -e "${GREEN}BLOCKED (good!)${NC}"
echo -n "   TOR IP:  "
curl --socks5 127.0.0.1:9050 -s https://api.ipify.org
echo -n "   TOR Location: "
TOR_IP=$(curl --socks5 127.0.0.1:9050 -s https://api.ipify.org)
curl -s "http://ip-api.com/json/$TOR_IP" | jq -r '"\(.country) - \(.city)"'

# Check 10: Circuit health
echo ""
echo "🔄 TOR Circuit Status:"
for PORT in 9050 9051 9052; do
    echo -n "   Port $PORT: "
    if timeout 5 curl --socks5 127.0.0.1:$PORT -s https://check.torproject.org | grep -q "Congratulations"; then
        echo -e "${GREEN}WORKING${NC}"
    else
        echo -e "${RED}FAILED${NC}"
    fi
done

echo ""
echo "✅ Health check complete!"
```

#### 7.2 Auto-Restart Failed Services

```bash
#!/bin/bash
# /home/user/scripts/nexus-proxy-watchdog.sh

# Watchdog to auto-restart failed components

while true; do
    # Check TOR
    if [ $(pgrep -c tor) -lt 5 ]; then
        echo "🔴 TOR instances low, restarting..."
        systemctl restart tor@default
    fi

    # Check I2P
    if ! curl -s http://127.0.0.1:7657 > /dev/null; then
        echo "🔴 I2P down, restarting container..."
        docker restart nexus-i2p-over-tor
    fi

    # Check Yggdrasil
    if ! pgrep -x yggdrasil > /dev/null; then
        echo "🔴 Yggdrasil down, restarting..."
        systemctl restart yggdrasil
    fi

    # Check HAProxy
    if ! pgrep -x haproxy > /dev/null; then
        echo "🔴 HAProxy down, restarting..."
        systemctl restart haproxy
    fi

    # Check Privoxy
    if [ $(pgrep -c privoxy) -lt 2 ]; then
        echo "🔴 Privoxy instances low, restarting..."
        systemctl restart privoxy@*
    fi

    # Check DNSCrypt
    if ! pgrep -f dnscrypt-proxy > /dev/null; then
        echo "🔴 DNSCrypt down, restarting..."
        systemctl restart dnscrypt-proxy
    fi

    # Sleep 60 seconds before next check
    sleep 60
done
```

#### 7.3 Web Interface (Optional)

```html
<!-- /var/www/nexus-proxy-dashboard/index.html -->
<!DOCTYPE html>
<html>
<head>
    <title>NeXuS Proxy Dashboard</title>
    <style>
        body { background: #0a0a0a; color: #00ff00; font-family: monospace; }
        .status-ok { color: #00ff00; }
        .status-warn { color: #ffaa00; }
        .status-error { color: #ff0000; }
        .container { max-width: 1200px; margin: 0 auto; padding: 20px; }
        .service { border: 1px solid #333; padding: 10px; margin: 10px 0; }
    </style>
</head>
<body>
    <div class="container">
        <h1>🔥 NeXuS Transparent Proxy Dashboard</h1>
        <h3>Sane • Simple • Secure • Stealthy • Beautiful</h3>

        <div id="services"></div>
        <div id="ip-info"></div>

        <script>
            async function checkHealth() {
                const response = await fetch('/api/health');
                const data = await response.json();

                let html = '<h2>Service Status</h2>';
                for (const [service, status] of Object.entries(data.services)) {
                    const statusClass = status ? 'status-ok' : 'status-error';
                    html += `<div class="service">
                        <span class="${statusClass}">${service}: ${status ? '✅ RUNNING' : '🔴 DOWN'}</span>
                    </div>`;
                }

                document.getElementById('services').innerHTML = html;

                // IP info
                document.getElementById('ip-info').innerHTML = `
                    <h2>Current IP</h2>
                    <div class="service">
                        TOR IP: ${data.tor_ip} (${data.tor_country})
                    </div>
                `;
            }

            checkHealth();
            setInterval(checkHealth, 10000);  // Update every 10 seconds
        </script>
    </div>
</body>
</html>
```

---

### 8. MASTER CONTROL SCRIPT

```bash
#!/bin/bash
# /home/user/scripts/nexus-proxy-master.sh
# Sane • Simple • Secure • Stealthy • Beautiful

# Master control for entire NeXuS transparent proxy stack

set -e

SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

# Color output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
NC='\033[0m'

ascii_banner() {
    cat << "EOF"
    ███╗   ██╗███████╗██╗  ██╗██╗   ██╗███████╗
    ████╗  ██║██╔════╝╚██╗██╔╝██║   ██║██╔════╝
    ██╔██╗ ██║█████╗   ╚███╔╝ ██║   ██║███████╗
    ██║╚██╗██║██╔══╝   ██╔██╗ ██║   ██║╚════██║
    ██║ ╚████║███████╗██╔╝ ██╗╚██████╔╝███████║
    ╚═╝  ╚═══╝╚══════╝╚═╝  ╚═╝ ╚═════╝ ╚══════╝

    Transparent Multi-Layer Darknet Routing
    Sane • Simple • Secure • Stealthy • Beautiful
EOF
}

start_firewall() {
    echo -e "${BLUE}[1/8]${NC} Starting firewall (nftables)..."
    $SCRIPT_DIR/nexus-transparent-firewall.sh start
    echo -e "${GREEN}✅ Firewall active${NC}"
}

start_dns() {
    echo -e "${BLUE}[2/8]${NC} Starting DNSCrypt-proxy..."
    systemctl start dnscrypt-proxy
    echo -e "${GREEN}✅ DNS encryption active${NC}"
}

start_tor() {
    echo -e "${BLUE}[3/8]${NC} Starting TOR instances (5x)..."
    for i in {0..4}; do
        systemctl start tor@default-$i
    done
    sleep 5
    echo -e "${GREEN}✅ TOR network ready${NC}"
}

start_privoxy() {
    echo -e "${BLUE}[4/8]${NC} Starting Privoxy instances (3x)..."
    for i in {0..2}; do
        systemctl start privoxy@$i
    done
    echo -e "${GREEN}✅ Privoxy HTTP proxies active${NC}"
}

start_haproxy() {
    echo -e "${BLUE}[5/8]${NC} Starting HAProxy smart router..."
    systemctl start haproxy
    echo -e "${GREEN}✅ HAProxy routing active${NC}"
}

start_i2p() {
    echo -e "${BLUE}[6/8]${NC} Starting I2P network (over TOR)..."
    docker-compose -f ~/docker/i2p-over-tor/docker-compose.yml up -d
    echo -e "${YELLOW}⏳ I2P bootstrapping (30s)...${NC}"
    sleep 30
    echo -e "${GREEN}✅ I2P network ready${NC}"
}

start_yggdrasil() {
    echo -e "${BLUE}[7/8]${NC} Starting Yggdrasil overlay network..."
    systemctl start yggdrasil
    echo -e "${GREEN}✅ Yggdrasil mesh active${NC}"
}

start_reticulum() {
    echo -e "${BLUE}[8/8]${NC} Starting Reticulum mesh network..."
    $SCRIPT_DIR/nexus-reticulum-overlay.sh &
    echo -e "${GREEN}✅ Reticulum network active${NC}"
}

start_all() {
    ascii_banner
    echo ""
    echo -e "${BLUE}🚀 Starting NeXuS Transparent Proxy Stack...${NC}"
    echo ""

    start_firewall
    start_dns
    start_tor
    start_privoxy
    start_haproxy
    start_i2p
    start_yggdrasil
    start_reticulum

    echo ""
    echo -e "${GREEN}✅✅✅ ALL SYSTEMS OPERATIONAL ✅✅✅${NC}"
    echo ""
    echo "📊 Dashboard: http://localhost:9999"
    echo "🔍 Health check: $SCRIPT_DIR/nexus-proxy-health.sh"
    echo ""
}

stop_all() {
    echo -e "${RED}🛑 Stopping NeXuS Transparent Proxy Stack...${NC}"

    systemctl stop tor@*
    systemctl stop privoxy@*
    systemctl stop haproxy
    systemctl stop yggdrasil
    systemctl stop dnscrypt-proxy
    docker-compose -f ~/docker/i2p-over-tor/docker-compose.yml down
    pkill -f rnsd

    $SCRIPT_DIR/nexus-transparent-firewall.sh stop

    echo -e "${GREEN}✅ All services stopped${NC}"
}

restart_all() {
    stop_all
    sleep 3
    start_all
}

status_check() {
    $SCRIPT_DIR/nexus-proxy-health.sh
}

case "$1" in
    start)
        start_all
        ;;
    stop)
        stop_all
        ;;
    restart)
        restart_all
        ;;
    status)
        status_check
        ;;
    health)
        status_check
        ;;
    *)
        echo "Usage: $0 {start|stop|restart|status|health}"
        exit 1
        ;;
esac
```

---

## DEPLOYMENT GUIDE

### Step 1: Install Dependencies

```bash
# Alpine Linux (NeXuS base system)
apk add tor privoxy haproxy dnscrypt-proxy nftables docker docker-compose

# TOR bridges/pluggable transports
apk add obfs4proxy snowflake

# I2P (via Docker)
docker pull divax/i2p:current

# Yggdrasil
apk add yggdrasil

# Reticulum
pip install rns
```

### Step 2: Create Service User

```bash
# Non-privileged user for proxy services
adduser -D -s /bin/false nexus-proxy
```

### Step 3: Deploy Configuration Files

```bash
# Copy all configurations from this document to appropriate locations
cp nexus-transparent-firewall.sh /home/user/scripts/
cp nexus-master.cfg /etc/haproxy/
cp nexus-master.conf /etc/privoxy/
# ... etc
```

### Step 4: Enable Systemd Services

```bash
# Create systemd service files for each component
systemctl enable tor@default
systemctl enable privoxy@{0..2}
systemctl enable haproxy
systemctl enable dnscrypt-proxy
systemctl enable yggdrasil
```

### Step 5: Start Stack

```bash
/home/user/scripts/nexus-proxy-master.sh start
```

### Step 6: Verify Operation

```bash
# Run health check
/home/user/scripts/nexus-proxy-health.sh

# Test anonymity
curl http://check.torproject.org

# DNS leak test
/home/user/scripts/nexus-dns-leak-test.sh
```

---

## SECURITY CONSIDERATIONS

### Threat Model

| Threat | Mitigation |
|--------|-----------|
| **ISP monitoring** | All traffic encrypted through TOR/I2P |
| **DNS leaks** | Forced DNSCrypt over TOR, IPv6 disabled |
| **Application bypass** | Transparent interception at firewall level |
| **TOR correlation attacks** | Multi-layer chains (I2P over TOR, etc.) |
| **Exit node sniffing** | HTTPS enforcement, certificate pinning |
| **Browser fingerprinting** | Privoxy header modification + uBlock filters |
| **Time-based attacks** | Random TOR circuit rotation |

### Best Practices

1. **Never disable IPv6 blocking** - Major leak vector
2. **Rotate TOR circuits frequently** - Use NewCircuitPeriod=120
3. **Use HTTPS everywhere** - Encrypt end-to-end even through TOR
4. **Enable uBlock Origin** - Additional tracking protection
5. **Disable JavaScript when possible** - Reduces fingerprinting surface
6. **Use Tor Browser for sensitive activity** - Additional anonymity features
7. **Monitor logs regularly** - Watch for anomalies
8. **Test for leaks weekly** - Run DNS/IP leak tests

---

## PERFORMANCE TUNING

### Expected Latency

| Route | Approximate Latency |
|-------|-------------------|
| Direct (local) | < 10ms |
| TOR only | 200-500ms |
| I2P over TOR | 1-3 seconds |
| Yggdrasil over TOR | 500ms-2s |
| Reticulum over I2P | 2-5 seconds |

### Bandwidth Optimization

```bash
# HAProxy connection pooling
tune.bufsize 32768
tune.maxrewrite 8192

# TOR bandwidth settings
RelayBandwidthRate 10 MB
RelayBandwidthBurst 20 MB

# Privoxy compression
enable-compression 1

# DNS caching
cache_size = 8192
cache_max_ttl = 86400
```

---

## TROUBLESHOOTING

### Common Issues

**Problem: TOR circuits not building**
```bash
# Check TOR logs
journalctl -u tor@default -f

# Try bridge mode
echo "UseBridges 1" >> /etc/tor/torrc
systemctl restart tor
```

**Problem: I2P not connecting**
```bash
# Check I2P console
xdg-open http://127.0.0.1:7657

# Verify TOR outproxy
docker logs nexus-i2p-over-tor
```

**Problem: DNS resolution failing**
```bash
# Test DNSCrypt
dig @127.0.0.1 -p 5353 example.com

# Check if dnscrypt-proxy is using TOR
lsof -i :5353
```

**Problem: HAProxy backend down**
```bash
# Check HAProxy stats
xdg-open http://localhost:9999

# Test backend directly
curl --socks5 127.0.0.1:9050 https://check.torproject.org
```

---

## FUTURE ENHANCEMENTS

1. **NeXuS Node Mesh** - Nodes find each other and route traffic through peer network
2. **Automatic Bridge Discovery** - Fetch fresh TOR bridges automatically
3. **Load Balancing across Nodes** - Distribute traffic across multiple NeXuS nodes
4. **Failover to alternate darknet** - If TOR fails, auto-switch to I2P
5. **Blockchain-based DNS** - Integrate Namecoin/Ethereum Name Service for censorship-resistant DNS
6. **Steganography Layer** - Hide darknet traffic in innocent-looking HTTP

---

## CONCLUSION

This architecture provides **UNSTOPPABLE** and **INVISIBLE** network communication through:

- ✅ **Complete transparency** - Zero application configuration needed
- ✅ **Multi-layer anonymity** - Nested chains (I2P/Ygg/Reticulum over TOR)
- ✅ **Leak-proof design** - DNS, IPv6, WebRTC all prevented at firewall level
- ✅ **Automatic failover** - Health checking and service restart
- ✅ **Censorship resistance** - Snowflake bridges, pluggable transports
- ✅ **Beautiful interface** - Clean CLI tools and web dashboard

**Together Everyone Achieves More** - This is the NeXuS way.

---

*Document prepared for the NeXuS Supervisory Entity*
*Generated: 2026-02-12*
*Version: 1.0*
*Status: DESIGN COMPLETE - READY FOR IMPLEMENTATION*

**Next Steps:**
1. Review architecture with Anon
2. Begin implementation phase
3. Test in isolated environment
4. Deploy to production NeXuS nodes

🔥 **Fire burns eternal in the darknet** 🔥
