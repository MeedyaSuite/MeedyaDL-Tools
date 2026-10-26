# Handoff — MeedyaDL-Tools

**Read this first in any new session.** Then read
[`STANDING_RULES.md`](STANDING_RULES.md) and [`CLAUDE.md`](CLAUDE.md).

Last updated: 2026-10-26 (MKVToolNix Linux + macOS fixes consolidated)

---

## Current task — consolidated MKVToolNix Linux + macOS PR

- **Branch:** `claude/kind-goodall-ygobby`, targeting `alpha` through **one**
  PR with the `update-tools` label. On 2026-10-26 the owner asked for PR #28
  (macOS, branch `claude/great-galileo-116la9`) and PR #29 (Linux, this
  branch) to be merged into a single PR to avoid merge races; both were closed
  in favour of the consolidated PR and #28's branch was deleted (its commit
  `2e028d5` is merged into this branch). Plan: get every check green, merge
  into `alpha`, then watch the Actions runs that follow.
- **Conflict resolution:** the Linux AppImage step keeps this branch's version
  (it already fails loudly); the macOS job, Windows `::warning::`, and
  `upload-release` wiring come from #28; docs merged by hand (DEV_Status now
  has separate Linux and macOS rows).

### MKVToolNix Linux asset

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
- **CI run 1 (run 40331555265, commit e262668):**
  - Old `latest` asset on `ubuntu-latest`: `./mkvmerge --version` → exit 127;
    `libboost_filesystem.so.1.85.0`, `libFLAC.so.8`, `libQt6Core.so.6`,
    `libcmark.so.0.28.3`, `libdvdread.so.4` not found. Original bug confirmed.
    The temporary check step has now been removed.
  - New step stopped with `::error::` and produced no asset (as designed):
    `libgmp.so.10` is **not inside the AppImage** — upstream expects the host to
    provide it. The job still showed green because of `continue-on-error`.
  - The step now lists every such library at once (and what it bundled).
- **CI run 2 (run 40333164882, commit 3380cd2):** bundled 22 libraries
  (FLAC, Qt6Core, Boost atomic + filesystem, cmark, libdvdread, libffi, GLib +
  gthread, GnuTLS, Nettle/hogweed, ICU data/i18n/uc 56, libidn2, Ogg,
  p11-kit, PCRE, libtasn1, libunistring, Vorbis). Only `libgmp.so.10` is left
  to the host. Decision: treat GMP as host-provided, like the upstream
  AppImage does (added to `HOST_LIBS`, README + LICENSE.md updated with the
  real bundled list). Copying the runner's Ubuntu GMP was rejected: it mixes
  non-upstream builds into the tarball.
- **CI run 3 (run 40334308316, commit 12e2736):** passed. From a clean
  unpack all four tools print `v103.0 ('The Darkness') 64-bit`, also through
  a symlink. Tarball 24 MB; needs glibc ≥ 2.28 and GLIBCXX_3.4.21 (README +
  DEV_Status updated).

## Background
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

### MKVToolNix macOS fix (from PR #28, now in the consolidated branch)

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
| 2a | MKVToolNix macOS: restore the asset, make failures loud (was PR #28) | ⏳ In the consolidated PR; macOS step logs still to check |
| 3 | GPL disc tools: cdrdao, cdparanoia, wodim (Linux), one at a time, each confirmed on CI | ⏳ Not started |
| 4 | GPL disc tools: macOS builds (reuse the `build-tracing-tools` macOS pattern) | ⏳ Not started |
| 5 | Thorough documentation pass (CHANGELOG history still says "monthly schedule") | ⏳ Not started |
| 6 | Device-wide AI fallback rule on the owner's own machines (`~/.claude/CLAUDE.md`, `~/.codex/AGENTS.md`) | ❓ Owner to do locally |
| 7 | MKVToolNix Linux asset: bundle libraries + launchers, clean-unpack smoke test (was PR #29) | ✅ Passed on CI (run 40334308316); now in the consolidated PR |
| 8 | Consolidated PR (#28 + #29) → green → merge to `alpha` → watch post-merge Actions | ⏳ In progress |

Open issues: #19 (publish SHA256SUMS — done on `main` by PR #20, could be
closed), #17 (add rclone), #16 (MeedyaSuite org secrets), #8 (automatic
upstream version checking — largely done by the `check-versions` job).

## How to pick up in a fresh session

1. `git fetch origin && git checkout claude/great-galileo-116la9 && git pull`
   (or, once merged, start a new branch from `alpha`).
2. Read this file, then `STANDING_RULES.md`, then `CLAUDE.md`.
3. Carry on with the task queue from the first unfinished item.
4. Keep this file updated as you go.
