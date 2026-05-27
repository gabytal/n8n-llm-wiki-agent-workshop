---
type: entity
category: tool
created: 2026-05-02
updated: 2026-05-02
sources: [2026-04-07-llm-wiki-revolution-analyticsvidhya.md]
---

# qmd

## Overview

Local search engine for Markdown files developed by Tobi Lütke (CEO of Shopify). Provides hybrid BM25/vector/LLM-reranking search with MCP server support for Claude Code integration. Fully on-device, no API connections required.

## Key Information

- **Developer**: Tobi Lütke (Shopify CEO)
- **Purpose**: Scale LLM Wiki search beyond ~100-200 pages where index.md becomes unwieldy
- **Installation**: `npm install -g @tobilu/qmd`
- **Underlying tech**: node-llama-cpp with GGUF models (fully local)
- **Privacy**: All processing on-device, data never leaves computer

## Search Modes

**Three search methods**:

1. **Keyword search (BM25)**:
   ```bash
   qmd search "mixture of experts routing efficiency"
   ```
   Traditional keyword matching

2. **Semantic search (vsearch)**:
   ```bash
   qmd vsearch "how do sparse models handle load balancing"
   ```
   Finds related concepts even with different words

3. **Hybrid search with LLM re-ranking (query)**:
   ```bash
   qmd query "what are the tradeoffs between top-k and expert-choice routing"
   ```
   Highest quality results, combines multiple approaches

## Key Features

- **Collection management**: Register wiki as searchable collection
  ```bash
  qmd collection add ./wiki --name my-research
  ```

- **JSON output for automation**:
  ```bash
  qmd query "scaling laws" --json | jq '.results[].title'
  ```

- **MCP server mode**: Exposes qmd as native tool for Claude Code
  ```bash
  qmd mcp
  ```
  Enables seamless integration into agent workflows

## When to Use

- Wiki exceeds 100-200 pages
- Index.md too large for single context window
- Need precise retrieval across large corpus
- Want to maintain local-first, privacy-preserving architecture

## Related Concepts

- [LLM Wiki Pattern](wiki/concepts/llm-wiki-pattern.md) — scaling solution for large wikis
- [Knowledge Compilation](wiki/concepts/knowledge-compilation.md)

## Related Entities

- [Tobi Lütke](wiki/entities/tobi-lutke.md) — creator
- [Claude Code](wiki/entities/claude-code.md) — integrates via MCP

## Mentions

- [Source: Analytics Vidhya article (2026-04-07)](wiki/sources/2026-04-07-llm-wiki-revolution-analyticsvidhya.md) — Describes qmd as scaling solution with code examples

## References

- [2026-04-07-llm-wiki-revolution-analyticsvidhya.md](wiki/sources/2026-04-07-llm-wiki-revolution-analyticsvidhya.md)
