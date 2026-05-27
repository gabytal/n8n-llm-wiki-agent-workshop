---
name: ingest
description: Compile raw sources into structured wiki pages following Karpathy's LLM wiki pattern
---

# Ingest: Transform Raw Sources into Wiki Knowledge

## What This Does
Reads files from `raw/` folder and creates structured, cross-linked wiki pages. Like compiling source code into a binary - process once, query forever.

## Workflow

### 1. Discover Structure
```bash
ls raw/        # What sources to process
ls wiki/       # What categories exist (dynamic!)
```

### 2. Process Each Source
- Read file content (markdown, PDF, text)
- Extract: entities (people, tools, orgs), concepts (ideas, methods), relationships
- Classify into wiki categories based on folder structure

### 3. Create Wiki Pages
- **Entity pages** (`wiki/entities/tool-name.md`):
  - What it is
  - Key capabilities  
  - When to use
  - Related entities

- **Concept pages** (`wiki/concepts/idea-name.md`):
  - Definition
  - Context and use cases
  - Related concepts
  - Source citations

- **Source summaries** (`wiki/sources/YYYY-MM-DD-source-name.md`):
  - Link to original
  - Key takeaways (3-5 bullets)
  - Which pages updated

### 4. Add Cross-Links
Use `[[page-name]]` syntax to connect related pages

### 5. Update Index
Add entries to `wiki/index.md`:
```markdown
## Entities
- **[Tool Name](wiki/entities/tool-name.md)** — One-line description

## Concepts
- **[Concept](wiki/concepts/concept.md)** — One-line description
```

### 6. Log Changes
Append to `wiki/log.md`:
```markdown
## YYYY-MM-DD | HH:MM UTC
### Operation: Ingest [Source Name]
**Source:** `raw/filename.ext`
**Pages Created:** [list]
**Pages Updated:** [list]
**Summary:** [1-2 sentences]
```

### 7. Verify
```bash
git diff wiki/  # Show what changed
git add wiki/ && git commit -m "Ingest: [source-name]"
```

## Key Rules
- NEVER edit files in `raw/` - they are ground truth
- ALWAYS use `[[wikilinks]]` for cross-references
- ASK before creating new top-level categories
- PRESERVE existing wiki structure
- UPDATE don't replace - enhance pages with new information

## Example Flow
```
raw/ai-agents-paper.pdf
  ↓ (ingest)
wiki/concepts/agentic-rag.md (created)
wiki/concepts/tool-calling.md (updated)
wiki/entities/langchain.md (created)
wiki/sources/2026-05-25-ai-agents-paper.md (created)
wiki/index.md (2 new entries)
wiki/log.md (operation logged)
```

## Usage
```bash
# Manual
"Ingest the file at raw/article.md"

# Bulk
"Ingest all files in raw/"

# Targeted
"Ingest raw/docs/ and focus on technical concepts"
```