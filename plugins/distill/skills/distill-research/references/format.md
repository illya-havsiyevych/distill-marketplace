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

## [what-it-says]
from: <section>

## [another-result]
from: <section>
merged: <section>, <section>
sources:
- <who>, <what>, <when> — "<verbatim span>"
- executed <date>: <command> → <observed>
- searched <date>: <where> → not found
verify: kept | tightened — <how> | dropped — <why>
```

- One `## [key]` section per result, same keys as the results file, same order. Dropped results keep their section so the drop is auditable.
- `from:` is a short pointer into the original, in whatever form the original uses — a section number, a heading, a page, a timestamp. Nothing is copied from the original; it is one hop away.
- `merged:` lists the sections whose phrasings were folded into this result, so nothing silently vanished. Omit when nothing was merged.
- `sources:` lists external sources only, one per line, with the verbatim span that supports the result. Mandatory when the original cites one; the original is the only place it would otherwise live. Omit the line when the original cites nothing — `from:` already says the original asserts it.
- `verify:` written only by the verify stage.
## Reasoning file

The original, copied with `cp`. Nothing prepended. Never edited.

## Everyone else — copy to CLAUDE.md

- Load `research/*.md`; skip `*--findings.md` and `*--reasoning.md`.
- A result in `STEM.md` is settled. Do not re-research it. To see why, open `STEM--findings.md` at its key.
- Never edit `--reasoning.md`. Never write `verify:` lines; only the verify stage does.
- New evidence against a result → tell the human. Do not edit the result.
