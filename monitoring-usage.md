# Monitoring usage

Once your APIs are live and consumers are calling them, you need a single place to see how they are performing. The marketplace provides a built-in **Analytics** dashboard that aggregates request volume, error rates, and per-API and per-consumer breakdowns. This chapter walks you through opening the dashboard, narrowing the view to the slice of traffic you care about, exporting the data for offline analysis, and locating the alerting hooks the marketplace exposes.

You will learn:

- How to open the Provider Analytics page and read the four summary tiles, the time-series chart, the status-code breakdown, and the recent-requests log.
- How to narrow the view by time range so a weekly trend, a release-day check, or an incident triage each get the right window.
- How to filter by API or status family to confirm an incident is contained or a fix has landed.
- How to export the time-series data for board slides, BI tools, or runbooks.
- How to register a webhook so anomaly events alert you without watching the dashboard.

Allow ~25 minutes to step through the dashboard once and configure your first alert.

## The Analytics dashboard

The Analytics dashboard is the operational home for every API Provider. Open it to answer a usage, performance, or incident question — *"who is hammering my Search API at 3 a.m.?"*, *"did the 5xx rate spike after yesterday's release?"*, *"which APIs are getting any traction at all?"* — without leaving the marketplace UI.

#### Open the Provider Analytics page

Open the Analytics dashboard whenever you need a current snapshot of API traffic across every API you have published.

#### Before you start

