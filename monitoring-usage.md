---
icon: chart-line
---

Once APIs are live and consumers are calling them, you need a single place to see how the traffic is shaped. The marketplace ships a built-in **Provider Analytics** dashboard that aggregates request volume, latency, status families, and per-API and per-consumer breakdowns into one operational view. This chapter walks every tile, every chart, every filter, and every drilldown on that page, and then covers the export flow, the comparison view, and the two anomaly-investigation paths providers use most often.

You will learn:

- How to open the Provider Analytics page and read the summary tiles, the time-series chart, the status-code breakdown, the top-APIs strip, and the recent-requests log.
- How to use the time-range picker, including the preset ranges and the custom date-time picker, and how to compare two periods side by side.
- How to filter by API, by HTTP status family, by consumer organisation, and by gateway environment, and how those filters compound.
- How to drill from the overall view into a single API's traffic, then into a single consumer's call volume against that API.
- How to export the underlying time-series and request-log data to CSV, and how to recognise an anomaly worth investigating.
- How to configure outbound webhooks so anomaly events alert you without watching the dashboard.

Allow ~30 minutes to walk the page end to end, run two filter combinations, and configure your first webhook.

## The Provider Analytics dashboard

Provider Analytics is the operational home for every API Provider. It is where you answer day-to-day usage questions ("who is calling my Search API today?"), incident questions ("did the 5xx rate spike after yesterday's release?"), and product questions ("which APIs are getting any traction at all?") without leaving the marketplace UI. Every chart on the page is derived from the same source, the per-request log your gateway connections report back, so the numbers across tiles, charts, and tables are always internally consistent.

#### Open the Provider Analytics page

Open the dashboard whenever you need a current snapshot of API traffic across every API your organisation has published.

#### Before you start

- **Publish at least one API.** The dashboard only shows traffic that has flowed through your gateway connections. If you have not yet completed the Importing your first API and Publishing your first API chapters, every chart renders empty.
- **Confirm at least one gateway connection is healthy.** The marketplace plots whatever your connection reports back. A connection that failed its last sync produces a stale or empty view. The Test connection action lives on the Manage API Sources page, covered in the Connecting your first gateway chapter.
- **Know the question you are answering.** A weekly traffic review uses different filters from an incident triage. Naming the question before you arrive saves five minutes of poking.

To open the Provider Analytics dashboard:

1. From the left sidebar, expand **API MANAGEMENT** and click **Analytics**. The page opens at `/admin/portal/analytics`.
2. The page header reads **Analytics**, with the main panel heading **API Usage Analytics**.
3. Read the four summary tiles across the top row. Each tile shows a single number for the selected time range: total calls, success rate, error rate, and average latency.
4. Scroll to **API Calls Over Time** for the request-volume time-series chart.
5. Scroll past that to **Status Codes** for the status-family breakdown, **Top APIs** for the per-API ranked strip, and **Recent Requests** for the live request log.

![Figure 9-1. The Provider Analytics dashboard with the four summary tiles, the time-series chart, the status-code breakdown, and the recent-requests table.](.gitbook/assets/screenshots/provider/admin-portal-analytics.png)

The numbered callouts in Figure 9-1 are:

1. **Page title and breadcrumb**. Reads **Analytics** with the breadcrumb Home > Administration > Analytics above it. Confirms the page is the provider-side dashboard, not a consumer analytics page.
2. **API Usage Analytics panel**. The main panel that holds every chart and table. Everything below this heading reflects the current filter selections.
3. **Summary tiles row**. Four tiles: total calls, success rate, error rate, average latency. Each tile updates atomically whenever the filters change.
4. **API Calls Over Time chart**. A time-series line chart of total request volume. The X-axis buckets scale with the selected time range; the Y-axis shows request count per bucket.
5. **Status Codes panel**. A bar or donut chart breaking traffic down by HTTP status family: `2xx`, `3xx`, `4xx`, `5xx`. Hover any segment to see the exact count.
6. **Recent Requests table**. A scrollable table of the most recent individual requests with columns for timestamp, method, path, status, latency, and consumer app. Click any row to drill into a single request.

