# Raw Sources Folder

Place your original documents here. The ingestion skill (`.claude/skills/ingest.md`) will read these files and generate structured wiki pages.

## What to Add

- **Articles** - Blog posts, papers, documentation
- **Notes** - Meeting notes, research notes, personal notes
- **Code** - Important code snippets with context
- **PDFs** - Research papers, ebooks, manuals
- **Text files** - Any plain text knowledge

## Supported Formats

- Markdown (`.md`)
- Plain text (`.txt`)
- PDF (`.pdf`)
- Code files (`.py`, `.js`, `.go`, etc.)

## How It Works

1. Add files to this `raw/` folder
2. Run `/ingest` in Claude Code
3. Claude reads your files and creates:
   - `wiki/concepts/` - Ideas and methods
   - `wiki/entities/` - People, tools, organizations
   - `wiki/sources/` - Source summaries
4. Update `index.html` with new page descriptions
5. Commit and push to GitHub

## Example

```bash
# Add a document
cp ~/my-article.md raw/

# Run ingestion
/ingest

# Check what was created
git status
git diff wiki/

# Update index
nano index.html

# Push
git add .
git commit -m "Ingest: my-article"
git push
```

## Important

- **Never edit files in raw/** - they are your ground truth
- Keep originals intact
- If you need to update knowledge, add a new file or update the wiki directly