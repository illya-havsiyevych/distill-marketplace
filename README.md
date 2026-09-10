# distill

Turns one raw deep-research output into three files: a terse report agents load by default, typed findings under mnemonic source keys, and the untouched reasoning original.

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

Feed it raw research with sources. An already-synthesized playbook with no citations produces an empty findings file — that is the skill refusing, correctly.
