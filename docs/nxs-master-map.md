# NeXuS Master Architecture Map
**Status:** Living Document
**Created:** 2026-03-21
**Principles:** Sane • Simple • Secure • Stealthy • Beautiful

---

## The One Line

> Boot. Browse. Power off. It never happened.

---

## Two Tracks

```
NXS-WEB    → browser + WebVM + CheerpX    (Camps 1, 2)
NXS-IRON   → bare metal + USB IMG         (Camps 3, 4)
```

Same NeXuS underneath. Different armour.

---

## The Four Camps

```
Camp 1 — Node Runner
  motivation  → Nexium, passive income from dormant resources
  entry       → browser + USB → WebVM → NeXuS node running
  asks        → how do I earn, how do I start
  needs       → zero friction, zero Linux knowledge

Camp 2 — Pri/Sec Community
  motivation  → tried Tails/Whonix/Qubes, close but not quite
  entry       → IMG flash or WebVM + USB permission
  asks        → show me the code, prove no blobs
  needs       → auditable stack, I2P/Tor baked in

Camp 3 — Minimalist
  motivation  → true Unix + KISS + principles that should always have existed
  entry       → NXS-CORE + NXS-TERM, nothing else
  asks        → what does it NOT include
  needs       → bare metal, no overhead, pure

Camp 4 — Shadows / Ghosts
  motivation  → survival, not preference
  entry       → USB IRON only, no cloud, no WebVM, no phone home
  asks        → prove it leaves nothing
  needs       → power off = forensically unrecoverable
```

---

## The Three Files

```
vmlinuz      ← the kernel
initrd       ← bootstrap: finds NXS.sfs, loads to RAM, sets up overlay
NXS.sfs      ← the NeXuS squashfs system, read-only, immutable
```

Everything NeXuS needs to boot. Nothing more.

---

## The Bootloader

```
Limine
  → modern, BIOS + UEFI from one config
  → clean, auditable
  → same config for IMG and ISO
  → no blobs
```

---

## Delivery Formats

```
NXS.img   ← preferred
            Limine + 3 files + claims ALL remaining USB space as ext4
            full persistence, full NeXuS

NXS.iso   ← on-ramp
            VirtualBox, Windows, Ubuntu users
            test drive, no persistence beyond session
            natural nudge toward IMG when they want more
```

---

## The Installer

```
find ext4 partition
create /NXS/ directory
copy vmlinuz + initrd + NXS.sfs
done
```

Uninstall:
```
rm -rf /NXS/
done
```

---

## USB Behaviour

```
dd NXS.img to USB
     ↓
first boot:
  initrd detects remaining unallocated space
  creates ext4 partition using ALL of it
  /NXS/ dir created
  3 files copied across
  overlay lives here
  lbu commit
  unmount what isn't needed
     ↓
64GB USB = ~63.5GB persistence storage
```

ISO on USB wastes all that space. IMG owns it.

---

## The Filesystem Stack

```
NXS.sfs            ← read-only, compressed, immutable (never changes)
     +
overlay/           ← all user changes, deltas only
     =
merged/            ← FUSE mount, what the system sees
```

Reset:
```
wipe overlay → NXS.sfs takes over instantly → pristine
```

---

## Persistence Modes (Automatic)

```
ext4 found  → overlay writes to /NXS/overlay/    (persistent)
no ext4     → overlay lives in RAM only           (stealthy, no trace)
```

User configures nothing. Hardware decides.

---

## Delta Snapshots

```
NXS.sfs              ← never snapshot (never changes)
overlay/
  snap-001/          ← first changes
  snap-002/          ← delta from 001 only
  snap-003/          ← delta from 002 only
  current → snap-003
```

Rollback = move pointer. Core untouched. Always.

---

## Meta Packs

```
/NXS/
  NXS-CORE.sfs       ← mandatory, always
  NXS-HYPR.sfs       ← desktop choice
  NXS-BROWSER.sfs    ← browser OS layer
  NXS-DEV.sfs        ← development tools
  NXS-PRIVACY.sfs    ← Tor + I2P + Medusa
  NXS-GAMING.sfs     ← Cataclysm DDA etc.
  NXS-NODE.sfs       ← DIVA + XMR + mesh
  overlay/
```

initrd stacks all present SFS files at boot. Merged seamlessly.

**Atomic:** pack present = fully available. Pack absent = completely gone.
**Instant:** squashfs mounts, no extraction, no install steps.
**Upgrade:** replace .sfs file. Overlay untouched.
**Rollback:** swap .sfs.prev back in. One file.

---

## After Setup

```
lbu commit              ← snapshot current overlay state
unmount unused drives   ← reduce attack surface
running surface:
  NXS.sfs in RAM        ← read-only, untouchable
  overlay in /NXS/      ← writes go here only
  everything else       ← unmounted, invisible, unreachable
```

---

## The Browser OS Layer (NXS-BROWSER.sfs)

```
Three layers of isolation:

Layer 1 → bare metal host     (NeXuS never touches it)
Layer 2 → NeXuS in RAM        (browser never touches it)
Layer 3 → browser VM          (internet never touches anything real)
```

Powered by **WebVM + CheerpX** for NXS-WEB.
Powered by **QEMU/KVM** for NXS-IRON.

Browser VM is destroyed on close. Not cleaned. Destroyed.
Reopened = factory fresh from NXS-BROWSER.sfs. No state. No history.

---

## NXS-WEB Entry Point (Camp 1 + 2)

