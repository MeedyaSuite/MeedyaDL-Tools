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

**Y** = pre-built binary &nbsp; **Y*** = Perl script (requires Perl runtime) &nbsp; **Y**** = Windows installer &nbsp; **Y***** = x86_64 via Rosetta 2 &nbsp; **—** = not available

## Asset Naming Convention

```text
{tool_id}-{os}-{arch}.{ext}
```

| Component | Values |
| --------- | ------ |
| `tool_id` | `ffmpeg`, `yt-dlp`, `mp4decrypt`, `mp4box`, `nm3u8dlre`, `aria2c`, `fpcalc`, `get_iplayer`, `votify`, `gytmdl`, `gamdl`, `amdecrypt`, `wrapper`, `mediainfo`, `ofscraper`, `mkvtoolnix` |
| `os` | `linux`, `windows`, `macos` |
| `arch` | `x86_64`, `x86`, `aarch64`, `armhf` |
| `ext` | `.tar.gz` (Linux/macOS), `.zip` (Windows), `.exe` (installers) |

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
