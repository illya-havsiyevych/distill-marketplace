# Format

Three files per research topic. `STEM` = `research/YYYY-MM-DD-<slug>`.

| File | Content | Read by |
|---|---|---|
| `STEM.md` | results — the human review surface | humans; every later agent, first |
| `STEM--findings.md` | evidence grouped under result keys | agents, only when a result is questioned |
| `STEM--reasoning.md` | the original, byte for byte | nobody by default |

## Results file

```markdown
# <Title as a scope>
<!-- YYYY-MM-DD -->

<Scope paragraph. What this covers, in the source's own summary if it has one. No key.>

## [convergent-architecture] A named, converging architecture
<One paragraph, 1–6 sentences, the argument intact. This is the result.
A human accepts or rejects it whole.>

## [scoped-token-inside-sandbox] Commercial platforms mostly keep a scoped token inside the sandbox
<paragraph>
```

- One `##` block per result. The heading carries the mnemonic key and a short title. The body is one paragraph.
- The key names the result — `[what-it-says]`, lowercase, hyphens, 2–4 tokens. Not the source. `[gate-reduces-not-prevents]`, `[pr-prep-is-offline]`, `[no-text-c2pa]`.
- Nothing else in this file. No sources, dates, confidence, verdicts, questions, or sentences about the source or the audit. The file is the knowledge, not a review of it. Test: delete the source; this file must still read as a standalone statement of what is known.
- Negative results are results: `## [no-text-c2pa] No C2PA profile for prose exists` with a paragraph saying what was looked for and where.

## Findings file

```markdown
# <title> — findings
<!-- from: STEM--reasoning.md -->

## [convergent-architecture]
from: --reasoning.md §Key Findings 1; §Details "Agent engineering patterns"
merged: "the four-part Cloudflare shape"; "harness vs compute split"
sources:
- Kahn, Building background agents on Cloudflare, 2026-05 — <url>
  "controlled connectivity via an egress proxy that injects credentials at the boundary"
- OpenAI, Sandbox Agents guide — <url>
- Willison, The Dual LLM pattern, 2023 — <url>
verify: kept

## [pr-prep-is-offline]
from: --reasoning.md §Key Findings 4
sources:
- git-scm docs, git-bundle — <url>
- executed 2026-09-09: `git bundle create x.bundle main` succeeded with no remote configured
verify: kept

## [industry-has-converged]
from: --reasoning.md §TL;DR bullet 1
sources:
- Kahn 2026-05 (one engineer's blog post)
verify: dropped — single secondary source; the same file's own finding is "almost no shipping platform implements the pure form"
```

- One `## [key]` section per result, same keys as the results file, same order. Dropped results keep their section so the drop is auditable.
- `from:` points into `--reasoning.md` by section, so the derivation is one hop away.
- `merged:` lists the phrasings folded into this result, so nothing silently vanished.
- `sources:` one line each: who, what, when, url; a verbatim span on the next line when one exists. A source can be `executed <date>: <command> → <observed>`, `searched <date>: <where> → not found`, or `human-review <date>` for something the human asserted.
- `verify:` `kept`, `tightened — <how>`, or `dropped — <why>`. Written only by the verify stage.

## Reasoning file

The original. One line prepended: `<!-- source: <tool> · <date> -->`. Never edited.

## Everyone else — copy to CLAUDE.md

- Load `research/*.md`; skip `*--findings.md` and `*--reasoning.md`.
- A result in `STEM.md` is settled. Do not re-research it. To see why, open `STEM--findings.md` at its key.
- Never edit `--reasoning.md`. Never write `verify:` lines; only the verify stage does.
- New evidence against a result → tell the human. Do not edit the result.
