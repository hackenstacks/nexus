# The Janus Protocol
*NeXuS Sovereign Storage + Emergency Response Specification*
*Version 1.0 — 2026-04-02 | Sane • Simple • Secure • Stealthy • Beautiful*

<!-- NEXUS-META
id:           NXS-SPEC-JANUS-0001
type:         SPEC
principles:   Sane Simple Secure Stealthy
project:      nexus-build
layer:        physical-security
version:      1.0
status:       active
-->

---

> **The Core Thesis:** Two objects. Both required. Neither sufficient alone.
> The Ring of Janus holds cryptographic authority.
> The Key of Janus holds physical permission.
> NeXuS requires both. The Protocol governs every threshold.

---

## The Two Objects

### Ring of Janus
- **What:** The PERMITTL Ring — software/cryptographic layer
- **Lives:** In the system, on the node
- **Role:** Seal of authority. Generates scoped permission keys.
- **Never leaves:** The node. Ever.
- **Without the Key:** Silent. Cannot act. Cannot boot.

### Key of Janus
- **What:** The USB physical token
- **Lives:** On your person. Always.
- **Role:** Physical gate opener. LUKS header carrier.
- **Without the Ring:** Orphaned. JANUS Recovery activates.
- **Contains:**
  - Detached LUKS header
  - Decryption keys
  - Ring of Janus unlock material
  - JANUS Recovery hash chain seed

---

## The LUKS Header Detachment

The most important security decision in the entire stack.

### Standard LUKS (vulnerable)
```
Drive:
├── LUKS header     ← forensics finds this immediately
├── Encrypted data  ← at least they know it's encrypted
└── Partition table ← proves an OS existed
```

### NeXuS JANUS LUKS (sovereign)
```
Drive:                          Janus Key (USB):
├── Random noise                ├── Detached LUKS header
├── Random noise                ├── Master decryption key
├── Random noise                ├── Ring of Janus material
└── Random noise                └── Recovery hash chain

Seized machine = provably blank
No header = no evidence of encryption
No partition = no evidence of OS
Random noise = random noise
```

### The Legal Protection
You cannot be compelled to decrypt what cannot be proven encrypted.
A drive of random noise is a drive of random noise.
No header. No partition table. No forensic footprint.

### Implementation
```bash
# Create detached header LUKS volume
cryptsetup luksFormat \
    --header /mnt/janus-key/nexus.header \
    --type luks2 \
    /dev/sda

# Open with detached header
cryptsetup luksOpen \
    --header /mnt/janus-key/nexus.header \
    /dev/sda nexus-vault

# Without USB present:
# /dev/sda = random noise
# No tool can prove otherwise
```

---

## The Four States

### JANUS OPEN
```
Ring of Janus  +  Key of Janus  =  Gate Opens
Both present       USB inserted     NeXuS lives
                                    Full operation
                                    All services available
```

### JANUS SILENT
```
Ring present  —  Key removed  =  Immediate response
USB pulled        USB absent      RAM wipe begins
                                  Session zeroed
                                  Network killed
                                  Node waits in silence
                                  Drive = random noise
                                  Was never running
```

### JANUS RECOVERY
```
Key lost  OR  Ring compromised  =  Hash chain activates
                                    R0 → R1 succession
                                    New Key forged
                                    Identity migrates
                                    Chain witnessed you
                                    You are recognized
                                    Continuity preserved
```

### JANUS VOID
```
Emergency wipe  =  Total erasure
Deliberate          Nothing survives
Final               Machine is stone
                    Identity complete
                    Begin again
```

---

## Emergency Triggers

Three ways to activate JANUS VOID. Any one fires it.

### Trigger 1 — Physical (Fastest)
```
Pull the Janus Key

USB removed = immediate cascade
No user action required
Fires in milliseconds
Just pull it and walk
```

### Trigger 2 — Keyboard Sequence
```
Left CTRL + Left ALT + SPACE + ENTER

Both hands required
Cannot be fat-fingered
Cannot be hit accidentally
Deliberate. Conscious. Final.
```

### Trigger 3 — Automated
```
Wrong PIN × 3          → fires without human present
Forced access detected → network tamper response
Hardware tamper sensor → physical breach response
Watchdog timeout       → dead man's switch
```

---

## JANUS VOID Wipe Sequence

When triggered — executed in order — cannot be interrupted:

