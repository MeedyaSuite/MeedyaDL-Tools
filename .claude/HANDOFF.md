# Handoff — MeedyaDL-Tools

**Read this first in any new session.** Then read
[`STANDING_RULES.md`](STANDING_RULES.md) and [`CLAUDE.md`](CLAUDE.md).

Last updated: 2026-10-26 (MKVToolNix Linux packaging fix)

---

## Current task — MKVToolNix Linux asset (branch `claude/kind-goodall-ygobby`)

- **Problem:** `mkvtoolnix-linux-x86_64.tar.gz` held only the four programs
  copied out of the AppImage. They need Qt 6, Boost, FLAC, cmark, libdvdread,
  GMP, Vorbis/Ogg from inside the AppImage (plus an RPATH pointing at the
  upstream build machine), so they fail with "error while loading shared
  libraries". Reproduced locally on Ubuntu 24.04 with the current `latest`
  asset: `./mkvmerge --version` → exit 127, `libboost_filesystem.so.1.85.0`
  missing.
- **MeedyaDL check:** MeedyaDL (`main`, `alpha`, `work-in-progress`) does not
  use MKVToolNix yet. Its mirror installer unpacks a tarball into
  `tools/<tool_id>/` and expects programs at the top level, so the top level
  must keep `mkvmerge` etc.
- **Fix (in `download-binaries`, step "MKVToolNix: Linux x86_64 (AppImage)"):**
  the unmodified programs go in `libexec/`, every library `ldd` says they load
  from the AppImage goes in `lib/` (glibc, libstdc++, libgcc_s and zlib are left
  to the host), and a small `/bin/sh` launcher at the top level sets
  `LD_LIBRARY_PATH` and runs the real program. No `patchelf`, so LICENSE.md's
  "unmodified" wording still holds (a note about the bundled libraries was
  added). The tarball is unpacked fresh and checked: every library must come
  from `lib/` or the host set, and `--version` must report `v<ver>` (also via a
  symlink). Any failure → `::error::`, run-summary line, no asset (EXIT trap),
  `continue-on-error` kept — same pattern as the macOS job in PR #28.
- **Temporary step** "MKVToolNix: Linux x86_64 — check current release asset
  (temporary)" runs the *old* `latest` asset on CI to confirm the failure.
  Remove it once its log has been read.
- **Overlap with PR #28** (open, also targets `alpha`): it changes the same
  Linux step's "no binaries" branch to `::error::` + `exit 1`. Whichever PR
  merges second takes this branch's version of that step.
- **Status:** see the task queue row 7 below.

- **Working branch:** `claude/nifty-shannon-wla302`, started from `main` at
  `18e4eb3` (2026-10-25). It is the **single** work-in-progress branch and
  targets `alpha` through one pull request.
- **`alpha` branch:** created from `main` on 2026-10-26 (it did not exist
  before). Pushes to `alpha` do **not** cut a release — only pushes to `main`
  do. Getting the work released means a later `alpha` → `main` merge.
- **Branch clean-up (2026-10-26):** five older branches were merged into the
  working branch and then deleted. Nothing from them was dropped, except the
  one file noted below.

| Old branch | What it held | Status of the work |
| ---------- | ------------ | ------------------ |
| `feat/vector-tracing-tools` (PR #26) | Builds potrace + vtracer from source for MeedyaConverter | Complete. Its CI build was checked: real binaries on all 3 runners |
| `feat/gpl-disc-tools` (PR #25) | GPL optical-disc tools — ddrescue built from source + source archive; cdrdao/cdparanoia/wodim pinned only | ddrescue done; the other three are still to do (see `DEV_Status.md`) |
| `feat/macos-arm64-ffmpeg-and-sha256sums` | Follow-up commit pushed 3 minutes *after* PR #20 merged, so it never reached `main`: macOS ffmpeg/ffprobe failure now fails the build; ffplay stays optional | Complete |
| `chore/claude-config-recovery-2026-07-20` | `.claude/settings.json` (turns on the `dev-team` plugin) + `.claude/settings.local.json` | `settings.json` kept. `settings.local.json` **not** kept: it is a personal, machine-specific permissions file (old Mac paths, one-off commands) that `.gitignore` deliberately keeps out of git. It is still in commit `609c856` if ever wanted |
| `claude/upbeat-wright-178l0v` | Standing rules, this handoff, Codex/OpenAI context | Complete (this file has since been rewritten) |

### Fixes made while joining the branches

- PR #25 + PR #26 both added version pins in the same places — kept both.
- ddrescue is now wired into `check-versions` like potrace/vtracer (it used to
  rebuild on every run, and the daily `versions.json` auto-commit would have
  silently removed its entry).
- cdrdao / cdparanoia / wodim removed from `versions.json` (they are not built
  yet; the daily auto-commit would have removed them anyway). Their pins stay in
  the `env:` block.
- The release "flatten" step now keeps `.tar.lz` files (fix from PR #26). Without
  it, PR #25's `ddrescue-<ver>.src.tar.lz` GPL source archive would have been
  silently left out of every release.
- New job uses `actions/checkout@v7.0.1`, matching the rest of the workflow.
- Docs: README, DEV_Status (table column count fixed), LICENSE.md, CHANGELOG,
  CLAUDE.md updated for the 3 new tools.
- **macOS Intel FFmpeg source replaced.** The first CI run of the consolidated
  PR showed evermeet.cx returning HTTP 410 (Gone). Checking `main`'s logs: it has
  failed every run since 2026-10-13, but `main` hid it (`continue-on-error`) and
  each release carried forward old macOS ffmpeg/ffprobe/ffplay copies from
  different dates. x86_64 now comes from ffmpeg.martin-riedl.de (evermeet.cx
  kept as fallback), and each macOS binary is checked to be the right
  architecture before packaging.

## Task queue

| # | Task | Status |
| - | ---- | ------ |
| 1 | Consolidate old branches + PRs #25/#26 into one WIP branch | ✅ Done |
| 2 | Open the consolidated PR to `alpha`, get CI green, merge | ⏳ In progress |
| 3 | GPL disc tools: cdrdao, cdparanoia, wodim (Linux), one at a time, each confirmed on CI | ⏳ Not started |
| 4 | GPL disc tools: macOS builds (reuse the `build-tracing-tools` macOS pattern) | ⏳ Not started |
| 5 | Thorough documentation pass (CHANGELOG history still says "monthly schedule") | ⏳ Not started |
| 6 | Device-wide AI fallback rule on the owner's own machines (`~/.claude/CLAUDE.md`, `~/.codex/AGENTS.md`) | ❓ Owner to do locally |
| 7 | MKVToolNix Linux asset: bundle libraries + launchers, clean-unpack smoke test (branch `claude/kind-goodall-ygobby`) | ⏳ Built, waiting on CI |

Open issues: #19 (publish SHA256SUMS — done on `main` by PR #20, could be
closed), #17 (add rclone), #16 (MeedyaSuite org secrets), #8 (automatic
upstream version checking — largely done by the `check-versions` job).

## How to pick up in a fresh session

1. `git fetch origin && git checkout claude/nifty-shannon-wla302 && git pull`
   (or, once merged, start a new branch from `alpha`).
2. Read this file, then `STANDING_RULES.md`, then `CLAUDE.md`.
3. Carry on with the task queue from the first unfinished item.
4. Keep this file updated as you go.
