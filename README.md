# rattlesnakesbymail-data

This repository is a nightly export of the Rattlesnakes By Mail dataset, a public record of documented and observed behaviour of AI crawlers, covering 15 crawlers from OpenAI, Anthropic, Perplexity, Google, Microsoft and Apple, with 141 current claims as of 2026-09-17, each claim carrying a vendor quote, a date and a source URL. Claim ids run to 150; ids 142 to 150 are observed_here claims, recorded from this site's own logs.

## Source site

The canonical site is https://rattlesnakesbymail.com. This repository mirrors its data tables. For the live pages, page families, and full context, start at the home page: https://rattlesnakesbymail.com.

## MCP server

The same data is reachable live over a remote MCP server (streamable HTTP) at https://rattlesnakesbymail.com/mcp. It supports read tools for looking up a crawler, identifying a user agent, listing changes, and asking a question. See /data on the site for the current list of tools and their schemas; exact tool names are not restated here to avoid drift from the live server.json.

### Install in Claude Code

```
claude mcp add --transport http rattlesnakes https://rattlesnakesbymail.com/mcp
```

### Install in Claude Desktop

Add a custom connector with this URL:

```
https://rattlesnakesbymail.com/mcp
```

## Files and schema

Each table is exported as both JSON and CSV. Columns are not restated here; see /data on the site for the current schema of each table.

| Table | Description | Files |
|---|---|---|
| entities | One row per crawler, fetcher, search bot, ads bot, or robots.txt policy token. | entities.json, entities.csv |
| claims | One row per atomic, dated, evidenced fact about an entity. | claims.json, claims.csv |
| changes | The cross-vendor changelog of claim additions, updates, and disputes. | changes.json, changes.csv |
| observation_daily | Nightly rollup of crawler requests by entity and day. | observation_daily.json, observation_daily.csv |
| questions | Published questions from the ledger: normalised text, count, sources, gap status. | questions.json, questions.csv |
| notes | Published notes only. | notes.json, notes.csv |
| mcp_calls_daily | Daily aggregate of MCP tool calls by tool and client, with a mean latency. | mcp_calls_daily.json, mcp_calls_daily.csv |

Versioned snapshots are published under `/data/<date>/` on the site, with a latest manifest at /data/latest.json.

## Publisher

Rattlesnakes By Mail is published by Benes the Menace, the digital marketing consultancy of Peter Benes in Prince Edward County, Ontario, and the findings feed Attention Optimization, a service on AI-era search visibility.

## Licence

This dataset is released under CC BY 4.0. Attribute rattlesnakesbymail.com as Rattlesnakes By Mail, rattlesnakesbymail.com, and link to the specific claim URL used.

## Citation format

When citing an individual claim, use this exact format:

```
Rattlesnakes By Mail, claim <id>, verified <date>, <url>
```

For example: Rattlesnakes By Mail, claim 1, verified 2026-09-13, https://rattlesnakesbymail.com/claims/1

## Census findings (2026-09-15)

These are observations of the source site only, recorded on 2026-09-15. Five crawlers arrived between 13 and 35 hours before the sitemap was submitted. ClaudeBot read all 170 pages in HTML, Markdown and JSON. No crawler requested llms.txt. GoogleOther fetched /mcp. Of 704 requests, 99 percent were verified. The site now publishes 315 sitemap URLs with 945 fetchable views: each URL available in HTML, Markdown and JSON formats.

## Page families on the source site

- Crawlers: `https://rattlesnakesbymail.com/crawlers/<slug>`
- Claims: `https://rattlesnakesbymail.com/claims/<id>`
- Observed: `https://rattlesnakesbymail.com/observed/<slug>`
- Questions: https://rattlesnakesbymail.com/questions
- Changes: https://rattlesnakesbymail.com/changes, Atom feed at https://rattlesnakesbymail.com/changes.xml
- Fields: `https://rattlesnakesbymail.com/fields/<field>`
- Compare: `https://rattlesnakesbymail.com/compare/<a>-vs-<b>`
- Data: https://rattlesnakesbymail.com/data
- Method: https://rattlesnakesbymail.com/method

## Discovery files

- https://rattlesnakesbymail.com/sitemap.xml
- https://rattlesnakesbymail.com/sitemap-machine.xml
- https://rattlesnakesbymail.com/.well-known/agent.json
- https://rattlesnakesbymail.com/.well-known/mcp/server.json

## Home

https://rattlesnakesbymail.com
