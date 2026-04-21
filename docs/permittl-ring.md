# 🗝️ PERMITTL RING — Sovereign Trust Anchor Specification
*Version 1.0 — 2026-04-01 | Sane • Simple • Secure • Stealthy • Beautiful*

<!-- NEXUS-META
id:           NXS-SPEC-PERMITTL-0001
type:         SPEC
principles:   Sane Simple Secure Stealthy
project:      nexus-eco
layer:        identity
version:      1.0
status:       active
score-composite: 20
score-tier:    EXEMPLARY
-->

---

> 💡 **The Core Thesis:** The PERMITTL RING is the offline, cold trust anchor of a NeXuS node. It is an active logic engine that generates task-specific, time-limited keys, ensuring that the node's global identity remains hidden while its economic contributions are verified.

---

## 🏗️ 1. The Ring Hierarchy

The system operates on a three-tier isolation model to prevent total compromise.

| Ring | State | Purpose | Storage |
| :--- | :--- | :--- | :--- |
| **Ring 03 (Cold)** | **OFFLINE** | The PERMITTL Master. Key schedule, master entropy, and protocol rules. | USB + hardware TPM (JANUS Protocol) |
| **Ring 02 (Semi-Cold)**| **PASSIVE** | The Vault (**PLUTUS**). Receives credits. Only online to sweep or trade. | Local Encrypted Drive |
| **Ring 01 (Hot)** | **ACTIVE** | The Service Node. Signs packets, participates in multicasts, handles traffic. | RAM / Ephemeral |

---

## 🔑 2. Key Derivation Logic

Keys are not just random strings; they are **Permission-Scoped** and **TTL-Bound**.

### The Derivation Formula:
`Derived_Key = HMAC-SHA256(Master_Entropy, Permission_Scope || TTL_Window || Sequence)`

### Active Scopes:
- **`routing`**: Authorized to sign Gleipnir transit blocks.
- **`storage`**: Authorized to sign mPoS storage proofs.
- **`vault`**: Authorized to move NeXiuM from Ring 02.
- **`governance`**: Authorized to vote at the Round Table.
- **`multicast`**: Authorized to decrypt HOTP block participation codes.

---

## 🔄 3. Lifecycle & Continuity (The JANUS Protocol)

To ensure sovereignty without central authorities, the Ring utilizes a pre-committed hash chain for its lifecycle.

### A. Silent Rotation (The R0...Rn Chain)
At birth, the Ring generates a chain of hashes:
`R(n) = hash(R(n+1) + nonce)`
- **R0 (Anchor):** Submitted to the DIVA chain at registration.
- **R1...Rn:** Stored offline.
- **Rotation:** To rotate, reveal `R1`. The network verifies `hash(R1) == R0`. Identity is preserved; math is updated.

### B. The Poison Pill (Revocation)
A one-time emergency broadcast:
- **Protocol:** `sign(R0, "REVOKE", timestamp)`
- **Effect:** Immediately invalidates all keys derived from the current Ring. The Vault is **FROZEN** pending recovery.

### C. The Recovery Protocol
- **Logic:** The owner uses the next hash in the chain (`R1`) to prove they are the legitimate successor to the frozen Ring (`R0`).
- **Claim:** Math proves continuity; assets are migrated to the new Ring anchor.

---

## 🎲 4. The Word of Truth (Styx Truth Tokens)

To prevent "Lazy Signing," the Ring generates **Prime Truth Tokens** for every service hop.
- **Mechanism:** `token = next_prime( HMAC-SHA256(node_secret, contract_id || sequence) )`
- **Fact:** A node cannot produce the correct prime token without having the specific work context. The prime is burned on use.

---

## 🛡️ 5. Security Mandates
1.  **Hardware Binding:** The Ring must be cryptographically bound to the node's hardware fingerprint (CPU/Serial) and a physical USB key.
2.  **No Persistence:** Ring 01 keys must reside in **RAM ONLY** and be wiped upon power-off or SIGTERM.
3.  **Isolation:** A compromise of a `routing` key must provide zero path to the `vault` or `governance` keys.

---

> **NeXuS: Together Everyone Achieves More**
> *mewe — sovereign me inside a we*
> *The Ring never leaves the cave. The work never leaves the law.*
