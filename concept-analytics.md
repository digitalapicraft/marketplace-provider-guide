---
icon: chart-line
---

Analytics and reporting is the adoption lens over your marketplace: it turns the usage the gateway records into a view of who is adopting what and how it performs. Provider analytics gives product owners the built-in dashboards, while custom reports let teams build the specific slices they watch. Both read the same underlying data, so the picture stays consistent.

![Figure. Provider analytics showing adoption and traffic.](.gitbook/assets/screenshots/provider/admin-portal-analytics.png)

## Provider analytics

Provider analytics answers the product owner's first questions: what is being adopted, and how healthy is it. The built-in dashboards present adoption, traffic, the top products, and the health of the subscriber base.

- **Adoption:** new subscriptions and active apps over time.
- **Traffic:** call volume, error rates, and latency across APIs and products.
- **Top products:** which products and APIs drive the most usage.
- **Subscriber health:** active, idle, and at-risk subscriptions at a glance.

A point worth keeping in mind: Astra reports what the gateway meters. The marketplace is not in the request path, so the freshness of these numbers depends on how usage is collected. Where usage is collected continuously, the view is close to real time; where it is batched, expect a collection delay.

## Custom reports

Beyond the built-in dashboards, custom reports answer specific questions by building a filtered, saved view. You pick the dimensions and metrics, choose a time window, and the report reruns on demand.

- **Dimensions:** group by service, environment, product, status, or route.
- **Metrics:** request count, error rate, and latency percentiles.
- **Time window:** bound a report to a release, an incident, or a billing period.
- **Saved and shareable:** a saved report reruns on demand and shares a stable link, so a team watches the same slice over time.

This makes reports the right tool when a built-in dashboard is too broad: scope a view to one release window, one environment, or one product line, then share that link with the people who need it.

{% hint style="info" %}
**Note:** analytics reports on usage; turning that usage into revenue (plans, invoices, and payouts) is covered separately under monetization.
{% endhint %}

> **How-to:** for step-by-step configuration of dashboards and custom reports, see the How-to guides.