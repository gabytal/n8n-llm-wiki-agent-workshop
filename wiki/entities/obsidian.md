---
type: entity
category: tool
created: 2026-05-02
updated: 2026-05-02
sources: [2026-04-06-karpathy-llm-wiki-mindstudio.md, 2026-04-07-llm-wiki-revolution-analyticsvidhya.md]
---

# Obsidian

## Overview

Local-first markdown editor recommended as the front-end for LLM Wiki systems. Provides graph view, backlinks, and plugin ecosystem while keeping all data as plain markdown files on disk.

## Key Information

- **Type**: Markdown editor and knowledge management tool ("Wiki IDE")
- **Website**: obsidian.md
- **Philosophy**: Local-first, file-based (no proprietary database)
- **Vault concept**: A "vault" is simply a folder of markdown files
- **Key features for LLM Wiki**:
  - **Graph view**: Visualizes all wiki pages as nodes, wiki links as edges. Hub pages appear larger, orphan pages isolated. Provides immediate visual representation of knowledge density and gaps.
  - **Backlinks**: See what links to each note
  - **`[[wiki links]]` syntax**: Internal linking between notes
  - **Dataview plugin**: Turn wiki into queryable database using SQL-like queries against YAML frontmatter
  - **Web Clipper**: Browser extension (Chrome, Firefox, Safari, Edge, Arc, Brave) converts web articles to clean Markdown with YAML frontmatter in one click, saves directly to raw folder
  - **Plugin ecosystem**: Extensible architecture
  - **Clean interface**: Reading/writing markdown
- **Data ownership**: All files remain local as `.md` files; Obsidian is just the viewer/editor
- **Image handling**: Hotkey Ctrl+Shift+D downloads all images from clipped article to local disk

## Recommended Setup for LLM Wiki

**Folder structure**:
```
wiki/
├── _templates/
├── projects/
├── research/
├── reference/
├── meetings/
└── inbox/
```

**Note template** with:
- Title
- One-sentence summary
- Tags
- Created/updated timestamps
- Content section
- Related notes section

## Related Concepts

- [LLM Wiki Pattern](wiki/concepts/llm-wiki-pattern.md)
- [Markdown as LLM-Native Format](wiki/concepts/markdown-llm-native.md)

## Mentions

- [Source: MindStudio article (2026-04-06)](wiki/sources/2026-04-06-karpathy-llm-wiki-mindstudio.md) — Recommended as the front-end tool for managing LLM wikis
- [Source: Analytics Vidhya article (2026-04-07)](wiki/sources/2026-04-07-llm-wiki-revolution-analyticsvidhya.md) — Details Graph View, Dataview plugin, Web Clipper with code examples

## References

- [2026-04-06-karpathy-llm-wiki-mindstudio.md](wiki/sources/2026-04-06-karpathy-llm-wiki-mindstudio.md)
- [2026-04-07-llm-wiki-revolution-analyticsvidhya.md](wiki/sources/2026-04-07-llm-wiki-revolution-analyticsvidhya.md)
