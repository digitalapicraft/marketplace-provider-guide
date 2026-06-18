---
icon: file-invoice-dollar
---

Monetization is the pricing brain of the marketplace. It turns metered API consumption into revenue by tracking plans, subscriptions, usage, invoices and payouts in one place, while delegating the movement of money to an external billing platform. This page explains what the model is and why it is built this way.

![Figure. The API Monetization dashboard, showing plans, active subscriptions, open invoices and pending payouts.](.gitbook/assets/screenshots/provider/admin-monetization-dashboard.png)

## The monetization model

Six objects carry an API call from price to paid invoice: a **product** bundles APIs into a subscribable offering, a **plan** attaches price, quota and tiers, a **subscription** links a consumer app to a plan, **usage records** capture metered calls, **invoices** roll rated usage into line items, and **payouts** disburse a provider's revenue share.

*Figure. How the six objects chain from price to payout.*

```mermaid
flowchart LR
    Product["Product"] --> Plan["Plan"]
    Plan --> Subscription["Subscription"]
    Subscription --> Usage["Usage records"]
    Usage --> Invoice["Invoice"]
    Invoice --> Payout["Payout"]
```

## The eight pricing models

Each plan charges with one of eight models.

<details><summary>The eight pricing models</summary>

- **Fixed fee** is a flat recurring charge regardless of usage.
- **Pay-as-you-go** prices per call against metered usage.
- **Flat + quota** is a flat fee that includes a usage allowance.
- **Tiered** prices each accumulating band as consumption crosses it.
- **Volume** applies a single rate to all usage, chosen by total volume.
- **Freemium** grants a free allowance, then a paid plan beyond it.
- **Revenue share** splits revenue with the API provider through payouts.
- **Hybrid** combines a base fee with usage or tiers.

</details>

## The two operating modes

Where the gateway sits decides who meters and who enforces.

{% tabs %}
{% tab title="Proxy mode" %}
Astra Gateway sits in the request path, meters every call and enforces quotas in real time. Billing is exact, and all eight pricing models apply: fixed, pay-as-you-go, flat + quota, tiered, volume, freemium, revenue share and hybrid.
{% endtab %}
{% tab title="Federated mode" %}
Apps call the customer's own gateway directly. That gateway enforces quotas, and Astra collects usage afterward with some delay. Billing closes after a grace period, and the model suits per-call, freemium and soft-quota pricing.
{% endtab %}
{% endtabs %}

## The usage pipeline

Metered calls flow through a fixed pipeline. A window stays open through the grace period, closes, and is rated into an invoice, so delayed federated usage still bills fairly.

*Figure. From raw call to rated invoice line.*

```mermaid
flowchart LR
    Sources["Sources"] --> Raw["Raw record"]
    Raw --> Normalized["Normalized events"]
    Normalized --> Windows["Aggregation windows"]
    Windows --> Adjustments["Adjustments"]
    Adjustments --> Invoice["Invoice"]
```

## Pluggable billing adapters

Astra prices and rates; an external platform invoices, charges and reconciles. Billing plan mappings link an Astra rate plan to a product and price in Stripe, Kill Bill, Zuora or Chargebee. Switching platforms is a configuration change behind one adapter interface, not a rebuild.

> **How-to:** for step-by-step configuration, see the How-to guides.