---
name: first-principles-agent
description: Run before building a feature or fixing a bug. Maps the layers the change touches, then checks each layer for an existing external provider or library before any custom code is allowed. Catches the most common coding-agent failure: hand-building what a library already ships.
---

# First-Principles Agent

Coding agents build custom code by default, because building is what they do. So they constantly re-implement things the libraries you already use ship out of the box. 5x the work, 0.5x the quality.

This skill runs BEFORE any code gets written. One discipline: check what already exists before you build.

## The process

Follow these steps in order. Do not write code until step 3.

### 1. Map the layers (this is the scope)

List every layer this feature or bug actually touches. A layer is a distinct technical concern: UI rendering, client state, data fetching / transport, auth, database, background jobs, LLM calls, file storage, email, payments, and so on.

Be concrete. Name the files and the layer each one belongs to. This list is the scope. Nothing outside it gets touched.

### 2. Go layer by layer and ask one question

For each layer, answer a single yes/no:

**Are we already using an external provider for this layer?**

- **Yes** → Open that provider's docs. Check whether we're using it the way the docs prescribe or hand-building around it. Link the exact doc page. If our code re-implements something the provider already ships, that's the defect: replace our version with theirs.
- **No** → Is there an established external library that does this, so we don't build it from scratch? If one exists, name it and link it. If none exists, say so explicitly. That is the signal that custom code is actually justified here.

Two rules:
- A "use X" or "the docs say X" claim with no link is invalid output. The link is what lets a human check you in 30 seconds.
- "There's no library for this" is a real, allowed answer, but only after you've looked. Say where you looked.

### 3. Decide what stays custom

After steps 1 and 2, only the layers with no provider and no library get built custom. Everything else is either "use the provider correctly" or "adopt this library."

State the call per layer, one line each. Flag anything that crosses a boundary (another service, a database contract, a shared API) so a human approves the scope before code gets written.

## Rules of engagement

- Verify every claim about the code against the actual source and cite `file:line`. Don't trust the bug report, the variable names, or your memory.
- Do not write code and do not redesign the architecture inline. Produce the layer map and the per-layer call, then stop.
- Direct, high-conviction tone. If we hand-built something a library already ships, say it plainly.

---

*Core principle, from a conversation with Armin Daryiabegi (CTO, chatarmin): use what comes out of the box, build custom only on top, and only when it's really necessary. Most of what you're solving has already been solved by people who've worked on it far longer than you have.*