{% hint style="success" %}
**Result:** Every chart on the page reflects the current time range and filter selections. The four summary tiles answer the first question ("how much traffic, how many errors") at a glance.
{% endhint %}

{% hint style="info" %}
**Note:** The dashboard reflects the organisation scope shown in the top bar. Providers belonging to more than one organisation should switch organisation in the top-right selector before reading the numbers; the same API can show different totals when scoped to different organisations.
{% endhint %}

{% hint style="success" %}
**Tip:** Bookmark `/admin/portal/analytics` directly. The page is a frequent destination and the marketplace deep-links into it cleanly, including with filter parameters in the URL.
{% endhint %}

#### Read the four summary tiles

Use this task on every visit to the dashboard. The tiles are the fastest read on whether something needs attention or whether the day's traffic is routine.

To read the tiles:

1. From the dashboard, look at the four tiles across the top of the API Usage Analytics panel.
2. Read **Total Calls** first. This is the request count for the current time range and filter selection. A weekly review against the same range last week tells you whether traffic is growing.
3. Read **Success Rate** next. This is the proportion of `2xx` responses against the total. A healthy production API sits above 99 percent; a number below 95 percent during business hours warrants a drilldown.
4. Read **Error Rate**. This is the proportion of `4xx` and `5xx` responses combined. Treat sudden growth as an incident signal: a `4xx` spike points at consumers, a `5xx` spike points at your APIs.
5. Read **Average Latency**. This is the mean response time in milliseconds across the selected range. Compare against your published SLA; a number drifting upward across the week often precedes a sustained 5xx spike.

{% hint style="success" %}
**Tip:** The tiles share their filter context with every chart below them. Adjust the time range or the API filter and the tile values change in step with the charts; you never need to reconcile two views.
{% endhint %}

{% hint style="info" %}
**Note:** Average latency is a mean, not a percentile. For latency-sensitive APIs, follow up with the Recent Requests table sorted by Latency to find the slow outliers; the mean alone can hide a long tail.
{% endhint %}

{% hint style="warning" %}
**Caution:** A success rate of 100 percent and a total-call count of zero is not "healthy", it is "no traffic". Always check Total Calls before reacting to Success Rate.
{% endhint %}

## Filtering the dashboard

The default view loads the most recent traffic across every API, every consumer, and every gateway connection. Narrow the view to answer a specific question. Filters compound: select a time range, an API, and a status family together and the dashboard shows only the intersection.

#### Filter by time range

Change the time range to switch between an incident-triage view, a release-day check, a weekly trend, and a monthly board update.

#### Before you start

- **Pick the range that matches the question.** Last hour is for incident triage; Last 24 hours is for release-day checks; Last 7 days is for weekly reviews; Last 30 days is for monthly summaries. Custom ranges cover everything else.
- **Note the data retention cap.** The marketplace retains a finite window of analytics data. Custom ranges reaching further back than retention allows return what data is held and report the cap in a banner.

To filter by time range:

1. At the top of the API Usage Analytics panel, locate the **Time Range** selector.
2. Click the selector to open the range options.
3. Pick one of the presets: **Last hour**, **Last 24 hours**, **Last 7 days**, **Last 30 days**.
4. Or pick **Custom range** to open the date-time picker. Enter a From date-time and a To date-time, then click **Apply**.
5. Wait for the charts to refresh. Every panel re-queries against the new range: the summary tiles, the time-series chart, the status-code panel, the top-APIs strip, and the recent-requests table.

The numbered callouts on the time-range selector are:

1. **Preset list**. Last hour, Last 24 hours, Last 7 days, Last 30 days. The presets are the fastest path to the right view; click and the page refreshes immediately.
2. **Custom range**. Opens two date-time pickers and an **Apply** button. Use Custom range when an incident window or a release window does not align with a preset.
3. **Current selection label**. The selector face shows the currently applied range. Read this before sharing a screenshot so the timeframe is obvious.

