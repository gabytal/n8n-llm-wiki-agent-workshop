# Workshop: 5-Slide Outline

Presentation ideas for the LLM Wiki Agent workshop. Each slide has a title, the core message, and talking points.

---

## Slide 1 — The Problem

**Title:** "Your docs are useless to your LLM"

**Core message:** Off-the-shelf chatbots hallucinate because they don't know your stuff. Most "chat with your docs" solutions (vector RAG) are complex, opaque, and hard to debug.

**Talking points:**
- LLMs are smart but generic — they don't know your codebase, your wiki, your team's decisions
- Vector RAG: embeddings DB, chunking strategy, retrieval tuning, re-ranking… and you still get wrong chunks
- When it fails, you can't tell *why* it failed
- What if the LLM could just… read the files?

**Visual idea:** Split screen — left: tangled RAG diagram with 6 boxes. Right: a folder of `.md` files.

---

## Slide 2 — The Pattern

**Title:** "Compile once, query forever" (Karpathy's LLM Wiki)

**Core message:** Curate a clean markdown wiki *once*. Let the LLM navigate it like a human would: list files, read what looks relevant, cite sources.

**Talking points:**
- Inspired by Karpathy's tweet + Vannevar Bush's Memex (1945)
- Knowledge compilation is the hard part — but you do it ONCE
- Storage = git. Retrieval = `ls` + `cat`. Citations = filenames.
- 4 folders: `concepts/`, `entities/`, `sources/`, `analysis/`
- Updating the KB = `git push`. No re-embed pipeline.

**Visual idea:** Comparison table

| | RAG | LLM Wiki |
|---|---|---|
| Storage | Vector DB | Markdown in git |
| Updates | Re-embed | `git push` |
| Debug | 🤷 | Read the file |

---

## Slide 3 — What We're Building

**Title:** "A webhook that cites its sources"

**Core message:** One HTTP endpoint. Ask anything. Get an answer grounded in your wiki, with `[filename.md]` citations. Off-topic questions get refused.

**Talking points:**
- Live demo (or screenshot) of the curl:
  ```bash
  curl -X POST http://localhost:5678/webhook/wiki-agent \
    -d '{"chatInput": "What is the LLM Wiki pattern?"}'
  ```
- Response includes citation: `...[concepts/llm-wiki-pattern.md]`
- "What's the capital of France?" → `"I don't know - not in knowledge base"`
- Architecture: `Webhook → Sync Repo → AI Agent ⇄ Tools → Respond`
- Stack: n8n + Claude Haiku 4.5 + 2 tiny JS tools

**Visual idea:** Architecture diagram from README + screenshot of a real curl response.

---

## Slide 4 — How It Works

**Title:** "5 nodes, 50 lines of glue"

**Core message:** The whole agent is 5 n8n nodes. The "magic" is two ~10-line JS tools that the LLM decides when to call.

**Talking points:**
- **Webhook** — entry point
- **Sync Wiki Repo** — `git pull` the latest content (so updates are instant)
- **AI Agent** — Claude Haiku 4.5 with a strict system prompt: "cite or say IDK"
- **Tool 1: list_wiki_files** — `fs.readdirSync` returns all `.md` paths
- **Tool 2: read_wiki_page** — `fs.readFileSync` returns one file's content
- **Respond** — JSON response

**Key insight:** The LLM is the retriever. No embeddings, no chunking, no scoring. Just "here are the filenames, decide what to read."

**Visual idea:** Screenshot of the n8n canvas with the 5 nodes wired together.

---

## Slide 5 — Try It / Extend It

**Title:** "Your turn — clone, run, extend"

**Core message:** 3 commands to running. Add your own knowledge by dropping markdown files and pushing.

**Talking points:**
- Quick start:
  ```bash
  git clone <repo>
  echo "ANTHROPIC_API_KEY=..." > .env
  docker compose up -d
  ```
- Extending:
  1. Drop `.md` into `wiki/concepts/` (or `entities/`, `sources/`, `analysis/`)
  2. `git push`
  3. Next webhook call auto-pulls the new content
- Where to go next:
  - Swap Claude for local Ollama (privacy)
  - Add a chat UI on top of the webhook
  - Multi-repo (search across several knowledge bases)
  - Add a wiki linter as a separate workflow
- **Repo:** github.com/gabytal/n8n-llm-wiki-agent-workshop
- **Workshop guide:** `WORKSHOP.md`

**Visual idea:** QR code to the repo + the working curl as a closing image.

---

## Optional: Speaker Notes / Timing

- ~15 min total (3 min/slide) for a lightning version
- ~45 min with live demo at slide 3 + hands-on at slide 5
- Good audience: engineers who've heard "RAG is hard" and want a simpler mental model
- Punchy hook for slide 1: "Show of hands — who has built a RAG pipeline that actually works in production?"
