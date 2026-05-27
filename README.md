# n8n Automation Platform + LLM Wiki Workshop

🚀 Run automation workflows locally with n8n - includes everything for the **LLM Wiki Chatbot Workshop**!

## What This Gives You

- **n8n** (v2.16.1) - Automation platform with visual workflow builder
- **Auto Setup** - Owner account + workflows automatically configured
- **Sample Workflows** - Including wiki chatbot workflow
- **Claude Skills** - Pre-built ingest & query skills for LLM wiki pattern
- **Workshop Guide** - Complete step-by-step tutorial → See [WORKSHOP.md](WORKSHOP.md)

## 🎓 Workshop: Build Your Knowledge Chatbot

Want to build a chatbot that answers from YOUR documents? 

**👉 Start here:** [WORKSHOP.md](WORKSHOP.md)

Learn Karpathy's "compile once, query forever" pattern and build a working chatbot in 2-3 hours.

## Quick Start

```bash
# 1. Clone and setup
git clone <your-repo>
cd n8n-ollama
cp .env.example .env

# 2. Start everything
docker compose up -d

# 3. Wait for setup to complete (~2 minutes)
# The system automatically creates your account and imports workflows
docker compose logs -f n8n-init

# 4. Access n8n once you see "✅ Setup complete!"
open http://localhost:5678
```

**Login Credentials:**
- Email: `admin@localhost.local`
- Password: `Admin123`

> ⏱️ **Important:** Wait at least 2 minutes after starting for the authentication process to complete. The system will automatically create your account and configure everything. You'll know it's ready when you see "✅ Setup complete!" in the logs.

## Using n8n

After the setup completes and you log in:

1. Go to **Workflows** to see your imported workflows
2. Create new workflows using the visual editor
3. All workflows are automatically saved to your persistent volume

## Configuration

Edit `.env` file to customize:

```env
# n8n
N8N_PORT=5678
N8N_BASIC_AUTH_USER=admin@localhost.local
N8N_BASIC_AUTH_PASSWORD=Admin123
```

## Common Commands

```bash
# Start
docker compose up -d

# Stop
docker compose down

# Reset everything (fresh start)
docker compose down -v
docker compose up -d

# View logs
docker compose logs -f n8n
docker compose logs -f n8n-init

# Check status
docker compose ps
```

## Troubleshooting

**Can't login after starting:**
- Wait at least 2 minutes for the auth setup to complete
- Check setup logs: `docker compose logs -f n8n-init`
- Look for "✅ Setup complete!" message

**Workflows not appearing:**
- Check import logs: `docker compose logs n8n-init`
- Workflows are in the `/workflows` folder
- Restart the init container: `docker compose restart n8n-init`

**n8n not accessible:**
- Verify n8n is running: `docker compose ps`
- Check health status: `docker compose logs n8n`

## Adding Your Own Workflows

1. Create workflow in n8n UI
2. Export as JSON
3. Save to `workflows/` folder
4. Restart: `docker compose restart n8n-init`

## Architecture

```
┌─────────────────────────────────┐
│  Docker Containers              │
│                                 │
│  ┌──────────┐                   │
│  │   n8n    │                   │
│  │  :5678   │                   │
│  └──────────┘                   │
│       │                         │
│  ┌──────────┐                   │
│  │ n8n-init │                   │
│  └──────────┘                   │
└─────────────────────────────────┘
```

- **n8n**: Workflow automation platform
- **n8n-init**: Auto-creates account + imports workflows

## License

MIT - Free to use for learning and projects!