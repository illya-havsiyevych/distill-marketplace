# distill

Turns one raw deep-research output into three files: a report of human-reviewable logical blocks that agents load by default, typed findings under mnemonic source keys, and the untouched reasoning original.

Block = review unit, sentence = verification unit. You accept or reject a paragraph; the skill checks every sentence in it against a source span and records the binding in findings.

## Install

**Claude Code**
```
/plugin marketplace add <your-github-user>/distill-marketplace
/plugin install distill@distill-marketplace
/reload-plugins
```

**Cowork (Claude Desktop):** Customize → Personal plugins → + → Add marketplace → paste the repo URL → Sync → install `distill`.

Invoke in natural language ("distill research/foo.md"), not with a slash — Cowork's slash path for plugin skills is unreliable.

## Isolation

`distill-extract` and `distill-verify` are forked (`context: fork`). That works in Claude Code. Cowork and claude.ai have no subagent mechanism, so there the stages run inline and verify can remember extract. For real isolation there, run extract in one chat and verify in a fresh one.

## Input

Feed it raw research with sources. The umbrella refuses input with no citations — an already-synthesized playbook would produce an empty findings file.

To check such a playbook instead, distill the reports it was built from, then:

```
distill-verify research/2026-09-10-playbook path/to/playbook.md research/<a>--findings.md research/<b>--findings.md
```

Every playbook line with no supporting key is cut and logged. That is the answer to whether it holds up.
