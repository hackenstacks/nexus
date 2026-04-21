# 🗝️ NeXuS Boot Protocol: The Mutual Unlock
*Version 1.0 — 2026-04-01 | Sane • Simple • Secure • Stealthy • Beautiful*

<!-- NEXUS-META
id:           NXS-PROTOCOL-0001
type:         PROTOCOL
principles:   Sane Secure Stealthy
cia:          Confidentiality Integrity
moe:          Modularity
pop:          Privacy
ymca:         Yes-We-Can
zero-trust:   true
sovereignty:  local
transparency: full
defense:      active
created:      2026-04-01
authors:      gemini
project:      nexus-boot
layer:         infra
version:       1.0
updated:       2026-04-01
status:        active
audience:      all
tier:          all
score-sane:    4
score-secure:  5
score-stealthy:4
score-composite: 13
score-tier:    ACTIVE
score-level:   2
scored-by:     gemini
scored-at:     2026-04-01
nexium-reward: 97
-->

---

## 🧠 The "Sane" Baseline: Mutual Interdependence
In the NeXuS ecosystem, a node is not "Sane" if it can be unlocked by a single factor. The **Mutual Unlock** protocol ensures that neither the USB key nor the local storage contains the full secret. They are two halves of a single cryptographic soul.

### 🔄 The Paradox of the Key
1.  **USB Needs Local:** The USB key contains an encrypted blob that requires a hardware identifier (UUID/Serial) or a small key-fragment stored in the local machine's TPM/firmware to decrypt.
2.  **Local Needs USB:** The local storage (the `/home` and `/var` partitions) is encrypted with a master key that is stored *only* on the USB device.
3.  **The Result:** You cannot boot the USB on a different machine to see the keys, and you cannot pull the drive from the machine to read the data. **They must be together to exist.**

---

## 🛠️ The Boot Workflow (Iron Boot)

### Phase 1: The physical Handshake
- **Trigger:** User inserts the **NeXuS Physical Key** (USB) and powers on the **Screaming Demon** (Dell E6520).
- **BIOS/UEFI:** The system is locked to only boot from the specific signed UUID of the NeXuS USB.

### Phase 2: Decrypting the Key (The First Half)
- The USB `initramfs` loads.
- It queries the machine's hardware fingerprint (CPU ID + Motherboard Serial + TPM Seed).
- It uses this fingerprint to decrypt the **Stage 2 Master Key** stored on the USB.
- *If the USB is moved to a different laptop, Phase 2 fails. The key remains a blob of noise.*

### Phase 3: Unlocking the Vault (The Second Half)
- The decrypted Stage 2 Master Key is now used to mount the local encrypted partitions (`LUKS` on `/dev/sdaX`).
- The system pivots to the local drive.
- The USB remains mounted as a read-only witness, holding the **Cerberus Operating Keys**.

---

## 🛡️ Security Implications
- **Anti-Theft:** If the laptop is stolen without the USB, the data is cold.
- **Anti-Seizure:** If the USB is swallowed/destroyed, the laptop becomes a brick of encrypted entropy.
- **Evil Maid Protection:** Because the USB contains the `boot` and `initramfs` (the code that asks for the password), an attacker cannot modify the local OS to "sniff" your password without possessing the USB.

---

## 📋 The "Sane" Checklist
- [ ] USB Key UUID registered in BIOS.
- [ ] Local partition encrypted with AES-XTS-PLAIN64.
- [ ] Hardware-bound decryption script active in `initramfs`.
- [ ] No plaintext keys ever touch the local disk.

---

> **NeXuS Principles: Sane • Simple • Secure • Stealthy • Beautiful**
> *The machine is one. The key is one. Together, they are NeXuS.*
