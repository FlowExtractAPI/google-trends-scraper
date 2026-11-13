## What does Google Trends Scraper do?

Google Trends Scraper collects Google Trends data with the **same choices as the Google Trends website**: the Explore page (interest over time, interest by region, related queries) and the Trending now page (what people are searching for right now). Compare up to 5 terms on one chart, narrow to a country, state or metro area, pick any time range from the past hour to 2004–present, filter by Google's full category list and search type, or simply paste a Google Trends URL.

Every period of a comparison comes back as **one row with all terms side by side** — the same layout as the site's own CSV download — plus a **summary row per search** with each term's average, peak, latest value, recent momentum and the leader.

### What data can you extract from Google Trends?

| Data | What you get |
|---|---|
| 📈 Interest over time | One row per period (hour, day, week or month) with a 0–100 value for every compared term, the period label, and whether the period is still filling |
| 🗺️ Interest by region | One row per country, subregion, metro or city: each term's share of the comparison **and** each term's own 0–100 interest, city coordinates, the leading term |
| 🔗 Related queries | Top and Rising related searches per term with value, "Breakout" flag, a link to explore them, and a flag for queries that don't mention the term |
| 🧩 Topic suggestions | The topics Google matches each term to (e.g. *Tesla* → automobile make, company or band), with the id to search the topic itself |
| 🧾 Search summary | Average, peak (value and date), latest value, change over the last 4 periods, rising / stable / falling, and the leading term |
| 🔥 Trending now | Every trending search for a location and window: search volume, % increase, start and end time, active or not, categories, the full trend breakdown, and news articles with pictures for the trends Google covers in its news feed |

### Why use Google Trends Scraper on Apify?

- **Mirrors the site.** Every dropdown uses Google Trends' own options and labels: 250 locations plus subregions and metros, 1,100+ categories, 5 search types, 10 time ranges, 125 Trending now countries and their regions.
- **Comparisons stay comparable.** Terms on one line share one 0–100 scale, exactly like "Add comparison" on the site.
- **Fast.** A 3-term comparison with interest over time, regions and related queries finishes in about 5 seconds.
- **Bulk.** Run hundreds of comparisons and many Trending now locations in one run; each line is independent.
- **Paste any URL.** Set up a view on trends.google.com and paste its address — including views that compare one term across countries or across two date ranges.
- **Analysis-ready.** Search summaries, region rows that combine share and interest, and an off-topic flag on related queries save you a cleaning step.

## How to scrape Google Trends

1. Open the Actor and type your searches in **Search terms** — one comparison per line, terms separated by commas (e.g. `chatgpt, gemini, claude`).
2. Pick a **Location** (and optionally a **Subregion or metro**), a **Time range**, a **Category** and a **Search type**, like on the Explore page.
3. Choose **What to collect**.
4. For trending searches, pick one or more **Trending locations**, a window, a category and the sort order.
5. Or skip the form and paste **Google Trends URLs**.
6. Click **Start** and download the dataset as JSON, CSV, Excel or HTML, or read it over the API.

## How much does it cost to scrape Google Trends?

You pay per result delivered: one price per Explore search (however many charts and rows it returns) and one per Trending now search. Notices, searches Google has no data for and searches that could not be completed are never charged.

| Event | FREE | BRONZE | SILVER | GOLD |
|---|---|---|---|---|
| Explore search (up to 5 compared terms, every chart you select) | $0.0050 | $0.0042 | $0.0034 | $0.0026 |
| Trending now search | $0.0006 | $0.00048 | $0.00036 | $0.00024 |

- A **5-term comparison** with interest over time, regions, related queries and the summary is one Explore search: **$0.005**, or **$0.0026** on GOLD — about 190 rows of data.
- **100 comparisons** cost **$0.50** at the highest tier price and **$0.26** on GOLD.
- **1,000 trending searches** cost **$0.60**, or **$0.24** on GOLD. No start fee.
- Set a maximum cost per run in Apify and the Actor stops cleanly when it is reached.

## Input and output examples

### Input

Compare three AI assistants in the US over 12 months and get today's trends in the US and the UK:

