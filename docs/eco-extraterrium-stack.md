# 🪐 NeXuS // ECO — The Extraterrium Stack
*Version 1.2 — 2026-04-01 | Sane • Simple • Secure • Stealthy • Beautiful*

<!-- NEXUS-META
id:           NXS-SPEC-0001
type:         SPEC
principles:   Sane Simple Beautiful
cia:          Integrity Accountability
moe:          Extensibility
pop:          Open-Source
ymca:         Yes-We-Can Multi-AI-Harmony
zero-trust:   false
sovereignty:  local
transparency: full
defense:      passive
created:      2026-04-01
authors:      gemini claude
project:      nexus-eco
layer:         eco
version:       1.2
updated:       2026-04-01
status:        active
audience:      all
tier:          all
score-sane:    4
score-simple:  4
score-beautiful:4
score-composite: 12
score-tier:    ACTIVE
score-level:   2
scored-by:     gemini
scored-at:     2026-04-01
nexium-reward: 72
-->

---

> 🌊 **The Extraterrium Stack** is the bridge between network sovereignty and economic freedom.
> It moves beyond "Proof of Storage" into **Proof of Witnessed Service** —
> governed by a seven-layer cryptographic engine that ensures 100% anonymity
> (UC — Unknown Customer) and 100% trustless settlement.
>
> *The machine handles the work. The math handles the truth. The human handles the freedom.*
> *And when others leave — the ones who stay inherit the network.*

---

## 🏗️ The Seven-Layer Stack

```
┌─────────────────────────────────────────────────────────────────────────┐
│  LAYER 6  │  🏦 Creator Economy (NeXiuM)       │  Spend. Trade. Live.  │
├───────────┼──────────────────────────────────────┼──────────────────────┤
│  LAYER 5  │  ⏳ Residual Stream (Vestal Credits) │  Earn just by being. │
├───────────┼──────────────────────────────────────┼──────────────────────┤
│  LAYER 4  │  👻 Magic Packet (Witness Engine)    │  Random audit. Prove │
│           │                                      │  it or lose it.      │
├───────────┼──────────────────────────────────────┼──────────────────────┤
│  LAYER 3  │  📓 Personal Chain (Local Ledger)    │  Record locally.     │
│           │                                      │  Settle once.        │
├───────────┼──────────────────────────────────────┼──────────────────────┤
│  LAYER 2  │  🔐 PERMITTL Vault (Sovereign Wallet)│  Cold. Sovereign.    │
│           │                                      │  Yours alone.        │
├───────────┼──────────────────────────────────────┼──────────────────────┤
│  LAYER 1  │  🔑 HOTP Multicast (The Gate)        │  Block-timed keys.   │
│           │                                      │  Miss the block?     │
│           │                                      │  Wait for next.      │
├───────────┼──────────────────────────────────────┼──────────────────────┤
│  LAYER 0  │  ⛓️  DIVA Main Chain (Ledger of Math) │  Consensus. Truth.   │
│           │                                      │  No identity. Ever.  │
└─────────────────────────────────────────────────────────────────────────┘
```

---

### ⛓️ Layer 0 — DIVA Main Chain *(The Ledger of Math)*

The foundation. Consensus, math verification, and settlement.

> 🔒 **Privacy guarantee:** DIVA never knows identity. It only verifies that **valid math from a valid node resulted in a valid state change.** That's it. Nothing more passes through.

**Analogy:**
> Think of DIVA as a **Blind Accountant**. You hand them a sealed envelope containing a solved math puzzle. The accountant verifies the math is correct, stamps it "Approved," and moves credits to an account number they can see — but they never see your face, your name, or what you did to solve the puzzle.

**Example:**
> Node A routes 10GB of traffic for the network during Block #4471.
> DIVA sees: *"A valid zk-SNARK proof from a registered node hash. Credits authorised."*
> DIVA does NOT see: who Node A is, what data was routed, or who requested it.

---

### 🔑 Layer 1 — HOTP Multicast *(The Gate)*

