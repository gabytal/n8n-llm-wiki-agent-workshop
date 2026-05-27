---
type: concept
created: 2026-05-02
updated: 2026-05-02
sources: [2026-04-07-llm-wiki-revolution-analyticsvidhya.md]
---

# Query Result Filing

## Definition

The practice of saving valuable query responses back into the wiki as new pages, preserving insights and synthesis that would otherwise be lost in chat history. Tagged with `query-result` label.

## Key Principles

1. **Preserve valuable thinking**: Good answers shouldn't disappear
2. **Queries produce knowledge**: Well-formed questions + insightful responses = new wiki content
3. **Tag for tracking**: Use `query-result` tag to distinguish from ingested sources
4. **Selective filing**: Not every query needs filing — only those providing lasting value
5. **Cumulative benefit**: "Your best thinking has been collected rather than lost in chat logs"

## How It Works

### When to File a Query Result

**File when the response contains**:
- Comparison between frameworks/concepts
- Explanation of relationships between ideas
- Synthesis across multiple sources
- Novel insight from connecting disparate information
- Clarification of confusion/contradiction
- Decision rationale or tradeoff analysis

**Don't file when the response is**:
- Simple factual lookup (already in a source)
- Temporary/context-specific information
- Redundant with existing pages
- Trivial or low-value

### Filing Workflow

1. Ask well-formed question to LLM
2. Receive valuable, synthesized response
3. Assess: "Would I want to find this answer again later?"
4. If yes: Save as new wiki page in `wiki/analysis/` or relevant category
5. Add `query-result` tag in frontmatter
6. Update index.md
7. Cross-reference from related concept/entity pages

## Examples

### Query Worth Filing

```
Query: "What are the key differences between Karpathy's LLM Wiki 
pattern and traditional RAG systems?"

Response: [Detailed comparison with specific points about knowledge 
accumulation, processing timing, cross-referencing...]

Action: Save as wiki/analysis/rag-vs-llm-wiki-comparison.md
Tag: query-result
Update: Link from both RAG and LLM Wiki concept pages
```

### Query Not Worth Filing

```
Query: "What date was source X published?"

Response: "2026-04-07"

Action: Don't file — simple fact lookup, already in source metadata
```

### Progressive Knowledge Building

```
Week 1: Query about concept A → File insightful response
Week 2: Query about concept B → File response
Week 3: Query about A vs B → LLM reads both previous query results, 
synthesizes deeper comparison → File this meta-synthesis

Result: Knowledge compounds over time
```

## Related Concepts

- [LLM Wiki Pattern](wiki/concepts/llm-wiki-pattern.md)
- [Knowledge Compilation](wiki/concepts/knowledge-compilation.md)
- [RAG vs. LLM Wiki](wiki/concepts/rag-vs-llm-wiki.md) — addresses RAG's "lost in chat history" problem

## Related Entities

- [Claude Code](wiki/entities/claude-code.md) — interface for queries and filing

## Contradictions / Open Questions

- **Selection criteria**: What's the precise threshold for "valuable enough to file"? Seems subjective.
- **Maintenance burden**: Do filed query results become stale and need updating like source-derived pages?
- **Distinction from analysis pages**: How do `query-result` pages differ from planned analysis pages?

## References

- [2026-04-07-llm-wiki-revolution-analyticsvidhya.md](wiki/sources/2026-04-07-llm-wiki-revolution-analyticsvidhya.md)
