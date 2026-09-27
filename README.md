# Similarweb Scraper: Website Traffic & Rank, No Login

Scrape Similarweb's free public website overview for any domain: estimated global and category rank, monthly visits with month-over-month change, engagement metrics, a traffic-source split across direct, search, social, referral, paid, and mail, top countries by traffic share, and a list of similar sites. No login, no paid Similarweb seat.

**Run it on Apify:** [apify.com/themineworks/similarweb-scraper](https://apify.com/themineworks/similarweb-scraper)
**Docs, FAQ and pricing:** [themineworks.com/actors/similarweb-scraper](https://themineworks.com/actors/similarweb-scraper/)

**Price:** $2.00 per 1,000 domains on Apify's free plan, down to $1.50 on higher plans, plus a $0.01 start fee per run. Failed and empty results are never charged.

## What it returns

* Global, category, and country rank per domain
* Estimated monthly visits with month-over-month change
* Engagement: bounce rate, pages per visit, avg. visit duration
* Traffic-source split: direct, search, social, referral, paid, mail
* Top countries and similar/competitor sites
* Zero charge on blocked or empty lookups

## Quick start

You need a free [Apify account](https://console.apify.com/sign-up) and its API token (Settings, API & Integrations).

### Python

```bash
pip install apify-client
```

```python
from apify_client import ApifyClient

client = ApifyClient("YOUR_APIFY_TOKEN")
run = client.actor("themineworks/similarweb-scraper").call(run_input={
    "domains": [
        "wikipedia.org"
    ],
    "maxRetriesPerDomain": 3
})

for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(item)
```

### Node.js

```bash
npm install apify-client
```

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: 'YOUR_APIFY_TOKEN' });
const run = await client.actor('themineworks/similarweb-scraper').call({
    "domains": [
        "wikipedia.org"
    ],
    "maxRetriesPerDomain": 3
});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items);
```

### cURL

One request that runs the actor and returns the results in the response (for runs under 5 minutes):

```bash
curl -X POST "https://api.apify.com/v2/acts/themineworks~similarweb-scraper/run-sync-get-dataset-items?token=YOUR_APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"domains": ["wikipedia.org"], "maxRetriesPerDomain": 3}'
```

### Command line

This repo includes ready-made clients that save results to JSON and CSV:

```bash
python3 similarweb_scraper.py --token YOUR_APIFY_TOKEN --domains "wikipedia.org" --max-retries-per-domain "3"
node similarweb_scraper.mjs --token YOUR_APIFY_TOKEN --domains "wikipedia.org" --max-retries-per-domain "3"
```

## Input

| Field | Type | Default | Description |
|---|---|---|---|
| `domains` (required) | array | `["wikipedia.org"]` | One or more root domains to look up on Similarweb's free public overview tool (for example "wikipedia.org") |
| `maxRetriesPerDomain` | integer | `3` | How many times to retry a domain with a fresh proxy session if the report doesn't fully load |

## Output

One row per result, as JSON, CSV, Excel or through the API.

| Field | Type | Description |
|---|---|---|
| `domain` | string | The domain that was looked up |
| `url` | string | Similarweb overview URL for this domain |
| `global_rank` | number | Similarweb global traffic rank |
| `category` | string | Similarweb category the site is classified under |
| `category_rank` | number | Rank within its Similarweb category |
| `country_rank` | number | Rank within its top country |
| `top_country` | string | The single top country by traffic share |
| `total_visits` | string | Estimated monthly visits (formatted, for example 4.2B) |
| `visits_change_pct` | string | Month-over-month change in total visits, as shown on the overview page |
| `bounce_rate_pct` | number | Average bounce rate percentage |
| `pages_per_visit` | number | Average pages viewed per visit |
| `avg_visit_duration` | string | Average visit duration (for example 00:06:12) |
| `traffic_sources` | object | Traffic-source breakdown percentages, keyed by the channels Similarweb reports (for example organic, direct… |
| `top_countries` | array | Top countries by traffic share: [{ country, share_pct }] |
| `similar_sites` | array | Similar / competitor sites Similarweb lists for this domain |
| `checked_at` | string | ISO-8601 timestamp when this record was scraped |

## Use it from an AI agent

The actor works as a tool in Claude, Cursor or any MCP client through Apify's MCP server:

```
https://mcp.apify.com/?tools=themineworks/similarweb-scraper
```

## FAQ

### Do I need a Similarweb account?

No. This reads Similarweb's own free public overview page for any domain, the same page anyone can open in a browser. No login, no paid Similarweb plan.

### How current is the traffic data?

Similarweb's overview reflects its own estimation model, refreshed on Similarweb's schedule, generally on a rolling monthly basis. This actor returns whatever is currently published, plus a timestamp of when the check ran.

### Can I check many domains in one run?

Yes. Pass a list of domains and each is checked independently. Only domains with a real report resolve; the rest are skipped and never charged.

### What's included in the traffic-source split?

Direct, organic search, paid search, social, referral, mail, and display, shown as a percentage split of estimated traffic, when Similarweb has published a report for that domain.

### What does it cost?

Pay-per-event at $2 per 1,000 domains scraped ($0.002 each) plus a small per-run start fee. Blocked or empty lookups are never charged.

### Can I export the results to CSV or Excel?

Yes. Every run saves to an Apify dataset you can download as JSON, CSV, Excel or XML, or read through the API. The Python and Node clients in this repo also write the results to local files.

### Can I run it on a schedule?

Yes. Save your input as a task on Apify and attach a schedule, or call the API from your own cron job. Scheduled runs are billed the same way as manual ones.

## Related scrapers

* [Semrush Authority Score Scraper](https://themineworks.com/actors/semrush-scraper/): Authority score, backlinks, and traffic, no Semrush account
* [ASO Keyword Rank Tracker](https://themineworks.com/actors/aso-keyword-rank-tracker/): App Store and Google Play rank tracking, one dataset
* [Backlink Building Agent](https://themineworks.com/actors/backlink-building-agent/): 3-stage pipeline: discover, contact, draft. Pay per outcome

Part of [The Mine Works](https://themineworks.com/): 151 pay-per-result scrapers with no login and no browser setup on your side.

## License

MIT © The Mine Works
