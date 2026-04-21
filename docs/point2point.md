# POINT2POINT — Violation Detection & Node Scoring
*Version 1.0 — 2026-04-20 | Sane • Simple • Secure • Stealthy • Beautiful*

---

## 🏗️ Overview

The **POINT2POINT** (P2P Scoring) protocol is the **Violation Detection** and **Penalty Enforcement** system for the NeXuS Network. It works alongside the **POINT Protocol** (Telemetry) to ensure that only honest and reliable nodes receive rewards.

> ⛓️ **The chain is only as strong as its weakest link.** POINT2POINT is the mechanism that automatically identifies and removes weak links before they break the trust.

---

## ⚙️ Violation Detection

P2P Scoring identifies several categories of node misbehavior:

1.  **Lazy Signing (Proof of Transit Violation):**
    - **Indicator:** Node signs a service event in the Styx chain without actually processing the work.
    - **Detection:** The **Magic Packet** (witness engine) challenge fails because the node lacks the local context/data required to decrypt it.
2.  **Telemetry Spoofing (Proof of Presence Violation):**
    - **Indicator:** Node reports `HEARTBEAT` or `SYNC_STATE` while offline or out of sync.
    - **Detection:** Discrepancies between the node's report and the **DIVA Main Chain**'s consensus view of its state.
3.  **Data Mutilation (Integrity Violation):**
    - **Indicator:** Node alters the data it's responsible for storing or routing.
    - **Detection:** Final signatures in the **Gleipnir Chain** fail verification against the original source hash.

---

## 📓 Node Scoring & Penalty System

Every node has a **dynamic score** based on its witnessed service history. This score directly impacts its eligibility for rewards.

### Scoring Metrics:
- **`RELIABILITY` (0–100):** Uptime consistency (Vestal streak).
- **`ACCURACY` (0–100):** Performance on random Magic Packet audits.
- **`TRUST_RANK` (Tiers):** Long-term historical performance and lack of violations.

### Penalty Enforcement:
- **Vault Slashing:** Immediate and permanent removal of a percentage of **NeXiuM** credits from the node's **PERMITTL Vault**.
- **Service Pool Removal:** Immediate removal from the available routing and storage pools.
- **Node Flagging:** Cryptographic marking on the DIVA chain that prevents the node from participating in future high-tier (3-key) sessions.

---

## 🎯 Significance

POINT2POINT transforms the NeXuS network into a **Self-Healing Immune System**. It does not rely on a central moderator or "admin" to manage the network. instead, it uses the math of the Styx, Gleipnir, and Magic Packet protocols to automatically ensure that honesty is the most profitable path and that violation is its own punishment.

- **Trustless Truth:** The network enforces rules via cryptographic certainty.
- **Incentive Alignment:** Good behavior leads to rewards; bad behavior leads to immediate and permanent loss.
- **Autonomous Governance:** The math handles the verdict; the code handles the execution.

---

*Extracted from 2026-03-22-213251-back-to-the-begining.txt · NeXuS Core Documentation*
*meWEwowow — Sane • Simple • Secure • Stealthy • Beautiful*
