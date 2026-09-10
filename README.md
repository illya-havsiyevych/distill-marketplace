# distill

Turns a deep-research report into three files: results a human reviews and later agents trust; findings grouped under each result's key; the original, untouched.

The results file is the review surface. A result is a paragraph under a heading like `## [gate-reduces-not-prevents] The gate reduces blast radius; it does not prevent`. You accept, edit, or delete it and merge. Sources, spans, and the derivation live in findings under the same key, one hop away, opened only when someone asks how a result was reached.

Four passes: extract results (starting from the report's own TL;DR / Key Findings), merge and key them, group the evidence under each key, then a fresh-context check that keeps only what is verified and on-topic.

## Install

Claude Code: `/plugin marketplace add illya-havsiyevych/distill-marketplace` then `/plugin install distill@distill-marketplace`.

Cowork: Customize → Personal plugins → Add marketplace → this repo → install. Cowork sometimes registers a plugin skill's name without mounting its file; if the first line of output is not `STEM = research/…`, connect this folder to the session and point the agent at `plugins/distill/skills/distill-research/SKILL.md` directly.

## Use

Say *distill `<report.md>`* in natural language. The verify pass is forked in Claude Code; elsewhere it runs inline, and a fresh-look verify is `distill-verify research/<stem>` in a new chat.

Copy the four lines under "Everyone else" in `references/format.md` into your CLAUDE.md so other skills treat results as settled.
