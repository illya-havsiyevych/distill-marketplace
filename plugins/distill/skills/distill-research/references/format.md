# Format

Three files per topic. `STEM` = `research/YYYY-MM-DD-<slug>`.

| File | Content | Read by |
|---|---|---|
| `STEM.md` | results — the human review surface | humans; every later agent, first |
| `STEM--findings.md` | evidence grouped under result keys | agents, only when a result is questioned |
| `STEM--reasoning.md` | the original, byte for byte | nobody by default |

## Results file

```markdown
# <Title — what this file covers>

<One paragraph: scope.>

## <Topic>
- <Result. The first sentence states it; the rest support it.> [what-it-says]
- <Result.> [another-result]

## <Topic>
- <Result.> [third-result]
```

- `## <topic>` headers group results by subject. Follow the source's structure when it has one; otherwise group by what the results are about.
- One bullet per result. As long as its argument needs and no longer, connectives intact. A human accepts or rejects the bullet whole.
- The mnemonic key closes the bullet: `[what-it-says]`, lowercase, hyphens, a few words. It names the result, never the source.
- Nothing else. No dates, sources, confidence, verdicts, questions, or sentences about the source or the process. The file is the knowledge, not a review of it. Test: delete the source; this file must still read as a standalone statement of what is known.
- A negative result is a bullet like any other: what does not exist and where it was looked for.

## Findings file

```markdown
# <title> — findings
<!-- from: STEM--reasoning.md -->

## [what-it-says]
from: --reasoning.md §<section>
merged: "<phrasing folded into this result>"; "<another>"
sources:
- <who>, <what>, <when> — <url>
  "<verbatim span, when one exists>"
- the original, §<section>
- executed <date>: <command> → <observed>
- searched <date>: <where> → not found
- human-review <date>
verify: kept | tightened — <how> | dropped — <why>
```

- One `## [key]` section per result, same keys as the results file, same order. Dropped results keep their section so the drop is auditable.
- `from:` points into `--reasoning.md` by section, so the derivation is one hop away.
- `merged:` lists the phrasings folded into this result, so nothing silently vanished. Omit when nothing was merged.
- `sources:` one per line. When the original cites nothing for a result, the original itself is the source: `- the original, §<section>`. That is honest and lets verify treat it as single-source.
- `verify:` written only by the verify stage.

## Reasoning file

The original. One line prepended: `<!-- source: <origin> · <date> -->`. Never edited.

## Everyone else — copy to CLAUDE.md

- Load `research/*.md`; skip `*--findings.md` and `*--reasoning.md`.
- A result in `STEM.md` is settled. Do not re-research it. To see why, open `STEM--findings.md` at its key.
- Never edit `--reasoning.md`. Never write `verify:` lines; only the verify stage does.
- New evidence against a result → tell the human. Do not edit the result.
