---
type: concept
created: 2026-05-02
updated: 2026-05-02
sources: [2026-04-07-llm-wiki-revolution-analyticsvidhya.md]
---

# Document Classification for Ingestion

## Definition

The practice of classifying document types before extraction to determine appropriate level of detail and extraction strategy. Different document types require different processing approaches for optimal knowledge capture.

## Key Principles

1. **Not all documents are equal**: A research paper requires different extraction than a tweet
2. **Classify before extracting**: Determine document type first, then apply appropriate extraction strategy
3. **Match extraction depth to source value**: Dense sources get section-by-section analysis; brief sources get primary insight + context
4. **Preserve appropriate metadata**: Different types need different metadata (papers need citations; meetings need attendees)

## How It Works

### Document Type Examples and Strategies

**50-page research whitepaper**:
- **Strategy**: Section-by-section extraction
- **Detail level**: High — capture methodology, findings, limitations per section
- **Output**: Multiple concept pages + detailed source summary
- **Metadata**: Authors, institution, publication date, citations

**Tweet or social media thread**:
- **Strategy**: Extract primary insight + context
- **Detail level**: Low — capture core claim and supporting evidence
- **Output**: Brief note, possibly update existing concept page
- **Metadata**: Author, date, URL

**Meeting transcript**:
- **Strategy**: Extract decisions, action items, key quotations
- **Detail level**: Medium — focus on actionable information
- **Output**: Meeting summary with tagged decisions and owners
- **Metadata**: Attendees, date, meeting type, follow-ups

**Book**:
- **Strategy**: Chapter-by-chapter processing
- **Detail level**: High — themes, arguments, examples per chapter
- **Output**: Multiple concept/entity pages + chapter summaries
- **Metadata**: Author, publication date, edition, ISBN

**Article (blog post, news)**:
- **Strategy**: Full read with key points extraction
- **Detail level**: Medium — main argument, supporting evidence, conclusions
- **Output**: Source summary + updates to relevant concept pages
- **Metadata**: Author, publication, date, URL

**Video/podcast transcript**:
- **Strategy**: Timestamp-based extraction of key segments
- **Detail level**: Medium — main topics with timestamps
- **Output**: Timestamped notes + concept updates
- **Metadata**: Speakers, date, duration, platform

## Examples

### Good Classification Flow

```
New source: "Attention Is All You Need" (paper, 8 pages)
→ Classify as: Academic paper
→ Strategy: Section-by-section (abstract, intro, architecture, experiments, conclusion)
→ Extract: Technical concepts, architectural diagrams, performance metrics
→ Create: Transformer concept page, attention mechanism page, update related architecture pages
→ Metadata: Authors (Vaswani et al.), venue (NIPS 2017), citations count
```

### Poor Classification (Anti-pattern)

```
New source: Tweet thread by researcher
→ No classification: treat like research paper
→ Strategy: Attempt detailed section analysis
→ Result: Over-extraction, noise, wasted LLM tokens
→ Better: Classify as "brief insight", extract main claim + link
```

## Related Concepts

- [LLM Wiki Pattern](wiki/concepts/llm-wiki-pattern.md) — overall framework
- [Knowledge Compilation](wiki/concepts/knowledge-compilation.md)
- [Lint Passes](wiki/concepts/lint-passes.md)

## Related Entities

- [Claude Code](wiki/entities/claude-code.md) — performs classification and extraction

## Contradictions / Open Questions

- **Automation**: Can document type classification be automated reliably? Or does it require human judgment?
- **Edge cases**: How to classify hybrid documents (e.g., blog post with embedded research data)?
- **Over-classification**: Is there a point where too many document types create excessive complexity?

## References

- [2026-04-07-llm-wiki-revolution-analyticsvidhya.md](wiki/sources/2026-04-07-llm-wiki-revolution-analyticsvidhya.md)
