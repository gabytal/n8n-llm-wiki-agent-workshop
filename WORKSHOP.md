# Workshop: Build an LLM Wiki Agent

Build a chatbot that answers questions from a markdown knowledge base, with citations, in ~90 minutes.

You'll learn:
- Karpathy's "compile once, query forever" knowledge pattern
- How to wire an n8n AI Agent to custom JS tools
- How to ground LLM answers in your own docs (no hallucinations)

## What You're Building

A webhook that takes a question and returns a cited answer:

```bash
curl -X POST http://localhost:5678/webhook/wiki-agent \
  -d '{"chatInput": "What is the LLM Wiki pattern?"}'
# → {"answer": "The LLM Wiki pattern is... [concepts/llm-wiki-pattern.md]"}
```

Behind the scenes:

```
Webhook → Sync Wiki Repo (git clone/pull + load index.html) → AI Agent ⇄ Tools → Respond
```

The agent gets a **curated index** (with one-line descriptions of every page) injected into its system prompt, plus two tools it can call:
- `read_wiki_page` — load a specific file's content
- `list_wiki_files` — fallback file listing if the index misses something

> **Why both an index and a list tool?** The index (`index.html`) is hand-curated — it tells the LLM what each page is *about*, not just its filename. That lets the model pick the right page in one shot instead of trial-reading several. The `list_wiki_files` tool is a safety net for content not yet in the index.

## Prerequisites

