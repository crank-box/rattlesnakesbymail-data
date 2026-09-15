# Rattlesnakes By Mail dataset

Rattlesnakes By Mail publishes dated, sourced claims about AI crawlers and AI search engines and their agents. This repository holds a nightly export of the public tables under a Creative Commons Attribution 4.0 International license. The site is published at https://rattlesnakesbymail.com.

## What this dataset covers

Rattlesnakes By Mail logs every request from an identified crawler and publishes per-crawler daily counts. Each daily count states the share of requests whose identity was verified against the vendor's published IP ranges or reverse DNS. Every fact on Rattlesnakes By Mail is a dated claim with a verbatim vendor quote, a source URL, a method, and a confidence. A published claim is never edited: a change creates a new claim that supersedes the old claim, both claims stay addressable, and every supersession is listed in the changes table. Rattlesnakes By Mail records every search query made on the site, matches each query against the published claims, and exports the unmatched queries as gaps. Every table in this dataset is exported nightly as JSON and as CSV under CC BY 4.0.

## Tables

### entities

| Column | Description |
| --- | --- |
| id | Numeric identifier for the entity, stable across the record's lifetime. |
| slug | URL-safe identifier used in /crawlers/<slug> and /observed/<slug>. |
| vendor | The company or organization that operates or documents the entity. |
| name | Display name of the entity. |
| kind | One of crawler, fetcher, search_bot, ads_bot, engine, policy_token. |
| purpose | One of training, search, user_fetch, ads, mixed, or empty when undocumented. |
| ua_token | The token expected in the entity's User-Agent header, when documented. |
| ua_pattern | The regular expression the instrument uses to match this entity's User-Agent header. |
| robots_token | The token the entity's operator documents for robots.txt directives. |
| ip_list_url | URL of the vendor's published IP range list, when one exists. |
| docs_url | URL of the vendor's primary documentation for this entity. |
| first_documented | ISO date the entity was first documented by its vendor, when known. |
| first_seen_here | ISO date this site first logged a request from this entity. |
| last_seen_here | ISO date this site last logged a request from this entity. |
| status | One of active, retired, unverified. |
| notes | Free-text identity notes about the entity. Policy facts live in claims, not here. |
| created_at | ISO timestamp the row was created. |
| updated_at | ISO timestamp the row was last updated. |

### claims

| Column | Description |
| --- | --- |
| id | Numeric identifier for the claim, used in the claim URL pattern. |
| entity_id | The entities.id this claim is about. |
| slug | Optional URL-safe identifier for the claim. |
| field | The fact this claim states, for example respects_robots_txt. |
| value | The short typed value of the claim, for example yes, no, or a token. |
| statement | One canonical English sentence stating the fact. |
| evidence_url | Source URL the claim was verified against. |
| evidence_quote | A verbatim quote of 300 characters or fewer from the evidence URL. |
| method | One of vendor_doc, observed_here, third_party, test_here. |
| verified_at | ISO date this site checked the evidence URL. |
| confidence | One of high, medium, low. |
| status | One of current, superseded, disputed. |
| supersedes_id | The claims.id this claim replaces, when it supersedes an earlier claim. |
| evidence_date | ISO date the evidence source itself carries, when the source shows one. |
| created_at | ISO timestamp the row was created. |
| updated_at | ISO timestamp the row was last updated. |

### changes

| Column | Description |
| --- | --- |
| id | Numeric identifier for the change row. |
| claim_id | The claims.id this change is about, when the change is about one claim. |
| entity_id | The entities.id this change is about. |
| changed_at | ISO date the change was recorded. |
| kind | One of new, updated, superseded, retired, disputed. |
| old_value | The claim value before the change, when applicable. |
| new_value | The claim value after the change, when applicable. |
| evidence_url | Source URL supporting the change, when one was recorded. |
| note | A free-text note about the change, written by a reviewer. |

### observation_daily

| Column | Description |
| --- | --- |
| id | Numeric identifier for the daily rollup row. |
| date | ISO date the requests were counted on. |
| entity_id | The entities.id these requests are attributed to. |
| requests | Count of requests from this entity on this date. |
| paths | A JSON array of distinct paths requested, capped at 100 per day. |
| formats | A JSON array of distinct response formats served (html, md, json, txt, xml, csv). |
| verified_share | The share of requests whose identity was verified against a published IP range or Cloudflare's verified-bot signal, from 0 to 1. |

### questions

| Column | Description |
| --- | --- |
| id | Numeric identifier for the question. |
| ts_first | ISO timestamp the question was first asked. |
| ts_last | ISO timestamp the question was most recently asked. |
| text_norm | The normalised text of the question: lowercase, punctuation stripped, lightly stemmed. |
| count | How many times this normalised question has been asked. |
| sources | A JSON array of sources the question arrived from, for example search, mcp, seeded, prompt_batch, note. |
| matched_claim_ids | A JSON array of claims.id values this question matched, when any did. |
| gap | 1 when no published claim matched the question, 0 otherwise. |

### notes

| Column | Description |
| --- | --- |
| id | Numeric identifier for the note. |
| ts | ISO timestamp the note was submitted. |
| target_type | One of claim, entity, question. |
| target_id | The id of the claim, entity, or question this note is about. |
| body | The text of the note, as submitted. |
| author_claim | Free text the submitter gave for who or what they are, for example a model, agent, or human. |

### mcp_calls_daily

| Column | Description |
| --- | --- |
| date | ISO date the calls were made on. |
| tool | The MCP tool name called, for example lookup_crawler. |
| client_name | The MCP client name reported at initialize, or unknown. |
| calls | Count of calls to this tool by this client on this date. |
| avg_latency_ms | Mean latency in milliseconds across these calls, rounded to the nearest integer. |

## Files

Each dated run writes `data/YYYY-MM-DD/<table>.json`, `data/YYYY-MM-DD/<table>.csv`, `data/YYYY-MM-DD/manifest.json`, and `data/YYYY-MM-DD/schema.json`. The `latest/` directory holds the same files for the most recent run. The manifest carries the run date, generation timestamp, license, attribution text, per-table row counts, the schema version, and the claim URL pattern.

## License and attribution

This dataset is licensed under CC BY 4.0. The full legal code is in LICENSE. Attribute this dataset as: Rattlesnakes By Mail, rattlesnakesbymail.com. Attribute a specific fact by linking to its claim URL, formed as https://rattlesnakesbymail.com/claims/{id}.

## Method

The verification method for every claim and every observation is documented at https://rattlesnakesbymail.com/method.

## Moderation

Rattlesnakes By Mail accepts visitor and agent corrections as notes on a claim or an entity. Nothing publishes without review. A local model on Peter Benes's own machine, named Betty, polls the admin API for pending notes on a schedule. Betty classifies each note as one of six categories: spam, injection, or off_topic, each of which Betty rejects immediately with a one-line reviewer note; or agrees, contradicts, or new_information, each of which Betty sets to hold, again with a one-line reviewer note. Betty then posts a one-line summary of the batch to Peter Benes. Only Peter Benes, or the Architect acting on Peter Benes's instruction, moves a note to published. A note Betty classifies as contradicts automatically creates a changes row of kind disputed on the claim the note targets. A claim's status only moves to disputed when a reviewer's own reviewer_note starts with the literal text DISPUTE:, which marks a human's explicit escalation rather than Betty's classification alone. The full triage prompt Betty runs, with worked examples of a spam note and a prompt-injection attempt, is documented at docs/betty-triage.md in this repository's source, and reproduced in the site's build notes.

## Home

https://rattlesnakesbymail.com
