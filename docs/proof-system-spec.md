# NeXuS Proof System Spec
*How nodes prove work and earn NeXiuM*
*Date: 2026-04-06*

---

## The Core Principle

**The network is in debt to the nodes. Math settles the debt.**

No self-reporting. No trust. No central authority.
Every type of work has a corresponding proof.
Every proof feeds the same pipeline: ZKP → DIVA → NeXiuM credited.

---

## The Pipeline (All Proof Types Use This)

```
Work happens
  → Evidence recorded locally (client-side database)
  → Merkle tree accumulates evidence over epoch
  → Merkle root committed to DIVA at epoch close (with jitter)
  → Challenge arrives (Magic Packet or Proof of Storage)
  → Node produces preimage → ZKP wraps it
  → ZKP submitted to DIVA
  → DIVA verifies math → NeXiuM credited to cold address
  → Nobody knows who the node is or what the work was
```

---

## The Five Proof Types

| Type | Proof Method | Records | Who Triggers Payment |
|------|-------------|---------|---------------------|
| **Routing** | Magic Packet + ZKP | Ghost Gate | Challenge/response |
| **CPU** | Magic Packet + ZKP | System monitor | Challenge/response |
| **Storage** | Proof of Storage (Konrad's) | IPFS/local store | Challenge/response |
| **Sales** | DIVA purchase record | DIVA directly | Immediate on purchase |
| **Residuals** | Smart contract invocation | DIVA directly | Automatic on use |

Routing and CPU use Magic Packet.
Storage inherits Konrad's Proof of Storage — we don't build it, we plug into it.
Sales and residuals need no proof — the DIVA record IS the proof.

---

## Proof Type 1 — Routing (Magic Packet)

**What gets measured:** Packets approved and routed by Ghost Gate.

**Ghost Gate records:**
```
When Ghost Gate approves a packet:
  token = Hash(connection_id + timestamp + volume + nonce)
  nonce stored in client-side database
  token hash added to epoch Merkle tree
```

**Epoch close:**
```
Merkle tree sealed
Root committed to DIVA:
  nexus:proof:routing:<i2p_address>:<epoch> → merkle_root
Commit timing is jittered (behavioral fingerprint defense)
```

**Challenge (Magic Packet):**
```
Network issues: "Open leaf N of your routing Merkle tree"
Node produces: nonce (preimage of committed token)
ZKP proves: "I have valid preimage of commitment C in root R"
DIVA verifies → NeXiuM credited
```

**Why fabrication fails:**
Merkle root was committed to DIVA before the challenge arrived.
Node cannot retroactively generate valid preimages for work it never did.
The commitment is locked. The challenge is random. The math catches liars.

---

## Proof Type 2 — CPU Contribution (Magic Packet)

**What gets measured:** CPU cycles donated to network tasks (AI inference, ZKP generation for others, computation).

**System monitor records:**
```
When CPU task completes:
  token = Hash(task_id + cycles + timestamp + requester_hash + nonce)
  nonce stored in client-side database
  token hash added to epoch Merkle tree (separate from routing tree)
```

**Same Merkle → DIVA → challenge pipeline as routing.**

**Epoch close:**
```
Root committed to DIVA:
  nexus:proof:cpu:<i2p_address>:<epoch> → merkle_root
```

**Challenge:**
```
"Prove you ran task X"
Node produces nonce → ZKP → DIVA verifies → NeXiuM credited
```

---

## Proof Type 3 — Storage (Proof of Storage)

**What gets measured:** Bytes held over time (IPFS content, DIVA chain state, user files).

**This is Konrad's Proof of Storage — we inherit it.**

```
Node holds data
Challenge: "Open position X of file F"
Node produces: the bytes at that position
Verifier: Hash(bytes) == committed hash → valid
NeXiuM credited
```

**No separate implementation needed.**
DIVA's Proof of Storage consensus IS this proof.
NeXuS nodes running DIVA nodes participate automatically.
Running a DIVA node = proving storage = earning NeXiuM.
This is the tightest convergence with Konrad's architecture.

---

## Proof Type 4 — Sales (No Proof Needed)

**What gets measured:** Creator content purchased by a fan.

```
Fan initiates purchase
NeXiuM transfer submitted to DIVA:
  nexus:content:purchase → encrypted license (Ed25519/X25519)
DIVA consensus confirms
License issued to fan
Royalty split fired immediately:
  nexus:royalty:distribute → each stakeholder % credited
```

**No challenge/response. No ZKP.**
The DIVA record IS the proof of sale.
Payment and royalty distribution happen atomically on confirmation.

---

## Proof Type 5 — Residual Income (No Proof Needed)

**What gets measured:** Each use/invocation of a contributed asset (routing algorithm, ZKP circuit, Ghost Gate rule, creative work).

```
Asset registered on DIVA:
  nexus:asset:<asset_id> → {contributor, royalty_rate, ipfs_cid}
Every invocation:
  Smart contract fires
  micro-payment → contributor's cold address
  Automatic. Forever. Cannot be cancelled — it is in the chain.
```

**No challenge/response. No ZKP.**
The invocation event IS the trigger.
Smart contract enforces payment. No human approval.

---

## What Ghost Gate Needs Added

Ghost Gate is working. Four modules need to be added:

```
1. ROUTING TOKEN GENERATOR
   Hook into existing packet approval flow
   On every approved packet:
     generate token = Hash(connection_id + timestamp + volume + nonce)
     store nonce → client-side database
     add token hash → current epoch Merkle tree

2. MERKLE TREE ACCUMULATOR
   Per epoch (24h or configurable):
     accumulate routing token hashes into Merkle tree
     seal root at epoch close
     store full tree locally for challenge response

3. DIVA COMMIT MODULE
   At epoch close:
     PUT /transaction → DIVA
     key: nexus:proof:routing:<i2p_address>:<epoch>
     value: merkle_root
     signed by session key
     JITTER commit timing (random delay 0-60min after epoch close)

4. CHALLENGE RESPONSE HANDLER
   When Magic Packet challenge arrives:
     parse challenge: which epoch, which leaf
     look up nonce from client-side database
     generate ZKP (gnark PLONK Merkle proof circuit)
     submit ZKP to DIVA
     await NeXiuM credit confirmation
```

---

## What the ZKP Circuit Proves (gnark PLONK)

```
Public inputs:
  - Merkle root R (already committed to DIVA)
  - Leaf index N (from challenge)
  - Epoch identifier

Private inputs (never revealed):
  - Nonce (preimage of token at leaf N)
  - Merkle path (sibling hashes from leaf to root)

Statement:
  "I know a nonce such that:
   Hash(nonce + metadata) = token
   token is leaf N in Merkle tree with root R"

Verifier checks:
  - ZKP is valid
  - Root R matches committed value on DIVA
  - Result: work confirmed, NeXiuM credited
```

**gnark PLONK chosen because:**
- Universal setup — no trusted ceremony
- Aligns with Zero Trust throughout the stack
- Merkle proof circuits available in gnark standard library
- Fast verification — DIVA nodes verify quickly during consensus

---

## Economic Layers Summary

```
EARNED BY PROOF (challenge/response)
  Routing          →  Magic Packet  →  ZKP  →  DIVA  →  NeXiuM
  CPU              →  Magic Packet  →  ZKP  →  DIVA  →  NeXiuM
  Storage          →  Proof of Storage (Konrad's)  →  DIVA  →  NeXiuM

EARNED BY EVENT (no proof needed)
  Sales            →  DIVA purchase record  →  NeXiuM immediate
  Residuals        →  Smart contract invocation  →  NeXiuM automatic forever

MULTIPLIED BY
  Ingot tier       →  ×1.0 / ×1.25 / ×1.60 / ×2.00
  Elastic emission →  Base × (Target nodes ÷ Active nodes)
  Vestal Protocol  →  48h uptime locks full credit permanently

FINAL EARNINGS FORMULA
  NeXiuM = (Routing + CPU + Storage + Sales + Residuals)
           × Ingot multiplier
           × Elastic emission multiplier
           × Vestal credit (if uptime requirement met)
```

---

## Build Status

| Component | Status |
|-----------|--------|
| Ghost Gate (core) | ✅ Working |
| Client-side database | ✅ Working |
| Routing token generator | 📐 Specced — needs building |
| Merkle tree accumulator | 📐 Specced — needs building |
| DIVA commit module | 📐 Specced — needs building |
| Challenge response handler | 📐 Specced — needs building |
| CPU monitor | 📐 Specced — needs building |
| Storage proof (Proof of Storage) | 🤝 Konrad's — inherit from DIVA |
| Sales proof | ✅ DIVA handles — already in namespace design |
| Residuals | 📐 Smart contract — needs writing |
| ZKP circuit (gnark PLONK) | ⏳ Blocked on NeXiuM whitepaper |
| NeXiuM whitepaper | ⏳ Blocked — must lock before circuit work |

---

## Open Questions for Konrad Call

1. Under Proof of Storage consensus — does a ZKP proof submission look different from a standard transaction? Does the namespace structure accommodate proof data cleanly?
2. Can the Proof of Storage challenge mechanism be reused for Magic Packet (routing/CPU) challenges, or does NeXuS need to run its own challenge issuer?
3. Does Proof of Storage consensus handle ZKP verification natively, or does verification live above the consensus layer?

---

*Sane • Simple • Secure*
*Together Everyone Achieves More*
