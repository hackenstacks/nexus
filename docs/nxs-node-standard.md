# NeXuS Node Standard
**Status:** Draft
**Created:** 2026-03-21
**Principles:** Sane • Simple • Secure • Stealthy • Beautiful

---

## 1. The Core Problem We Are Solving

Most systems fail in one of three ways:
- **Too trusting** — binary blobs, prebuilt images, "just trust us"
- **Too complex** — plugin hell, configuration drift, no known-good state
- **Too fragile** — no fallback, no reset, no resilience

NeXuS solves all three with one coherent architecture.

---

## 2. The Filesystem Foundation

### SquashFS + FUSE + Overlay

```
core.sfs  (read-only, compressed, immutable)
     +
overlay/  (writable, user/node-specific changes only)
     =
merged/   (FUSE mount — what the app or OS sees)
```

- **Core never changes** after it is built
- **All user changes** go to the overlay layer only
- **Reset** = wipe overlay, core is instantly pristine
- **No reinstall** ever needed to reach a known-good state

### The Sane Default Principle

> Every NeXuS tool ships with a defined, documented, restorable default state.
> If you cannot define a clean default, the tool is not designed yet.

The default is not an afterthought — it is the first design decision.

### Delta Snapshots

```
core.sfs              ← never snapshot, never changes
overlay/
  snap-001/           ← first user changes
  snap-002/           ← delta from 001 only
  snap-003/           ← delta from 002 only
  current → snap-003  ← pointer, trivial to move
```

- Core consumes **zero bytes** in every snapshot
- Rollback = move the `current` pointer
- Storage cost approaches zero over time
- Updates = replace `core.sfs` only, overlay survives untouched

---

## 3. The Boot Architecture

### iPXE Network Boot

```
bare metal (any machine)
     ↓
iPXE  (chip, USB, or network card)
     ↓
fetch core.sfs into RAM
     ↓
overlay mounts on top
     ↓
NeXuS node is live
```

The core OS lives in RAM. No disk required. No forensic trace by default. **Stealthy by architecture, not by configuration.**

### Fallback Chain (Mesh Resilience)

```
1. LAN server         (fast, private, trusted)
     ↓ fail
2. USB                (portable, always available)
     ↓ fail
3. Another NeXuS node (peer-to-peer, mesh)
     ↓ fail
4. Internet server    (last resort, still verified by hash)
```

The mesh fallback is the key insight — **any NeXuS node can serve any other NeXuS node.** No central server required. The network grows stronger as nodes are added. Round Table principle applied to infrastructure.

---

## 4. No Prebuilt Blobs — Ever

### The Trust Model

```
I trust what I can read.
I can read the build script.
The build script produces the core.
Therefore I trust the core.
```

No certificate authority. No signed blob from a stranger. No "just trust us."

### What This Means

- No binary blobs in any NeXuS repo — ever
- `nexus-build.sh` produces `core.sfs` from source
- Same script + same source = same hash, every time (reproducible builds)
- The build script IS the documentation
- Anyone can audit the full supply chain end to end

This is stronger than Nix or Guix — both ship prebuilt binary substitutes by default. NeXuS builds from source. The tradeoff is build time. The gain is sovereignty.

---

## 5. The NXS Script Layer

### Naming Convention

```
NXS-CORE-RO          ← read-only base, always present, always first
NXS-TERM-TMUX        ← terminal only (kmscon or bare), + tmux
NXS-CAGE-FOOT        ← minimal Wayland, single application focus
NXS-LABWC            ← lightweight Wayland compositor
NXS-HYPR             ← full Hyprland desktop experience
NXS-DEV              ← development layer (editors, compilers, tools)
NXS-PRIVACY          ← Tor, I2P, Medusa proxy stack
NXS-GAMING           ← Cataclysm DDA, gaming tools
```

### How It Composes

