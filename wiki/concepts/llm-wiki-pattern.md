---
type: concept
created: 2026-05-02
updated: 2026-05-02
sources: [2026-04-06-karpathy-llm-wiki-mindstudio.md]
---

# LLM Wiki Pattern

## Definition

A personal knowledge management system using plain markdown files structured specifically for LLM querying rather than human browsing. The LLM reads, synthesizes, and maintains the knowledge base on behalf of the user through natural language queries.

## Key Principles

1. **Optimize for machine reading**: Structure information for AI consumption, not just human navigation
2. **Persistent synthesis**: Knowledge is compiled and maintained incrementally, not re-derived on every query
3. **Plain text foundation**: Use markdown for portability, future-proofing, and LLM-native understanding
4. **Local-first**: Files remain under user control, no proprietary databases
5. **Minimal architecture**: Just files + LLM interface, no complex infrastructure needed initially

## How It Works

### Three Components

1. **Folder of markdown files** — The knowledge base containing research notes, meeting summaries, documentation, reference material
2. **Consistent structure** — Each file has title, summary, tags, content with predictable formatting
3. **LLM query interface** — Claude Code (or similar) reads files directly and synthesizes answers

### Core Workflow

- **Capture**: Add new notes in markdown format
- **Query**: Ask questions in natural language
- **Synthesis**: LLM reads relevant files and generates grounded answers with citations
- **Maintenance**: LLM helps organize, cross-reference, and clean up the knowledge base

### Distinction from Traditional Systems

| Traditional Notes App | LLM Wiki |
|----------------------|----------|
| User browses and searches manually | User asks questions in natural language |
| Optimized for navigation | Optimized for machine reading |
| User finds and synthesizes | LLM finds and synthesizes |
| Static organization | Dynamic cross-referencing |

## Examples

**Use cases**:
- Personal knowledge management (goals, health, learning)
- Research projects (papers, articles, evolving thesis)
- Reading companions (chapter notes, character/theme wikis)
- Business wikis (meeting notes, project docs, customer calls)
- Course notes, hobby deep-dives, competitive analysis

**Sample queries**:
- "What notes do I have about machine learning interpretability?"
- "Summarize everything related to RAG systems"
- "Find mentions of vendor Acme Corp and summarize key points"
- "I'm writing a proposal on X — what relevant notes do I have?"

## Related Concepts

- [Markdown as LLM-Native Format](wiki/concepts/markdown-llm-native.md) — why markdown is the ideal format
- [Query vs. Browse Interface](wiki/concepts/query-vs-browse.md) — fundamental UX shift
- [Knowledge Base Maintenance](wiki/concepts/knowledge-base-maintenance.md) — keeping the wiki healthy

## Related Entities

- [Andrej Karpathy](wiki/entities/andrej-karpathy.md) — originator of the pattern
- [Claude Code](wiki/entities/claude-code.md) — primary implementation tool
- [Obsidian](wiki/entities/obsidian.md) — recommended front-end editor

## Contradictions / Open Questions

None identified yet. This is the first source ingested.

## References

- [2026-04-06-karpathy-llm-wiki-mindstudio.md](wiki/sources/2026-04-06-karpathy-llm-wiki-mindstudio.md)
