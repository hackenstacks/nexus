---
hide:
  - navigation
  - toc
---

# :dragon: NeXuS

<div class="hero-tagline" markdown>

## *Sane • Simple • Secure • Stealthy • Beautiful* 🔥

### **Together Everyone Achieves More** 🌀

*Your hardware. Your data. Your network. Your rules.*

</div>

---

![NeXuS 30,000ft Architecture Blueprint](nexus-30kft-view-blueprint.png)

---

## :question: What Is NeXuS?

**NeXuS** *(Network eXchange Universal System)* is a **CLI-first, privacy-native operating system** and a **decentralized mesh network** — *built by the people, for the people.*

- 🔒 **Your hardware** generates value that flows **directly back to you**
- 🌐 **Every node** that joins makes the network **stronger for every other node**
- 🚫 **No platform. No intermediary. No company that can be shut down.**
- 🤖 **AI-enhanced locally** — no cloud required, no telemetry, ever
- 🛡️ **Censorship-resistant by default.** *Anonymous by design.*

> *"The system is in debt to the user and shall mathematically pay out."* 💰

Built on **Alpine Linux Edge.** Runs on *anything* — from a 2011 laptop to a modern server rack.

---

## :material-layers-triple: The 8-Layer Stack

*Each layer is independent — a node with only the bottom three layers is already a valid private node.*

| Layer | Component | What It Does |
|-------|-----------|-------------|
| 🖥️ **Interface** | NeXuS Wallet | *Command Center — your browser into the network* |
| 💬 **Social** | Townhall + Badges | *Marketplace, NFT Forge, peer reputation* |
| 💰 **Economy** | NeXuS Chain + DIVA | *Private value transfer, atomic swaps, credits* |
| 🤖 **AI** | Ollama + aichat | *Local intelligence — no cloud, no telemetry* |
| 🔐 **Security** | Zero Trust Fortress | *Rootless containers, AppArmor, nftables* |
| 👻 **Privacy** | Medusa Routing | *Tor + I2P + Privoxy — all traffic darknet by default* |
| 📡 **Mesh** | Transport Stack | *Tor + I2P + Yggdrasil + Reticulum — survives infrastructure collapse* |
| 🏔️ **OS** | Alpine Linux Edge | *Hardened, minimal, runs on legacy hardware* |

---

## :rocket: Quick Start

!!! warning "⚠️ Status: v0.1 Network Layer"
    The **privacy and transport layer** is ready and working right now!
    The **economy layer** (chain, wallet) is Phase 1 — *actively being built.*

=== ":material-script: Option 1 — Install Script *(Recommended)*"

    ```bash
    # One command deploys the full stack
    curl -fsSL https://hackenstacks.github.io/nexus/install.sh | bash
    ```

    !!! note "🚧 Coming Soon"
        The install script is in active development. See [Install Architecture](install-architecture.md) for the full spec.

=== ":material-penguin: Option 2 — Manual Alpine Setup"

    ```bash
    # 1. Start with a fresh Alpine Linux install
    # 2. Clone the network stack
    git clone https://github.com/hackenstacks/NeXuS-NetWork-Stack.git
    cd NeXuS-NetWork-Stack

    # 3. Run the installer
    ./nexus-install.sh

    # 4. Start the stack
    podman-compose -f nexus-compose.yaml up -d

    # 5. Verify everything is healthy
    ./nexus-doctor.sh
    ```

=== ":material-docker: Option 3 — Container Only *(Try It Now!)*"

    ```bash
    # Run just the privacy stack — no OS install needed
    # Requires: podman or docker

    git clone https://github.com/hackenstacks/NeXuS-NetWork-Stack.git
    cd NeXuS-NetWork-Stack
    podman-compose up -d

    # ✅ Your traffic now routes through Tor + I2P + Privoxy
    # SOCKS5 proxy: localhost:1080
    # HTTP proxy:   localhost:8888
    ```

---

