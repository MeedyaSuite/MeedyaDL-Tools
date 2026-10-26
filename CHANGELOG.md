# Changelog

All notable changes to MeedyaDL-Tools will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Added
- **potrace + VTracer**: compiled from source for MeedyaConverter vector tracing (Linux x86_64/aarch64, macOS aarch64 + x86_64), version-pinned, with source tarballs attached to each release
- **GNU ddrescue**: compiled from source (Linux x86_64) with its GPL source tarball attached to each release — first of the GPL optical-disc tools for MeedyaConverter
- **SHA256SUMS**: every release publishes a `SHA256SUMS` asset; macOS FFmpeg is now a real arm64 build plus x86_64
- **AI tooling context**: `.claude/HANDOFF.md`, `.claude/STANDING_RULES.md`, `.claude/settings.json`, `.OpenAI/`, root `AGENTS.md`
- **9 new tools**: yt-dlp, aria2c, fpcalc, get_iplayer, Votify, gytmdl, gamdl, AMdecrypt, Wrapper
- **Expanded platform support**: Linux aarch64, Windows ARM64, macOS ARM (Apple Silicon), Linux armhf (Raspberry Pi)
- **PyInstaller builds**: Standalone executables for Python tools (Votify, gytmdl, gamdl) across all platforms
- **Multi-job CI workflow**: Parallel builds for binary downloads and Python tool compilation
- **CI triggers**: Push to main, PR with `update-tools` label, daily schedule, manual dispatch
- **GitHub Issue templates**: Bug report, feature request, tool update request
- **Pull request template**: Standardized PR checklist
- **Dependabot**: Automated GitHub Actions version updates
- **Documentation**: LICENSE.md, CHANGELOG.md, DEV_Status.md, .claude/CLAUDE.md
- **.gitignore**: Exclude build artifacts, OS files, local settings
- **.gitattributes**: Collapse .claude/ in GitHub UI, normalize line endings

### Changed
- **CI schedule**: daily (06:00 UTC) smart rebuild — only tools whose upstream version changed are rebuilt
- **macOS FFmpeg**: a failure to fetch ffmpeg/ffprobe now fails the build instead of silently shipping a release without them; ffplay stays optional
- **Release assets**: `.tar.xz`, `.tar.lz` and `.tar.bz2` files are no longer silently dropped when assembling a release
- **populate.yml**: Complete rewrite from single-job to multi-job architecture
- **README.md**: Expanded with full tool listing, platform matrix, and usage instructions
- **Asset naming**: Standardized to `{tool}-{os}-{arch}.{ext}` convention

## [0.1.0] - 2026-02-22

### Added
- Initial repository setup
- Populate Tools workflow for FFmpeg, mp4decrypt, N_m3u8DL-RE, MP4Box
- Support for Linux x86_64, Windows x86_64, macOS (via Rosetta 2)
- Monthly automated updates
- Basic README documentation