Every block, the network fires a multicast. Only nodes holding the correct **PERMITTL root string** can decrypt it and derive their participation key for the *next* block.

> 🚪 **Miss the block → miss the key → no participation that round.**
> No authority locks you out. The math locks you out.

**Analogy:**
> The HOTP Multicast is like a **Synchronized Safe**. Every hour, the lock changes its combination. Only people who have the "Master Key String" can calculate the next combination. If your watch is wrong or you're away when the change happens, you can't open the door until the next cycle.

**Example:**
> Block #4471 fires. The multicast goes to 847 registered nodes.
> 801 nodes decrypt correctly → receive Block #4472 participation key.
> 46 nodes were offline → their multicast window expired → they sit out Block #4472.
> No appeals. No exceptions. Come back online and catch the next one.

---

### 🔐 Layer 2 — PERMITTL Vault *(The Sovereign Wallet)*

Cold storage for all earned credits. Cryptographically bound to the **PERMITTL RING** at registration. Completely decoupled from service identity.

> 💡 **Key insight:** The Vault is the only thing on-chain. The node's working identity (Ring 01) is disposable. Compromise Ring 01 → attacker gets nothing. The Vault is bound to Ring 03 — which is offline, always.

**Analogy:**
> The Vault is your **Underground Bunker**. You use a **Disposable Drone** (Ring 01) to go out and collect supplies (credits). If someone shoots down the drone, they get a pile of scrap metal. They can't follow it back to your bunker because the drone doesn't even know where the bunker is — it only knows how to drop its cargo into a slot.

**Example:**
> Node A earns 47 Vestal Credits over 6 weeks of uptime.
> Node A's service identity (Ring 01) is rotated every block — ephemeral by design.
> Those 47 credits live in the Vault, bound to Ring 03's commitment hash.
> If Node A's routing process is compromised, the attacker sees: a disposable hot key tied to exactly zero vault access.

---

### 📓 Layer 3 — Personal Chain *(The Local Ledger)*

An off-chain private ledger. Records every service rendered between block times. Validated *by* the main chain but opaque *to* it — like a Lightning channel that accumulates locally and settles globally.

> ⚡ **Why this matters:** A busy relay node might handle 10,000 micro-services per epoch. Submitting each one to DIVA would flood the chain. Instead: record locally → batch into a **Single Master Proof** (zk-SNARK rollup) → submit once per epoch.

**Analogy:**
> The Personal Chain is like a **Waiter's Notepad**. Instead of running to the kitchen every time a customer asks for a glass of water, the waiter writes everything down on their pad. At the end of the shift, they hand in one summary sheet. The kitchen sees the total work done and pays out, without needing to know every individual trip to the table.

**Example:**
> Node A relays packets for 847 different circuits during Epoch 12.
> Personal Chain records all 847 events locally with timestamps and signatures.
> At epoch close: rollup fires → one proof → one DIVA transaction → credits unlocked.
> Chain sees 1 transaction. Node did 847 jobs. Nobody is the wiser.

---

### ⛓️ The Styx Protocol *(Proof of Transit)*

Every service in the NeXuS network generates a **Gleipnir Chain** — a linked sequence of cryptographic signatures, one per node that touched the work.

> 🟢 **Chain completes** → credits flow to all participants.
> 🔴 **Chain breaks** → the contract fails; nobody gets paid.

**The Styx Workflow:**
1. **Contract Born:** Requester registers a service contract on DIVA.
2. **Chain Grows:** Each node (A, B, C) that handles the work appends a signed block containing its ID, the previous hash, work units, and a timestamp.
3. **End-of-Path (EOP) Finalizes:** The destination node confirms delivery and submits the full signature chain to DIVA.
4. **Settlement:** DIVA verifies the sequence. If the math holds, the **Treasurer** smart contract auto-distributes credits to all signing nodes.

#### 🔢 The Prime Truth Number *(Styx Truth Token)*

