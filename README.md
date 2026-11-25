# claude-lead-finder-agents

One Claude Code subagent that finds good-fit businesses and the real email address an owner reads, from free public data only.

- `lead-finder`: sources businesses from public license records (or another public list), proves each website belongs to that business, keeps only email addresses printed on the business's own site, checks MX over DNS-over-HTTPS, scores the batch with weights learned from past bounces and replies, and hands a batch file or CSV to your outreach step.

This repo started as two near-identical agents (a general prospect pipeline and a home-service contractor finder). They shared the same method, rules and evals, so they were merged into one generic agent. The contractor-specific company profile is kept as an optional enrichment step.

Hard rules it follows: no paid data, no SMTP probing, no scraping sites whose terms forbid it, never guess an address, keep the URL where every address was found, never re-add anyone already contacted or suppressed, and never write or send email.

## Install

```
cp agents/*.md ~/.claude/agents/
# or, for one project
cp agents/*.md <your-project>/.claude/agents/
```

Restart Claude Code or run `/agents` to check it loads.

## Files you need to create

- `knowledge/GOAL.md`: your current business goal.
- `<project>/docs/VERTICALS.md`: which verticals to source and why, plus verticals to skip because outreach or the product is restricted.
- `<project>/docs/SERVICE-CATALOG.md`: what you sell, so the agent can judge fit.
- `<project>/docs/AUTHORITY.md` and `<project>/research/DECISIONS-CURRENT.md`: your rulings and decision log.
- `prospects/evals/email-gold.jsonl`: hand-checked records (business, site, correct address or "none", what you verified). Plus a separate holdout gold file used only to check rule changes.
- Optional `prospects/evals/enrich-gold.jsonl` if you use the enrichment step.
- A suppression list and a record of everyone already contacted.
- `lessons/lead-finder.md`: lessons file, append only.

## Scripts you supply

The agent file describes each step (source, find email, enrich, learn, score and export) and what it must and must not do. Write these as small resumable scripts in `tools/prospect/`. Each should use short timeouts, fetch one page at a time per site, and support an eval mode where noted.

## Notes

Check CAN-SPAM, your local laws, and each data source's terms before using any list for outreach.
