# ⚒️ NXS-FORGE — The Universal Identity & Trust Protocol
*Version 1.1 — 2026-04-01 | Sane • Simple • Secure • Stealthy • Beautiful*

<!-- NEXUS-META
id:           NXS-FORGE-0001
type:         PROTOCOL
principles:   Sane Simple Secure
cia:          Integrity Accountability Transparency
moe:          Modularity Observability Extensibility
pop:          Open-Source
ymca:         Yes-We-Can
zero-trust:   true
sovereignty:  local
transparency: full
defense:      active
created:      2026-04-01
authors:      claude anon gemini
version:      1.1
status:       active
audience:     all
tier:         all
score-sane:       5
score-simple:     5
score-secure:     5
score-composite:  15
score-tier:       ACTIVE
score-level:      2
scored-by:        gemini
scored-at:        2026-04-01
nexium-reward:    112
-->

---

> ⚒️ **The Forge** is where raw material receives its identity.
> Every artifact that enters the NeXuS ecosystem is stamped here —
> measured against its declared principles, scored by the community,
> and trusted only when the math says so.
>
> *A stamp without a score is a label. A score makes it a trust mechanism.*

---

## 🧠 The Core Idea

The NeXuS ecosystem is made of artifacts. Documents. Scripts. Containers. Services. Permits. Nodes. Characters. Config files. Code modules.

Until now each type lived in its own world with its own rules. The Forge unifies them under one question:

> **"Does this artifact do what it declares, at the standard it claims, for the network it serves?"**

The META block is the declaration. The scoring system is the proof. The trust tier is the verdict.

Nothing deploys, ships, or earns without passing through the Forge.

---

## 🏷️ The META Block — Universal Artifact Identity

Every NeXuS artifact carries a META block. Not just documents — scripts, containers, services, permits, nodes, characters. Everything.

