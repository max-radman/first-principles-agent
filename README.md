# First-Principles Agent

A skill for your coding agent. It runs before any code gets written and stops the agent from hand-building what your libraries already ship out of the box.

Coding agents build custom code by default, because building is what they do. So they constantly re-implement things the libraries you already use ship for free. 5x the work, 0.5x the quality.

## Before / after

You ask for streaming AI chat. Your agent hand-rolls a parser for the server-sent events, a state machine for the message list, and a retry loop.

With the first-principles agent:

```
Layer: LLM transport → provider already installed: Vercel AI SDK
useChat() does all three. Docs: sdk.vercel.ai/docs/ai-sdk-ui/chatbot
Delete the custom parser.
```

Same story with auth, uploads, date pickers, form validation. The library shipped it. You just rebuilt it.

## How it works

Before writing code, it maps the layers your change touches (UI, data fetching, auth, database, LLM calls, storage, and so on). Then it walks each layer and asks one question:

```
Are we already using a provider for this layer?
  Yes → read its docs. Are we using it their way, or hand-building around it?
  No  → is there a library that does this? If not, custom is justified.
```

Only the layers with no provider and no library get built custom. Everything else is "use the provider correctly" or "adopt this library."

Every answer is backed by the actual docs and the actual code, cited by `file:line`. No link, no claim. "There's no library for this" is allowed, but only after it has looked, and it says where.

**The rule was never "use more libraries."** It is: don't hand-build what a provider you already pay for already ships. Custom code is what survives the check, not what you reach for first.

## Use it

Download [`first-principles-agent.md`](./first-principles-agent.md) and hand it to your coding agent (Claude Code, Cursor, Codex, or any chat) with this prompt:

```
I'm adding a skill to my coding workflow. Attached is a Markdown file, first-principles-agent.md.

1. Read it in full.
2. Give me a short, plain-language overview of what it does and when it should fire.
3. Look at my agent instructions file (CLAUDE.md, AGENTS.md, .cursorrules, or the equivalent).
   Propose the exact edit that wires this skill into my workflow: where it slots in, what
   triggers it, and how its output is used. Show me the change as a diff and wait for my
   approval before writing anything.
```

It reads the skill, explains it back, and proposes how to wire it into your workflow before touching a thing.

## Credit

Built by [Max Radman](https://github.com/max-radman). Inspired by a conversation with [Armin Daryabegi](https://github.com/saasjesus) (CTO, chatarmin): use what comes out of the box, build custom only on top, and only when it is really necessary. Most of what you're trying to solve has already been solved by people smarter than you who have worked on it for far longer.
