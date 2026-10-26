# Codex / OpenAI Memory — MeedyaDL-Tools

Short-lived notes for Codex. The authoritative state is always
[`.claude/HANDOFF.md`](../.claude/HANDOFF.md) — if they disagree, the handoff wins.

- 2026-09-23: Standing rules and handoff created on branch
  `claude/upbeat-wright-178l0v`. No workflow code changed yet, so there is
  nothing for Codex to review from this session so far. Owner has a Codex
  review planned for 00:08.
- 2026-10-26: Old branches (`claude/upbeat-wright-178l0v`,
  `chore/claude-config-recovery-2026-07-20`,
  `feat/macos-arm64-ffmpeg-and-sha256sums`, `feat/gpl-disc-tools` / PR #25,
  `feat/vector-tracing-tools` / PR #26) consolidated into
  `claude/nifty-shannon-wla302` and deleted. `alpha` created from `main`.
  Workflow changes from that merge (potrace/vtracer/ddrescue builds, macOS
  ffmpeg hard-fail, ddrescue version gating) are ready for a Codex review.
- 2026-10-26: Branch `claude/kind-goodall-ygobby` (from `alpha`): MKVToolNix
  Linux asset now ships `libexec/` (unmodified programs) + `lib/` (AppImage
  libraries) + root launcher scripts, with a clean-unpack smoke test. Ready for
  a Codex review of the "MKVToolNix: Linux x86_64 (AppImage)" step.
  CI confirmed (run 40334308316): `--version` → v103.0 from a clean unpack;
  `libgmp.so.10` treated as host-provided (the AppImage does not bundle it).
