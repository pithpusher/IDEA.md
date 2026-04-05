# IDEA.md Standard

Version: 0.2
Status: Proposed
Purpose: A vendor-neutral file for communicating product or project intent to agents and humans.

---

## What IDEA.md Is

`IDEA.md` is an idea file. It communicates what should exist, why it matters, and what it should not become. It is written for agents to read, interpret, and act on in collaboration with the user.

It is not a requirements doc. It is not a task list. It is not the place for repo-specific rules, tool configuration, or implementation detail. It describes a *pattern* or *intent* clearly enough that any capable agent can propose a concrete version tailored to the user's context.

## Why It Exists

Agentic workflows already have instruction files for agent behavior: `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, Copilot instruction files. Those describe *how* an agent should work inside a repo.

What is missing is a standard file for the idea itself.

In an era where agents can build anything from a prompt, the scarce resource is knowing what to build and why. The code is downstream. The idea is upstream. `IDEA.md` is the upstream file.

This draws directly from the observation that sharing ideas is becoming more valuable than sharing implementations. You share the idea; the other person's agent customizes and builds it for their specific needs. `IDEA.md` is the portable format for that.

## Design Philosophy

An `IDEA.md` should read like someone explaining an idea to a smart colleague — not like a form that was filled in.

The goal is communication, not compliance. The document should convey enough intent that an agent can propose a concrete plan, but stay abstract enough that the agent adapts the plan to the user's actual situation. Overspecification kills the collaboration. Underspecification wastes the first hour on wrong assumptions.

Write prose when prose communicates better. Use bullets when bullets communicate better. Do not default to bullets because a template told you to.

### Core principles

1. **Vendor-neutral.** Readable by any agent or human. No tool-specific assumptions.
2. **Upstream of implementation.** Exists before architecture, tasks, or code.
3. **Intentionally abstract.** Describes the pattern, not the specific build. The agent figures out the specifics with the user.
4. **Stable under iteration.** Core intent changes slowly. Status, questions, and details evolve.
5. **Honest about uncertainty.** Names what is unknown, assumed, or risky.
6. **Short enough to load repeatedly.** The full file should fit comfortably in a single context window alongside other working files.

## Structure

`IDEA.md` has two layers: a required core and optional extended sections. The core is always present. The extended sections are added when they earn their place.

### Required Core

These five sections communicate the idea. They can be written in prose, bullets, or a mix — whatever fits the idea best.

#### 1. Title and Thesis
A clear name and one or two sentences capturing the core idea.

#### 2. Problem
What problem exists today and for whom. Why the current state of things is insufficient.

#### 3. How It Works
A plain-language description of the solution shape. Enough to understand the approach, not enough to constrain the implementation. This is where you describe the *pattern* — the key insight, the architecture at a conceptual level, how the pieces relate. Think explanation, not specification.

#### 4. What It Does Not Do
Explicit boundaries. What the idea is not trying to solve. What the agent should not build. This section prevents scope creep and overbuilding — it is often the most valuable section in the file.

#### 5. Where To Start
A nudge toward first steps. Not a task list — more like "if you're an agent reading this, here's how to begin the conversation with the user." Should point toward planning and discussion, not prescribe a build order.

### Optional Extended Sections

Add these when the idea needs them. Do not include empty sections to satisfy a template.

- **Why Now** — what makes this timely or newly possible.
- **Who Benefits** — primary and secondary users, if not obvious from the problem statement.
- **Constraints** — real boundaries: platform, budget, legal, performance, operational.
- **Risks** — what could go wrong. Failure modes worth naming early.
- **Open Questions** — what still needs to be answered before or during building.
- **Success Criteria** — how to know the idea is working. Observable outcomes, not vanity metrics.
- **Context and References** — links to related work, prior art, source material, or inspiration.

## Frontmatter

Optional but recommended. Keep it minimal.

```yaml
---
title: <idea title>
author: <name or handle>
status: draft | exploring | building | shipped | archived
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

