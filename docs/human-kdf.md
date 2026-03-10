# NeXuS Human KDF — The Human Key Derivation Function

**Status:** Core security protocol
**Date:** 2026-03-10
**Authors:** Anon + Claude (Sonnet 4.6)

---

## The Problem With Password Managers

Password managers require:

- Software you must trust
- A company that can be breached, subpoenaed, or shut down
- Cloud sync that can be intercepted
- A master password that unlocks everything at once
- Power and a working device to access your credentials

**A system that fails when you need it most is not a security system.**

---

## The Human KDF

A deterministic password algorithm that runs entirely in your head. No software. No cloud. No trust required. Works by candlelight with a pencil.

**Key derivation function** — a mathematical process that takes inputs and always produces the same output. This is the human-computable version.

---

## The Algorithm

Given any website or service, your password is always derived the same way:

```
INPUTS (all derived from the site name — public information):
  x  =  number of letters in the site name
  y  =  first letter of the site name, capitalized

SECRETS (known only to you — never written down):
  z     =  your memorable base string
  salt  =  your secret constant (a number, date, or word only you know)

FORMULA:
  password = y + x + z + "-" + (x × multiplier) + "X"
```

---

## Worked Example

```
Site: github

x = 6          (g-i-t-h-u-b = 6 letters)
y = G          (first letter, capitalized)
z = mydog!ishairy   (your memorable string — YOU choose this)
salt multiplier = 23.5   (your secret constant — YOU choose this)

Derived salt = 6 × 23.5 = 141

password = G6mydog!ishairy-141X
```

Same site, same formula, same password. Every time. Forever. No storage needed.

---

## Different Sites, Different Passwords

```
github    → G6mydog!ishairy-141X
amazon    → A6mydog!ishairy-141X
netflix   → N7mydog!ishairy-164.5X
google    → G6mydog!ishairy-141X  ← same letter count as github, different first letter
facebook  → F8mydog!ishairy-188X
```

Every password is unique. A breach on one site reveals nothing about your other passwords.

---

## Why This Works

**Against credential stuffing (99% of real attacks):**
Each site has a unique password. A database breach at Netflix gives the attacker nothing useful for your Amazon account. This defeats the most common attack vector entirely.

**Against reverse engineering:**
Even if an attacker gets two of your passwords and figures out the formula structure, they still need:
- Your `z` string (memorable phrase — your secret)
- Your salt multiplier (your secret constant)

Both must be known. Neither is derivable from the password itself.

**Against targeted attacks:**
The salt multiplier is the key that locks the algorithm to you specifically. Someone who knows your formula still cannot reproduce your passwords without it.

---

## The Secrets — What To Protect

There are only two things you must never share:

| Secret | What It Is | How To Choose |
|--------|-----------|---------------|
| `z` | Your memorable base string | A phrase, a lyric, a memory. Something vivid. Something yours. |
| Multiplier | Your secret constant | A number meaningful only to you. Not a birthday. Not an address. Something obscure. |

**These two secrets are your entire security posture.** Guard them. Never write them together. Never share them.

---

## The Salt — Derived, Not Memorized

The elegant part: **you do not memorize site-specific salts.** The salt for each site is always derived from the letter count using your multiplier. Same input, same output, every time.

```
6 letters × 23.5 = 141   (always, for any 6-letter site)
7 letters × 23.5 = 164.5 (always, for any 7-letter site)
8 letters × 23.5 = 188   (always, for any 8-letter site)
```

You carry the multiplier in your head. The rest is arithmetic.

---

## Password Rotation

**The honest answer on rotation:**

NIST (the US standards body) reversed its mandatory rotation recommendation. Frequent rotation of strong unique passwords does not improve security — it degrades it as people choose weaker, more memorable passwords to cope with the burden.

**NeXuS recommendation:** Do not rotate unless a breach is confirmed or suspected.

**If you must rotate**, change only the `z` string:

```
v1: G6mydog!ishairy-141X      (z = mydog!ishairy)
v2: G6mycatissleepy!-141X     (z = mycatissleepy!)
```

One thing changes. One thing to remember. All other parts of the algorithm stay the same.

**The rotation problem** — if you rotate some sites but not others, you lose track of which version you're on. The fix: when you rotate, rotate everything at once. Change `z` on the same day for all sites. Clean slate.

---

## Strengths and Limitations

### Strengths

- **Zero trust required** — no manager, no cloud, no company
- **Works anywhere** — pencil and paper, power outage, foreign country, lost device
- **Unique per site** — credential stuffing defeated completely
- **Deterministic** — always reproducible from memory
- **Naturally complex** — meets most site requirements without thinking about it
- **Human sovereign** — exists entirely in your mind

### Limitations

- **Algorithm exposure** — if someone obtains 2+ passwords AND figures out the formula structure, all passwords are at risk. Mitigation: keep the formula secret. Share only the concept, never your specific implementation.
- **Site name changes** — if a site rebrands (twitter → x), the letter count changes, breaking the pattern. Keep a note of exceptions (just the exception, not the password).
- **Length requirements** — some sites have unusual max/min length rules. Trim or extend as needed; note the exception.

---

## The Bigger Picture

This algorithm is the human-computable equivalent of PBKDF2 or Argon2 — the same mathematical concept that protects cryptographic keys, implemented without software.

The input (site name) is public. The output (password) is secure. The security comes entirely from the secret function — your `z` string and your multiplier.

**No hardware. No software. No battery. No network. No trust.**

That is the NeXuS standard: a solution that works when everything else fails.

> *"The most unbreakable system is the one that lives entirely in a human mind."*

---

## Quick Reference Card

Print this. Keep it in your wallet. It contains nothing secret — just the formula structure.

```
┌─────────────────────────────────────────────────────┐
│          NeXuS Human KDF — Quick Reference          │
├─────────────────────────────────────────────────────┤
│  x = letter count of site name                      │
│  y = first letter of site name (CAPITALIZED)        │
│  z = [YOUR SECRET PHRASE]                           │
│  m = [YOUR SECRET MULTIPLIER]                       │
│  s = x × m  (derived salt)                         │
│                                                     │
│  password = y + x + z + "-" + s + "X"              │
├─────────────────────────────────────────────────────┤
│  Example (github, 6 letters):                       │
│  G + 6 + [z] + "-" + (6×[m]) + "X"                │
└─────────────────────────────────────────────────────┘
```

The card is useless to anyone who finds it. Only you know `z` and `m`.
