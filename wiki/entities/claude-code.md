---
type: entity
category: tool
created: 2026-05-02
updated: 2026-05-02
sources: [2026-04-06-karpathy-llm-wiki-mindstudio.md]
---

# Claude Code

## Overview

Anthropic's terminal-based coding agent that runs in local environments with direct filesystem access. Key tool for implementing the LLM Wiki pattern.

## Key Information

- **Type**: Terminal-based AI coding agent
- **Developer**: Anthropic
- **Installation**: `npm install -g @anthropic-ai/claude-code`
- **Key capabilities**:
  - Direct filesystem access (reads/writes local files)
  - Search across files for relevant content
  - Create new files and update existing ones
  - Execute shell commands for searching, filtering, organizing
  - Natural language interaction
- **Model**: Uses Claude 3.5 Sonnet with ~200k token context window
- **Context capacity**: Can read tens of thousands of words in a single session

## Key Advantages for LLM Wiki

Unlike browser-based Claude, Claude Code:
- Works directly with local files (no copy-pasting)
- Can traverse directory structures
- Can execute commands to manipulate the knowledge base
- Maintains session context across file operations

## Related Concepts

- [LLM Wiki Pattern](wiki/concepts/llm-wiki-pattern.md) — primary use case
- [Query vs. Browse Interface](wiki/concepts/query-vs-browse.md)
- [Knowledge Base Maintenance](wiki/concepts/knowledge-base-maintenance.md)

## Mentions

- [Source: MindStudio article (2026-04-06)](wiki/sources/2026-04-06-karpathy-llm-wiki-mindstudio.md) — Describes Claude Code as the query interface for LLM wikis

## References

- [2026-04-06-karpathy-llm-wiki-mindstudio.md](wiki/sources/2026-04-06-karpathy-llm-wiki-mindstudio.md)
