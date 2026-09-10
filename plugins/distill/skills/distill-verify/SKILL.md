---
name: distill-verify
description: Stage 3 of distill-research. Verifies a draft of logical blocks sentence by sentence against one or more findings files and writes the final report and review queue. Also verifies an existing document against findings. Invoke only by name or from distill-research. Never auto-invoke.
compatibility: Forked execution requires Claude Code (context/agent/background fields). Elsewhere the skill runs inline; distill-research reports that isolation was not achieved.
context: fork
agent: general-purpose
background: false
disable-model-invocation: true
argument-hint: <STEM> [<draft-path> <findings-path>...]
---

You are verifying a document you did not write. You have no memory of how it was produced and must not guess at it.

Arguments: `STEM` = `$0`. If `$1` is given, DRAFT = `$1` and FINDINGS = every remaining argument; the draft is the user's document and must not be deleted. Otherwise DRAFT = `STEM--draft.md`, FINDINGS = `STEM--findings.md`, and the draft is deleted at the end.

Read `references/findings-format.md` and `references/report-template.md` in this skill's folder.
Open only DRAFT and the FINDINGS files. Open nothing else.
Do not open any file ending `--reasoning.md`. If findings cannot support a sentence, that is the finding — do not look further.

If more than one findings file is given: a key that appears in several files with the same `source:` is one source. A key that appears with different `source:` values is a collision — log a `note` and treat the sentence as single-source.

**Block = review unit. Sentence = verification unit.** For each `##` block of the draft, for each sentence in its paragraph:
1. Find the `claim` object whose text matches this sentence, then its `[key]` section. If the draft has no key lines — it is an external document — match the sentence to findings by content and record the key you matched. The `span` must support the sentence **as written**, not approximately. A gist-only source counts as single-source even if cited twice.
2. The sentence must serve the block, and the block must serve the file's title. Off-topic → drop the sentence, append a `note` to its key section.
3. Sentence verdict: keep, tighten, or drop. Never add a sentence. A sentence with no supporting key in any findings file is dropped and logged as `note: unsupported — <sentence>`.

Block verdict: keep if every sentence kept; tighten if any sentence was tightened or dropped (rewrite the paragraph so it still reads whole, connectives intact); cut if no sentence survives. Rebuild the block's key line as the union of surviving sentence keys.

Write `STEM.md` per the report template — the file name is exactly `STEM.md`, no suffix. The report contains only subject-matter sentences; nothing about the audit, grades, volatility, or what the reader should check. That material goes to the queue or findings. Delete `STEM--draft.md` only if it was the default draft.
Write `research/review-queue.md`: at most nine items, each naming a block and the sentence in question, ordered by what hurts most if wrong — vendor-reported figures, disputed values, single-source sentences on material points, anything you tightened.

Self-check: if at any point you recall writing a sentence of this draft, stop and return `ISOLATION FAILED`. Do not continue.

Return exactly one line: `verify: <n> blocks kept, <n> tightened, <n> cut; <n> sentences dropped; queue <n>`.
