# Report template

`research/YYYY-MM-DD-<slug>.md`. The only research file agents load by default. Humans review it block by block, as a PR diff.

```markdown
# <Title as a scope>
<!-- YYYY-MM-DD -->

<One scope paragraph. What this covers. No keys.>

## <Logical block>
<One paragraph, 1–6 sentences, in the source's own logical order, connectives
intact. Dense, confident, self-contained: a reader accepts or rejects it whole.>
[key] [key] [key]

## <Logical block>
...

## Searched, absent
<One paragraph naming what does not exist and where it was looked for.>
[absent]
```

## The report is the knowledge, not a review of it

Every sentence is about the subject. No sentence is about the document, the research, the audit, the sources' grades, or the reader. Prohibited anywhere in the report: verdicts, grades, percentages of claims audited, volatility, "worth checking", "easy to miss", "the report says", "before relying on this", questions, recommendations to the reader, corpus or coverage notes. That material is real, and it goes to `review-queue.md` (weak sentences), findings `note:` (caveats), or nowhere.

Test: delete the source. The report must still read as a standalone statement of what is known, with no trace that a distillation happened.

The source's own summary — a TL;DR, an abstract, an executive summary — becomes the scope paragraph. The source's own logical sections — key findings, numbered findings, headed sections — become the blocks, in the source's order. The source's recommendations and caveats are not knowledge; they are dropped, and any the human adopts return as `[human-review]` lines.

## The rule

**Block = review unit. Sentence = verification unit.**

- A block is a `##` heading, one paragraph, and a trailing key line. Nothing else.
- The paragraph keeps the argument's shape — "so", "therefore", "but" — because that shape is what a human is accepting. Splitting it into bullets loses the inference and hides misleading order behind individually-true facts.
- Every sentence is verified on its own against findings. The sentence→key binding is recorded in findings, not here. The key line shows the union.
- An unsupported sentence is removed from the paragraph and logged in findings. If that empties the block, the block is cut.
- Blocks follow the source's sections when the source has them; otherwise one block per coherent topic. A block should make sense cut out and read alone.
- Keys only on the key line — no URLs, dates, confidence words, or reviewer names in the paragraph. Keys are `[who-what]`, lowercase; `[executed]` marks a sentence verified by running something in-session, `[absent]` a negative finding, `[human-review]` a human assertion.
- No length cap on the report. The checklist is `review-queue.md`, capped at nine; each item names a block and the sentence in question.

## A field earns a place only if

it varies per block **and** is needed at read time. Everything else is constant, derivable from git, or consumed once in review.

| Dropped | Lives in |
|---|---|
| status / verified | merge to main |
| reviewer, date | `git blame` |
| confidence | which file the block is in |
| falsifier | PR description |
| recheck | human's call, not a field |
| url, span, sentence→key | findings under the key |
