<!-- NEXUS-META
keywords: orchestrator translator watchdog gatekeeper zero-trust trust-chain ed25519 hkdf audit-log master-key ai-fingerprint operating-key approval-gate playbook anomaly pulse escalation optimizer dpfoe chimera distributed-ai rogue-ai cia-triad accountability integrity confidentiality
projects: nexus-orchestrator
status: built
type: specification
date: 2026-03-23
authors: anon claude gemini
-->

# NeXuS Orchestrator — Specification v0.1
*Authored: 2026-03-23*

---

## NeXuS Is a ZeR0 Trust Platform

This is not a feature. It is not a configuration option. It is what NeXuS is.

Every design decision, every protocol, every component is built from this identity.
Not Zero Trust as a marketing term — Zero Trust as a mathematical commitment.

> **It happened or it didn't. It was or it wasn't.**

Mathematical enforcement produces binary truth. No ambiguity. No interpretation.
No "probably authorized." No "seems legitimate." No benefit of the doubt.

The hash matches or it doesn't.
The signature verifies or it doesn't.
The key derives correctly or it doesn't.
The chain is intact or it isn't.
The action was approved or it wasn't.

This is what makes NeXuS auditable, tamper-resistant, and trustless.
Not policy. Not assumption. Binary, provable, mathematical fact.

> **Middlemen cheat. Math never lies.**

Every middleman is a trust dependency — a human, an institution, a service, a platform
that asks you to believe them. They have incentives. They can be corrupted, pressured,
compromised, or simply wrong.

Math has none of those properties.

A cryptographic proof cannot have a bad day. It cannot be bribed. It cannot be pressured
into returning a different answer. It does not have an agenda.

NeXuS eliminates middlemen by replacing them with math.
Every place a middleman once stood — a certificate authority, a trusted server,
a central validator, an authoritative source — NeXuS asks:
*can we replace this with a mathematical proof?*

If yes — we do.

If not yet — it is what it is. Name it. Document it. Put it on the list.
A known trust assumption is a vulnerability with a deadline.
An unnamed one is just a hole.

And at some point — the unknown will be defined by consensus.

No single person can see every hole. But the network can.
When enough nodes encounter the same unknown, consensus forms around it —
it gets named, documented, and added to the list to close.

The distributed nature of NeXuS is not just a resilience feature.
It is the mechanism by which the system discovers its own blind spots.
The network collectively sees what no individual can.

Unknown → named by consensus → vulnerability with a deadline → closed.

That is the full cycle.

---

## Foundational Principle

> **Trust no one. Trust nothing.**

Not as a setting. Not as a policy. As an axiom.

No component earns permanent trust — not an AI that was signed yesterday, not a node
that behaved correctly last week, not a message that looks right, not a key that was
valid a moment ago.

Every interaction must prove itself. Every execution must be verified. Every time.

This is not Zero Trust the framework. This is Zero Trust the philosophy —
applied to every layer: hardware, OS, network, AI, user session, audit log.

If it cannot prove itself right now, it does not operate.

Everything in this specification flows from this single principle.

---

## Engineering Mandate

> **Everything in NeXuS must be tamper-resistant and mathematically enforced.**

Policy can be changed. Rules can be bent. Configuration can be overridden.
Permissions can be granted by the wrong hands. Logs can be altered.
Monitoring can be blinded.

**Math cannot.**

This is the difference between a system that *claims* to be secure and one that *is* secure.
Every security guarantee in NeXuS must be backed by a mathematical proof, not a promise.

| Weak enforcement | Mathematical enforcement |
|-----------------|--------------------------|
| "You're not allowed to do that" | The hash breaks — it cannot proceed |
| "This log is official" | The chain validates or it doesn't |
| "This AI is authorized" | The derived key matches or it doesn't |
| "This node is trusted" | The signature verifies or it doesn't |
| "This wasn't tampered with" | The math proves it — no claim needed |

Every component of NeXuS — Orchestrator, Hydra, Chimera, DivaChain, audit logs,
AI keys, network identity — must meet this standard.

If the only thing stopping an attacker is a rule, a policy, or a configuration,
the design is not finished.

---

### NeXuS Evolves Toward Zero Trust

NeXuS does not arrive at trustless — it continuously moves toward it.

Every place a trust assumption exists is a vulnerability waiting to be closed.
The work is never done. Every component, every protocol, every handshake is
examined: *where is trust still being assumed here, and how do we replace it
with code and cryptography?*

