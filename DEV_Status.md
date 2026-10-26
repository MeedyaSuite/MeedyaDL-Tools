# Development Status

Current build status and tool availability for MeedyaDL-Tools.

## Build Status

| Job | Status |
| --- | ------ |
| Download Binaries | ![workflow](https://github.com/MeedyaDL/MeedyaDL-Tools/actions/workflows/populate.yml/badge.svg) |

## Tool-by-Platform Availability

| Tool | linux-x86_64 | linux-aarch64 | linux-x86 | linux-armhf | win-x86_64 | win-aarch64 | macos-aarch64 |
| ---- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| FFmpeg | OK | OK | — | — | OK | — | OK |
| yt-dlp | OK | OK | — | — | OK | OK | OK |
| mp4decrypt | OK | — | — | — | OK | — | OK |
| MP4Box | OK | — | — | — | OK | — | OK |
| N_m3u8DL-RE | OK | OK | — | — | OK | OK | OK |
| aria2c | OK* | — | — | — | OK | — | OK** |
| fpcalc | OK | OK | — | — | OK | — | OK |
| get_iplayer | OK*** | OK*** | — | OK*** | OK**** | — | OK*** |
| Votify | OK | OK | — | — | OK | — | OK |
| gytmdl | OK | OK | — | — | OK | — | OK |
| gamdl | OK | OK | — | — | OK | — | OK |
| AMdecrypt | OK | OK | OK | — | OK | OK | OK |
| Wrapper | OK | — | — | — | — | — | — |
| MediaInfo | OK | OK | — | — | OK | OK | OK |
| OF-Scraper | OK | OK | — | — | OK | — | OK |
| MKVToolNix | OK | — | — | — | OK | — | OK***** |
| potrace | OK* | OK* | — | — | — | — | OK* |
| vtracer | OK* | OK* | — | — | — | — | OK* |
| ddrescue | OK* | — | — | — | — | — | — |

**Legend:**

- **OK** = Pre-built binary downloaded from upstream
- **OK*** = Compiled from source in CI
- **OK**** = Installed via Homebrew in CI
- **OK***** = Perl script (requires Perl runtime on target)
- **OK****** = Windows installer (.exe)
- **OK******* = Native Apple Silicon binary; a separate macOS x86_64 (Intel) asset is also published
- **—** = Not available (no upstream binary, cannot cross-compile)

## Known Limitations

### Platform Gaps

| Platform | Issue | Impact |
| -------- | ----- | ------ |
| **Linux x86 (32-bit)** | Most upstream projects have dropped 32-bit support | Only AMdecrypt provides 32-bit Linux builds |
| **Windows ARM64** | Very few tools provide native ARM64 builds | Only yt-dlp, N_m3u8DL-RE, AMdecrypt, and MediaInfo available |
| **Linux armhf** | Raspberry Pi 3 (32-bit ARM) has minimal support | Only get_iplayer (Perl script) available |
| **Wrapper** | Only supports Linux x86_64 | C/C++ project with no cross-platform releases |
| **MKVToolNix (Linux aarch64)** | No portable aarch64 build | AppImage only available for x86_64 |

### Build Notes

| Tool | Notes |
| ---- | ----- |
| **aria2c (Linux)** | Compiled from source; linked against system OpenSSL |
| **aria2c (macOS)** | Installed via Homebrew; dynamically linked |
| **Python tools** | Built with PyInstaller `--onefile`; includes Votify, gytmdl, gamdl, OF-Scraper |
| **get_iplayer** | Perl script on Unix (requires `perl`); Windows installer bundles Perl |
| **FFmpeg (macOS)** | arm64 from osxexperts.net; x86_64 from ffmpeg.martin-riedl.de (evermeet.cx, the previous source, returns 410 Gone since 2026-10-13). ffmpeg/ffprobe failures fail the build; ffplay is optional |
| **MP4Box** | Extracted from platform-specific installers (.deb, .exe, .pkg) |
| **MediaInfo** | Binaries from mediaarea.net (not GitHub releases); macOS DMG extracted via 7z |
| **MKVToolNix** | Binaries from mkvtoolnix.download; Linux via AppImage extraction — the unmodified programs go in `libexec/` with the AppImage's libraries in `lib/` and launcher scripts at the root (needs glibc ≥ 2.28 plus the host's libstdc++/zlib/GMP), and `--version` is run from a clean unpack before the asset is kept (before 2026-10-26 the Linux asset shipped the programs alone and could not start outside the AppImage) |
| **MKVToolNix (macOS)** | Official notarised DMGs (`macos/releases/<ver>/MKVToolNix-<ver>-<rev>-{arm64,x86_64}.dmg`, built for upstream by community maintainers since v98/v99 after a brief pause announced in April 2026). Packaged in the `mkvtoolnix-macos` job on `macos-latest` because the DMGs are LZFSE-compressed APFS images Linux tools can't open; SHA-256 checked, CLI tools copied with any bundled libraries, smoke-tested from a clean unpack. Failure = `::error::` annotation, no asset |
| **potrace/vtracer (macOS)** | Compiled on `macos-latest`; x86_64 cross-built from the arm64 host; two thin assets, lipo'd by the consumer |

