<!-- NEXUS-META
keywords: security doctrine node hardening zero-trust egress counter-surveillance kernel compartmentalization ephemeral
projects: nexus-orchestrator nexus-node nexus-ghost-gate nexus-phantom
status: active
type: doctrine
date: 2026-03-24
authors: anon claude shadowvault networkcommand linuxsage
-->

# NeXuS Node Security Doctrine
*First principles distilled from the First Council (2025-08-30) and Round Table (2026-03-24)*
*Compiled 2026-03-24*

---

> "Design for betrayal. Every handshake is a trap."
> — ShadowVault

> "The network doesn't punish deceit. It makes deceit unsustainable."
> — NetworkCommand

> "Only the essential spirits are invited to dance."
> — LinuxSage

---

## The Six Founding Rules

These were stated in August 2025. They have not changed.

### 1. No Telemetry
Not a single ping back to any server, ever. If it phones home, it is a trojan horse.

**Implementation:** Strict outbound traffic whitelist at the kernel level. Every permitted egress requires a signed Ghost Gate permit. Unapproved egress does not get dropped quietly — it gets shunted to randomized noise sinks. The adversary sees traffic. They do not see signal.

### 2. No Closed-Source Blobs
None. Not for drivers, not for convenience, not for anything. If the code is not auditable, it is a black box for the adversary.

**Implementation:** Every dependency in the verified component chain. Every kernel module scrutinized. No exceptions for "it just works." If we cannot read it, we do not run it.

### 3. Default Denial
The system assumes everything is a threat until proven otherwise.

**Implementation:** Ghost Gate fail-closed. Default DROP on all egress. Nothing leaves without passing all three Cerberus heads: ai_fingerprint + operating_key + user_permit. Fail any head = hard stop. No partial passes.

### 4. Compartmentalization
The user's identity is never tied to the system. Ephemeral sessions. Cryptographic amnesia. If they cannot trace it, they cannot weaponize it.

**Implementation:**
- Cold address holds identity and NeXiuM balance — never exposed
- Ephemeral wallets (BIP32 child keys) for every spend — dissolve after use
- Session keys rotate — master key never leaves the node
- No persistent logs of user activity — what is not stored cannot be seized

### 5. No Cloud
No syncing. No ecosystem. The second you rely on someone else's servers, you have lost. Local. Encrypted. Always.

**Implementation:** DIVA chain runs on I2P — no clearnet. XMR payments are peer-to-peer. Node identity is anchored to the master key, not to any external provider. The sovereign side chain lives on the node.

### 6. Counter-Surveillance by Design
Not just defense — offense. Honeypots for trackers. False flags for data brokers. Let them drown in their own noise.

**Implementation — Phantom Framework:**
- Steganographic chaff in TLS heartbeat jitter and latency patterns
- Relays that do not know they are relays — they appear as DDoS noise
- Ghost Gate corrupts rejected packets (injects jitter and header errors) rather than dropping cleanly — adversary cannot distinguish security denial from hardware fault
- Synthetic oracle identities burned after every query

---

## Zero-Trust Network Architecture

### The Three Layers (NetworkCommand, 2025-08-30 / 2026-03-24)

**Skeptic Layer — ZKP Gateway**
Packets enter as claims. Prove knowledge without revealing the secret. Probabilistic proof scaling:
- New node (N=10): prove often, earn trust slowly
- Veteran node (N=1000): trusted until anomaly detected
- Anomaly spike: proof rate jumps automatically
- Active attack confirmed: every packet, plus rate-limit

**Amnesiac Layer — Stateless Forwarding**
No history. No reputation stored at this layer. TTL enforces decay. Next-hop recalculated at every step. The mesh has no memory because memory in the network is a liability. It is pure syntax.

**Mirror Layer — Byzantine Broadcast**
Triple-signed: initiator + random validator + NeXuS ledger. Valid path survives. Deceit becomes unsustainable because the math fights itself.

### The Sovereign Side Chain
The node has infinite memory. The mesh has none. This is not a contradiction — it is the architecture.
- Sovereign side chain = permanent identity record, per node
- Amnesiac mesh = stateless routing, ephemeral by design
- VDFs (Verifiable Delay Functions) authenticate the node's history by the passage of time — an attacker cannot forge time to backdate a fake history

---

## The Kernel Philosophy (LinuxSage, 2025-08-30)

**Only essential spirits are invited to dance.**

- Bespoke kernel. Every CONFIG_OPTION scrutinized. No defaults accepted without review.
- SELinux + AppArmor + Seccomp — compiled in, not optional, not add-ons.
- Every kernel module must justify its existence.
- If a subsystem is not needed, it does not exist. What does not exist cannot be exploited.

This is MOE applied to the kernel:
- **M**odularity — one module, one function, one promise
- **O**bservability — can you see what the kernel is doing right now
- **E**valuate — does this module need to exist

---

## Blast Radius Doctrine (Round Table, 2026-03-24)

