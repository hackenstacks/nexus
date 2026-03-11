# NeXuS Two-Layer Architecture — Public and Dark

**Status:** Core architecture document
**Date:** 2026-03-10
**Authors:** Anon + Claude (Sonnet 4.6)

---

> *"The public house is on the corner. Everyone sees it. The real room is through the back, past Cerberus, for those who know the way."*

---

## The Two Layers

NeXuS operates on two distinct layers. Each has a purpose. Each has an audience. Each has a different entry requirement.

```
LAYER 1 — PUBLIC FACE
nexusnet.network

LAYER 2 — DARK INTERIOR
nexusnet.nexus
```

They point to the same world. They serve different doors into it.

---

## Layer 1 — The Public Face

**`nexusnet.network`**

```
Purpose:      The front door to the world
Audience:     Everyone — curious, new, exploring
Entry:        Open — no authentication required
Tone:         Welcoming, clear, educational
Content:      What is NeXuS, why it matters,
              how to join, documentation,
              the vision, the community
```

This is what you tell people. This is what you put on a business card. This is the URL that appears in a podcast description or a forum post.

Clean. Professional. Accessible. No barriers.

**The public face says:**
- Come learn what we are building
- Come understand why privacy matters
- Come join the community
- Come find your ring

---

## Layer 2 — The Dark Interior

**`nexusnet.nexus`**

```
Purpose:      The internal network — where the real NeXuS lives
Audience:     Authenticated members, the community, the faithful
Entry:        Cerberus guards this gate
              Three heads, all must be defeated
              Anonymous identity verified on-chain
Tone:         Sovereign, stealthy, dark
Content:      Member network, Oracle interactions,
              RIN, exchange, governance, voting,
              the forge internals, node management
```

The `.nexus` TLD is the signal itself. If you know to type `.nexus` you are already one of us. If you found it you belong here.

No outsiders. No surveillance. No clearnet tracking. No advertising. No platform watching what you do inside.

**The dark interior says:**
- You found the door
- Cerberus knows you
- Welcome home

---

## The Speakeasy Model

```
                    OUTSIDE WORLD
                          │
                          ▼
              ┌─────────────────────┐
              │   nexusnet.network  │
              │   The Public House  │
              │   Open to all       │
              │   Front door        │
              └──────────┬──────────┘
                         │
                    Know the way?
                         │
                         ▼
              ┌─────────────────────┐
              │    🐕🐕🐕 CERBERUS  │
              │    Three heads      │
              │    All must pass    │
              └──────────┬──────────┘
                         │
                   Authenticated
                         │
                         ▼
              ┌─────────────────────┐
              │   nexusnet.nexus    │
              │   The Dark Interior │
              │   Members only      │
              │   Sovereign space   │
              └─────────────────────┘
```

---

## The `.nexus` TLD as Signal

The choice of `.nexus` as the internal domain is intentional and architectural.

- A normie browsing the internet does not type `.nexus`
- Search engines do not surface it the same way
- It requires knowing what you are looking for
- Finding it means you already understand the project

**The TLD is the first gate.** Before Cerberus even appears, the domain itself filters for intent.

---

## nexus.locker — The Vault

Inside the dark interior lives the vault.

```
nexus.locker        — the key vault
                      where identity is stored
                      where credentials live
                      where the Cerberus Protocol outputs are managed
                      sovereign, encrypted, yours alone
```

```
LAYER 1             nexusnet.network    public face
LAYER 2             nexusnet.nexus      dark interior
LAYER 2 — VAULT     nexus.locker        the keys to everything
```

The locker is where:
- Anonymous identities are managed
- Oracle credentials are stored
- Ring memberships are held
- Cerberus Protocol passwords are derived and managed
- XMR wallet connections are secured
- Everything that is yours and only yours lives

**Cerberus guards the gate. The locker is what is behind it.**

---

## The Domain Roles — Full Map

```
PUBLIC LAYER:
nexusnet.network    — home base, public face, front door
nexusnet.wiki       — knowledge base, open documentation
nexusnet.blog       — voice, content, podcast home
nexusnet.help       — support, onboarding, guides
nexusnet.foundation — governance, non-profit, mission

DARK INTERIOR:
nexusnet.nexus      — authenticated member network
nexusnet.exchange   — XMR payment rail, Oracle economy
nexusnet.forum      — member discussion, deep community
nexusforge.nexus    — the forge, builder internals

THE VAULT:
nexus.locker        — identity vault, key management

THE FORGE:
nexusforge.nexus    — where NeXuS gets built
nexusforge.quest    — builder challenges, quests

SPECIALIZED:
nexusnet.studio     — AI entities, ratings, media
nexusnet.email      — email backbone
nexusnetwork.cloud  — cloud services layer
```

---

## Why This Architecture Matters

**Single layer systems have one failure point.**

If `nexusnet.network` is seized, censored, or blocked — `nexusnet.nexus` continues. The dark interior operates independently. The community survives.

If `nexusnet.nexus` is discovered and attacked — the public face continues welcoming new members. The forge keeps building.

**The two layers protect each other by being separate.**

The public face is the visible target — it absorbs attention. The dark interior is the real infrastructure — it operates in the shadows.

This is not paranoia. This is architecture. The same principle that makes Tor resilient, that makes I2P survive, that makes Medusa route around censorship.

**Redundancy is sovereignty. Separation is survival.**

---

## Entry Philosophy

```
nexusnet.network    anyone can enter
                    no judgment, no barrier
                    come as you are

nexusnet.nexus      Cerberus decides
                    identity verified on-chain
                    anonymous but authenticated
                    the community knows you
                    even if the world does not

nexus.locker        you and you alone
                    the math is the key
                    Cerberus Protocol guards this gate
                    three heads, all yours
```

---

> *"Two doors. One world. The public face welcomes everyone. The dark interior belongs to those who found their way through. Both are NeXuS. Both are necessary. Neither is complete without the other."*
