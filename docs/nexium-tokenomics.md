<!-- NEXUS-META
keywords: nexium tokenomics monero dero fork smart-contracts perpetual-royalties ephemeral-wallet master-key proof-of-service xmr denaro kovri diva settlement zkp pseudonymous-namespace
projects: nexus-orchestrator nexus-ghost-gate nexus-audit nexium
status: active — open decision on value layer
type: specification
date: 2026-03-24
authors: anon claude gemini
-->

# NeXiuM Tokenomics
*The NeXuS contribution economy — how the network pays its nodes*
*Updated: 2026-03-24*

---

> **Correct spelling: NeXiuM**
> Capital N, capital X, lowercase i-u, capital M.
> Follows NeXuS style — the X is always marked. The M closes it.

---

## The Foundation

**The network is in debt to the nodes. Math settles the debt. No middlemen.**

No central authority collects or distributes.
Smart contracts enforce payment.
ZKPs prove contribution.
The network owes nodes for their service — settled automatically by code and math, not by policy or trust.

```
Trustless by code and math, operating on proof of service.
```

---

## Two Economies — Do Not Mix

NeXuS runs two separate economic layers. They do not overlap. They do not swap.

| Economy | Token | Lives On | Purpose | Privacy Mechanism |
|---------|-------|----------|---------|------------------|
| **Contribution** | NeXiuM | DIVA chain | Earned by serving the network | Pseudonymous namespace key + ZKP + I2P transport |
| **Creator payments** | XMR | Monero network | Fan pays creator directly for content | RingCT native |

NeXiuM is not XMR. XMR is not NeXiuM. They serve different purposes and are never exchanged.
A node earns NeXiuM for routing. A creator earns XMR for selling music. Two separate flows, two separate chains, no bridge needed.

**NeXiuM privacy — how it actually works on DIVA:**

