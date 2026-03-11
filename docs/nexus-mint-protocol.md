# NeXuS Mint Protocol

> *We accidentally reinvented the pharmaceutical economic model. Then we made it honest. You're welcome.*

---

## The Problem With How Creators Get Paid

Let's be blunt about how the music industry works:

1. Artist creates something beautiful
2. Label takes the masters
3. Platform takes a cut
4. Lawyer takes a cut
5. Artist gets a check for $0.0032 per stream
6. Everyone else buys a yacht

The pharma industry does the same thing — inventor creates a drug, corporation takes the patent, charges $400 for a $4 pill, inventor gets a salary and a handshake. The *structure* isn't the problem. Exclusivity windows, primary markets, secondary markets, residuals — that's actually sound economics. The problem is **who controls the structure and who it serves.**

NeXuS Mint Protocol takes the same shape and flips the power back to the person who actually made the thing.

No label. No platform. No lawyer. Just math.

---

## What It Is

The **NeXuS Mint Protocol** is a trustless content monetization system. A creator mints a fixed supply of access keys to their work. Those keys are sold on a primary market the creator controls. Once sold out, a secondary market opens where buyers can resell — and the creator earns royalties forever. Every rule is enforced by a smart contract. No middlemen. No platform risk. No one can change the deal after it's signed.

It works for music, art, writing, software, film — anything that can be encrypted and stored.

---

## The Stack

```
Cold Master Key  (never online, lives under your mattress)
      ↓ derives
Project Key  (one per work — your song, your painting, your novel)
      ↓ derives
Blockchain Coin  (the minting token — one per project)
      ↓ + unique salt × N
      ↓
N Unique Access Keys  (the limited edition run)        CEK  (encrypts the content)
      ↓                                                        ↓
Blockchain Coin  🔥 BURNT                          encrypt(content, CEK) → IPFS → CID
      ↓
H(CID + burnt_coin_hash) = Original Hash  (certificate of authenticity)
      ↓
Smart Contract  (burnt coin hash + key commitments + CEK escrow + original hash)
      ↓                              ↓
Key validation                  IPFS  (encrypted content, distributed, permanent)
```

Simple. Each layer does one job. Nothing talks to what it doesn't need to.

---

## The Mint Process

### Step 1 — Encrypt Your Work

The creator encrypts their content with a **Content Encryption Key (CEK)**. The encrypted blob is uploaded to IPFS. The IPFS CID (content identifier) is a cryptographic hash — it proves the content hasn't been tampered with and can be fetched by anyone. They just can't read it without the CEK.

```
encrypt(song, CEK) → encrypted_blob → IPFS → CID
```

### Step 2 — Burn the Project Coin

The creator has a blockchain coin representing this project. It goes into the key generator. Out come N unique access keys, each derived from the project coin + a unique salt. The moment minting begins, the original project coin is **burnt**. Gone forever.

```
project_coin + salt_1 → key_1
project_coin + salt_2 → key_2
...
project_coin + salt_N → key_N
project_coin → 🔥 BURNT
```

The burn is the proof of creation. It's immutable, on-chain, timestamped. Nobody mints fake keys — they'd need to burn a coin to do it, and that burn is visible to everyone.

### Step 3 — The Original Hash

Here's where it gets elegant. A fingerprint of authenticity is created by hashing the content together with the burnt coin:

```
H(IPFS_CID + burnt_coin_hash) = original_hash
```

This is the **certificate of authenticity**. If someone downloads your encrypted song off IPFS, re-encrypts it, burns their own coin, and tries to sell a million knockoff keys at half price — their hash won't match yours. The market can verify in three lines of math who created the original. Their mint is provably derivative. Yours is provably original.

No authentication authority. No DRM server. Just math.

### Step 4 — The Smart Contract

The smart contract is deployed with everything locked in:

```
creator_address:    your wallet
content_cid:        IPFS address of encrypted content
original_hash:      H(CID + burnt_coin)
total_keys:         N (e.g. 100,000)
primary_sold:       0
nexus_fee:          N%
creator_royalty:    R%  (on secondary sales, forever)
cek_escrow:         CEK (sealed, released only to valid key holders)
```

Once deployed, **nobody changes it.** Not you, not NeXuS, not a lawyer, not a judge. The contract is the deal.

---

## The Economic Model

### Primary Market — You Eat First

All N keys must sell through the primary market before a single one can be resold. The creator sets the price. Every sale pays out:

```
Buyer pays X XMR
─────────────────────────────
NeXuS fee:    N%  →  NeXuS protocol
Creator:   (100-N)%  →  creator wallet
```

