---
name: distill-research
description: Distill a raw deep-research output into three files — a report of human-reviewable logical blocks that agents load by default, a findings file of typed evidence under mnemonic keys, and the untouched reasoning original. Use when the user has a deep-research report with sources and wants it turned into reviewable, reusable knowledge, or wants an existing document verified line by line against findings from one or more distilled reports. Orchestrates distill-extract and distill-verify.
argument-hint: <path-to-raw-research.md>
---

# distill-research

Umbrella. Runs in the main conversation. The two stages run forked where the harness supports it, so neither sees this conversation or each other.

| Stage | Skill | Context | Writes |
|---|---|---|---|
| intake | this skill | main | `STEM--reasoning.md` |
| extract | `distill-extract` | fork | `STEM--findings.md`, `STEM--draft.md` |
| verify | `distill-verify` | fork | `STEM.md` (no suffix — this is the report), `research/review-queue.md`; deletes draft |

## Canary — before anything else

Your first line of output, before any tool call: `STEM = research/YYYY-MM-DD-<slug>` with the real date and slug, then the three paths it implies. If you cannot produce that line from this file, the skill did not load and you are improvising. Stop and say so.

## Run

0. **Source check.** Open nothing, but confirm the input file contains citations — URLs, DOIs, `[n]` refs, or a references section. If it has none, it is already a synthesis: if `research/*--findings.md` files exist for its topic, route it to *Verify an existing document* below instead of extract; if none exist, stop and ask the user for the raw research it was built from.
1. **Intake.** Copy `$0` verbatim to `research/YYYY-MM-DD-<slug>--reasoning.md`. Prepend one line: `<!-- source: <tool> · <date> -->`. Slug: lowercase, hyphens, at most eight words from the title. Let `STEM` = `research/YYYY-MM-DD-<slug>`.
2. Invoke `distill-extract` with `STEM`. Wait for its result.
3. Invoke `distill-verify` with `STEM`. Wait for its result.
4. Tell the user: report line count, queue size, both stage results verbatim, and whether isolation was achieved (see below).

Open no research file yourself beyond step 0. Paths only.

## Verify an existing document

To check a synthesized document — a playbook, a summary, someone else's report — against findings you already have:

```
distill-verify STEM <path-to-document> <findings-1> [<findings-2> ...]
```

Verify treats the document as the draft and the listed findings as its evidence. It writes `STEM.md` and does not delete the document. Lines with no supporting key across any findings file are cut and logged — that is the answer to "does this playbook hold up."

## If isolation was not achieved

Three cases, in order of likelihood:

- **Harness has no subagents** (Cowork, claude.ai, Codex without `exec`): the stages run inline and verify will remember extract. Run them anyway, then tell the user plainly: *isolation not achieved — for a fresh-look verify, run `distill-verify STEM` in a new chat.* That standalone run is real isolation.
- **Fork not honored** (some Claude Code versions — anthropics/claude-code #17283, #49559): a stage's working output appears in this conversation instead of a single result line. Stop. Re-run with the Task tool, `subagent_type: general-purpose`, prompt `Run the distill-verify skill on STEM`.
- **Slash invocation stalls** (Cowork): invoke the stages in natural language instead of `/distill-extract`.

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

`distill-improve` is the only skill that reads `--findings.md` outside this flow. Run it periodically. It proposes rule changes and never applies them.
