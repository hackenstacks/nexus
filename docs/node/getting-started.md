# Getting Started

!!! note "Status"
    The network/privacy layer is ready. The economy layer (chain, wallet, DIVA) is Phase 1.

## Requirements

- Alpine Linux (recommended) or any Linux distribution
- 1GB RAM minimum (2GB recommended)
- Any storage — contribution is percentage-based, no minimum
- Internet connection (or LoRa radio for off-grid via Reticulum)

## Install

```bash
# Coming soon — one script deploys everything
curl -fsSL https://nexus.example/install.sh | bash
```

## What Gets Installed

See [Required Applications](../required-applications.md) for the full list.

The installer sets up:

1. Transport layer — Tor bridges + I2P + Reticulum
2. Privacy stack — all traffic routed through darknet by default
3. Optional desktop — labwc + foot terminal + fuzzel launcher
4. CLI toolkit — tmux, fzf, bat, ripgrep, neovim

## After Install

Your node is part of the mesh. No registration. No account. Your master key is your identity.

Set your contribution percentage in the Command Center (once wallet is built — Phase 1).