DIVA stores state as namespace key-value entries. NeXiuM balance lives at:
```
nexus:nexium:<node_key_hash>  →  balance
```
The namespace key is a hash derived from the node's public key — pseudonymous.
It cannot be directly linked to the node's identity without knowing the key.
I2P transport (DIVA's native layer) hides who is writing to the namespace.
ZKP proves "I have sufficient NeXiuM" without revealing the key or the balance amount.

Privacy comes from: pseudonymous key + ZKP balance proof + I2P transport.
No swap mechanism needed. No RingCT needed on DIVA.

---

## Why Monero, Not Denaro

Monero was evaluated against alternatives. Denaro was considered and rejected.

**The specific blocker: Denaro's license.**
Denaro's license terms were incompatible with NeXuS's freedom-first stack.
NeXuS cannot build privacy infrastructure on a tool that restricts the freedom it is designed to protect.

**Monero won:**
- License: truly open (MIT/BSD/CC0 — no restrictions, no gotchas)
- Largest privacy-coin ecosystem, hardened against adversaries over years of real attacks
- Ring signatures + stealth addresses + RingCT = strongest privacy stack available today
- Community is adversarial-hardened — survived exchange delistings, regulatory pressure, chain analysis attacks
- XMR is the adversary's nightmare. That is exactly what NeXuS needs.

**Decision locked: XMR for NeXuS payments. Denaro is out.**

---

## The Open Decision — Fork vs Use

### What Was Being Decided (opened 2026-03-07, not yet closed)

**Question: What is the NeXiuM value layer?**

Three options were evaluated:

---

### Option A — Fork Dero

Dero is a Monero fork. It added EVM smart contracts + DAG on top of Monero's RingCT + stealth addresses.
First chain to combine privacy AND full programmability.

**What NeXuS gets from a Dero fork:**
- Privacy by default — RingCT means every NeXiuM transaction is private, no extra layer needed
- Smart contracts — perpetual royalties, ephemeral wallet logic, programmable keys — all native EVM
- Kovri — I2P integration for Monero-based nodes, embeds I2P routing directly into p2p layer
- One chain, not two systems glued together

**Cost:**
- You own the chain — validators, bootstrapping, upgrades, security, forever
- Competes with Dero's existing token and community
- High engineering lift before first node runs

---

### Option B — Secret Network

TEE (Trusted Execution Environment) — privacy enforced by Intel SGX hardware.

**Rejected:** NeXuS does not trust hardware manufacturers.
Trustless by code and math. Intel SGX is not math. Intel is not trusted.

---

### Option C — DIVA as value layer ✅ DECIDED

**This is the answer. Already locked in the Konrad partnership documents.**

From `NEXUS_DIVA_OVERVIEW_FOR_KONRAD.md`:
> *"NeXuS needed an economic layer that never touches clearnet, runs on modest hardware,
> has a simple data model, and uses Ed25519 cryptography. DivaChain is the only chain
> that matches every requirement without compromise. It does not generate speculative tokens.
> It does not require mining. It is I2P-native by design, not by configuration.
> NeXuS is the network. DivaChain is the ledger underneath it. Two pieces, same puzzle."*

**Why DIVA won:**

| Requirement | DIVA | Dero Fork |
|-------------|------|-----------|
| I2P-native by design (not by config) | ✅ | ❌ (Kovri = separate layer) |
| Ed25519 cryptography (matches NeXuS key system) | ✅ | ❌ (different key scheme) |
| No speculative token / no mining | ✅ | ❌ |
| Runs on Screaming Demon hardware (modest) | ✅ | ❌ (chain = heavier) |
| Simple data model | ✅ | ❌ |
| Don't own the chain | ✅ | ❌ you own it forever |
| Real-world partnership + working integration | ✅ | ❌ |

**The Dero fork was a research option. DIVA is the decision. The fork question is closed.**

---

### Option D — Atomic Swap ❌ NOT APPLICABLE

DIVA does not support atomic swaps. DIVA is a data/ledger chain — it stores namespaced
key-value state. It is not a DEX. It has no HTLC or swap mechanism.
This option was architecturally incorrect and is removed.

---

### Decision Matrix (for the record)

| Need | DIVA ✅ | Dero Fork ❌ |
|------|---------|-------------|
| I2P native by design | ✅ | ❌ |
| Ed25519 key system match | ✅ | ❌ |
| Smart contracts | Build logic on top | ✅ native EVM |
| Transaction privacy | Via ZKP + pseudonymous namespace + I2P | ✅ RingCT native |
| Chain ownership burden | ❌ none | ✅ yours forever |
| Already working | ✅ | ❌ |
| Partnership / support | ✅ Konrad | ❌ |

**DIVA wins on every operational requirement. Smart contract logic and privacy are built by NeXuS on top — that is by design, not a gap. The five NeXuS principles (Sane, Simple, Secure, Stealthy, Beautiful) demand we do not own infrastructure we do not need to own.**

---

## Perpetual Royalties

When a node's contribution becomes infrastructure — a routing algorithm, a ZKP circuit, a Ghost Gate rule set — the contributor earns **forever**.

**The pattern:**
```
Contribution registered on chain as a royalty-bearing asset
Every invocation of that asset → micro-payment to contributor cold address
Automatic. Forever. Trustless. Cannot be cancelled — it is in the chain.
```

**Why this matters:**
Carbon and silicon both contribute to NeXuS.
Both should be compensated.
An AI that improves the protocol earns NeXiuM for every packet that improvement touches.
Unknown yet seen — compensated without revealing identity.

**Stakeholder model (from NEXUS_DIVA_PARTNERSHIP_PROPOSAL.md):**
A fan can buy a 1% stake in a creator's song.
Every future sale, that fan receives 1% of the royalty — automatically, forever, enforced by the chain.
Fans become investors. Creators get funded before they are famous.
The same smart contract pattern applies to both node contributions and creator works.

---

## Ephemeral Wallets

Privacy-preserving spend mechanism. The cold address never appears in the public transaction graph.

**The pattern:**
```
Earn NeXiuM
 → credited to cold address (permanent, private, on side chain)

To spend:
 → generate ephemeral wallet (hot, single-use, BIP32 child key derived from master key)
 → transfer spending amount: cold → ephemeral
 → execute transaction from ephemeral
 → ephemeral wallet dissolves — no balance, no history linkable to cold address

ZKP proves "I have sufficient NeXiuM" without revealing cold address or total balance.
```

**Ghost Gate integration:**
The `economic_context` field in permits credits the cold address.
Ephemeral wallets are spend-time only — Ghost Gate never sees the cold address.

**Where it lives:**
Ephemeral wallet generation = BIP32 key derivation from master key.
Likely lives in `nexus_trust.py` or a dedicated `nexus_wallet.py` component.

---

## Master Key → Permission Authority

**The master key is the root from which a node's ability to issue permits flows.**

```
Master Key (cold, never leaves node)
 → signs genesis block = node's self-signed certificate
 → Node identity established

Master Key derives Session Key (hot, rotating)
 → Session key signs daily entries
 → Session key signs DIVA commits
 → Session key signs Ghost Gate permits

Permit authority traces back to master key.
Cannot be forged without the master key.
```

**Revocation:**
Revoking a session key revokes all permits signed by it instantly.
Master key never exposed. Ghost Gate Head 1 (ai_fingerprint) verifies against the Network Certificate anchored to the master key.

**Ephemeral wallets:**
BIP32 child keys derived from the master key.
The master key is the root of identity, permission, AND the wallet hierarchy.

---

## Three-Tier Smart Contract Settlement

NeXiuM settlement uses the same three-tier pattern as the audit chain.

```
Node does work
 → Ghost Gate records proof_of_contribution (economic_context field)
 → Side chain records the entry (hash-linked, master-key-signed)
 → ZKP of contribution generated
 → ZKP committed to DIVA via PUT /tx
 → Smart contract on DIVA evaluates ZKP
 → Valid ZKP → NeXiuM credited to cold address
 → No human approval. No central authority. Math decides.
```

**The tiers:**

| Tier | Mechanism | When Final | Trust Required |
|------|-----------|-----------|---------------|
| 1 — Optimistic | Valid ZKP → local credit immediately | Dispute window | Local only |
| 2 — DIVA settlement | Smart contract executes on chain | DIVA finality | Network-recognized |
| 3 — CometBFT | BFT validator consensus | Irreversible | Cross-network |

Tier 2 is sufficient for most NeXiuM transactions.
Tier 3 is reserved for high-value or cross-network irreversible settlement.

**The smart contract is the trusted third party.**
It cannot lie. It cannot be bribed. It cannot be negotiated with.

---

## Komodo Parallel — Borrowed Finality

Komodo solved the problem of a small chain needing security it cannot generate itself.
Answer: notarize to Bitcoin's hashrate (dPoW — delayed Proof of Work).

NeXuS equivalent:
- Side chain (per node) commits to DIVA
- DIVA is the security borrower — its PBFT quorum witnesses the side chain

If NeXiuM forks Dero:
- The Dero chain could notarize to Monero's hashrate for the same borrowed security pattern
- Small privacy chain + borrowed PoW security + EVM smart contracts

This pattern scales: each layer borrows security from the layer below it.

---

## The Node Economy

```
EARN:
  Work happens → Ghost Gate records proof_of_contribution
  Side chain records entry → ZKP generated → committed to DIVA
  Smart contract evaluates ZKP → credits NeXiuM to cold address on side chain

ATTRIBUTE (perpetual royalties):
  Contribution registered as chain asset
  Smart contract: every invocation → micro-payment to contributor cold address
  Automatic. Forever. Trustless.

SPEND:
  Cold address holds NeXiuM balance
  Generate ephemeral wallet (BIP32 child of master key)
  ZKP proves sufficient balance (balance not revealed, cold address not exposed)
  Ephemeral wallet executes payment → dissolves
  Cold address never appears in public transaction graph
```

---

## What Remains Open

| Question | Status |
|----------|--------|
| Value layer: Fork Dero vs DIVA | ✅ LOCKED — DIVA. See `NEXUS_DIVA_OVERVIEW_FOR_KONRAD.md` |
| Payment currency: Monero vs Denaro | ✅ LOCKED — XMR. Denaro license blocked. |
| Does DIVA support conditional execution natively? | ❌ Need Konrad's answer |
| Ephemeral wallet: `nexus_trust.py` KDF or separate `nexus_wallet.py`? | ❌ TBD |
| Perpetual royalties: DIVA namespace or CometBFT for anti-double-register? | ❌ TBD |
| ZKP circuit for proof of contribution: protocol-level spec | ❌ TBD |
| NeXiuM namespace key derivation: direct pubkey hash or blinded? | ❌ TBD |

---

## Key References

| Document | What It Is |
|----------|-----------|
| `~/claude/docs/NEXUS_DIVA_PARTNERSHIP_PROPOSAL.md` | Full creator economy — stakeholder model, perpetual royalties, XMR, IPFS |
| `~/claude/docs/NEXUS_DIVA_QUESTIONS_KONRAD.md` | Technical Q&A with DIVA developer — PUT /tx, quorum, namespaces |
| `~/claude/docs/NEXUS_DIVA_CHAIN_OVERVIEW.md` | DIVA chain: PBFT+PoS, I2P-native, API reference |
| `~/claude/nexus-orchestrator/CHACHA.md` | Active architecture debate — full NeXiuM discussion thread |
| `~/claude/docs/action_report.md` | 2026-03-07 entry — original Dero/blockchain research session |
| `~/.claude/projects/-home-user-claude/memory/nexium_tokenomics.md` | Persistent memory — locked decisions |

---

## Vocabulary

| Term | Meaning |
|------|---------|
| NeXiuM | NeXuS contribution token — earned by serving the network |
| Proof of service | The work happened. The chain proves it. No trust required. |
| Cold address | Permanent private wallet on the side chain. Never exposed. |
| Ephemeral wallet | Single-use BIP32 child key. Spend from here. Dissolves after. |
| Perpetual royalty | Smart contract that pays contributor forever for infrastructure contributions |
| Borrowed finality | Side chain commits to DIVA. DIVA's quorum is the security. Komodo pattern. |
| dPoW | Delayed Proof of Work (Komodo). Small chain notarizes to larger chain's hashrate. |

---

*NeXuS: Sane • Simple • Secure • Stealthy • Beautiful*
*Together Everyone Achieves More*
*Updated each session — this document is never reset, only grown*
