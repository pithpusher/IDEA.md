# Integrating IDEA.md With Other Project Files

This guide shows how each file in an agentic project should reference and use `IDEA.md`. The pattern is simple: `IDEA.md` is the source of truth for intent. Every other file reads from it, preserves its boundaries, and points back to it.

---

## AGENTS.md

`AGENTS.md` defines how agents behave in the repo. It should read `IDEA.md` first and treat it as the authority on what the project is trying to do.

```md
## Source of Truth

Read `IDEA.md` before starting any work.

Preserve:
- the thesis and core intent
- all items listed under "What It Does Not Do"
- any constraints mentioned in the idea

If `IDEA.md` is ambiguous, surface the ambiguity to the user.
Do not invent requirements that are not implied by `IDEA.md`.
Do not silently resolve open questions — ask.
```

Place this at or near the top of `AGENTS.md` so agents encounter it before any repo-specific rules.

---

## CLAUDE.md

`CLAUDE.md` is the Claude Code project memory file. It should mirror the relevant parts of `IDEA.md` and `AGENTS.md` rather than redefining intent from scratch.

```md
# Project Intent

See `IDEA.md` for the full idea. Summary:

<paste your thesis here — one or two sentences>

## Boundaries

<paste your "What It Does Not Do" section here>

## Working Rules

<Claude-specific behavior rules, coding conventions, etc.>
```

Keep the intent section short — just enough that Claude has context without needing to read the full file on every invocation. If `IDEA.md` changes, update the mirrored summary here.

---

## GEMINI.md

Same pattern as `CLAUDE.md`. Mirror the thesis and boundaries, then add Gemini-specific configuration.

```md
# Project Intent

See `IDEA.md` for the full idea. Summary:

<thesis>

## Boundaries

<what it does not do>

## Gemini-Specific Rules

<tool preferences, output format conventions, etc.>
```

---

## Copilot Instructions

For `.github/copilot-instructions.md` or scoped `.instructions.md` files, reference `IDEA.md` at the top.

```md
## Project Context

This project's intent is defined in `IDEA.md` at the repo root.

Core idea: <thesis>

Do not build features that conflict with the boundaries in `IDEA.md`.

## Coding Instructions

<Copilot-specific rules>
```

---

## PRD.md

`PRD.md` expands on `IDEA.md` when the idea needs more detailed requirements. It should explicitly derive from the idea file, not replace it.

```md
# Product Requirements Document

## Source Idea

This PRD expands on the intent defined in [`IDEA.md`](IDEA.md).

**Thesis:** <paste thesis>

**Boundaries:** All non-goals and constraints from `IDEA.md` apply here
unless explicitly revised below with justification.

## Detailed Requirements

<expanded requirements, user stories, acceptance criteria, etc.>
```

If a requirement in `PRD.md` conflicts with `IDEA.md`, one of the two files needs to be updated. They should never silently disagree.

---

## ARCHITECTURE.md

`ARCHITECTURE.md` defines the technical shape of the system. It should trace design decisions back to the idea.

```md
# Architecture

## Intent

This architecture implements the idea described in [`IDEA.md`](IDEA.md).

Key constraints from the idea:
- <constraint 1>
- <constraint 2>

## System Design

<technical architecture, component diagrams, data flow, etc.>
```

Architecture decisions that push against boundaries in `IDEA.md` should be flagged explicitly, not buried in technical detail.

---

## TASKS.md

`TASKS.md` is the execution plan. Tasks should trace back to scope defined in `IDEA.md`.

```md
# Tasks

Source intent: [`IDEA.md`](IDEA.md)

## Active

- [ ] <task derived from IDEA.md scope>
- [ ] <task derived from IDEA.md scope>

## Deferred

- [ ] <task outside initial scope per IDEA.md>
```

If a task doesn't connect to something in `IDEA.md`, ask whether the idea file needs updating or the task is out of scope.

---

## General Rules

1. **One direction of authority.** `IDEA.md` flows down into other files. Other files do not redefine the idea.

2. **Mirror, don't duplicate.** Tool-specific files should contain a short summary of the thesis and boundaries — not a full copy of `IDEA.md`. One source of truth, thin mirrors everywhere else.

3. **Flag conflicts, don't hide them.** If any downstream file needs to deviate from `IDEA.md`, it should say so explicitly and explain why. Silent contradictions are the most common way projects drift from their original intent.

4. **Keep mirrors fresh.** When `IDEA.md` changes, update the summaries in `CLAUDE.md`, `GEMINI.md`, `AGENTS.md`, and any other files that reference it. This is a good candidate for an agent lint task.
