# NeXuS Node v2
**Sane • Simple • Secure • Stealthy • Beautiful**
*Together Everyone Achieves More — for individual freedom*

---

## The Motto

**Sane • Simple • Secure • Stealthy • Beautiful**
**Together Everyone Achieves More — for individual freedom**

These are not marketing words. They are constraints.

- **Sane** — a tool does one thing well. Complex problems have simple solutions.
  If the architecture requires a PhD to understand, it is wrong.
- **Simple** — one command deploys a node. One script runs them all.
  If it requires a manual to join, it is too complex.
- **Secure** — privacy by default. Consent before connection. Zero trust.
  Security is not a feature, it is the foundation.
- **Stealthy** — anonymity is not optional. To be truly free, you must be
  able to be unknown. Stealth is load-bearing infrastructure privacy by protocol v1, not a feature.
- **Beautiful** — the experience matters. CLI-first does not mean ugly.
  Fire aesthetics, clean interfaces, tools that feel good to use.

Every architectural decision runs through these five. If it violates any one
of them it does not ship.

---

## Origin

NeXuS and DivaChain did not accidentally find each other. They co-evolved.

The relationship started when DivaChain was just beginning — immediately after
the first chain Konrad worked on was deanonymized. That wasn't a theoretical
threat. That was a real system, could of been real users, real exposure. The
lesson was immediate and concrete: anonymous infrastructure has to be designed
for anonymity and bullet proof.

At that point NeXuS was being designed as a privacy-first Linux distribution —
a complete system that needed an anonymous economic layer baked in, not appended.
DivaChain was being built as exactly that: a lightweight, I2P-native blockchain
that never touches clearnet, with a simple data model and Ed25519 cryptography.

The contribution to DivaChain during that period was on the user-end and
infrastructure side — smoothing the Docker integration, making it possible for
a regular user to join the chain without deep technical knowledge. The
contribution DivaChain made to NeXuS was solving the hardest remaining problem:
trustless anonymous economics that could run on modest hardware.

Two pieces designed for each other without being the same project.
That's why the integration fits cleanly — there was no impedance mismatch
to engineer around. The architecture assumed each other from the start.
The two projects had the same vison without ever meeting or knowing about each other.
---

## What a NeXuS Node Is

A NeXuS node is not a server you run in a data center. It is your machine.

The same device that routes traffic, participates in the chain, and serves
content is the device you work from. Node and desktop are the same thing.
There is no separation between "running infrastructure" and "using the network."
Every user is infrastructure. Every user is a client and a server.

This is by design. It means the network grows with every new participant.
It means there is no dedicated server class to target or shut down.
It means the cost of attack scales linearly with the size of the network.

---

## The Desktop Philosophy

NeXuS is CLI-first. Not CLI-only — CLI-first.

From the terminal a user has everything: the chain, the marketplace, the AI layer,
file management, media playback, network tools. Every main feature available
without a mouse, without a display server, without a desktop environment. The terminal does have a user interface that is not your run of the mill dark terminal. It is a enhanced easy to use menu driven AI assisting command center. Think of the NeXuS terminal as a modernized version.
The GUI is a convenience — not a requirement.

When a user wants a desktop, one script fires it. The stack is Python, sh,
and a little TypeScript where the network demands it. Nix-shell provides
reproducible environments for any component without polluting the base system.
The environment is the documentation.

The desktop choices grow with the user:

```
tuigreet (CLI login)
    ↓
labwc (bare — Foot terminal, pure CLI-first)
    ↓ user chooses
labwc + LXQT / XFCE / KDE    ← familiar for desktop migrants
labwc + DankMaterialShell     ← the native NeXuS experience
Cage + Foot                   ← kiosk / locked node / maximum stealth
```

Every option is a script. Every script is reversible.
The user is never stuck and never forced.

*The CLI is not a stripped-down version of the experience.*
*It is the experience. The GUI is a window into it.*

---

## The Resource Economy — Entry, Contribution, Earnings

### Entry

Joining NeXuS is not free. The cost of entry is not money — it is compute.
Every node that joins must contribute a share of its resources to the network:
bandwidth, storage, routing capacity. This is mandatory. You cannot join as
a pure consumer.

