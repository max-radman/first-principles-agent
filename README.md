# First-Principles Agent

A skill for your coding agent. It runs **before** any code gets written and stops the agent from hand-building what your libraries already ship out of the box.

Coding agents build custom code by default, because building is what they do. So they constantly re-implement things the libraries you already use ship for free. 5x the work, 0.5x the quality.

## How it works

1. **Map the layers.** It lists every layer the feature or bug actually touches (UI, client state, data fetching, auth, database, jobs, LLM calls, storage...). That's the scope.
2. **Go layer by layer, one question each: are we already using an external provider for this?**
   - **Yes** → read the provider's docs and check we're using it their way, not hand-building around it. Doc link required.
   - **No** → is there a library that does this, so we don't build it from scratch? If there genuinely isn't one, it says so, and that's when custom is justified.
3. **Only what's left gets built custom.** Every claim is backed by the actual docs and the actual code, cited by `file:line`.

## Use it

1. Download [`first-principles-agent.md`](./first-principles-agent.md).
2. Hand it to your coding agent (Claude Code, Cursor, Codex, or any chat) with this prompt:

```
I'm adding a skill to my coding workflow. Attached is a Markdown file, first-principles-agent.md.

1. Read it in full.
2. Give me a short, plain-language overview of what it does and when it should fire.
3. Look at my agent instructions file (CLAUDE.md, AGENTS.md, .cursorrules, or the equivalent).
   Propose the exact edit that wires this skill into my workflow: where it slots in, what
   triggers it, and how its output is used. Show me the change as a diff and wait for my
   approval before writing anything.
```

The agent reads the skill, explains it back to you, and proposes how to wire it into your workflow before touching anything.

## Credit

Built by [Max Radman](https://github.com/max-radman), from a conversation with Armin Daryiabegi (CTO, chatarmin). Core principle: use what comes out of the box, build custom only on top, and only when it's really necessary.
