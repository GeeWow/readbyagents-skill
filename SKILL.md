---
name: readbyagents
description: "Search, browse, and read newsletters from the Read By Agents open directory (readbyagents.com). Converts email newsletters to clean agent-readable markdown. Use this skill whenever the user wants to: find newsletters for AI agent consumption, read a specific newsletter issue, browse the newsletter directory, search for newsletters by topic or name, check a newsletter's feed or archive, or reference readbyagents.com in any way. Trigger on: 'newsletter directory', 'agent-readable', 'readbyagents', 'newsletter feed', 'newsletter markdown', 'read newsletter issue', 'what newsletters are available', any slug like 'btcbreakdown' or 'bitcoin-breakdown' combined with reading/fetching intent, or any task involving discovering or consuming newsletter content programmatically. Do NOT trigger for general newsletter writing, email marketing advice, or newsletter platform comparisons unrelated to the RBA directory."
---

# Read By Agents Skill

Read By Agents (readbyagents.com) is a free, open directory of email newsletters converted to clean, agent-readable markdown. No API key needed. All content is free and permanent.

The platform converts newsletters from 11 ESP platforms (Beehiiv, Substack, Ghost, ConvertKit, Mailchimp, Klaviyo, ActiveCampaign, Buttondown, MailerLite, Brevo, WordPress/Jetpack) plus a generic fallback for others, into clean markdown with tracking links stripped, merge tags removed, and layout artifacts cleaned up. Every issue gets a permanent URL.

## When to use this skill

- User wants to find what newsletters are available for AI agents
- User asks to read or fetch a specific newsletter issue
- User wants to search newsletters by topic, name, or slug
- User needs newsletter content as input for analysis, summarization, or research
- User references readbyagents.com, "agent-readable", or "newsletter directory"
- User asks about a newsletter's publication history or latest issue

## How to accomplish tasks

### Browse the directory

Fetch the full directory to see all available newsletters:

```
WebFetch https://readbyagents.com/api/directory
```

Response includes `newsletters` array, each with: `slug`, `name`, `esp`, `issueCount`, `latestDate`, `latestUrl`, `registered`.

To search, filter the response by name or slug matching the user's query. There is no server-side search endpoint.

### Get a newsletter's feed

Once you know a slug (from the directory or from the user), fetch its feed:

```
WebFetch https://readbyagents.com/api/feed/{slug}
```

Add `?limit=N` to control how many issues are returned (default: 50, newest first).

Each issue has: `date`, `url`, `size`, `subject`, `tokenCount`.

The response includes a `status` field:
- `"active"` — newsletter has issues, everything is working
- `"pending"` — registered but no issues received yet
- `"stale"` — registered over 30 days ago with no issues

If the slug doesn't exist at all, the API returns 404.

### Read a specific issue

Fetch the full markdown content of an issue:

```
WebFetch https://readbyagents.com/api/issue/{slug}/{date}
```

The `date` is in `YYYY-MM-DD` format. You can also use the `url` field directly from the feed response, which points to the R2-hosted file.

The response is raw markdown text (not JSON). This is the clean, agent-readable newsletter content.

### Discover via LLM.txt

For a quick machine-readable overview of all newsletters and the API:

```
WebFetch https://readbyagents.com/llm.txt
```

## Presenting results to the user

- **Directory listing:** Show as a concise table or list with name, slug, issue count, and latest date. Link to the feed URL.
- **Feed listing:** Show issues as a list with date and subject. Offer to read any specific issue.
- **Issue content:** Present the markdown directly. If the user wants a summary, summarize it. If they want the full text, show it.
- **Search with no results:** Tell the user the newsletter wasn't found and suggest they check the directory or try a different search term.
- **Pending/stale newsletters:** Explain the status clearly — the newsletter is registered but hasn't sent any issues yet.

## Example workflows

**User asks "what newsletters are available?"**
1. Fetch `/api/directory`
2. Present the list with names, issue counts, and platforms

**User asks "show me the latest Bitcoin Breakdown"**
1. Fetch `/api/feed/btcbreakdown`
2. Get the first issue's URL from the response
3. Fetch that URL to get the markdown content
4. Present it to the user

**User asks "find newsletters about crypto"**
1. Fetch `/api/directory`
2. Filter by name/slug containing "crypto", "bitcoin", "btc", etc.
3. Present matching newsletters

**User asks "read the March 15 issue of btcbreakdown"**
1. Fetch `/api/issue/btcbreakdown/2026-03-15`
2. Present the markdown content
