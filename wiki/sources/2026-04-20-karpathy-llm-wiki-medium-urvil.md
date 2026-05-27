---
type: source
date: 2026-04-20
raw_path: raw/Andrej Karpathy's LLM Wiki Create your own knowledge base.md
created: 2026-05-02
---

# Andrej Karpathy's LLM Wiki: Create your own knowledge base

## Metadata

- **Type**: article / tutorial
- **Author**: Urvil Joshi
- **Date**: 2026-04-20
- **URL**: https://medium.com/@urvvil08/andrej-karpathys-llm-wiki-create-your-own-knowledge-base-8779014accd5

## Summary

Medium article providing step-by-step tutorial for building an LLM Wiki, emphasizing Karpathy's compilation analogy from software engineering. Includes visual diagrams explaining three-layer architecture and three core operations. Walks through practical setup with Claude Code and Obsidian.

## Key Takeaways

**Origin story**: 
- Karpathy's April 2, 2026 tweet about using LLMs to build personal knowledge bases
- Followed by GitHub gist "llm-wiki.md" — an "idea file" meant to be copy-pasted into LLM agents
- Not a product or code, but a *pattern*

**Core insight in one sentence**:
> "Instead of having the LLM re-read your raw documents every time you ask a question, build a persistent, structured wiki once and keep it updated forever."

**Software compilation analogy**:
```
Source Code → [compile once] → Binary (runs fast every call)
       ⇕  same idea  ⇕
Raw Sources → [LLM compiles] → Wiki (pre-synthesized, interlinked, always ready)
```

**Key quote capturing philosophy**:
> "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase."

**Three-layer architecture** (with visual diagram):
1. **Raw sources** (immutable) — PDFs, articles, notes, images; LLM reads but never modifies
2. **The wiki** (LLM owns entirely) — Entity pages, concept pages, summaries, index.md, log.md
3. **The schema** (CLAUDE.md/AGENTS.md) — Rules, conventions, workflows; turns generic agent into disciplined wiki maintainer

**Three operations** (with visual diagram):
1. **Ingest**: Drop source → LLM reads, summarizes, updates 10-15 wiki pages
2. **Query**: Ask question → LLM reads wiki, synthesizes answer; good answers filed back as new pages
3. **Lint**: Health-check wiki → find contradictions, orphans, stale data

**Immutability principle**: Raw sources never modified — deliberate design choice so wiki can be re-compiled from scratch if needed

**Practical setup tutorial**:
1. Create folder structure (`mkdir llm-wiki-demo && mkdir raw`)
2. Open Claude Code, paste idea file
3. Claude asks clarifying questions (topic, sources, page types)
4. Answer → Claude writes tailored CLAUDE.md, initializes index/log
5. Ingest sources

**Example setup**: Wiki about "AI research and philosophy of software", 10-20 sources (essays from Rich Sutton, Karpathy), concept pages + essay summaries + author pages

## Entities Mentioned

- [Andrej Karpathy](wiki/entities/andrej-karpathy.md)
- [Claude Code](wiki/entities/claude-code.md)
- [Obsidian](wiki/entities/obsidian.md)

## Concepts Discussed

- [Knowledge Compilation](wiki/concepts/knowledge-compilation.md) — software engineering analogy emphasized
- [LLM Wiki Pattern](wiki/concepts/llm-wiki-pattern.md) — three-layer architecture, three operations
- [Lint Passes](wiki/concepts/lint-passes.md)
- [Query Result Filing](wiki/concepts/query-result-filing.md) — "good answers filed back as new pages"

## Notable Quotes

> "You don't execute source code every time you want to run a program. You compile it once into a binary and run *that*. Karpathy says: treat knowledge the same way. Your PDFs and notes are the source code. The wiki is the binary."

> "Every time you add a new document, the LLM doesn't just index it. It **reads it, extracts the key information, updates existing pages, revises summaries, flags contradictions, and strengthens cross-links**. The wiki is a persistent, compounding artifact."

> "You rarely write the wiki yourself. You curate sources, ask questions, and think. The LLM handles the whole work summarizing, cross-referencing, filing, and bookkeeping."

## Integration Notes

**Reinforces from previous sources**:
- Compilation metaphor (from Analytics Vidhya)
- Three-layer architecture
- Three operations (ingest/query/lint)
- Immutable raw sources

**New emphasis**:
- **Software engineering analogy**: Most explicit use of compilation metaphor (source code → binary ≈ raw sources → wiki)
- **Visual diagrams**: Provides clear visual representations of architecture and operations
- **"Idea file" concept**: Emphasizes that llm-wiki.md is meant to be copy-pasted, not followed literally
- **Setup tutorial**: Step-by-step walkthrough of initial conversation with Claude
- **Immutability rationale**: Explains *why* raw sources are never modified (re-compilation capability)

**Confirms**:
- Karpathy's tweet (April 2, 2026) as origin
- GitHub gist as primary reference document
- Pattern (not product) framing
- Claude Code as primary agent
- Obsidian as viewer/IDE

**Practical contribution**: Most beginner-friendly tutorial-style explanation with concrete steps for first-time setup.
