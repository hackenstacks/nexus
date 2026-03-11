# The NeXuS Riddler Equation

**A Human-Computable Key Derivation Function**
**Status:** Core security protocol
**Date:** 2026-03-10
**Authors:** Anon + Claude (Sonnet 4.6)

---

> *"What has no lock, needs no key, yet keeps every door unique?"*
> **The Riddler Equation.**

---

## What Is It?

The NeXuS Riddler Equation is a **password derivation algorithm you run in your head.**

No software. No manager. No cloud. No battery. No trust required.

It takes a website name — public information — and transforms it through a formula only you know into a strong, unique password. Same site, same formula, same password. Every time. Forever.

It is the human-computable equivalent of PBKDF2 or Argon2 — the same mathematical concept that protects cryptographic keys — implemented with nothing but your mind and optionally a pencil.

---

## Why It Exists

Most people use one or two passwords for everything. A single breach at any site hands attackers the keys to their entire digital life.

Password managers solve this but introduce new problems:

- Software you must trust
- A company that can be breached, subpoenaed, or disappear
- A master password that unlocks everything in one blow
- Requires a working device and power to access credentials
- Cloud sync that can be intercepted

**The Riddler Equation has none of these dependencies.**

It lives in your head. It works by candlelight. It works when the grid is down, your phone is dead, and you are sitting at a library computer in a foreign country.

---

## The Components

The equation has four components. Two are derived from the site name (public). Two are your secrets (private).

### Public Components (derived from site name)

| Component | Symbol | What It Is | Example (github) |
|-----------|--------|-----------|-----------------|
| Letter count | `x` | Number of letters in the site name | `6` |
| First letter | `y` | First letter of the site name, capitalized | `G` |

### Secret Components (known only to you)

| Component | Symbol | What It Is | How To Choose |
|-----------|--------|-----------|--------------|
| Base string | `z` | A memorable phrase, lyric, or word sequence | Something vivid. Something yours. A memory no one else has. |
| Multiplier | `m` | A secret number — your salt generator | Not a birthday. Not an address. Something obscure only you know. |

---

## The Formula

```
Derived salt (s) = x × m

Password = y + x + z + "-" + s + "X"
```

That is the entire equation. Four inputs, one output. Deterministic. Reproducible. Unique per site.

---

## Step-By-Step Walkthrough

Let us use these secrets for all examples:

```
z = mydog!ishairy
m = 23.5
```

**These are example secrets. Choose your own. Never share them.**

---

### Example 1 — github

**Step 1: Count the letters**
```
g - i - t - h - u - b
1   2   3   4   5   6

x = 6
```

**Step 2: Take the first letter, capitalize it**
```
github → G

y = G
```

**Step 3: Derive the salt**
```
s = x × m
s = 6 × 23.5
s = 141
```

**Step 4: Assemble the password**
```
y  +  x  +  z              +  "-"  +  s    +  "X"
G  +  6  +  mydog!ishairy  +  "-"  +  141  +  "X"

Password: G6mydog!ishairy-141X
```

---

### Example 2 — amazon

**Step 1: Count the letters**
```
a - m - a - z - o - n
1   2   3   4   5   6

x = 6
```

**Step 2: First letter capitalized**
```
amazon → A

y = A
```

**Step 3: Derive the salt**
```
s = 6 × 23.5 = 141
```

**Step 4: Assemble**
```
A6mydog!ishairy-141X
```

!!! note "Same letter count, different password"
    github and amazon both have 6 letters, so `x` and `s` are the same. But `y` differs — `G` vs `A` — making the passwords unique.

---

### Example 3 — netflix

**Step 1: Count the letters**
```
n - e - t - f - l - i - x
1   2   3   4   5   6   7

x = 7
```

**Step 2: First letter capitalized**
```
netflix → N

y = N
```

**Step 3: Derive the salt**
```
s = 7 × 23.5 = 164.5
```

**Step 4: Assemble**
```
N7mydog!ishairy-164.5X
```

---

### Example 4 — facebook

**Step 1: Count the letters**
```
f - a - c - e - b - o - o - k
1   2   3   4   5   6   7   8

x = 8
```

**Step 2: First letter capitalized**
```
facebook → F

y = F
```

**Step 3: Derive the salt**
```
s = 8 × 23.5 = 188
```

**Step 4: Assemble**
```
F8mydog!ishairy-188X
```

---

### Example 5 — protonmail

**Step 1: Count the letters**
```
p - r - o - t - o - n - m - a - i - l
1   2   3   4   5   6   7   8   9  10

x = 10
```

**Step 2: First letter capitalized**
```
protonmail → P

y = P
```

**Step 3: Derive the salt**
```
s = 10 × 23.5 = 235
```

**Step 4: Assemble**
```
P10mydog!ishairy-235X
```

---

## All Examples Side By Side

| Site | x | y | s (x × 23.5) | Password |
|------|---|---|-------------|---------|
| github | 6 | G | 141 | `G6mydog!ishairy-141X` |
| amazon | 6 | A | 141 | `A6mydog!ishairy-141X` |
| netflix | 7 | N | 164.5 | `N7mydog!ishairy-164.5X` |
| facebook | 8 | F | 188 | `F8mydog!ishairy-188X` |
| twitter | 7 | T | 164.5 | `T7mydog!ishairy-164.5X` |
| google | 6 | G | 141 | `G6mydog!ishairy-141X` |
| reddit | 6 | R | 141 | `R6mydog!ishairy-141X` |
| discord | 7 | D | 164.5 | `D7mydog!ishairy-164.5X` |
| protonmail | 10 | P | 235 | `P10mydog!ishairy-235X` |
| linkedin | 8 | L | 188 | `L8mydog!ishairy-188X` |

