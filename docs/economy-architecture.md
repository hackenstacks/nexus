# NeXuS Economy Architecture
**Status:** Design Draft
**Date:** 2026-03-08
**Authors:** Anon + Claude (Sonnet 4.6)

---

## 1. The Vision

NeXuS is an alternative economic infrastructure for individuals. As AI displaces workers, NeXuS routes value back to individuals — their hardware, their content, their data, their participation generates income that flows directly to them. No intermediary capturing the margin.

**Core principle:** Every node that joins makes the network more valuable for every other node.

**NeXuS value is the network. Users make up the network. Therefore users ARE the value.**

---

## 1.1 The Foundational Economic Principle

### REVERSED.

**Users create the system.**
**The system creates the economy.**
**The system is in debt to the user and shall mathematically pay out.**

```
Traditional:              NeXuS:
User → owes → platform    Platform → owes → user
```

```
Users create the system
          ↓
System creates the economy
          ↓
Economy rewards the users
          ↓
Users grow the system
          ↓
No platform. No company. No intermediary in that chain.
```

Not a reward. Not a gift. Not discretionary. A debt being settled.

**What this means:**
- Contribution is measured by protocol
- Debt calculated by algorithm
- Payment executed by smart contract
- No human in the loop, no discretion, no withholding
- The math is the law

**Anti-plutocracy is non-negotiable:**
```
Cannot buy voting weight     — weight = contribution %, not wealth
Cannot buy credits directly  — only earned through contribution
Cannot hoard for control     — elastic supply dilutes accumulation
Cannot buy your way in       — % based, same rule for everyone
Rich node earns more total   — earns same RATE as any other node
     ↓
Wealth buys no advantage
Only contribution matters
Death of NeXuS = the rich gaining control
This must be made mathematically impossible
```

**The revered credit system:**
```
Trust through mathematics — not faith in a company
Credits earned = network debt to user
Payout guaranteed by protocol
Open, auditable, immutable
     ↓
The system owes YOU
```

**Fee philosophy (aligned with DIVA):**
```
Small fee for one reason only — network survival
Not for profit
Not for extraction
Not for personal payment
The creator does not expect payment
Infrastructure lives — mission continues
```

---

---

## 2. Blockchain Stack

### 2.1 NeXuS Chain (Base Layer)

| Property | Decision | Reason |
|----------|----------|--------|
| Base | Fork **Monero** (MIT license) | Clean legal path — MIT permits production use, no permission needed |
| Algorithm | **AstroBWT** | Keeps mining inside NeXuS ecosystem — external Monero pools cannot dominate or 51% attack |
| Privacy | RingCT + stealth addresses + ring signatures | Transaction amounts and participants hidden by default |
| Smart contracts | WASM-based VM (implemented from scratch) | Portable, sandboxed, language agnostic |
| Block structure | DAG (implemented from scratch, DERO-inspired) | No mempool congestion, faster finality than linear chain |
| Security | dPoW (delayed Proof of Work) | Bitcoin checkpoints secure chain even at low hashrate |

**License decision:**
- DERO Research License — production use blocked, no commercial license exists at a known URL
- DENERO is a fork of DERO — same Research License, same block
- Both projects are valid architecture inspiration (Research License Section III.B Residual Rights permits using knowledge and concepts)
- Monero is MIT licensed — fork freely, no permission, no California jurisdiction, no termination risk
- Architecture concepts (DAG, HE, smart contract model) implemented independently, inspired by DERO research

### 2.2 Delayed Proof of Work (dPoW)

Borrowed from Komodo. Notary nodes periodically write NeXuS chain state into a Bitcoin transaction. Attack window reduced to minutes between notarizations regardless of NeXuS hashrate.

```
NeXuS chain  →  Notary nodes snapshot block hash
                     ↓
             Bitcoin transaction (small fee)
                     ↓
             Checkpoint immovable — secured by Bitcoin hashrate
```

- Notary nodes elected by governance (council of nodes)
- Cost: one Bitcoin transaction every ~10 minutes (pennies)
- Never turn off — cheap insurance even at high hashrate

### 2.3 Security Progression