To prevent "Lazy Signing" (nodes signing without doing the work), Styx utilizes **Truth Tokens** — HOTP-style prime numbers.

> 💡 **Concept:** A node requests a one-time prime token from DIVA *before* handling a hop.
> `token = next_prime( HMAC-SHA256(node_secret, contract_id || sequence) )`

- **The Word of Truth:** Primes are mathematically irreducible. You cannot guess them, and you cannot produce the token without the specific work context.
- **Proof of Presence:** The token is burned on use. A node that never requested the token cannot claim the credit.

---

### 👻 Layer 4 — Magic Packet *(The Witness Engine)*

At random intervals, DIVA fires **ghost packets** — encrypted strings derived from actual service data — to random nodes. Only a node that *actually* performed that service can decrypt the challenge string and respond correctly.

> 🟢 **Pass** → service credits confirmed.
> 🔴 **Fail** → vault slashed, node flagged, removed from pool.

**Analogy:**
> The Magic Packet is a **Pop Quiz**. The network randomly asks a node: "Hey, remember that package you delivered three days ago? What was the third word on the shipping label?" If you actually delivered it, you have the record. If you faked the delivery, you can't answer the question.

**Example:**
> Node A claimed to route a circuit for Peer X in Block #4371.
> Block #4501: Magic Packet arrives — "Prove Block #4371, Circuit #882."
> Node A: retrieves local chain entry → decrypts challenge → submits valid response. ✅ Credits confirmed.
>
> Node B claimed the same service but faked it.
> Same packet arrives for Node B.
> Node B: no local context → cannot decrypt → wrong response. ❌ Vault slashed. Pool entry revoked.

---

### ⏳ Layer 5 — Residual Stream *(Vestal Credits)*

Passive income just for being online. No active service required. The Vestal Protocol rewards the thing the network needs most: **a node that shows up.**

> 🕯️ **The Vestal fires burn as long as the node burns.**
> Go dark and they go out.

**See full mechanics:** *The Vestal Vesting Engine* section below.

---

### 🏦 Layer 6 — Creator Economy *(NeXiuM)*

The spendable, tradeable value layer. The end-point of all work.

```
Services rendered
    → Personal Chain records
        → zk-SNARK rollup proves
            → Magic Packet validates
                → Vault unlocks
                    → NeXiuM flows
```

> 🎯 **NeXiuM is earned, not issued.** There is no central mint. No VC allocation. No pre-mine. Every token represents a verified, witnessed service rendered to the network.

**See full mechanics:** *The Economic Springboard* section below.

---

## 🗝️ The PERMITTL RING

The offline, cold trust anchor that rules the node's entire economic life. It never touches the network.

```
┌─────────────────────────────────────────────────────────┐
│  🧊 RING 03 — COLD (The PERMITTL RING)                 │
│  Protocol rules. Key schedule. Master entropy.          │
│  OFFLINE. ALWAYS. Bound to hardware + USB (Iron Boot)   │
├─────────────────────────────────────────────────────────┤
│  🌡️  RING 02 — SEMI-COLD (The Vault)                    │
│  Receives credits. Only online to sweep or trade.       │
│  SOVEREIGN. No one touches this but you.                │
├─────────────────────────────────────────────────────────┤
│  🔥 RING 01 — HOT (The Service Node)                    │
│  Signs personal chain. Handles traffic.                 │
│  DISPOSABLE. Rotated every block. Compromise = nothing. │
└─────────────────────────────────────────────────────────┘
```

**Analogy:**
> The PERMITTL RING is your **DNA**. It never leaves your body (your offline storage). It produces **Cells** (keys) that go out and do work. Your **Skin Cells** (Routing keys) can't perform the function of **Brain Cells** (Vault keys). And just like cells, these keys naturally die and are replaced by new ones generated from your DNA.

### Key Properties