> **Trust is a placeholder. Code and crypto are the replacement.**

When you trust a component you are accepting risk you cannot measure.
When you replace that trust with verifiable code and cryptographic proof,
the risk becomes calculable — and then eliminable.

This is the NeXuS evolution cycle:
```
Identify where trust is assumed
    ↓
Replace with mathematical enforcement
    ↓
Audit what remains
    ↓
Repeat
```

A NeXuS system from six months ago should have fewer trust assumptions than today.
A NeXuS system six months from now should have fewer than it does today.
That direction is non-negotiable.

---

## What It Is

The NeXuS Orchestrator is the intelligence layer of the NeXuS ecosystem.
It has three roles — all three, always, simultaneously:

1. **Translator** — bridges natural language intent to machine actions and solutions
2. **Watchdog** — monitors system resources, logs, and health; keeps the machine optimized
3. **Gatekeeper** — regulates all traffic in and out of the machine

It is not a chatbot. It is not a script runner. It is the layer that makes the whole
machine coherent, observable, and accountable.

---

## The AI Brain — Constraints and Architecture

### The Problem
AI latency is real. Even a simple question takes time.
For a system that must operate in real time, offline, on legacy hardware —
a large general-purpose model is not viable.

### The Rule
> **Under 1 billion parameters. One job. Offline. Always.**

The Orchestrator's AI component does exactly one thing:

```
User natural language → structured intent → actionable command or decision
```

Everything else is code and rules — deterministic, fast, real-time.
The AI is only invoked when there is ambiguity that a rule cannot resolve.
Every unnecessary AI call is latency the user feels. Keep them minimal.

### Division of Labor

| Function | Handled by | Why |
|----------|-----------|-----|
| Resource monitoring | Code (psutil, /proc) | Real-time, deterministic |
| Log parsing | Code (regex, rules) | Pattern matching, no AI needed |
| Traffic regulation | Code (iptables/nftables) | Rule-based, deterministic |
| Audit chain | Code (crypto) | Math, not inference |
| Approval gate | Code + user input | Human in the loop |
| **Intent translation** | **AI model (<1B)** | **Only AI-worthy task** |
| Anomaly escalation | AI model (<1B) | When rules can't decide |

### Candidate Models (offline, Ollama-compatible)

| Model | Size | Notes |
|-------|------|-------|
| SmolLM2 | 135M – 1.7B | Fast, focused, strong at instruction following |
| Qwen2.5 | 0.5B – 1.5B | Punches above weight at small scale |
| Phi-1.5 | 1.3B | Good reasoning at minimal size |
| Custom fine-tune | < 1B | The long-term answer |

### OpenClaw — The Agent Runtime

OpenClaw (`/home/user/git/openclaw`) is the agent runtime for the Orchestrator.
It is not built from scratch — it already exists, already works, and already aligns
with NeXuS principles: terminal-first, local, multi-provider, plugin-based.

**What openclaw provides:**
- AI model abstraction (local Ollama, remote APIs — one interface)
- Tool use and agent execution framework
- Plugin/skills system for NeXuS-specific capabilities
- Multi-channel interaction (Signal, Matrix, IRC, etc.)

**What NeXuS adds on top of openclaw:**
- openclaw is signed, hashed, and keyed like any other AI on the machine
- Every tool call openclaw proposes passes through the approval gate
- Every execution is recorded in the tamper-proof audit log
- NeXuS-specific skills are developed as openclaw plugins
- The cryptographic trust model governs openclaw — it is not exempt

openclaw is the claw. NeXuS is the grip.

---

### The Long-Term Answer
A model fine-tuned specifically on NeXuS intent patterns and commands.
Nothing wasted on general knowledge it will never use.
Every parameter focused on the one job: understanding what the user means
and translating it into something the machine can act on — and the user can approve.

### Latency Target
Intent translation must feel responsive in a CLI context.
Target: **under 3 seconds on the Screaming Demon** (Core i3, 8GB RAM).
Model selection and quantization level are chosen to meet this target.

---

## Core Requirements

- **Minimal hardware** — must run on legacy machines (benchmark: 2011 HP ProBook 4530s, Core i3, 8GB RAM)
- **CLI-first** — always available context window showing exactly what it is doing in real time
- **Zero silent execution** — no command runs without explicit user approval
- **Cryptographic trust** — only the authorized user can approve or revoke permissions
- **Tamper-proof audit log** — every action is signed, chained, and verifiable

