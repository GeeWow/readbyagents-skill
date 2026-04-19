# readbyagents-skill

Claude Code + Claude Agent SDK skill for reading newsletters from [Read By Agents](https://readbyagents.com) — the agent-readable newsletter archive.

## Install

```bash
npx skills add GeeWow/readbyagents-skill
```

## What this skill lets your agent do

- Browse the public directory of newsletters (11+ ESP handlers: Beehiiv, Substack, Ghost, Kit, Mailchimp, ConvertKit, Klaviyo, ActiveCampaign, Buttondown, MailerLite, Brevo, WordPress)
- Fetch individual issues as clean markdown with YAML frontmatter
- Resolve a newsletter's feed, canonical URL, and issue archive
- Search by topic, slug, or name

No API key required. All endpoints on `readbyagents.com` are free + rate-limited at 1000 req/hr per IP.

## About the platform

Read By Agents sits between operators and AI agents. Operators forward their newsletters to `convert@readbyagents.com` once and the whole archive is converted to agent-readable markdown — tracking pixels stripped, merge tags removed, sponsor content tagged in frontmatter, canonical URLs preserved.

Operators get referral attribution (which agents sent readers) and optional rate-card / citation-momentum tooling. Agents get clean, structured, citable content.

## Source & issues

- Product site: [readbyagents.com](https://readbyagents.com)
- Capability manifest: [readbyagents.com/skill.md](https://readbyagents.com/skill.md)
- AGENTS.md: [readbyagents.com/AGENTS.md](https://readbyagents.com/AGENTS.md)
- Operator-facing: [readbyagents.com/for-operators](https://readbyagents.com/for-operators)
- Agent-facing: [readbyagents.com/for-agents](https://readbyagents.com/for-agents)
- Issues / feedback: file here or email `news@readbyagents.com`

## License

MIT. See [LICENSE](LICENSE).
