# BGCM-Pryme-Reports-Launcher

Installer & version tracking for **Pryme Reports** — the reporting suite for
Boys & Girls Clubs of the Midlands (BGCM).

This repository is the update channel for the compiled Pryme application. It
does not contain source code; it holds the version manifest and hosts release
downloads.

## How the auto-update works

1. On startup, `Pryme_Launcher.exe` fetches `version.json` from this repo's
   `main` branch (via `raw.githubusercontent.com`).
2. It compares the `version` field against the launcher's own `APP_VERSION`.
3. If `version.json` is newer, the launcher shows an update banner and, on
   confirmation, downloads `download_url`, extracts it, and hands the new
   executables to `Pryme_Updater.exe`, which swaps them in and relaunches.

## Files

- **version.json** — the update manifest the launcher reads:
  - `version` — latest released version (must match the compiled launcher's `APP_VERSION`)
  - `download_url` — link to the **update zip** on the matching GitHub release
  - `notes` — short release notes shown in the update banner

## Releasing a new version

Keep these four values identical every release (call the version `X.Y.Z`):

| Location | Value |
|---|---|
| `APP_VERSION` in `BGCM_Pryme_Launcher.py` | `X.Y.Z` |
| `#define MyAppVersion` in `Pryme_Reports_Installer.iss` | `X.Y.Z` |
| GitHub release tag | `vX.Y.Z` |
| `version` in `version.json` | `X.Y.Z` |

Steps:

1. Run `build_all.bat` to compile the executables and produce `Pryme_Update.zip`.
2. Create a GitHub release tagged `vX.Y.Z` on this repo.
3. Upload **both** assets to the release:
   - `Pryme_Reports_Setup_vX.Y.Z.exe` — full installer, for first-time installs.
   - `Pryme_Update.zip` — the compiled exes, for the in-app auto-updater.
4. Update `version.json` (`version`, `download_url`, `notes`) and commit to `main`.

> The repository must remain **public** so the launcher can read `version.json`
> and download release assets without authentication.