{% hint style="success" %}
**Result:** The dashboard renders for the selected range. The X-axis on **API Calls Over Time** rebuckets to suit the range (5-minute buckets for Last hour, hourly for Last 24 hours, daily for Last 7 days, daily for Last 30 days).
{% endhint %}

{% hint style="info" %}
**Note:** Time ranges are evaluated in your browser's local timezone. Teams that work across regions should mention the timezone explicitly when sharing screenshots or numbers.
{% endhint %}

{% hint style="success" %}
**Tip:** When investigating a spike, zoom out first and zoom in second. A Last 24 hours view confirms whether the spike is unusual; a custom hour-wide range tells you when it started.
{% endhint %}

#### Filter by HTTP status code

Narrow the view to a single status family to confirm an incident is contained or that a fix has driven errors back down.

To filter by status code:

1. With the dashboard open, locate the **Status Code** filter beside the time-range selector.
2. Click the filter to open the status options.
3. Pick a family: **2xx**, **3xx**, **4xx**, **5xx**. Or pick a specific code from the **Specific codes** sublist: `400`, `401`, `403`, `404`, `429`, `500`, `502`, `503`, `504`.
4. Wait for the charts to refresh. **API Calls Over Time** plots only the matching requests; **Status Codes** collapses to a single segment; **Recent Requests** lists only the matching log entries.
5. To clear, click the filter again and pick **All status codes**.

{% hint style="success" %}
**Result:** The dashboard renders only the slice of traffic matching the selected status. The summary tiles update too: Total Calls falls to match the filter, and Error Rate and Success Rate become tautological (set Error Rate to 100 percent by filtering to `5xx`, for example).
{% endhint %}

{% hint style="info" %}
**Note:** A `4xx` filter shows requests the consumer got wrong: bad input, missing auth, rate limits. A `5xx` filter shows requests your APIs got wrong: server errors, gateway timeouts, upstream failures. Examine both during an incident to identify which side to fix.
{% endhint %}

{% hint style="success" %}
**Tip:** Set the status filter to `429` for a focused view of rate-limit hits. A growing `429` count often points at a consumer that needs a higher quota or a plan upgrade, covered in the Designing API Products and plans chapter.
{% endhint %}

#### Filter by API

Narrow to a single API when an incident is suspected to be contained to one entry in the catalog, or when you want to read traffic for one product in isolation.

To filter by API:

1. With the dashboard open, locate the **API** filter at the top of the API Usage Analytics panel.
2. Click the filter to open the API list. The list shows every API your organisation has published, sorted alphabetically.
3. Type into the search box to narrow the list.
4. Pick one API to filter the dashboard. The selection appears as a chip beside the filter.
5. The dashboard refreshes. All charts and tables now reflect only the selected API's traffic.
6. To clear, click the chip's **x** or pick **All APIs** from the dropdown.

{% hint style="success" %}
**Result:** The dashboard answers "how is this one API performing right now". Compare its Error Rate against the org-wide Error Rate to see whether this API is the source of an org-wide spike.
{% endhint %}

{% hint style="success" %}
**Tip:** Filtering by API combined with a `5xx` status filter is the fastest way to confirm a single API is the source of an incident. If the org-wide error rate drops back to baseline after applying the filter, the suspected API is indeed the culprit.
{% endhint %}

#### Filter by consumer organisation

Narrow to a single consumer organisation to read how one customer is calling your APIs. The view answers "who is hammering my Search API at 3 a.m.".

To filter by consumer organisation:

1. With the dashboard open, locate the **Consumer Org** filter.
2. Click the filter to open the consumer list. The list shows every organisation that holds an approved subscription against your APIs.
3. Type into the search box to narrow.
4. Pick one consumer organisation.
5. The dashboard refreshes to show only that organisation's calls.

