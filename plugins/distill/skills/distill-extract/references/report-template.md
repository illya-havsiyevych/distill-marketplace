# Report template

`research/YYYY-MM-DD-<slug>.md`. The only research file agents load by default. Humans review it as a PR diff.

```markdown
# <Title as a question or a scope>
<!-- YYYY-MM-DD -->

## <Topic group>
- <one atomic claim> [key]
- <one atomic claim> [key] [key]

## Searched, absent
- <what does not exist> [absent]
```

## Rules

- One claim per line. If a line has "and," it is probably two claims.
- Every line ends with at least one key. `[absent]` counts.
- Keys only — no URLs, dates, confidence words, or reviewer names. Those live in findings, git, or nowhere.
- Group by topic, not by source and not by confidence. Agents read this by subject.
- Negative findings always get their own final section. They are the most expensive results to re-derive and the easiest to lose.
- Title is the scope. Verify cuts any line that does not serve it.
- No length cap. This is a store, not a checklist. The checklist is `review-queue.md`, capped at nine.

## A field earns a line only if

it varies per claim **and** is needed at read time. Everything else is constant, derivable from git, or consumed once in review.

| Dropped | Lives in |
|---|---|
| status / verified | merge to main |
| reviewer, date | `git blame` |
| confidence | which file the claim is in |
| falsifier | PR description |
| recheck | human's call, not a field |
| url, span | findings under the key |
