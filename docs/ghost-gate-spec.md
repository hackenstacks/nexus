# NeXuS Ghost Gate Specification
*Established 2026-03-23 | Anon + Claude + Gemini*

<!-- NEXUS-META
keywords: ghost-gate network-egress packet-control zero-trust mtls cerberus ai-fingerprint operating-key user-permit nftables ebpf permit-lifecycle audit-chain exfiltration rogue-ai faraday perimeter ingress proxy
projects: nexus-ghost-gate nexus-orchestrator nexus-cerberus
status: specced
type: specification
date: 2026-03-23
authors: anon claude gemini
depends-on: nexus-orchestrator nexus-cerberus-framework
-->

---

> *"The Orchestrator governs what runs. The Ghost Gate governs what leaves."*

---

## The Problem

The NeXuS Orchestrator controls command execution — every shell command is verified,
approved, and logged before it runs. This closes the command surface.

But it leaves one surface open: **the network.**

An AI artifact that has been registered and issued an operating key can currently:
- Run approved shell commands ✅ governed
- Write to the audit log ✅ governed
- Interact with the approval gate ✅ governed
- **Reach the network** ❌ ungoverned

Without the Ghost Gate, a rogue or compromised AI could exfiltrate data, beacon to
a command-and-control server, or establish persistence — and no packet would be
logged, blocked, or attributed.

The Ghost Gate closes this surface. **No packet leaves the machine without passing
a Cerberus gate.**

---

## What It Is

The Ghost Gate is a Zero Trust network egress proxy built on the
**Cerberus Framework** (see `NEXUS_CERBERUS_FRAMEWORK.md`).

It sits between every process on the machine and the network interface.
Every outbound connection attempt must present credentials to the Ghost Gate
before a single byte is transmitted.

```
[AI Process] ──→ [Ghost Gate] ──→ [Network]
                      ↑
               Three heads.
               All must pass.
               Any fail = packet dropped + logged.
```

---

## The Three Heads

### HEAD 1 — AI Identity (`ai_fingerprint`)
The process attempting network access must present its registered `ai_fingerprint` —
the SHA-256 hash of `(artifact_bytes + signature_bytes)` derived during registration
in `nexus_trust.py`.

- Proof type: cryptographic identity
- Source: `nexus_orchestrator.register_ai()` registry
- Failure condition: fingerprint not registered, or does not match a known artifact
- Independence: derived entirely from the artifact itself — cannot be forged without
  access to both the artifact and the master private key

### HEAD 2 — Operating Key (`operating_key`)
The process must present its `operating_key` — the 32-byte HKDF-SHA256 key derived
from `derive_key(master_private_key, ai_fingerprint)`.

- Proof type: derived cryptographic proof
- Source: `nexus_trust.derive_key()` — deterministic from master key + fingerprint
- Failure condition: key does not match what the Ghost Gate derives independently
- Independence: derived from master key + fingerprint — neither head alone produces it.
  An attacker with the fingerprint but not the master key cannot derive the operating key.

### HEAD 3 — Network Permit (`user_permit`)
The user must have explicitly issued a network permit for this AI artifact.

A permit is a signed JSON object:
```json
{
  "ai_fingerprint": "<fingerprint>",
  "permitted_destinations": ["<host:port>", ...],
  "permitted_protocols": ["https", "nats", ...],
  "issued_at": <unix_timestamp>,
  "expires_at": <unix_timestamp or null>,
  "permit_signature": "<Ed25519 signature by master key>"
}
```

- Proof type: human decision — explicit, signed authorization
- Source: user issues permits via `nexus.py` CLI (`permit <fingerprint> <destination>`)
- Failure condition: no permit exists, permit is expired, destination not in permit,
  protocol not in permit, or permit signature fails verification
- Independence: only the user with the master private key can issue a valid permit.
  Neither the AI's fingerprint nor its operating key can produce a valid permit.

---

## Gate Logic

```
GHOST GATE EVALUATION (per outbound connection attempt)
────────────────────────────────────────────────────────
1. Intercept outbound packet
2. Extract source process identity → ai_fingerprint

HEAD 1: Is ai_fingerprint registered?
  NO  → DROP packet, LOG "unregistered AI attempted egress", ALERT

HEAD 2: Does operating_key verify against fingerprint?
  NO  → DROP packet, LOG "key mismatch on egress attempt", ALERT

HEAD 3: Does a valid user_permit exist for this destination?
  NO  → DROP packet, LOG "no permit for destination", ALERT (optionally notify user)

ALL THREE PASS:
  → ALLOW packet
  → LOG permitted egress: fingerprint, destination, protocol, timestamp, permit_id
────────────────────────────────────────────────────────
```

Every decision — pass or fail — is appended to the audit chain via `nexus_audit.py`.
The Ghost Gate never silently drops. Every dropped packet is a signed audit entry.

---

## Architecture

```
nexus-ghost-gate/
├── nexus_ghost_gate.py        ← Gate daemon — intercepts, evaluates, allows/drops
├── nexus_ghost_permit.py      ← Permit system — issue, verify, revoke, archive
├── nexus_ghost_rules.py       ← Default rules — loopback always allowed, etc.
└── nexus_ghost_audit.py       ← Gate-specific audit (extends nexus_audit.py pattern)
```