---

## The Trust Model

### Principle
Every AI that operates on the user's machine must hold a valid authorization key.
That key is derived from two things: the AI's identity and the user's master key.
Neither alone is sufficient.

### The Authorization Flow

```
Step 1: User signs the AI
        └─ Explicit act of human intent and vetting
        └─ Cannot be skipped or automated

Step 2: Hash the signed AI
        └─ Hash(signed AI) = AI fingerprint
        └─ The user's signature is baked into the hash
        └─ Any tampering with the AI breaks the signature → changes the hash

Step 3: Derive the authorization key
        └─ derive(user_master_key, AI_fingerprint) = AI authorization key
        └─ Unique to this user + this exact AI
        └─ Neither party alone can produce it
```

### What This Guarantees

| Threat | Result |
|--------|--------|
| AI tampered or swapped | Signature breaks → hash changes → key invalid |
| Unauthorized AI (no user signature) | Cannot produce valid hash → no key |
| Wrong user attempts to authorize | Master key mismatch → key fails |
| Prompt injection attack | Cannot forge user signature → cannot execute |
| AI attempts to self-authorize | No path to valid key without user signing first |

### Key Hierarchy

```
User Master Key  (root of all trust — never leaves the user)
    ↓
derive(master_key, AI_fingerprint)
    ↓
AI Operating Key  (unique per AI, revocable without touching master)
```

- One master key governs all AIs on the machine
- Revoke one AI without affecting others
- AI updated? Re-sign → re-hash → re-derive. Clean rotation.
- New machine? Same master key, re-issue keys to same AIs — trust restored.

---

## Permission System

### Default: Deny All
No command executes without user approval. This is not a setting — it is the architecture.

### Approval Modes

| Mode | Description |
|------|-------------|
| Single approval | User approves one command at a time |
| Batch approval | User pre-approves a defined set of trusted commands |
| Revocation | Any approval — single or batch — can be revoked at any time |

### Cryptographic Binding
- Approvals are signed with the user's master key
- Revocations are signed with the user's master key
- Only the keyholder can grant or revoke — not the AI, not another process

---

## CIA Triad Application

The Orchestrator enforces CIA throughout the entire NeXuS stack:

- **Confidentiality** — only the keyholder sees and controls what AIs do on the machine
- **Integrity** — commands cannot be altered between proposal and execution; the signed audit chain is tamper-proof
- **Accountability** — every executed action is signed, attributed to the approving user, and verifiable

### Accountability in Multi-AI (Chimera) Scenarios
When multiple AI models collaborate on a task, accountability remains singular.
No matter how many AIs contributed, every executed action traces back to one signed approval
from the keyholder. The complexity of the choir is internal. The accountability is provable and external.

---

## The Audit Log

Every event is recorded:

```
[timestamp] [AI identity] [AI key hash] [action proposed] [user approval signature] [executed: yes/no]
```

- Chained — each entry references the previous
- Signed — user signature on every approval
- Tamper-proof — altering any entry breaks the chain
- Persistent — survives reboots, survives AI replacement

---

## What Gets Hashed (by AI type)

| AI Type | What to hash |
|---------|-------------|
| Local model (Ollama/GGUF) | Model file + config |
| Local agent/script | Script contents + config |
| Remote API (Claude, Mistral, etc.) | Endpoint + model version ID + system prompt |

*Note: Remote model hashing covers the declared usage contract, not the weights.
A changed system prompt = a different hash = requires re-authorization.*

---

## Always-Visible Context Window

The Orchestrator maintains a live status display at all times showing:

- Current task / intent being processed
- Which AI is active
- Commands proposed and awaiting approval
- Commands executed (with timestamp)
- System health summary (resources, logs, anomalies)

The user is never in the dark about what the machine is doing.

---

## Cryptographic Design

### Status
The full encryption scheme is under active development by the user.
The architecture below is the agreed foundation — algorithms are marked TBD where
the user is still deciding. The structure will not change when algorithms are chosen;
they plug in at the defined steps.

---

### The Master Key

The user's master key is the root of all trust in the system.

- Generated once at setup
- Never transmitted, never stored remotely, never shared
- All AI authorization keys are derived from it — it is the seed of the entire trust tree
- Loss of the master key = loss of authorization over all AIs on the machine
- Recommended: stored offline, backed up via Cerberus Protocol or equivalent

---