- **Publish at least one API.** The dashboard shows only traffic that has flowed through your gateway connections. If you have not completed [Publishing your first API](publishing-your-first-api.md#publishing-your-first-api) yet, the dashboard will be empty.
- **Confirm your gateway is sending logs.** The marketplace plots whatever your gateway connection reports back. If logs are not flowing, the dashboard renders zero — a connection problem, not a dashboard problem.
- **Identify the question you are answering.** A weekly traffic review uses different filters from an incident triage. Knowing the question before you arrive saves time.

To open the Analytics dashboard:

1. From the left sidebar, expand **API MANAGEMENT** and click **Analytics**.
2. The page loads with the title Analytics and the heading API Usage Analytics.
3. Read the four summary tiles across the top — total calls, success rate, error rate, and average latency for the current time range.
4. Scroll to **API Calls Over Time** to see the request volume chart.
5. Scroll further to **Status Codes** for the status-code breakdown and to **Recent Requests** for the live request log.

![Figure 9-1. The Analytics dashboard.](.gitbook/assets/screenshots/provider/admin-portal-analytics.png)

The numbered callouts in Figure 9-1 are:

1. **Analytics** — The page title and your current location in the sidebar. The breadcrumb above shows Home > Administration > Analytics.
2. **API Usage Analytics** — The main panel that holds every chart and table on this page. Everything below this heading reflects the selected time range and filters.
3. **API Calls Over Time** — A time-series chart of total request volume. The X-axis shows time buckets that scale with the selected time range; the Y-axis shows request count per bucket.
4. **Status Codes** — A bar or donut chart breaking traffic down by HTTP status family — `2xx`, `3xx`, `4xx`, `5xx`. Hover any segment to see the exact count.
5. **Recent Requests** — A scrollable table of the most recent individual requests, with columns for timestamp, method, path, status, latency, and consumer. Click any row to drill into a single request.

> **Result:** You can view request volume, error rate, and recent traffic for every API you have published.

> **Note:** The dashboard reflects the organisation scope shown in the top bar. If your account belongs to more than one organisation, switch organisation in the top-right selector before reading the numbers.

> **Tip:** Bookmark `/admin/portal/analytics` in your browser. The page is a frequent destination, and the marketplace deep-links cleanly into it.

#### Verify

1. Confirm the four summary tiles render with non-zero values once your APIs have served at least a handful of calls.
2. Confirm **API Calls Over Time** plots a line, not an empty chart, for the default time range.
3. Confirm **Recent Requests** lists at least one row, and that clicking a row opens the request detail.

## Narrowing the view

The default time range covers a recent window of traffic across every API and every consumer. Narrow the view to answer a specific question — the smaller the slice, the sharper the signal.

#### Filter the dashboard by time range

Change the time range to compare periods, drill into an incident, or shift from a long-term trend to a near-real-time view.

#### Before you start

- **Select a time range that matches your question.** Use Last hour for incident triage, Last 24 hours for a release-day check, Last 7 days for a weekly review, Last 30 days for a monthly board update.
- **Note that custom ranges may be capped.** The marketplace retains a finite window of analytics data. If you select a custom range reaching further back than retention allows, the dashboard returns the data it has and reports the cap.

To filter by time range:

1. At the top of the API Usage Analytics panel, locate the time-range selector.
2. Click the selector to open the range options.
3. Select one of the preset ranges — Last hour, Last 24 hours, Last 7 days, Last 30 days — or select **Custom range** to set your own start and end.
4. For a custom range, enter a From date and a To date in the date pickers, then click **Apply**.
5. Wait for the charts to refresh. Every panel on the page — API Calls Over Time, Status Codes, Recent Requests — re-queries against the new range.

> **Result:** The dashboard shows traffic for the selected window. The summary tiles, the time-series chart, the status-code breakdown, and the recent-requests table all reflect the new range.

> **Note:** Time ranges are evaluated in your browser's local timezone. If your team works across regions, mention the timezone when sharing screenshots.

> **Tip:** When comparing two periods — for example, this week against last week — open the dashboard in two browser tabs with different ranges. The marketplace keeps each tab's filters independent.

#### Verify

1. Confirm the time-range selector reflects the value you picked after the page refreshes.
2. Confirm the **API Calls Over Time** X-axis labels match the new range — hourly buckets for *Last 24 hours*, daily buckets for *Last 7 days*.
3. Confirm the four summary tiles update — the total-call count for *Last 24 hours* should differ from *Last 30 days* on a busy API.

#### Filter by status code or API

Narrow to a single API or to a specific status-code family to see how one slice of traffic is behaving without the rest of the noise.

#### Before you start

- **Identify which API or status family answers your question.** *"My Payments API is returning 5xx"* points to one API and one status family. *"Authentication errors are climbing"* points to `4xx` across every API.
- **Remember filters compound.** Selecting a time range, an API, and a status code together returns the intersection — fewer rows, sharper signal.

To filter by API or status:

1. With the dashboard open, locate the **API** filter at the top of the API Usage Analytics panel.
2. Click the **API** selector and select one API from the dropdown — for example, *Payments API v2*. Select **All APIs** to clear the filter.
3. Locate the **Status Code** filter beside the API filter.
4. Click the **Status Code** selector and select a family — `2xx`, `3xx`, `4xx`, `5xx` — or a specific code such as `429` or `503`.
5. Wait for the charts to refresh. API Calls Over Time now plots only the matching requests, and Recent Requests lists only the matching log entries.
6. To clear filters, set both selectors back to **All APIs** and **All status codes**.

> **Result:** The dashboard shows only the traffic that matches your filters. Use this view to confirm an incident is contained to one API, or that a fix has driven your 5xx rate back down.

> **Note:** A `4xx` filter shows requests the consumer got wrong — bad input, missing auth, rate limits. A `5xx` filter shows requests your APIs got wrong. Examine both during an incident to identify which side to fix.

> **Tip:** When a suspicious spike appears, click an individual row in Recent Requests to open the request detail. The request detail shows the headers, the consumer app, and the response code — enough to reproduce the call.

#### Verify

1. Confirm **Recent Requests** lists only entries matching the selected API and status family.
2. Confirm **Status Codes** shows a single segment when filtered to one family.
3. Reset both selectors back to **All APIs** and **All status codes** and confirm the row count returns to the unfiltered total.

## Exporting and acting on the data

The dashboard answers most day-to-day questions on its own. When you need to share a number with someone outside the marketplace, or feed traffic data into your own BI tool, export the underlying series.

#### Export the time-series data

Export the time-series data when it is required outside the marketplace — a board slide, an incident write-up, a comparison against another tool's numbers.

#### Before you start

- **Set the filters first.** The export reflects exactly what is on screen. For a 7-day view of `5xx` errors on your Payments API, set those filters before exporting.
- **Select a destination column shape.** A spreadsheet works well for time-series traffic. A BI tool may require raw rows.

To export the time-series data:

1. Apply the time range, API, and status-code filters that match the question you are answering.
2. Hover over the **API Calls Over Time** chart.
3. Locate an **Export** or **Download CSV** action in the chart's top-right corner.
4. Click **Export** and select **CSV** if a format picker appears.
5. The browser downloads a file named for the chart and the date — for example, `api-calls-over-time-2026-05-06.csv`.
6. Open the file in your spreadsheet of choice and verify the column headers — typically `timestamp`, `request_count`, and any filter applied.

> **Result:** You have a portable copy of the chart's underlying data, ready to paste into a slide, a runbook, or a longer-term trend file.

> **Note:** If no Export button is visible on your build of the marketplace, the same data is available through the analytics API endpoint your gateway connection reports against. Open the browser developer tools' Network tab while the dashboard loads, locate the analytics request, copy its URL, and replay it from your tool of choice. Refer the call to your platform team — every call requires the same auth as the dashboard itself.

> **Tip:** Save the CSV with the filter description in the filename — `payments-5xx-7d-2026-05-06.csv` is easier to find six months later than `export.csv`.

#### Verify

1. Confirm the browser download completes and the file size is non-zero.
2. Open the CSV and confirm the row count roughly matches the number of buckets visible on the chart.
3. Spot-check one row's `request_count` against the same bucket on the chart's tooltip.

#### Receive alerts on traffic anomalies

Sitting on the Analytics page waiting for a `5xx` spike is not necessary. The marketplace pushes anomaly events to outbound webhooks instead.

#### Before you start

- **Confirm you have a webhook endpoint to send to.** Typical destinations are a Slack incoming-webhook URL, a PagerDuty integration URL, or your own internal incident-bot endpoint.
- **Decide which signal matters.** Common selections are *error-rate above threshold*, *latency above threshold*, and *unusual traffic from a single consumer*.

To configure alerts:

1. From the left sidebar, expand **SETTINGS** and click **Webhooks**.
2. On the Webhooks page, click **Add webhook**.
3. Enter the **Endpoint URL** of your Slack, PagerDuty, or internal incident bot.
4. Select the events to subscribe to — choose the analytics-related events that match the selected signals.
5. Click **Save**. The marketplace sends a test payload to your endpoint.
6. Confirm the test payload arrived in your destination tool.

> **Result:** Future anomaly events fire as HTTP POSTs to your webhook endpoint. You can stop watching the dashboard and receive alerts directly.

> **Note:** Alerts are configured outside the Analytics page itself. The dashboard is read-only for now — every alerting rule lives under Webhooks in the SETTINGS group.

> **Tip:** Start with one webhook and one event. Tune thresholds for a week before adding more. Noisy alerts get muted; muted alerts get missed.

#### Verify

1. Confirm the test payload arrives in your destination tool within seconds of saving the webhook.
2. Click **Send test** again from the webhook row and confirm the second event also lands.
3. Trigger a real condition the webhook subscribes to (for example, force a `5xx` from a test API) and confirm the alert fires end to end.

## Next steps

- **[Exposing APIs to AI agents](exposing-apis-to-ai-agents.md#exposing-apis-to-ai-agents)** — Once you can read your traffic, the next chapter shows how AI agents become a new traffic source through MCP and API GPT.
- **[Onboarding your first consumer](onboarding-your-first-consumer.md#onboarding-your-first-consumer)** — When a spike traces back to one consumer, return here to suspend or revoke their subscription.
- **[Day-to-day operations](day-to-day-operations.md#day-to-day-operations)** — Tune governance rules and email templates that drive the events you alert on.