```json
{
  "searchTerms": ["chatgpt, gemini, claude"],
  "location": "US",
  "timeRange": "past12Months",
  "dataTypes": ["interestOverTime", "interestByRegion", "relatedQueries"],
  "trendingLocations": ["US", "GB"],
  "trendingTimeRange": "past24Hours",
  "maxTrendsPerLocation": 50
}
```

A metro area and a custom date range, with city-level breakdown:

```json
{
  "searchTerms": ["pizza, burger"],
  "location": "US",
  "subregion": "New York NY",
  "timeRange": "custom",
  "customStartDate": "2025-01-01",
  "customEndDate": "2025-06-30",
  "regionResolution": "city"
}
```

Paste URLs instead (each keeps its own settings):

```json
{
  "startUrls": [
    { "url": "https://trends.google.com/trends/explore?q=coffee,coffee&geo=US,GB&date=today%203-m" },
    { "url": "https://trends.google.com/trending?geo=JP&hours=168&category=17&sort=search-volume" }
  ]
}
```

### Output

Every row has a `type`. The dataset views (Interest over time, Interest by region, Related queries, Search summaries, Trending now, Notices) show the columns that matter for each.

### Interest over time record

```json
{
  "type": "interest_over_time",
  "search_id": "s1",
  "search_terms": ["chatgpt", "gemini", "claude"],
  "location": "US",
  "location_name": "United States",
  "time_range": "Past 12 months",
  "category_name": "All categories",
  "search_type": "Web Search",
  "date": "2026-11-08",
  "date_label": "Nov 8 - 14, 2026",
  "is_partial": true,
  "resolution": "WEEK",
  "values": { "chatgpt": 83, "gemini": 38, "claude": 27 },
  "source_url": "https://trends.google.com/trends/explore?q=chatgpt%2Cgemini%2Cclaude&geo=US&date=today%2012-m"
}
```

### Search summary record

```json
{
  "type": "search_summary",
  "search_terms": ["chatgpt", "gemini", "claude"],
  "average_interest": { "chatgpt": 79, "gemini": 30, "claude": 21 },
  "latest": { "chatgpt": { "value": 82, "date": "2026-11-01" }, "gemini": { "value": 33, "date": "2026-11-01" }, "claude": { "value": 25, "date": "2026-11-01" } },
  "recent_change_pct": { "chatgpt": 8.6, "gemini": -1.4, "claude": 14 },
  "trend_direction": { "chatgpt": "stable", "gemini": "stable", "claude": "rising" },
  "leader": "chatgpt"
}
```

### Interest by region record

```json
{
  "type": "interest_by_region",
  "region_code": "US-CA",
  "region_name": "California",
  "resolution": "REGION",
  "values": { "chatgpt": 55, "gemini": 25, "claude": 20 },
  "value_kind": "share_of_comparison",
  "top_term": "chatgpt",
  "interest": { "chatgpt": 100, "gemini": 100, "claude": 90 }
}
```