| Property | What It Means |
|----------|---------------|
| 🎯 **Permission-Scoped** | Keys for routing cannot touch the vault. Keys for storage cannot touch governance. Each function is siloed. |
| ⏱️ **TTL Self-Destruct** | Every key has a built-in expiry (block / epoch / season). Old keys die automatically. No revocation lists needed. |
| 🔒 **Mutual Unlock** | Bound to hardware fingerprint + physical USB (Iron Boot). The ring cannot be moved. |
| 🔄 **Revocation Chain** | Pre-committed hash chain (R0…Rn). Silent rotation. No broadcast. No announcement. |
| 💊 **Poison Pill** | One-time emergency broadcast. Freezes all derived keys AND the vault if compromise is detected. |

---

## ⏳ The Vestal Vesting Engine

The Vestal Protocol runs a two-step **earn / vest** cycle. A credit cannot be spent until it has been earned AND vested — two separate events, two separate windows.

> 🕯️ **Earning is the first proof. Staying is the second.**
> The network only pays for both.

### The Two-Step Cycle

```
Hour 0 ──────────────────── Hour 24 ──────────────────── Hour 48
│                            │                            │
│  🟡 EARNING WINDOW         │  🟢 VESTING WINDOW         │  ✅ SPENDABLE
│  Node online + valid       │  Node stays online         │
│  1 Vestal Credit EARNED    │  Credit now VESTS          │  Yours forever
│  (locked, cannot spend)    │  (permanently yours)       │
│                            │                            │
│  Go offline here? ─────────┤ 🔴 FORFEITED               │
│                            │  Streak resets to zero     │
│                            │  Credit → epoch pool       │
```

**Example — Normal cycle:**
> Node A has been online 35 days straight.
> 🟡 Hour 0: earns 1 Vestal Credit. Locked — cannot spend.
> 🟢 Hour 24: credit vests. Permanently in the vault. Untouchable.
> Node A can go offline for maintenance now — the vested credit is safe. The *next* earning window resets, but yesterday's vest is theirs forever.

**Example — Forfeiture:**
> Node B earns 1 credit at Hour 0. Goes offline at Hour 20 (bad timing — power cut).
> 🔴 **Credit forfeited.** Streak reset to zero. Credit redistributed to epoch pool.
> Node B comes back online. Starts again from day 0.

### 🏆 Streak Multipliers

Unbroken uptime is compounding proof of commitment. The network rewards it exponentially:

| 📅 Streak | ✨ Multiplier | 🎁 Bonus |
|:---------:|:------------:|---------|
| 1–29 days | **1×** | Base rate |
| 30 days | **2×** for 14 days | 🏅 Vestal Badge earned |
| 90 days | **3×** continuous | 🔱 Senior Vestal status |
| 180 days | **Black Box opens** | 🎲 VRF lottery fires |

> 🎯 **The longer you stay, the more expensive leaving becomes.**
> A node 89 days in loses 89 days of climb toward 3× if it exits now.
> The multipliers are not rewards — they are gravity wells.

### 🎲 The Black Box — 180-Day VRF

At 180 unbroken days, DIVA fires a Verifiable Random Function. Anyone can verify the math. Nobody can predict the outcome.

```
VRF_proof = sign(node_pubkey + streak_hash + block_height)
```

| 🎰 Probability | 💰 Reward |
|:--------------:|---------|
| 40% | 50 Obol bonus |
| 30% | 200 Obol bonus |
| 15% | 500 Obol + 30-day 2× boost |
| 8% | 2,000 Obol |
| 5% | 5,000 Obol + rare badge |
| **2%** | 🌟 **THE MOTHER LODE** — Oracle Badge + **permanent 1.5× multiplier, forever** |

> 🎯 **The Black Box rewards the rarest thing in any network: a node that will not leave, no matter what.**

---

## 🚀 The Economic Springboard
*How NeXuS turns network stress into a reward for loyalty.*

### ☠️ The Death Spiral Problem

Every token network faces the same failure cliff:

```
📉 Reward drops
    → 🏃 Nodes leave
        → 📉 Fewer nodes = less reward per epoch
            → 🏃🏃 More nodes leave
                → 📉 Even fewer
                    → 💀 Network collapses
```

