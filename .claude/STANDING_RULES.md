# Standing Rules & Standing Tasks

These rules apply to **every** session on this repo, whichever AI tool is doing
the work (Claude Code, Codex, or another). They are the single source of truth.
`.claude/CLAUDE.md`, `.OpenAI/AGENTS.md` and the root `AGENTS.md` all point here.

Last revised: 2026-09-23 (set by the repo owner)

---

## 1. Plain English

When reporting back or explaining anything, use plain, everyday English. Avoid
technical jargon, even for technical readers. If a technical term can't be
avoided, explain it in a few simple words the first time it's used.

## 2. Keep the handoff document up to date

Update [`.claude/HANDOFF.md`](HANDOFF.md) **as you go**, not only at the end,
so work can be picked up at any moment if a session is cut off. The handoff
must always let a brand-new session, with no chat history, carry on straight
away.

## 3. How to think, plan and build

- Think hard about the work before starting. Use workflows to plan and do the work.
- Do deep analysis and deep planning with **Opus agents, one after another
  (not in parallel)**. The latest Opus (Opus 5.5 at time of writing) is cheaper
  than, and at least as good as, the latest Fable.
- Do the actual building with **Sonnet or Haiku**, whichever fits best. If the
  building work is complex, use **Opus**.
- Aim: use tokens/credits efficiently while still producing top-quality,
  correct code. **GIRFT — Get It Right First Time.**

## 4. Use plugins to help

- The `dev-team` plugins may be used for any of the work, and for suggesting
  extra fixes, tweaks, improvements and new features.
- Use them to **cross-check with a different AI**: if Claude plans and builds,
  Codex reviews (and the other way round).

## 5. Steps after each task

After each piece of work is finished:

1. Commit and push it to the working branch (the one that will later be merged
   into `alpha`), and update the related GitHub issue(s) — one update per task.
2. Update Claude memory and context in `.claude/`.
3. Update OpenAI/Codex memory and context in `.OpenAI/`.
4. Update the handoff document (`.claude/HANDOFF.md`).

## 6. Thorough documentation updates

When asked for a documentation update, cover **all** of:

- Every `.md` file in the repo.
- Any in-app help, guides, etc. (this repo has no app, so currently none).
- Claude memory, context and everything else in `.claude/`.
- If the project offers an API: its OpenAPI/Swagger docs. If it also has
  web-based parts and no Swagger UI, add Swagger UI, set up so it can run on
  ordinary shared web hosting (no Docker etc.).
  (This repo has no API and no web parts, so this doesn't currently apply.)

## 7. Work efficiently

Feel free to reorder the tasks above or bundle them together where that's
more efficient.

## 8. Work on your own

- Carry out all queued work without stopping, unless an **explicit** decision
  or approval from the owner is needed.
- If one is needed, say clearly and simply what's needed and why.
- Raise all questions and decisions **up front**, not one at a time as they
  come up.
- After getting an answer, carry on with **all** remaining queued work.

## 9. Progress updates

Give frequent progress updates as a table listing each queued task and its
status.

## 10. Code review loop

All code goes through review by **Codex**. Any problems it finds are fixed
automatically, then Codex reviews again — repeat until it finds nothing.

## 11. No PR stacking

Don't open several pull requests. Commit everything to the single working
branch that will later target `alpha` through **one** pull request, created
later. This avoids clashes when merging.

## 12. Fallback between AI tools

- If one AI service (e.g. Claude Code, Codex, or any other) or its agents
  becomes unavailable or runs out of credits, hand the work to another
  suitable one — as long as it can be done without losing context or progress.
- Switch back to the main AI service for the project as soon and as often as
  possible.
- Once the main service is back, do a **full** review of anything the fallback
  service did. The cross-AI reviews (rule 10) should catch differences in
  approach, but a full review is still required.
- This is why the handoff document (rule 2) must be kept up to the minute.
- The rule is tool-neutral: it doesn't depend on which AI tools are used.
- This rule also applies device-wide (see `~/.claude/CLAUDE.md` and
  `~/.codex/AGENTS.md` on the owner's machines).

---

## Existing repo conventions (still in force)

These were in place before and are unchanged. Full detail is in
[`.claude/CLAUDE.md`](CLAUDE.md):

- Shell steps start with `set -euo pipefail`.
- Downloads use `curl -fL --retry 3`.
- Log lines: `→` start, `✓` success, `⚠` warning.
- Platforms that often fail use `continue-on-error: true`.
- Markdown tables have spaces around the pipes (linter rule MD060).
- CI's own auto-commits end with `[skip ci]`.
- Asset names follow `{tool_id}-{os}-{arch}.{ext}`.