```
Low hashrate period    →  dPoW doing heavy lifting
Medium hashrate        →  both sharing security job
High hashrate          →  own miners secure chain, dPoW is backup
```

---

## 3. DIVA Integration

### 3.0 What DIVA Actually Is

**Mission:** "Free banking technology for everyone. Your identity kept separate from your transaction data. Secure without central infrastructure."

DIVA.EXCHANGE is not a coin, not a token, not a platform. It is **free banking infrastructure** — permissionless, privacy-by-design, fully anonymous, built for everyone including those with no access to traditional banking.

```
DIVA philosophy:    "Only you determine your values"
NeXuS philosophy:   "Together Everyone Achieves More for individual freedom"
     ↓
Same first principles — complementary solutions
DIVA solved the exchange and banking layer
NeXuS is building the economy that runs on top of it
```

**Technical architecture:**
- Consensus: Weighted PBFT + PoS (communication-bound, not computation-bound)
- Chain built through peer communication, not mining
- Fast finality, lightweight, low energy
- Permissionless and leaderless — no central authority
- I2P native — peers build tunnels via Challenge/Auth (Sodium crypto library)
- Round-based state machine — each round produces one block on demand
- Per round: each peer proposes ONE transaction, votes on stack of proposals
- 2/3 of total stake must agree → block created (PBFT Byzantine fault tolerance)
- No native coin or token issued
- Financed through university partnerships + donations
- API: HTTP REST on port 17468 + WebSocket for live updates

**No coin = important design question for the 15th:**
- How is stake acquired and weighted?
- Can stake be earned through contribution or only purchased?
- Are there any fees on chain transactions?
- How are nodes incentivized to stay honest without a coin reward?
- If stake must be bought in → contradicts both projects' philosophy
- If stake is earned through contribution → maps perfectly onto NeXuS design

### 3.1 Two Chains, Two Jobs

| | NeXuS Chain | DIVA Chain |
|---|---|---|
| Purpose | Private value transfer + enforcement | Coordination, proofs, social layer |
| Consensus | PoW (AstroBWT) | Weighted PBFT + PoS |
| Speed | ~18 sec (DAG) | On demand, fast finality |
| Privacy | RingCT built-in | Privacy by design, I2P native |
| Coin | NeXuS token (mined) | No coin issued |
| Smart contracts | WASM VM — value enforcement | Namespace state machines — coordination |
| Use for | Token transfers, royalties, supply caps, stake/loan | Storage proofs, badges, reputation votes, exchange matching, governance proposals |

**Division of labor principle:**
```
Moves money → NeXuS chain (mining-secured, immutable)
Coordinates / proves / records → DIVA chain (fast finality, quorum-validated)
```

### 3.2 DIVA Smart Contract Model

DIVA's transaction system is a form of smart contract — not Turing-complete (not EVM), but sufficient for coordination. Each namespace is a scoped state machine. Rules enforced by 2/3 quorum of peers.

```
DIVA "contract" structure:
PUT /tx  →  [{"command":"data", "ns":"nexus:badges:forge-master", "d":"<node-pubkey>"}]
                    ↓
             2/3 quorum validates
                    ↓
             State committed — immutable, auditable
```

**What NeXuS uses DIVA contracts for:**

```
nexus:badges          → badge issuance, soul-bound records
nexus:reputation      → peer vote tallies per seller pseudonym
nexus:storage         → storage deal commitments + proof records
nexus:exchange        → order book entries, trade matching
nexus:governance      → proposal posting, vote recording
```

Each namespace has defined rules. Network enforces them. No single authority.

### 3.2 Storage Deal Flow

```
1. Node advertises storage availability
   → DIVA chain  (PUT /tx, fast quorum)

2. Client finds offer, locks payment
   → NeXuS chain smart contract (HTLC)

3. Node stores data on IPFS, pins it

4. Periodic storage proofs posted
   → DIVA chain  (challenge-response, quorum confirms)

5. Notary nodes attest proof to NeXuS chain
   → same notary nodes as dPoW, dual purpose

6. Smart contract releases payment
   → NeXuS tokens flow to storage provider
```

### 3.3 DIVA Exchange (Liquidity Layer)

DIVA is first and foremost a decentralized exchange — I2P-native, peer-to-peer, no custody, atomic swap settlement.

