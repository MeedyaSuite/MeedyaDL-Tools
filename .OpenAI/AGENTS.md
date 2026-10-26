# MeedyaDL-Tools — Codex / OpenAI Context

This is the Codex (and any other OpenAI tool) context for this repo. It mirrors
the Claude context so either tool can pick up the work.

## Read these first (shared by all AI tools)

1. [`.claude/HANDOFF.md`](../.claude/HANDOFF.md) — current state of play and task queue.
2. [`.claude/STANDING_RULES.md`](../.claude/STANDING_RULES.md) — how we work. These rules bind Codex too.
3. [`.claude/CLAUDE.md`](../.claude/CLAUDE.md) — project facts (architecture, tools, naming, conventions).

## What this repo is

A mirror of third-party tool binaries used by MeedyaDL. No app code — just the
GitHub Actions workflow (`.github/workflows/populate.yml`), `versions.json`,
and docs. Binaries live in GitHub Releases, not in git.

## Codex's usual role here

- **Reviewer:** Claude plans and builds; Codex reviews. Findings get fixed and
  re-reviewed until Codex finds nothing (standing rule 10).
- **Fallback builder:** if Claude is unavailable, Codex may carry on the work
  (standing rule 12). Update `.claude/HANDOFF.md` as you go so Claude can take
  back over, and note what you did so it gets a full review.

## Rules in brief

- Plain English when reporting back.
- One working branch (see handoff); no extra pull requests.
- After each task: commit + push, update the GitHub issue, update `.claude/`,
  `.OpenAI/` and the handoff.
