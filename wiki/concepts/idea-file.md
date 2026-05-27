---
type: concept
created: 2026-05-02
updated: 2026-05-02
sources: [2026-04-20-karpathy-llm-wiki-medium-urvil.md]
---

# Idea File

## Definition

A document that describes a pattern or system at a conceptual level, designed to be copy-pasted into an LLM agent which then instantiates the pattern for the specific user's needs. Not code or a rigid specification, but a flexible template for the LLM to interpret and adapt.

## Key Principles

1. **Conceptual, not prescriptive**: Describes what, not exactly how
2. **Copy-paste interface**: Meant to be fed directly to LLM agent
3. **LLM interprets**: Agent asks clarifying questions and adapts pattern
4. **Flexible instantiation**: Same idea file → different implementations per user
5. **Pattern as artifact**: Unit of sharing is the pattern itself, not code

## How It Works

### Traditional Pattern Sharing

**Code/libraries**: Rigid implementation, use as-is  
**Documentation**: Describes pattern, user implements manually  
**Templates**: Fill-in-the-blanks structure  

**Problem**: Too rigid (code) or too much work (docs, templates)

### Idea File Approach

1. **Author writes** conceptual description of pattern
2. **User copy-pastes** entire idea file into LLM agent chat
3. **LLM reads** and understands pattern at abstract level
4. **LLM asks** clarifying questions about user's specific context
5. **LLM instantiates** pattern tailored to user's needs
6. **User and LLM iterate** to refine implementation

### Karpathy's llm-wiki.md as Exemplar

> "GitHub gist called llm-wiki.md. The idea isn't a product. It's not code. It's a *pattern*"

**What the file contains**:
- Abstract description of LLM Wiki concept
- Three-layer architecture explanation
- General workflows (ingest, query, lint)
- Design principles and philosophy
- Intentionally abstract — not specific directory structures

**How it's used**:
Urvil Joshi's tutorial demonstrates:

**Step 1**: Paste full llm-wiki.md gist into Claude Code  
**Step 2**: Prompt like: *"Help me set up an LLM Wiki. Ask me what it will be about."*  
**Step 3**: Claude asks: What topic? What sources? How many? What page types?  
**Step 4**: User answers  
**Step 5**: Claude writes tailored CLAUDE.md schema, creates directory structure  

Result: Same idea file → personalized wiki for AI research, personal health, book reading, etc.

## Examples

### Idea File Structure (Typical)

```markdown
# Pattern Name

## The Core Idea
[Conceptual description in plain language]

## Architecture
[High-level components and relationships]

## Operations
[Key workflows and processes]

## Tips and Tricks
[Optional enhancements, recommendations]

## Note
"This document is intentionally abstract. The exact [details] will
depend on your domain, your preferences, and your LLM of choice."
```

### Contrast with Code

**Code**: `npm install llm-wiki && llm-wiki init`  
**Idea file**: Paste into Claude, discuss your needs, Claude writes custom setup

**Code**: One implementation fits all  
**Idea file**: LLM customizes to your context

**Code**: Versioned, dependencies, breaking changes  
**Idea file**: Timeless pattern, LLM adapts to current tech

### Other Potential Idea Files

**Not yet widespread, but pattern enables**:

- **Project structure patterns** (monorepo setups, microservices architectures)
- **Writing workflows** (Zettelkasten with AI, progressive summarization)
- **Learning systems** (spaced repetition with LLM, concept maps)
- **Code review processes** (what to check, how to give feedback)
- **Meeting protocols** (agenda structures, note-taking conventions)

Any pattern where flexibility matters more than rigid execution.

## Advantages

**For pattern authors**:
- Don't need to write code for every use case
- Pattern expressed at right abstraction level
- LLM handles adaptation complexity
- Can update by editing description, not refactoring code

**For pattern users**:
- No installation, dependencies, version conflicts
- Customized to specific needs automatically
- Can iterate with LLM in natural language
- Lower barrier to experimentation

**For the ecosystem**:
- Patterns shareable as simple markdown
- Works with any LLM agent that can read files
- Composable (multiple idea files can combine)
- Language/framework agnostic

## Limitations

**Requires capable LLM**:
- Not all LLMs can interpret and instantiate well
- Quality depends on LLM's instruction-following

**Ambiguity**:
- Idea files are intentionally vague
- Bad LLM might instantiate poorly
- User must guide with clarifying answers

**Not a substitute for code**:
- Complex algorithms still need implementation
- Idea files for patterns and structures, not logic

**Discovery**:
- No central repository (yet)
- Idea files scattered in gists, blog posts
- Harder to find than npm packages

## Related Concepts

- [LLM Wiki Pattern](wiki/concepts/llm-wiki-pattern.md) — the pattern shared as idea file
- [Knowledge Compilation](wiki/concepts/knowledge-compilation.md)

## Related Entities

- [Andrej Karpathy](wiki/entities/andrej-karpathy.md) — popularized approach with llm-wiki.md gist
- [Claude Code](wiki/entities/claude-code.md) — agent that interprets idea files

## Contradictions / Open Questions

- **Standardization**: Should idea files have a standard format? Or stay flexible?
- **Discovery**: How to make idea files findable? GitHub topic tags? Dedicated registry?
- **Versioning**: If pattern evolves, how to version idea files? Or are they meant to be timeless?
- **Validation**: How to know if an LLM correctly instantiated the pattern? Test suites? Examples?
- **Ownership**: Who maintains idea files? Original authors? Community?

## References

- [2026-04-20-karpathy-llm-wiki-medium-urvil.md](wiki/sources/2026-04-20-karpathy-llm-wiki-medium-urvil.md)