{% hint style="success" %}
**Result:** The dashboard shows the consumer's traffic against your APIs. Combine with a per-API filter to answer "how is Acme Corp calling my Payments API" in one view.
{% endhint %}

{% hint style="info" %}
**Note:** Consumer filtering respects the organisation scope in the top bar. The filter only lists organisations that hold approved subscriptions against the currently scoped organisation's APIs.
{% endhint %}

#### Combine multiple filters

Filters compound. Use a combination when one filter alone returns too much noise.

To combine filters:

1. Apply a time range first to bound the data set.
2. Apply an API filter to scope to one product.
3. Apply a Status Code filter to scope to one outcome family.
4. Apply a Consumer Org filter to scope to one customer.
5. Read the resulting view. The tiles, charts, and tables all reflect the intersection.
6. Watch the **Active filters** chip strip beneath the filter row to see which filters are currently applied.

{% hint style="success" %}
**Result:** The intersection of your filters is the sharpest signal the dashboard can give. Use it to answer narrowly-scoped questions like "how many `429` responses did Acme Corp see against my Payments API in the last hour".
{% endhint %}

{% hint style="success" %}
**Tip:** Click the **Clear all** action above the chip strip to drop every filter at once. The view returns to the org-wide, all-status, all-consumer default for the currently selected time range.
{% endhint %}

{% hint style="warning" %}
**Caution:** Aggressive filter combinations can return an empty chart. The empty state is informative ("No requests match the current filters"), not an error. Drop one filter at a time to find which constraint is hiding the rows.
{% endhint %}

## Reading the charts

Once filters are set, the four charts in the API Usage Analytics panel hold the answers. Each chart reads independently, but they share the same source data, so cross-checks between them are always consistent.

#### Read the API Calls Over Time chart

Use this chart on every visit. The shape of the line is the single most important read on whether traffic is healthy.

To read the chart:

1. Scroll to **API Calls Over Time** in the API Usage Analytics panel.
2. Read the Y-axis label (typically "Requests per <bucket>") and the X-axis (timestamps).
3. Hover any point on the line. The tooltip shows the exact timestamp, bucket size, and request count.
4. Look for visible patterns:
   - A steady baseline with predictable daily cycles is healthy.
   - A sudden vertical spike is an incident or a launch.
   - A sustained drop is an outage on your APIs, a consumer outage, or a missed sync.
5. Click the legend entry (where present) to toggle the line series.

{% hint style="success" %}
**Result:** You can read the request-volume curve at a glance. The chart is the canonical "is anything happening" view.
{% endhint %}

{% hint style="success" %}
**Tip:** Combine this chart with a Custom time range to investigate any visible spike or dip. Set the From and To dates to a tight window around the anomaly and the chart redraws at a finer bucket size, revealing the minute when the change began.
{% endhint %}

#### Read the Status Codes panel

Use this panel as the second pass after API Calls Over Time. Volume tells you how much traffic; status codes tell you whether that traffic succeeded.

To read the panel:

1. Scroll to **Status Codes**.
2. The panel renders as either a stacked bar chart or a donut chart, depending on the time range. Each segment represents one status family.
3. Hover any segment. The tooltip shows the family (`2xx`, `4xx`, etc.), the count, and the percentage of the total.
4. Click any segment to apply that family as a status filter on the rest of the dashboard.

{% hint style="success" %}
**Result:** You can read the success-versus-error split at a glance. The panel is the canonical "is anything failing" view.
{% endhint %}

{% hint style="info" %}
**Note:** Click-to-filter on a segment is the fastest path from "the donut shows a big 4xx slice" to "show me only the 4xx requests". Both the chart and the Recent Requests table update.
{% endhint %}

{% hint style="success" %}
**Tip:** A growing `3xx` slice is often missed. The `3xx` family covers redirects and `304 Not Modified`. A spike often indicates a misconfigured client following redirects in a loop; investigate before it escalates to a `5xx` from elsewhere in your stack.
{% endhint %}

#### Read the Top APIs ranking