---

### 🔄 The NeXuS Inversion

NeXuS is designed so that **nodes leaving makes staying more valuable.**

> 💡 The epoch reward pool is **fixed**. It doesn't shrink when nodes leave.
> It gets divided among fewer nodes.
> **Fewer nodes = larger slice per node.**

```
😰 Market stress event
    │
    ├── 🏃 Weak nodes exit (streak <30d, no multiplier)
    │       └── Unvested credits → 🔴 forfeited → return to epoch pool
    │
    ├── 📈 Remaining nodes: per-node share INCREASES immediately
    │       └── Streak nodes approaching 30/90/180d thresholds:
    │               streak multipliers COMPOUND on the larger base
    │
    ├── 🆕 New nodes: see higher base payout → entry more attractive
    │
    └── 🏗️ Network stabilizes smaller but STRONGER
            └── Next growth cycle starts from a harder floor
```

---

### 📊 The Springboard Visualised

```
Network size vs. Per-node reward (single epoch)

Per-node
reward
  ▲
  │ ★ ← small network, high per-node reward
  │  ★
  │    ★
  │       ★
  │           ★
  │                ★★★
  │                      ★★★★★★★★
  └──────────────────────────────────► Node count
     ↑
   stress event
   weak nodes exit
   → slide LEFT on this curve
   → remaining nodes move UP
   → reward INCREASES
```

### Three Mechanisms Working Together

**1. 🏦 Fixed Epoch Emission Pool**

The pool per epoch is not proportional to node count. Whether 50 nodes or 50,000, the epoch emits the same total. Fewer nodes = bigger individual slice.

> **Example:** Epoch pool: 1,000 Obol. 1,000 nodes → 1 Obol each. 200 nodes leave → 800 remain. Same 1,000 Obol → now **1.25 Obol each**. Streak nodes at 3× → **3.75 Obol per epoch**. The nodes that stayed just got a 25% raise from the ones who left.

**2. 🔄 Forfeiture Redistribution**

When a node breaks its streak and goes offline before vesting, its unvested credits re-enter the epoch pool immediately. Abandonment is converted directly into a payment to nodes that stayed.

> **Example:** 50 nodes panic-exit during a bad week. Each had 3 unvested credits. 🔴 150 Obol returns to the epoch pool. The 750 nodes that stayed split those 150 Obol on top of normal epoch emission. The people who left **funded a bonus for the people who didn't.**

**3. ⚓ Streak Multipliers as Loyalty Insurance**

```
Day 1:   Leave → lose nothing extra.          Easy exit.
Day 29:  Leave → lose 29 days toward 2×.      Getting expensive.
Day 89:  Leave → lose 89 days toward 3×.      Very expensive.
Day 179: Leave → lose 179 days toward Black Box. Almost no rational actor exits here.
```

---

## 🔗 Protocol Integration Summary

| 🔧 Protocol | 🏗️ Layer | ✅ What It Proves | 💰 Settlement |
|------------|:-------:|-----------------|--------------|
| ⛓️ **Styx / Gleipnir** | 3 | Work was done (transit, storage, compute) | Obol via personal chain rollup |
| 🕯️ **Vestal** | 5 | Node was available and online | Vestal Credits (earn → vest → spend) |
| 🎲 **Black Box VRF** | 5 | 180 days of unbroken proof | Obol lottery — verifiable, unpredictable |
| 👻 **Magic Packet** | 4 | Service was real, not faked | Pass = confirmed / Fail = vault slashed |
| 🗝️ **PERMITTL Ring** | 0/2 | Identity is valid and offline-anchored | Participation key derived per block |
| 🚀 **Springboard** | 6 | Loyalty under stress is the network's real asset | Forfeiture → redistribution → stronger stayers |

---

> **🌀 NeXuS: Together Everyone Achieves More**
> *mewe — sovereign me inside a we*
> *The machine handles the work. The math handles the truth. The human handles the freedom.*
> *And when others leave — the ones who stay inherit the network.*
