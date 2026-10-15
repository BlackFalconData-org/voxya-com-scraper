# Voxya Consumer Complaints Scraper

Extract structured data from [voxya.com](https://voxya.com) — voxya.com consumer complaints — the complete public feed of Indian consumer grievances, with title and full text, company named, amount involved and compensation claimed, desired resolution, status and filing dates for every complaint.

**[Voxya Consumer Complaints Scraper on Apify →](https://apify.com/blackfalcondata/voxya-com-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Detail enrichment** — Fetch full descriptions, structured metadata for each complaint.

**Incremental mode** — Only get new or changed complaints since your last run. Content hash per complaint — no duplicates, no re-processing.

**Change classification** — Track unchanged, cross-run duplicate detection across runs. Build audit trails of how complaints evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N complaints (up to 100.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every complaint returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured complaints from voxya.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor complaints, track trends, and analyze market dynamics with structured, deduplicated data from voxya.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per complaint is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "maxResults": 100,
  "descriptionMaxLength": 0,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all",
  "includeDetails": false
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `maxResults` | integer | `100` | Maximum number of complaints to return, newest first (0 = unlimited — the feed holds 170,000+ complaints). |
| `descriptionMaxLength` | integer | `0` | Truncate the complaint body to N characters. 0 = keep the full text. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state and return only new or changed complaints. stateKey is optional — defaults to a value derived from your start URLs. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked complaint universe. Leave empty to auto-generate it. |
| `emitUnchanged` | boolean | `false` | When incremental, also emit complaints that have not changed since the previous run. |
| `skipReposts` | boolean | `false` | When incremental, skip complaints whose text matches a complaint that disappeared in a prior run (the same grievance re-filed under a new id). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED complaints. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of complaints included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results. Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the complaint text to include. `all` keeps every variant; the others keep only the selected one. |
| `startUrls` | array | — | Optional. The complaint feed URL, https://voxya.com/consumer-complaints, optionally with ?page=N to begin at page N (10 complaints per page). Other voxya.com URLs are not supported. Leave empty to start at the newest complaint. |
| `includeDetails` | boolean | `false` | Every complaint already carries its full text in the feed, so this option adds no further fields. Leave off. |

---

## Output fields

Every complaint returns the same 32-field schema. Missing values are `null` — never omitted.

- `listingId`
- `complaintId`
- `url`
- `portalUrl`
- `title`
- `description`
- `companyName`
- `companyId`
- `complainantName`
- `amountInvolved`
- `compensationClaimed`
- `currency`
- `desiredResolution`
- `status`
- `complaintStage`
- `lawyerAssigned`
- `category`
- `subcategory`
- `district`
- `state`
- `country`
- `evidenceImage`
- `audioFile`
- `postedAt`
- `updatedAt`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per complaint. Here is a real example from a production run:

```json
{
  "listingId": "5e81f57cee5e716d69fd10f12a64bdae2cdf4c7f30023c483339ce4eda68390f",
  "complaintId": "274010",
  "url": "https://voxya.com/consumer-complaints/package-not-delivered-for-which-i-fully-paid-through-gpay/274010",
  "portalUrl": "https://voxya.com/consumer-complaints/package-not-delivered-for-which-i-fully-paid-through-gpay/274010",
  "title": "Package not delivered for which I fully paid through GPay",
  "description": "I placed and order on 28th September 2026 from shadesltd now renamed shadoowinc on Instagram for a set of *sets of clothes in which they offer ₹****+*** for express delivery. i pai…",
  "companyName": "Shadoowinc at Instagram",
  "companyId": 1288034,
  "complainantName": "Himanshi Narang",
  "amountInvolved": 4999,
  "compensationClaimed": null,
  "currency": "INR"
}
```

*Truncated — full records contain 32 fields. See Output fields for the complete schema.*

**[Try Voxya Consumer Complaints Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/voxya-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.000014 |

See the [actor on Apify](https://apify.com/blackfalcondata/voxya-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape voxya.com?**
Use this actor on Apify to extract structured data from voxya.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get voxya.com data as JSON, CSV, or Excel?**
The actor writes each complaint to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape voxya.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check voxya.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each complaint gets a content hash. On subsequent runs, only new or changed complaints are emitted — saving time, compute, and storage. Expired complaints can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

- [Djinni Scraper](https://apify.com/blackfalcondata/djinni-scraper?fpr=1h3gvi) — Ukraine & CEE tech jobs with salary and English level
- [Work.ua Scraper](https://apify.com/blackfalcondata/work-ua-scraper?fpr=1h3gvi) — Ukraine's work.ua job listings
- [NoFluffJobs Scraper](https://apify.com/blackfalcondata/nofluffjobs-scraper?fpr=1h3gvi) — Poland & CEE IT jobs with transparent salary ranges
- [OLX Jobs Scraper](https://apify.com/blackfalcondata/olx-jobs-scraper?fpr=1h3gvi) — OLX job listings across 7 CEE countries
- [Prace.cz Scraper](https://apify.com/blackfalcondata/prace-scraper?fpr=1h3gvi) — Czech Republic's prace.cz job board
- [Profession.hu Scraper](https://apify.com/blackfalcondata/profession-scraper?fpr=1h3gvi) — Hungary's profession.hu job board

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Voxya Consumer Complaints Scraper](https://apify.com/blackfalcondata/voxya-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
