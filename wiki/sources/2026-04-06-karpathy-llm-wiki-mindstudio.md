---
type: source
date: 2026-04-06
raw_path: raw/What Is Andrej Karpathy's LLM Wiki? How to Build a Personal Knowledge Base With Claude Code.md
created: 2026-05-02
---

# What Is Andrej Karpathy's LLM Wiki? How to Build a Personal Knowledge Base With Claude Code

## Metadata

- **Type**: article
- **Author**: MindStudio Team
- **Date**: 2026-04-06
- **URL**: https://www.mindstudio.ai/blog/andrej-karpathy-llm-wiki-knowledge-base-claude-code

## Summary

A comprehensive guide explaining Karpathy's LLM Wiki pattern: a personal knowledge management system using plain markdown files structured for LLM querying rather than human browsing. The article covers setup with Obsidian and Claude Code, best practices for queryable knowledge bases, and the advantages of markdown as a format for AI-assisted knowledge work.

## Key Takeaways

- **Core distinction**: Traditional notes apps are optimized for human navigation; LLM wiki is optimized for the model to read and synthesize on your behalf
- **Three-component architecture**: (1) folder of markdown files, (2) consistent internal structure per file, (3) Claude Code as query interface
- **Markdown advantages**: portable, future-proof, LLMs read it natively, forces clarity, no vendor lock-in
- **Claude Code's role**: terminal-based agent with direct filesystem access - can read, search, create, and update files without copy-pasting
- **Setup with Obsidian**: local-first markdown editor, vault = folder, recommended for graph view and backlinks
- **Best practices**: write one-line summaries per note, use consistent terminology, link notes with `[[wiki links]]`, keep notes focused, use `/inbox` for rough captures
- **Scaling**: direct file-reading works well up to hundreds of notes; add semantic search (RAG with LlamaIndex) at larger scale
- **Context window**: Claude 3.5 Sonnet supports ~200k tokens, enough for tens of thousands of words in one session

## Entities Mentioned

- [Andrej Karpathy](wiki/entities/andrej-karpathy.md)
- [Claude Code](wiki/entities/claude-code.md)
- [Obsidian](wiki/entities/obsidian.md)
- [MindStudio](wiki/entities/mindstudio.md)

## Concepts Discussed

- [LLM Wiki Pattern](wiki/concepts/llm-wiki-pattern.md)
- [Markdown as LLM-Native Format](wiki/concepts/markdown-llm-native.md)
- [Query vs. Browse Interface](wiki/concepts/query-vs-browse.md)
- [Knowledge Base Maintenance](wiki/concepts/knowledge-base-maintenance.md)

## Notable Quotes

> "Most note-taking apps are built for human reading. You browse, search, click. They're optimized for you to find things manually. An LLM wiki is optimized for the model to read on your behalf. That shift changes everything about how you structure information."

> "The one-line summary at the top of each note is surprisingly important. Claude reads it to decide whether the full note is relevant. A good summary costs you ten seconds and saves the model from reading files that don't apply."

> "The best knowledge management system is one you'll actually use. A folder of markdown files is about as low-friction as it gets — and pointing Claude at it takes five minutes."

## Integration Notes

First source ingested into the wiki. Establishes foundational understanding of:
- Karpathy's vision for LLM-assisted knowledge management
- Practical implementation with Claude Code + Obsidian
- Distinction between LLM wiki pattern and traditional RAG systems
- Best practices for structuring queryable knowledge bases

Sets baseline for comparing with other sources about the LLM Wiki concept.
