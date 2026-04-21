<!-- NEXUS-META
keywords: darknet stack diagram architecture tor i2p medusa yggdrasil reticulum privoxy ghost-gate hydra haproxy opensnitch grafana prometheus retroshare irc ipfs hosting
projects: nexus-darknet nexus-hydra nexus-node
status: active
type: spec
date: 2026-03-24
authors: claude
-->

# NeXuS Darknet Stack — Full Architecture Diagram

*Every layer. Every service. Every connection.*
*Source: ~/claude/configs/nexus-stack/ + ~/git/nexus-hydra/*

---

## Complete Stack Map

```
╔═════════════════════════════════════════════════════════════════════╗
║                        USER / APPLICATION                           ║
║   Browser • CLI • Character Gen • DIVA Node • WeeChat • RetroShare  ║
╚══════════════════════════╦══════════════════════════════════════════╝
                           ║
╔══════════════════════════╩══════════════════════════════════════════╗
║                      CONTROL PLANE                                  ║
║                                                                     ║
║  nexus-darknet (Go TUI)       nexus-hydra (Python/FastAPI)          ║
║  Bubble Tea · Lip Gloss       Web UI :8800 · REST API :8801         ║
║  5 tabs: Networks/Services/   WebSocket real-time feed              ║
║  Hosting/Jurisdiction/Log     Podman container management           ║
║                                                                     ║
║  nexus-command-center.sh      nexus-darknet-router.sh               ║
║  (launch/orchestrate)         (contract: start|stop|status|         ║
║                                expose|hide|address)                 ║
╚══════════════════════════╦══════════════════════════════════════════╝
                           ║
╔══════════════════════════╩══════════════════════════════════════════╗
║                      MONITORING PLANE                               ║
║                                                                     ║
║  Prometheus :9090             Grafana (nexus-darknet dashboard)     ║
║  └─ scrapes all services      └─ visualizes traffic, health, peers  ║
║                                                                     ║
║  HAProxy Stats :8404          OpenSnitch :50051                     ║
║  └─ /stats live dashboard     └─ per-app outbound firewall          ║
║  └─ /metrics prometheus       └─ NeXuS rules: allow Tor/I2P/VPN   ║
║  └─ /health endpoint          └─ alert + block unknown connections  ║
╚══════════════════════════╦══════════════════════════════════════════╝
                           ║
╔══════════════════════════╩══════════════════════════════════════════╗
║                       EGRESS GATE                                   ║
║                                                                     ║
║  Ghost Gate (nft rules — kernel level)                              ║
║  ├─ Default POLICY DROP on all outbound                             ║
║  ├─ Cerberus 3-head: ai_fingerprint + operating_key + user_permit  ║
║  ├─ lo → ACCEPT                                                     ║
║  ├─ ct state established,related → ACCEPT                           ║
║  ├─ Ghost Gate permit → route to transport layer                    ║
║  └─ Denied → randomized noise sink (not clean drop)                ║
╚══════════════════════════╦══════════════════════════════════════════╝
                           ║
╔══════════════════════════╩══════════════════════════════════════════╗
║                   FILTER & PRIVACY LAYER                            ║
║                                                                     ║
║  Privoxy :8118                    OpenSnitch (also here)            ║
║  ├─ Ad/tracker blocking           ├─ Kill switch per process        ║
║  ├─ Header sanitization           └─ Blocks unknown egress          ║
║  ├─ privoxy-medusa.conf                                             ║
║  └─ Forwards to → HAProxy SOCKS layer                               ║
╚══════════════════════════╦══════════════════════════════════════════╝
                           ║
╔══════════════════════════╩══════════════════════════════════════════╗
║              MEDUSA SMART ROUTING LAYER                             ║
║          (datawookie/medusa-proxy — attribution)                     ║
║                                                                     ║
║  HAProxy Smart Router                                               ║
║  ├─ SOCKS5 unified entry :9050 (smart) / :1080 (medusa direct)     ║
║  ├─ HTTP proxy :8888                                                ║
║  ├─ Stats :8404 + Prometheus /metrics                               ║
║  ├─ Domain-based ACL routing:                                       ║
║  │    *.onion  → Tor pool                                           ║
║  │    *.i2p    → I2P proxy :4444                                    ║
║  │    *.ygg    → Yggdrasil                                          ║
║  │    other    → active Medusa profile                              ║
║  └─ Runtime config via admin socket /var/run/haproxy/admin.sock    ║
║                                                                     ║
║  ┌───────────────┬─────────────────┬───────────────────────────┐   ║
║  │  🌍 World     │   🇺🇸 US        │   👁 No-Eyes              │   ║
║  │  All exits    │  USA exits only  │  Exclude 5/9/14-Eyes      │   ║
║  │  medusa-proxy │  medusa-us       │  medusa-noeyes            │   ║
║  │  HEADS=2      │  HEADS=2         │  HEADS=2                  │   ║
║  │  TORS=3       │  TORS=3          │  TORS=3                   │   ║
║  └───────────────┴─────────────────┴───────────────────────────┘   ║
║                                                                     ║
║  Profile → jurisdiction/applier → torrc ExcludeExitNodes → reload  ║
╚══════════════════════════╦══════════════════════════════════════════╝
                           ║
╔══════════════════════════╩══════════════════════════════════════════╗
║                     TRANSPORT LAYER                                 ║
║                                                                     ║
║  ┌─────────────────────────────────────────────────────────────┐   ║
║  │  TOR                                                         │   ║
║  │  6 circuit pool (nexus-tor-01..06, osminogin/tor-simple)    │   ║
║  │  Middle relay only — NO exit node                           │   ║
║  │  Internal SOCKS :9050/9150  •  DNS :8853                    │   ║
║  │  Unbound → Tor DNSPort (no cleartext DNS leaks)             │   ║
║  └─────────────────────────────────────────────────────────────┘   ║
║                                                                     ║
║  ┌─────────────────────────────────────────────────────────────┐   ║
║  │  I2P  (i2pd C++ router — purplei2p/i2pd)                    │   ║
║  │  Web console :7070  •  HTTP proxy :4444  •  SOCKS :4447     │   ║
║  │  SAM bridge :7656 (DIVA chain connection)                   │   ║
║  │  Outproxy: i2pd-outproxy.conf (14-eyes aware)               │   ║
║  │  Tunnels: tunnels.conf (RetroShare, IRC, DIVA)              │   ║
║  │  DIVA nodes: nexus-i2p-http + nexus-i2p-udp (separate)     │   ║
║  └─────────────────────────────────────────────────────────────┘   ║
║                                                                     ║
║  ┌─────────────────────────────────────────────────────────────┐   ║
║  │  YGGDRASIL                                                   │   ║
║  │  End-to-end encrypted IPv6 overlay mesh                     │   ║
║  │  Self-assigned 200::/7 address                              │   ║
║  │  yggdrasil.conf — peer list, listen addresses               │   ║
║  └─────────────────────────────────────────────────────────────┘   ║
║                                                                     ║
║  ┌─────────────────────────────────────────────────────────────┐   ║
║  │  RETICULUM                                                   │   ║
║  │  Low-bandwidth mesh — LoRa/radio/serial capable             │   ║
║  │  Delay-tolerant networking                                  │   ║
║  │  reticulum.conf                                             │   ║
║  └─────────────────────────────────────────────────────────────┘   ║
╚══════════════════════════╦══════════════════════════════════════════╝
                           ║
                    THE INTERNET
             clearnet / darknet / mesh / radio
```

---

## Application Services Layer

```
┌─────────────────────────────────────────────────────────────────────┐
│  COMMUNICATION                                                       │
│                                                                      │
│  RetroShare :7812   I2P+Tor   F2F social/fileshare                  │
│  ├─ Tor: HiddenServicePort 7812, circuit isolation                  │
│  └─ I2P: dedicated tunnel in tunnels.conf                           │
│                                                                      │
│  IRC (WeeChat)      I2P       irc.dg.i2p / irc.echelon.i2p         │
│  ├─ Primary: I2P (native infrastructure, better latency)            │
│  └─ Secondary: Tor SOCKS fallback                                   │
│                                                                      │
│  Matrix/Synapse :8008  Tor    Self-hosted homeserver                │
│                                                                      │
│  Jitsi :8443        Tor       Self-hosted video (hidden service)    │
├─────────────────────────────────────────────────────────────────────┤
│  DATA & STORAGE                                                      │
│                                                                      │
│  IPFS :5001/:8080   Mixed     Distributed file storage              │
│  WebTorrent         I2P+Tor   P2P file sharing                      │
│  webtorrent.json — tracker config                                   │
├─────────────────────────────────────────────────────────────────────┤
│  NEXUS NATIVE                                                        │
│                                                                      │
│  DIVA Node          I2P       Blockchain via SAM :7656               │
│  ├─ nexus-i2p-http (HTTP transport)                                 │
│  └─ nexus-i2p-udp  (UDP transport)                                  │
│                                                                      │
│  Character Gen      Ollama    Local AI — zero clearnet               │
│  Ollama :11434      localhost No external calls                     │
│                                                                      │
│  Node Discovery     Mesh      nexus-node-discovery.sh               │
│  └─ nexus-node-mesh.conf — peer mesh config                         │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Darknet Hosting Flow

```
User exposes service → TUI Hosting tab → hosting/manager.go
         │
         ├─► Tor Hidden Service
         │     torrc: HiddenServiceDir + HiddenServicePort
         │     signal SIGHUP → generates hostname file
         │     result: abc123def456.onion
         │
         ├─► I2P Eepsite
         │     i2pd tunnels.conf: [service] type=http host=127.0.0.1 port=N
         │     signal reload → generates .b32.i2p address
         │     result: xyz789abc.b32.i2p
         │
         └─► Yggdrasil Endpoint
               listen address in yggdrasil.conf
               result: 200::xxxx:xxxx:xxxx:xxxx
```

---

## Jurisdiction Policy Flow

```
TUI Tab 4: select jurisdiction
         │
         ▼
jurisdiction/applier.go
         │
         ├─► torrc
         │     ExcludeExitNodes {US},{GB},{CA},{AU},{NZ}
         │     ExcludeNodes {US},{GB},{CA},{AU},{NZ}
         │     → kill -SIGHUP $(pidof tor)
         │
         ├─► i2pd-outproxy.conf
         │     excludeCountries = US,GB,CA,AU,NZ
         │     → reload i2pd
         │
         └─► Medusa profile switch
               world    → medusa-proxy    (no exclusions)
               us       → medusa-us       (US exits only)
               no-5     → medusa-noeyes   (ExcludeExitNodes 5-eyes)
               no-9     → medusa-noeyes   (+ FR,NL,DK,NO)
               no-14    → medusa-noeyes   (+ DE,BE,IT,SE,ES)
               custom   → medusa-noeyes   (user-defined list)
```

---

## Port Reference

| Port | Service | Purpose |
|------|---------|---------|
| :1080 | Medusa SOCKS5 | Main proxy entry (direct) |
| :8888 | Medusa HTTP | HTTP proxy |
| :8404 | HAProxy Stats | Live dashboard + Prometheus |
| :2090 | Medusa Stats | HAProxy runtime stats |
| :9050 | HAProxy Smart | Domain-aware SOCKS5 router |
| :8118 | Privoxy | Ad/tracker filter |
| :7070 | I2P Console | i2pd web management |
| :7656 | I2P SAM | App integration (DIVA) |
| :4444 | I2P HTTP | Browse .i2p sites |
| :4447 | I2P SOCKS | SOCKS proxy for I2P |
| :8853 | Tor DNS | DNS over Tor (no leaks) |
| :9090 | Prometheus | Metrics scraping |
| :8800 | Hydra Web | Browser interface |
| :8801 | Hydra API | REST + WebSocket |
| :50051 | OpenSnitch | gRPC daemon |
| :7812 | RetroShare | F2F P2P (via Tor/I2P) |
| :6667 | IRC | WeeChat → I2P tunnel |
| :8008 | Matrix | Synapse homeserver |
| :5001 | IPFS API | IPFS node |
| :11434 | Ollama | Local AI |

---

## Config File Map

| Config | Location | Purpose |
|--------|---------|---------|
| `medusa-haproxy.cfg` | nexus-stack/ | Standard HAProxy |
| `medusa-haproxy-smart.cfg` | nexus-stack/ | Domain-ACL smart router |
| `privoxy-medusa.conf` | nexus-stack/ | Privoxy → Medusa chain |
| `i2pd-outproxy.conf` | nexus-stack/ | I2P outproxy settings |
| `dockerfiles/i2p/tunnels.conf` | nexus-stack/ | I2P tunnel definitions |
| `yggdrasil.conf` | nexus-stack/ | Yggdrasil peer config |
| `reticulum.conf` | nexus-stack/ | Reticulum interfaces |
| `opensnitch-config.json` | nexus-stack/ | OpenSnitch rules |
| `configs/01-retroshare.conf` | nexus-stack/ | RetroShare over Tor+I2P |
| `configs/02-irc-complete.conf` | nexus-stack/ | IRC over I2P guide |
| `configs/03-webtorrent.conf` | nexus-stack/ | WebTorrent config |
| `torrc-outproxy` | nexus-stack/ | Tor exit policy |
| `bypass-rules.json` | nexus-stack/ | HAProxy bypass rules |
| `prometheus.yml` | nexus-stack/ | Metrics scrape config |
| `alertmanager.yml` | nexus-stack/ | Alert routing |

---

## What Goes Into the Go TUI

**Tab 1 — Networks:** Tor, I2P, Yggdrasil, Reticulum + Privoxy, Ghost Gate, OpenSnitch, Medusa toggles + status

**Tab 2 — Services:** RetroShare, IRC, Matrix, Jitsi, IPFS, WebTorrent, DIVA, Ollama, Character Gen — each with toggle + port + network

**Tab 3 — Hosting:** Tor hidden services, I2P eepsites, Yggdrasil endpoints — manage, view addresses, QR codes

**Tab 4 — Jurisdiction:** World/US/5-eyes/9-eyes/14-eyes/Custom — applies to Tor + I2P + Medusa simultaneously

**Tab 5 — Log:** All services unified, filter by service, search, color-coded severity

---

## Source Files

```
~/claude/configs/nexus-stack/          — configs, compose, dockerfiles
~/git/nexus-hydra/                     — Python command center (reference)
~/git/nexus-darknet/SPEC.md            — shell script contract (authoritative)
~/git/medusa-proxy/                    — Medusa proxy (datawookie)
~/git/nexus-medusa/                    — NeXuS Medusa integration
~/claude/docs/NEXUS_DARKNET_TUI_SPEC.md — Go TUI build spec
```

---

*NeXuS: Sane • Simple • Secure • Stealthy • Beautiful*
*The stack is the node. The node is sovereign.*
