# Handoff — MeedyaDL-Tools

**Read this first in any new session.** Then read
[`STANDING_RULES.md`](STANDING_RULES.md) and [`CLAUDE.md`](CLAUDE.md).

Last updated: 2026-09-23 ~20:00 UTC

---

## Where we are right now

- **Working branch:** `claude/upbeat-wright-178l0v` (started from `main` at
  `96980a3`). All new work goes here. It will later go into `alpha` through
  one pull request (not created yet — see open questions).
- **This session so far:** only housekeeping — wrote the standing rules, this
  handoff, and the Codex/OpenAI context files. **No workflow or tool changes
  have been made yet.**
- **`main` is healthy:** the daily build ran today and published release
  `2026-09-23` plus the `latest` tag. 16 tools are mirrored (17 entries in
  `versions.json`, because macOS ARM FFmpeg is tracked separately).
- **Upcoming:** the owner mentioned a **Codex review at 00:08**. No scheduled
  job for it was found in this account, so it's presumably run from the owner's
  own machine. The owner may restart the session before then to update Claude
  Code — nothing here depends on this session staying alive.

## Task queue

| # | Task | Status |
| - | ---- | ------ |
| 1 | Write standing rules (`.claude/STANDING_RULES.md`) | ✅ Done |
| 2 | Write this handoff document | ✅ Done |
| 3 | Add Codex/OpenAI context in `.OpenAI/` + root `AGENTS.md` pointer | ✅ Done |
| 4 | Add AI-tool fallback rule device-wide (`~/.claude/CLAUDE.md`, `~/.codex/AGENTS.md`) | ⚠ Done in the cloud container only — it gets wiped; needs repeating on the owner's own machine(s) |
| 5 | Commit and push all of the above | ✅ Done (see git log on the branch) |
| 6 | Thorough documentation update (all `.md` files, `.claude/`, `.OpenAI/`) | ⏳ Queued — not started. Known stale bits: `CHANGELOG.md` still says "monthly schedule" and lists only the first 9 new tools; `.claude/CLAUDE.md` says `populate.yml` is ~1340 lines (now 1579) |
| 7 | Codex review loop on whatever code changes come next | ⏳ Waiting on the 00:08 Codex review / Codex being available |
| 8 | Decide what happens to the older open pull requests and branches (below) | ❓ Needs owner decision |

## Older work still in flight (from before this session)

Open pull requests, all aimed at `main`:

| PR | What it does | State |
| -- | ------------ | ----- |
| [#26](https://github.com/MeedyaSuite/MeedyaDL-Tools/pull/26) | Builds potrace + vtracer (vector tracing tools for MeedyaConverter) | Open, has `update-tools` label, last touched 2026-09-03 |
| [#25](https://github.com/MeedyaSuite/MeedyaDL-Tools/pull/25) | Starts building GPL optical-disc tools (ddrescue first) for MeedyaConverter | Open, last touched 2026-09-02; cdrdao/cdparanoia/wodim still to do |
| [#23](https://github.com/MeedyaSuite/MeedyaDL-Tools/pull/23) | Dependabot: update `actions/checkout` 6.0.2 → 7.0.1 | Open |

Branches with no pull request:

| Branch | What it holds |
| ------ | ------------- |
| `feat/macos-arm64-ffmpeg-and-sha256sums` | Real Apple-Silicon FFmpeg + publishing SHA256SUMS checksums (meant to close issues #15 and #19) |
| `chore/claude-config-recovery-2026-07-20` | Recovers `.claude/settings.json` (turns on the `dev-team` plugin) and an old local permissions file |
| `ci/actionlint-workflow-lint` | Already merged (PR #21) — safe to delete |

Open issues: #19 (publish SHA256SUMS), #17 (add rclone), #16 (MeedyaSuite org
secrets), #8 (automatic upstream version checking — largely done already by
the `check-versions` job; could probably be closed).

## Open questions for the owner

1. **`alpha` branch doesn't exist in this repo** — only `main`. Should I
   create `alpha` from `main`, or should the final pull request go to `main`?
2. **Old pull requests #25 / #26 and the SHA256SUMS branch** — they conflict
   with the "one working branch, no stacked PRs" rule. Fold them into the
   working branch, leave them as they are, or close them?
3. **`dev-team` plugin** is only switched on in the unmerged
   `chore/claude-config-recovery-2026-07-20` branch. Bring its
   `.claude/settings.json` into the working branch so the plugin is on by
   default?
4. **Device-wide rule** — this cloud session can't write to your own
   computer. Next time you run Claude Code/Codex locally, ask it to copy
   rule 12 from `STANDING_RULES.md` into `~/.claude/CLAUDE.md` and
   `~/.codex/AGENTS.md`.

## How to pick up in a fresh session

1. `git fetch origin && git checkout claude/upbeat-wright-178l0v && git pull`
2. Read this file, then `STANDING_RULES.md`, then `CLAUDE.md`.
3. Get answers to the open questions above (if not already answered).
4. Carry on with the task queue from the first unfinished item.
5. Keep this file updated as you go.
