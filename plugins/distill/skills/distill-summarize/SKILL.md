---
name: distill-summarize
description: Pass 5 of distill-research. Adds a Summary section — one TL;DR, one table, or one mermaid diagram — to a verified results file, built from the results alone. Invoke only by name or from distill-research, after distill-verify.
compatibility: Runs forked in Claude Code. Elsewhere runs inline — for a guaranteed results-only view, invoke it in a new chat.
context: fork
agent: general-purpose
background: false
disable-model-invocation: true
argument-hint: <STEM> [tldr|table|diagram]
---

`STEM` = `$0`. Optional `$1` forces the form.

Read `references/format.md`. Open only `STEM.md`. Open nothing else — not findings, not reasoning, not the web. The summary must contain nothing that is not in the results file.

If `STEM.md` already has a `## Summary` section, replace it.

## Choose the form

Unless `$1` says otherwise:

- **table** — the results compare several things on the same attributes. Rows are the things; columns are the attributes the results actually use; a cell holds only what a result states and is blank otherwise. No derived columns, no totals.
- **diagram** — the results describe a flow, a structure, or a causal chain. One mermaid `flowchart`. Nodes are things the results name; edges are relationships the results state, labeled in the results' own words. Nothing the results do not say.
- **tldr** — otherwise. Prose, no bullets. The throughline a reader needs before deciding whether to read the rest. Every sentence restates or combines results; none introduces a claim, number, or entity absent from them.

## Write it

Insert `## Summary` after the scope paragraph, before the first topic. Exactly one form. No keys in it.

Then trace it: for every sentence, row, column, node, and edge, list the keys it comes from under `## summary` at the end of `STEM--findings.md`, per the format. If you cannot name a key for something, delete it from the summary. Do this before returning; it is the only thing that makes "from the results alone" true.

Return one line: `summarize: <form> — <n> sentences|rows|nodes, all traced`.
