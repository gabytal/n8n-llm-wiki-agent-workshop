---
name: query
description: Search wiki and synthesize answers from your compiled knowledge base
---

# Query: Search and Answer from Wiki

## What This Does
Searches your wiki for relevant information and synthesizes answers with citations. Reads compiled knowledge, doesn't re-process sources.

## Workflow

### 1. Understand the Question
- Identify key concepts, entities, or topics
- Determine if factual lookup or synthesis needed
- Check if question requires multiple pages

### 2. Search Index
```bash
# Read the catalog
cat wiki/index.md

# Look for relevant entries
grep -i "keyword" wiki/index.md
```

### 3. Read Relevant Pages
- Start with pages matching keywords
- Follow `[[wikilinks]]` to connected pages
- Read 3-5 most relevant pages max

### 4. Synthesize Answer
- Combine information from multiple pages
- Use actual content, don't invent
- Cite sources: `[concept-name.md]` or `[[concept-name]]`
- If information missing, say "Not in wiki"

### 5. Structure Response
```markdown
[Direct answer in 2-3 sentences]

**Sources:**
- [[entity-name]] - [relevant detail]
- [[concept-name]] - [relevant detail]

**Related:**
- Consider also: [[related-topic]]
```

## Key Rules
- ONLY use information from wiki/ pages
- NEVER hallucinate or add external knowledge
- ALWAYS cite source pages
- If uncertain, say "Not clearly documented in wiki"
- Suggest adding sources if gaps found

## Example Queries

**Q:** "What are the main concepts about AI agents?"
```
Based on your wiki, AI agents involve three key concepts:

1. **Agentic RAG** - Systems that plan retrieval strategies 
   rather than simple vector search [[agentic-rag]]

2. **Tool Calling** - Agents using external APIs to take 
   actions [[tool-calling]]

3. **Human-in-Loop** - Review workflows before execution 
   [[hitl-patterns]]

**Sources:**
- [[agentic-rag]] - Covers planning and multi-step retrieval
- [[tool-calling]] - Explains function calling patterns
- [[hitl-patterns]] - Safety and review mechanisms

**Related:** See also [[rag-evolution]] for historical context
```

**Q:** "How are LangChain and CrewAI different?"
```
Your wiki compares them:

**LangChain** [[langchain]]:
- General-purpose framework
- LCEL for composition
- Good for single-agent workflows

**CrewAI** [[crewai]]:
- Multi-agent specialization
- Role-based agents
- Good for team coordination

**Source:** [[frameworks-comparison]] - Side-by-side analysis

**Gap:** No information on performance or pricing differences
```

**Q:** "What's the best way to debug agents?"
```
Not clearly documented in your wiki.

Found mentions in:
- [[observability]] - Logging importance mentioned
- [[production-patterns]] - Testing strategies

**Suggestion:** Add a source about agent debugging best 
practices to fill this gap.
```

## When to Save Answers

If synthesis is valuable, suggest saving:
```
"This answer synthesizes [A] and [B] - should I save it as 
wiki/concepts/agent-debugging.md for future reference?"
```

This makes queries improve your wiki over time.

## Usage
```bash
# Interactive
"query: what are the main RAG approaches?"
"/query what did I learn about MCP?"

# Context-aware
"Based on my wiki, how should I build this feature?"
"What does my knowledge base say about X vs Y?"

# Gap finding
"What questions can't I answer from my wiki yet?"
```

## Performance Tips

**For large wikis (50+ pages):**
1. Search index.md first (titles only)
2. Read only top 3-5 matches
3. Use `grep` to pre-filter: `grep -l "keyword" wiki/**/*.md`

**For speed:**
- Keep index.md under 500 lines
- Use page summaries in index
- Read full pages only when needed