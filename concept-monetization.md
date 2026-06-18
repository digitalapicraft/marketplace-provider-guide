---
icon: file-invoice-dollar
---

Monetization is the pricing brain of the marketplace. It turns metered API consumption into revenue by tracking plans, subscriptions, usage, invoices and payouts in one place, while delegating the movement of money to an external billing platform. This page explains what the model is and why it is built this way.

![Figure. The API Monetization dashboard, showing plans, active subscriptions, open invoices and pending payouts.](.gitbook/assets/screenshots/provider/admin-monetization-dashboard.png)

## The monetization model

Six objects carry an API call from price to paid invoice. A **product** is the commercial offering: one or more APIs bundled into something a consumer can subscribe to. A **plan** attaches the price, quota and tiers to a product, and one product can carry several plans. A **subscription** links a consumer app to a product under a plan, with its own lifecycle and credentials. **Usage records** capture metered calls, **invoices** roll rated usage into billable line items, and **payouts** disburse a provider's share of the revenue.

## The eight pricing models

Each plan charges with one of eight models:

- **Fixed fee** is a flat recurring charge regardless of usage.
- **Pay-as-you-go** prices per call against metered usage.
- **Flat + quota** is a flat fee that includes a usage allowance.
- **Tiered** prices each accumulating band as consumption crosses it.
- **Volume** applies a single rate to all usage, chosen by total volume.
- **Freemium** grants a free allowance, then a paid plan beyond it.
- **Revenue share** splits revenue with the API provider through payouts.
- **Hybrid** combines a base fee with usage or tiers.

## The two operating modes

Where the gateway sits decides who meters and who enforces. In **proxy mode**, Astra Gateway sits in the request path, meters every call and enforces quotas in real time, so billing is exact and all eight pricing models apply. In **federated mode**, apps call the customer's own gateway directly; that gateway enforces quotas, and Astra collects usage afterward with some delay. Federated billing closes after a grace period and suits per-call, freemium and soft-quota pricing.

## The usage pipeline

Metered calls flow through a fixed pipeline: **sources** feed an immutable **raw** record of each call, which is cleaned into **normalized** events, summed into **aggregation windows** per subscription and period, then corrected by **adjustments** for late data. A window stays open through the grace period, closes, and is rated into an invoice. This is how delayed federated usage still bills fairly.

## Pluggable billing adapters

Astra prices and rates; an external platform invoices, charges and reconciles. Billing plan mappings link an Astra rate plan to a product and price in Stripe, Kill Bill, Zuora or Chargebee. Switching platforms is a configuration change behind one adapter interface, not a rebuild.

> **How-to:** for step-by-step configuration, see the How-to guides.