---
icon: chart-line
---

Provider analytics is the operational dashboard where you read API traffic across every API your organisation has published. Use it to answer daily usage questions and to triage incidents without leaving the marketplace.

![Figure. The provider analytics dashboard with summary tiles, the time-series chart, the status-code breakdown, and recent requests.](.gitbook/assets/screenshots/provider/admin-portal-analytics.png)

The dashboard shows four panels for the selected time range:

1. **Summary tiles**: total calls, success rate, error rate, and average latency.
2. **API Calls Over Time**: a request-volume time-series chart that rebuckets to suit the range.
3. **Status Codes**: traffic split by HTTP family (`2xx`, `3xx`, `4xx`, `5xx`).
4. **Recent Requests**: a per-request log with timestamp, method, path, status, latency, and consumer app.

## Configure

1. From the left sidebar, expand **API MANAGEMENT** and click **Analytics**.
2. At the top of the API Usage Analytics panel, choose a time-range preset: **1H**, **6H**, **24H**, **7D**, **30D**, **90D**, or **Custom**.
3. For an arbitrary window, click **Custom** and enter a From and a To date-time.
4. Read the summary tiles for an at-a-glance answer on volume and errors.
5. Read **API Calls Over Time** for the traffic curve, then **Status Codes** for the success-versus-error split.
6. Scan **Recent Requests** to identify individual failing or slow calls.
7. Export the underlying request data where the panel offers it.

{% hint style="success" %}
**Result:** Every panel reflects the selected time range, giving one consistent view of traffic volume and health.
**Tip:** Switch between **24H** and **7D** to confirm whether a spike is unusual or routine variance.
{% endhint %}

The time range is the only control that scopes the page. There are no per-API, status, or consumer dropdown filters. An environment with no recorded traffic renders each panel in its empty state, with identical layout once requests begin to flow.