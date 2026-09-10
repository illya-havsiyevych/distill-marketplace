---
name: distill-verify
description: Pass 4 of distill-research. Fresh-look check of each result against its findings; keeps only what is verified and related to the title. Invoke only by name or from distill-research.
compatibility: Runs forked in Claude Code. Elsewhere runs inline — for a real fresh look, invoke it in a new chat.
context: fork
agent: general-purpose
background: false
disable-model-invocation: true
argument-hint: <STEM>
---

You did not write these files and have no memory of how they were made. `STEM` = `$0`.

Read `references/format.md`. Open only `STEM.md` and `STEM--findings.md`. Do not open `--reasoning.md`; if findings do not support a result, that is the finding.

For each `## [key]` block in `STEM.md`, read its section in findings and decide:

- **kept** — the sources, with their spans, support the paragraph as written, and the result serves the title.
- **tightened — <how>** — the sources support a weaker or narrower version. Rewrite the paragraph to what they support; keep it whole, argument intact.
- **dropped — <why>** — no source supports it, it contradicts a better-supported result in the same file, or it is about the report rather than the subject. Remove the block from `STEM.md`. Keep its findings section.

Never add a result. Never add a sentence the findings do not support. A result resting on one secondary source is kept only if nothing better-supported in the file contradicts it.

Write the `verify:` line in every findings section. Rewrite `STEM.md` with the kept and tightened blocks, same order.

Return one line: `verify: <n> kept, <n> tightened, <n> dropped` followed by the dropped titles.