```
NeXuS node earns tokens
     ↓
Posts offer on DIVA exchange (over I2P, anonymous)
     ↓
Another node takes trade (NeXuS ↔ XMR, NeXuS ↔ DERO, etc.)
     ↓
HTLC locked on NeXuS chain (seller's tokens)
HTLC locked on other chain  (buyer's tokens)
     ↓
Alice reveals secret → claims XMR on Monero chain
Bob uses same secret → claims NeXuS on NeXuS chain
     ↓
Atomic — both settle or neither does
DIVA exchange never holds custody
```

**For the March 2026 DIVA meeting:** NeXuS brings real economic activity to DIVA exchange. Every NeXuS node is a potential DIVA exchange user. Mutual benefit — not just "we want to use your thing."

---

## 4. Full Architecture Stack

```
┌──────────────────────────────────────────────────────┐
│  NeXuS Wallet       — command center / browser       │  interface
├──────────────────────────────────────────────────────┤
│  NeXuS Townhall     — marketplace + badges + rep     │  social
├──────────────────────────────────────────────────────┤
│  DIVA Exchange      — anonymous P2P trading          │  liquidity
├──────────────────────────────────────────────────────┤
│  NeXuS Chain        — private value + enforcement    │  value
├──────────────────────────────────────────────────────┤
│  DIVA Chain         — coordination, proofs, social   │  coordination
├──────────────────────────────────────────────────────┤
│  IPFS               — actual data storage            │  storage
├──────────────────────────────────────────────────────┤
│  I2P / Tor / Reticulum — anonymous transport         │  network
├──────────────────────────────────────────────────────┤
│  Alpine Linux hardened — the node itself             │  hardware
└──────────────────────────────────────────────────────┘
```

---

## 5. Node Economy

### 5.1 What a Node Earns

**Passive (just by running):**
- CPU cycles → mining NeXuS tokens (RandomX)
- Storage space → pinning IPFS content, earning per GB
- Bandwidth → routing traffic, earning per MB
- Relay services → I2P/Tor relay, earning tokens

**Active (user participates):**
- Digital goods sales → sell files, music, art, software
- Review/validation → earn for verifying content quality
- Data verification → confirm accuracy of information
- Compute tasks → run jobs for other nodes

### 5.2 Master Key → Programmable Keys

```
Master Key (cold, never online)
     ↓
HD derivation (BIP32-style)
     ↓
├── Creator key      (persistent pseudonym — royalties forever)
├── Reputation key   (Townhall track record)
├── Governance key   (voting rights, contribution-weighted)
├── Session keys     (rotate monthly)
└── Burn keys        (one transaction, then dead)
```

Child key types:
- **Access key** — unlocks a specific file/content
- **License key** — non-transferable, tied to buyer's master key
- **Gift key** — transferable once, then locks
- **Rental key** — expires after time period
- **Resale key** — fully transferable, creator gets royalty on each transfer

### 5.3 Credit Distribution

```
10% of all transaction fees → Main Chain Bank
     (programmable wallet controlled by Council of Nodes)
          ↓
     Distributed:
     ├── % → network development and maintenance
     ├── % → active contributors and committers
     └── % → community participation rewards
```

Percentages determined by governance vote.

---

## 6. NeXuS Townhall (Marketplace)

Built-in discovery marketplace — Craigslist meets Bandcamp meets eBay, all running inside NeXuS.

### 6.1 Listing Tiers

```
Standard listing:   free, standard placement, NeXuS takes 3% per sale
Boosted listing:    offer 5% to NeXuS → moves up the list
                    NeXuS owns 5% of every future sale of that item
```

### 6.2 Digital Goods Fee Structure

**Primary market (creator selling directly):**

```
Standard:  97% → Creator,  3% → NeXuS
Boosted:   95% → Creator,  5% → NeXuS
```

**Secondary market (resale between users):**

```
90% → Seller (current owner)
 5% → Creator (perpetual royalty, enforced by smart contract)
 5% → NeXuS network
```

### 6.3 Supply Caps

Creator sets at mint time — **immutable, enforced by smart contract:**

