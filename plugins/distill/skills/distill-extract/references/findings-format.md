# Findings format

`research/YYYY-MM-DD-<slug>--findings.md`. Written by distill-extract, read by distill-verify and distill-improve. Never loaded as default context.

## Layout

```markdown
# <title> — findings
<!-- from: YYYY-MM-DD-<slug>--reasoning.md -->

## [key]
<source object>
<claim objects>
<memory objects>
<note objects>

## [absent]
<negative objects>

## [human-review]
<review object>          ← humans only
```

One `## [key]` section per distinct source. `source` first, always. Sections are append-only once merged; a re-verification adds objects, never rewrites them.

## Keys

`[<who>-<what>]` — lowercase, hyphens, at most three tokens, self-describing enough that a reader can judge sourcing without opening the section.

| Good | Why |
|---|---|
| `[exa-repo]` | official repository, primary |
| `[irons-2603.11393]` | first author + arXiv id, resolvable |
| `[grade-2013]` | standard + year |
| `[medium-post]` | honest about weakness |

Never numeric. Two branches allocating `[S-15]` collide; two branches allocating `[exa-repo]` refer to the same thing, which is correct.

Reserved: `[absent]` for negative findings, `[human-review]` for human decisions.

## Objects

Each object is a fenced key/value block. Fields in the order shown.

### source

```
source:    <url>
title:     <document title>
type:      primary | secondary | vendor | forum | analogy
authority: official-repo | peer-reviewed | standard | press | blog | none
accessed:  YYYY-MM-DD
span:      "<verbatim text that backs the claims below>"
gist:      <paraphrase, only when no verbatim span was captured>
archive:   <snapshot url>                                   (optional)
```

`span` is the load-bearing field. Verification is a glance at the span, not a read of the document. A source with only `gist` is weaker; verify treats it as single-source even if cited twice.

### claim

```
claim:   <one atomic statement, as it appears in the report>
verdict: supported | partial | contradicted | unverifiable
```

`partial` means the span supports a weaker version — verify tightens the report line to match. `contradicted` and `unverifiable` never reach the report; they go to `review-queue.md`.

### negative

```
negative: <what does not exist / could not be found>
searched: <where and how — venues, queries, tools>
as_of:    YYYY-MM-DD
```

Negatives decay silently: nothing in the world contradicts a stale "not found." `as_of` is mandatory here and optional nowhere else.

### memory

```
memory: <reusable knowledge not tied to this title>
```

Feeds distill-improve, not the report.

### note

```
note: <dead end, dropped item, doubt, or why something was cut>
```

### review (humans only)

```
reviewed: YYYY-MM-DD
decisions:
  - <what changed and the one-line reason>
```

## What is not here

No per-claim confidence, reviewer, status, or recheck date. Confidence is where a claim sits — report, queue, or cut. Reviewer and date are in git. Recheck is a human decision, not a field the agent guesses.
