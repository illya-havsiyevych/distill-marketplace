---
name: distill-improve
description: Reads memory and note objects across all research/*--findings.md and proposes edits to the distill-* skills. The only skill permitted to read findings outside the distill flow. Invoke only by name. Never auto-invoke.
compatibility: Forked execution requires Claude Code (context/agent/background fields). Elsewhere the skill runs inline; distill-research falls back to explicit subagent dispatch.
context: fork
agent: general-purpose
background: false
disable-model-invocation: true
---

Grep `^memory:` and `^note:` across `research/*--findings.md`. Open nothing else.

Cluster recurring items. For each cluster with two or more occurrences, propose one rule change: target skill, target section, the proposed line, and the finding keys that motivated it. One proposal per cluster; no proposal without at least two motivating keys.

Write `research/skill-proposals/YYYY-MM-DD.md`. Do not edit any SKILL.md.

Return exactly one line: `improve: <n> proposals from <n> findings files`.
