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
Use an LLM to incrementally build and maintain a persistent wiki of markdown files so that knowledge compounds over time instead of being re-derived from scratch on every question.

## Problem
Most LLM-and-documents workflows are retrieval-first. You upload files, the model searches them at query time, and generates an answer. This works, but the model is rediscovering knowledge from scratch every time. Ask a question that requires synthesizing five sources and it has to find and stitch together the fragments on every ask. Nothing accumulates. NotebookLM, ChatGPT file uploads, and most RAG setups work this way.

The missing layer is a durable synthesis — a place where cross-references are already built, contradictions are already flagged, and the picture gets richer with every source you add. That layer should be maintained by the LLM, not by you.

## How It Works
There are three layers. Raw sources are your curated collection — articles, papers, transcripts, notes. These are immutable; the LLM reads them but never modifies them. The wiki is a directory of LLM-generated markdown pages — summaries, entity pages, concept pages, comparisons, an evolving synthesis. The LLM owns this layer entirely. A schema file tells the LLM how the wiki is structured, what conventions to follow, and what workflows to use for ingesting sources, answering questions, and maintaining the wiki over time.

The key operations are ingest (process a new source into the wiki, updating all relevant pages), query (answer questions using the wiki first, raw sources second, and optionally file good answers back as new wiki pages), and lint (health-check for contradictions, stale claims, orphan pages, and missing links).

An index file gives the LLM a catalog of everything in the wiki. A log file tracks what happened and when. At small to moderate scale, the index is enough for navigation — no embedding infrastructure required.

This can apply to personal knowledge systems, research projects, book reading, team knowledge bases, competitive analysis, or anything where you're accumulating understanding over time. The human curates sources, directs analysis, and asks good questions. The LLM handles the bookkeeping that makes the knowledge base actually stay maintained.

## What It Does Not Do
- Does not replace raw sources as the evidence layer. The wiki is synthesis; the sources are truth.
- Does not require heavyweight RAG infrastructure. Start with an index file and plain markdown.
- Does not prescribe a single directory layout, page format, or toolchain. The idea is the pattern, not the implementation.
- Does not try to be a collaborative multi-user wiki platform from day one. Start personal.

## Where To Start
Discuss the user's domain and what kind of knowledge they want to accumulate. Propose a directory structure, page templates, and a schema file tailored to their use case. Implement the ingest workflow first — that's where value appears fastest. Keep it simple: markdown files, an index, a log. Add search tooling and more structure only as scale demands it.