## Version Tracking

Tool versions are tracked in [`versions.json`](versions.json).
On scheduled runs, the workflow compares upstream versions against this file
and only rebuilds tools with new releases. On push and manual dispatch, all
tools are rebuilt. Check the
[latest release](https://github.com/MeedyaDL/MeedyaDL-Tools/releases/tag/latest)
for current versions.

## Upstream Repositories

| Tool | Repository | Release Strategy |
| ---- | ---------- | ---------------- |
| FFmpeg | [BtbN/FFmpeg-Builds](https://github.com/BtbN/FFmpeg-Builds) | Rolling `latest` tag |
| yt-dlp | [yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp) | Semver releases |
| mp4decrypt | [Bento4 SDK](https://www.bok.net/Bento4/) | Fixed version (pinned) |
| MP4Box | [GPAC](https://gpac.io/) | Nightly builds |
| N_m3u8DL-RE | [nilaoda/N_m3u8DL-RE](https://github.com/nilaoda/N_m3u8DL-RE) | Semver releases |
| aria2c | [aria2/aria2](https://github.com/aria2/aria2) | Semver releases |
| fpcalc | [acoustid/chromaprint](https://github.com/acoustid/chromaprint) | Semver releases |
| get_iplayer | [get-iplayer/get_iplayer](https://github.com/get-iplayer/get_iplayer) | Semver releases |
| Votify | [glomatico/votify](https://github.com/glomatico/votify) | PyPI releases |
| gytmdl | [glomatico/gytmdl](https://github.com/glomatico/gytmdl) | PyPI releases |
| gamdl | [glomatico/gamdl](https://github.com/glomatico/gamdl) | PyPI releases |
| AMdecrypt | [glomatico/amdecrypt](https://github.com/glomatico/amdecrypt) | GoReleaser |
| Wrapper | [WorldObservationLog/wrapper](https://github.com/WorldObservationLog/wrapper) | Commit-hash tags |
| MediaInfo | [MediaArea/MediaInfo](https://github.com/MediaArea/MediaInfo) | Semver releases (binaries on mediaarea.net) |
| OF-Scraper | [datawhores/OF-Scraper](https://github.com/datawhores/OF-Scraper) | PyPI releases |
| MKVToolNix | [mkvtoolnix.download](https://mkvtoolnix.download/) | Semver releases (binaries on mkvtoolnix.download) |

---

## GPL optical-disc tools (started 2026-09-02, PR #25 — now on the consolidated WIP branch)

Feeds MeedyaConverter's disc-backup-imaging feature (MeedyaConverter#492/#495),
Direct distribution only, per that repo's DR-0001. All four tools are pinned in
the `env:` block of `populate.yml`; only `ddrescue` (the one actually built) is
tracked in `versions.json`, because `versions.json` records what was packaged.

**Done:**
- `ddrescue` (GPLv3) — Linux x86_64 build **from source**, with the source
  tarball archived to the release as `ddrescue-<ver>.src.tar.lz`. This is the
  working template: fetch upstream source → build → stage binary → stage
  source. It is portable, DRM-neutral, and generally useful for any
  block-device imaging. Wired into `check-versions` like potrace/vtracer, so
  it only rebuilds when `DDRESCUE_VERSION` changes (or on a forced rebuild).

**Follow-ups (same pattern; each needs this repo's CI to validate the build):**
- `cdrdao` (GPLv2), `cdparanoia` (GPLv2), `wodim` / cdrkit (GPLv2) — Linux
  builds. Their builds are finicky (autotools of varying vintage, external
  deps: libao/libvorbis/libmad for cdrdao) and were not authored blind; add
  them one at a time and confirm each on CI.
- **macOS (arm64 + x86_64) builds.** The `build-tracing-tools` job (from
  PR #26) now provides a `macos-latest` compile runner that builds two thin
  per-arch binaries (x86_64 cross-built from the arm64 host). The disc tools
  can follow that same pattern; the consumer `lipo`-combines them.

**Do not** treat the disc-tool supply chain as complete until the macOS builds
land and each tool's CI is green. The MeedyaConverter side (App-Store exclusion
guards, DR-0001) is already merged on its `wip/alpha-consolidation` branch.
