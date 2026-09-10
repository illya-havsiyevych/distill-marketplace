---
name: distill-verify
description: Stage 3 of distill-research. Verifies STEM--draft.md line by line against STEM--findings.md and writes the final report and review queue. Invoke only by name or from distill-research. Never auto-invoke.
compatibility: Forked execution requires Claude Code (context/agent/background fields). Elsewhere the skill runs inline; distill-research falls back to explicit subagent dispatch.
context: fork
agent: general-purpose
background: false
disable-model-invocation: true
argument-hint: <research/YYYY-MM-DD-slug>
---

You are verifying a research report you did not write. You have no memory of how it was produced and must not guess at it. `STEM` = `$0`.

Read `references/findings-format.md` and `references/report-template.md` in this skill's folder.
Open only `STEM--draft.md` and `STEM--findings.md`.
Do not open any file ending `--reasoning.md`. If findings cannot support a line, that is the finding — do not look further.

For each line of the draft:
1. Open its `[key]` section in findings. The `span` must support the line **as written**, not approximately. A gist-only source counts as single-source even if cited twice.
2. The line must serve the file's title. Off-topic → cut, and append a `note` to its key section.
3. Verdict: keep, tighten, or cut. Never add a claim.

Write `STEM.md` per the report template. Delete `STEM--draft.md`.
Write `research/review-queue.md`: at most nine items, ordered by what hurts most if wrong — vendor-reported figures, disputed values, single-source claims on material points, anything you tightened.

Self-check: if at any point you recall writing a line of this draft, stop and return `ISOLATION FAILED`. Do not continue.

Return exactly one line: `verify: <n> kept, <n> tightened, <n> cut, queue <n>`.
