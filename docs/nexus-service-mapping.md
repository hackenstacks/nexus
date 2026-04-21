# 🛰️ NeXuS Service Mapping & Interface Registry
*Version 1.1 — 2026-04-01 | Sane • Simple • Secure • Stealthy • Beautiful*

<!-- NEXUS-META
id:           NXS-REF-0001
type:         REFERENCE
principles:   Sane Simple Secure
cia:          Integrity Accountability
moe:          Observability
pop:          Open-Source
ymca:         CLI-First
zero-trust:   false
sovereignty:  local
transparency: full
defense:      passive
created:      2026-04-01
authors:      gemini
project:      nexus-hydra
layer:         infra
version:       1.1
updated:       2026-04-01
status:        active
audience:      all
tier:          all
score-sane:    4
score-simple:  4
score-secure:  4
score-composite: 12
score-tier:    ACTIVE
score-level:   2
scored-by:     gemini
scored-at:     2026-04-01
nexium-reward: 54
-->

---

## 🛡️ The Ghost Gate Perimeter
**Kernel-level Default Deny Enforcement**
*No packet leaves the machine without a signed permit. All violations are logged to the Witness Engine.*

| Port | Protocol | Service | Policy | Direction |
|:---:|:---:|:---|:---:|:---:|
| `9050` | TCP | 🧅 **Tor SOCKS Proxy** | **PERMIT** | In/Out |
| `9051` | TCP | 🛡️ **Tor Control Port** | **PERMIT** | Inbound |
| `4444` | TCP | 🌌 **I2P HTTP Proxy** | **PERMIT** | In/Out |
| `7070` | TCP | 🖥️ **I2P Web Console** | **PERMIT** | Inbound |
| `7656` | TCP | ⚡ **I2P SAM Bridge** | **PERMIT** | Outbound |
| `4885` | UDP | 🌐 **Yggdrasil Control** | **PERMIT** | Outbound |

---

## 🌑 Dark Stack Component Registry

### 🧅 Tor (The Onion Router)
*Primary anonymity layer for web and command-and-control.*
- **SOCKS5 Proxy (9050):** The universal egress for all Tor-enabled applications.
- **Control Port (9051):** Identity rotation and circuit management via the Orchestrator.
- **DNS Port (9053):** Transparent, leak-proof DNS resolution.

### 🌌 I2P (Invisible Internet Project)
*The backbone of the NeXuS Mesh and DIVA Chain.*
- **HTTP Proxy (4444):** Standard egress for I2P-native services and APIs.
- **SOCKS5 Proxy (4447):** High-compatibility egress for legacy protocols.
- **SAM Bridge (7656):** The high-speed interface for Reticulum, DIVA, and custom AI agents.
- **Web Console (7070):** Read-only status and tunnel management dashboard.

### 🌐 Yggdrasil & Reticulum
*The mesh and transport layers.*
- **Admin/Peering (9001):** Yggdrasil mesh management and peering.
- **Control (UDP 4885):** Yggdrasil mesh signaling.
- **SAM Port (7656):** Reticulum utilizes the I2P SAM Bridge for its encrypted transport.

---

## 🕹️ Primary Control Interfaces

| Interface | Type | Access Point | Purpose |
|:---|:---:|:---|:---|
| **Darknet TUI** | 🖥️ Go TUI | `cd ~/claude/nexus-darknet-go` | **The Main Deck.** Unified control for all 7 Hydra Heads. |
| **Orchestrator** | 🐍 Python | `cd ~/claude/nexus-orchestrator` | **The Brain.** Decision-making, registration, and permits. |
| **CharGen CLI** | 🎭 Go TUI | `cd ~/claude/char-gen-cli` | **The Soul.** AI character creation, vault, and chat. |
| **Hydra Dashboard**| 🕸️ React | `http://localhost:5173` | **The Eyes.** Visual health monitoring and real-time metrics. |
| **Backend API** | ⚡ Node.js | `http://localhost:8080` | **The Nerve.** System-wide REST integration and data flow. |

---

## 🤖 AI & Storage Layers

- **Ollama API (11434):** The local Chimera intelligence layer. No external keys required.
- **SeaweedFS (8333/18080):** Distributed, S3-compatible storage (Queued).
- **IPFS Gateway (8080):** Immutable content addressing and decentralized publishing.

---

## 🕵️ Sentinel Scripts (Health & Audit)

These scripts represent the active monitoring interface of the NeXuS node:

1.  **`big-catfish.sh`**: The OAAE Auditor. Verifies Human-AI interdependence and ethical alignment.
2.  **`ghost-gate-enforce.sh`**: The Witness Engine. Monitors the kernel perimeter and generates hash-linked evidence.
3.  **`screaming-demon.sh`**: The Hardware Sentinel. Monitors the Dell E6520 thermal envelope and namespace integrity.
4.  **`nexus-vault.sh`**: The Identity Guardian. Manages GPG-encrypted backups of all master and character keys.

---

> **NeXuS: Together Everyone Achieves More**
> *mewe — sovereign me inside a we*
