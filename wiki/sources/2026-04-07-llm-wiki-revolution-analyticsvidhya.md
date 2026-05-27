---
type: source
date: 2026-04-07
raw_path: raw/LLM Wiki Revolution How Andrej Karpathy's Idea is Changing AI.md
created: 2026-05-02
---

# LLM Wiki Revolution: How Andrej Karpathy's Idea is Changing AI

## Metadata

- **Type**: article
- **Author**: Riya Bansal
- **Date**: 2026-04-07
- **URL**: https://www.analyticsvidhya.com/blog/2026/04/llm-wiki-by-andrej-karpathy/

## Summary

Technical deep-dive into Karpathy's LLM Wiki concept, contrasting it with traditional RAG systems. Provides detailed implementation steps, complete tool stack (Obsidian, qmd, Git), classification-based ingestion strategy, and practical code examples for building a production-grade personal knowledge base.

## Key Takeaways

- **Core criticism of RAG**: "the model doesn't accumulate knowledge" — each query rediscovers information from scratch
- **LLM Wiki paradigm shift**: Process documents at ingestion time (once) vs. query time (repeatedly)
- **Knowledge compilation**: "Compile knowledge once, query forever" — knowledge is built and maintained continuously
- **Six-step workflow**: (1) gather resources, (2) classify before extracting, (3) AI writes wiki pages, (4) create index, (5) record query results, (6) conduct lint passes
- **Classification strategy**: Different document types need different extraction approaches (50-page whitepaper = section-by-section; tweet = primary insight + context; meeting transcript = decisions + action items + quotes)
- **Query result filing**: Save valuable query responses as new wiki pages tagged `query-result` — "your best thinking has been collected rather than lost in chat logs"
- **Lint passes**: Periodically audit wiki for contradictions, obsolete statements, orphan pages, missing concept pages
- **Tool stack**: Obsidian (graph view + Dataview plugin + Web Clipper), qmd (local search with BM25/semantic/hybrid), Git (version control)
- **qmd capabilities**: Hybrid search with three modes (keyword BM25, semantic vsearch, LLM re-ranking query), MCP server mode for Claude Code integration, fully local (node-llama-cpp with GGUF models)
- **Git workflow**: Version history for knowledge, commit after each ingest, diff to see changes, revert bad compilations, collaborative via GitHub
- **Practical challenges**: (1) setup complexity, (2) prompt engineering iteration, (3) scalability beyond few hundred pages, (4) consistency maintenance, (5) agent proficiency as learnable skill

## Entities Mentioned

- [Andrej Karpathy](wiki/entities/andrej-karpathy.md)
- [Obsidian](wiki/entities/obsidian.md)
- [Claude Code](wiki/entities/claude-code.md)
- [qmd](wiki/entities/qmd.md) — NEW
- [Tobi Lütke](wiki/entities/tobi-lutke.md) — NEW

## Concepts Discussed

- [LLM Wiki Pattern](wiki/concepts/llm-wiki-pattern.md) — updated with more operational detail
- [RAG vs. LLM Wiki](wiki/concepts/rag-vs-llm-wiki.md) — NEW: explicit comparison
- [Document Classification for Ingestion](wiki/concepts/document-classification-ingestion.md) — NEW
- [Query Result Filing](wiki/concepts/query-result-filing.md) — NEW
- [Lint Passes](wiki/concepts/lint-passes.md) — NEW
- [Knowledge Compilation](wiki/concepts/knowledge-compilation.md) — NEW

## Notable Quotes

> "With LLMs, knowledge is created and maintained continuously and consistently rather than being able to use individual queries to create knowledge." — Andrej Karpathy

> "Your knowledge, finally, working as hard for you as you worked to acquire it."

> "The wiki becomes more valuable with each new source material you include. The data belongs to you. The files exist in formats which can be used by any system."

## Integration Notes

This source significantly deepens technical understanding from the MindStudio article:

**New insights**:
- Explicit RAG critique and comparison table
- Classification-based ingestion strategy (different approaches for different document types)
- Query result filing as a practice (save good answers back into wiki)
- Lint passes as explicit maintenance workflow
- Complete tool stack with code examples (qmd, Git, Dataview)
- qmd as scaling solution with MCP integration
- Practical challenges and learning curve acknowledgment

**Confirms from first source**:
- Markdown foundation
- Obsidian as front-end
- Index.md as navigation
- Incremental knowledge building

**Extends understanding**:
- Adds concrete workflows (classify → extract → write → index → file queries → lint)
- Provides actual code examples for qmd, Git, Dataview
- Discusses scalability threshold (~100-200 pages before needing search)
- Emphasizes prompt engineering as critical skill
