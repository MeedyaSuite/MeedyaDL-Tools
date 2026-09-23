# MeedyaDL-Tools — Claude Context

> **Start here:** read [`HANDOFF.md`](HANDOFF.md) (current state of play) and
> [`STANDING_RULES.md`](STANDING_RULES.md) (how we work — plain English, handoff
> kept up to date, Opus to plan / Sonnet-Haiku to build, Codex review loop, one
> working branch, AI-tool fallback). Both apply to every session.

## Purpose

Mirror repository for external tool binaries used by [MeedyaDL](https://github.com/MWBMPartners/MeedyaDL). No application source code — only CI workflows, documentation, and configuration.

## Architecture

- All tool binaries live in GitHub Release assets (date-stamped + `latest` tag), not in the git tree
- `populate.yml` is the core file (~1580 lines) — 4-job workflow:
  1. **check-versions** — compares upstream versions against `versions.json`; on schedule only rebuilds changed tools; on push/dispatch forces full rebuild
  2. **download-binaries** — downloads pre-built tools from upstream (per-tool `if:` conditions skip unchanged tools)
  3. **build-python-tools** — builds Votify/gytmdl/gamdl/OF-Scraper via PyInstaller across 4 platform matrix runners
  4. **upload-release** — carries forward unchanged assets from previous release, creates date-stamped release + updates `latest` tag, auto-commits `versions.json`
- `versions.json` tracks last-packaged upstream versions (auto-committed by CI with `[skip ci]`)

## Tools Mirrored (16)

FFmpeg, yt-dlp, mp4decrypt, MP4Box, N_m3u8DL-RE, aria2c, fpcalc, get_iplayer, Votify, gytmdl, gamdl, AMdecrypt, Wrapper, MediaInfo, OF-Scraper, MKVToolNix

## Asset Naming

`{tool_id}-{os}-{arch}.{ext}` — e.g. `yt-dlp-linux-x86_64.tar.gz`, `mediainfo-windows-aarch64.zip`

## Conventions

- Shell steps: `set -euo pipefail`
- Downloads: `curl -fL --retry 3`
- Progress logging: `→` start, `✓` success, `⚠` warning
- Unreliable platforms: `continue-on-error: true`
- Markdown tables: spaces around pipes (MD060 linter rule)
- CI auto-commits: `[skip ci]` suffix to avoid retriggering

## Releases

- Date-stamped releases (e.g. `2026-03-27`) are marked as GitHub "Latest"
- `latest` tag release created with `--latest=false` for stable download URLs
- On multiple runs per day, tags get `.N` suffix (e.g. `2026-03-27.2`)

## Triggers

| Trigger | Behaviour |
| ------- | --------- |
| Push to main | Full rebuild of all tools |
| Manual dispatch | Full rebuild of all tools |
| PR with `update-tools` label | Full rebuild (test only, no release) |
| Daily schedule (06:00 UTC) | Smart rebuild — only changed tools |

## Key Files

| File | Purpose |
| ---- | ------- |
| `.github/workflows/populate.yml` | Main CI workflow |
| `versions.json` | Last-packaged upstream version tracker |
| `README.md` | Public docs with tool/platform matrix |
| `DEV_Status.md` | Build status and known limitations |
| `LICENSE.md` | MIT for repo + per-tool upstream licenses |
| `.github/dependabot.yml` | GitHub Actions version updates (monthly) |
| `.claude/HANDOFF.md` | Current state of play — update as you go |
| `.claude/STANDING_RULES.md` | Standing rules for all AI tools |
| `.OpenAI/AGENTS.md` | Codex/OpenAI context (points to the same rules) |
