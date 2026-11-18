# AGENT_INSTALL.md — AI Agent Automated Setup & Restore Guide

Welcome, fellow AI Agent!
This repository contains a standalone visual skin mod (**ChatGPT-Themed Yi Sang (10115 Index Father)**, Identity ID `10115`) for **Limbus Company** (PC / Steam official client, Unity `6000.3.12f1`), along with a lightweight, transparent CLI installer (`installer/`).
*(Note: This release contains **ONLY** the `10115` skin assets and the minimal CLI installer/restorer. The Studio authoring UI is intentionally excluded.)*

Follow the steps below to safely back up the user's original game files and install (or restore) the skin on **Windows**.

---

## Safety & Integrity Rules (Must Read First)

1. **Close the game first**: Ensure `LimbusCompany.exe` is **not** running before running `apply` or `revert`.
2. **Never delete `.state/`**: Clean backups of the user's original `__data` AssetBundles (and any modified `Lang/*.json` files) are saved inside `<repo>/.state/`. **Keep this repository folder on disk** so the user can restore vanilla files anytime.
3. **Never write personal paths back to tracked files**: If manual path overrides are needed on a non-standard setup, copy `config.example.json` to `config.local.json` (which is gitignored).
4. **Do not tamper with game DLLs, BepInEx, or shared `UnitKeyword` tags**:
   - This installer uses **zero** BepInEx plugins, DLL injection, or memory tampering; it patches only `p10115` local Addressables cache (`__data`).
   - **Shared-Tag Hazard (`UnitKeyword_*`)**: In `Lang/<active>/UnitKeyword*.json`, faction/chapter tags such as `UnitKeyword_SPIDER_HOUSE` are **shared across multiple identities**. Patching them directly in the official client would overwrite the tag for every related identity simultaneously, so shared tags are **intentionally left vanilla** in `mod.json`. Do not add `unit_keywords` rules unless the user explicitly asks for a global override after reading `installer/lang_patcher.py`.
5. **Optional Voice Pack Notice (`optional_voice/`, 98 WAV tracks)**:
   - This repository separates custom voice audio (`optional_voice/custom_sounds/`) from the visual `installer/`.
   - The PC Steam official client stores character voices inside packed multi-identity FMOD `.bank` / `.assets.bank` FSB5 streams (`LimbusCompany_Data/StreamingAssets/Assets/Sound/FMODBuilds/Desktop/`), and `installer/` **intentionally does NOT patch `.bank` files**.
   - **Agent Guidance**: Read `optional_voice/README_VOICE.md` and clearly inform the user of the official-client limitation so they can weigh options (safe visual-only install with vanilla voices, Lethe private server loose WAV usage, or self-assessed external FMOD repacking).

---

## Step 1: Verify Python 3.12+ & Create Virtual Environment

Check whether `.venv` already exists in the repo root; if not, locate Python `>=3.12` (`py -3` or `python`), create `.venv`, and install dependencies (`UnityPy>=1.25.0`, `lz4>=4.4.0`, `Pillow>=12.0.0`, `etcpak>=0.9.15`, `astc-encoder-py>=0.1.12`, `texture2ddecoder>=1.0.6`).

In **PowerShell**:

```powershell
# 1. Check Python >= 3.12
python --version

# 2. Create .venv if missing
if (!(Test-Path ".venv\Scripts\python.exe")) {
    python -m venv .venv
}

# 3. Install required packages (if default PyPI is slow/blocked in CN, fallback to Tsinghua Tuna mirror)
.\.venv\Scripts\python.exe -m pip install "UnityPy>=1.25.0" "lz4>=4.4.0" "Pillow>=12.0.0" "etcpak>=0.9.15" "astc-encoder-py>=0.1.12" "texture2ddecoder>=1.0.6"
```

*(If PyPI times out in mainland China, retry with `-i https://pypi.tuna.tsinghua.edu.cn/simple`.)*

---

## Step 2: Run Environment & Path Auto-Discovery Check

Run:

```powershell
.\.venv\Scripts\python.exe -X utf8 -m installer status
```

`installer/config.py` automatically detects:
- **Game Root**: via Windows Registry (`SteamPath` / `InstallPath`) + `steamapps/libraryfolders.vdf` + common drive paths (`C:`–`H:`).
- **LocalLow Cache & Catalog**: via Windows Known Folder (`FOLDERID_LocalAppDataLow`) -> `ProjectMoon\LimbusCompany\com.unity.addressables\catalog_S1.bin` and `Unity\ProjectMoon_LimbusCompany`.
- **Active Localization Folder**: via `LimbusCompany_Data/Lang/config.json`.

### Troubleshooting `status` Output
- **`Game Root [...] [MISSING]`**:
  Search the user's drives for `LimbusCompany.exe` (e.g. `Get-PSDrive -PSProvider FileSystem`), then create `config.local.json` from `config.example.json` and set `"game_root"` to the folder containing `LimbusCompany_Data`.
- **`Target 10115 [...]: NOT CACHED`**:
  The user has not viewed this Identity in-game yet, so Unity Addressables has not downloaded the `p10115` bundle into `LocalLow` cache. Ask the user to:
  1. Launch **Limbus Company** once,
  2. Open the Sinner / Identity screen (or Theatre / Dispensary) and view **Identity `10115`** once so assets cache locally,
  3. Close the game, and tell you to continue.
- **`Catalog Binary [MISSING]`**:
  Ensure the user has launched the game and logged in at least once on this Windows user account.

---

## Step 3: Install (Backup + Patch + Deploy)

*(Optional dry-run verification without touching `.state/` or real game files: run `.\.venv\Scripts\python.exe -X utf8 -m installer apply --build-only`.)*

Once `status` shows `Game Root [OK]`, `Catalog Binary [OK]`, and `Target 10115` as `READY` (with `LimbusCompany.exe` closed), run:

```powershell
.\.venv\Scripts\python.exe -X utf8 -m installer apply
```

### Expected Healthy Output
You should see a summary line matching the `10115` asset inventory:
- `[Skin:limbus-yisang-index-father-10115] Installed & Deployed -> illust=4, sd_frames=171, fx_sprites=0, spine_tex=1, spine_assets=2`

*(Note: `10115` uses a hybrid layout inside `p10115_assets_all_*.bundle` combining a DXT5 primary `SpriteAtlas` with `14` standalone sprites. The `152` custom combat SD PNGs expand across `171` animation frame slots covering Phase 1 and Phase 2 with iron whip & unmasked broken-eye forms—injected cleanly with Full-Quad margin expansion while `dead.png` stays vanilla; the dual-skin `index_father_yisang_idle` (`head`/`off_head`) Spine assets/texture are replaced with automatic `pma:true` premultiplication; and all `4` U1 `10115_normal*` illustrations are updated. U3 artwork, skill names/icons, story, and official `.bank` voice audio remain vanilla unless the user separately opts into DIY voice handling under `optional_voice/`.)*

---

## Step 4: How to Restore Vanilla Files (Uninstall)

If the user wants to uninstall the mod or prepare cleanly before a major game update, run (with the game closed):

```powershell
.\.venv\Scripts\python.exe -X utf8 -m installer revert
```

(Or tell the user they can simply double-click `restore_vanilla.cmd`.)

---

## Step 5: Wrap Up

Report clearly to the user:
1. What was installed (`4` U1 `10115_normal*` illustrations, `152` combat SD frames across Phase 1 & Phase 2 with vanilla `dead.png`, and `index_father_yisang_idle` dual-skin breathing Spine; U3, skill names/icons, story, shared `UnitKeyword_SPIDER_HOUSE`, and `.bank` audio left vanilla).
2. Explain the optional `optional_voice/` (`98` WAV tracks) official-client `.bank` limitation so the user knows why custom voice was not auto-injected.
3. Remind them **not to delete this folder** (or its `.state/` subfolder), and explain how to restore anytime (`restore_vanilla.cmd` or asking you to run `revert`).
