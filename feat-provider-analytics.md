---
icon: chart-line
---

Provider analytics is the operational dashboard where you read API traffic across every API your organisation has published. Use it to answer daily usage questions and to triage incidents without leaving the marketplace. Every panel derives from the same per-request log your gateway connections report back, so the numbers across tiles, charts, and tables stay internally consistent.

![Figure. The provider analytics dashboard with summary tiles, the time-series chart, the status-code breakdown, and recent requests.](.gitbook/assets/screenshots/provider/admin-portal-analytics.png)

## What you see

The page header reads **Analytics**, with the main panel heading **API Usage Analytics**. Everything below that heading reflects the selected time range:

- **Summary tiles**: four tiles across the top row, each a single number for the selected range. **Total Calls** is the request count, **Success Rate** the proportion of `2xx` against the total, **Error Rate** the proportion of `4xx` and `5xx` combined, and **Average Latency** the mean response time in milliseconds.
- **API Calls Over Time**: a request-volume time-series chart. The Y-axis is request count per bucket; the X-axis rebuckets to suit the range, with finer buckets for short presets and daily buckets for longer ones.
- **Status Codes**: traffic split by HTTP status family (`2xx`, `3xx`, `4xx`, `5xx`). Hover a segment to read the exact count.
- **Recent Requests**: a per-request log with columns for timestamp, method, path, status, latency, and consumer app. It is the only view that shows individual requests rather than aggregates.

![Figure. The four summary tiles at the top of the dashboard.](.gitbook/assets/screenshots/provider/monitor-summary-tiles.png)

## Options

The time range is the only control that scopes the page. A row of preset buttons sits at the top of the API Usage Analytics panel:

- **1H** and **6H**: incident triage.
- **24H**: release-day checks.
- **7D**: weekly reviews.
- **30D** and **90D**: monthly and quarterly summaries.
- **Custom**: an arbitrary window set by a From and a To date-time.

These are preset buttons, not dropdown filters. There are no per-API, status-code, or consumer dropdowns on this page; selecting a button re-queries every panel at once. Where a panel offers it, export the underlying request data for offline analysis.

## Configure

1. From the left sidebar, expand **API MANAGEMENT** and click **Analytics**.
2. At the top of the API Usage Analytics panel, choose a time-range preset: **1H**, **6H**, **24H**, **7D**, **30D**, **90D**, or **Custom**.
3. For an arbitrary window, click **Custom** and enter a From and a To date-time.
4. Read the summary tiles for an at-a-glance answer on volume and errors.
5. Read **API Calls Over Time** for the traffic curve, then **Status Codes** for the success-versus-error split.
6. Scan **Recent Requests** to identify individual failing or slow calls.
7. Export the request data where the panel offers it.

## Verify

- Every panel reflects the same selected range: change the preset and the tiles, chart, and tables update together.
- A confirmed read has a window, a status family, and the failing paths from Recent Requests behind any change.
- An environment with no recorded traffic renders each panel in its empty state, with identical layout once requests begin to flow. A Total Calls of zero with a 100 percent Success Rate means no traffic, not health.

{% hint style="success" %}
**Tip:** Switch between **24H** and **7D** to confirm whether a spike is unusual or routine variance.
**Result:** Every panel reflects the selected time range, giving one consistent view of traffic volume and health.
{% endhint %}