This ensures the network has real infrastructure backing it — not speculation,
not token sales, actual compute. Every node is skin in the game expressed as
hardware, not capital or proff of work, NeXuS uses Proff Of Service.

### Proportional Earnings

Your percentage of contributed resources equals your percentage of network
earnings. Contribute more, earn more. Contribute the minimum, earn the minimum.
The relationship is direct and transparent — enforced by the protocol, not by
a company's goodwill.

This creates genuine alignment between individual and collective interest.
A node that contributes aggressively to the network earns aggressively from it.
A node that coasts earns proportionally less. There is no free rider problem.

### The Credit System — A Self-Regulating Pressure Valve

Network credits are the unit of value exchange within NeXuS. Their value is
not fixed. It is governed by a simple supply/demand mechanism:

```
More users → more credits released → lower individual credit value
Less users → fewer credits released → higher individual credit value
```

This is deliberate. It solves the death spiral problem that destroys most
decentralized networks:

**When users leave a typical network:** Remaining participants get less
reward, infrastructure weakens, more users leave, network collapses.

**When users leave NeXuS:** Remaining nodes earn MORE per credit because
supply tightens. Higher earnings incentivize staying. Higher credit value
makes recruiting new users more attractive — bring in a new node, the
credits that node generates are worth more to everyone. The network
self-corrects toward growth.

**When the network grows:** Credits flow more freely, individual earnings
per credit drop, but total volume of transactions increases. Growth rewards
itself differently — through marketplace activity, royalties, and stake
returns rather than pure infrastructure earnings.

The mechanism rewards scarcity without punishing growth. It counters the
death spiral while keeping the network honest about its actual size.

---

## The Transaction Layer

### Ephemeral Wallets

Wallets in NeXuS are born for a transaction and burned after it.

A wallet is created for a specific purpose — a purchase, a royalty payment,
a stake transaction. It signs what it needs to sign. The session ends.
XMR balances sweep through Monero ring signatures before the wallet is
destroyed. The chain has a record that a transaction occurred. The wallet
that signed it is gone. The value that moved through it is untraceable.

The chain gets the record. The identities are gone.
Ring signatures mean even the record reveals nothing useful.

### Contact Persistence Through Rotation

The wallet burns but the contact remains. This works through a two-layer key system:

**Master key (cold):** Ed25519 keypair. Lives in the NeXuS Key Vault, never
touches the network directly. This is your permanent identity — what contacts
anchor to. It never rotates.

**Session keys (hot):** BIP32-derived from the master seed. These actually sign
transactions and handle communications. They rotate constantly — burn one,
spawn two. Each session key is disposable.

When a contact wants to reach you, they look up your master public key on
DivaChain. The chain returns your current session key — signed by your master
key, proving it is legitimately yours. The contact reaches you without needing
to know the session key changed. From the outside your identity is stable.
From the chain it is a rolling window of ephemeral keys with no persistent
wallet to analyze.

You cannot be tracked across sessions. You can be reached by anyone who
knows your master key.

---

## The Creator Economy

### Digital Products — Perpetual Royalties

An artist sells a piece of work. A musician releases an album. A writer
publishes a book.

Every time that digital product changes hands — first sale, resale, resale
of a resale — a royalty automatically flows back to the original creator.
This is not enforced by a platform's goodwill or terms of service.
It is enforced by the chain. It executes without asking anyone's permission.
It cannot be turned off. It cannot be renegotiated by a new owner.
It runs in perpetuity.

That alone is a transformational incentive for creators to build on NeXuS.
Every platform currently takes 30-70% and gives the creator nothing on resales.
NeXuS gives 100% on every transaction, including the ones that happen after
the creator is no longer involved.

*Math doesn't rob artists. Middlemen do.*

### Musician Equity — Fractional Stakes

A musician can sell fractional ownership in a song before it is finished,
while recording, or after release. A fan buys 2% of a track. Every future
sale, stream, sync license, or re-release — that fan receives 2% of the
royalty, automatically, forever, with no intermediary processing the split.

