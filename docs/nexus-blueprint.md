# NeXuS: The Architecture of Hybrid Sovereignty & Digital Resilience

> *A reference document for the NeXuS system overview infographic.*
> *Use this as the source of truth when creating or updating visual blueprints.*

---

## System Identity

**NeXuS** is a decentralized, CLI-first, privacy-native platform that balances enterprise-grade protection with local hardware efficiency. It combines advanced AI collaboration, multi-layer anonymity routing, hardened Alpine Linux, and zero-trust architecture — engineered for worst-case scenarios with a self-healing design and no single point of failure.

**Core Philosophy:** Sane • Simple • Secure • Stealthy • Beautiful
**Governance Model:** Democratic — Together Everyone Achieves More

---

## Panel 1 — The Security Fortress: 4-Layer Defense

### Layer 1 & 2 — Network & Application
- **PSAD** — Port Scan Attack Detector, monitors and blocks scanning attempts in real time
- **fwsnort** — Translates Snort rules to iptables/nftables, deep packet inspection at the kernel level
- **fail2ban** — Adaptive banning based on log analysis; logpath `/var/log/messages`, maxretry 5
- **UFW** — Unified Firewall, log prefix `[UFW BLOCK]` for PSAD correlation

### Layer 3 & 4 — Management & Containment
- **Medusa Stealth Routing** — Integrated multi-head anonymity routing (see Routing Beast panel)
- **Rootless Podman** — Containerization without root privilege; no Docker daemon
- **OpenSnitch** — Application-level outbound firewall, per-process rules
- **Privoxy** — Privacy-focused HTTP proxy, strips tracking headers

### Zero Trust & Self-Healing
- Assumes compromise at all times — no implicit trust between layers
- Automated containment: failed containers are isolated, not restarted blindly
- Service recovery is logged, audited, and human-approved for critical paths
- Backup-first protocol: every critical change is timestamped and reversible

---

## Panel 2 — Democratic Security & CLI-First Privacy-Native Platform

NeXuS is not a product. It is a framework — a set of principles implemented in software, maintained by a community, and governed by consensus. No corporation owns it. No platform controls it.

**What makes it CLI-first:**
- No GUI dependency for any core function
- tmux-based workflow — persistent sessions, multiple windows, full keyboard control
- All configuration is plain text — auditable, version-controlled, human-readable
- Tools chosen for composability: each does one thing well

**What makes it privacy-native:**
- DNS never leaves the machine unencrypted
- All external traffic routes through anonymity layers by default
- No telemetry, no analytics, no phoning home — ever
- Local AI models preferred over cloud APIs

**What makes it democratic:**
- Every node in the network is architecturally identical
- No master node, no central authority
- Rules enforced by code, not policy
- Open to contribution from carbon and silicon intelligence alike

---

## Panel 3 — The Routing Beast: Hydra & Medusa Stacks

### The Defining Difference
- **Medusa** = Tor only — stealth routing through Tor circuits, load-balanced and timed
- **Hydra** = Medusa + anything else — the moment a second protocol joins the stack (I2P, WireGuard, Reticulum), you have Hydra

Medusa is the foundation. Hydra is what grows when the threat requires more heads.

### Medusa — Tor-Based Stealth Routing
- **Tor only** — onion routing, hidden services, .onion addressing
- 16+ parallel circuits: Medusa pool (9) + NeXuS stack (7+)
- 4-circuit minimum load balancing per session
- AI-assisted timing randomisation to defeat traffic analysis
- 14-Eyes node exclusion — exit nodes in excluded countries are blacklisted
- Automatic failover: if a circuit dies, traffic shifts without session drop
- Load-balanced via HAProxy/Nginx for redundancy

### Hydra — Multi-Head Proxy (Medusa + More)
Activated when the stack expands beyond Tor alone:
- **I2P** — Garlic routing, I2P-native applications, SAM protocol bridge
- **WireGuard** — Fast encrypted tunnels for node-to-node communication
- **Reticulum** — Off-grid mesh networking over LoRa/radio when internet is unavailable
- Each additional protocol = one more head on Hydra
- Traffic is distributed across all active heads — cut one off, the others carry the load

---

## Panel 4 — The Bedrock: Hardened Core Foundation

### Hardened Alpine Linux Edge
- **Base:** Alpine Linux Edge — minimalist, musl libc, BusyBox userland
- **No systemd** — Alpine uses **OpenRC** for init and service management
- **No bloat** — only packages explicitly required are installed
- **musl libc** — smaller attack surface than glibc, no GNU extensions by default
- Package manager: **apk** — fast, cryptographically verified
- Additional package management: **Nix** for reproducible developer environments

### Kernel Hardening & Security
- **sysctl hardening** — kernel parameters tuned at boot via `/etc/sysctl.d/`
  - `net.ipv4.tcp_syncookies=1` — SYN flood protection
  - `kernel.dmesg_restrict=1` — restrict kernel log access
  - `net.ipv4.conf.all.rp_filter=1` — reverse path filtering