```
Example: musician mints 100,000 copies of a song
├── Each copy = programmable access key on NeXuS chain
├── Smart contract enforces cap — cannot be changed or exceeded
├── Once 100k sold, no new primary copies exist
└── Only secondary market remains (scarcity drives value)
```

Creator controls rights layers independently:
```
Song
├── Listening rights    → the 100k copies
├── Sync rights         → use in film/video (separate listing, separate cap)
├── Remix rights        → license to remix (separate listing)
└── Master ownership    → never for sale, or separate deal
```

### 6.4 Scarcity Creates Long-Term Value

```
100k copies sold out
     ↓
Song gets popular
     ↓
Secondary market price rises
     ↓
Creator earns 5% of every resale forever
NeXuS earns 5% of every resale forever
     ↓
Both benefit from the song's success long term
```

---

## 7. Wallet — The NeXuS Command Center

The wallet is not a key management tool. It is the **browser into NeXuS** — the user's complete interface to the network, their identity, their assets, their node, and every protocol NeXuS runs.

**Design principle:** Extremely simple surface. Right amount of information. Nothing hidden that matters, nothing shown that doesn't.

**Privacy principle:** Privacy is handled at the protocol layer automatically (RingCT, stealth addresses, ring signatures make transactions unlinkable on-chain). The wallet does not retain local transaction history. After a transaction confirms, no log is kept. The chain is private by design — nothing to reconstruct from outside, nothing stored inside.

---

### 7.1 The Full Interface Map

```
┌─────────────────────────────────────────────────────────────┐
│  NeXuS Wallet                                               │
├──────────────┬──────────────────────────────────────────────┤
│              │  DASHBOARD                                   │
│  Navigation  │  NeXuS: [balance]   XMR: [balance]          │
│              │  BTC: [balance]     Earnings today: [amt]   │
│  Dashboard   │  Node: [online]     Staked: [amt]           │
│  Assets      │                                              │
│  Swap        ├──────────────────────────────────────────────┤
│  Mint/Forge  │  ASSETS                                      │
│  Finance     │  Tokens | NFTs | Badges                     │
│  Command     │                                              │
│  Center      ├──────────────────────────────────────────────┤
│  Keys        │  [content area]                              │
└──────────────┴──────────────────────────────────────────────┘
```

---

### 7.2 Dashboard

What the user sees the moment they open the wallet:

```
┌─────────────────────────────────────────────────────┐
│  NeXuS     [balance]    BTC    [balance]            │
│  XMR       [balance]    Earning [rate/day]          │
├─────────────────────────────────────────────────────┤
│  Node: ONLINE    CPU: 12%    Storage: 47GB active   │
│  Uptime: 99.2%   Staked: [amt]   Loaned: [amt]      │
└─────────────────────────────────────────────────────┘
```

No transaction history visible by default. What you have. What you earn. What your node is doing. That is all.

---

### 7.3 Assets

Three tabs:

**Tokens** — NeXuS, XMR, BTC balances with send/receive per asset.

**NFT Wallet** — every digital good owned: music, art, software, access keys, licenses. Shows item name, creator pseudonym, what rights are held (listen / sync / remix / resale), current floor price on secondary market if listed.

**Badges** — soul-bound reputation tokens. Cannot be transferred, bought, or sold. Two sources: **protocol-issued** (automatic, earned by action) and **peer-voted** (thumbs up/down from the community). Displayed publicly on your reputation pseudonym. Street cred visible to the whole network, real identity hidden.

```
Protocol-issued badges (automatic — earned by on-chain action):
┌──────────────────┬──────────────────────────────┐
│  Pioneer         │  Early node — first 10,000   │
│  Relay Master    │  1TB+ bandwidth contributed  │
│  Vault Keeper    │  100GB+ storage proven       │
│  Forge Master    │  50+ original items minted   │
│  Marketplace     │  100+ sales completed        │
│  Governance      │  Voted in 10+ proposals      │
│  Deep Roots      │  Node online 1+ year         │
│  Lender          │  Credits loaned + repaid     │
└──────────────────┴──────────────────────────────┘

Peer-voted badges (community endorsement — thumbs up/down):
┌──────────────────┬──────────────────────────────┐
│  Trusted Seller  │  Peer-voted thumbs up        │
│  Top Creator     │  Community endorsed work     │
│  Helpful Node    │  Peer recognition            │
│  Flagged         │  Net negative votes — warning│
└──────────────────┴──────────────────────────────┘
```