```
NXS-CORE-RO  (mandatory base)
     +
[one desktop/terminal choice]
     +
[zero or more optional layers]
     ↓
build script runs
     ↓
core.sfs produced fresh, hash verified
```

User selects their stack. Script builds it. The output is always auditable, always reproducible.

### The Plugin Philosophy

> A tool should do one thing and do it well.
> Complex problems, simple solutions.
> The core is sacred. Extensions are chosen, not accumulated.

The difference between NeXuS plugins and editor plugin hell:
- **Plugin hell** — marketplace, temptation, accumulation, drift, breakage
- **NeXuS extensions** — deliberate, published, audited, rated, versioned

Every extension is a published script. Every script is readable. Every read is rateable.

---

## 6. The Community Trust Layer

### The Problem It Solves

Open source has always had a gap:
- Code is transparent — **but only to those who can read it**
- Non-technical users are left choosing between blind trust and exclusion

NeXuS bridges this gap without dumbing down the code.

### How It Works

```
Developer publishes NXS-* script
     ↓
Community members who can read code review it
     ↓
They leave a rated comment (1-5 stars + explanation)
     ↓
Non-technical user sees aggregate rating + plain-language summaries
     ↓
They run the script with confidence they earned, not blind faith
```

### Review Standards

A quality review covers:
- **What the script does** — plain language summary
- **What it does NOT do** — no hidden behavior
- **Blob check** — confirms no prebuilt binaries pulled in
- **Dependency audit** — what external things it fetches and why
- **Rating** — 1 to 5 stars with reasoning

### Incentive

Reviewers earn **Nexium** for quality, verified reviews.
The technical-to-non-technical knowledge gap closes over time as users read comments and learn.

---

## 7. Package Manager Position

### The Guix vs Nix Question

| | Guix | Nix |
|---|---|---|
| FOSS purity | 100% FSF-approved | Allows non-free |
| Philosophy fit | NeXuS soul | NeXuS pragmatics |
| Corp adoption | Hard sell | Already there |
| Reproducibility | Excellent | Excellent |

### The Resolution

**NeXuS sits above both.**

The node boot standard, squashfs core, overlay system, and script layer are package-manager agnostic. A Guix-built `core.sfs` and a Nix-built `core.sfs` are both valid NeXuS nodes.

- Community nodes → Guix-built cores (pure FOSS, aligned with NeXuS values)
- Corporate nodes → Nix-built cores (pragmatic, wider adoption)

Same NeXuS standard underneath. Same trust model. Same reset guarantee.

---

## 8. The Full Node Lifecycle

```
INSTALL   → iPXE fetches core.sfs, empty overlay mounts
USE       → all writes go to overlay only, core untouched
SNAPSHOT  → delta of overlay changes only (core = 0 bytes)
RESET     → overlay cleared, core.sfs takes over instantly
UPDATE    → new core.sfs drops in, overlay survives unchanged
REMOVE    → unmount, delete overlay (core shared, not duplicated)
SERVE     → this node can now be a fallback for other nodes
```

---

## 9. Core Principles Reaffirmed

Every decision in this standard traces back to the five:

| Principle | How It Manifests |
|-----------|-----------------|
| **Sane** | Default state is defined, documented, always restorable |
| **Simple** | One script builds the system, one command resets it |
| **Secure** | No blobs, read-only core, community-audited scripts |
| **Stealthy** | RAM-resident OS, no disk writes by default, no trace |
| **Beautiful** | Composable layers, clean naming, user chooses their aesthetic |

---

## 10. Pending Design Decisions

- [ ] Hash verification protocol for `core.sfs` across nodes
- [ ] Nexium reward formula for community reviewers
- [ ] Minimum review count before a script is considered trusted
- [ ] Versioning scheme for NXS-* scripts
- [ ] How nodes advertise themselves as fallback peers
- [ ] Offline-first consideration for air-gapped NeXuS nodes

---

*"Together Everyone Achieves More"*
*Sane • Simple • Secure • Stealthy • Beautiful*
