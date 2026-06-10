# Workshop: Build an LLM Wiki Agent

Build a chatbot that answers questions from YOUR knowledge base - with citations, grounded answers, and zero hallucinations.

**Time:** 60 minutes
**Pattern:** Karpathy's "compile once, query forever"

## The Complete Flow

```
📄 Raw docs → 🤖 Claude Code ingest → 📚 Wiki → 📤 Git push → 🔄 n8n sync → 💬 Chatbot
```

## What You're Building

1. **Local knowledge compilation** - Claude Code reads your docs and creates a structured wiki
2. **Git-based storage** - Wiki lives in GitHub (or any git repo)
3. **n8n chatbot** - Queries the wiki via webhook, returns cited answers

## Prerequisites

- Docker + Docker Compose
- [Claude Code](https://claude.ai/code) installed
- An account with any LLM provider supported by n8n (Anthropic, OpenAI, Google Gemini, Ollama, etc.) — you configure the credential in the n8n UI, no API key is needed at startup
- Basic familiarity with git and terminal

---

## Part 1: See It Working (10 min)

### 1.1 Clone & Start

```bash
git clone https://github.com/gabytal/n8n-llm-wiki-agent-workshop
cd n8n-llm-wiki-agent-workshop
docker compose up -d
```

Wait ~2 minutes for setup. Watch for: `✅ Setup complete!`

### 1.2 Configure n8n

1. Open http://localhost:5678
2. Login: `admin@localhost.local` / `Admin123`
3. Open workflow **Wiki AI Agent (Git)**
4. Click the **chat model** node (ships as an Anthropic node by default) and set up your LLM provider:
   - **Anthropic:** create/select an **Anthropic** credential, then pick a model (e.g. `Claude Haiku 4.5`)
   - **Another provider:** delete the node, add the chat model node for your provider (*OpenAI Chat Model*, *Ollama Chat Model*, *Google Gemini Chat Model*, …), connect its **ai_languageModel** output to the **AI Agent** node, add your credential, and select a model
5. **Activate** the workflow (toggle in top-right)

### 1.3 Test the Demo Bot

```bash
curl -X POST http://localhost:5678/webhook/wiki-agent \
  -H "Content-Type: application/json" \
  -d '{"chatInput": "What is the LLM Wiki pattern?"}'
```

**Expected:** Cited answer like:
```json
{
  "answer": "The LLM Wiki pattern is knowledge management using markdown files... [concepts/llm-wiki-pattern.md]"
}
```

**Try an off-topic question:**
```bash
curl -X POST http://localhost:5678/webhook/wiki-agent \
  -d '{"chatInput": "What is the weather in Tokyo?"}'
```

**Expected:** `"I don't know - not in knowledge base"`

---

## Part 2: Ingest YOUR Knowledge (35 min)

Now use Claude Code to compile YOUR documents into wiki pages.

### 2.1 Add Raw Sources

Create a `raw/` folder and add your documents:

```bash
mkdir raw
cp ~/your-article.md raw/
cp ~/your-notes.txt raw/
cp ~/your-doc.pdf raw/
```

**Tip:** Start with 2-3 documents to see the pattern. Any format works (markdown, PDF, text, code).

### 2.2 Run Ingestion Skill

Open Claude Code in this directory and run the ingestion:

```bash
# In Claude Code terminal
/ingest
```

**What happens:**
- Claude reads all files in `raw/`
- Extracts entities (people, tools, products)
- Extracts concepts (ideas, methods, patterns)
- Creates structured wiki pages with cross-links
- **Automatically updates `index.html`** with new pages and descriptions
- Logs changes to `wiki/log.md`

**Folder structure after ingestion:**
```
wiki/
├── concepts/        # Ideas, methods, patterns
├── entities/        # People, tools, organizations
├── sources/         # Source summaries (dated)
├── analysis/        # Meta-analysis (optional)
└── index.md         # Page catalog (auto-updated by skill)
```

### 2.3 Review Generated Pages

Check what was created:

```bash
git status
git diff wiki/
```

**Example output:**
```
wiki/concepts/your-concept.md (created)
wiki/entities/your-tool.md (created)
wiki/sources/2026-05-31-your-article.md (created)
index.html (updated - new entries added automatically!)
```

**Important:** The ingestion skill automatically updates `index.html` with:
- New page entries
- One-line descriptions
- Updated page counts

Review the changes to make sure descriptions are clear.

### 2.4 Commit & Push

```bash
git add wiki/ index.html raw/
git commit -m "Ingest: [your domain] knowledge"
git push
```

**Note:** `index.html` was auto-updated by the ingestion skill!

---

## Part 3: Query Your Knowledge (10 min)

### 3.1 Test Your New Content

```bash
curl -X POST http://localhost:5678/webhook/wiki-agent \
  -H "Content-Type: application/json" \
  -d '{"chatInput": "What did I learn about [your topic]?"}'
```

**Check:**
- Answer includes your content
- Citations point to your wiki pages: `[concepts/your-concept.md]`
- n8n Executions tab shows which files were read

### 3.2 Try Complex Queries

**Multi-page synthesis:**
```bash
curl -X POST http://localhost:5678/webhook/wiki-agent \
  -d '{"chatInput": "How does [concept A] relate to [concept B]?"}'
```

**Entity lookup:**
```bash
curl -X POST http://localhost:5678/webhook/wiki-agent \
  -d '{"chatInput": "What is [your tool] and when should I use it?"}'
```

**Should refuse:**
```bash
curl -X POST http://localhost:5678/webhook/wiki-agent \
  -d '{"chatInput": "What is machine learning?"}'
# → "I don't know - not in knowledge base" (unless you ingested ML content)
```

---

## How It Works

### Architecture

```
Webhook → Sync Repo (git pull) → AI Agent ⇄ Tools → Respond
```

### 5 n8n Nodes

1. **Webhook** - Entry point (`POST /webhook/wiki-agent`)
2. **Sync Wiki Repo** - Clones/pulls from GitHub, loads `index.html`
3. **AI Agent** - Claude Haiku 4.5 with strict system prompt
4. **Tool: list_wiki_files** - Fallback file listing
5. **Tool: read_wiki_page** - Reads specific page content
6. **Respond** - Returns JSON with cited answer

### The Index Strategy

Instead of blindly calling `list_wiki_files` and guessing from filenames, the agent gets `index.html` injected into its system prompt:

```
--- WIKI INDEX ---
## Concepts
- [LLM Wiki Pattern](wiki/concepts/llm-wiki-pattern.md) — Compile once, query forever
- [RAG vs LLM Wiki](wiki/concepts/rag-vs-llm-wiki.md) — Query-time vs ingestion-time processing
...
--- END INDEX ---
```

Now the agent can jump straight to the right file in one shot.

### The Ingestion Pattern

**Local (Claude Code):**
- Reads `raw/` sources
- Classifies content (concept, entity, source)
- Generates wiki pages with cross-links
- Updates `wiki/index.md`

**Remote (n8n):**
- Pulls latest wiki from GitHub
- Reads `index.html` for page descriptions
- Uses `read_wiki_page` tool to load content
- Refuses to answer outside the knowledge base

---

## Claude Code Skills

The repo includes two skills in `.claude/skills/`:

### ingest.md
**Purpose:** Transform raw sources into structured wiki pages

**Usage:**
```bash
/ingest                           # Process all files in raw/
/ingest raw/article.md            # Process one file
/ingest raw/docs/ focus:concepts  # Targeted ingestion
```

**Creates:**
- Entity pages: `wiki/entities/tool-name.md`
- Concept pages: `wiki/concepts/idea-name.md`
- Source summaries: `wiki/sources/YYYY-MM-DD-source-name.md`
- Cross-links with `[[wikilinks]]`
- **Automatically updates `index.html`** with new entries and descriptions

### query.md
**Purpose:** Search and answer from your compiled wiki (for local use)

**Usage:**
```bash
/query what are the main concepts about X?
/query how does A differ from B?
/query what did I learn about [topic]?
```

**Returns:** Cited answer with `[[wikilinks]]` and source references

---

## Extending the Knowledge Base

### Add More Content

1. Drop new files into `raw/`
2. Run `/ingest` in Claude Code
3. Update `index.html` with new page descriptions
4. Commit and push
5. Next webhook call auto-pulls the update

### No re-deploy needed - the agent does `git pull` on every request.

---

## Troubleshooting

**Webhook returns 404**
- Workflow not active → Toggle the switch in workflow editor

**Agent says "I don't know" for everything**
- Check n8n Executions tab for errors
- Verify `index.html` has your pages listed
- Confirm your LLM provider credential is configured and a model is selected on the chat model node

**Ingestion skill errors**
- Ensure you're in the repo directory
- Check `raw/` folder exists and has files
- Try `/ingest raw/single-file.md` first

**Pages not in index.html**
- The agent falls back to `list_wiki_files` (sees filenames only)
- Without descriptions, it can't reason about relevance as well
- Always update `index.html` after ingestion

---

## Workshop Complete! 🎉

**You now have:**
- ✅ Raw docs → wiki compilation (Claude Code)
- ✅ Wiki → GitHub storage (git push)
- ✅ Chatbot queries GitHub wiki (n8n)

**The pattern:** Compile once, query forever

---

## Homework: Upgrade to Slackbot

**Challenge:** Turn your HTTP webhook into a Slack bot

**Current:**
```bash
curl → webhook → JSON response
```

**Goal:**
```
@bot what is X? → threaded reply with citations
```

**Hints:**
1. Create Slack app at api.slack.com/apps
2. Use n8n's **Slack Trigger** node (app mention event)
3. Connect to existing **AI Agent** workflow
4. Use **Slack → Send Message** node for replies
5. Bonus: Add reactions (✅ for answered, ❓ for "not in KB")

**Estimated time:** 30-45 minutes

**Guide:** See `HOMEWORK-SLACK.md` (coming soon)

---

## What's Next

- **Obsidian integration** - Visualize your wiki as a graph
- **qmd search** - Scale to 100+ pages with local hybrid search
- **Multiple repos** - Query across several knowledge bases
- **Lint workflow** - Add periodic wiki validation

**Full architecture guide:** See `README.md`

---

## Pattern Comparison

| | RAG (vectors) | LLM Wiki |
|---|---|---|
| Storage | Embeddings DB | Markdown in git |
| Retrieval | Similarity search | Agent navigates files |
| Updates | Re-embed pipeline | `git push` |
| Citations | Chunk IDs | Filenames |
| Debug | Hard | Read the file |

**More details:** See [`wiki/concepts/rag-vs-llm-wiki.md`](wiki/concepts/rag-vs-llm-wiki.md)

---

Have fun! 🚀