```
Step 1  — Network kill
          Ghost Gate drops all connections immediately
          No data leaves the node

Step 2  — RAM overwrite
          3-pass random overwrite of all RAM
          Keys zeroed first
          Session data gone

Step 3  — Swap overwrite
          All swap space wiped
          Nothing recoverable from virtual memory

Step 4  — Ring of Janus zeroed
          PERMITTL Ring material wiped
          Scoped keys invalidated
          No cryptographic material survives

Step 5  — Session .cow wipe
          Copy-on-write layer destroyed
          User data gone

Step 6  — Log wipe
          All logs zeroed
          Audit trail gone
          Was never here

Step 7  — LUKS header check
          If Janus Key present: header already left with USB
          If key still inserted: eject and wipe header from USB
          Drive = confirmed random noise

Step 8  — Shutdown
          Cold. Clean. Silent.
          The machine was never running anything.
```

---

## JANUS Recovery Protocol

Lost your Janus Key. Node compromised. Need to recover identity.

Based on pre-committed hash chain generated at Ring creation:
```
R(n) = hash(R(n+1) + nonce)

R0 = Anchor — submitted to DivaChain at registration
R1...Rn = Stored offline (separate from Janus Key)
```

### Recovery Steps
```
1. Prove identity
   Reveal R1
   Network verifies: hash(R1) == R0
   Identity confirmed without exposing master key

2. Forge new Janus Key
   New USB provisioned
   New LUKS header generated
   New Ring of Janus material derived
   New anchor R1 submitted to DivaChain

3. Migrate assets
   NeXiuM balance migrates to new Ring
   Pink Slip NFT transfers to new anchor
   DivaChain witnesses the succession
   Old R0 invalidated

4. Resume
   New Key. Same identity.
   Chain remembers you.
   Continuity preserved.
```

---

## Physical Security Recommendations

```
Janus Key (USB):
├── Wear it — lanyard, keychain, on your person always
├── Never leave it in the machine unattended
├── Backup: R1...Rn hash chain stored SEPARATELY from Key
├── Consider: hardware encrypted USB (Kingston IronKey etc)
└── Never: photograph it, share it, or store it digitally

The Machine:
├── LUKS detached — drive is noise without the Key
├── RAM-only session — nothing written to disk
├── Ghost Gate default DROP — no network without permits
└── Physical: BIOS password + boot order locked

The Recovery Chain:
├── R1...Rn stored offline — NOT on the Janus Key
├── Paper backup acceptable — offline is sovereign
├── Multiple copies — different physical locations
└── Never: cloud storage, email, digital photos
```

---

## The .sfs Integration

```
Janus Key (USB) contains:
├── nexus.header          ← detached LUKS header
├── janus.key             ← master decryption key  
├── permittl.ring         ← Ring of Janus unlock material
├── recovery.chain        ← R0 anchor + R1 seed
└── nexus-boot.sfs        ← minimal boot layer (optional)
                             lets USB boot NeXuS directly
                             without any files on drive

Drive contains:
├── Random noise          ← encrypted .sfs stack
├── Random noise          ← encrypted session data
├── Random noise          ← encrypted AI models
└── Random noise          ← everything else
```

Without the USB the drive is forensically blank.
With the USB the drive becomes a complete sovereign OS.

---

## Implementation Scripts Needed

```
nexus-janus-init.sh      — provision new Janus Key + Ring
nexus-janus-open.sh      — mount encrypted volumes on USB insert
nexus-janus-close.sh     — wipe and close on USB remove
nexus-janus-void.sh      — emergency full wipe sequence
nexus-janus-recover.sh   — hash chain recovery protocol
nexus-janus-verify.sh    — verify Key + Ring integrity
```

---

## Summary

```
The machine knows nothing without the Key.
The Key means nothing without the Ring.
The Ring trusts nothing without the Protocol.
The Protocol answers to the Chain.
The Chain was witnessed by the network.
The network is you.

Pull the Key.
NeXuS was never here.
```

---

> *Janus guards every threshold.*
> *Two faces. One looking out. One looking in.*
> *Neither world sees the other without his permission.*
>
> *That is NeXuS.*

---

*Janus Protocol v1.0 — NeXuS Sovereign Computing*
*Sane • Simple • Secure • Stealthy • Beautiful*
*meWEwowow*
