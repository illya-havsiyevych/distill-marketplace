---
name: distill-research
description: Distill a deep-research report into results a human reviews and later agents trust, with evidence and the original kept one hop away. Use when the user has a research report and wants it turned into reviewable, reusable knowledge. Runs passes 1–3; hands pass 4 to distill-verify in a fresh context.
argument-hint: <path-to-report.md>
---

# distill-research

Read `references/format.md` first. It defines the three files.

`STEM` = `research/YYYY-MM-DD-<slug>` — today's date, slug lowercase, hyphens, at most six words from the title. Print `STEM` as your first line.

## Pass 0 — keep the original
Copy `$0` byte for byte to `STEM--reasoning.md`, one line prepended: `<!-- source: <tool> · <date> -->`.

## Pass 1 — extract results
Pull results out of the report. Start from its summary and findings sections — TL;DR, Key Findings, Executive Summary, numbered findings — because those are already results; then walk the rest for results the summary skipped. A result is a claim about the subject, 1–6 sentences, argument intact. Not a result: recommendations, caveats, methodology, self-assessment, anything about the report rather than the subject.

## Pass 2 — merge and key
Merge duplicates and rephrasings into one result each; keep the strongest phrasing, remember the others. Give each result a mnemonic key naming what it says: `[what-it-says]`, lowercase, hyphens, 2–4 tokens. Keys name results, never sources.

## Pass 3 — group findings
For each key, gather from `--reasoning.md` everything that supports it: the section it came from, the sources with a verbatim span where one exists, the phrasings you merged. Write `STEM--findings.md` per the format, one `## [key]` section per result, no `verify:` lines yet. Write `STEM.md` per the format: title, scope paragraph, one `## [key] Title` block per result.

Do not evaluate, grade, or comment on the report anywhere in `STEM.md`.

## Pass 4 — hand off
Invoke `distill-verify` with `STEM`. It runs in a fresh context so it does not remember writing the draft. If this harness cannot start a fresh context, say so and tell the user to run `distill-verify STEM` in a new chat.

When it returns, report: the three paths, the results kept, and the titles of any dropped — one line each.

## After the human reviews
The human edits `STEM.md` and merges; that is the sign-off. A result the human adds or changes gets a findings section with `sources: - human-review <date>`.