- Docker + Docker Compose
- An [Anthropic API key](https://console.anthropic.com/)
- Basic familiarity with JSON & HTTP

## Step 1 — Spin Up n8n

```bash
git clone https://github.com/gabytal/n8n-llm-wiki-agent-workshop.git
cd n8n-llm-wiki-agent-workshop

docker compose up -d
docker compose logs -f n8n-init
```

Wait for `✅ Setup complete!` (~2 min). Open http://localhost:5678 and log in:
- `admin@localhost.local` / `Admin123`

## Step 2 — Add Your Anthropic Credential

The workflow ships referencing a credential by name; you need to create it once.

1. **Settings → Credentials → New** → search "Anthropic"
2. Paste your API key, save
3. Open the **Wiki AI Agent (Git)** workflow → click the **Anthropic Chat Model** node → select your credential

## Step 3 — Activate & Test

Toggle the workflow **Active** (top-right). Then from your terminal:

```bash
curl -X POST http://localhost:5678/webhook/wiki-agent \
  -H "Content-Type: application/json" \
  -d '{"chatInput": "Who is Vannevar Bush?"}'
```

You should get a grounded answer with a citation like `[entities/vannevar-bush.md]`.

Now try an off-topic question:

```bash
curl -X POST http://localhost:5678/webhook/wiki-agent \
  -d '{"chatInput": "What is the capital of France?"}'
# → "I don't know - not in knowledge base"
```

That refusal is the whole point — the agent only answers from the wiki.

## Step 4 — Walk the Workflow

Open `Wiki AI Agent (Git)` in the n8n editor. Five nodes:

### 4.1 Webhook
- **Method:** POST
- **Path:** `wiki-agent`
- **Response Mode:** Response Node (so we can return JSON at the end)

### 4.2 Sync Wiki Repo (Code node)

Clones the public wiki repo on first call, pulls on subsequent calls, then loads `index.html` (the curated TOC) so we can inject it into the agent's system prompt:

```javascript
const fs = require('fs');
const path = require('path');
const { execSync } = require('child_process');

const repoPath = '/tmp/wiki-repo';
const repoUrl = 'https://github.com/gabytal/n8n-llm-wiki-agent-workshop.git';

if (fs.existsSync(repoPath + '/.git')) {
  execSync(`cd ${repoPath} && git pull --ff-only`);
} else {
  execSync(`git clone ${repoUrl} ${repoPath}`);
}
const indexPath = path.join(repoPath, 'index.html');
const wikiIndex = fs.existsSync(indexPath) ? fs.readFileSync(indexPath, 'utf-8') : '';
return [{ json: { ...$input.first().json, repoPath, wikiIndex, status: 'ready' } }];
```

> **Why a Code node?** It needs `child_process` for `git`. `docker-compose.yml` allows this via `NODE_FUNCTION_ALLOW_BUILTIN=fs,path,child_process`.

> **Why load `index.html` here?** We want the agent to know what's in the wiki *before* it thinks about which tool to call. Injecting the index into the system prompt lets the model jump straight to `read_wiki_page('concepts/llm-wiki-pattern.md')` instead of round-tripping through `list_wiki_files` first.

### 4.3 AI Agent

The brain. Its system prompt embeds the wiki index from the Sync node and enforces the pattern:

```
You are a knowledge base assistant for the LLM Wiki workshop.
Answer questions ONLY using the wiki content provided through the tools.

--- WIKI INDEX ---
{{ $json.wikiIndex }}    ← injected at runtime from index.html
--- END INDEX ---

ALWAYS:
1. Use the INDEX above to identify which page(s) likely contain the answer
2. Call read_wiki_page with the relative path to load full content
3. Only call list_wiki_files if the INDEX doesn't cover the topic
4. Cite sources using [filename.md] format
5. If the answer isn't in the wiki, reply: "I don't know - not in knowledge base"
```

Note the `=` prefix on the systemMessage in the JSON — that turns it into an n8n expression so `{{ $json.wikiIndex }}` resolves to the actual index content.

Connected to:
- **Anthropic Chat Model** (Claude Haiku 4.5) — the LLM
- **Tool: Read Page** — `read_wiki_page` (primary)
- **Tool: List Files** — `list_wiki_files` (fallback)

### 4.4 Tool: List Files (toolCode)

```javascript
const fs = require('fs');
const path = require('path');
const wikiDir = '/tmp/wiki-repo/wiki';
const files = fs.readdirSync(wikiDir, { recursive: true });
const mdFiles = files.filter(f => typeof f === 'string' && f.endsWith('.md'));
const fileList = mdFiles.map(f => {
  const stats = fs.statSync(path.join(wikiDir, f));
  return `- ${f} (${(stats.size / 1024).toFixed(1)}KB)`;
}).join('\n');
return `Found ${mdFiles.length} wiki files:\n${fileList}`;
```

### 4.5 Tool: Read Page (toolCode)

```javascript
const fs = require('fs');
const path = require('path');
const filename = (typeof query === 'string' ? query : (query && query.filename) || '').trim();

if (!filename) return 'Error: filename is required';
const wikiDir = '/tmp/wiki-repo/wiki';
const candidates = [
  path.join(wikiDir, filename),
  path.join(wikiDir, filename.endsWith('.md') ? filename : filename + '.md')
];
for (const p of candidates) {
  if (fs.existsSync(p) && fs.statSync(p).isFile()) {
    return `Content of ${filename}:\n\n` + fs.readFileSync(p, 'utf-8');
  }
}
return `Error: File "${filename}" not found in wiki.`;
```

> **Why `toolCode` and not `toolWorkflow`?** `toolCode` runs JS inline and binds directly to the agent. `toolWorkflow` requires a separate sub-workflow with a trigger — more moving parts.

### 4.6 Respond to Webhook

Returns `{ "answer": "<agent output>" }` as JSON.

## Step 5 — Extend the Knowledge Base

The wiki uses 4 folders, each with a purpose:

| Folder       | Contains                              |
|--------------|---------------------------------------|
| `concepts/`  | Pattern definitions, mental models    |
| `entities/`  | People, tools, products               |
| `sources/`   | Original articles, dated references   |
| `analysis/`  | Meta-commentary, evolution writeups   |

To add new content:

1. Drop a markdown file into the right folder
2. Use clear headings (the agent uses them as cues)
3. Cross-link with `[[other-file]]` — the agent picks up on these
4. **Add an entry to `index.html`** with a one-line description — this is what the agent reads to find your page:
   ```markdown
   - **[My New Concept](wiki/concepts/my-new-concept.md)** — One-line description of what this page covers
   ```
5. Commit and push to GitHub:

```bash
git add wiki/concepts/my-new-page.md index.html
git commit -m "add my-new-page concept"
git push
```

The next webhook call will `git pull` automatically and have the new content available — *and* the index will tell the agent it exists.

> **What if I forget to update `index.html`?** The agent falls back to `list_wiki_files`, which sees the new file by name. It just can't reason about its content as well without the description.

## Step 6 — Try Harder Questions

```bash
# Multi-hop: agent must read multiple files
curl -X POST http://localhost:5678/webhook/wiki-agent \
  -d '{"chatInput": "How does the LLM Wiki pattern differ from RAG?"}'

# Entity question
curl -X POST http://localhost:5678/webhook/wiki-agent \
  -d '{"chatInput": "What is Memex and who built it?"}'

# Should refuse
curl -X POST http://localhost:5678/webhook/wiki-agent \
  -d '{"chatInput": "What is the weather in Tokyo?"}'
```

Watch the **Executions** tab in n8n while testing — you can see exactly which files the agent read for each answer.

## The Pattern (Why This Works)

Karpathy's insight: instead of stuffing every doc into a vector DB and hoping retrieval finds the right chunk, **compile** your knowledge into a clean, hierarchical wiki. Then let the LLM **navigate** it the way a person would — list files, read what looks relevant, cite sources.

Compared to traditional RAG:

| Aspect    | RAG (vectors)        | LLM Wiki                       |
|-----------|----------------------|--------------------------------|
| Storage   | Embeddings DB        | Markdown files in git          |
| Retrieval | Similarity search    | LLM reads filenames + content  |
| Updates   | Re-embed pipeline    | `git push`                     |
| Citations | Chunk IDs            | Filenames                      |
| Debug     | Hard                 | Read the file                  |

For more, see [`wiki/concepts/llm-wiki-pattern.md`](wiki/concepts/llm-wiki-pattern.md) and [`wiki/concepts/rag-vs-llm-wiki.md`](wiki/concepts/rag-vs-llm-wiki.md).

## Troubleshooting

**Webhook returns 404**
- Workflow not active. Toggle the switch in the top-right of the editor.

**Agent says "I don't know" for everything**
- Check the **Executions** tab — did `list_wiki_files` succeed? If it errored with `Cannot find module 'fs'`, your `NODE_FUNCTION_ALLOW_BUILTIN` env var didn't propagate. Restart: `docker compose restart n8n`.

**Anthropic Chat Model node shows as "not installed"**
- Wrong typeVersion. Should be `1.3` for `lmChatAnthropic`.

**Sync Wiki Repo fails with "could not read Username"**
- Repo URL is wrong (private repo). The shipped workflow uses the public GitHub URL.

## What's Next

- **Add a chat UI**: build a simple HTML page that POSTs to the webhook
- **Swap the model**: try Sonnet 4.5 for harder questions, or local Ollama for privacy
- **Multi-repo**: clone several knowledge bases and let the agent search across them
- **Embed-on-write lint**: add a workflow that validates new wiki pages on PR

Have fun.
