# 🌀 NeXuS Medusa Proxy - Future Roadmap

## Completed ✅

- ✅ Multi-network routing (Tor, Snowflake, I2P, Yggdrasil, Reticulum)
- ✅ Automagical self-healing router
- ✅ Protocol-specific routing rules
- ✅ Application support (DeltaChat, OnionShare, Briar, RetroShare, IRC, BitTorrent, Matrix)
- ✅ Comprehensive documentation

## Planned Features

### 0. OpenSnitch Application Firewall 🛡️

**Priority:** High
**Status:** Planned

Interactive application firewall for monitoring and controlling outbound connections:
- Per-application network access control
- Visual alerts for unauthorized connection attempts
- Integration with NeXuS routing (enforce Tor-only policy)
- Block clearnet access by default, allow with user approval + warnings
- Containerized deployment with persistent rules

**Security Benefits:**
- Prevents accidental clearnet leaks
- User awareness of all network activity
- Enforces "no clearnet unless explicitly approved" policy
- Application-level network segmentation

---

### 1. IPFS Integration 🌐

**Vision:** Add IPFS (InterPlanetary File System) as 6th anonymity network

**Why IPFS + NeXuS:**
- Distributed content addressing (censorship-resistant)
- Peer-to-peer file storage and sharing
- Content permanently available (no takedowns)
- Perfect for Smart Scrolls hosting
- Works over Tor/I2P/Yggdrasil

**Implementation:**
```python
# proxy/ipfs.py
class IPFS(Service):
    - Run IPFS daemon (go-ipfs or kubo)
    - SOCKS proxy support (run over Tor/I2P)
    - HTTP gateway on port 8080
    - Swarm listening on port 4001
    - Register with SmartRouter (priority: 25)
```