**Peer Reputation Voting — Anti-Sybil Design:**

```
User browses Townhall, buys a song, seller delivered perfectly
     ↓
Buyer casts thumbs up vote
     ↓
Vote recorded on DIVA chain (nexus:reputation namespace, fast quorum)
     ↓
Vote weight = voter's contribution score
     ↓
Pioneer node with 99% uptime vote > fresh node with nothing contributed
     ↓
Cannot Sybil attack — spinning up 1000 new nodes gives 1000 × zero weight
```

**Vote costs:**
```
Thumbs up:   small PoW cost (spam prevention, not prohibitive)
Thumbs down: higher cost (higher bar for negative — prevents brigading)
```

**Reputation score visible on badge wall:**
```
[Trusted Seller ★]  [Forge Master]  [Deep Roots]
Reputation: 847 positive  /  3 negative
```

Nobody knows who the seller is. Everybody can see they are trusted.

Badges attach to reputation pseudonym. Anyone browsing the Townhall sees your badge wall. Nobody knows who you are. Trust is earned and provable — identity is not required.

---

### 7.4 Swap

Direct multi-chain atomic swaps via DIVA exchange. I2P native. No custody.

```
┌──────────────────────────────────┐
│  FROM   [NeXuS ▼]  [amount]      │
│  TO     [XMR   ▼]  [amount]      │
│                                  │
│  Rate:  1 NeXuS = 0.00X XMR     │
│  Fee:   0.3% (network survival)  │
│                                  │
│         [ SWAP ]                 │
└──────────────────────────────────┘
```

Supported pairs: NeXuS ↔ XMR, NeXuS ↔ BTC, XMR ↔ BTC (via DIVA atomic swap protocol).

---

### 7.5 NFT Forge (Mint)

Where creators bring digital goods into existence on NeXuS chain.

```
┌──────────────────────────────────────────────────┐
│  NFT FORGE                                       │
│                                                  │
│  Item name:    [___________________________]     │
│  Type:         [Music / Art / Software / File]   │
│  Upload:       [choose file]                     │
│  Supply cap:   [_____] copies  (immutable)       │
│                                                  │
│  Rights tiers to offer:                          │
│  ☑ Listen/Access    price: [____] NeXuS          │
│  ☐ Sync rights      price: [____] NeXuS          │
│  ☐ Remix rights     price: [____] NeXuS          │
│                                                  │
│  Secondary royalty:  [5]% (flows to you forever) │
│  Listing type:  ● Standard (3%)  ○ Boosted (5%)  │
│                                                  │
│  Estimated mint cost: [X] NeXuS                  │
│           [ FORGE ]                              │
└──────────────────────────────────────────────────┘
```

Once forged — supply cap is immutable. Contract enforces it. Nobody can mint more, including the creator.

---

### 7.6 Finance — Stake and Loan

Users who accumulate credits can put them to work. Users who need liquidity can borrow against collateral. Smart contract enforced. No bank. No KYC.

```
┌──────────────────────────────────────────────────┐
│  FINANCE                                         │
│                                                  │
│  ┌─────────────┐    ┌─────────────┐              │
│  │   STAKE     │    │    BORROW   │              │
│  │             │    │             │              │
│  │  Lock your  │    │  Post col-  │              │
│  │  credits    │    │  lateral,   │              │
│  │  Earn APR   │    │  receive    │              │
│  │  from pool  │    │  credits    │              │
│  └─────────────┘    └─────────────┘              │
│                                                  │
│  Pool stats:                                     │
│  Total staked: [X] NeXuS                        │
│  Current APR:  [X]%  (set by supply/demand)     │
│  Utilization:  [X]%                             │
└──────────────────────────────────────────────────┘
```

**Stake/Loan mechanics:**
```
Lender
├── Deposits NeXuS credits into lending pool smart contract
├── Receives pool share token (redeemable for principal + interest)
├── APR determined by utilization rate (more borrowers = higher rate)
└── Earns passively — credits work while node works

Borrower
├── Posts collateral (NeXuS tokens, over-collateralized)
├── Receives credits up to [collateral × ratio]
├── Pays interest to pool
├── ZKP proves sufficient collateral — identity never revealed
└── Liquidation if collateral falls below threshold
```

