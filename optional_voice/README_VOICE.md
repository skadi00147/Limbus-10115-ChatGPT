# Optional Voice Pack & Official Client Limitations (`10115`)

> **Identity ID**: `10115` | **Included Audio Files**: `98` WAV track(s) under `custom_sounds/`
> **Audio Specification**: `28000 Hz`, Mono, `16-bit PCM WAV`

This directory contains optional custom character voice WAV replacements provided **separately** from the default visual skin installer (`installer/`).

## Why `installer/` Does NOT Automatically Install Voice Files

1. **No Loose WAV Loader on Official Steam Client**:
   Unlike Lethe private servers (which support loose `custom_sounds/Voice/*.wav` loading and hot-reload), the **PC Steam official client (`Unity 6000.3.12f1`) has zero loose WAV loading support** and no Addressables bundle entry for character voice clips.
2. **Packed Multi-Character FMOD `.bank` / `.assets.bank` Architecture**:
   Official character voices are packed inside multi-identity **FSB5** audio streams under:
   `<LimbusCompany_Root>\LimbusCompany_Data\StreamingAssets\Assets\Sound\FMODBuilds\Desktop\*.bank` and `*.assets.bank` (often tens of megabytes per bank, shared across multiple identities).
3. **Intentional Safety Boundary**:
   To keep `install_skin.cmd` / `restore_vanilla.cmd` fast, lightweight, non-invasive, and 100% safe to revert via `.state/`, this repository's `installer/` **intentionally leaves all `.bank` files untouched**.

## How Users & AI Agents Should Weigh Their Options

- **Option A — Official Steam Client (Safe Visual-Only, Recommended)**:
  Run `install_skin.cmd` as usual. You get the full visual replacement (SD combat frames, idle Spine, U1 illustrations) with clean vanilla voice lines and zero `.bank` tampering risk.
- **Option B — Lethe Private Server + `WhaleMaidCompat` / Custom Sound Support**:
  Copy `custom_sounds/` from this folder directly into your Lethe mod directory (or follow your server's `Sound/` layout).
- **Option C — Official Steam Client Advanced DIY (Self-Assessed Risk)**:
  If you or your AI Agent decide to inject these WAVs into the official client, you must independently locate the matching `Voice_*.assets.bank` + `.bank` pair, perform FSB5 sub-stream replacement/repacking with external FMOD tooling, back up the original `.bank` files first, and accept full responsibility for game updates, file integrity checks, or account/client risks.