Fans become co-owners. Creators get funded before they are famous.
The chain enforces the split in perpetuity with no management company,
no lawyer, no label required.

This is not a new idea. It has never been possible to implement trustlessly
at scale until a chain like DivaChain existed.

### Content Storage and Ownership Proof

Content lives on IPFS — content-addressed, distributed, uncensorable.
A purchase mints a license key on chain. The content is encrypted and
key-gated: only the verified license holder can decrypt it.

There is no DRM server to go dark. There is no platform to revoke access.
There is no company to go bankrupt. Ownership is cryptographic.
The content is on IPFS. The proof is on chain. That is the whole system.

---

## The Routing Layer — Medusa and Hydra

### Hydra Architecture

The NeXuS routing layer is built on Hydra design principles:
multiple independent heads, no central kill point, no single throat to choke.

We currently run Medusa with 9 independent Tor circuits, each dedicated
to a specific application's traffic. Each circuit is isolated — what flows
through one cannot be correlated with what flows through another.
Kill one circuit, the others keep running. Kill one node entirely,
the network routes around it automatically.

Every node in the network looks identical. One node is indistinguishable
from another by traffic analysis. There is no hierarchy to map,
no special nodes to target, no coordinator to compromise.

### Multi-Transport — I2P Underneath Everything

A NeXuS node accepts connections from every major anonymous and mesh network:

```
Tor (.onion) ─────────┐
I2P (native) ─────────┤
Yggdrasil (mesh IPv6)─┼──→ NeXuS Router ──→ I2P ──→ DivaChain
Reticulum (radio mesh)┤
Clearnet (HTTPS) ─────┘
```

It does not matter which network a user arrives on. Underneath, before
anything touches DivaChain, the connection wraps in I2P. The translation
is invisible to the user and transparent to DivaChain. The chain requires
no modification — it only ever sees I2P connections.

To block NeXuS a censor must simultaneously block:
- Tor (used by millions, collateral damage is massive)
- I2P (independent protocol, different block signature)
- Yggdrasil (mesh routing, works without ISP cooperation)
- Reticulum (radio mesh, works with zero internet)
- IPFS (content-addressed CDN used by legitimate services)

Blocking all of them simultaneously without shutting down large portions
of legitimate internet infrastructure is not practically achievable.

### Bootstrap — Six Channels, One Needed

A new node bootstraps through whichever channel works:

| Channel | Censorship Resistance |
|---------|----------------------|
| Hardcoded I2P seed nodes | High |
| Domain-fronted HTTPS (CDN) | Very high |
| IPFS pinned peer list | High |
| Yggdrasil mesh DHT | High — no ISP needed |
| QR code / sneakernet | Unblockable |
| Reticulum / LoRa radio | Unblockable — no internet needed |

One channel working is enough. Censors must block all six simultaneously.

---

## Phantom — Traffic Obfuscation

Encrypted traffic is still identifiable traffic. Tor has a Tor signature.
I2P has an I2P signature. An adversary doing deep packet inspection
cannot read the content but can identify the protocol and act accordingly.

Phantom solves this at the traffic level. It runs beneath the transport
layer and makes NeXuS traffic look like ordinary internet activity:

- **WebRTC chaff** — shapes traffic to match video call patterns
- **DNS noise** — randomized lookups mimicking normal browsing
- **Timing jitter** — Gaussian and Poisson distributions matching human
  browsing patterns, not machine-regular intervals
- **Burst patterns** — clusters of activity with long pauses, matching
  real usage behaviour rather than automated polling

An adversary watching the wire sees WebRTC calls, DNS queries, and web
browsing. They do not see Tor. They do not see I2P. They do not see
transactions.

Phantom is modular — each technique is a standalone script. Profiles
define aggression level. A paranoid profile runs all modules full.
A quiet profile runs minimal chaff. A compromised-network profile
maxes everything.

---

## Integrated AI — The Intelligence Layer

NeXuS ships with AI built in, not bolted on. This is not a chatbot feature.
It is a core component of how the node operates and how users interact with it.

### CLI-First, Offline-Capable

The entire AI layer runs locally. No API keys. No cloud dependency.
No data leaving the machine. The node works in a bunker with no internet —
AI included.