### Integration with Orchestrator

The Ghost Gate shares the same master key and AI registry as the Orchestrator.
It does not duplicate trust infrastructure — it extends it to the network layer.

```
nexus_trust.py          ← shared — both Orchestrator and Ghost Gate use this
nexus_audit.py          ← shared — single audit chain for commands AND packets
nexus_orchestrator.py   ← governs: commands
nexus_ghost_gate.py     ← governs: network egress
```

Same spine. One audit chain. Commands and packets governed by the same cryptographic
identity system.

---

## Enforcement Mechanism

The Ghost Gate intercepts traffic using one of two methods:

### Option A — nftables/iptables (preferred for Alpine)
- Default DENY all outbound traffic via nftables rule on startup
- Ghost Gate process owns the ALLOW rules
- When a permit is validated, a temporary nftables rule is added for that connection
- Rule is scoped to: source PID, destination IP:port, protocol, time window
- On connection close or permit expiry: rule removed

### Option B — eBPF (preferred for long-term)
- eBPF program attached to network egress path
- Evaluates Cerberus heads in kernel space
- No packet reaches the wire until all three heads pass
- Integrates naturally with the process recorder vision

Alpine Linux has full nftables support — Option A is the immediate build target.
eBPF is the upgrade path once the kernel module story is confirmed.

---

## Permit Lifecycle

```
Issue:    user runs: nexus permit <fingerprint> <destination> [--ttl <seconds>]
          Ghost Gate creates signed permit JSON
          Permit archived to ~/.nexus/permits/

Active:   Ghost Gate checks permit on every connection
          Permit checked: fingerprint match + signature valid + not expired + dest match

Revoke:   user runs: nexus permit revoke <permit_id>
          Permit archived (not deleted — archive principle)
          nftables rule removed immediately
          Revocation logged to audit chain

Expire:   TTL reached → permit automatically deactivated
          Expiry logged to audit chain
          User notified if on_notify callback registered
```

---

## Audit Trail

Every Ghost Gate event is a first-class audit entry:

```json
{
  "timestamp": 1711234567.89,
  "event_type": "egress_denied",
  "ai_fingerprint": "a3f9b2c1...",
  "destination": "192.168.1.100:443",
  "protocol": "https",
  "head_failed": "HEAD_3_no_permit",
  "prev_hash": "...",
  "entry_hash": "..."
}
```

```json
{
  "timestamp": 1711234570.12,
  "event_type": "egress_permitted",
  "ai_fingerprint": "a3f9b2c1...",
  "destination": "nats.nexus.local:4222",
  "protocol": "nats",
  "permit_id": "p-7f3a...",
  "prev_hash": "...",
  "entry_hash": "..."
}
```

The audit chain is the same chain used by the Orchestrator. One tamper-proof record
of everything that happened on the machine — commands and packets together.

---

## ZeR0 Trust Properties

| Property | How Ghost Gate Enforces It |
|----------|---------------------------|
| No implicit trust | Every connection attempt evaluated — registered AIs included |
| Default deny | nftables default DROP on all outbound, permits are explicit exceptions |
| Math enforces | Fingerprint + key verification is cryptographic, not policy |
| Human decision required | No packet permitted without a user-signed permit |
| Tamper-proof record | Every decision in the audit chain |
| Nothing deleted | Revoked permits archived, not deleted |
| Rogue AI contained | Unregistered or key-mismatched process cannot reach the network |

---

## What This Closes

With the Orchestrator + Ghost Gate together:

```
BEFORE Ghost Gate:
  AI can run approved commands ✅
  AI cannot run unapproved commands ✅
  AI can reach the network freely ❌  ← open surface

AFTER Ghost Gate:
  AI can run approved commands ✅
  AI cannot run unapproved commands ✅
  AI can reach permitted destinations ✅
  AI cannot reach unpermitted destinations ✅
  All egress is logged to the audit chain ✅
  Rogue AI cannot exfiltrate without a user-signed permit ✅
```

The machine becomes a **cryptographic fortress**.
The AI is physically incapable of data exfiltration without a signed user permit.
The audit chain becomes a complete black box recorder — every command, every packet.

---

## Build Order

1. `nexus_ghost_permit.py` — permit issuance, verification, revocation, archive
2. `nexus_ghost_rules.py` — nftables rule management (default deny, permit rules)
3. `nexus_ghost_gate.py` — main gate daemon, Cerberus evaluation loop
4. `nexus_ghost_audit.py` — audit integration, egress event types
5. Wire into `nexus.py` — `permit`, `permit revoke`, `permit list` commands
6. Tests — permit lifecycle, gate evaluation, tamper detection

---

## CLI Commands (proposed)

```
nexus permit <fingerprint> <host:port> [--ttl <seconds>] [--proto <protocol>]
nexus permit list
nexus permit revoke <permit_id>
nexus permits <fingerprint>        ← all permits for an AI
nexus gate status                  ← is Ghost Gate running, rule count, last event
nexus gate log                     ← recent egress audit entries
```

---

*NeXuS Principles: Sane • Simple • Secure • Stealthy • Beautiful*
*The Orchestrator governs what runs. The Ghost Gate governs what leaves.*
*Together: a complete black box. Nothing hidden. Nothing ungoverned.*
