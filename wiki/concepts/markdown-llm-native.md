---
type: concept
created: 2026-05-02
updated: 2026-05-02
sources: [2026-04-06-karpathy-llm-wiki-mindstudio.md]
---

# Markdown as LLM-Native Format

## Definition

The principle that markdown (`.md` files) is the optimal format for knowledge bases meant to be read and processed by LLMs, due to its simplicity, portability, and prevalence in LLM training data.

## Key Principles

1. **Plain text foundation**: A `.md` file is just text with minimal syntax
2. **LLM training alignment**: Models like Claude are trained on vast amounts of markdown (GitHub READMEs, documentation, forums)
3. **Structural clarity**: Headers, lists, code blocks, bold text are interpreted as structure, not noise
4. **No lock-in**: Files open in any editor, on any OS, forever
5. **Forces clarity**: Format nudges toward well-organized, clearly structured notes

## How It Works

### Why LLMs Read Markdown Well

- **Native understanding**: Markdown syntax is part of LLM training data
- **Semantic structure**: Headers convey hierarchy, lists convey relationships, code blocks preserve formatting
- **Minimal noise**: Unlike rich text formats, markdown has little formatting overhead that obscures content
- **Text-based**: Models fundamentally process text; markdown is text + light annotations

### Practical Advantages

**Portability**:
- Opens in any text editor
- No proprietary encoding
- No export/import friction
- Git-friendly (diffs work naturally)

**Future-proofing**:
- Not dependent on company survival
- No backward compatibility concerns
- Will remain readable indefinitely

**Tooling compatibility**:
- Works with VS Code, Obsidian, Typora, iA Writer, Vim, etc.
- Can sync via git
- Can process with standard unix tools (grep, sed, awk)
- Can push to GitHub, GitLab, etc.

## Examples

### Markdown Elements LLMs Understand

```markdown
# Headers convey topic hierarchy
## Subsections organize subtopics

- Bullet points indicate lists of related items
- Multiple points at same level = same category

**Bold text** signals emphasis or key terms

> Block quotes preserve important verbatim content

`code blocks` maintain formatting for technical content

[Links](url) show relationships between documents
```

### Contrast with Other Formats

- **Rich text (Word, Google Docs)**: Proprietary, formatting-heavy, not text-based
- **PDF**: Not easily editable, formatting obscures structure
- **HTML**: Too much markup noise, not human-friendly to write
- **Plain text**: No structure signals for the LLM to parse
- **Markdown**: Goldilocks zone — structure without noise

## Related Concepts

- [LLM Wiki Pattern](wiki/concepts/llm-wiki-pattern.md) — uses markdown as foundation
- [Query vs. Browse Interface](wiki/concepts/query-vs-browse.md)

## Related Entities

- [Claude Code](wiki/entities/claude-code.md) — reads markdown natively
- [Obsidian](wiki/entities/obsidian.md) — markdown editor for LLM wikis

## Contradictions / Open Questions

- **Scale question**: At what point do you need semantic embeddings beyond plain markdown? Source suggests "a few hundred notes" is the threshold, but this may vary.
- **Image handling**: Markdown references images but LLMs can't read inline images in one pass — workaround needed for visual content.

## References

- [2026-04-06-karpathy-llm-wiki-mindstudio.md](wiki/sources/2026-04-06-karpathy-llm-wiki-mindstudio.md)