## :material-download: Downloads

!!! info "🚧 Downloads coming soon — *placeholders show what's on the way*"

<div class="grid cards" markdown>

-   :material-disc: **NeXuS ISO** *(Coming Phase 1)*

    ---

    *Bootable Alpine Linux image with full NeXuS stack pre-installed.*
    **Boot. Login. Connected.**

    - 🔒 Privacy stack pre-configured
    - 🤖 Local AI included (Ollama)
    - 🖥️ Optional Wayland desktop (labwc)

    **Status:** 🔨 In development — Phase 1

    [:octicons-arrow-right-24: Track Progress](#){ .md-button .md-button--primary }

-   :material-script-text: **Install Script** *(Coming Phase 1)*

    ---

    *One script. Alpine, Debian, Arch, or Fedora.*
    Detects your OS, installs everything, configures automatically.

    - 🔍 Auto-detects distro
    - ⚙️ Full stack in one command
    - 🩺 Built-in health checker

    **Status:** 🔨 In development

    [:octicons-arrow-right-24: Track Progress](#){ .md-button .md-button--primary }

-   :material-docker: **Container Stack** *(Available Now!)*

    ---

    `podman-compose` file for the **privacy stack.**
    *Run Tor + I2P + Medusa routing on any Linux system right now.*

    - ✅ Works today
    - 🔒 Rootless Podman containers
    - 👻 5-circuit rotating anonymity

    **Status:** ✅ Available — see Quick Start above

    [:octicons-arrow-right-24: View on GitHub](https://github.com/hackenstacks/NeXuS-NetWork-Stack){ .md-button }

-   :material-wallet: **NeXuS Wallet** *(Command Center)*

    ---

    *The Command Center.* Dashboard, swap, NFT Forge, node controls,
    stake/loan, badge display. **Your browser into NeXuS.**

    - 💰 Multi-chain (XMR/BTC/NeXuS)
    - 🏆 Peer reputation & badge system
    - 🎨 NFT Forge built-in

    **Status:** 🔨 Phase 1 — not yet built

    [:octicons-arrow-right-24: Read the Design](economy-architecture.md){ .md-button }

</div>

---

## :white_check_mark: What's Working *Right Now*

<div class="grid" markdown>

<div markdown>

### ✅ Ready Today

- ✅ **Tor + obfs4 + Snowflake + WebTunnel** — *censorship-resistant transport*
- ✅ **I2P (i2pd)** — *garlic routing, hidden services*
- ✅ **Yggdrasil** — *encrypted mesh overlay*
- ✅ **Reticulum** — *LoRa / radio / off-grid mesh*
- ✅ **Medusa Proxy** — *5-circuit rotating anonymity*
- ✅ **dnscrypt-proxy + unbound** — *zero clearnet DNS leaks*
- ✅ **nftables firewall** — *all traffic darknet by default*
- ✅ **Podman rootless containers** — *no root daemon anywhere*
- ✅ **Ollama + aichat** — *local AI, zero cloud*
- ✅ **labwc + sway** — *Wayland desktop (optional)*
- ✅ **foot + tmux** — *CLI-first terminal standard*
- ✅ **Full CLI toolkit** — *fzf, bat, rg, neovim, htop...*

</div>

<div markdown>

### 🔨 Building Now *(Phase 1)*

- 🔨 **NeXuS chain daemon** — *Monero fork, AstroBWT mining*
- 🔨 **NeXuS wallet** — *Command Center interface*
- 🔨 **IPFS integration** — *distributed storage layer*
- 🔨 **DIVA chain connector** — *social + coordination layer*
- 🔨 **One-command install script** — *curl | bash magic*
- 🔨 **NeXuS ISO** — *boot-and-go image*

### 🌅 Coming *(Phase 2)*

- 🌅 **Townhall marketplace** — *buy/sell/trade on-chain*
- 🌅 **NFT Forge** — *creator tools built-in*
- 🌅 **Stake / Loan contracts** — *earn from your credits*

</div>

</div>

---

## :books: Documentation

<div class="grid cards" markdown>

-   :material-information-outline: **What Is NeXuS**

    ---
    *Full history, vision, and philosophy* — from personal privacy fortress to the full crypto-republic economy.

    [:octicons-arrow-right-24: Read the Story](what-is-nexus.md)

-   :material-network: **Medusa Stealth Routing**

    ---
    *How the 5-circuit Tor load balancer* and I2P routing work together for **anonymous traffic.**

    [:octicons-arrow-right-24: Dive In](medusa-stealth-routing.md)

-   :material-currency-usd: **Economy & Blockchain**

    ---
    *The full design* — Monero fork, DIVA integration, wallet architecture, Townhall marketplace, tokenomics.

    [:octicons-arrow-right-24: Read the Spec](economy-architecture.md)

-   :material-application-cog: **Required Applications**

    ---
    *Every application in the stack* — why it's there, container vs native, and current status.

    [:octicons-arrow-right-24: See the Stack](required-applications.md)

-   :material-server-network: **Install Architecture**

    ---
    *Container vs native decisions*, the 9-step install flow, open questions answered.

    [:octicons-arrow-right-24: Architecture Deep Dive](install-architecture.md)

-   :material-robot-happy: **AI in NeXuS**

    ---
    *Local AI, Project Chimera, DPFOE framework,* merged identity, *the Event Horizon.*

    [:octicons-arrow-right-24: Meet the AI Layer](ai-merged-identity.md)

-   :material-shield-lock: **Three-Key Security Model**

    ---
    *Never Trust. Always Verify. Expect Betrayal.* Zero-trust security from boot to app.

    [:octicons-arrow-right-24: Security Deep Dive](3key-security.md)

-   :material-zombie: **Zombie Apocalypse Mode**

    ---
    *What happens when the internet dies?* NeXuS keeps running on LoRa, radio, and mesh.

    [:octicons-arrow-right-24: Survive Anything](zombie-apoc-doc.md)

</div>

---

## :bulb: The Philosophy

```
╔═══════════════════════════════════════════════════════════╗
║                    R E V E R S E D.                       ║
╠══════════════════════════╦════════════════════════════════╣
║  Traditional             ║  NeXuS                        ║
╠══════════════════════════╬════════════════════════════════╣
║  User owes platform      ║  Platform owes user  💰       ║
║  Buy votes with money    ║  Weight = contribution  🗳️    ║
║  Buy credits             ║  Only earned, never bought 🏆 ║
║  Hoard control           ║  Elastic supply dilutes it 📉 ║
║  Rich node earns more    ║  Same RATE for every node  ⚖️  ║
╚══════════════════════════╩════════════════════════════════╝

        The math is the law.
        The protocol settles the debt.
        No human in the loop.
```

*"Where everyone has a voice, and every voice protected."* 🌀

---

## :heart: Join the Collective

*NeXuS is built by contributors like you.* Every line of code, every node running, every idea shared — it all matters.

- 🐛 **Found a bug?** Open an issue on GitHub
- 💡 **Have an idea?** Start a discussion
- 🔧 **Want to contribute?** Read the docs, clone the repo, send a PR
- 📡 **Run a node?** You're already part of the network

> *Together Everyone Achieves More* 🌀

---

<div style="text-align: center; padding: 2rem 0; color: var(--md-default-fg-color--light)">

**:dragon: NeXuS** — *Network eXchange Universal System*

*Sane • Simple • Secure • Stealthy • Beautiful*

[GitHub :fontawesome-brands-github:](https://github.com/hackenstacks/nexus){ .md-button } &nbsp;
[Network Stack](https://github.com/hackenstacks/NeXuS-NetWork-Stack){ .md-button } &nbsp;
[Quick Start :rocket:](#quick-start){ .md-button .md-button--primary }

</div>