**Features:**
- Content-addressed storage (ipfs://CID)
- IPFS gateway accessible from all networks
- Pin important content (keep it available)
- Distributed hosting for Smart Scrolls
- Participate in IPFS network (DHT routing)

**Use Cases:**
- Host Smart Scrolls on IPFS (permanent, distributed)
- Share files censorship-resistant
- Distributed documentation
- NeXuS node discovery via IPFS DHT

**Priority:** High (implements with Smart Scrolls)

**Network Priority Chain (updated):**
1. Tor (10)
2. Snowflake (15)
3. I2P (20)
4. **IPFS (25)** ← NEW
5. Yggdrasil (30)
6. Reticulum (40)

---

### 2. Smart Scrolls - Multi-Network Web Hosting 📜

**Vision:** Easy-to-use web hosting template accessible from ALL anonymity networks simultaneously

**Features:**
- Single content source, multiple network addresses
- Tor hidden service (.onion)
- I2P eepsite (.i2p)
- Yggdrasil IPv6 address
- Optional clearnet access (via exit nodes)

**Implementation Ideas:**
```
Smart Scroll Architecture:
    Content (HTML/Markdown)
           ↓
    Static Site Generator
           ↓
    ┌──────┴──────┐
    ↓      ↓      ↓
  Tor    I2P   Yggdrasil
.onion  .i2p   [IPv6]
```

**Use Cases:**
- Censorship-resistant publishing
- Anonymous blogs/documentation
- NeXuS node info pages
- Distributed knowledge base
- Resistance manifestos

**Technical Approach:**
- Lightweight web server (nginx/caddy)
- Auto-configure hidden services on all networks
- Simple template system (Markdown → HTML)
- One command: `nexus-scroll publish document.md`
- Outputs all network addresses

**User Experience:**
```bash
# Create a scroll
nexus-scroll create my-manifesto

# Edit content
vim scrolls/my-manifesto/index.md

# Publish to all networks
nexus-scroll publish my-manifesto

# Output:
# 🌀 Smart Scroll Published!
# Tor:       http://abc123...onion
# I2P:       http://xyz789...i2p
# Yggdrasil: http://[200:1234::5678]
# Status:    Accessible from all networks ✅
```

**Priority:** Medium (foundation is built, this is application layer)

---

### 2. B.A.T.M.A.N. Advanced Mesh

**Status:** Partially planned
**Purpose:** Layer 2 mesh routing for local network resilience
**Priority:** Low (nice-to-have, local mesh only)

**Implementation:**
- `proxy/batman.py` service class
- Kernel module management
- Mesh interface configuration
- Integration with Yggdrasil for overlay

---

### 3. Enhanced Dockerfile

**Status:** Planned
**Purpose:** Easy Docker deployment with all dependencies
**Priority:** High (needed for production deployment)

**Tasks:**
- Add i2pd package
- Add yggdrasil package
- Add reticulum package (rnsd)
- Add snowflake-proxy
- Add batman-adv (optional)
- Multi-stage build for smaller images
- Health checks for all services

**Estimated Size:**
- Base Alpine: ~100MB
- With all networks: ~300MB
- Multi-stage optimized: ~200MB

---

### 4. Web Dashboard

**Purpose:** Monitor all networks from web interface
**Priority:** Medium

**Features:**
- Network health status display
- Traffic statistics
- Routing decisions log
- Configuration editor
- Start/stop services

**Technology:**
- Lightweight backend (Flask/FastAPI)
- Real-time updates (WebSocket)
- Accessible via Tor/I2P (dogfooding!)

---

### 5. Mobile Support

**Purpose:** Run NeXuS on mobile devices
**Priority:** Low (future consideration)

**Challenges:**
- Battery life (background services)
- Network restrictions (Android/iOS)
- Resource constraints

**Approach:**
- Termux on Android (full NeXuS)
- iOS app (lightweight client only)

---

### 6. Network Bridges

**Purpose:** Allow cross-network communication
**Priority:** Medium

**Features:**
- Tor → I2P gateway (access .i2p from Tor)
- I2P → Tor gateway (access .onion from I2P)
- Yggdrasil → Clearnet gateway
- Reticulum → IP gateway

**Architecture:**
```
Tor Browser → NeXuS Bridge → I2P Network
    ↓                            ↓
Access http://example.i2p from Tor Browser
```

---

### 7. App Integration Guides

**Status:** Routing rules done, detailed guides pending
**Priority:** Medium

**Guides Needed:**
- RetroShare over I2P/Tor setup
- Briar via NeXuS proxy
- DeltaChat configuration
- BitTorrent client setup
- IRC client (Irssi/WeeChat) configuration
- Matrix homeserver behind NeXuS

---

### 8. Performance Optimizations

**Areas for Improvement:**
- HAProxy configuration tuning
- I2P bandwidth allocation
- Tor circuit building optimization
- Smart router algorithm improvements
- Caching layer for frequently accessed content

---

### 9. Security Enhancements

**Features:**
- Automatic updates for security patches
- Anomaly detection (unusual traffic patterns)
- Tor exit node fingerprinting detection
- I2P tunnel encryption verification
- Network isolation (separate namespaces)

---

### 10. Distributed Hash Table (DHT)

**Purpose:** Service discovery across NeXuS nodes
**Priority:** Low (future research)

**Vision:**
- Find other NeXuS nodes automatically
- Discover services (smart scrolls, bridges, etc.)
- Peer-to-peer coordination
- No central directory needed

**Technology:**
- Kademlia-based DHT
- Accessible via all networks
- Privacy-preserving (no identity leakage)

---

## Smart Scrolls - Detailed Design (Future Implementation)

### Template System

**Simple Markdown-based:**
```yaml
# scroll.yaml
title: "My Censorship-Resistant Blog"
author: "Anonymous"
networks:
  - tor
  - i2p
  - yggdrasil
content: index.md
theme: minimal
```

**Auto-generated structure:**
```
scrolls/
  my-blog/
    scroll.yaml       # Configuration
    index.md          # Main content
    about.md          # Additional pages
    assets/           # Images, CSS
    .tor/             # Generated Tor config
    .i2p/             # Generated I2P config
    .yggdrasil/       # Generated Yggdrasil config
```

### Publishing Workflow

```bash
# Initialize
nexus-scroll init my-blog

# Edit content
cd scrolls/my-blog
vim index.md

# Preview locally
nexus-scroll serve my-blog
# Opens: http://localhost:8080

# Publish to networks
nexus-scroll publish my-blog
# Configures Tor hidden service
# Configures I2P eepsite
# Configures Yggdrasil web server
# Returns all addresses

# Update existing scroll
vim index.md
nexus-scroll update my-blog
# Content updated on all networks
```

### Network Configuration

**Tor Hidden Service:**
```
# Auto-generated in /var/lib/tor/my-blog/
HiddenServiceDir /var/lib/tor/scrolls/my-blog
HiddenServicePort 80 127.0.0.1:8080
```

**I2P Eepsite:**
```
# Auto-configured I2P tunnel
tunnel.type=http
tunnel.name=my-blog
tunnel.targetPort=8080
```

**Yggdrasil:**
```
# Web server listening on Yggdrasil IPv6
Listen: [yggdrasil-ipv6]:80
```

### Content Formats

**Supported:**
- Markdown (primary)
- HTML (direct)
- Static sites (Hugo, Jekyll, etc.)
- Single-page apps (React, Vue)

**Features:**
- Automatic HTTPS on clearnet
- Always-on availability
- Version control (git integration)
- Multi-language support
- Search engine (local)

### Discovery

**How users find scrolls:**
- Share .onion/.i2p addresses directly
- NeXuS directory (optional centralized listing)
- DHT lookup (future: distributed discovery)
- Word of mouth (most secure)

### Privacy Considerations

**Author Privacy:**
- No analytics by default
- No tracking scripts
- No identifying metadata
- Timestamps optional
- Author pseudonymous

**Reader Privacy:**
- No JavaScript tracking
- No third-party resources
- No CDNs
- No cookies
- Tor/I2P/Yggdrasil anonymity preserved

---

## Timeline (Rough Estimates)

**Q1 2026:**
- ✅ Core multi-network routing (DONE)
- ✅ Smart router implementation (DONE)
- ✅ Documentation (DONE)

**Q2 2026:**
- Dockerfile with all dependencies
- Smart Scrolls MVP (basic multi-network hosting)
- App integration guides

**Q3 2026:**
- Web dashboard for monitoring
- B.A.T.M.A.N. Advanced integration
- Network bridges (Tor↔I2P)

**Q4 2026:**
- Performance optimizations
- Security enhancements
- DHT service discovery (research)

---

## Community Contributions Welcome

Areas where help is needed:
- Testing on different platforms
- Additional network integrations
- Documentation improvements
- Translation to other languages
- Security audits
- Performance benchmarking

---

🌀 **Together Everyone Achieves More** 🌀

*Building the censorship-resistant future, one feature at a time.*
