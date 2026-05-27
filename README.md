# LLM Wiki Agent (n8n + Claude)

A working chatbot that answers questions from a markdown knowledge base. Built on [n8n](https://n8n.io) with Claude Haiku 4.5.

Implements Karpathy's [LLM Wiki pattern](wiki/concepts/llm-wiki-pattern.md): **compile knowledge once, query forever**.

## How It Works

```
POST /webhook/wiki-agent
   │
   ▼
┌──────────────────┐
│ Sync Wiki Repo   │  git clone/pull from GitHub → /tmp/wiki-repo
└──────────────────┘
   │
   ▼
┌──────────────────┐     ┌─────────────────┐
│   AI Agent       │ ──► │ list_wiki_files │
│ (Claude Haiku)   │ ──► │ read_wiki_page  │
└──────────────────┘     └─────────────────┘
   │
   ▼
{ "answer": "...[concepts/llm-wiki-pattern.md]" }
```

The agent fetches the latest wiki content from this public GitHub repo on every request, then uses two tools to list and read markdown files. It cites sources inline like `[concepts/llm-wiki-pattern.md]` and refuses to answer questions outside the knowledge base.

## Prerequisites

- Docker + Docker Compose
- An [Anthropic API key](https://console.anthropic.com/)

## Quick Start

```bash
git clone https://github.com/gabytal/n8n-llm-wiki-agent-workshop.git
cd n8n-llm-wiki-agent-workshop

# Set your API key
echo "ANTHROPIC_API_KEY=sk-ant-..." > .env

# Start everything
docker compose up -d

# Wait for setup (~2 minutes), then watch for "✅ Setup complete!"
docker compose logs -f n8n-init
```

Open http://localhost:5678 and log in:
- Email: `admin@localhost.local`
- Password: `Admin123`

The `Wiki AI Agent (Git)` workflow is imported automatically. **Activate it** with the toggle in the top-right of the workflow editor.

## Test It

```bash
curl -X POST http://localhost:5678/webhook/wiki-agent \
  -H "Content-Type: application/json" \
  -d '{"chatInput": "What is the LLM Wiki pattern?"}'
```

Expected response:
```json
{
  "answer": "The LLM Wiki pattern is... [concepts/llm-wiki-pattern.md]"
}
```

Off-topic questions get refused:
```bash
curl -X POST http://localhost:5678/webhook/wiki-agent \
  -H "Content-Type: application/json" \
  -d '{"chatInput": "What is the capital of France?"}'
# → "I don't know - not in knowledge base"
```

## Extend the Knowledge Base

1. Add markdown files under `wiki/` (use the existing `concepts/`, `entities/`, `sources/`, `analysis/` structure)
2. Commit and push to GitHub
3. The next webhook call automatically pulls the latest content

No re-deploy needed — the agent does `git pull` on every invocation.

## Workshop

Want to learn how this was built and replicate it from scratch? See **[WORKSHOP.md](WORKSHOP.md)**.

## Common Commands

```bash
docker compose up -d            # Start
docker compose down             # Stop
docker compose down -v          # Reset (wipe all data)
docker compose logs -f n8n      # View n8n logs
docker compose restart n8n      # Restart n8n
```

## Project Layout

```
.
├── docker-compose.yml             # n8n + auto-setup
├── workflows/
│   └── wiki-agent-langchain.json  # The shipped workflow (auto-imported)
├── wiki/                          # Knowledge base (markdown files)
│   ├── concepts/
│   ├── entities/
│   ├── sources/
│   └── analysis/
└── WORKSHOP.md                    # Step-by-step build guide
```

## License

MIT