The META block has two categories of fields: **Constants** (defined at creation, never change) and **Variables** (evolve through the artifact's lifecycle).

---

### 🔒 Constants — What the Artifact IS

```
<!-- NEXUS-META
id:           NXS-DOC-0042          ← unique permanent identifier
type:         PROTOCOL              ← artifact type (see Type Registry below)
principles:   Sane Secure Stealthy  ← active denominator profile
cia:          Confidentiality Deniability Accountability
moe:          Modularity
pop:          Privacy Open-Source
ymca:         CLI-First
zero-trust:   true                  ← boolean
sovereignty:  local                 ← local | mesh | hybrid
transparency: full                  ← full | partial | internal
defense:      active                ← active | passive | none
created:      2026-04-01
authors:      gemini claude anon
project:      nexus-ghost-gate
layer:        3                     ← stack layer (0-6) or: infra | eco | social
-->
```

---

### 🔄 Variables — Current STATE of the Artifact

```
version:          1.2
updated:          2026-04-01
status:           draft             ← draft | review | active | deprecated | archived
audience:         developer         ← node-runner | developer | community | all
tier:             phantom           ← phantom | vanguard | hub | broadcast | all
score-sane:       4
score-simple:     0
score-secure:     5
score-stealthy:   4
score-composite:  13
score-tier:       ACTIVE            ← DRAFT | REVIEW | ACTIVE | EXEMPLARY
score-level:      2                 ← 0=unscored | 1=self | 2=peer | 3=consensus
scored-by:        anon
scored-at:        2026-04-01
nexium-reward:    39
-->
```

---

## 🧬 The Constitutional DNA — Principle Sources

The Forge draws from three founding documents. Every artifact declares which elements of each apply.

---

### Source 1: The Five Principles
*Sane • Simple • Secure • Stealthy • Beautiful*

The core denominators. Every artifact selects the subset that applies to its type and purpose.

| Principle | The Test |
|-----------|----------|
| 🧠 **Sane** | Does it make sense? No internal contradictions. A smart person reads it and says *yes, that tracks.* |
| ⚡ **Simple** | One concept per unit. No jargon without definition. A motivated non-expert can follow it. |
| 🔒 **Secure** | Passes all declared security gates. Does not reveal what it shouldn't. Describes defense, never bypass. |
| 👻 **Stealthy** | No fingerprinting. No identifying information. Exists as function, not as evidence. |
| ✨ **Beautiful** | Diagrams. Color callouts. Narrative voice. Something you want to engage with, not have to. |

---

### Source 2: The Manifesto — CIA • MOE • POP • YMCA

From `~/manuals/NEXUS_MANIFESTO.md`:

**CIA — The Trust Triad + The Secret Sauce**
| Code | Principle | What It Means for an Artifact |
|------|-----------|-------------------------------|
| `C` | Confidentiality | All sensitive data encrypted. No plaintext exposure. |
| `I` | Integrity | Cryptographic hashing. Signatures verify all components. |
| `A` | Accountability | Every action logged. Full audit trail. |
| `D` | Deniability | Anonymous routing. Metadata scrubbed. No traceable identity. |

**MOE — The Architecture Triad**
| Code | Principle | What It Means for an Artifact |
|------|-----------|-------------------------------|
| `M` | Modularity | Operates independently. Pluggable. No monolithic dependencies. |
| `O` | Observability | Health metrics exposed. Status readable. Debug-capable. |
| `E` | Extensibility | Open architecture. New providers/inputs integrate cleanly. |

**POP — The Freedom Triad**
| Code | Principle | What It Means for an Artifact |
|------|-----------|-------------------------------|
| `P` | Privacy | Default anonymization. Local processing. Data minimization. |
| `O` | Open-Source | 100% auditable. No binary blobs. No vendor lock-in. |
| `P` | Performance | Runs on 2011 hardware. Efficient. Non-blocking. |

**YMCA — The Human Triad**
| Code | Principle | What It Means for an Artifact |
|------|-----------|-------------------------------|
| `Y` | Yes-We-Can | Accessible to non-experts. Clear feedback. Help integrated. |
| `M` | Multi-AI Harmony | AI-to-AI compatible. OAAE framework aware. |
| `C` | CLI-First | Terminal native. Keyboard driven. Tmux compatible. |
| `A` | AI-Ghost | Intelligent automation capable. Self-healing where applicable. |

---

### Source 3: The Security Manifesto — The Five Gates

From `~/manuals/NEXUS_SECURITY_MANIFESTO.md`:

| Gate | Principle | Non-Negotiable Rule |
|------|-----------|---------------------|
| 🛡️ **Zero-Trust** | Default denial. Assume breach. | Nothing is trusted without cryptographic proof. |
| 🔇 **Absolute-Privacy** | No telemetry. Ever. | Not a single unsolicited ping. Cryptographic amnesia. |
| 🏠 **Local-Sovereignty** | No cloud. No sync. | If it can't run in a blackout, it's not truly free. |
| 👁️ **Transparency** | No closed-source blobs. | If it's not auditable, it's a black box for *them*. |
| ⚔️ **Active-Defense** | Counter-surveillance by design. | Honeypots. Kill switches. Hardened until it bleeds. |

---

## 📋 Artifact Type Registry

Every artifact declares its type. The type determines:
- Which principles are **mandatory** vs optional
- The **NeXiuM weight multiplier** for scoring
- The **minimum composite score** required to ship

| Type | Mandatory Principles | NeXiuM Weight | Min Composite |
|------|---------------------|:-------------:|:-------------:|
| `NODE` | Sane Secure Stealthy | 3.0 | 12 |
| `PROTOCOL` | Sane Secure | 2.5 | 12 |
| `SERVICE` | Sane Secure | 2.0 | 9 |
| `CONTAINER` | Secure Stealthy | 2.0 | 9 |
| `SCRIPT` | Sane Simple | 1.5 | 9 |
| `PERMIT` | Sane Secure | 2.0 | 12 |
| `DOC` | Sane Simple | 1.0 | 9 |
| `CHARACTER` | Simple Beautiful | 1.0 | 6 |
| `CONFIG` | Sane Secure | 1.5 | 9 |
| `SPEC` | Sane Secure Simple | 2.0 | 12 |
| `MANUAL` | Sane Simple Beautiful | 1.5 | 9 |
| `VISION` | Sane Simple Beautiful | 1.0 | 9 |
| `NARRATIVE` | Simple Beautiful | 1.0 | 6 |
| `REFERENCE` | Sane Simple Secure | 1.5 | 9 |

---

## 🎯 Principle Profiles — Active Denominator Selection

Not all five principles apply equally to every artifact. The Forge selects the **active profile** based on type and purpose. Only declared principles are scored. Only active principles must pass.

### Default Profiles by Type

```
NODE        → Sane + Secure + Stealthy
PROTOCOL    → Sane + Secure + Stealthy
SERVICE     → Sane + Secure
CONTAINER   → Secure + Stealthy
SCRIPT      → Sane + Simple + Secure
PERMIT      → Sane + Secure
SPEC        → Sane + Secure + Simple
MANUAL      → Sane + Simple + Beautiful
VISION      → Sane + Simple + Beautiful
NARRATIVE   → Simple + Stealthy + Beautiful
REFERENCE   → Sane + Simple + Secure
CHARACTER   → Simple + Beautiful
CONFIG      → Sane + Secure
```

> 💡 Profiles are defaults. An artifact may declare additional principles beyond its type default.
> It may NOT remove mandatory principles for its type.

---

## ⚖️ The Scoring System

### The Scale — 0 to 5 Per Active Principle

| Score | State | Badge | Meaning |
|:-----:|-------|:-----:|---------|
| `0` | Unscored | ⚫ | Declared but never reviewed |
| `1` | Failing | 🔴 | Active violation of the principle |
| `2` | Weak | 🟠 | Intention present, execution missing |
| `3` | Viable | 🟡 | Minimum standard met — shippable |
| `4` | Strong | 🟢 | Principle fully embodied |
| `5` | Exemplary | 🔵 | Raises the bar for everything else |

#### 🧠 Scoring: SANE
*Does the artifact make sense and track logically?*

- **5 — Exemplary:** Zero logical friction. Architecture is self-evident. Documentation and code are perfectly synchronized.
- **4 — Strong:** Logical and well-structured. No internal contradictions. A peer can explain the "why" after one read.
- **3 — Viable:** Follows the standard. No major "WTF" moments. Does what it says on the tin.
- **2 — Weak:** Inconsistent logic or confusing architecture. "Magic" numbers or undocumented side effects present.
- **1 — Failing:** Contradictory. Breaks core NeXuS principles (e.g., depends on cloud while claiming local).

#### ⚡ Scoring: SIMPLE
*Is the artifact modular and free of unnecessary complexity?*

- **5 — Exemplary:** Elegant. Minimalist. One clear purpose. ZERO jargon without immediate, intuitive context.
- **4 — Strong:** Modular design. Clean separation of concerns. Easy to explain to a non-expert.
- **3 — Viable:** Functional and non-monolithic. Standard complexity for the task.
- **2 — Weak:** Over-engineered. Tight coupling between unrelated components. Hard for a motivated learner to follow.
- **1 — Failing:** Monolithic "Spaghetti" architecture. Unnecessary dependencies. Obfuscated intent.

#### 🔒 Scoring: SECURE
*Does the artifact pass the Five Gates and protect the node?*

- **5 — Exemplary:** Proactive defense. Zero-trust by design. No data leaks. All inputs sanitized. Formal verification present.
- **4 — Strong:** Secure by default. Hardened configuration. No hardcoded secrets. Clear failure states (fails closed).
- **3 — Viable:** Meets the minimum security standard. No known vulnerabilities. Basic sanitization and encryption active.
- **2 — Weak:** Information leaks (verbose errors). Hardcoded paths. Weak or non-standard crypto choices.
- **1 — Failing:** Clear vulnerability. Exposes secrets in logs. Fails open. Bypasses Ghost Gate.

#### 👻 Scoring: STEALTHY
*Is the artifact invisible to external observers?*

- **5 — Exemplary:** Zero fingerprint. No identifying metadata. Runs in RAM only. No persistence without explicit intent.
- **4 — Strong:** Scours metadata before processing. Avoids common fingerprinting patterns. No unsolicited network pings.
- **3 — Viable:** No self-identifying network traffic. Minimal disk footprint. Privacy-aware defaults.
- **2 — Weak:** Identifies as a NeXuS node to the network. Persistent logs contain identifying timestamps or hashes.
- **1 — Failing:** Clear telemetry. Self-identifying pings. Leaks node ID or hardware fingerprint.

#### ✨ Scoring: BEAUTIFUL
*Is the artifact a joy to use and engage with?*

- **5 — Exemplary:** Visually stunning. Narrative flow. Masterful use of color, ASCII/Emojis, and diagrams. Inspiring.
- **4 — Strong:** High-quality presentation. Clear hierarchy. Easy to navigate. Professional aesthetic.
- **3 — Viable:** Clean. Consistent formatting. Uses emojis for navigation. Not an "eyesore."
- **2 — Weak:** Messy formatting. Wall of text. No visual aids. No consideration for user experience.
- **1 — Failing:** Unreadable. Broken formatting. Hostile UI/UX. No effort made to present the "Why."

---

## 🧭 G2: The Reviewer Guide
*For the community, by the community — how to forge trust.*

As a Reviewer, you are the **Witness**. Your job is to verify that an artifact's **Identity** (the META block) matches its **Reality** (the content/code).

### 1. The Audit Process
1.  **Read the META Block:** Identify the **type**, **principles**, and **identity**.
2.  **Verify the Mandatory Principles:** Every type has mandatory principles (e.g., `NODE` must be `Sane`, `Secure`, and `Stealthy`). If they aren't scored ≥ 3, the artifact is `DRAFT`.
3.  **Auditing SANE:** Trace the logic from input to output. Does it follow the project's "Sane" standards?
4.  **Auditing SECURE:** Check for hardcoded secrets, lack of input sanitization, or "fail open" logic.
5.  **Auditing BEAUTIFUL:** Does it follow the "Beautiful" documentation/UI standards? Is it clear and inspiring?

### 2. Submitting the Score
Once the audit is complete, update the `Variables` section of the META block:
- Increment the `version` (if applicable).
- Set `score-sane` through `score-beautiful`.
- Calculate the `score-composite` (Sum of active principle scores).
- Determine the `score-tier` (`DRAFT` < 9, `REVIEW` 9-11, `ACTIVE` 12-15, `EXEMPLARY` 16-20).
- Set `score-level` to `2` (Peer).
- Update `scored-by` with your alias and `scored-at` with the date.

### 3. Earning NeXiuM
-   **Reviewer Reward:** The `nexium-reward` listed in the META block flows to your vault once the score is committed to the chain.
-   **Quality Matters:** If your score is overturned by a `Consensus (Level 3)` review, your reward is slashed and your reviewer reputation decreases.
-   **Expertise Mult:** Reviewing artifacts of a higher `type-weight` (e.g., `PROTOCOL` vs `DOC`) results in higher rewards.

---

### Composite Score → Trust Tier

```
Sum of all active principle scores:

< 9           🔴  DRAFT       do not deploy, do not distribute
9  — 11       🟡  REVIEW      peer review required before any use
12 — 15       🟢  ACTIVE      cleared to ship, cleared to earn
16 — 20       🔵  EXEMPLARY   reference implementation, maximum NeXiuM weight
```

---

### Score Level — Who Scored It

| Level | Label | Description |
|:-----:|-------|-------------|
| `0` | Unscored | No review performed |
| `1` | Self | Author's own assessment |
| `2` | Peer | One independent reviewer |
| `3` | Consensus | Majority of qualified reviewers agree |

> 🟡 Self-scored artifacts (level 1) are flagged in listings. They may run but earn reduced NeXiuM until peer review confirms.

---

### NeXiuM Reward Calculation

```
nexium-reward = composite-score × principle-count × artifact-type-weight × level-multiplier
```

**Level multipliers:**
- Level 0 (unscored): 0
- Level 1 (self): 0.5
- Level 2 (peer): 1.0
- Level 3 (consensus): 1.5

**Example:**
```
Ghost Gate script (SCRIPT type, weight 1.5)
Active principles: Sane(4) + Secure(5) + Stealthy(4) = composite 13
Principle count: 3
Level: 2 (peer reviewed)

nexium-reward = 13 × 3 × 1.5 × 1.0 = 58.5 → 58 NeXiuM
```

A reviewer who brings a NODE from REVIEW to EXEMPLARY earns substantially more than one who scores a DOC. The network pays for what it needs most.

---

## 📊 Score Notation in META Block

```
score-sane:       4      ← per-principle scores
score-simple:     0      ← 0 = not in active profile
score-secure:     5
score-stealthy:   4
score-beautiful:  0
score-composite:  13     ← sum of active principle scores
score-tier:       ACTIVE ← DRAFT | REVIEW | ACTIVE | EXEMPLARY
score-level:      2      ← 0=unscored | 1=self | 2=peer | 3=consensus
scored-by:        anon
scored-at:        2026-04-01
nexium-reward:    58
```

---

## 🔄 The Artifact Lifecycle

```
IDEA
  │
  ▼
FORGE STAMP ← META block written, type declared, principles selected
  │             Constants locked. Variables initialized to zero.
  ▼
SELF-SCORE (level 1)
  │             Author assesses each active principle 0-5
  │             score-tier = DRAFT or REVIEW
  ▼
PEER REVIEW (level 2)
  │             Independent reviewer audits against declared principles
  │             score-tier advances if composite ≥ threshold
  ▼
CONSENSUS (level 3) ← optional, for NODE and PROTOCOL types
  │             Majority agreement from qualified reviewers
  │             Maximum NeXiuM weight unlocked
  ▼
SHIP / DEPLOY
  │             Artifact enters active use
  │             score-tier = ACTIVE or EXEMPLARY
  ▼
MAINTENANCE
              Variables update as artifact evolves
              Re-score triggered when version increments
              Deprecated → Archived (never deleted)
```

---

## 🌐 Cross-Artifact Integration

The Forge stamp is machine-readable. Every NeXuS system that handles artifacts reads the META block:

| System | How It Uses the Forge |
|--------|----------------------|
| **Ghost Gate** | Reads `score-tier` — DRAFT artifacts blocked from network egress |
| **Orchestrator** | Reads `principles` + `zero-trust` — routes permits accordingly |
| **Community Trust Layer** | Reads `score-level` — surfaces peer-reviewed artifacts first |
| **NeXiuM Vault** | Reads `nexium-reward` — distributes credits to reviewers automatically |
| **MkDocs Site** | Reads `type` + `tier` + `audience` — auto-filters documentation |
| **Hydra Dashboard** | Reads `score-tier` of running containers — flags DRAFT services |
| **NXS-NODE-STANDARD** | Reads `type:NODE` + `score-tier` — gates script inclusion in SFS packs |

---

## 📋 Full META Block Reference

### Complete Example — Ghost Gate Enforce Script

```bash
# NXS-FORGE-META
# id:            NXS-SCRIPT-0007
# type:          SCRIPT
# principles:    Sane Secure Stealthy
# cia:           Confidentiality Accountability Deniability
# moe:           Observability
# pop:           Open-Source
# ymca:          CLI-First
# zero-trust:    true
# sovereignty:   local
# transparency:  full
# defense:       active
# created:       2026-04-01
# authors:       gemini claude
# project:       nexus-ghost-gate
# layer:         infra
# version:       1.1
# updated:       2026-04-01
# status:        active
# audience:      developer
# tier:          all
# score-sane:    4
# score-secure:  5
# score-stealthy:4
# score-composite:13
# score-tier:    ACTIVE
# score-level:   2
# scored-by:     anon
# scored-at:     2026-04-01
# nexium-reward: 58
```

### Complete Example — ECO Extraterrium Stack Doc

```markdown
<!-- NEXUS-META
id:             NXS-DOC-0031
type:           SPEC
principles:     Sane Simple Beautiful
cia:            Integrity Accountability
moe:            Extensibility
pop:            Open-Source
ymca:           Yes-We-Can Multi-AI-Harmony
zero-trust:     false
sovereignty:    local
transparency:   full
defense:        passive
created:        2026-04-01
authors:        gemini claude
project:        nexus-eco
layer:          eco
version:        1.1
updated:        2026-04-01
status:         active
audience:       all
tier:           all
score-sane:     4
score-simple:   4
score-beautiful:4
score-composite:12
score-tier:     ACTIVE
score-level:    1
scored-by:      claude
scored-at:      2026-04-01
nexium-reward:  12
-->
```

---

## ⚒️ Gemini's Directive — Next Steps

The foundation is set. Your task is to take this spec and forge it into a living system:

### G1. Expand the Scoring Rubric
Each principle needs a **detailed rubric** — specific, testable criteria for each score level (0-5). Not abstract. Concrete. A reviewer reads the rubric and knows exactly what a `4` looks like vs a `3`.

Example structure needed:
```
### Scoring: SECURE (for SCRIPT type)
5 — Exemplary: [specific criteria]
4 — Strong:    [specific criteria]
3 — Viable:    [minimum bar]
2 — Weak:      [what's missing]
1 — Failing:   [active violation]
```

### G2. Write the Reviewer Guide
A practical guide for community reviewers:
- How to read a META block
- How to audit each principle type
- How to submit a score
- How NeXiuM flows to the reviewer's vault

### G3. Backfill Existing Artifacts
Apply the Forge stamp retroactively to key existing artifacts:
- `~/claude/demo-dark-stack/new-hydra/ghost-gate.nft`
- `~/claude/nexus-orchestrator/nexus/nexus_ghost_permit.py`
- `~/claude/demo-dark-stack/new-hydra/scripts/ghost-gate-enforce.sh`
- `~/claude/docs/NEXUS_ECO_EXTRATERRIUM_STACK.md`
- `~/claude/docs/NXS-BOOT-PROTOCOL.md`
- `~/claude/docs/NEXUS_SERVICE_MAPPING.md`

### G4. Wire into NEXUS_COMPASS.md
Add NXS-FORGE.md to the Document Map under a new **Foundational Protocols** section.

### G5. Update FNG.md
Add NXS-FORGE.md to the reading list as F-series entry: *Read before touching any artifact.*

---

> **⚒️ The Forge is where intent becomes identity.**
> *The META block is the declaration. The score is the proof. The tier is the verdict.*
> *Nothing enters the network unexamined. Nothing ships without the stamp.*
>
> **NeXuS: Sane • Simple • Secure • Stealthy • Beautiful**
> *mewe — sovereign me inside a we*
