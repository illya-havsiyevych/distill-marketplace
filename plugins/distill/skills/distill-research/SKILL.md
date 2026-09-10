---
name: distill-research
description: Distill a raw deep-research output into three files — a terse results report agents load by default, a findings file of typed evidence under mnemonic keys, and the untouched reasoning original. Use when the user has a deep-research report and wants it turned into reviewable, reusable knowledge. Orchestrates distill-extract and distill-verify, each in a forked context.
argument-hint: <path-to-raw-research.md>
---

# distill-research

Umbrella. Runs in the main conversation. The two stages run forked, so neither sees this conversation or each other.

| Stage | Skill | Context | Writes |
|---|---|---|---|
| intake | this skill | main | `STEM--reasoning.md` |
| extract | `distill-extract` | fork | `STEM--findings.md`, `STEM--draft.md` |
| verify | `distill-verify` | fork | `STEM.md`, `research/review-queue.md`; deletes draft |

## Run

1. **Intake.** Copy `$0` verbatim to `research/YYYY-MM-DD-<slug>--reasoning.md`. Prepend one line: `<!-- source: <tool> · <date> -->`. Slug: lowercase, hyphens, at most eight words from the title. Let `STEM` = `research/YYYY-MM-DD-<slug>`.
2. Invoke `/distill-extract STEM`. Wait for its result.
3. Invoke `/distill-verify STEM`. Wait for its result.
4. Tell the user: report line count, queue size, and both stage results verbatim.

Open no research file yourself. Paths only.

## If the fork was not honored

Some Claude Code versions run forked skills inline (anthropics/claude-code #17283, #49559). If a stage's working output appears in this conversation instead of returning as a single result line, stop. Re-run that stage with the Task tool — `subagent_type: general-purpose`, prompt: `Run the distill-extract skill on STEM` (or `distill-verify`). Never continue inline. The point is that verify does not remember extract.

## Read rules — copy these to AGENTS.md or CLAUDE.md

They apply to every agent in the repo, not only this skill.

- Default research context is `research/*.md` **excluding** `*--reasoning.md`, `*--findings.md`, `*--draft.md`.
- A result line outranks search results, your own reasoning, and findings. Do not re-research it.
- Never edit `--reasoning.md`. Never write under `## [human-review]`.
- New evidence contradicts a result line → append to `research/review-queue.md`. Do not edit the line.

## After human review

The human edits the report directly and merges. Sign-off is a source. Record it in findings:

```
## [human-review]
reviewed: YYYY-MM-DD
decisions:
  - <what changed and the one-line reason>
```

A line the human asserted without another source cites `[human-review]`.

## Improving the stages

`/distill-improve` is the only skill that reads `--findings.md` outside this flow. Run it periodically. It proposes rule changes and never applies them.
