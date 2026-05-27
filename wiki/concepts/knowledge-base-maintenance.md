---
type: concept
created: 2026-05-02
updated: 2026-05-02
sources: [2026-04-06-karpathy-llm-wiki-mindstudio.md]
---

# Knowledge Base Maintenance

## Definition

The practices and workflows for keeping a personal knowledge base healthy, organized, and queryable over time. In the LLM Wiki pattern, the LLM assists with or automates much of this maintenance burden.

## Key Principles

1. **Consistent structure**: Every note follows a predictable format (summary, tags, content)
2. **Cross-referencing**: Notes link to related notes using wiki links
3. **Focused scope**: Each note covers a specific topic rather than being a catch-all
4. **Triage pattern**: Use `/inbox` for rough captures, then periodically organize
5. **LLM-assisted cleanup**: Claude can help file, tag, and organize notes

## How It Works

### Critical Maintenance Habits

**Write summaries**:
- One-line summary at top of each note
- Claude reads summaries to decide relevance before loading full file
- "Costs you ten seconds and saves the model from reading files that don't apply"

**Use consistent terminology**:
- Pick one term for each concept (e.g., "RAG" vs. "retrieval augmented generation")
- Add alias lines if a concept has multiple common names
- Improves query precision

**Link notes to each other**:
- Use Obsidian's `[[wiki links]]` format
- Creates a richer graph for LLM reasoning
- Helps LLM follow conceptual connections

**Keep notes focused**:
- Prefer ten 1,000-word notes over one 10,000-word document
- Specific files = precise retrieval
- Split notes when they cover multiple distinct topics

**Use `/inbox` for capture**:
- Don't let perfect organization block capture
- Dump rough notes to inbox
- Periodically ask Claude: *"Look at my inbox folder and suggest where each note should be filed and what tags it should get"*

### Scaling Considerations

**Direct file reading** (simple approach):
- Works well up to a few hundred notes
- Claude's 200k token context = tens of thousands of words per session
- No additional infrastructure needed

**Semantic search layer** (advanced):
- Add when Claude struggles to find known information
- Tools like LlamaIndex build vector index over markdown files
- Enables more precise retrieval before synthesis
- Overkill for most personal wikis initially

## Examples

### Maintenance Query Examples

```
"Look at my inbox folder and suggest where each note should be filed and what tags it should get"

"Find any notes that mention X but don't have a dedicated page for X"

"Are there contradictions between my notes on topic Y?"

"Suggest cross-references I'm missing between these related notes"
```

### Note Template Example

```markdown
# Note Title

**Summary**: One sentence describing this note.
**Tags**: #topic1 #topic2
**Created**: 2026-04-06
**Last Updated**: 2026-04-06

---

## Content

Main content here.

## Related Notes

- [[Related Note 1]]
- [[Related Note 2]]
```

## Related Concepts

- [LLM Wiki Pattern](wiki/concepts/llm-wiki-pattern.md) — overall framework
- [Query vs. Browse Interface](wiki/concepts/query-vs-browse.md)
- [Markdown as LLM-Native Format](wiki/concepts/markdown-llm-native.md)

## Related Entities

- [Claude Code](wiki/entities/claude-code.md) — assists with maintenance
- [Obsidian](wiki/entities/obsidian.md) — provides graph view for seeing connections

## Contradictions / Open Questions

- **Automation level**: How much should be automated vs. user-guided? Source suggests LLM assistance but doesn't fully specify the balance.
- **Metadata standards**: What frontmatter/metadata is essential vs. optional?

## References

- [2026-04-06-karpathy-llm-wiki-mindstudio.md](wiki/sources/2026-04-06-karpathy-llm-wiki-mindstudio.md)