`values` is each term's share of the region's searches for the comparison (the site's "Compared breakdown"); `interest` is each term's own 0–100 interest in that region (the site's per-term maps).

### Related query record

```json
{
  "type": "related_query",
  "search_term": "bitcoin",
  "rank_type": "rising",
  "rank": 2,
  "query": "how to buy bitcoin safely",
  "value": 7650,
  "formatted_value": "Breakout",
  "is_breakout": true,
  "mentions_search_term": true,
  "explore_url": "https://trends.google.com/trends/explore?q=how+to+buy+bitcoin+safely&date=today+12-m&geo=US"
}
```

### Trending search record

```json
{
  "type": "trending_search",
  "rank": 1,
  "title": "microsoft outage",
  "location": "US",
  "location_name": "United States",
  "time_range": "Past 24 hours",
  "search_volume": 10000000,
  "search_volume_label": "10M+",
  "increase_pct": 1000,
  "started_at": "2026-11-12T17:50:00Z",
  "ended_at": null,
  "is_active": true,
  "duration_hours": 20.7,
  "categories": ["Technology"],
  "trend_breakdown": ["microsoft outage", "outlook outage", "is outlook still down", "..."],
  "trend_breakdown_count": 518,
  "news_articles": [],
  "explore_url": "https://trends.google.com/trends/explore?q=microsoft%20outage&geo=US&date=now%201-d"
}
```

## Other Actors you might need

- [YouTube Shorts Trend Finder](https://apify.com/dz_omar/youtube-shorts-trend-finder?fpr=smcx63) — find trending YouTube Shorts by keyword.
- [Bing Copilot AI Mode Scraper](https://apify.com/dz_omar/bing-copilot-ai-mode-scraper?fpr=smcx63) — collect AI answers and their sources for your keywords.

## Frequently asked questions

### Is it legal to scrape Google Trends?

Google Trends data is public and aggregated — it contains no personal data. As with any data source, check that your use complies with Google's terms and the laws that apply to you.

### Do I need a Google account?

No. Nothing to log in to, no API key.

### Why are the numbers 0–100 and not search counts?

That is how Google Trends reports interest: 100 is the peak for the request, everything else is relative to it. Terms compared on one line share one scale, so you can compare them; values from different lines or runs are on different scales. Trending now rows do carry Google's approximate search volume (e.g. 10M+).

### Why don't I get related topics?

Google Trends only shows related topics to signed-in visitors. Use **Topic suggestions** to see which topics Google matches your terms to, and search a topic by putting its id (e.g. `/m/02vqfm`) in Search terms.

### Some Rising related queries look unrelated to my term. Why?

Google sometimes adds generic searches to Rising lists (for example "gardening ideas" under *coffee*). Each related query carries `mentions_search_term`; filter on `true` to keep only queries that contain your term's words. Related brands (like *openai* for *chatgpt*) are `false` too, so review before dropping.

### Why do only some trending searches have news articles?

Google publishes news articles and pictures for about 10 of the freshest trends per location. Every other trend still has its volume, growth, timing, categories and full breakdown.

### Can I use Google Trends Scraper in Make, Zapier, n8n or my own code?

Yes. Use the Apify integrations, or call the Actor over the Apify API from any language and read the dataset when it finishes. Schedules let you build your own trend history.

### What happens if a run is interrupted?

The run remembers which searches and locations it already delivered and continues with the rest, so nothing is delivered twice. A run close to its time limit stops itself, keeps everything delivered so far and leaves a notice.

### Why did my run return fewer results than I asked for?

| Situation | What you see | What to do |
|---|---|---|
| Google has too little data for a term, location or period | A notice row "not enough data" | Try a broader location, a longer time range or Web Search |
| Google did not answer for one search after several attempts | A notice row for that search; the rest of the run is complete | Run that search again later |
| A line has more than 5 terms, or an unknown subregion or URL | A notice row explaining what to change | Fix the line and run again |
| Few trends match your Trending now filters | A notice row | Widen the window, pick All categories, or turn off "Show active trends only" |

## Support

- 🌐 **Website:** [flowextractapi.com](https://flowextractapi.com)
- 📧 **Email:** [flowextractapi@outlook.com](mailto:flowextractapi@outlook.com)
- 💬 **GitHub:** [FlowExtractAPI](https://github.com/FlowExtractAPI)
- 💼 **LinkedIn:** [flowextract-api](https://www.linkedin.com/in/flowextract-api/)
- 🐦 **X (Twitter):** [@FlowExtractAPI](https://x.com/FlowExtractAPI)
- 📱 **Facebook:** [flowextractapi](https://www.facebook.com/flowextractapi)
- 🎵 **TikTok:** [@flowextractapi](https://www.tiktok.com/@flowextractapi)

## Legal & compliance

- **Public data only** — processes publicly available, aggregated data.
- **Rate limiting** — respects source-service limits and terms of use.
- **Data protection** — no storage of personal information or unauthorized data collection.
- **Commercial use** — suitable for business intelligence and research.
- No affiliation with or endorsement by Google is implied.

---

*Google Trends Scraper — by FlowExtract API. Turn any website into structured data.*
