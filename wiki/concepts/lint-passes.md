---
type: concept
created: 2026-05-02
updated: 2026-05-02
sources: [2026-04-07-llm-wiki-revolution-analyticsvidhya.md]
---

# Lint Passes

## Definition

Periodic audits of the entire wiki to detect and fix quality issues: contradictions, obsolete statements, orphan pages, missing concept pages, and structural inconsistencies. Performed by LLM at appropriate intervals.

## Key Principles

1. **Proactive maintenance**: Don't wait for problems to accumulate
2. **Automated detection**: LLM scans for patterns humans would miss
3. **Periodic rhythm**: Conduct at intervals (e.g., after every N ingests, weekly)
4. **Actionable output**: Lint pass produces specific recommendations for improvement
5. **Wiki health**: Keeps knowledge base accurate, consistent, and well-connected

## How It Works

### What Lint Passes Check

**Contradictions between pages**:
- Conflicting claims across different sources
- Inconsistent definitions of same concept
- Disagreement on dates, facts, interpretations

**Obsolete statements**:
- Claims superseded by newer sources
- Outdated information that's been corrected
- Deprecated techniques/tools mentioned as current

**Orphan pages**:
- Pages with no inbound links
- Isolated knowledge not connected to the graph
- Potentially indicates missing cross-references

**Missing concept pages**:
- Concepts frequently referenced but lacking dedicated page
- Important ideas mentioned across multiple sources without synthesis
- Gaps in coverage

**Structural issues**:
- Broken wiki links
- Inconsistent frontmatter
- Missing metadata (tags, dates, sources)
- Pages that should be split or merged

### Lint Workflow

1. **Trigger lint pass**: User asks LLM to audit wiki (or scheduled automatically)
2. **LLM scans**: Reads index, samples pages across categories, identifies patterns
3. **Report generation**: LLM produces findings with specific page references
4. **User review**: Assess priorities — what to fix now vs. later
5. **Remediation**: LLM makes updates based on user approval
6. **Log update**: Record lint pass results in log.md

## Examples

### Sample Lint Pass Report

```markdown
# Lint Pass Report — 2026-05-02

## Contradictions Detected (2)

1. **Scaling threshold**: 
   - `llm-wiki-pattern.md` says "few hundred notes" 
   - `rag-vs-llm-wiki.md` says "100-200 pages"
   → Recommendation: Standardize to "100-200 pages" based on Analytics Vidhya source

2. **qmd installation**:
   - `qmd.md` shows `npm install -g @tobilu/qmd`
   - No source verification yet
   → Recommendation: Verify package name before next user installation attempt

## Obsolete Statements (0)

None detected.

## Orphan Pages (1)

- `tobi-lutke.md` — only linked from qmd.md, not from any concept pages
  → Recommendation: Add link from llm-wiki-pattern.md in "Related Entities" section

## Missing Concept Pages (2)

1. **Git workflow** — mentioned in Analytics Vidhya source but no dedicated page
2. **Prompt engineering** — referenced as "critical skill" but not explored

## Structural Issues (1)

- `obsidian.md` missing `updated` field in frontmatter
  → Recommendation: Add `updated: 2026-05-02`
```

### Lint Pass Frequency

**After initial setup**: High frequency (every 2-3 ingests)
- Catch structural inconsistencies early
- Establish conventions

**Mature wiki**: Lower frequency (weekly or every 10 ingests)
- Maintenance mode
- Periodic health checks

**Triggered lint**: On-demand when user suspects issues
- After large batch ingest
- Before important queries
- When noticing quality degradation

## Related Concepts

- [LLM Wiki Pattern](wiki/concepts/llm-wiki-pattern.md)
- [Knowledge Base Maintenance](wiki/concepts/knowledge-base-maintenance.md)
- [Knowledge Compilation](wiki/concepts/knowledge-compilation.md)

## Related Entities

- [Claude Code](wiki/entities/claude-code.md) — performs lint passes

## Contradictions / Open Questions

- **Automation level**: Should lint passes be fully automated (scheduled) or user-triggered?
- **False positives**: How to handle cases where "contradictions" are actually legitimate different perspectives?
- **Performance**: At what wiki size do full lint passes become too expensive (token-wise)?

## References

- [2026-04-07-llm-wiki-revolution-analyticsvidhya.md](wiki/sources/2026-04-07-llm-wiki-revolution-analyticsvidhya.md)