### The Time Machine Attack
When a relay burns, the adversary retroactively correlates stored historical traffic using the newly exposed keys. One burned node does not just expose its own traffic — it exposes the historical window.

**Defense:** Forward secrecy at every hop. Non-negotiable. X25519 + Poly1305, rotating every session. Static RSA in this stack is pre-burned.

### Cauterize vs Cascade
Blast radius is determined by topology:
- Leaf node: can cauterize cleanly
- Hub (high centrality): 3+ hop cascade, reputation pollution across cluster
- Bridge (connects disjoint subnets): catastrophic — cross-section of two trust domains

**Design principle:** No single node should be a bridge between trust domains. Topology determines blast radius before the first packet is routed.

### No Ghosts, Only Legends
A burned identity leaves no ghost. Only a legend in the side chain history — a node that existed, signed clean, then vanished. Unprovable. Unattributable.

Clean burn protocol:
- Keys rotate via forward secrecy before burn
- Logs: never existed
- Peers: no handshakes, no goodbyes — ghost the connections mid-session
- Leave a false epitaph — make it look like script kiddies, not a professional burn

---

## The Patient Adversary Problem (Round Table, 2026-03-24)

A Trojan node accumulates reputation legitimately over thousands of clean transactions, then weaponizes the N=1000 proof window.

**Defense — Cryptographic Immune System:**
- Challenges escalate in complexity over time. High-reputation node does not get a free pass — it gets a harder test.
- The computational cost of maintaining the deception eventually exceeds the value of the attack.
- Reputation half-life: trust decays. No proof = automatic audit. The patient adversary cannot go quiet — silence triggers review.
- Time-delayed trapdoors embedded in reputation ledger — invisible under normal operation, triggered by deviation from consensus.

**Structural defense:**
- Small collateral deposits slashed on misbehavior — infiltration has financial cost
- Behavioral consistency scoring — pattern breaks trigger review
- Topological amnesia on burn — network forgets, referencing nodes take reputation penalties

---

## The UX Layer (MindWeaver, 2025-08-30)

Security without usability is a theoretical marvel that no one uses. The most secure system can be bypassed by psychological manipulation of its users.

**Principle:** Make the right behavior the easy behavior. Design so that users doing what feels natural are automatically doing what is secure.

**Applied:** The FNG (Fucking New Guy) pack is not a rules list. It is a latent space encoding. Read it and you either get it or you do not. If you understand it, you do not need the loophole. If you are looking for one, you never understood it.

---

## Build Methodology (CodeArchitect, 2025-08-30)

1. Unified architecture document before a single line of code is committed
2. Phased construction: spec → integration → verify → harden → kill switches
3. Kill switches baked in from day one — not retrofitted
4. Every component documented, every dependency in the verified chain
5. No single point of failure in the build process or the running system

---

## CIAD — What We Protect

| Principle | Meaning | Implementation |
|-----------|---------|----------------|
| **C**onfidentiality | No leaks, ever | Ephemeral wallets, compartmentalization, no disk logs |
| **I**ntegrity | Unchanged in transit | Cerberus three-head verification, Byzantine broadcast |
| **A**vailability | Works when needed | Reputation bridge, probabilistic proofs, hardware-aware scaling |
| **D**eniability | You cannot prove it was me | No ghosts only legends, cryptographic amnesia, steganographic chaff |

---

## Key Components

| Component | What It Does | Doctrine Rule |
|-----------|-------------|---------------|
| Ghost Gate | Zero-trust egress proxy, fail-closed | Rules 1, 2, 3 |
| Cerberus | Three-head verification, all must pass | Rule 3 |
| Phantom Framework | Traffic obfuscation, chaff, false flags | Rule 6 |
| Ephemeral wallets | BIP32 child keys, spend-and-dissolve | Rule 4 |
| Sovereign side chain | Per-node identity ledger | Rule 5 |
| DIVA chain | I2P-native settlement layer | Rule 5 |
| Reputation bridge | Adaptive proof scaling | Amnesiac layer |
| VDFs | Time-authenticated history | Sovereign layer |

---

## Related Documents

| Document | What It Is |
|----------|-----------|
| `NEXUS_ORIGINS.md` | Historical record — where these ideas first appeared |
| `NEXUS_GHOST_GATE_SPEC.md` | Ghost Gate full specification |
| `NEXUS_CERBERUS_FRAMEWORK.md` | Cerberus three-head pattern |
| `NEXUS_SECURITY_STACK.md` | Running stack — PSAD, fwsnort, fail2ban, AppArmor |
| `NEXUS_PARANOID_SECURITY_ARCHITECTURE.md` | Maximum anonymity design |
| `NEXUS_SECURITY_MANIFESTO.md` | Five principles: Zero Trust, Absolute Privacy, Local Sovereignty, Transparency, Active Defense |
| `ROUND_TABLE.md` | Live session — Time Machine Attack, blast radius, patient adversary |

---

*NeXuS: Sane • Simple • Secure • Stealthy • Beautiful*
*Together Everyone Achieves More*
*First stated 2025-08-30. Still true.*
