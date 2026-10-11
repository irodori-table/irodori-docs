# Distribution & Updates

How users get Irodori Table and how it updates.

- Current stable GitHub Release: **0.8.5** (`v0.8.5`, published
  2026-07-30).
- Current lightweight prerelease: **0.8.12** (`v0.8.12`, published
  2026-08-20).

## Already in place

`.github/workflows/release.yml` supports three channels. A `v*` tag push runs
the lightweight channel and publishes Linux AppImage, `.deb`, and `.rpm`
packages as a prerelease. The manual preview channel can append unsigned
universal macOS and Windows packages. The manual stable channel publishes the
full Linux/macOS/Windows set and becomes GitHub's Latest release. Updater and
platform signing are enabled independently when their credentials are valid;
missing signing credentials no longer block an otherwise complete stable build.

## Channel matrix

| Channel | For | Status | Notes |
| --- | --- | --- | --- |
| GitHub Releases (Linux) | Linux users | ✅ exists | tag pushes publish AppImage, `.deb`, and `.rpm` in the lightweight prerelease lane |
| Stable Windows/macOS releases | end users | ✅ exists | `v0.8.5` includes Windows NSIS/MSI and a universal macOS DMG/app archive; current platform packages are unsigned |
| Tauri in-app updater | end users (auto-update) | ✅ exists | `v0.8.5` includes `latest.json` and signed updater artifacts |
| Terminal package download | Linux users | ✅ exists | use `gh release download` and run the `.AppImage`, or install the `.deb`/`.rpm` |
| APT repository (Debian/Ubuntu) | Linux users | ✅ exists | signed repository served from `irodori-table.github.io/irodori-packages`; stable releases only |
| AUR | Arch users | ✅ exists | `irodori-table`, repackaged from the release `.deb`; stable releases only |
| `cargo install --git` | Rust devs (headless server) | ✅ exists | installs `irodori-server` from `irodori-kit v0.7.5`, not the desktop app |
| crates.io | Rust devs | ⬜ later | crates.io forbids git/path deps; all `irodori-*` must be published first |
| Homebrew cask / Scoop / winget | mac/Windows | ⬜ later | manifests auto-bumped from releases |
| Flatpak | Linux | ⬜ later | from releases |

Public registration text, support/privacy/disclaimer URLs, and package manager
channel notes are collected in [store-registration.md](store-registration.md).
The APT and AUR channels are published by the release workflow; see
[RELEASING.md](https://github.com/irodori-table/irodori-table/blob/main/RELEASING.md)
for the signing secrets and how to reproduce a repository locally.

## Quick terminal install

Use GitHub CLI to fetch the newest stable AppImage without opening a browser:

```bash
tmp="$(mktemp -d)"
gh release download --repo irodori-table/irodori-table --pattern "*.AppImage" --dir "$tmp"
chmod +x "$tmp"/*.AppImage
"$tmp"/*.AppImage
```

Use an explicit tag to test a lightweight prerelease:

```bash
tmp="$(mktemp -d)"
gh release download v0.8.12 --repo irodori-table/irodori-table --pattern "*.AppImage" --dir "$tmp"
chmod +x "$tmp"/*.AppImage
"$tmp"/*.AppImage
```

The current checked assets are:

- `v0.8.5`: `Irodori.Table_0.8.5_amd64.AppImage`, plus `.deb`, `.rpm`,
  universal macOS, Windows NSIS/MSI, and updater assets.
- `v0.8.12`: `Irodori.Table_0.8.12_amd64.AppImage`, plus `.deb` and `.rpm`.

Check the release notes before installing: a stable release may publish all
platform packages without platform code signing, and a lightweight prerelease
does not include macOS, Windows, or updater artifacts.

## Package-manager install (stable releases)

Stable releases also publish a signed APT repository and an AUR package, so
Debian/Ubuntu and Arch users can install through their package manager instead
of downloading an AppImage. Both channels are refreshed only for stable
releases.

### Debian / Ubuntu (APT)

```bash
curl -fsSL https://irodori-table.github.io/irodori-packages/apt/irodori-archive-keyring.gpg \
  | sudo tee /usr/share/keyrings/irodori-archive-keyring.gpg >/dev/null

echo "deb [signed-by=/usr/share/keyrings/irodori-archive-keyring.gpg] https://irodori-table.github.io/irodori-packages/apt stable main" \
  | sudo tee /etc/apt/sources.list.d/irodori.list

sudo apt update
sudo apt install irodori-table
```

Supported architectures: `amd64` and `arm64`. The signing key is published at
`https://irodori-table.github.io/irodori-packages/apt/irodori-archive-keyring.gpg`.

### Arch Linux (AUR)

```bash
yay -S irodori-table
# or: paru -S irodori-table
```

The AUR package repackages the same binaries, desktop entry, and icons as the
Debian package.

## On "cargo is fastest"

`cargo install` only installs Rust binaries. It is not a desktop app installer:
the Tauri app bundles a webview, native packaging metadata, and a built frontend.
Use GitHub Release installers for the desktop app.

For the headless local HTTP API, install `irodori-server` from the foundation
tag used by the current desktop workspace:

```bash
cargo install --git https://github.com/irodori-table/irodori-kit --tag v0.7.5 --locked irodori-server
```

The old `irodori-table` repo command is no longer correct because
`irodori-server` moved to `irodori-kit`.

## Recommended order

1. **Keep lightweight tags moving** so Linux AppImage/`.deb`/`.rpm`
   prereleases continue to publish from main.
2. **Verify every stable promotion** across Linux, macOS, Windows, and updater
   assets before making it GitHub Latest.
3. **Finish platform signing** by configuring a Windows signing backend and
   macOS signing/notarization; the workflow intentionally falls back to
   unsigned packages when those credentials are absent.
4. **Keep desktop terminal installs package-based** with `gh release download`;
   keep `cargo install` scoped to the `irodori-kit` headless server.
5. **Remaining package managers** (brew/scoop/winget/Flatpak) once the stable
   assets and checksums are durable enough for manifests. Debian/Ubuntu (APT)
   and Arch (AUR) are already published from stable releases.