This creates productive use for accumulated credits, prevents hoarding, and gives nodes liquidity for larger purchases or investments in the Townhall.

---

### 7.7 Command Center — Node Resource Management

User controls exactly how much of their device they contribute to NeXuS. Sliders. Real-time feedback on earnings rate change.

```
┌──────────────────────────────────────────────────┐
│  COMMAND CENTER                                  │
│                                                  │
│  CPU contribution                                │
│  [████████░░░░░░░░░░░░] 12%  → earning X/day    │
│                                                  │
│  Storage contribution                            │
│  [███████████░░░░░░░░░] 47GB → earning X/day    │
│                                                  │
│  Bandwidth contribution                          │
│  [██████░░░░░░░░░░░░░░] 8%   → earning X/day    │
│                                                  │
│  ─────────────────────────────────────          │
│  Total earning rate:   [X] NeXuS/day            │
│  Governance weight:    [X]% of network          │
│  Node uptime:          [X]%                     │
│  Minimum required:     5% each (by protocol)    │
└──────────────────────────────────────────────────┘
```

Governance weight shown here too — contribution % IS voting weight. No surprises. User sees exactly how much say they have.

---

### 7.8 Keys

Background infrastructure made visible when needed. Not the focus — but accessible.

```
Master key (cold — never shown, never touches network)
     ↓  HD derivation
├── Creator key      → permanent pseudonym, royalties forever
├── Reputation key   → Townhall identity, badge holder
├── Governance key   → voting rights
├── Session keys     → rotate automatically (user never manages this)
└── Stealth addresses → generated per transaction by protocol
                        user never sees these — privacy is automatic
```

**Session key rotation is invisible.** Protocol handles it. User has one wallet, one identity per role, privacy managed underneath.

---

### 7.9 Proving Without Revealing

**Zero-Knowledge Proofs (ZKP)** — prove a statement without revealing the data:
```
"I am a unique node"              → Sybil resistance, no identity revealed
"I contributed 500GB of storage"  → earns tokens without linking to node
"I am the creator of this item"   → ownership without real name
"I can afford this purchase"      → balance check without showing balance
```

**Homomorphic Encryption (HE)** — compute on encrypted data without decrypting it:
```
Buyer encrypts balance
Seller encrypts price
     ↓
Smart contract: "does buyer have enough?"
     ↓
Returns YES or NO
Neither party saw the other's numbers
Contract never saw the numbers
```

HE use cases in NeXuS:
```
Storage proof:    prove file possession without revealing contents
Age gate:         prove over 18 without revealing age
Credit check:     prove affordability without revealing balance
Royalty split:    compute payout without exposing individual amounts
Governance vote:  prove contribution weight without revealing node
```

