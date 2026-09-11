---
name: distill-research
description: Distill a research source — a report, paper, notes, transcript — into results a human reviews and later agents trust, with evidence and the original kept one hop away. Use when the user has research and wants it turned into reviewable, reusable knowledge. Runs passes 1–3; hands pass 4 to distill-verify in a fresh context.
argument-hint: <path-to-source.md>
---

# distill-research

Read `references/format.md` first. It defines the three files.

`STEM` = `research/YYYY-MM-DD-<slug>` — today's date; slug short, lowercase, hyphens, from the title. State `STEM` before anything else.

## Pass 0 — keep the original
`cp "$0" STEM--reasoning.md`. A shell copy, not a read and a write. Nothing prepended.

## Pass 1 — extract results
Walk the whole source. A result is a claim about the subject, with the argument that carries it. If the source has a summary of its own, it is a good first pass, not the boundary. Not a result: advice to the reader, and anything about the source itself rather than its subject.

## Pass 2 — merge, key, group
Merge duplicates and rephrasings into one result each; keep the strongest phrasing, remember the others. Give each result a mnemonic key naming what it says: `[what-it-says]`, lowercase, hyphens, a few words. Keys name results, never sources. Group results under topic headers, following the source's structure when it has one.

## Pass 3 — group findings
For each key, record where in the original it came from (`from:`, in the original's own reference style), which sections were merged into it, and every external source the original cites for it with the verbatim span. Point at the original; do not quote it. Write `STEM--findings.md` per the format, no `verify:` lines yet. Write `STEM.md` per the format.

Do not evaluate, grade, or comment on the source anywhere in `STEM.md`.

## Pass 4 — hand off
Invoke `distill-verify` with `STEM`. It runs in a fresh context so it does not remember writing the draft. If this harness cannot start a fresh context, say so and tell the user to run `distill-verify STEM` in a new chat.

When it returns, report: the three paths, the results kept, and the titles of any dropped — one line each.

## After the human reviews
The human edits `STEM.md` and merges; that is the sign-off. A result the human adds or changes gets a findings section with `sources: - human-review <date>`.
