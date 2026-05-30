---
icon: chart-line
---

Once APIs are live and consumers are calling them, you need a single place to see how the traffic is shaped. The marketplace ships a built-in **Provider Analytics** dashboard that aggregates request volume and status families into one operational view. This chapter walks every tile and every panel on that page, explains the time-range controls, and then covers the anomaly-investigation routine and webhook alerts that providers rely on most.

You will learn:

- How to open the Provider Analytics page and read the summary tiles, the API Calls Over Time chart, the Status Codes panel, and the Recent Requests log.
- How to switch the time range using the preset buttons and the Custom range option.
- How to read each panel to answer "how much traffic" and "is anything failing".
- How to recognise a traffic anomaly worth investigating.
- How to configure outbound webhooks so anomaly events alert you without watching the dashboard.

Allow ~20 minutes to walk the page end to end and configure your first webhook.

## The Provider Analytics dashboard

Provider Analytics is the operational home for every API Provider. It is where you answer day-to-day usage questions ("how much traffic is flowing today?") and incident questions ("did the 5xx rate spike after yesterday's release?") without leaving the marketplace UI. Every chart on the page is derived from the same source, the per-request log your gateway connections report back, so the numbers across tiles, charts, and tables are always internally consistent.

#### Open the Provider Analytics page

Open the dashboard whenever you need a current snapshot of API traffic across every API your organisation has published.

#### Before you start

- **Publish at least one API.** The dashboard only shows traffic that has flowed through your gateway connections. If you have not yet completed the Importing your first API and Publishing your first API chapters, every chart renders empty.
- **Confirm at least one gateway connection is healthy.** The marketplace plots whatever your connection reports back. A connection that failed its last sync produces a stale or empty view. The Test connection action lives on the Manage API Sources page, covered in the Connecting your first gateway chapter.
- **Know the question you are answering.** A weekly traffic review uses a different time range from an incident triage. Naming the question before you arrive saves five minutes of poking.

To open the Provider Analytics dashboard:

1. From the left sidebar, expand **API MANAGEMENT** and click **Analytics**. The page opens at `/admin/portal/analytics`.
2. The page header reads **Analytics**, with the main panel heading **API Usage Analytics**.
3. Read the summary tiles across the top row. Each tile shows a single number for the selected time range: total calls, success rate, error rate, and average latency.
4. Scroll to **API Calls Over Time** for the request-volume time-series chart.
5. Scroll past that to **Status Codes** for the status-family breakdown, and **Recent Requests** for the live request log.

![Figure 9-1. The Provider Analytics dashboard with the four summary tiles, the time-series chart, the status-code breakdown, and the recent-requests table.](.gitbook/assets/screenshots/provider/admin-portal-analytics.png)

The numbered callouts in Figure 9-1 are:

1. **Page title and breadcrumb**. Reads **Analytics** with the breadcrumb Home > Administration > Analytics above it. Confirms the page is the provider-side dashboard, not a consumer analytics page.
2. **API Usage Analytics panel**. The main panel that holds every chart and table. Everything below this heading reflects the selected time range.
3. **Summary tiles row**. Four tiles: total calls, success rate, error rate, average latency. Each tile updates whenever the time range changes.
4. **API Calls Over Time chart**. A time-series line chart of total request volume. The X-axis buckets scale with the selected time range; the Y-axis shows request count per bucket.
5. **Status Codes panel**. A bar or donut chart breaking traffic down by HTTP status family: `2xx`, `3xx`, `4xx`, `5xx`. Hover any segment to see the exact count.
6. **Recent Requests table**. A scrollable table of the most recent individual requests with columns for timestamp, method, path, status, latency, and consumer app. With no recorded traffic it renders its empty state.

{% hint style="success" %}
**Result:** Every chart on the page reflects the selected time range. The four summary tiles answer the first question ("how much traffic, how many errors") at a glance.
{% endhint %}

{% hint style="info" %}
**Note:** The dashboard reflects the organisation scope shown in the top bar. Providers belonging to more than one organisation should switch organisation in the top-right selector before reading the numbers; the same API can show different totals when scoped to different organisations.
{% endhint %}

{% hint style="success" %}
**Tip:** Bookmark `/admin/portal/analytics` directly. The page is a frequent destination and the marketplace deep-links into it cleanly.
{% endhint %}

#### Read the four summary tiles

Use this task on every visit to the dashboard. The tiles are the fastest read on whether something needs attention or whether the day's traffic is routine.

To read the tiles:

