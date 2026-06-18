---
icon: compass
---

Astra Marketplace is a multi-gateway API marketplace and developer portal. It federates the API gateways you already run into one branded place where teams publish APIs as products and consumers discover, subscribe to, and consume them. The key idea is that Astra manages APIs across gateways, so it is the marketplace layer rather than another gateway.

![Figure. The Astra control-plane dashboard.](.gitbook/assets/screenshots/provider/admin-dashboard.png)

## What the marketplace is

The platform brings four capabilities together over the gateways you operate:

- **Federate:** one portal over Apigee, Kong, APISIX, AWS, Azure, and IBM.
- **Publish:** turn APIs into governed, documented products.
- **Monetize:** products, plans, quotas, and usage metering.
- **Self-serve:** consumers discover, subscribe, and test on their own.

## The shift, and why it matters

Over the last decade, organisations that exposed their systems as governed, consumable API products outgrew the ones that kept them locked away. Winners turn capabilities into products others build on. A marketplace is how you productize APIs across every gateway and put them in front of the developers and agents who build on them.

The shift is from gatekept to self-service. The old way meant requesting access through an admin team, waiting on a gatekeeper to publish, and hand-written docs that drifted out of date. With Astra, consumers self-serve to discover, subscribe, and get keys; publishing runs through governance rather than tickets; and documentation stays current because it is generated from the spec.

## The management plane

Astra sits as a management plane above your gateways. It reads from them and never proxies live traffic. Live API calls flow straight from a consumer app to the gateway. Astra manages the APIs around that path, importing, governing, publishing, and metering, yet it never sits in front of a call. This keeps the marketplace out of the latency path while still giving it control over the catalogue.

## The two sides

The same platform serves two audiences, joined by Products and Subscriptions:

- **Provider side (publish and operate):** connect sources, import and govern APIs, publish, package APIs into products with plans, manage subscriptions, and monitor and administer the portal.
- **Consumer side (discover and consume):** browse the catalogue, evaluate docs and specs, sign up, create an app and subscribe, collect credentials, then test and track usage.

## Time to first call

The north-star metric is time to first call: how fast a new consumer can make their first successful call. Maturity runs from manual access taking days, to same-day self-registration, to under ten minutes, to instant in-browser try-it. Astra targets the top of that ladder with a public catalogue, always-current docs, in-browser testing, and copy-paste samples.

## Roles and responsibilities

- **Portal Admin / API Provider:** owns the platform and the APIs; lands on the control-plane dashboard.
- **Org Admin:** runs a tenant or organization; works in the Administration area, scoped to their org.
- **API Consumer:** discovers and consumes APIs; works in the developer portal.

> **How-to:** for step-by-step configuration, see the How-to guides.