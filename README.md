# distill

Turns a research source — a report, a paper, notes, a transcript — into three files: results a human reviews and later agents trust; findings grouped under each result's key; the original, untouched.

The results file is the review surface: `## <topic>` headers, one bullet per result, a key like `[what-it-says]` closing each bullet. You accept, edit, or delete bullets and merge. Sources, spans, and the derivation live in findings under the same key, one hop away, opened only when someone asks how a result was reached.

Five passes: extract results, merge and key them, group the evidence under each key, a fresh-context check that keeps only what is verified and on-topic, then a fresh-context summary — one TL;DR, one table, or one diagram — built from the verified results and nothing else.

## Install

Claude Code: `/plugin marketplace add illya-havsiyevych/distill-marketplace` then `/plugin install distill@distill-marketplace`.

Cowork: Customize → Personal plugins → Add marketplace → this repo → install. Cowork sometimes registers a plugin skill's name without mounting its file; if the first line of output is not `STEM = research/…`, connect this folder to the session and point the agent at `plugins/distill/skills/distill-research/SKILL.md` directly.

## Use

Say *distill `<source.md>`* in natural language. Verify and summarize are forked in Claude Code; elsewhere they run inline, and the fresh-context versions are `distill-verify research/<stem>` then `distill-summarize research/<stem> [tldr|table|diagram]` in new chats.

Copy the four lines under "Everyone else" in `references/format.md` into your CLAUDE.md so other skills treat results as settled.
