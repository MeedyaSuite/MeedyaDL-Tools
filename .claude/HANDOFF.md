# Handoff — MeedyaDL-Tools

**Read this first in any new session.** Then read
[`STANDING_RULES.md`](STANDING_RULES.md) and [`CLAUDE.md`](CLAUDE.md).

Last updated: 2026-10-26 (MKVToolNix macOS fix)

---

## Where we are right now

- **Working branch:** `claude/great-galileo-116la9`, started from `alpha` at
  `bf5667b` (after PR #27, the consolidated WIP branch
  `claude/nifty-shannon-wla302`, was merged into `alpha`). It targets `alpha`
  through one pull request carrying the `update-tools` label (so CI runs the
  full build as a test, without publishing a release).
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

### MKVToolNix macOS fix (this branch)

- **Problem:** `mkvtoolnix-macos-*.tar.gz` had been missing from every release
  since at least 2026-09-01, yet CI stayed green. The old URL
  `mkvtoolnix.download/macos/MKVToolNix-<ver>.dmg` now returns an empty file,
  and the step's "no binaries found" branch exited 0.
- **Upstream did NOT drop macOS.** A pause was announced in April 2026, but
  community maintainers (corticalcode, touchstone64) restored official,
  notarised DMGs from v98/v99. They live at
  `macos/releases/<ver>/MKVToolNix-<ver>-<rev>-{arm64,x86_64,universal}.dmg`
  with `.sha256` files. `<rev>` is a repackaging number (starts at 1).
- **Why a new macOS job:** the DMGs are LZFSE-compressed APFS images. 7-Zip
  23.01, bsdtar and dmg2img on Linux can't read them (checked locally against
  an equivalent community DMG). The new `mkvtoolnix-macos` job runs on
  `macos-latest` and uses `hdiutil`.
- **What it does:** finds the newest DMG revision, checks SHA-256, copies
  mkvmerge/mkvextract/mkvpropedit/mkvinfo plus any bundled libraries they load,
  checks the CPU type, then runs `--version` from a clean unpack (x86_64 through
  Rosetta 2). Any failure gives an `::error::` annotation plus a run-summary
  line, and no asset. The step keeps `continue-on-error`, so one missing asset
  never blocks the rest of the release.
- **Assets:** `mkvtoolnix-macos-aarch64` (now native; it used to hold the
  Intel build) and new `mkvtoolnix-macos-x86_64`.
- **Still to check on the PR run:** the step log, not just the job status. If
  a library loads from `Contents/Frameworks` (outside `Contents/MacOS`), the
  step fails on purpose with a clear message and the packaging layout needs
  adjusting.

## Task queue

| # | Task | Status |
| - | ---- | ------ |
| 1 | Consolidate old branches + PRs #25/#26 into one WIP branch | ✅ Done |
| 2 | Open the consolidated PR to `alpha`, get CI green, merge | ✅ Done (PR #27 merged) |
| 2a | MKVToolNix macOS: restore the asset, make failures loud | ⏳ PR to `alpha` open, checking CI logs |
| 3 | GPL disc tools: cdrdao, cdparanoia, wodim (Linux), one at a time, each confirmed on CI | ⏳ Not started |
| 4 | GPL disc tools: macOS builds (reuse the `build-tracing-tools` macOS pattern) | ⏳ Not started |
| 5 | Thorough documentation pass (CHANGELOG history still says "monthly schedule") | ⏳ Not started |
| 6 | Device-wide AI fallback rule on the owner's own machines (`~/.claude/CLAUDE.md`, `~/.codex/AGENTS.md`) | ❓ Owner to do locally |

Open issues: #19 (publish SHA256SUMS — done on `main` by PR #20, could be
closed), #17 (add rclone), #16 (MeedyaSuite org secrets), #8 (automatic
upstream version checking — largely done by the `check-versions` job).

## How to pick up in a fresh session

1. `git fetch origin && git checkout claude/great-galileo-116la9 && git pull`
   (or, once merged, start a new branch from `alpha`).
2. Read this file, then `STANDING_RULES.md`, then `CLAUDE.md`.
3. Carry on with the task queue from the first unfinished item.
4. Keep this file updated as you go.
