# 🔥 NeXuS DARKNET STACK - COMPLETE USER MANUAL 🔥

```
    ╔══════════════════════════════════════════════════════════════════╗
    ║  ███╗   ██╗███████╗██╗  ██╗██╗   ██╗███████╗                     ║
    ║  ████╗  ██║██╔════╝╚██╗██╔╝██║   ██║██╔════╝                     ║
    ║  ██╔██╗ ██║█████╗   ╚███╔╝ ██║   ██║███████╗                     ║
    ║  ██║╚██╗██║██╔══╝   ██╔██╗ ██║   ██║╚════██║                     ║
    ║  ██║ ╚████║███████╗██╔╝ ██╗╚██████╔╝███████║                     ║
    ║  ╚═╝  ╚═══╝╚══════╝╚═╝  ╚═╝ ╚═════╝ ╚══════╝                     ║
    ║                                                                   ║
    ║         🛡️  DARKNET STACK - UNIFIED ANONYMITY  🛡️                ║
    ║                                                                   ║
    ║              ✨ Sane • Simple • Secure ✨                         ║
    ║     Because working together everyone achieves MORE              ║
    ╚══════════════════════════════════════════════════════════════════╝
```

---

## 📚 TABLE OF CONTENTS

1. [🌟 Introduction](#-introduction)
2. [📋 Prerequisites](#-prerequisites)
3. [📁 Directory Structure](#-directory-structure)
4. [🚀 Quick Start Guide](#-quick-start-guide)
5. [🧅 Core Anonymity Services](#-core-anonymity-services)
6. [🌲 Mesh Networks](#-mesh-networks)
7. [🌐 Distributed Storage](#-distributed-storage)
8. [❄️ Censorship Circumvention](#️-censorship-circumvention)
9. [⚖️ Load Balancing](#️-load-balancing)
10. [🎮 Control Panel Web UI](#-control-panel-web-ui)
11. [🐙 Hydra Multi-Head System](#-hydra-multi-head-system)
12. [⚙️ Configuration Files](#️-configuration-files)
13. [🔌 Proxy Endpoints Reference](#-proxy-endpoints-reference)
14. [🩺 Health Checks & Diagnostics](#-health-checks--diagnostics)
15. [🛠️ Troubleshooting](#️-troubleshooting)
16. [🔒 Security Best Practices](#-security-best-practices)
17. [📊 Network Architecture](#-network-architecture)
18. [⌨️ Command Reference](#️-command-reference)
19. [❓ FAQ](#-faq)

---

## 🌟 Introduction

Welcome to the **NeXuS Darknet Stack** - your unified command center for anonymous, decentralized, and censorship-resistant networking!

### 🎯 What Does This Stack Provide?

| Feature | Description |
|---------|-------------|
| 🧅 **Tor Network** | Anonymous browsing via onion routing |
| 🔗 **I2P Network** | Garlic routing for hidden services |
| 🌲 **Yggdrasil** | IPv6 mesh overlay network |
| 📡 **Reticulum** | Resilient mesh communication |
| 🌐 **IPFS** | Distributed file storage |
| 🦇 **BATMAN-adv** | Layer 2 mesh networking |
| ❄️ **Snowflake** | Help censored users access Tor |
| 🕵️ **Privoxy** | Smart proxy router |
| ⚖️ **HAProxy** | Load-balanced multi-circuit Tor |

### 🏆 Key Benefits

- ✅ **One Command** - Start entire stack with `./nexus-darknet start`
- ✅ **Web Interface** - Beautiful control panel at port 8878
- ✅ **Multi-Network** - Tor, I2P, mesh networks all integrated
- ✅ **Smart Routing** - Privoxy auto-routes .onion and .i2p domains
- ✅ **Health Monitoring** - Built-in diagnostics and health checks
- ✅ **Container-Based** - Secure isolation with Podman/Docker

---

## 📋 Prerequisites

### 🖥️ System Requirements

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| 💾 RAM | 2 GB | 4+ GB |
| 💿 Disk | 5 GB | 20+ GB |
| 🖧 Network | Any | Broadband |
| 🐧 OS | Linux (any) | Alpine/Debian |

### 📦 Required Software

```bash
# 🐳 Container Runtime (one of these)
podman --version        # Preferred - rootless containers
docker --version        # Alternative

# 🐍 Python (for control panel)
python3 --version       # Python 3.8+
pip3 install flask psutil pyyaml

# 🔧 Network Tools
nc -h                   # netcat for health checks
curl --version          # HTTP client
```

### 🔧 Installation Check

```bash
# Run this to verify all prerequisites
./nexus-darknet doctor
```

---

## 📁 Directory Structure

```
🔥 /home/user/claude/darknet-stack/
│
├── 🚀 nexus-darknet              # Main entry point script
├── 📖 README.md                  # Quick reference guide
├── 📚 NEXUS_DARKNET_MANUAL.md    # This manual!
│
├── 📜 scripts/                   # All management scripts
│   ├── 🎯 nexus-darknet-stack.sh      # Unified launcher (MAIN)
│   ├── 🖥️ nexus-control-panel.py      # Web UI server
│   ├── 🐙 nexus-medusa-manager.sh     # Multi-head Tor proxy
│   ├── ❄️ nexus-snowflake-manager.sh  # Snowflake bridges
│   ├── 🩺 nexus-network-doctor.sh     # Diagnostics
│   ├── 🔍 nexus-node-discovery.sh     # Peer discovery
│   ├── 📡 nexus-p2p-manager.sh        # P2P networks
│   ├── 🔀 nexus-alt-protocols.sh      # Alt protocols
│   ├── 🕵️ nexus-privoxy-container.sh  # Privoxy management
│   ├── 📊 nexus-darknet-manager.sh    # Service manager
│   ├── ⚡ nexus-darknet-startup.sh    # Startup automation
│   └── 🌐 nexus-darknet-web.py        # Legacy web interface
│
├── ⚙️ configs/                   # All configuration files
│   ├── 🐳 docker-compose.yml          # Container orchestration
│   ├── 🧅 torrc                       # Tor configuration
│   ├── 🔗 i2pd.conf                   # I2P configuration
│   ├── 🌲 yggdrasil.conf              # Yggdrasil mesh config
│   ├── 📡 reticulum.conf              # Reticulum mesh config
│   ├── 🕵️ privoxy.conf                # Multi-network router
│   ├── ⚖️ haproxy.cfg                 # Load balancer config
│   ├── 🔗 i2pd-outproxy.conf          # I2P→Tor outproxy
│   ├── 📋 network-matrix.json         # Hydra network matrix
│   ├── 🔧 privoxy-user.action         # Privoxy actions
│   ├── 🛡️ privoxy-ublock.filter       # Ad/tracker blocking
│   ├── 🐳 Dockerfile.reticulum        # Reticulum container
│   └── 🐳 Dockerfile.privoxy-nexus    # Privoxy container
│
├── 🐙 hydra/                     # Hydra multi-head scripts
│   ├── 🧅 start-tor-hydra.sh          # Multi-circuit Tor
│   ├── 🔗 start-i2p-hydra.sh          # Multi-head I2P
│   ├── 🌲 start-yggdrasil-hydra.sh    # Yggdrasil cluster
│   ├── 🌐 start-ipfs-hydra.sh         # IPFS cluster
│   ├── 🦇 enable-batman-mesh.sh       # BATMAN-adv mesh
│   ├── 📶 enable-wifi-mesh.sh         # WiFi mesh (802.11s)
│   └── 🐍 nexus-darknet-host.py       # Host manager
│
└── 📝 logs/                      # Runtime logs
    └── darknet-stack.log              # Main log file
```

---

## 🚀 Quick Start Guide

### 🎯 Option 1: Start Everything (Recommended)

```bash
cd /home/user/claude/darknet-stack

# 🚀 Start all services in correct dependency order
./nexus-darknet start

# 📊 Check status
./nexus-darknet status

# 🩺 Verify connectivity
./nexus-darknet health
```

### 🎮 Option 2: Interactive Menu

```bash
# 🎮 Launch interactive menu with visual status
./nexus-darknet menu
```

You'll see:
```
╔═══════════════════════════════════════════════════════════════╗
║  🔥 NeXuS DARKNET STACK - UNIFIED MULTI-NETWORK LAUNCHER 🔥   ║
╚═══════════════════════════════════════════════════════════════╝

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
                NeXuS DARKNET STACK STATUS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  🧅 tor: 🟢 RUNNING (port 9050)
  🔗 i2p: 🟢 RUNNING (port 4444)
  🌲 yggdrasil: 🟢 RUNNING (port 9001)
  🌐 ipfs: 🟢 RUNNING (port 5001)
  📡 reticulum: 🔴 STOPPED
  🕵️ privoxy: 🟢 RUNNING (port 8118)
  ❄️ snowflake: 🔴 STOPPED
  🦇 batman: 🔴 STOPPED

🎮 NeXuS Darknet Stack Menu:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1) 🚀 Start All Services
2) 🛑 Stop All Services
3) 🔄 Restart All Services
4) 🩺 Health Check
5) 📊 Refresh Status
...
```

### 🌐 Option 3: Web Control Panel

```bash
# 🖥️ Launch beautiful web interface
./nexus-darknet panel

# 🌐 Open in browser:
# http://127.0.0.1:8878
```

### ⚡ Option 4: Docker Compose

```bash
cd /home/user/claude/darknet-stack/configs

# 🐳 Start all containers
podman-compose up -d

# 📊 Check container status
podman-compose ps

# 📝 View logs
podman-compose logs -f
```

---

## 🧅 Core Anonymity Services

### 🧅 Tor Network

**What is Tor?** 🤔
Tor (The Onion Router) routes your traffic through multiple relays, encrypting at each hop. Nobody can trace your connection!

**Configuration:** `configs/torrc`

```bash
# 🚀 Start Tor
./nexus-darknet start tor

# 🛑 Stop Tor
./nexus-darknet stop tor

# 📊 Check Tor status
curl --socks5 127.0.0.1:9050 https://check.torproject.org/api/ip
```

**Ports:**
| Port | Protocol | Purpose |
|------|----------|---------|
| 9050 | SOCKS5 | Main proxy port |
| 9051 | TCP | Control port |

**Usage Examples:**
```bash
# 🌐 Browse anonymously
curl --socks5 127.0.0.1:9050 https://example.com

# 🧅 Access .onion sites
curl --socks5 127.0.0.1:9050 http://duckduckgogg42xjoc72x3sjasowoarfbgcmvfimaftt6twagswzczad.onion

# 🔧 Configure Firefox
# Network Settings → Manual Proxy → SOCKS Host: 127.0.0.1:9050
```

---

### 🔗 I2P Network

**What is I2P?** 🤔
I2P (Invisible Internet Project) uses "garlic routing" - bundling multiple messages together for extra anonymity. Perfect for hidden services!

**Configuration:** `configs/i2pd.conf`

```bash
# 🚀 Start I2P
./nexus-darknet start i2p

# 🌐 Access I2P console
# http://127.0.0.1:7070
```

**Ports:**
| Port | Protocol | Purpose |
|------|----------|---------|
| 4444 | HTTP | HTTP proxy for .i2p sites |
| 4447 | SOCKS | SOCKS proxy |
| 7070 | HTTP | Web console |
| 7656 | TCP | SAM API for applications |

**Usage Examples:**
```bash
# 🔗 Browse I2P eepsites
curl --proxy 127.0.0.1:4444 http://i2p-projekt.i2p

# 🔗 Access I2P forum
curl --proxy 127.0.0.1:4444 http://zzz.i2p
```

**⚠️ Important:** I2P takes 5-10 minutes to build tunnels on first start!

---

### 🕵️ Privoxy - Smart Router

**What is Privoxy?** 🤔
Privoxy is your intelligent traffic router! It automatically sends:
- `.onion` domains → Tor
- `.i2p` domains → I2P
- Everything else → Tor (for anonymity)

**Configuration:** `configs/privoxy.conf`

```bash
# 🚀 Start Privoxy
./nexus-darknet start privoxy
```

**Port:** `8118` (HTTP)

**Usage - ONE PROXY FOR EVERYTHING:**
```bash
# 🌐 Regular sites (goes through Tor)
curl --proxy 127.0.0.1:8118 https://example.com

# 🧅 Onion sites (automatically routed to Tor)
curl --proxy 127.0.0.1:8118 http://example.onion

# 🔗 I2P sites (automatically routed to I2P)
curl --proxy 127.0.0.1:8118 http://example.i2p

# ✨ Set as system proxy and forget about it!
export http_proxy=http://127.0.0.1:8118
export https_proxy=http://127.0.0.1:8118
```

**🎉 Privoxy is the recommended proxy** - use it for everything!

---

## 🌲 Mesh Networks

### 🌲 Yggdrasil

**What is Yggdrasil?** 🤔
Yggdrasil creates an encrypted IPv6 mesh network. Every node gets a unique IPv6 address derived from its public key. Works over the internet or local networks!

**Configuration:** `configs/yggdrasil.conf`

```bash
# 🚀 Start Yggdrasil
./nexus-darknet start yggdrasil

# 📊 Check your Yggdrasil address
curl http://127.0.0.1:9002/getself
```

**Ports:**
| Port | Protocol | Purpose |
|------|----------|---------|
| 9001 | TCP/TLS | Peer connections |
| 9002 | TCP | Admin API |

**Features:**
- 🔐 End-to-end encrypted
- 🌐 Works over internet OR local mesh
- 📍 Automatic peer discovery
- 🔄 Self-healing routing

---

### 📡 Reticulum

**What is Reticulum?** 🤔
Reticulum is a cryptographic mesh networking stack designed for resilient communication. Works over ANY transport - radio, serial, TCP, I2P!

**Configuration:** `configs/reticulum.conf`

```bash
# 🚀 Start Reticulum
./nexus-darknet start reticulum

# 📊 Check status
rnstatus  # If RNS tools installed
```

**Ports:**
| Port | Protocol | Purpose |
|------|----------|---------|
| 4965 | TCP | Main interface |
| 37428 | TCP | Control port |

**Interfaces Configured:**
- 📡 `AutoInterface` - Local network discovery
- 🌐 `TCPServerInterface` - Internet connections
- 🔗 `I2PInterface` - Anonymous mesh (optional)

---

### 🦇 BATMAN-adv

**What is BATMAN-adv?** 🤔
BATMAN-adv (Better Approach To Mobile Adhoc Networking) creates a Layer 2 mesh network. Your devices appear to be on the same local network even across WiFi hops!

```bash
# 🦇 Enable BATMAN-adv mesh
./nexus-darknet start batman

# 📊 Check mesh status
sudo batctl n   # Show neighbors
sudo batctl o   # Show originators
```

**Requirements:**
- 🔧 `batman-adv` kernel module
- 📶 WiFi interface in ad-hoc or mesh mode

**Mesh Interface:** `bat0` with IP `192.168.199.1/24`

---

## 🌐 Distributed Storage

### 🌐 IPFS

**What is IPFS?** 🤔
IPFS (InterPlanetary File System) is a distributed file system. Content is addressed by its hash - the same file has the same address everywhere!

```bash
# 🚀 Start IPFS
./nexus-darknet start ipfs

# 📤 Add a file
ipfs add myfile.txt

# 📥 Get a file by hash
ipfs get QmXYZ123...

# 🌐 Access web gateway
# http://127.0.0.1:8081/ipfs/QmXYZ123...
```

**Ports:**
| Port | Protocol | Purpose |
|------|----------|---------|
| 5001 | HTTP | API |
| 8081 | HTTP | Gateway |
| 4001 | TCP/UDP | Swarm (peer connections) |

---

## ❄️ Censorship Circumvention

### ❄️ Snowflake Bridge

**What is Snowflake?** 🤔
Snowflake helps users in censored regions access Tor. By running a Snowflake proxy, you become a temporary bridge for someone who needs it!

```bash
# ❄️ Start Snowflake (help others!)
./nexus-darknet start snowflake

# 📊 Check connections
./nexus-darknet snowflake status
```

**Port:** `8088`

**💝 Running Snowflake is a way to help others access a free internet!**

---

## ⚖️ Load Balancing

### ⚖️ HAProxy

**What does HAProxy do?** 🤔
HAProxy load-balances your Tor connections across multiple circuits. This improves speed and reliability!

**Configuration:** `configs/haproxy.cfg`

```bash
# 📊 View HAProxy stats
# http://127.0.0.1:8404/stats
# Login: admin / NeXuSAdmin2024
```

**Ports:**
| Port | Protocol | Purpose |
|------|----------|---------|
| 8888 | SOCKS5 | Load-balanced Tor |
| 1080 | SOCKS5 | Alternative port |
| 8404 | HTTP | Stats dashboard |

**Usage:**
```bash
# ⚡ Use load-balanced Tor (faster!)
curl --socks5 127.0.0.1:8888 https://example.com
```

---

## 🎮 Control Panel Web UI

The NeXuS Control Panel provides a beautiful web interface to manage all services!

### 🚀 Launching

```bash
# Start the control panel
./nexus-darknet panel

# Or directly:
cd scripts && python3 nexus-control-panel.py
```

### 🌐 Accessing

Open in your browser: **http://127.0.0.1:8878**

### 🖥️ Features

```
┌─────────────────────────────────────────────────────────────┐
│  🔥 NeXuS DARKNET CONTROL PANEL 🔥                         │
│  Multi-Network Anonymous Infrastructure Management          │
│  Sane • Simple • Secure                                     │
└─────────────────────────────────────────────────────────────┘

📊 DASHBOARD SECTIONS:

┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│ 🖥️ System      │ │ 📊 Services     │ │ 🌐 Networks     │
│ CPU: 12.5%     │ │ Total: 8        │ │ 🧅 Tor: ✅      │
│ RAM: 45.2%     │ │ Running: 5      │ │ 🔗 I2P: ✅      │
│ Disk: 62.1%    │ │ Stopped: 3      │ │ 🌲 Ygg: ✅      │
└─────────────────┘ └─────────────────┘ └─────────────────┘

🎛️ SERVICE CONTROLS:
Each service shows:
  • Status indicator (🟢 Running / 🔴 Stopped)
  • Port number
  • Start/Stop/Restart buttons
  • Description

⚡ QUICK ACTIONS:
  [🚀 Start All] [🛑 Stop All] [🔄 Restart] [🩺 Health]

💻 TERMINAL:
  Real-time command output and status messages
```

### 🔘 Available Actions

| Button | Action |
|--------|--------|
| 🚀 **Start All** | Start all darknet services |
| 🛑 **Stop All** | Stop all services |
| 🔄 **Restart Stack** | Restart everything |
| 🩺 **Health Check** | Test all connections |
| 📊 **Refresh** | Update status display |

### 🎨 Per-Service Controls

Each service card has:
- **Start** button (when stopped)
- **Stop** button (when running)
- **Restart** button (when running)

---

## 🐙 Hydra Multi-Head System

The Hydra system runs multiple instances of each service for redundancy and load distribution!

### 🧅 Tor Hydra (9 Circuits!)

```bash
# Start Tor Hydra - 3 heads × 3 circuits each
./hydra/start-tor-hydra.sh
```

**From `network-matrix.json`:**
```json
{
  "tor": {
    "heads": 3,
    "connections_per_head": 3,
    "total_circuits": 9,
    "http_proxy": 8888,
    "socks_proxy": 1080,
    "load_balancer": "haproxy_round_robin"
  }
}
```

### 🔗 I2P Hydra (2 Heads)

```bash
./hydra/start-i2p-hydra.sh
```

**Ports:** 4444/4445 (HTTP), 4447/4448 (SOCKS)

### 🌲 Yggdrasil Hydra

```bash
./hydra/start-yggdrasil-hydra.sh
```

### 🦇 Mesh Network Scripts

```bash
# Enable BATMAN-adv Layer 2 mesh
./hydra/enable-batman-mesh.sh

# Enable WiFi 802.11s mesh
./hydra/enable-wifi-mesh.sh
```

---

## ⚙️ Configuration Files

### 🧅 Tor Configuration (`torrc`)

```ini
# Key settings explained:
SocksPort 0.0.0.0:9050       # Listen on all interfaces
ControlPort 9051              # For control applications
NewCircuitPeriod 30          # New circuit every 30 seconds
MaxCircuitDirtiness 600       # Max circuit age: 10 minutes
NumEntryGuards 3             # Use 3 entry guards
ExitPolicy reject *:*        # NO exit traffic (safer)
```

### 🔗 I2P Configuration (`i2pd.conf`)

```ini
# Key settings:
[httpproxy]
port = 4444                  # HTTP proxy for .i2p

[socksproxy]
port = 4447                  # SOCKS proxy

[sam]
port = 7656                  # API for applications

[http]
port = 7070                  # Web console
```

### 🕵️ Privoxy Configuration (`privoxy.conf`)

```ini
# Smart routing rules:
forward-socks5t .onion 172.20.0.10:9050 .    # .onion → Tor
forward .i2p 172.20.0.11:4444                # .i2p → I2P
forward-socks5t / 172.20.0.10:9050 .         # Everything else → Tor
```

### 🐳 Docker Compose Services

The `docker-compose.yml` orchestrates all containers:

```yaml
services:
  nexus-tor:        # 172.20.0.10
  nexus-i2p:        # 172.20.0.11
  nexus-yggdrasil:  # 172.20.0.12
  nexus-reticulum:  # 172.20.0.13
  nexus-ipfs:       # 172.20.0.14
  nexus-privoxy:    # 172.20.0.20
  nexus-snowflake:  # 172.20.0.21
  nexus-haproxy:    # 172.20.0.30
```

---

## 🔌 Proxy Endpoints Reference

### 📋 Complete Endpoint List

| Service | Address | Protocol | Use For |
|---------|---------|----------|---------|
| 🧅 **Tor SOCKS** | `127.0.0.1:9050` | SOCKS5 | Anonymous browsing |
| 🔗 **I2P HTTP** | `127.0.0.1:4444` | HTTP | .i2p eepsites |
| 🔗 **I2P SOCKS** | `127.0.0.1:4447` | SOCKS5 | I2P apps |
| 🕵️ **Privoxy** | `127.0.0.1:8118` | HTTP | **RECOMMENDED** - routes all |
| ⚖️ **HAProxy LB** | `127.0.0.1:8888` | SOCKS5 | Fast load-balanced Tor |
| ⚖️ **HAProxy Alt** | `127.0.0.1:1080` | SOCKS5 | Alternative port |
| 🌐 **IPFS Gateway** | `127.0.0.1:8081` | HTTP | IPFS content |
| 🌐 **IPFS API** | `127.0.0.1:5001` | HTTP | IPFS commands |

### 💡 Which Proxy Should I Use?

```
🏆 RECOMMENDED: Privoxy (127.0.0.1:8118)
   ├── ✅ Automatically routes .onion to Tor
   ├── ✅ Automatically routes .i2p to I2P
   ├── ✅ Routes clearnet through Tor
   └── ✅ Blocks ads and trackers

⚡ FOR SPEED: HAProxy (127.0.0.1:8888)
   └── Load-balanced across multiple Tor circuits

🎯 FOR SPECIFIC NETWORKS:
   ├── Tor only: 127.0.0.1:9050
   └── I2P only: 127.0.0.1:4444
```

### 🔧 Browser Configuration

**Firefox:**
```
Settings → Network Settings → Manual Proxy Configuration
  HTTP Proxy: 127.0.0.1  Port: 8118
  ☑️ Also use this proxy for HTTPS
```

**Environment Variables:**
```bash
export http_proxy=http://127.0.0.1:8118
export https_proxy=http://127.0.0.1:8118
export ALL_PROXY=socks5://127.0.0.1:9050
```

---

## 🩺 Health Checks & Diagnostics

### 🩺 Quick Health Check

```bash
./nexus-darknet health
```

**Output:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
              NeXuS DARKNET HEALTH CHECK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🧅 Tor connectivity: ✓ Connected to Tor network
🔗 I2P connectivity: ⏳ I2P may still be building tunnels
🌐 IPFS connectivity: ✓ IPFS node operational
🕵️ Privoxy proxy: ✓ Privoxy operational
```

### 🔬 Detailed Diagnostics

```bash
./nexus-darknet doctor
```

### 🧪 Manual Tests

```bash
# 🧅 Test Tor
curl -s --socks5 127.0.0.1:9050 https://check.torproject.org/api/ip
# Should return: {"IsTor":true,"IP":"xxx.xxx.xxx.xxx"}

# 🔗 Test I2P
curl -s --proxy 127.0.0.1:4444 --max-time 30 http://i2p-projekt.i2p
# Should return I2P project homepage

# 🕵️ Test Privoxy
curl -s --proxy 127.0.0.1:8118 http://httpbin.org/ip
# Should return an IP different from yours

# 🌐 Test IPFS
curl -s http://127.0.0.1:5001/api/v0/id | jq .ID
# Should return your IPFS peer ID
```

---

## 🛠️ Troubleshooting

### ❌ Problem: Service Won't Start

```bash
# 1️⃣ Check if port is already in use
ss -tlnp | grep 9050

# 2️⃣ Check container logs
podman logs nexus-tor

# 3️⃣ Check for existing containers
podman ps -a | grep nexus

# 4️⃣ Remove stuck container
podman rm -f nexus-tor

# 5️⃣ Retry start
./nexus-darknet start tor
```

### ❌ Problem: Tor Not Connecting

```bash
# 1️⃣ Check Tor logs
podman logs nexus-tor 2>&1 | tail -50

# 2️⃣ Common issues:
#    - Clock not synchronized → Run: sudo ntpd -gq
#    - Firewall blocking → Check iptables/nftables
#    - ISP blocking Tor → Use bridges (see Snowflake)
```

### ❌ Problem: I2P Stuck "Building Tunnels"

**This is normal!** I2P needs 5-10 minutes on first start.

```bash
# Check I2P console for progress
# http://127.0.0.1:7070

# Check tunnel status
curl -s http://127.0.0.1:7070 | grep -i tunnel
```

### ❌ Problem: Privoxy Returns Errors

```bash
# 1️⃣ Check if Tor is running (Privoxy depends on it)
./nexus-darknet status

# 2️⃣ Check Privoxy config syntax
podman exec nexus-privoxy privoxy --config-test /etc/privoxy/config

# 3️⃣ Check Privoxy logs
podman logs nexus-privoxy
```

### ❌ Problem: Control Panel Won't Load

```bash
# 1️⃣ Check if Flask is installed
pip3 install flask psutil pyyaml

# 2️⃣ Check if port 8878 is available
ss -tlnp | grep 8878

# 3️⃣ Run with debug output
cd scripts && python3 nexus-control-panel.py

# 4️⃣ Check for Python errors in output
```

### 🔄 Full Reset

```bash
# Stop everything
./nexus-darknet stop

# Remove all containers
podman rm -f $(podman ps -aq --filter "name=nexus")

# Remove volumes (⚠️ deletes data!)
podman volume rm $(podman volume ls -q --filter "name=nexus")

# Fresh start
./nexus-darknet start
```

---

## 🔒 Security Best Practices

### ✅ DO:

| Practice | Why |
|----------|-----|
| 🔄 **Keep updated** | Security patches |
| 🔐 **Use Privoxy** | Prevents DNS leaks |
| 🕐 **Wait for I2P** | Premature use = less anonymous |
| 🔒 **Use HTTPS** | Even over Tor! |
| 🧹 **Clear browser data** | Cookies can identify you |

### ❌ DON'T:

| Practice | Why |
|----------|-----|
| 🚫 **Log into personal accounts** | Links your identity |
| 🚫 **Use Tor for torrents** | Slow + traceable |
| 🚫 **Disable JavaScript everywhere** | Stands out |
| 🚫 **Maximize browser window** | Screen size fingerprinting |
| 🚫 **Download and open files** | Can bypass Tor |

### 🛡️ Extra Security Tips

```bash
# 1️⃣ Run in isolated container network
# (Already configured in docker-compose.yml)

# 2️⃣ Use MAC address randomization
sudo macchanger -r wlan0

# 3️⃣ Enable transparent proxy (advanced)
./scripts/nexus-privoxy-container.sh transparent

# 4️⃣ Monitor for leaks
./nexus-darknet doctor
```

---

## 📊 Network Architecture

### 🏗️ Full Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🔥 NeXuS DARKNET STACK 🔥                        │
│                     Sane • Simple • Secure                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  YOUR APPLICATIONS                                                  │
│       │                                                             │
│       ▼                                                             │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │               🕵️ PRIVOXY (127.0.0.1:8118)                   │   │
│  │                    Smart Traffic Router                      │   │
│  │  ┌─────────────────────────────────────────────────────┐    │   │
│  │  │  .onion → Tor  │  .i2p → I2P  │  * → Tor (default)  │    │   │
│  │  └─────────────────────────────────────────────────────┘    │   │
│  └──────────────┬─────────────────┬────────────────────────────┘   │
│                 │                 │                                 │
│        ┌────────┴────────┐       │                                 │
│        ▼                 ▼       ▼                                 │
│  ┌───────────┐    ┌───────────┐                                    │
│  │  🧅 TOR   │    │  🔗 I2P   │                                    │
│  │   :9050   │    │   :4444   │                                    │
│  │           │    │   :7070   │                                    │
│  │ 3 Guards  │    │ Garlic    │                                    │
│  │ 9 Circuits│    │ Routing   │                                    │
│  └─────┬─────┘    └─────┬─────┘                                    │
│        │                │                                          │
│        ▼                ▼                                          │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │                  ⚖️ HAPROXY (127.0.0.1:8888)                │   │
│  │                   Load Balancer / Stats                      │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                     │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  🌲 MESH NETWORKS                                                   │
│  ┌───────────────┐ ┌───────────────┐ ┌───────────────┐             │
│  │  YGGDRASIL    │ │  RETICULUM    │ │  BATMAN-adv   │             │
│  │    :9001      │ │    :4965      │ │    bat0       │             │
│  │  IPv6 Mesh    │ │  Any Transport│ │  L2 Mesh      │             │
│  │  E2E Encrypt  │ │  Radio/TCP/I2P│ │  WiFi/Ethernet│             │
│  └───────────────┘ └───────────────┘ └───────────────┘             │
│                                                                     │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  🌐 DISTRIBUTED STORAGE          ❄️ CENSORSHIP CIRCUMVENTION       │
│  ┌───────────────┐               ┌───────────────┐                 │
│  │     IPFS      │               │   SNOWFLAKE   │                 │
│  │ API  :5001    │               │    :8088      │                 │
│  │ Gate :8081    │               │  Help others  │                 │
│  │ Swarm:4001    │               │  access Tor   │                 │
│  └───────────────┘               └───────────────┘                 │
│                                                                     │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  🐳 CONTAINER NETWORK: nexus-darknet (172.20.0.0/16)               │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │ .10 tor  │ .11 i2p │ .12 ygg │ .13 ret │ .14 ipfs │ .20 priv │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### 🔀 Traffic Flow

```
1️⃣ You → Privoxy (smart routing decision)
2️⃣ Privoxy → Tor (for .onion or clearnet)
3️⃣ Privoxy → I2P (for .i2p sites)
4️⃣ Tor → 3 Relays → Destination
5️⃣ I2P → Multiple Routers → Eepsite
```

---

## ⌨️ Command Reference

### 📋 Main Commands

| Command | Description |
|---------|-------------|
| `./nexus-darknet start` | 🚀 Start all services |
| `./nexus-darknet stop` | 🛑 Stop all services |
| `./nexus-darknet restart` | 🔄 Restart everything |
| `./nexus-darknet status` | 📊 Show service status |
| `./nexus-darknet health` | 🩺 Run health checks |
| `./nexus-darknet menu` | 🎮 Interactive menu |
| `./nexus-darknet panel` | 🖥️ Web control panel |

### 🎯 Service-Specific Commands

| Command | Description |
|---------|-------------|
| `./nexus-darknet start tor` | Start only Tor |
| `./nexus-darknet start i2p` | Start only I2P |
| `./nexus-darknet start yggdrasil` | Start Yggdrasil mesh |
| `./nexus-darknet start ipfs` | Start IPFS node |
| `./nexus-darknet start reticulum` | Start Reticulum |
| `./nexus-darknet start privoxy` | Start Privoxy router |
| `./nexus-darknet start snowflake` | Start Snowflake bridge |
| `./nexus-darknet start batman` | Enable BATMAN-adv mesh |

### 🐙 Sub-Commands

| Command | Description |
|---------|-------------|
| `./nexus-darknet medusa` | 🐙 Multi-head Tor manager |
| `./nexus-darknet snowflake` | ❄️ Snowflake bridge manager |
| `./nexus-darknet doctor` | 🩺 Network diagnostics |
| `./nexus-darknet discovery` | 🔍 Peer discovery |
| `./nexus-darknet p2p` | 📡 P2P network tools |
| `./nexus-darknet alt` | 🔀 Alternative protocols |

### 🐳 Docker Compose Commands

```bash
cd configs

# Container management
podman-compose up -d          # Start all
podman-compose down           # Stop all
podman-compose restart        # Restart all
podman-compose ps             # List containers
podman-compose logs -f        # Follow all logs
podman-compose logs nexus-tor # Specific service logs
```

---

## ❓ FAQ

### 🤔 General Questions

**Q: Is this legal?**
> ✅ Yes! Using Tor, I2P, and mesh networks is legal in most countries. These tools protect privacy and enable free speech.

**Q: Will this slow down my internet?**
> ⚠️ Anonymity networks add latency. Tor adds ~200-500ms. Use HAProxy load balancing for better speeds.

**Q: Can my ISP see I'm using Tor?**
> 🔍 They can see you're connecting to Tor, but not what you're doing. Use bridges or Snowflake to hide Tor usage.

### 🔧 Technical Questions

**Q: How much disk space do I need?**
> 💾 The stack itself needs ~500MB. Container images add ~2GB. IPFS storage varies by usage.

**Q: Can I run this on a Raspberry Pi?**
> 🥧 Yes! Works on any Linux. RPi 4 (4GB+) recommended for full stack.

**Q: How do I add more Tor circuits?**
> 🐙 Use the Hydra system: `./hydra/start-tor-hydra.sh` creates 9 circuits!

**Q: Why is I2P so slow?**
> 🔗 I2P prioritizes anonymity over speed. It gets faster after running for a while as it learns the network.

### 🛡️ Security Questions

**Q: Is this 100% anonymous?**
> ⚠️ No system is perfect. Operational security matters. Don't log into personal accounts, don't download files carelessly.

**Q: Should I use a VPN with Tor?**
> 🤔 Generally no. VPN+Tor can actually reduce anonymity. Tor alone is designed for anonymity.

**Q: How do I know if it's working?**
> ✅ Run `./nexus-darknet health` or visit https://check.torproject.org through the proxy.

---

## 🎉 Congratulations!

You now have a complete understanding of the **NeXuS Darknet Stack**! 🚀

```
    ╔══════════════════════════════════════════════════════════╗
    ║                                                          ║
    ║  🎊 You're ready to browse anonymously! 🎊              ║
    ║                                                          ║
    ║  Remember:                                               ║
    ║    ✨ Use Privoxy (8118) for automatic routing          ║
    ║    ✨ Wait for I2P to build tunnels                     ║
    ║    ✨ Run ./nexus-darknet health to verify              ║
    ║    ✨ Stay safe and respect others' privacy             ║
    ║                                                          ║
    ║  🛡️ Sane • Simple • Secure 🛡️                          ║
    ║  Because working together everyone achieves MORE        ║
    ║                                                          ║
    ╚══════════════════════════════════════════════════════════╝
```

---

**📚 Document Version:** 1.0.0
**📅 Last Updated:** 2026-02-04
**✍️ Generated by:** Claude Opus 4.5 for NeXuS
**🔥 NeXuS Darknet Stack** - The Path to Individual Freedom