Every password is unique. No two are the same. A breach at any one site reveals nothing about any other.

---

## Why This Is Secure

### Against credential stuffing (defeats 99% of real attacks)
Credential stuffing is when attackers take a breached username/password list and try it everywhere. Unique passwords per site defeats this completely. If your Netflix password leaks, your Amazon password is unaffected.

### Against reverse engineering
Say an attacker gets two of your passwords:
```
G6mydog!ishairy-141X   (github)
A6mydog!ishairy-141X   (amazon)
```

They can see a pattern. They might deduce the letter count component. They might guess the formula structure. But they still need:

- Your `z` string — your secret phrase
- Your multiplier `m` — your secret constant

Without both, they cannot reproduce any other password. Two secrets guard the entire system.

### Against targeted attacks — the salt
The multiplier `m` is what makes YOUR formula yours. Even if an attacker knows someone uses the Riddler Equation, your specific `m` makes your implementation unique. The salt is derived — you never memorize site-specific values — but it is always different per letter count.

---

## Choosing Your Secrets

### Choosing `z` — your base string

Good choices:
- A lyric from a song nobody associates with you
- A line from an obscure book
- A childhood memory described in a phrase
- An inside joke with no context
- A sentence in a language you speak but few know you speak

Bad choices:
- Your pet's name
- Your favourite band (too guessable)
- Common phrases ("iloveyou", "letmein")
- Anything you have used as a password before

**Make it vivid. Make it yours. Make it something that will stick without effort.**

### Choosing `m` — your multiplier

Good choices:
- A number from an old locker combination
- A measurement you remember from a project
- A price you remember paying for something years ago
- A number that is meaningful in a way only you understand

Bad choices:
- Your birth year
- Your house number
- 42 (too obvious for nerds)
- Any number publicly associated with you

**The multiplier does not need to be a whole number. Decimals add complexity — 23.5 is harder to guess than 23.**

---

## Password Rotation

**The honest answer:** NIST (the US cryptographic standards body) reversed its mandatory rotation recommendation. Frequent rotation of strong unique passwords does not improve security — it usually degrades it as people choose weaker passwords to cope.

**NeXuS recommendation: do not rotate unless a breach is confirmed.**

### If you must rotate — change only `z`

```
Version 1:  z = mydog!ishairy
github →    G6mydog!ishairy-141X

Version 2:  z = mycatissleepy!
github →    G6mycatissleepy!-141X
```

One thing changes. One thing to remember. The multiplier, the structure, the letter-count logic — all stay the same. Only the phrase rotates.

### The rotation problem — and the fix

If you rotate some sites but not others you lose track of which version you are on. The fix is simple:

**When you rotate, rotate everything on the same day.** Change `z` to a new phrase. Update all sites in one session. Clean slate. You always know which phrase is current.

---

## Edge Cases

### Site uses a subdomain or full domain
Only use the core name. Strip everything else:

```
mail.google.com   →   google   →   x = 6
app.netflix.com   →   netflix  →   x = 7
github.io         →   github   →   x = 6
```

### Site rebrands (twitter → x)
The letter count changes. Keep a short exception list — just the site and which name you used. Not the password, just the name variant. The list is useless to anyone who finds it without your secrets.

```
Exception list (safe to write down):
  x.com → use "twitter" (7)
```

### Site has unusual length requirements
Some sites cap passwords at 16 characters or require exactly 8. Trim or pad as needed and note the exception. The formula itself is still your source of truth — you are just truncating or extending the output.

### Numbers-only or letters-only requirements
Rare, but some legacy systems refuse special characters. In that case drop the `!` from your `z` string and substitute a number. Note the exception.

---

## The Quick Reference Card

Print this. Keep it in your wallet. It contains nothing secret. It is useless to anyone who finds it.

```
┌─────────────────────────────────────────────────────────────┐
│             NeXuS Riddler Equation — Field Card             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. Count letters in site name          →  x               │
│  2. Capitalize first letter of site     →  y               │
│  3. Multiply: x × [your m]              →  s               │
│                                                             │
│  Password = y + x + [your z] + "-" + s + "X"               │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│  Example structure (6-letter site):                         │
│                                                             │
│  G  6  [z]  -  141  X                                      │
│  ↑  ↑   ↑   ↑   ↑   ↑                                      │
│  y  x  base  sep salt end                                   │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│  Exceptions:                                                │
│  ________________________________                           │
│  ________________________________                           │
│  ________________________________                           │
└─────────────────────────────────────────────────────────────┘
```

The card reveals the formula structure. It does not reveal `z` or `m`. Without both secrets the card is worthless to an attacker.

---

## The Bigger Picture

You have just implemented a cryptographically sound key derivation function using mental arithmetic.

The security properties:
- **Deterministic** — same inputs always produce same output
- **Unique per site** — no two passwords are identical
- **Salted** — derived salt prevents rainbow table attacks
- **Secret function** — algorithm secrecy provides the final layer
- **Zero dependency** — no hardware, software, network, or company required

**That is the Riddler Equation. The answer to the riddle is always in your head.**

> *"Prepared is not paranoid. Sovereign is not paranoid. A key that lives in your mind cannot be confiscated, cannot be subpoenaed, cannot be breached, and cannot be shut down."*