`title` and `author` identify the idea and its origin. `status` tracks lifecycle. `created` and `updated` give temporal context. That's it — five fields.

Fields like `id`, `version`, and `format` are redundant with the filename, git history, and the file being called IDEA.md. `owner` is operational metadata that belongs in `AGENTS.md` or your project tool, not in the idea itself.

Additional fields when useful:

```yaml
owner: <person or team responsible for the idea now>
source: <url or reference>
tags: []
```

## Writing Rules

- Write to communicate, not to fill in fields.
- Prefer prose for explaining ideas. Use bullets for lists of discrete items.
- Distinguish facts from assumptions. Say "I assume" or "this is unproven" when appropriate.
- Avoid marketing language. Avoid jargon that obscures rather than clarifies.
- Do not embed implementation details, repo-specific rules, or tool configuration.
- Keep the file short. If it exceeds roughly 500 words of content (excluding frontmatter), consider whether some material belongs in `PRD.md` or `ARCHITECTURE.md` instead.
- The best test: could you hand this file to a different agent on a different stack and have it understand what to build? If yes, the abstraction level is right.

## Relationship to Other Files

`IDEA.md` is the upstream intent file. Other files are downstream.

- `IDEA.md` = what and why
- `AGENTS.md` = how agents should behave in the repo
- `CLAUDE.md` / `GEMINI.md` / Copilot files = tool-specific adapters
- `PRD.md` = expanded requirements when the idea needs more detail
- `ARCHITECTURE.md` = system design
- `TASKS.md` = execution plan

Rule: downstream files must preserve the thesis, boundaries, and constraints from `IDEA.md`. If they conflict, `IDEA.md` wins or `IDEA.md` gets updated — the intent file is the source of truth.

## How Agents Should Read IDEA.md

When an agent reads `IDEA.md`, it should:

1. Understand the core idea and restate it to confirm alignment.
2. Note what the idea explicitly does not do — these are hard boundaries.
3. Identify what is unknown or assumed — surface these to the user rather than silently deciding.
4. Propose a concrete version adapted to the user's context, tools, and preferences.
5. Avoid inventing requirements not implied by the document.
6. Treat the file as the start of a collaboration, not as a spec to execute.

The agent's job is to turn an abstract idea into a concrete plan *with* the user, not *for* the user.

## Lifecycle

Suggested statuses:

- `draft` — raw idea, not yet refined
- `exploring` — problem and approach are being shaped
- `building` — implementation is active
- `shipped` — a working version exists
- `archived` — no longer active

## Compatibility

To integrate with existing agent ecosystems:

- Keep one root-level `IDEA.md` per project or idea.
- Reference it from `AGENTS.md` as the source of truth for intent.
- Mirror only the operationally necessary parts into tool-specific files.
- When multiple ideas exist in a repo, use `ideas/` directory with individual files.

Recommended `AGENTS.md` reference block:

```md
## Source of Truth

Read `IDEA.md` first.

Preserve its thesis, boundaries, and constraints in all downstream work.
If `IDEA.md` is ambiguous, surface the ambiguity to the user.
Do not invent requirements that conflict with `IDEA.md`.
```

## Template

```md
---
title: <title>
author: <name or handle>
status: draft
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# <Title>

## Thesis
<one or two sentences — what is this and why does it matter>

## Problem
<what problem exists today, for whom, and why current approaches fall short>

## How It Works
<the shape of the solution — the pattern, the key insight, how the pieces
relate — written at a level of abstraction that communicates the idea without
constraining the implementation>

## What It Does Not Do
<explicit boundaries — what this idea is not, what the agent should not build,
where scope ends>

## Where To Start
<a nudge toward first steps — how an agent should begin working with the user
to turn this idea into something concrete>
```

## Positioning

`IDEA.md` is the standard file for portable idea intent in agentic software work.

It is deliberately abstract. It describes ideas, not implementations. The agent and the user figure out the specifics together. The file's only job is to communicate the pattern clearly enough that any agent can pick it up and run with it.
