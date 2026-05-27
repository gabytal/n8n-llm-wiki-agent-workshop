---
type: concept
created: 2026-05-02
updated: 2026-05-02
sources: [2026-04-06-karpathy-llm-wiki-mindstudio.md]
---

# Query vs. Browse Interface

## Definition

The fundamental shift in knowledge base interaction when moving from human navigation (browsing folders, searching, clicking) to LLM-mediated access (asking questions in natural language and receiving synthesized answers).

## Key Principles

1. **Human navigation is manual**: User remembers where things are, searches for keywords, navigates folder hierarchies
2. **LLM navigation is semantic**: User describes what they need, LLM finds and synthesizes across the entire knowledge base
3. **Different optimization targets**: Browse interfaces optimize for human scanning; query interfaces optimize for machine reading and synthesis
4. **Cognitive load shift**: User no longer needs to remember locations or manually connect information across sources

## How It Works

### Traditional Browse Pattern

1. User recalls (or searches for) where information might be
2. User navigates to that location
3. User reads document
4. If information spans multiple docs, user manually synthesizes

**Optimization**: Clear folder structure, good naming, fast search, easy navigation

### LLM Query Pattern

1. User asks question in natural language
2. LLM searches across all files for relevant content
3. LLM reads relevant files
4. LLM synthesizes answer with citations

**Optimization**: Consistent structure per file, good summaries, clear terminology, dense cross-references

### What Changes

| Aspect | Browse | Query |
|--------|--------|-------|
| **Navigation** | Manual (folders, links, search) | Natural language description |
| **Synthesis** | Human performs | LLM performs |
| **Structure priority** | User-friendly hierarchy | Machine-readable consistency |
| **Discovery** | User browses, stumbles upon | LLM finds across corpus |
| **Memory burden** | User must recall locations | LLM handles retrieval |

## Examples

### Browse Interaction

```
User: "Where did I put that note about RAG systems?"
→ Searches for "RAG"
→ Opens folder "research/AI"
→ Scans file list
→ Opens "rag-overview.md"
→ Reads full document
→ If incomplete, repeats for other docs
→ Manually connects information
```

### Query Interaction

```
User: "What notes do I have about RAG systems?"
Claude: Searches files → Reads relevant ones → Synthesizes:
"You have three notes discussing RAG:
1. rag-overview.md - describes basic architecture
2. llm-wiki-comparison.md - contrasts RAG with wiki pattern
3. research-notes.md - mentions LlamaIndex as RAG tool

Key points: [synthesis of all three sources]"
```

## Related Concepts

- [LLM Wiki Pattern](wiki/concepts/llm-wiki-pattern.md) — built around query interface
- [Markdown as LLM-Native Format](wiki/concepts/markdown-llm-native.md) — optimized for LLM reading
- [Knowledge Base Maintenance](wiki/concepts/knowledge-base-maintenance.md)

## Related Entities

- [Claude Code](wiki/entities/claude-code.md) — provides query interface
- [Obsidian](wiki/entities/obsidian.md) — provides browse interface (complementary)

## Contradictions / Open Questions

- **Hybrid approach**: The pattern doesn't eliminate browsing — users still read and write in Obsidian. The distinction is more about primary interaction mode for retrieval and synthesis.
- **Discovery**: Browsing can lead to serendipitous discovery; querying is goal-directed. Trade-off not fully explored in source.

## References

- [2026-04-06-karpathy-llm-wiki-mindstudio.md](wiki/sources/2026-04-06-karpathy-llm-wiki-mindstudio.md)
