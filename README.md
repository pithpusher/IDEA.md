# IDEA.md — A Standard for Portable Idea Intent in Agentic Software

**IDEA.md** is a vendor-neutral file standard for expressing *what to build and why* before any code is written. It is the upstream intent document for AI coding agents — designed to sit above `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, and Copilot instruction files in the project hierarchy. Where those files tell agents how to behave in a repo, `IDEA.md` tells them what the idea actually is.

Inspired by Andrej Karpathy's [idea file concept](https://x.com/karpathy/status/2040470801506541998): in an era where AI agents can build anything from a prompt, the scarce resource is knowing what to build. Share the idea, not the code. The other person's agent customizes and builds it for their specific needs.

---

## Why IDEA.md Exists

AI coding agents like Claude Code, OpenAI Codex, Gemini CLI, and GitHub Copilot already have instruction files for agent behavior. But there is no broadly adopted, vendor-neutral file for the idea itself — the product thesis, problem framing, solution shape, and boundaries.

`IDEA.md` fills that gap. It is the source-of-truth file for product intent in any agentic software project.

## How It Works

Every project gets one root-level `IDEA.md`. It has five required sections:

| Section | Purpose |
|---------|---------|
| **Thesis** | One or two sentences capturing the core idea |
| **Problem** | What's broken today and for whom |
| **How It Works** | The pattern and key insight — not the implementation |
| **What It Does Not Do** | Boundaries that prevent scope creep and overbuilding |
| **Where To Start** | A nudge for agents to begin collaborating with the user |

Optional sections (add when they earn their place): Why Now, Who Benefits, Constraints, Risks, Open Questions, Success Criteria, Context and References.

The file is intentionally abstract. It describes ideas, not implementations. The agent and the user figure out the specifics together.

## Quick Start

1. Copy [`IDEA.md`](IDEA.md) into your project root as `IDEA.md`
2. Fill in the five required sections — or paste the template into your agent and write it together
3. Reference it from your `AGENTS.md` as the source of truth for intent

Or give your agent this entire repo and ask it to help you write an `IDEA.md` for your project.

## What's in This Repo

| File | Description |
|------|-------------|
| [`IDEA.standard.md`](IDEA.standard.md) | The full standard — design philosophy, structure, writing rules, agent consumption guidance |
| [`IDEA.md`](IDEA.md) | A blank five-section template ready to copy into any project |
| [`INTEGRATION.md`](INTEGRATION.md) | How other project files (AGENTS.md, CLAUDE.md, PRD.md, etc.) should reference and use IDEA.md |
| [`examples/LLM-Wiki.IDEA.md`](examples/LLM-Wiki.IDEA.md) | A worked example based on Karpathy's LLM Wiki idea file |

## How IDEA.md Fits With Other Agent Files

`IDEA.md` is upstream. Everything else is downstream. See [`INTEGRATION.md`](INTEGRATION.md) for copy-pasteable reference blocks showing exactly how each file should use `IDEA.md`.

| File | Role |
|------|------|
| `IDEA.md` | What to build and why — product intent and boundaries |
| `AGENTS.md` | How agents should behave in the repo |
| `CLAUDE.md` / `GEMINI.md` / Copilot instructions | Tool-specific agent configuration |
| `PRD.md` | Expanded product requirements (optional) |
| `ARCHITECTURE.md` | System design and technical shape |
| `TASKS.md` | Execution plan and work queue |

Downstream files must preserve the thesis, boundaries, and constraints from `IDEA.md`. If they conflict, `IDEA.md` wins.

## Design Philosophy

An `IDEA.md` should read like someone explaining an idea to a smart colleague — not like a form that was filled in. The goal is communication, not compliance.

Core principles:

- **Vendor-neutral** — readable by any agent or human, no tool-specific assumptions
- **Upstream of implementation** — exists before architecture, tasks, or code
- **Intentionally abstract** — describes the pattern, not the specific build
- **Stable under iteration** — core intent changes slowly, details evolve
- **Honest about uncertainty** — names what is unknown, assumed, or risky
- **Short enough to load repeatedly** — fits in a single context window alongside other working files

## Frontmatter

Minimal by default. Five fields:

```yaml
---
title: <idea title>
author: <name or handle>
status: draft | exploring | building | shipped | archived
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

Additional fields when useful: `owner`, `source`, `tags`.

## Example

From the [LLM Wiki example](examples/LLM-Wiki.IDEA.md):

```md
---
title: LLM Wiki
author: Andrej Karpathy
status: draft
created: 2026-04-05
updated: 2026-04-05
source: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
---

# LLM Wiki

## Thesis
Use an LLM to incrementally build and maintain a persistent wiki of markdown
files so that knowledge compounds over time instead of being re-derived from
scratch on every question.

## Problem
Most LLM-and-documents workflows are retrieval-first. You upload files, the
model searches them at query time, and generates an answer. Nothing accumulates.

## How It Works
Three layers: immutable raw sources, an LLM-maintained wiki of markdown pages,
and a schema file defining conventions and workflows...

## What It Does Not Do
Does not replace raw sources. Does not require heavyweight RAG infrastructure.
Does not prescribe a single directory layout or toolchain...

## Where To Start
Discuss the user's domain. Propose a directory structure and schema file.
Implement the ingest workflow first...
```

## Contributing

This is v0.2 of the standard. Open an issue or PR if you have ideas for improving it.

The goal is to keep `IDEA.md` simple and portable. Resist the urge to add fields and sections — the standard's value comes from what it leaves out, not what it includes.

## See Also

- [Karpathy's LLM Wiki idea file](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — the original idea file that inspired this standard
- [Karpathy's tweet thread on idea files](https://x.com/karpathy/status/2040470801506541998) — context on why idea files matter more than code
- [OpenAI Codex AGENTS.md](https://openai.com) — agent behavior file for Codex
- [Claude Code CLAUDE.md](https://docs.anthropic.com) — project memory and instructions for Claude Code
- [Gemini CLI GEMINI.md](https://github.com/google-gemini/gemini-cli) — context files for Gemini CLI
- [GitHub Copilot custom instructions](https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot) — Copilot instruction files
