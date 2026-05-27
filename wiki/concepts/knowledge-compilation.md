---
type: concept
created: 2026-05-02
updated: 2026-05-02
sources: [2026-04-07-llm-wiki-revolution-analyticsvidhya.md]
---

# Knowledge Compilation

## Definition

The core principle of LLM Wiki: process and synthesize knowledge once at ingestion time, creating a persistent, structured representation that can be queried indefinitely without re-processing. Contrasts with RAG's approach of retrieving and synthesizing at query time.

## Key Principles

1. **"Compile knowledge once, query forever"** — pay processing cost upfront
2. **Ingestion-time processing** vs. query-time processing
3. **Persistent synthesis** — knowledge built and maintained continuously
4. **Cumulative accumulation** — each new source enriches the entire system
5. **Editable artifacts** — compiled knowledge is Markdown, can be refined manually

## How It Works

### The Compilation Process

**At ingestion time**:
1. LLM reads new source document
2. Extracts key information
3. Identifies entities and concepts
4. **Integrates** into existing wiki:
   - Updates relevant entity pages
   - Updates relevant concept pages
   - Creates new pages as needed
   - Notes contradictions
   - Builds cross-references
5. Result: Knowledge is "compiled" into the wiki structure

**At query time**:
1. Read index.md to find relevant pages
2. Read compiled pages (already synthesized)
3. Generate answer from pre-compiled knowledge
4. Much faster and more consistent than re-synthesizing from raw sources

### Compilation vs. Retrieval

**RAG (Retrieval)**:
```
Query → Retrieve chunks → Synthesize on-the-fly → Temporary answer
```
Cost paid: every query

**LLM Wiki (Compilation)**:
```
Ingest → Compile into structured pages → [one-time cost]
Query → Read compiled pages → Fast answer
```
Cost paid: once per source

### Analogy to Software Compilation

**Interpreted language** (like RAG):
- Parse and execute code every time program runs
- Flexible, but slow
- No persistent optimizations

**Compiled language** (like LLM Wiki):
- Parse and optimize code once
- Fast execution from compiled artifact
- Can manually tune compiled output

## Examples

### Compilation in Action

```
Source 1 ingested:
→ Creates "Transformers" concept page
→ Creates "Attention mechanism" concept page  
→ Links them together

Source 2 ingested (about BERT):
→ Reads existing "Transformers" page
→ Updates it with "BERT is a transformer variant"
→ Creates "BERT" entity page
→ Links to both Transformers and Attention

Source 3 ingested (about GPT):
→ Reads existing "Transformers" page
→ Updates with "GPT is another transformer variant"
→ Creates "GPT" entity page
→ Now "Transformers" page has rich context compiled from 3 sources

Query: "What are transformer variants?"
→ Reads compiled "Transformers" page
→ Immediately sees BERT and GPT mentioned
→ Follows links to their pages
→ Fast, comprehensive answer
```

### Without Compilation (RAG)

```
Query: "What are transformer variants?"
→ Retrieves chunks mentioning transformers
→ Finds BERT in Source 2, GPT in Source 3
→ Synthesizes answer on-the-fly
→ May miss connections if chunks don't co-occur

Same query tomorrow:
→ Repeats entire process
→ No accumulated understanding
```

## Related Concepts

- [LLM Wiki Pattern](wiki/concepts/llm-wiki-pattern.md)
- [RAG vs. LLM Wiki](wiki/concepts/rag-vs-llm-wiki.md)
- [Document Classification for Ingestion](wiki/concepts/document-classification-ingestion.md)

## Related Entities

- [Andrej Karpathy](wiki/entities/andrej-karpathy.md) — originated this approach
- [Claude Code](wiki/entities/claude-code.md) — performs compilation

## Contradictions / Open Questions

- **Freshness tradeoff**: Compilation requires re-ingesting sources to update. RAG always reads latest. When does staleness become a problem?
- **Compilation cost**: Is there a document type where query-time synthesis is cheaper than ingestion-time compilation? (e.g., very long documents with rare queries)
- **Hybrid models**: Could you compile core knowledge but use RAG for supplementary/ephemeral sources?

## References

- [2026-04-07-llm-wiki-revolution-analyticsvidhya.md](wiki/sources/2026-04-07-llm-wiki-revolution-analyticsvidhya.md)
