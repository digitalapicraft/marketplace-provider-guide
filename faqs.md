---
icon: circle-question
---

### Q: How many gateways can I connect?

The marketplace supports any number of gateway connections. Each connection is treated as an independent source, including multiple connections to the same gateway product (for example a separate Apigee X connection per GCP project, or a separate AWS connection per region). There is no licensed cap on the number of connections an Organisation can hold.

### Q: How do I rotate a gateway credential?

Open API MANAGEMENT > Manage API Sources, locate the connection under Existing API Sources, and click Edit. Paste the new credential into the secret field. The marketplace blanks the existing value for safety, so clearing it first is not required. Click Test connection to verify, then Save. Already-imported APIs continue to work; the next import or scan uses the new credential.

### Q: Can two API Providers from the same Organisation publish the same API?

The marketplace identifies an API by the gateway it was imported from and the API's identifier in that gateway. Two providers from the same Organisation cannot publish a duplicate of the same gateway-side API. For two listings of the same underlying API (for example a public version and a private internal one), import it once and create two API Products that reference the same API with different visibility and plans.

### Q: What happens to active subscriptions when I revoke an API?

Revoking an API moves any subscriptions tied to it through the consumer-facing flow: their App's keys stop authorising calls to the revoked API on the next request. The subscription record is preserved in Revoked state for audit, and consumers see a notification in their inbox. To give consumers a grace period, use the Upcoming API Deprecation Notice content type to warn them weeks in advance and only revoke after the deadline.

### Q: Can I run the marketplace and my own consumer-facing developer portal in parallel?

Yes. The marketplace is the publishing and operations console; nothing prevents running a separate developer portal that links into the marketplace. The most common pattern is to use the marketplace's public catalog as the source of truth, then embed the marketplace's API rendering or chat assistant into your portal via iframe or link-out. Discuss available embedding options with your account manager based on your tier.

### Q: Does the marketplace store consumer call payloads?

No. The marketplace stores metadata about every call. Timestamp, API, consumer App, status code, latency. For analytics, but it does not store request or response bodies. Payloads remain in the gateway's logging tier as configured. For payload-level audit, export from your gateway, not from the marketplace.

### Q: How long is analytics data retained?

The default retention is 90 days for raw time-series data and 13 months for daily aggregates. Your Org Admin can extend retention on tiered plans. To export beyond the retention window, use the Export action on the Provider Analytics page on a regular cadence. Daily or weekly. And store the exports in your own data warehouse.

### Q: Can I export data via API?

Yes. The marketplace exposes an internal REST API for the same data shown in the UI (APIs, API Products, subscriptions, analytics). The endpoints require an API key issued under your account; obtain one from My account > API Keys and read the API reference linked from the marketplace's Help menu. Long-form integration patterns live in the Marketplace Technical Reference, not in this guide.

### Q: How do I move from Draft to Published without losing my edits?

The Draft and Published states share the same underlying record; the marketplace does not fork your edits. When you click Publish, the current Draft becomes the Published version. Subsequent edits go directly to the Published version with no separate Draft branch. There is no "Save as Draft" while a Published version exists. For a staging copy to experiment in, clone the API or API Product first, work on the clone, and publish only when ready.

### Q: How does API GPT decide which APIs to expose?

API GPT is backed by the MCP Server bundle configured under ADMINISTRATION > MCP Servers. The bundle defines which APIs the assistant can describe and call. APIs that are not part of the bundle are invisible to the chat assistant, even when published in the public catalog. To expose a new API to API GPT, add it to your MCP Server's tool list and republish.

### Q: Can I have more than one plan per API Product?

Yes. An API Product can carry as many plans as required. Typically a free tier, one or more paid tiers, and an enterprise tier with custom limits. Each plan defines its own quota and rate-limit settings, and consumers select a plan when they subscribe. Adding or removing plans on a published API Product does not affect existing subscriptions; they remain on the plan originally subscribed to until migrated.

### Q: How do I tell which API a consumer is calling when traffic spikes?

Open ADMINISTRATION > Provider Analytics, set the time range to the spike window, and group by API. The chart breaks traffic into per-API series. To narrow further, group by Consumer App to identify the specific App driving traffic. Status-code filters separate spikes that are working calls from spikes that are 4xx or 5xx storms.

### Q: What is the difference between a webhook and a system notification?

A **system notification** is an in-product message posted for users to read in their inbox. Release notes, outage announcements, policy updates. A **webhook** is an outbound HTTP callback the marketplace sends to a system you control when an event occurs (subscription created, governance scan completed). Use system notifications for human readers; use webhooks for downstream automation.

### Q: Can I delete an API I imported by mistake?

Yes. Open API MANAGEMENT > Manage APIs, locate the API, and click Delete in the row's action menu. The marketplace prompts for confirmation. Deleting an API removes it from the marketplace catalog only; the underlying API in your gateway is untouched. If the API has Active subscriptions, the prompt warns and offers to migrate or revoke those subscriptions first.