No reselling until the last key is sold. This isn't greed — it's protection. Early buyers can't undercut you before you've made your money. The exclusivity window is the creator's, not a corporation's.

### Secondary Market — Opens at Sold Out

Once all N keys are sold, the secondary market unlocks. Buyers can now resell their unburnt keys at whatever the market will bear. Every resale pays out:

```
Resale buyer pays Y XMR
─────────────────────────────────────
NeXuS fee:       N%  →  NeXuS protocol
Creator royalty: R%  →  creator wallet (forever)
Seller:    (100-N-R)%  →  seller wallet
```

The creator earns royalties on every future sale of their work. Forever. Automatically. Without a label, a lawyer, or a streaming platform taking their taste.

If the work becomes culturally significant — if that song becomes an anthem — the creator benefits from that value. Not a corporation that bought the masters for $50k twenty years ago.

### Key Scarcity — Why Unburnt Keys Have Value

An unburnt key is an unspent access token. It has intrinsic value because:

- Supply is fixed and provably scarce (burnt coin proves it)
- The content can only be decrypted once per key
- Demand may exceed supply over time

Early buyers who believe in the work can hold keys as an appreciating asset. Or use them. Their choice. The protocol doesn't care — it just enforces the rules.

---

## Accessing the Content

When a buyer is ready to decrypt:

1. Present key to smart contract
2. Contract validates: *Is this a valid key? Is it unspent?*
3. Contract burns the key, releases CEK to buyer
4. Buyer fetches encrypted blob from IPFS by CID
5. Buyer decrypts locally with CEK
6. Song plays. Nobody in the middle heard a thing.

The decryption happens **locally**. No server involved. No platform that can revoke access. No subscription that expires. You bought it, you have it.

---

## Distributed Storage — The Node Incentive

Buyers who want to contribute to the network can pin the encrypted content on their own IPFS node. The content stays encrypted — they're seeding something they can't read without their key. In return, they earn XMR micropayments for bandwidth and storage.

This is BitTorrent with economics baked in. The more popular the content, the more nodes pin it, the more resilient it becomes. The creator doesn't need to run servers forever. The network sustains itself.

---

## What the Protocol Doesn't Solve (And Is Honest About It)

Once a buyer decrypts content, they can share it. This is true. The protocol doesn't pretend otherwise.

But here's the thing — neither does any other system. DRM has been broken every single time. The difference is every other system *lies* about what it can do and punishes legitimate buyers in the process (looking at you, every major streaming platform circa 2003-forever).

NeXuS Mint Protocol is honest: **it enforces access, not redistribution.** What it does guarantee:

- You paid before you got the key ✓
- Nobody got the key twice with the same coin ✓
- The creator can't be cut out after the fact ✓
- The rules can't be changed on you after you buy ✓
- Provenance of the original work is cryptographically certain ✓

That's more than the music industry has ever offered anyone.

---

## Revocation — When Things Go Wrong

The key hierarchy exists for exactly this reason.

```
Cold Master Key  →  revokes Project Key  →  all N keys invalid
```

If the project key is compromised, the master key (which never touched the internet) issues a revocation. The smart contract registers it. All outstanding keys for that project are invalidated. Damage is contained to one project, not the creator's entire catalogue.

The master key is the nuclear option. It stays offline. It sees light exactly twice: once to generate project keys, once if something goes badly wrong.

---

## Summary

| Component | Role |
|-----------|------|
| Cold master key | Root of trust, stays offline |
| Project key | Per-work signing authority — derives the blockchain coin |
| Blockchain coin | Minting token — burnt at mint, proof of creation |
| N access keys | The product — scarce, unique, tradeable |
| Original hash | Cryptographic certificate of authenticity |
| Smart contract | The deal — immutable, trustless, automated |
| IPFS | Distributed content storage |
| CEK escrow | Content encryption key, released on valid redemption |
| XMR | Payment layer — private, final |
| DIVA chain | Ledger — tracks burns, prevents double-spend |
| NeXuS nodes | Optional storage/bandwidth network |

---

## The Philosophy

The pharmaceutical industry figured out that **structured scarcity plus residual economics** is a powerful model. They used it to extract maximum value from sick people while paying the inventors a salary.

We took the same structure and asked: *what if the person who made the thing actually kept control of it?*

That's NeXuS Mint Protocol. Same economics. Honest implementation. Creator in the driver's seat.

No label. No platform. No intermediary who adds no value and takes twenty percent.

Just the work, the math, and the people who love it enough to pay for it.

---

*Protocol status: Design phase*
*Target integration: DIVA chain + IPFS + XMR payment layer*
*Classification: NeXuS Core Protocol*