1. From the dashboard, look at the four tiles across the top of the API Usage Analytics panel.
2. Read **Total Calls** first. This is the request count for the current time range. A weekly review against the same range last week tells you whether traffic is growing.
3. Read **Success Rate** next. This is the proportion of `2xx` responses against the total. A healthy production API sits above 99 percent; a number below 95 percent during business hours warrants a closer look.
4. Read **Error Rate**. This is the proportion of `4xx` and `5xx` responses combined. Treat sudden growth as an incident signal: a `4xx` spike points at consumers, a `5xx` spike points at your APIs.
5. Read **Average Latency**. This is the mean response time in milliseconds across the selected range. Compare against your published SLA; a number drifting upward across the week often precedes a sustained 5xx spike.

![Figure 9-2. The four summary tiles at the top of the dashboard.](.gitbook/assets/screenshots/provider/monitor-summary-tiles.png)

The numbered callouts in Figure 9-2 are:

1. **Total Calls**. The request count for the current time range. With no recorded traffic the tile reads zero, which is the empty-state value, not an error.
2. **Success Rate**. The proportion of `2xx` responses against the total.
3. **Error Rate**. The proportion of `4xx` and `5xx` responses combined.
4. **Average Latency**. The mean response time in milliseconds across the selected range.

{% hint style="success" %}
**Tip:** The tiles share their time range with every panel below them. Change the time range and the tile values change in step with the charts; you never need to reconcile two views.
{% endhint %}

{% hint style="info" %}
**Note:** Average latency is a mean, not a percentile. For latency-sensitive APIs, follow up with the Recent Requests table sorted by Latency to find the slow outliers; the mean alone can hide a long tail.
{% endhint %}

{% hint style="warning" %}
**Caution:** A success rate of 100 percent and a total-call count of zero is not "healthy", it is "no traffic". Always check Total Calls before reacting to Success Rate.
{% endhint %}

## Choosing the time range

The dashboard loads with a default time range applied across every panel. A row of preset buttons sits at the top of the API Usage Analytics panel: **1H**, **6H**, **24H**, **7D**, **30D**, **90D**, and **Custom**. Selecting a button re-queries every panel at once. There are no separate dropdown filters for API, status code, or consumer on this page; the time range is the single control that scopes the view.

#### Switch the time range

Change the time range to switch between an incident-triage view, a release-day check, a weekly trend, and a longer-term summary.

#### Before you start

- **Pick the range that matches the question.** 1H and 6H are for incident triage; 24H is for release-day checks; 7D is for weekly reviews; 30D and 90D are for monthly and quarterly summaries. Custom covers everything else.
- **Note the data retention cap.** The marketplace retains a finite window of analytics data. A Custom range reaching further back than retention allows returns what data is held.

To switch the time range:

1. At the top of the API Usage Analytics panel, find the row of time-range buttons: **1H**, **6H**, **24H**, **7D**, **30D**, **90D**, **Custom**.
2. Click a preset button. The currently selected button is highlighted.
3. To set an arbitrary window, click **Custom** and enter a From date-time and a To date-time.
4. Every panel re-queries against the new range at once: the summary tiles, the API Calls Over Time chart, the Status Codes panel, and the Recent Requests table.

{% hint style="success" %}
**Result:** The dashboard renders for the selected range. The X-axis on **API Calls Over Time** rebuckets to suit the range: finer buckets for the short presets, daily buckets for the longer ones.
{% endhint %}

{% hint style="info" %}
**Note:** Time ranges are evaluated in your browser's local timezone. Teams that work across regions should mention the timezone explicitly when sharing screenshots or numbers.
{% endhint %}

{% hint style="success" %}
**Tip:** When investigating a spike, zoom out first and zoom in second. The 24H view confirms whether the spike is unusual; a Custom hour-wide range tells you when it started.
{% endhint %}

## Reading the panels

Once the time range is set, the three panels in the API Usage Analytics view hold the answers. Each panel reads independently, but they share the same source data, so cross-checks between them are always consistent. In an environment with no recorded API traffic, every panel renders its empty state rather than data; the layout is identical once traffic begins to flow.

#### Read the API Calls Over Time chart

Use this chart on every visit. The shape of the line is the single most important read on whether traffic is healthy.

To read the chart:

1. Scroll to **API Calls Over Time** in the API Usage Analytics panel.
2. Read the Y-axis label (request count per bucket) and the X-axis (timestamps).
3. Look for visible patterns:
   - A steady baseline with predictable daily cycles is healthy.
   - A sudden vertical spike is an incident or a launch.
   - A sustained drop is an outage on your APIs, a consumer outage, or a missed sync.
