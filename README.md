# MeedyaDL-Tools

Pre-built external tool binaries for [MeedyaDL](https://github.com/MWBMPartners/MeedyaDL).

## Purpose

This repository serves as a **mirror** for external tool binaries that MeedyaDL depends on at runtime:

- **Fallback distribution** — When upstream sources are unavailable (404, timeout, naming changes), MeedyaDL falls back to downloading from this repo's `latest` release.
- **Installer bundling** — Release builds bundle these tools directly into MeedyaDL installers for a zero-download first-run experience.

## Hosted Tools

| Tool | Description | Upstream Source | License |
| ---- | ----------- | -------------- | ------- |
| **FFmpeg** | Media encoder/decoder | [BtbN/FFmpeg-Builds](https://github.com/BtbN/FFmpeg-Builds) | GPL v2+ |
| **yt-dlp** | Video/audio downloader | [yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp) | Unlicense |
| **mp4decrypt** | MP4 DRM decryption | [Bento4 SDK](https://www.bok.net/Bento4/) | GPL v2 |
| **MP4Box** | MP4 multiplexer | [GPAC](https://gpac.io/) | LGPL v2.1 |
| **N_m3u8DL-RE** | HLS/DASH downloader | [nilaoda/N_m3u8DL-RE](https://github.com/nilaoda/N_m3u8DL-RE) | MIT |
| **aria2c** | Multi-protocol downloader | [aria2/aria2](https://github.com/aria2/aria2) | GPL v2 |
| **fpcalc** | Audio fingerprinting (AcoustID) | [acoustid/chromaprint](https://github.com/acoustid/chromaprint) | LGPL v2.1 |
| **get_iplayer** | BBC iPlayer downloader | [get-iplayer/get_iplayer](https://github.com/get-iplayer/get_iplayer) | GPL v3 |
| **Votify** | Spotify downloader | [glomatico/votify](https://github.com/glomatico/votify) | — |
| **gytmdl** | YouTube Music downloader | [glomatico/gytmdl](https://github.com/glomatico/gytmdl) | — |
| **gamdl** | Apple Music downloader | [glomatico/gamdl](https://github.com/glomatico/gamdl) | — |
| **AMdecrypt** | Apple Music decryption bridge | [glomatico/amdecrypt](https://github.com/glomatico/amdecrypt) | — |
| **Wrapper** | FairPlay DRM decryption server | [WorldObservationLog/wrapper](https://github.com/WorldObservationLog/wrapper) | — |
| **MediaInfo** | Media file metadata analysis | [MediaArea/MediaInfo](https://github.com/MediaArea/MediaInfo) | BSD-2-Clause |
| **OF-Scraper** | OnlyFans content downloader | [datawhores/OF-Scraper](https://github.com/datawhores/OF-Scraper) | MIT |
| **MKVToolNix** | MKV muxing/editing suite | [mkvtoolnix.download](https://mkvtoolnix.download/) | GPL v2 |
| **potrace** | Bitmap → vector tracing (outline / monochrome) | [potrace.sourceforge.net](https://potrace.sourceforge.net/) | GPL v2+ |
| **VTracer** | Raster → SVG colour tracing | [visioncortex/vtracer](https://github.com/visioncortex/vtracer) | MIT / Apache-2.0 |
| **GNU ddrescue** | Data recovery / block-device imaging | [gnu.org/software/ddrescue](https://www.gnu.org/software/ddrescue/) | GPL v3 |

## Platform Support

| Tool | Linux x86_64 | Linux aarch64 | Linux armhf | Windows x64 | Windows ARM64 | macOS ARM |
| ---- | :---: | :---: | :---: | :---: | :---: | :---: |
| FFmpeg | Y | Y | — | Y | — | Y |
| yt-dlp | Y | Y | — | Y | Y | Y |
| mp4decrypt | Y | — | — | Y | — | Y |
| MP4Box | Y | — | — | Y | — | Y |
| N_m3u8DL-RE | Y | Y | — | Y | Y | Y |
| aria2c | Y | — | — | Y | — | Y |
| fpcalc | Y | Y | — | Y | — | Y |
| get_iplayer | Y* | Y* | Y* | Y** | — | Y* |
| Votify | Y | Y | — | Y | — | Y |
| gytmdl | Y | Y | — | Y | — | Y |
| gamdl | Y | Y | — | Y | — | Y |
| AMdecrypt | Y | Y | — | Y | Y | Y |
| Wrapper | Y | — | — | — | — | — |
| MediaInfo | Y | Y | — | Y | Y | Y |
| OF-Scraper | Y | Y | — | Y | — | Y |
| MKVToolNix | Y | — | — | Y | — | Y*** |
| potrace | Y**** | Y**** | — | — | — | Y**** |
| VTracer | Y**** | Y**** | — | — | — | Y**** |
| ddrescue | Y**** | — | — | — | — | — |

**Y** = pre-built binary
**Y*** = Perl script (requires Perl runtime)
**Y**** = Windows installer
**Y***** = x86_64 via Rosetta 2
**Y****** = compiled from source; macOS ships separate aarch64 + x86_64 assets (no universal binary is produced in this repo)
**—** = not available

## Asset Naming Convention

```text
{tool_id}-{os}-{arch}.{ext}
```

| Component | Values |
| --------- | ------ |
| `tool_id` | `ffmpeg`, `yt-dlp`, `mp4decrypt`, `mp4box`, `nm3u8dlre`, `aria2c`, `fpcalc`, `get_iplayer`, `votify`, `gytmdl`, `gamdl`, `amdecrypt`, `wrapper`, `mediainfo`, `ofscraper`, `mkvtoolnix`, `potrace`, `vtracer`, `ddrescue` |
| `os` | `linux`, `windows`, `macos` |
| `arch` | `x86_64`, `x86`, `aarch64`, `armhf` |
| `ext` | `.tar.gz` (Linux/macOS), `.zip` (Windows), `.exe` (installers) |

Tools compiled from source in CI (potrace, VTracer, ddrescue) also ship the
exact source they were built from as `{tool_id}-{version}.src.{tar.gz,tar.lz}`
(e.g. `potrace-1.16.src.tar.gz`, `ddrescue-1.28.src.tar.lz`) — for the GPL
tools this is their corresponding source.

### MKVToolNix on Linux

`mkvtoolnix-linux-x86_64.tar.gz` is taken from the official AppImage. Its
programs only work next to the libraries the AppImage carries (Qt, Boost,
FLAC, ICU, GLib, GnuTLS, …), so the tarball ships them together:

```text
mkvmerge  mkvextract  mkvpropedit  mkvinfo   ← small launcher scripts — run these
libexec/                                     ← the unmodified upstream programs
lib/                                         ← libraries copied from the AppImage
```

Run the tools from the top level (or through a symlink to them); keep `lib/`
and `libexec/` next to them. The host only needs glibc, the C++/GCC runtime
(`libstdc++`, `libgcc_s`), zlib and GMP (`libgmp.so.10`), which every desktop
Linux has — the official AppImage expects the same. glibc must be 2.28 or
newer (Debian 10, Ubuntu 18.10, RHEL 8 and later). Each build
is checked by running `--version` from a fresh unpack before it is published.

## Releases

Each build creates a **date-stamped release** (e.g. `2026-02-25`) containing all tool binaries. The `latest` tag always points to the most recent build.

- **Date-stamped releases** — Permanent snapshots you can pin to (e.g. `2026-02-25`)
- **`latest` release** — Always updated to point to the newest build

## Automation

The **Populate Tools** workflow automatically:

1. **Checks upstream versions** against `versions.json` (on scheduled runs, only rebuilds tools with new versions)
2. Downloads latest binaries from upstream sources
3. Builds Python tools (Votify, gytmdl, gamdl, OF-Scraper) via PyInstaller for each platform
4. Repackages everything with standardized names
5. Creates a date-stamped release and updates the `latest` tag
6. Commits updated `versions.json` back to the repository

### Triggers

| Trigger | How | When to use |
| ------- | --- | ----------- |
| **Manual dispatch** | GitHub > Actions > Populate Tools > "Run workflow" | A specific tool was updated upstream |
| **Push to main** | Automatic on every commit to main | Workflow/config changes |
| **PR with label** | Add `update-tools` label to a PR | Testing workflow changes before merging |
| **Daily schedule** | Automatic every day at 06:00 UTC | Catches upstream updates promptly (only rebuilds changed tools) |

### How to update when an upstream tool releases a new version

No code changes are needed. The workflow always fetches the **latest** version from each upstream source. Simply:

1. Go to **Actions** > **Populate Tools** > **Run workflow**
2. Click the green **Run workflow** button
3. Wait for the build to complete (~6 minutes)
4. A new date-stamped release will appear with the updated tools

## Usage

Download assets from the [latest release](../../releases/tag/latest), or pin to a specific date:

```bash
# Always get the newest build
curl -fL -o yt-dlp.tar.gz \
  "https://github.com/MeedyaDL/MeedyaDL-Tools/releases/download/latest/yt-dlp-linux-x86_64.tar.gz"

# Pin to a specific date
curl -fL -o yt-dlp.tar.gz \
  "https://github.com/MeedyaDL/MeedyaDL-Tools/releases/download/2026-02-25/yt-dlp-linux-x86_64.tar.gz"
```

## Contributing

1. Fork and create a branch
2. Make your changes
3. Add the `update-tools` label to your PR to trigger a test build
4. Submit for review

See [DEV_Status.md](DEV_Status.md) for current build status and known issues.

## License

The automation code in this repository is MIT licensed. Bundled tool binaries are redistributed under their respective upstream licenses. See [LICENSE.md](LICENSE.md) for full details.