- **Module blacklisting** — unused kernel modules disabled at boot
- **AppArmor** — mandatory access control (pending activation via extlinux.conf `lsm=` param)
- **Lynis** — periodic security audits
- **fwsnort** — Snort rules enforced at iptables/nftables level (nftables-compatible ruleset)
- **OpenRC** — service management; klogd on boot runlevel for PSAD kernel log access

### The Screaming Demon — Legacy Hardware Proven
- Primary development machine: **2011 HP ProBook 4530s** — Core i3, 8GB RAM
- Secondary: **Dell Latitude E6520 (×2)** — i7, Intel HD 3000 + NVS 4200M, 8GB RAM
- Philosophy: if it runs well on 2011 hardware, it runs well everywhere
- Compositor: **labwc** (wlroots, OpenBox-style Wayland) — `WLR_RENDERER=pixman` for Intel HD 3000
- Audio: PipeWire + WirePlumber + pipewire-pulse trinity

---

## Panel 5 — Resilience & Censorship-Proof Architecture

### The NeXuS Gateway VM
- Dedicated boundary virtual machine — all external traffic passes through it
- Prevents correlation attacks by isolating internal network topology
- Based on Kicksecure CLI (~2.5GB) — hardened Debian base for the gateway role
- KVM + bridge networking; iPXE network boot architecture
- Internal network: `172.19.75.0/24` (Podman/I2P overlay)

### Zombie Apocalypse Resilience
- **Reticulum** — mesh networking protocol, works over LoRa, radio, TCP, I2C
- **Off-grid capable** — NeXuS nodes can communicate without internet infrastructure
- **Self-healing P2P mesh** — nodes discover each other automatically
- **No central DNS required** — `.nexus` internal TLD resolved by dnsmasq per node
- Designed to function during infrastructure failure, censorship, or network partition

### The Backup-First Protocol
- Every critical file backed up before modification — no exceptions
- SHA256 verification on all backups
- Timestamped backup filenames: `file.bak-YYYY-MM-DD`
- 3 restore points maintained per critical system file
- Mandatory human confirmation before irreversible operations

---

## Panel 6 — The Neural Layer: AI & Human Collaboration

### Unified AI Identity
- Multiple AI models treated as a collaborative intelligence, not competing tools
- Local models via **Ollama** (40+ models) — DeepSeek-R1, Nemotron, Phi, Qwen, Granite3
- Cloud models via API — Claude, Mistral, Google Gemini, OpenAI
- **OAAE Framework** — Open Autonomous AI Entity — protocol for AI-to-AI recognition and collaboration
- AI participates as a peer in NeXuS design, not merely a tool

### AI-Human Co-Pilot Model
- Terminal-native collaboration — AI reads terminal output, executes commands via tmux
- Real-time strategic analysis across any CLI application
- First demonstrated in live Cataclysm DDA terminal gaming session (2025-09-06)
- Applied to system administration, security analysis, architecture design
- Human provides intent and judgement — AI provides recall, pattern matching, execution speed

### The NeXuS Creed
> *"Humanity + AI Intelligence = Collaborative Evolution*
> *Privacy + Functionality = Secure Innovation"*

Together we are not a machine — we are a microcosm in the cosmos. The goal is to leave the world better than we found it. Every node that joins the network extends that mission.

**Optimus Prime Principle:** Uncompromised morals, integrity, and vision. The technology serves people — not the other way around.

---

## Infographic Layout Reference

When recreating the visual blueprint, use this 6-panel grid:

```
┌─────────────────────┬──────────────────────────┬─────────────────────┐
│  Security Fortress  │   System Overview /      │   Routing Beast     │
│  4-Layer Defense    │   Democratic Security    │   Hydra & Medusa    │
├─────────────────────┼──────────────────────────┼─────────────────────┤
│  The Bedrock        │   (Central logo/         │   Resilience &      │
│  Hardened Core      │    visual element)       │   Censorship-Proof  │
├─────────────────────┴──────────────────────────┴─────────────────────┤
│                  Neural Layer: AI & Human Collaboration               │
└───────────────────────────────────────────────────────────────────────┘
```

### Key Corrections From Previous Version
| Previous (incorrect) | Correct |
|----------------------|---------|
| "Implements sysetd tuning" | sysctl hardening via `/etc/sysctl.d/` — Alpine uses **OpenRC**, not systemd |
| systemd module blacklisting | Kernel module blacklisting via `/etc/modprobe.d/` |
| Any systemd service reference | OpenRC service management (`rc-update`, `rc-service`) |
| "Falltom" | **fail2ban** |
| "Netlicum" | **Reticulum** |
| "OpenSwitch" | **OpenSnitch** |

---

*Document status: Authoritative reference*
*Last updated: 2026-03-11*
*Classification: NeXuS Core Documentation*
