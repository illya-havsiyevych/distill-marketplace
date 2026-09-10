---
name: distill-extract
description: Stage 2 of distill-research. Splits STEM--reasoning.md into typed findings and a draft report of logical blocks. Invoke only by name or from distill-research. Never auto-invoke.
compatibility: Forked execution requires Claude Code (context/agent/background fields). Elsewhere the skill runs inline; distill-research reports that isolation was not achieved.
context: fork
agent: general-purpose
background: false
disable-model-invocation: true
argument-hint: <research/YYYY-MM-DD-slug>
---

You are extracting from a research file you have never seen. `STEM` = `$0`.

Read `references/findings-format.md` and `references/report-template.md` in this skill's folder.
Open only `STEM--reasoning.md`. Open nothing else.

**Pass 1 — split.** Walk the file once. Emit typed objects only:
factual statement → `claim` (record the section it came from as `block`) · cited document → `source` · "not found / could not verify / does not exist" → `negative` · reusable process knowledge → `memory` · dead end or doubt → `note`. Discard prose that is none of these.

**Pass 2 — merge and key.** Merge duplicate claims. Merge sources that resolve to the same document. Give each source a mnemonic key `[<who>-<what>]`, at most three tokens, lowercase, self-describing, never numeric. Reserved: `[human-review]`, `[absent]`.

**Pass 3 — group.** Write `STEM--findings.md`: one `## [key]` section per key, `source` object first, then its claims, memories, notes; negatives under `## [absent]`.
Write `STEM--draft.md` as blocks: title, one unkeyed scope paragraph, then one `##` block per source section (or per coherent topic if the source is unstructured). Each block is one paragraph of 1–6 sentences in the source's logical order with its connectives intact, followed by a key line holding the union of that paragraph's sentence keys. Negatives become a final `## Searched, absent` block keyed `[absent]`.

Do not verify. Do not start the next stage.

Return exactly one line: `extract: <n> blocks, <n> sentences, <n> sources, <n> negatives, <n> notes`.
