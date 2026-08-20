# Install Guide

Irodori Table publishes desktop packages from GitHub Releases. The release
state checked for this guide is:

- latest stable GitHub Release: `v0.8.5`, published on 2026-07-30;
- latest lightweight prerelease: `v0.8.12`, published on 2026-08-20.

<https://github.com/irodori-table/irodori-table/releases>

Stable releases publish Linux, universal macOS, and Windows packages. The
current stable release also includes signed Tauri updater metadata, although
its macOS and Windows installers are not platform-signed and may trigger
Gatekeeper or SmartScreen warnings. Lightweight prereleases publish Linux
AppImage, `.deb`, and `.rpm` packages only. `cargo install` does not install the
desktop application; it is only for the separate headless `irodori-server`.

This guide is for packaged desktop installs. Source build prerequisites and
WebView troubleshooting live in the platform development guides:
[Windows](windows-development.md), [macOS](macos-development.md), and
[Linux](linux-development.md).

## Quick terminal install

The shortest terminal path uses GitHub CLI. Install `gh` first if your shell
does not already have it. When no release tag is passed, `gh release download`
downloads assets from the latest release.

### Linux: portable AppImage

```bash
mkdir -p "$HOME/Applications"
gh release download --repo irodori-table/irodori-table --pattern "*.AppImage" --dir "$HOME/Applications"
chmod +x "$HOME/Applications"/Irodori*.AppImage
"$HOME/Applications"/Irodori*.AppImage
```

To test the newest lightweight prerelease instead of the latest stable release,
pass the tag explicitly:

```bash
gh release download v0.8.12 --repo irodori-table/irodori-table --pattern "*.AppImage" --dir "$HOME/Applications"
```

## Downloads by OS

| OS | Recommended asset | Notes |
| --- | --- | --- |
| Linux | `.AppImage` | Stable and lightweight releases also provide `.deb` and `.rpm` packages. |
| Windows | setup `.exe` | The stable release also provides an `.msi`; current installers are unsigned. |
| macOS | universal `.dmg` | The current package is unsigned and may trigger Gatekeeper warnings. |

## Windows

Download the setup `.exe` or `.msi` from the latest stable release. The current
installers are unsigned, so verify that the URL is under
`github.com/irodori-table/irodori-table/releases` before accepting a
SmartScreen warning. For a source build, use the
[Windows development guide](windows-development.md).

## macOS

Download the universal `.dmg` from the latest stable release. The current disk
image is unsigned and not notarized, so macOS may show a Gatekeeper warning.
For a source build, use the [macOS development guide](macos-development.md).

## Linux

Download the AppImage, make it executable, and run it:

```bash
chmod +x ./Irodori*.AppImage
./Irodori*.AppImage
```

Some distributions ship FUSE 3 by default while AppImage still expects FUSE 2.
If the AppImage does not launch, either install the distribution's FUSE 2
package or run it in extract-and-run mode:

```bash
APPIMAGE_EXTRACT_AND_RUN=1 ./Irodori*.AppImage
```

For release channel details and future package-manager plans, see
[Distribution and updates](distribution.md).

## Headless server

Rust users who need the local HTTP API, not the desktop app, can install the
headless server from the foundation tag used by the current desktop workspace:

```bash
cargo install --git https://github.com/irodori-table/irodori-kit --tag v0.7.5 --locked irodori-server
```

See [Headless local data API](headless-data-api.md) for runtime configuration.
