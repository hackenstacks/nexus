# nexus-docs - Action Report

**Project:** /home/user/Documents/nexus-docs
**Created:** 2026-03-10

---


## Added PSAD + fwsnort manuals, fixed dead links, added scripts to repo (2026-03-10 12:46)

**What:** Added security manuals to docs site, fixed dead hyperlinks, committed scripts to repo.
**Why:** Site had thin security section and broken links to scripts that weren't in the repo.
**How:** Copied PSAD_MANUAL.md + FWSNORT_MANUAL.md from ~/scripts/docs/ into nexus-docs/docs/. Added scripts/ dir with the 3 referenced scripts. Fixed dead relative links (../script.sh) to GitHub blob URLs. Updated mkdocs.yml nav. Committed to main + deployed gh-pages.
**Files:** docs/psad-manual.md, docs/fwsnort-manual.md, docs/security-stack.md, scripts/nexus-fix-psad-logging.sh, scripts/nexus-fix-fwsnort-nftables.sh, scripts/nexus-update-fwsnort.sh, mkdocs.yml
**Live at:** https://hackenstacks.github.io/nexus/

---


## Added NeXuS Integrity doc and Human KDF (NeXuS Riddler Equation) (2026-03-10 13:54)

**What:** Two new core philosophy/security docs added to the site.
**Why:** Conversation produced two important ideas that belong in the book: (1) NeXuS must deliver on its promises in grid-down scenarios, not just when everything works. (2) A human-computable password algorithm that requires no software, no cloud, no trust.
**How:** nexus-integrity.md covers prepared-not-paranoid philosophy, honest promise vs reality audit, physical security without black boxes (no TPM). human-kdf.md covers the full NeXuS Riddler Equation — human KDF with salted derivation, quick reference card, rotation strategy.
**Files:** docs/nexus-integrity.md, docs/human-kdf.md, mkdocs.yml
**Note:** Algorithm to be renamed NeXuS Riddler Equation in next edit.

---


## Major vision docs session — RIN, Rings, Education Oracle, Architecture Layers, Cerberus Protocol (2026-03-10 21:47)

**What:** Six major vision documents written and deployed to hackenstacks.github.io/nexus in one session.
**Why:** Vision conversation produced core NeXuS concepts that needed to be captured immediately.
**Docs Created:**
1. cerberus-protocol.md — Human-computable password KDF, three heads mythology, salted derivation, field card
2. nexus-integrity.md — Prepared not paranoid, honest promise vs reality audit, physical security without black boxes
3. rin.md — Real Intelligence Network, dual leaderboards, Oracle + AI profiles, XMR economy
4. nexus-rings.md — Family Sovereignty Protocol, parent domain rights, ring structure, privacy by architecture
5. education-oracle.md — Teachers/nannies/mentors as Oracles, residual earning model, constructive culture
6. nexus-architecture-layers.md — Two-layer public/dark architecture, nexusnet.network vs nexusnet.nexus, nexus.locker vault
**Domain Portfolio:** 15 domains secured on Porkbun including nexusnet.nexus, nexusforge.nexus, nexusnet.dev, nexusnet.studio, nexusnet.email and more
**Files:** All docs in ~/Documents/nexus-docs/docs/, mkdocs.yml nav updated, deployed to gh-pages

---


## NeXuS Mint Protocol + Blueprint docs (2026-03-11 05:57)

What: Created two new docs in MkDocs — nexus-mint-protocol.md and nexus-blueprint.md. Why: Needed to document the NeXuS Mint Protocol (content monetization architecture designed this session) and create a source-of-truth blueprint to replace the error-filled PNG infographic. How: nexus-mint-protocol.md covers the full protocol — key hierarchy, burn mechanism, IPFS storage, smart contract spec, economic model (primary/secondary market, royalties, NeXuS fees). nexus-blueprint.md covers all 6 system panels with correct Alpine/OpenRC references, Medusa vs Hydra distinction, and a corrections table listing errors from the old PNG (systemd→OpenRC, Falltom→fail2ban, Netlicum→Reticulum, OpenSwitch→OpenSnitch). Files: docs/nexus-mint-protocol.md, docs/nexus-blueprint.md, mkdocs.yml updated with new nav entries.

---

