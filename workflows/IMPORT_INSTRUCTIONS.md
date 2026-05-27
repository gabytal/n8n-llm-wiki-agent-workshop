# Import Wiki AI Agent to n8n

## Option 1: Import via n8n UI (Recommended)

The workflow JSON has been **copied to your clipboard**!

1. Open your n8n instance: http://localhost:5678/workflow/XBYTvSLeWE5kezSG
2. Click the **"+" button** (top right) or go to **Workflows → Add workflow**
3. Click the **⋮** (three dots menu) → **Import from File** or **Import from Clipboard**
4. Select **"Import from Clipboard"** (since JSON is already copied)
5. Click **Import**
6. Save the workflow (Ctrl/Cmd + S)

**If clipboard doesn't work:**
- Choose **"Import from File"**
- Select `/Users/gabyt/code/n8n-ollama/workflows/wiki-chatbot.json`

---

## Option 2: Import via n8n API

If you have n8n API access configured:

```bash
# Get your n8n API key from: Settings → API → Create API Key
export N8N_API_KEY="your-api-key-here"

curl -X POST http://localhost:5678/api/v1/workflows \
  -H "X-N8N-API-KEY: $N8N_API_KEY" \
  -H "Content-Type: application/json" \
  -d @/Users/gabyt/code/n8n-ollama/workflows/wiki-chatbot.json
```

---

## Option 3: Copy via Command

If the clipboard method failed, run this to copy again:

```bash
cat /Users/gabyt/code/n8n-ollama/workflows/wiki-chatbot.json | pbcopy
echo "Workflow JSON copied to clipboard!"
```

---

## After Import

### 1. Configure Environment Variables

The workflow needs your Anthropic API key:

**In n8n:**
- Go to **Settings** → **Environments**
- Add: `ANTHROPIC_API_KEY` = `your-api-key`

**Or in Docker Compose:**

Edit `/Users/gabyt/code/n8n-ollama/docker-compose.yml`:

```yaml
environment:
  - ANTHROPIC_API_KEY=sk-ant-your-key-here
```

Then restart:
```bash
docker-compose down && docker-compose up -d
```

### 2. Activate the Workflow

1. Open the imported workflow in n8n
2. Click **"Inactive"** toggle (top right) → **"Active"**
3. The webhook will be available at: `http://localhost:5678/webhook/wiki`

### 3. Test the Agent

```bash
# Test query
curl -X POST http://localhost:5678/webhook/wiki \
  -H "Content-Type: application/json" \
  -d '{
    "query": "What files are available in the wiki?"
  }'
```

**Expected agent behavior:**
1. Claude will call `list_wiki_files` or `search_wiki_index`
2. Review results
3. Possibly call `read_wiki_page` for specific files
4. Synthesize answer with citations

---

## Verify Import Success

You should see:
- ✅ 14 nodes in the workflow
- ✅ Webhook trigger at the start
- ✅ Tool routing with 3 tool nodes (Search Index, List Files, Read Page)
- ✅ Iteration loop connecting back to "Prepare Agent Request"
- ✅ "LLM Wiki Agent" as the workflow name

---

## Troubleshooting

### "Unauthorized" Error
- n8n API requires authentication
- Use UI import method instead

### "ANTHROPIC_API_KEY not found"
- Set the environment variable (see step 1 above)
- Restart n8n after setting env vars

### "Cannot find module 'fs'"
- The Code nodes use Node.js `fs` module
- This should work by default in n8n
- If not, check n8n version (requires n8n 1.0+)

### Webhook not responding
- Ensure workflow is **Active** (toggle top right)
- Check webhook URL: `http://localhost:5678/webhook/wiki`
- Look at **Executions** tab for error logs

---

## Next Steps

Once imported and tested:

1. **Create wiki content** in `/data/wiki/` (or your Docker volume)
2. **Add an index** at `/data/wiki/index.md` for better search
3. **Test multi-turn conversations** (see README-wiki-agent.md)
4. **Monitor agent metrics** in the response JSON

See `/workflows/README-wiki-agent.md` for full documentation!