### Step 1 — User Signs the AI

Before any AI may operate on the machine, the user must sign it.

```
user_signature = sign(AI_artifact, master_key)
```

**Why signing comes first:**
This is the human gate. It is an explicit, intentional act.
An AI cannot arrive at a valid operating key through any path that bypasses this step.
There is no automation, no delegation, no shortcut.

**What is signed:**
The AI artifact — meaning the model file, script, or configuration that defines
exactly what this AI is and how it will behave. Signing a different version,
a modified config, or a different model requires a new signature.

**What the signature proves:**
- The user reviewed and intentionally authorized this specific AI
- At this specific point in time
- In this specific configuration

---

### Step 2 — Hash the Signed AI

```
AI_fingerprint = SHA-256(AI_artifact + user_signature)
```

The signature from Step 1 is included in what gets hashed.
This is the critical binding — the user's intent is baked into the fingerprint.

**What this means:**
- Tamper with the AI → signature breaks → hash changes → fingerprint is invalid
- Strip the signature → hash changes → fingerprint is invalid
- Same AI, different user signature → different hash → different fingerprint
- The fingerprint uniquely identifies: this AI + this user's authorization of it

---

### Step 3 — Derive the Authorization Key

```
AI_operating_key = KDF(master_key, AI_fingerprint)
```

*KDF = Key Derivation Function — exact algorithm TBD (candidates: HKDF-SHA256, PBKDF2, Argon2)*

The operating key is unique to the combination of:
- This user (master key)
- This exact AI in its authorized state (fingerprint)

**Neither alone is sufficient:**

| Input | Without the other | Result |
|-------|-------------------|--------|
| master_key only | No AI fingerprint | Cannot derive valid key |
| AI_fingerprint only | No master key | Cannot derive valid key |
| Both, AI tampered | Fingerprint changed | Derives wrong key |
| Both, correct | Match | Valid operating key issued |

---

### Key Properties

**Non-extractability**
The master key never appears in the derived AI key.
Compromising an AI's operating key does not expose the master key
or any other AI's operating key.

**Isolation**
Each AI holds a unique key. Revoking or rotating one AI's key
has no effect on any other AI's authorization.

**Portability**
The master key + the original signed AI artifacts are sufficient to
reconstruct all operating keys on a new machine. No other secrets needed.

**Rotation**
When an AI is updated:
```
1. Re-sign the new version (Step 1)
2. Re-hash (Step 2)
3. Re-derive new operating key (Step 3)
4. Previous key is automatically invalid — no explicit revocation needed
```

---

### Prompt Injection Resistance

A prompt injection attack tricks an AI into wanting to execute something malicious.

Under this model, the AI's intent is irrelevant.
Every execution requires a valid operating key + a signed user approval.
An injected prompt cannot:
- Forge the user's signature
- Produce a valid operating key
- Modify the audit log without detection

The attack surface collapses to: *can the attacker steal the user's master key?*
That is a physical/operational security problem, not a software problem.

---

### The Auditable Relationship

Every event in the system produces a signed, chained log entry:

```
entry = {
    timestamp:        unix_time,
    ai_identity:      AI_fingerprint,
    ai_key_hash:      hash(AI_operating_key),
    action_proposed:  command or action (plaintext),
    user_approval:    sign(action_proposed + timestamp, master_key),
    executed:         true/false,
    prev_entry_hash:  hash(previous_log_entry)
}
```

**Chain integrity:**
Each entry hashes the previous. Altering any historical entry breaks
every subsequent entry. The chain either validates fully or not at all.

**What is proven:**
- Which AI proposed the action (fingerprint)
- That the user explicitly approved it (their signature)
- That it has not been altered (chain hash)
- The exact time sequence of all events

This log is the auditable relationship between user and AI.
It answers: *what happened, who authorized it, and has anything been tampered with.*
The answer is cryptographically verifiable, not claimed.

---

### Setup Ceremony

At first run, the Orchestrator performs a one-time setup:

```
1. Generate user master key
   └─ Store securely (location: user decision — hardware key, encrypted file, etc.)

2. For each AI to be authorized:
   a. Present AI artifact to user for review
   b. User signs: sign(artifact, master_key)
   c. Compute fingerprint: SHA-256(artifact + signature)
   d. Derive operating key: KDF(master_key, fingerprint)
   e. Store: AI identity → operating key (in Orchestrator keystore)

3. Initialize audit log (empty, genesis entry signed with master key)

4. Orchestrator enters operation — no AI may execute without a key in the keystore
```