4. With no recorded traffic, the chart shows a flat empty state. This is expected until consumers begin calling your published APIs.

{% hint style="success" %}
**Result:** You can read the request-volume curve at a glance. The chart is the canonical "is anything happening" view.
{% endhint %}

{% hint style="success" %}
**Tip:** Combine this chart with a Custom time range to investigate any visible spike or dip. Set the From and To dates to a tight window around the anomaly and the chart redraws at a finer bucket size, revealing when the change began.
{% endhint %}

#### Read the Status Codes panel

Use this panel as the second pass after API Calls Over Time. Volume tells you how much traffic; status codes tell you whether that traffic succeeded.

To read the panel:

1. Scroll to **Status Codes**.
2. The panel breaks traffic down by HTTP status family: `2xx`, `3xx`, `4xx`, `5xx`. Each segment represents one family and its share of the total.
3. A panel that is mostly `2xx` is healthy; a visible `5xx` share or a growing `4xx` share is a signal.
4. With no recorded traffic, the panel shows its empty state. The families populate once requests flow.

{% hint style="success" %}
**Result:** You can read the success-versus-error split at a glance. The panel is the canonical "is anything failing" view.
{% endhint %}

{% hint style="success" %}
**Tip:** A growing `3xx` share is often missed. The `3xx` family covers redirects and `304 Not Modified`. A spike often indicates a misconfigured client following redirects in a loop; investigate before it escalates.
{% endhint %}

#### Read the Recent Requests table

Use this table for forensic work. It is the only view in the dashboard that shows individual requests rather than aggregates.

To read the table:

1. Scroll to **Recent Requests**.
2. Read the columns left to right: timestamp, method, path, status, latency, and consumer app.
3. Scan for slow outliers in the latency column and grouped failures in the status column.
4. With no recorded traffic, the table shows its empty state ("No requests yet" or equivalent). Rows appear as soon as consumers call your APIs.

{% hint style="success" %}
**Result:** The table gives a per-request view that is enough to identify the consumer and correlate against gateway logs once traffic is flowing.
{% endhint %}

{% hint style="success" %}
**Tip:** During an incident, scan the status column for `5xx` rows. The table becomes a feed of the most recent failures in the selected time range.
{% endhint %}

## Investigating anomalies

The dashboard surfaces the data; you bring the interpretation. Two patterns appear often enough to deserve their own walk-throughs: identifying that an anomaly exists, and configuring webhook alerts so the next anomaly finds you rather than the other way around.

#### Identify a traffic anomaly

Use this task as a routine pre-flight check at the start of every day, and as the entry point whenever a consumer reports a problem.

To check for anomalies:

1. Open the Provider Analytics dashboard.
2. Set the time range to **24H**.
3. Look at the summary tiles. A normal day has Total Calls within a typical band, Success Rate above 99 percent, Error Rate below 1 percent, and Average Latency close to your baseline.
4. Read the **API Calls Over Time** chart. Look for vertical spikes (sudden bursts of traffic), sustained dips (likely outages), or a step change (a release that succeeded or failed).
5. Read the **Status Codes** panel. A panel that is mostly `2xx` is healthy; a visible `5xx` share or a growing `4xx` share is a signal.
6. Scan the **Recent Requests** table for the failing calls behind any status-code change. The status and path columns name the affected endpoint.
7. Compare the day's tiles against the same range a week earlier by switching the time range to **7D**, to confirm the anomaly is unusual rather than routine variance.

{% hint style="success" %}
**Result:** A confirmed anomaly has a window, an affected status family, and the failing paths from Recent Requests. The next step is operational: notify the consumer, suspend a subscription, roll back a release.
{% endhint %}

{% hint style="success" %}
**Tip:** The fastest "is this normal" check is to switch between **24H** and **7D**. If the day's tiles fall well outside the week's typical band, the anomaly is real.
{% endhint %}

{% hint style="info" %}
**Note:** A genuine anomaly is usually visible from at least two panels. A spike on the time-series alone with no change in the Status Codes panel is often a traffic shift rather than an incident; a status-code change with no volume change is usually the more serious incident signal.
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

![Figure 9-3. The Add webhook form with the endpoint URL and event selection.](.gitbook/assets/screenshots/provider/admin-config-services-webhook-add.png)

The numbered callouts in Figure 9-3 are:

1. **Endpoint URL**. The HTTPS URL the marketplace POSTs JSON payloads to when a subscribed event fires.
2. **Event selection**. The list of events to subscribe to. Pick the analytics-related events that matter to you.
3. **Save**. Commits the webhook and sends a test payload to the endpoint.

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