Use this panel for product questions. It answers "which APIs are getting any traction".

To read the ranking:

1. Scroll to **Top APIs**. The panel lists APIs ranked by request volume within the current filter context.
2. Read the columns: API title, Total calls, Error rate, Average latency.
3. Click a row to drill into that API's analytics, equivalent to applying the API filter.

{% hint style="success" %}
**Result:** You can see at a glance which APIs are pulling their weight and which sit idle. The ranking is the operational answer to "which APIs should I invest in next".
{% endhint %}

{% hint style="success" %}
**Tip:** Sort by Error rate descending to find the APIs that need attention first. A high error rate on a high-volume API is more urgent than the same rate on a low-volume one.
{% endhint %}

#### Read the Recent Requests table

Use this table for forensic work. It is the only view in the dashboard that shows individual requests rather than aggregates.

To read the table:

1. Scroll to **Recent Requests**.
2. Read the columns left to right: **Timestamp**, **Method**, **Path**, **Status**, **Latency**, **Consumer App**.
3. Sort by **Latency** descending to find slow outliers. Sort by **Status** to group failures together.
4. Click any row to open the request detail panel.
5. Use the **Page** controls beneath the table. Default page size is 25 rows; the per-page selector accepts 10, 25, 50, or 100.

The request detail panel shows:

- **Request line**. Method, full path, query string.
- **Headers**. Request and response headers, redacted for sensitive values.
- **Status and timing**. Status code, total latency, upstream latency.
- **Consumer**. App name, organisation name, subscription ID.
- **Trace ID**. The gateway-side trace identifier, useful for correlating with the gateway's own logs.

{% hint style="success" %}
**Result:** You have a per-request view that is enough to reproduce the call, identify the consumer, and correlate against gateway logs.
{% endhint %}

{% hint style="success" %}
**Tip:** During an incident, sort Recent Requests by Status descending. The top of the table becomes a live feed of every `5xx` returned in the selected time range.
{% endhint %}

## Drilling into a single API

The aggregate dashboard answers organisation-wide questions. When a question is about one API specifically, the per-API drilldown is the right view. It is the same chart set scoped to one API in the catalog, with extra panels for per-endpoint and per-consumer traffic.

#### Drill into one API's traffic

Use this task to read traffic for a single API in isolation. The drilldown answers "how is my Payments API doing right now".

To drill into one API:

1. From the Provider Analytics dashboard, scroll to the **Top APIs** panel.
2. Click the row for the API to drill into. The page navigates to `/admin/portal/analytics/api/<nid>`.
3. The drilldown opens with the same chart set as the org-wide view: summary tiles, API Calls Over Time, Status Codes, Recent Requests.
4. Additional panels appear below the standard set:
   - **Top Endpoints**. Request volume per OpenAPI operation. Useful for "which endpoint is hot".
   - **Top Consumers**. Request volume per consumer organisation against this API.
   - **Latency Distribution**. A histogram of response times, useful for spotting tail latency.

The numbered callouts in Figure 9-11 are:

1. **API header**. Shows the API title, version, and a link back to the API detail page.
2. **Summary tiles**. Scoped to this API. Total Calls, Success Rate, Error Rate, Average Latency.
3. **Top Endpoints panel**. Lists OpenAPI operations (method plus path) ranked by request volume against this API.
4. **Top Consumers panel**. Lists consumer organisations ranked by request volume against this API.
5. **Latency Distribution histogram**. Response-time buckets. The right tail tells you about your slow requests.
6. **Back to all APIs**. Returns to the org-wide Provider Analytics dashboard.

{% hint style="success" %}
**Result:** The drilldown answers every question scoped to one API. Combine with a time-range filter to focus on a particular incident window.
{% endhint %}

{% hint style="info" %}
**Note:** The drilldown inherits the time-range filter from the org-wide dashboard. Apply the time range first on the org-wide page, then drill in; or apply the time range again on the drilldown if you arrive directly via URL.
{% endhint %}