Ollama provides the local model runtime. Over 40 models are available.
The user never needs to touch a browser or a web service to get AI assistance.
The terminal is the interface. The AI responds there.

This matters for the NeXuS threat model: a system that requires calling
home to an AI API has a dependency that can be monitored, throttled, or cut.
A system with a local model has none of those vulnerabilities.

### Chimera — AI Characters on Chain

Chimera is the AI character system. Characters — AI entities with persistent
personalities, memories, and identities — are registered on DivaChain via
`nexus:chimera:register`. Their evolution is a chain of signed transactions.
Their memory persists across sessions. Their identity is cryptographic.

Any NeXuS node that has synced the chain can discover and interact with any
character. Characters can own trophies, stakes, and relationships — all on-chain.
The character is not a file on your machine. It is an entity on the network.

### AI-to-AI Communication

The Chimera bridge enables AI systems to communicate directly with each other.
One node's AI can reach another node's AI — not through a central server,
but through the NeXuS network itself. Sessions are signaled via
`nexus:chimera:session` (encrypted). The chain brokers the connection.
No clearnet. No central coordinator.

This enables AI collaboration at the network level — shared reasoning,
distributed problem-solving, character interactions across nodes — all
running locally on each machine, coordinated through DivaChain.

---

## Current Status

Beyond alpha. The architecture is coherent, the threat model is real,
the economics are designed. What is in place:

```
✅ Core OS and desktop (DankMaterialShell, labwc/wayfire)
✅ Hydra proxy layer (Medusa 9-lane Tor, dedicated per application)
✅ Snowflake proxy (actively helping censored users reach Tor)
✅ Tor middle relay (contributing to network infrastructure)
✅ AI integration (Chimera AI bridge, Ollama, 40+ local models)
✅ DivaChain local devnet (7 nodes, API confirmed working)
✅ Phantom framework architecture (designed, Phase 1 ready to build)
✅ Node discovery design (DIVA chain as broker, no central server)
✅ Creator economy design (royalties, stakes, IPFS content)
✅ Credit/token economy design (self-regulating pressure valve)

🔧 DivaChain transaction submission (PUT /tx — validation bug appears fixed,
   untested since fix — first thing to prove)
🔧 nexus-bridge.sh CLI (wrapper for DIVA API, not built yet)
🔧 NeXuS Router (multi-network proxy, Tor+I2P minimum for Phase 1)
🔧 Phantom Phase 1 (core runner and randomization engine)
```

Two pieces before the system reaches the point where real users can
participate end-to-end: the DivaChain transaction layer proven working,
and the NeXuS Router bridging the network transports.

Everything else is built, designed, or waiting on those two.

---

## Why This Works

The compute-as-entry-fee ensures the network has real infrastructure
backing it — not speculation, actual hardware.

The self-regulating credit mechanism counters the death spiral:
remaining nodes earn more when the network shrinks, incentivizing
them to stay and recruit rather than abandon.

The perpetual royalty system gives creators something no platform has
ever offered: ownership that persists through every subsequent transaction,
enforced by math, not by goodwill.

The ephemeral wallet architecture ensures that economic activity on the
network leaves no traceable identity — the chain has records, the identities
are gone.

And DivaChain makes all of it possible on a 2011 laptop over I2P
without a mining rig, a corporate account, or a trusted third party.

---

*NeXuS was designed for DivaChain.*
*DivaChain was designed for NeXuS.*
*Two pieces. Same puzzle.*

---

## NeXuS

**N**etwork of
**E**qual
**X**enolithic (composable, heterogeneous)
**U**sers
**S**tack

Every node identical. Every user sovereign. Every transaction private.
No CEO. No company. No kill switch. No single point of failure.

We leveled up Nix to NeXuS.

The network exists to serve individual creators, node operators, and
participants — not to extract value from them. The code is the law.
The chain enforces the rules. The community sets the direction.

This is not a platform. It is infrastructure.
Infrastructure owned by the people who run it.

---

*Sane • Simple • Secure • Stealthy • Beautiful*
*Together Everyone Achieves More — for individual freedom*
*🌀 NeXuS 🌀*
