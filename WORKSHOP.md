# 🧠 LLM Wiki Chatbot Workshop

Build a chatbot that answers questions using **your own knowledge base** with n8n and the LLM wiki pattern.

## What You'll Build

```
Your docs → Wiki structure → n8n workflow → Chatbot API
   📄           🗂️              🔄            🤖
```

**Soon you'll have:**
- A structured wiki built from your documents (Karpathy's pattern)
- An n8n workflow that queries your wiki
- A working chatbot API you can query

## Quick Start

### Prerequisites
- Docker & Docker Compose installed
- Anthropic API key ([get one here](https://console.anthropic.com/settings/keys))
- Basic command line knowledge

### Setup (5 minutes)

```bash
# 1. Clone this repo
git clone <your-repo>
cd n8n-ollama

# 2. Add your API key
cp .env.example .env
# Edit .env and add: ANTHROPIC_API_KEY=your-key-here

# 3. Start everything
docker compose up -d

# 4. Wait for setup (~2 minutes)
docker compose logs -f n8n-init
# Wait for: "✅ Setup complete!"

# 5. Open n8n
open http://localhost:5678
# Login: admin@localhost.local / Admin123
```

## Workshop Steps

### Step 1: Create Your Wiki Structure

```bash
# Create the wiki folders
mkdir -p my-wiki/{raw,wiki}
cd my-wiki

# Create category folders (you choose!)
mkdir -p wiki/concepts wiki/entities wiki/sources

# Or create your own categories:
# mkdir -p wiki/products wiki/competitors wiki/research
```

**✅ Checkpoint:** You have `raw/` and `wiki/` folders with categories inside

---

### Step 2: Add Your Source Documents

Add 2-3 documents to `raw/`:

```bash
# Copy your documents
cp ~/Documents/article1.md raw/
cp ~/Documents/notes.md raw/
cp ~/Documents/research.pdf raw/
```

**Pro tip:** Start small! 3 sources is perfect for learning.

**✅ Checkpoint:** `raw/` folder has your content

---

### Step 3: Create Ingestion Skill

Create `.claude/skills/ingest.md`:

```markdown
---
name: ingest
description: Compile raw sources into structured wiki pages
---

# Ingest: Transform Raw Sources into Wiki

Read files in raw/ folder
List wiki/ subdirectories to discover available categories
Extract key entities, concepts, and ideas from sources
Classify content into appropriate categories
Create markdown pages with cross-links using [[wikilinks]]
Update wiki/index.md with new entries (title + one-line description)
Append entry to wiki/log.md with timestamp and summary

Example: `raw/ai-agents.pdf` creates:
- `wiki/concepts/agentic-rag.md`
- `wiki/entities/langchain.md`
- Updates `wiki/index.md` and `wiki/log.md`
```

**✅ Checkpoint:** Skill file created

---

### Step 4: Run Ingestion

Use Claude Code to ingest your documents:

```bash
# In your wiki directory
claude

# Then in Claude:
/ingest
# Or: "Ingest the content in raw/"
```

Claude will:
- Read your source files
- Create wiki pages in the right categories
- Add cross-links between related pages
- Update index.md and log.md

**✅ Checkpoint:** Wiki pages generated in `wiki/concepts/`, `wiki/entities/`, etc.

---

### Step 5: Create Query Skill

Create `.claude/skills/query.md`:

```markdown
---
name: query
description: Search wiki and answer questions from your knowledge
---

# Query: Search and Synthesize

Search wiki/index.md for relevant page titles
Read those specific pages from wiki/
Synthesize answer combining multiple sources
Cite sources using [[page-name]] format
Return structured response with citations

If answer not in wiki, say: "I don't know - not in knowledge base"
```

**Test it:**
```bash
# In Claude Code
/query what did I learn about AI agents?
```

**✅ Checkpoint:** Query returns answers from YOUR wiki

---

### Step 6: Build n8n Workflow

In n8n (http://localhost:5678), create this workflow:

**Nodes:**
1. **Webhook** (Trigger)
   - Method: POST
   - Path: `wiki`
   - Accept: `{ "query": "your question" }`

2. **Read Files** (Read/Write Files from Disk)
   - Operation: Read files from disk
   - File Path: `/wiki/wiki/**/*.md`
   - Binary Property: `data`

3. **Convert to Text** (Code node)
```javascript
const items = $input.all();
const wikiContent = items.map(item => {
  const content = Buffer.from(item.binary.data.data, 'base64').toString('utf-8');
  return `File: ${item.binary.data.fileName}\n${content}\n---\n`;
}).join('\n');

return [{ json: { wiki: wikiContent, query: $('Webhook').item.json.query }}];
```

4. **AI Agent** (HTTP Request to Claude)
   - Method: POST
   - URL: `https://api.anthropic.com/v1/messages`
   - Headers:
     - `x-api-key`: `{{ $env.ANTHROPIC_API_KEY }}`
     - `anthropic-version`: `2023-06-01`
     - `content-type`: `application/json`
   - Body:
```json
{
  "model": "claude-3-5-sonnet-20241022",
  "max_tokens": 1024,
  "system": "You are a knowledge base assistant. Answer questions ONLY using the wiki content provided. Cite sources using [filename.md]. If not in wiki, say 'I don't know'.",
  "messages": [{
    "role": "user",
    "content": "Wiki:\n{{ $json.wiki }}\n\nQuestion: {{ $json.query }}"
  }]
}
```

5. **Format Response** (Code node)
```javascript
const response = $input.item.json.content[0].text;
return [{ json: { answer: response, sources: "From wiki files" }}];
```

6. **Respond to Webhook**
   - Response: `{{ $json }}`

**✅ Checkpoint:** Workflow saves successfully

---

### Step 7: Test Your Chatbot

```bash
curl -X POST http://localhost:5678/webhook/wiki \
  -H "Content-Type: application/json" \
  -d '{
    "query": "What did I learn about AI agents?"
  }'
```

**Expected response:**
```json
{
  "answer": "Based on your wiki, AI agents are...",
  "sources": "From wiki files"
}
```

**✅ Done! Your knowledge chatbot is live!**

---

## Understanding the Pattern

### Why This Works (Karpathy's Insight)

**Traditional RAG:**
- Query time: Embed query → Search vectors → Retrieve → Generate
- Starts from scratch every time
- No accumulated knowledge structure

**LLM Wiki:**
- Ingest time: Process sources → Create structured pages → Cross-link
- **Compile once, query forever**
- Knowledge accumulates like compiled code

### The Three Operations

1. **INGEST** - Compile raw sources into wiki (do this when adding new docs)
2. **QUERY** - Search and synthesize from wiki (do this constantly)
3. **LINT** - Audit and maintain wiki quality (do this periodically)

### Folder Structure

```
my-wiki/
├── raw/                     # Your source documents (never edit!)
│   ├── article1.md
│   └── notes.pdf
├── wiki/                    # LLM-maintained knowledge base
│   ├── concepts/            # Big ideas
│   │   └── agentic-rag.md
│   ├── entities/            # Tools, people, organizations
│   │   └── langchain.md
│   ├── sources/             # Source metadata
│   │   └── article1-summary.md
│   ├── index.md             # Catalog of all pages
│   └── log.md               # Change history
└── .claude/
    └── skills/
        ├── ingest.md        # Compile sources
        └── query.md         # Search wiki
```

## Troubleshooting

### API Key Issues
```bash
# Test your API key
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{"model":"claude-3-5-sonnet-20241022","max_tokens":10,"messages":[{"role":"user","content":"test"}]}'

# Should return JSON with "content" field
```

### n8n Can't Read Files
```bash
# Check file permissions
ls -la my-wiki/wiki/

# Fix permissions
chmod -R 755 my-wiki/wiki/

# Mount wiki folder in docker-compose.yml
volumes:
  - ./my-wiki/wiki:/wiki:ro
```

### Workflow Fails
1. Click the failing node
2. Click "Execute Node" (not "Execute Workflow")
3. Read the error in the output panel
4. Common issues:
   - Missing API key in environment
   - Wrong file paths (use `/wiki/` not `./wiki/`)
   - Binary data not converted to text

### Wiki Too Large (Context Overflow)
```bash
# Check wiki size
ls wiki/**/*.md | wc -l   # Number of pages
wc -c wiki/index.md       # Index size

# If >50 pages, load selectively:
# 1. Search index.md for relevant pages
# 2. Load only those 3-5 pages
# 3. Pass to AI agent
```

## Next Steps

After completing the workshop:

### 1. Add Obsidian for Visualization
```bash
# Open wiki/ in Obsidian
# See graph view of your knowledge
# Use web clipper to add new sources
```

### 2. Scale with qmd
```bash
npm install -g qmd
cd my-wiki/wiki
qmd init
# Now: qmd search "query" finds pages fast
```

### 3. Connect to Slack/Discord
- Use n8n's Slack/Discord nodes
- Trigger workflow from messages
- Bot answers from YOUR knowledge

### 4. Add More Categories
```bash
mkdir wiki/comparisons  # Side-by-side analysis
mkdir wiki/guides       # Step-by-step procedures
# Ingest skill adapts automatically!
```

### 5. Automate Ingestion
- Watch `raw/` folder for new files
- Auto-trigger ingestion on file add
- Get email summaries of new knowledge

## Resources

- [Karpathy's LLM Wiki Pattern](https://twitter.com/karpathy) - Original explanation
- [n8n Documentation](https://docs.n8n.io/) - Workflow automation
- [Claude API Docs](https://docs.anthropic.com/) - API reference
- [Obsidian](https://obsidian.md/) - Wiki visualization

## Philosophy

> **"What if knowledge compounded like code?"**
> 
> RAG systems re-process everything at query time. LLM wikis compile once and query forever. Your wiki gets smarter with every source you add, and past knowledge enriches future understanding.

---

**Built with:** n8n + Claude + Karpathy's LLM Wiki Pattern
**License:** MIT - Free to use and modify