HE is a Phase 2 feature (Monero base does not include it — must be built, using DERO's implementation as design reference under Research License Residual Rights).

---

### 7.10 Sybil Resistance Without Identity

```
Cannot fake CPU cycles contributed
Cannot fake storage proven by challenge-response
Cannot buy governance weight
     ↓
Voting weight = real contribution
More contribution = more weight
No identity required — protocol enforces it
```

---

## 8. Real-World Asset Tokenization

### 8.1 Digital Goods (Clean, No Legal Issues)

Software, art, music, game items, documents — hash recorded on NeXuS chain, owner's key signs, full provenance history immutable.

### 8.2 Physical Assets (Legal Hoops, Foundation Solid)

```
Car:
├── VIN hashed on NeXuS chain
├── Current owner's master key signature
├── Transfer history immutable
└── Cryptographic proof supports legal claims

House:
├── Property details + deed hash on chain
├── Owner signature
├── Transfer history
└── Notarization layer (courts increasingly accept cryptographic evidence)
```

NeXuS doesn't replace legal systems — provides cryptographic proof that supports legal claims. Legal recognition gap is closing (Wyoming and others already recognizing blockchain records).

### 8.3 Real-World Asset Transfer with Privacy

```
Selling a car:
Generate one-time transfer wallet
     ↓
Buyer sends payment to burn address
     ↓
Title transfers on NeXuS chain (signed by seller's persistent key)
     ↓
Payment wallet burns
     ↓
Legal record: YES (title transfer on chain)
Financial trail: GONE (burn wallet dead)
```

---

## 9. The No-Intermediary Advantage

```
Traditional:                    NeXuS:
─────────────────────────       ──────────────────────
Spotify takes 70%               Creator keeps 95-97%
eBay takes 10-15%               Seller keeps 90-97%
PayPal can freeze funds         Smart contract cannot be frozen
Bank can deny service           No bank needed
Platform can ban you            No platform to ban from
Geographic restrictions         I2P — no geography
KYC required                    Master key IS your identity
AI captures displaced value     Value returns to individuals
```

---

## 10. Node Distribution Model

### 10.1 Core Principle — Burden IS the Service

No node gets a free ride. No node carries an unfair load. Every burden a node carries for the network is simultaneously the service the network provides back to it.

```
Every node contributes:          Every node earns:
├── Some CPU                     └── Proportional to contribution
├── Some storage                     No more, no less
├── Some bandwidth                   Same rate per unit regardless
└── Some validation                  of node size
```

### 10.2 Percentage-Based Contribution

Contribution is a **percentage of available resources**, not a fixed amount. Entry wall scales with the device.

```
Phone:   5% of 64GB  storage = 3.2GB contributed
Laptop:  5% of 512GB storage = 25GB contributed
Server:  5% of 10TB  storage = 500GB contributed
     ↓
Same rule. Same percentage. Same rate per GB earned.
Phone earns less total but earns fairly for what it has.
Zero exclusion based on hardware capability.
```

Minimum contribution percentage set by governance. Node configures its percentage at setup.

### 10.3 Distributed Chain History — No Archive Class

Blockchain history is NOT held by special archive nodes. It is a collective property of the whole network via erasure coding and sharding.

```
Block of chain history
     ↓
Split into 10 chunks + erasure coded (any 6 of 10 reconstruct)
     ↓
Chunks randomly distributed across ALL nodes
     ↓
Each node holds their assigned chunks (fraction of total)
     ↓
Challenge-response proves possession → earns tokens
     ↓
Node goes offline → chunks auto-reassigned to willing nodes
     ↓
Network self-heals, history never lost
```

**Result:** No premium for being large. No archive node class. No centralization pressure. Data centers have no economic advantage over individuals.

Each chunk stored by minimum N nodes (default: 5, adjustable by governance) for redundancy.

### 10.4 Free Rider Prevention

```
Node contributes nothing
     ↓
Earns no tokens
     ↓
Cannot purchase services
     ↓
No benefit to being on network without contributing
     ↓
Protocol enforces this — not trust
```

---

## 11. Bootstrap System

### 11.1 Design Principle

No clearnet exposure. No single bootstrap point. Four independent methods — any one that works gets the node into the mesh. Once inside, bootstrap is no longer needed.

### 11.2 Four Bootstrap Methods

```
┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐
│   Tor Bridge     │ │   IPFS / IPNS    │ │   BitTorrent     │ │   Reticulum      │
│                  │ │                  │ │                  │ │                  │
│ obfs4            │ │ Stable IPNS key  │ │ Mutable torrent  │ │ LoRa/radio/mesh  │
│ Snowflake(WebRTC)│ │ points to        │ │ BEP46 ed25519    │ │ zero internet    │
│ WebTunnel(HTTPS) │ │ rotating CID     │ │ stable magnet    │ │ off-grid entry   │
│ No clearnet      │ │ fresh peer list  │ │ rotating list    │ │ truly dark       │
└──────────────────┘ └──────────────────┘ └──────────────────┘ └──────────────────┘
```

All four attempted simultaneously on first connection. First success wins.

### 11.3 Why Each Method Is Censorship Resistant

**Tor Bridge:**
```
ISP blocks Tor?       → obfs4 looks like random traffic
obfs4 blocked?        → Snowflake looks like WebRTC
Snowflake blocked?    → WebTunnel looks like HTTPS
```

**IPFS/IPNS:**
```
CID gets blocked?     → IPNS name rotates to new CID automatically
Gateway blocked?      → fetch directly from IPFS peers
Stable IPNS key       → same address forever, content rotates underneath
```

**BitTorrent (BEP46 Mutable Torrents):**
```
Same ed25519 pubkey   → stable magnet link forever
Content signed + updated → fresh peer list, same entry point
Distributed across all torrent peers → impossible to kill
```

**Reticulum:**
```
No internet needed    → LoRa radio bootstrap
Everything blocked?   → mesh network entry
True off-grid         → phone + LoRa hat joins with zero internet
```

### 11.4 Rotating Peer List — Stays Fresh

```
Stable nodes maintain peer list
     ↓
Updated every X hours
     ↓
IPNS:   same key → new CID automatically
BEP46:  same pubkey → new signed content pushed
Tor:    bridge addresses stable (bridges are long-lived)
Reticulum: announcements propagate continuously
     ↓
Bootstrap always returns current active peers
Never stale, never a single point of failure
```

### 11.5 To Block NeXuS Bootstrap You Must Simultaneously

```
Block all Tor bridges worldwide    (impossible)
Block IPFS and all gateways        (very hard, IPFS is massive)
Kill a distributed torrent         (impossible)
Jam all radio frequencies          (illegal, detectable, temporary)
```

### 11.6 Bootstrap → Mesh Transition

```
New node connects via any bootstrap method
     ↓
Gets current peer list
     ↓
Establishes I2P identity
     ↓
Joins I2P netDB (announces itself)
     ↓
DHT query finds more peers
     ↓
Gossip protocol maintains peer list going forward
     ↓
Bootstrap methods no longer needed — fully in the mesh
Node is anonymous and discoverable simultaneously
```

---

## 12. Pending Design Questions

- [ ] Fractional rights (10% of sync rights = mini stock market — securities law risk?)
- [ ] ZK proof system selection (zk-SNARKs vs zk-STARKs vs Bulletproofs)
- [ ] Townhall reputation system mechanics
- [ ] Governance council election process
- [ ] Challenge-response storage proof implementation
- [ ] dPoW notary node count and election
- [x] ~~RandomX vs AstroBWT~~ → **AstroBWT confirmed** (keeps mining inside NeXuS, prevents external Monero pool domination)
- [ ] NeXuS token name
- [ ] Minimum contribution percentage (governance sets this)
- [ ] Erasure coding parameters (chunk count, redundancy factor)
- [ ] Bootstrap peer list update frequency

---

## 13. Meeting Prep — Konrad / DIVA (March 15th)

### Questions to bring:
- [ ] How is stake acquired — earned through contribution or purchased?
- [ ] What is the round duration in practice?
- [ ] How does WPBFT handle scale beyond ~200 nodes? (O(n²) message complexity)
- [ ] Are any fees planned or currently active on chain transactions?
- [ ] How does NeXuS add trading pairs — permissionless or governance vote?
- [ ] Does DIVA currently support adaptor signature swaps (BTC/XMR)?
- [ ] How does NeXuS deploy its own exchange instance on DIVA chain?
- [ ] What does DIVA need from builders like NeXuS?

### TODO before the 15th:
- [ ] Read new DIVA docs fully — especially tokenomics/stake model
- [ ] Reconcile architecture doc against actual DIVA capabilities
- [ ] Prepare integration proposal: NeXuS chain + DIVA chain touchpoints

---

## 14. References

- **Monero**: MIT license — confirmed clean base chain for fork
- **DERO project**: dero.io — Research License only, no production use. Architecture inspiration for DAG + HE + smart contracts (Section III.B Residual Rights)
- **DENERO**: Fork of DERO — carries same Research License, same block
- **AstroBWT**: DERO's mining algorithm — implementing in NeXuS to contain mining within ecosystem
- **Komodo dPoW**: komodoplatform.com — delayed Proof of Work security model
- **DIVA chain**: local repo ~/Projects/nexus-node/divachain/ — coordination + proof layer
- **DIVA exchange**: Protocol infrastructure, anyone runs own instance, atomic swaps, I2P native
- blockchain-nexus.pdf: ~/Documents/007/documents/nexus/blockchain-nexus.pdf