```
Windows/any user
     ↓
opens browser
     ↓
grants USB permission (one click)
     ↓
WebVM sees /NXS/ on USB drive
     ↓
NXS.sfs loads via CheerpX (client-side, no server)
     ↓
full NeXuS node running in browser
     ↓
persistence writes back to USB only
     ↓
close browser → no trace on host machine
unplug USB → it never happened
```

No VirtualBox. No ISO. No install. No Linux knowledge required.

---

## The Authenticator Layer

```
Factor 1 → USB drive (something you have)
             NXS.sfs present = physical possession
Factor 2 → authenticator (something you know/prove)
             TOTP (Aegis, offline, open source)
             OR hardware key (YubiKey, WebAuthn)
             OR Cerberus Protocol (NeXuS-native KDF)
```

Even if USB is stolen — useless without Factor 2.

**Per camp:**
```
Camp 1  → TOTP via authenticator app, simple
Camp 2  → YubiKey passthrough to WebVM/VM
Camp 3  → Cerberus Protocol, hardware key
Camp 4  → hardware key minimum, Cerberus preferred
          key never stored anywhere, derived from memory
```

**The USB IS the first factor.** Authenticator is the second.
Two things required. One stolen = nothing gained.

---

## The AI Layer

```
NXS-WEB   → Claude API (online, full capability)
              guides non-technical users in plain language
              maps human intent to NeXuS choices
              Camp 1 primary interface

NXS-IRON  → local distilled NXS model (offline, auditable)
              trained on entire NeXuS project
              1MB boot translator for initrd stage
              Camp 3+4 — never phones home
```

**Training pipeline:**
```
all session logs + docs + scripts + philosophy
     ↓
NXS-MIND (7B fine-tuned, full knowledge)
     ↓ distillation
NXS-BOOT (0.5B-1B, setup/onboarding only)
     ↓ GGUF quantize
fits in bootstrap RAM stage
```

---

## The Fallback Chain

```
1. LAN server         (fast, private, trusted)
2. USB                (always available)
3. Another NeXuS node (peer mesh, any node serves any node)
4. Internet           (last resort, hash verified)
```

No central server required. Network grows stronger as nodes join.
Round Table applied to infrastructure.

---

## No Blobs — Ever

```
trust model:
  I trust what I can read
  I read the build script
  the build script produces NXS.sfs
  therefore I trust the core
```

No certificate authority. No signed blob from a stranger.
Build scripts are NeXuS-native. Auditable end to end.

---

## The Community Trust Layer

```
developer publishes NXS-*.sfs build script
     ↓
technical community reads and reviews it
     ↓
rated comment: ⭐⭐⭐⭐⭐ "clean, no blobs, does exactly what it says"
     ↓
non-technical user sees rating + plain english summary
     ↓
confidence earned, not blind faith
```

Reviewers earn **Nexium** for quality reviews.
The technical/non-technical gap closes over time.

---

## The Onboarding Ladder

```
1. Browser + NXS-WEB URL     ← zero commitment, test drive
2. Browser + USB permission  ← "I like it", full node, no install
3. Windows dir dual boot     ← C:\NXS\ + bcdedit, no repartition
4. USB IMG flash             ← daily driver, portable, full persistence
5. Internal drive install    ← all in, NeXuS native
```

Every rung reachable from the previous. Nobody jumps from zero to repartitioning.

---

## The Upgrade Story

```
upgrade  → replace NXS.sfs (or meta pack .sfs)
           overlay untouched, settings survive
           one file swap

rollback → swap NXS.sfs.prev back
           one file swap

uninstall → rm -rf /NXS/
            done
```

vs:
```
apt upgrade    → dependency hell
Windows Update → 45 minutes, 3 reboots, something broke
NeXuS upgrade  → cp NXS.sfs /NXS/
```

---

## Converging Projects

These projects arrived independently through NeXuS design decisions:

```
WebVM / CheerpX  ← arrived from Browser OS isolation requirement
OpenClaw         ← arrived from 1MB boot AI concept
Tailscale        ← arrived from mesh node networking
DIVA Chain       ← arrived from Nexium economy
I2P / Tor        ← arrived from Shadows/Ghosts camp
Limine           ← arrived from no-blobs bootloader requirement
LBU              ← arrived from snapshot + minimal surface requirement
```

When you keep arriving at the same project from different directions — it belongs.

---

## Core Principles Mapped

| Principle | Where It Lives |
|-----------|---------------|
| **Sane** | defined defaults, lbu commit, known-good state always restorable |
| **Simple** | 3 files, cp to install, rm to uninstall, one script builds all |
| **Secure** | no blobs, read-only core, authenticator, community audit |
| **Stealthy** | RAM-only mode, no ext4 = no trace, browser VM destroyed not cleaned |
| **Beautiful** | composable layers, clean naming, AI translator, every camp served |

---

## Pending

- [ ] Hash verification protocol for NXS.sfs across mesh nodes
- [ ] Nexium reward formula for community reviewers
- [ ] Minimum review count before script is trusted
- [ ] NXS-* script versioning scheme
- [ ] How nodes advertise as fallback peers
- [ ] Cerberus Protocol integration with authenticator layer
- [ ] NXS-MIND training dataset curation (session logs + docs + scripts)
- [ ] Browser choice for NXS-BROWSER.sfs (Firefox ESR vs hardened Chromium)
- [ ] WSL path for Camp 1 Windows users
- [ ] Air-gapped / offline-first node spec

---

*"Together Everyone Achieves More"*
*Sane • Simple • Secure • Stealthy • Beautiful*