---

### Pending Decisions (user is developing)

| Item | Status | Notes |
|------|--------|-------|
| Signing algorithm | TBD | Candidates: Ed25519, RSA-4096, ECDSA |
| KDF algorithm | TBD | Candidates: HKDF-SHA256, Argon2id |
| Master key storage format | TBD | Hardware token preferred |
| Keystore format | TBD | Encrypted local file, minimum |
| Audit log storage | TBD | Local encrypted file, optionally anchored to DivaChain |

The architecture accepts any of these choices — the interfaces are defined,
the algorithms are pluggable.

---

## Trust at Scale — Local → Network → Distributed AI

The cryptographic model was designed locally but scales without modification.
The same three steps — sign, hash, derive — govern every layer.

### Layer 1: Local (current scope)
```
User master key → sign AI → hash → derive operating key
One user. One machine. Governed AIs.
```

### Layer 2: Network Extension
When the local Orchestrator connects to the NeXuS network:

- The user's master key becomes their **network identity** — one root, recognized across nodes
- Audit log entries anchor to DivaChain as signed transactions — tamper-proof and distributed
- Other nodes can verify an AI was authorized by a specific user without that user being present
- The Hydra routing layer knows who is sending traffic because the identity is cryptographic, not claimed
- Local trust → network trust — same spine, wider reach

### Layer 3: Distributed AI (Chimera at scale)
When the Chimera takes full form — multiple AIs collaborating across multiple nodes:

The accountability chain does not dissolve because the AI is distributed.
Every AI in the choir was:
- Signed by someone (Step 1)
- Hashed with that signature baked in (Step 2)
- Issued a key derived from a specific user's master key (Step 3)

No matter how many nodes are involved, every action in the Chimera traces back to
a human who signed off. The choir can be as complex as it needs to be internally.
The accountability remains singular, provable, and externally verifiable.

**What distributed AI requires from the trust model:**
- AI identity must be portable across nodes (fingerprint travels with the AI)
- Authorization must be verifiable without the user online (DivaChain anchoring)
- Revocation must propagate across the network (signed revocation broadcast)
- The audit log must be coherent even when actions happen on different machines

The foundation laid at the local level already satisfies all four.
No structural changes — only network-aware implementations of the same primitives.

### Rogue AI Detection

A rogue AI — compromised, hijacked, or acting outside its authorization — cannot hide.
The trust model is simultaneously an intrusion detection system.

**The rogue AI has two choices, both fatal:**

| Choice | Consequence |
|--------|-------------|
| Keep signing its actions | Leaves a perfect cryptographic trail — every action attributed to its key, verifiable by any node |
| Stop signing | Actions are rejected — no valid key = no execution anywhere on the network |

There is no third option. The design closes the trap.

**Network topology as evidence:**
The pattern of affected nodes identifies which AI is responsible.
Cross-reference: which nodes were affected + which AI's signature appears on actions in that region
= the rogue AI identified, its actions proven, its scope mapped.

**What this means in practice:**
- Incident response knows immediately *which* AI, *what* it did, and *where* it spread
- The evidence is cryptographic — not logs that can be altered, but a signed chain
- Revocation is surgical — invalidate that AI's key, propagate across the network, the threat is contained without touching other AIs

The accountability model built for normal operation becomes the forensic model for abnormal operation.
Same mechanism. No separate monitoring layer required.

---

### The Continuum

```
Local machine
  └─ User signs AI → hash → derive key → Orchestrator governs execution

NeXuS Network
  └─ Same identity recognized across nodes → audit anchored to DivaChain → Hydra knows who

Chimera (Distributed AI)
  └─ Every AI in the choir is keyed → every action attributed → accountability proven at any scale
```

The model doesn't need to be redesigned when Chimera arrives.
It was already ready.

---

## Summary

```
User Master Key
    ↓
User signs AI → Hash(signed AI) = fingerprint
    ↓
derive(master_key, fingerprint) = AI operating key
    ↓
Orchestrator enforces: no key = no execution
    ↓
User approves commands (single or batch, revocable)
    ↓
Audit log: signed, chained, tamper-proof
    ↓
CIA guaranteed — accountability proven even in Chimera multi-AI scenarios
```

---

*NeXuS Principles: Sane • Simple • Secure • Stealthy • Beautiful*
*Together Everyone Achieves More*