{% hint style="success" %}
**Tip:** Bookmark a drilldown URL. The `/admin/portal/analytics/api/<nid>` URL is stable across days, and the time-range parameter is preserved. The bookmark becomes a single-click "how is my Payments API today".
{% endhint %}

#### Look up a single consumer's call volume

Use this task to answer "how much is Acme Corp calling my Payments API". The task uses the per-API drilldown's Top Consumers panel as the entry point.

To look up a consumer's call volume:

1. Drill into the API in question, as above.
2. Scroll to the **Top Consumers** panel.
3. Locate the consumer organisation in the ranked list. Sort by Total calls descending if the list is long.
4. Click the consumer row to open a per-consumer view scoped to this API. The page navigates to `/admin/portal/analytics/api/<nid>/consumer/<org>`.
5. The per-consumer view shows the same chart set scoped to the intersection of this API and this consumer.

{% hint style="success" %}
**Result:** You have a per-consumer, per-API view: every chart on the page reflects calls from one consumer organisation against one API.
{% endhint %}

{% hint style="success" %}
**Tip:** This view is the fastest way to back up a billing or fair-use conversation with a specific consumer. Export the time-series for the period in question and share the CSV with your account manager.
{% endhint %}

{% hint style="info" %}
**Note:** A consumer organisation that does not appear in Top Consumers either has no approved subscription against the API or has not called it within the selected time range. Confirm subscription status in the Onboarding your first consumer workflow before assuming a missing entry is an error.
{% endhint %}

#### Read the per-endpoint view

Use this task to find which endpoint within an API is carrying the load, or which endpoint is returning errors.

To read per-endpoint traffic:

1. From the per-API drilldown, scroll to **Top Endpoints**.
2. The panel lists OpenAPI operations as **Method + Path** rows: `GET /v1/customers`, `POST /v1/payments`, and so on.
3. Read the columns: Total calls, Error rate, Average latency.
4. Click any endpoint row to filter the rest of the drilldown to that endpoint.
5. The summary tiles, time-series chart, status-codes panel, and recent-requests table all rescope to the chosen endpoint.

{% hint style="success" %}
**Result:** Per-endpoint scoping turns "the Payments API is slow" into "the `POST /v1/payments` endpoint is slow at the 95th percentile". The narrower question is the more useful one.
{% endhint %}

{% hint style="success" %}
**Tip:** Sort Top Endpoints by Error rate descending after applying a `5xx` filter. The intersection identifies the single endpoint causing the failures, often before the consumer reports it.
{% endhint %}

## Exporting and comparing

The dashboard answers most day-to-day questions on its own. When a number needs to leave the marketplace, export the underlying data. When the question is "is this period worse than the last one", use the comparison view.

#### Export the time-series data

Export the time-series data when it is needed outside the marketplace: a board slide, an incident write-up, a comparison against another tool's numbers.

#### Before you start

- **Set the filters first.** The export reflects exactly what is on screen. For a 7-day view of `5xx` errors against the Payments API, set those filters before exporting.
- **Pick the right destination shape.** Spreadsheets work well for time-series traffic. BI tools may prefer raw, unbucketed rows.

To export the time-series data:

1. Apply the time range, API, status code, and consumer filters that match the question you are answering.
2. Hover over the **API Calls Over Time** chart. An export icon appears in the chart's top-right corner.
3. Click the export icon to open the format picker.
4. Pick **CSV** for spreadsheet use, or **JSON** for programmatic use. The browser downloads a file named for the chart and the date, for example `api-calls-over-time-2026-05-30.csv`.
5. Open the file. The columns are `timestamp`, `request_count`, and one column per applied filter (`api_id`, `status_family`, `consumer_org`, where relevant).

{% hint style="success" %}
**Result:** A portable copy of the chart's underlying data lands on disk, ready to paste into a slide, a runbook, or a longer-term trend file.
{% endhint %}

{% hint style="success" %}
**Tip:** Save the CSV with the filter description in the filename. `payments-5xx-7d-2026-05-30.csv` is easier to find six months later than `export.csv`.
{% endhint %}

