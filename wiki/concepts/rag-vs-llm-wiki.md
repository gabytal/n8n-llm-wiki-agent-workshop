---
type: concept
created: 2026-05-02
updated: 2026-05-02
sources: [2026-04-07-llm-wiki-revolution-analyticsvidhya.md]
---

# RAG vs. LLM Wiki

## Definition

The fundamental architectural difference between Retrieval-Augmented Generation (RAG) systems that process knowledge at query time and LLM Wiki systems that compile knowledge once at ingestion time.

## Key Principles

**RAG's core limitation**: "The model doesn't accumulate knowledge" — each query rediscovers information from scratch.

**LLM Wiki's paradigm**: "Compile knowledge once, query forever" — knowledge is built and maintained continuously.

## How They Differ

### Detailed Comparison Table

| Dimension | Traditional RAG | LLM Wiki |
|-----------|-----------------|----------|
| **When knowledge is processed** | At query time (for every question) | At ingest time (once per source) |
| **Cross-references** | Discovered ad hoc or missed entirely | Pre-built and continuously maintained |
| **Contradictions between sources** | Often overlooked | Detected and flagged during ingestion |
| **Knowledge accumulation** | None — resets with each query | Builds over time with every new source |
| **Output format** | Temporary chat responses | Persistent, editable Markdown files |
| **Data ownership** | Stored within provider systems | Fully controlled on local machine |
| **Synthesis quality** | Ad hoc, per query | Accumulated, refined over time |
| **Temporal connections** | Struggles to link across time (e.g., March article + October article) | Maintains timeline and evolution of concepts |

## The RAG Problem

**Core issue**: If a question requires synthesizing five documents:
- RAG retrieves and combines them for that one response
- Ask the same question tomorrow → repeats entire process
- No persistence of the synthesis
- No learning from previous queries

**Scaling limitation**: 
- Struggles with questions spanning many sources
- Doesn't connect information temporally
- "Produces answers, but doesn't build lasting knowledge"

## The LLM Wiki Solution

**Processing at ingestion**:
- LLM reads, understands, integrates source into knowledge base
- Updates all relevant existing pages
- Notes contradictions between new and existing claims
- Creates new concept pages as needed
- Reinforces relationships across entire wiki

**Cumulative benefits**:
- Each new source enriches the whole system
- Cross-references already built
- Contradictions already flagged
- Synthesis already performed and editable

## Examples

### RAG Workflow
```
User: "What are the tradeoffs between approach A and B?"
→ RAG retrieves chunks mentioning A and B
→ LLM synthesizes temporary answer
→ Answer disappears into chat history

Next day, same question:
→ Entire process repeats identically
```

### LLM Wiki Workflow
```
Ingestion:
→ Source 1 mentions approach A → creates concept page
→ Source 2 mentions approach B → creates concept page  
→ Source 3 compares A vs B → updates both pages, adds cross-references

User query: "What are the tradeoffs between approach A and B?"
→ Reads existing concept pages (already synthesized)
→ Provides answer with citations
→ Optionally: save this synthesis as new "A vs B comparison" page
```

## Related Concepts

- [LLM Wiki Pattern](wiki/concepts/llm-wiki-pattern.md)
- [Knowledge Compilation](wiki/concepts/knowledge-compilation.md)
- [Query Result Filing](wiki/concepts/query-result-filing.md)

## Related Entities

- [Andrej Karpathy](wiki/entities/andrej-karpathy.md) — articulated this critique

## Contradictions / Open Questions

- **Hybrid approaches**: Could RAG and Wiki patterns be combined? E.g., use wiki for core knowledge and RAG for very recent/external sources?
- **RAG advantages not discussed**: When might RAG's stateless nature be preferable (e.g., privacy for ephemeral queries)?

## References

- [2026-04-07-llm-wiki-revolution-analyticsvidhya.md](wiki/sources/2026-04-07-llm-wiki-revolution-analyticsvidhya.md)