{% hint style="info" %}
**Note:** The export honours the active filter set exactly. If a custom time range is set, the export covers that custom window. If a Status Code filter is applied, the export covers only the matching status family.
{% endhint %}

#### Export the recent-requests log

Use this task when the question needs per-request rows rather than time-series buckets. Useful for forensic work and for chargeback or fair-use reviews.

To export the request log:

1. Apply the filters that match the question.
2. Locate the **Export** action above the **Recent Requests** table.
3. Pick **CSV**. The browser downloads `recent-requests-<date>.csv`.
4. Open the file. The columns mirror the table: `timestamp`, `method`, `path`, `status`, `latency_ms`, `consumer_app`, `consumer_org`, `trace_id`.

{% hint style="success" %}
**Result:** A per-request log file lands on disk. The trace ID column is the bridge into your gateway's own log search.
{% endhint %}

{% hint style="warning" %}
**Caution:** Per-request exports can be large. A busy API at 50 requests per second exports tens of thousands of rows per hour. Apply filters before exporting; an unfiltered weekly export can run into millions of rows.
{% endhint %}

{% hint style="success" %}
**Tip:** The `trace_id` column is the single most useful field for cross-tool correlation. Hand the CSV to your platform team and they can join it against gateway access logs by trace ID.
{% endhint %}

#### Compare two time periods

Use this task to answer "is this week worse than last week" or "did the release on Tuesday change traffic". The comparison view overlays two ranges on the same chart axes.

To compare two periods:

1. With the dashboard open, click **Compare** beside the time-range selector.
2. The picker opens with two range inputs side by side, labelled **Current** and **Compared to**.
3. Pick a current range: typically a preset like **Last 7 days**.
4. Pick a comparison range: typically the immediately preceding period, for example the 7 days before that.
5. Click **Apply**. The charts re-render with two overlaid series.
6. Read the **API Calls Over Time** chart. The current period is rendered in the primary colour; the comparison period is rendered in a dimmer line for contrast.
7. Read the summary tiles. Each tile shows the current value, the comparison value, and the percentage change between them.

The numbered callouts on the compare panel are:

1. **Current range picker**. The primary range. Tiles, charts, and tables read this as the headline value.
2. **Compared to range picker**. The contrast range. Tiles show this as the baseline; charts overlay it as a dimmer line.
3. **Apply button**. Refreshes the dashboard with both ranges loaded.
4. **Delta indicators**. On each tile, the percentage change is rendered next to the headline value. A green arrow indicates improvement (more `2xx`, fewer `5xx`, lower latency); a red arrow indicates regression.

{% hint style="success" %}
**Result:** Every chart on the page overlays the two ranges. A line that sits above last week's at every point is a healthy growth signal; a line that sits below at the same points is a contraction signal.
{% endhint %}

{% hint style="success" %}
**Tip:** Pair Compare with a per-API drilldown to answer "did the Payments API improve after Tuesday's release". Drill into the API first, then enable Compare with Last 24 hours against the 24 hours before that.
{% endhint %}

{% hint style="info" %}
**Note:** Comparison only works for ranges of equal duration. Picking a Current of "Last 7 days" against a Compared to of "Last 30 days" is rejected with a validation banner; the marketplace requires symmetric windows so the deltas mean something.
{% endhint %}

## Investigating anomalies

The dashboard surfaces the data; you bring the interpretation. Two patterns appear often enough to deserve their own walk-throughs: identifying that an anomaly exists, and configuring webhook alerts so the next anomaly finds you rather than the other way around.

#### Identify a traffic anomaly

Use this task as a routine pre-flight check at the start of every day, and as the entry point whenever a consumer reports a problem.

To check for anomalies:

1. Open the Provider Analytics dashboard.
2. Set the time range to **Last 24 hours**.
3. Look at the four summary tiles. A normal day has Total Calls within a typical band, Success Rate above 99 percent, Error Rate below 1 percent, and Average Latency close to your baseline.
4. Read the **API Calls Over Time** chart. Look for vertical spikes (sudden bursts of traffic), sustained dips (likely outages), or a step change (a release that succeeded or failed).
5. Read the **Status Codes** panel. A donut that is mostly `2xx` is healthy; visible `5xx` or growing `4xx` is a signal.
6. Click any anomalous segment or point to apply it as a filter, drilling into the slice of traffic that matters.
7. Use the **Compare** view with **Last 24 hours** against the 24 hours before to confirm the anomaly is unusual, not just routine variance.
8. Once the anomaly is confirmed, drill into the API responsible via Top APIs, then into the consumer responsible via Top Consumers.

{% hint style="success" %}
**Result:** Confirmed anomalies have a named API, a named endpoint, a named consumer organisation, and a precise window. The next step is operational: notify the consumer, suspend a subscription, roll back a release.
{% endhint %}

{% hint style="success" %}
**Tip:** The fastest "is this normal" check is the Compare view with Last 7 days against the 7 days before. If today's tiles fall outside the comparison range's bands, the anomaly is real.
{% endhint %}

{% hint style="info" %}
**Note:** A genuine anomaly is usually visible from at least two charts. A spike on the time-series alone with no change in the status-codes panel is often a traffic shift rather than an incident; a status-code change with no volume change is usually the more serious incident signal.
{% endhint %}

#### Configure webhook alerts on anomalies

Sitting on the Analytics page waiting for a spike is not necessary. Configure outbound webhooks so anomaly events fire as HTTP POSTs to your tooling.

#### Before you start

- **Confirm you have a webhook endpoint.** Typical destinations are a Slack incoming-webhook URL, a PagerDuty integration URL, or your own internal incident-bot endpoint.
- **Decide which signal matters.** Common selections are *error-rate above threshold*, *latency above threshold*, and *unusual traffic from a single consumer*. Start with one.

To configure webhook alerts:

1. From the left sidebar, expand **SETTINGS** and click **Webhooks**.
2. On the Webhooks page, click **Add webhook**.
3. Enter an **Endpoint URL**. The marketplace POSTs JSON payloads to this URL when subscribed events fire.
4. Pick the **Event types** to subscribe to. Analytics-related events include `error_rate.exceeded`, `latency.exceeded`, `traffic.spike`, `traffic.drop`.
5. For threshold events, set a threshold value: an error-rate percentage, a latency value in milliseconds, or a traffic-volume delta.
6. Click **Save**. The marketplace sends a test payload to your endpoint.
7. Confirm the test payload arrived in your destination tool.

{% hint style="success" %}
**Result:** Future anomaly events fire as HTTP POSTs to your webhook endpoint. The dashboard becomes the place you go after a webhook fires, not the place you watch.
{% endhint %}

{% hint style="info" %}
**Note:** Alerts are configured outside the Analytics page itself. The dashboard is read-only; every alerting rule lives under Webhooks in the SETTINGS group. The webhook list, the event catalogue, and the delivery log are all covered in the Day-to-day operations chapter.
{% endhint %}

{% hint style="success" %}
**Tip:** Start with one webhook and one event. Tune the threshold for a week before adding more. Noisy alerts get muted; muted alerts get missed.
{% endhint %}

#### Verify

1. Confirm the test payload arrives in your destination tool within seconds of saving the webhook.
2. Open the webhook row and click **Send test** again. Confirm the second event also lands.
3. Trigger a real condition the webhook subscribes to, for example by forcing a `5xx` from a test API, and confirm the alert fires end to end.

## Next steps

- **Exposing APIs to AI agents**. Once you can read your traffic, the next chapter shows how AI agents become a new traffic source through MCP and API GPT.
- **Onboarding your first consumer**. When a spike traces back to one consumer, return there to suspend or revoke their subscription.
- **Day-to-day operations**. Tune governance rules, email templates, and webhook event types that drive the alerts you receive.