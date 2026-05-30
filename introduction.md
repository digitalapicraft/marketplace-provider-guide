---
icon: book-open
---

# Marketplace

**Your APIs are products. Marketplace is where you sell them.**

Every API your organisation builds carries business value: a payments call, a customer-lookup, a logistics feed. On their own, those endpoints sit behind a gateway where only the team that built them knows they exist. Marketplace turns them into a branded, discoverable catalog that customers, partners, and internal teams can find, trial, subscribe to, and build on. The API stops being plumbing and becomes a product with an audience.

{% hint style="success" %}
**Why this matters.** Companies in the API economy do not stop at exposing APIs. They package them as products and put them in front of the people who pay for them. A marketplace is the storefront, the contract desk, and the analytics room for that business, all in one console.
{% endhint %}

## Why an API marketplace

A marketplace opens three growth levers that a raw gateway cannot. Each one turns existing engineering work into reachable revenue.

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td><strong>Sell more in markets you already serve</strong></td><td>Publish your APIs as subscribable products to the customers and partners you have today. A self-service catalog with live docs and instant sign-up shortens the path from "interested" to "integrated" from weeks to minutes.</td></tr><tr><td><strong>Reach new markets through partners</strong></td><td>Onboard third-party providers and let them publish alongside you. Their APIs widen your catalog, their audiences become yours, and the marketplace becomes an ecosystem rather than a single vendor's price list.</td></tr><tr><td><strong>Build faster and cheaper by composing</strong></td><td>Consume partner and third-party APIs from the same catalog your own teams use. Integrate proven capabilities instead of rebuilding them, and cut both cost and time to market.</td></tr></tbody></table>

## Who it serves

A marketplace is a shared hub, not a one-way feed. It gives every audience a custom-branded experience and a reason to come back.

- **Providers** (your product teams and partner developers) publish APIs, enrich them with documentation and branding, set who can see them, and watch how they are used.
- **Consumers** (external customers and internal teams) discover APIs, read interactive docs, request access, manage their credentials, and build with confidence.
- **The business** gets a governed channel where access is controlled, usage is measured, and subscriptions become repeatable revenue streams.

This collaboration across your organisation, your customers, and your partners is what compounds into growth. A gateway moves traffic; a marketplace builds an ecosystem around it.

{% hint style="info" %}
**One API is rarely the product.** Customers buy outcomes, not endpoints. Marketplace lets you bundle related APIs into Products and Plans so a subscriber gets a complete capability, with the commercial terms attached, instead of a list of disconnected URLs.
{% endhint %}

## What Marketplace is

Marketplace is the enterprise console that delivers all of the above. The storefront publishes your APIs to a branded developer catalog. The control plane connects to every major API gateway, so one console covers APIs wherever they run. The governance and operations layer scores every spec, routes every subscription through approval, measures every call, and notifies every operator on every event.

{% hint style="info" %}
**Who this guide is for.** You are the API Provider or Portal Admin who runs the marketplace day-to-day: you connect the organisation's gateways, import APIs into the catalog, complete the consumer-facing metadata, approve subscription requests, monitor usage, and operate the administrative surfaces. This guide walks every surface you reach from sign-in onwards.
{% endhint %}

## The three working areas

The provider experience groups every surface under one of three working areas. Each maps to a section of this guide.

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Provider workflow</strong></td><td>Connect gateways, import APIs, review governance, publish, onboard consumers, monitor traffic, expose to AI agents.</td><td><a href="provider-workflow.md">provider-workflow.md</a></td></tr><tr><td><strong>Administration</strong></td><td>People and roles, sign-in and branding, day-to-day operations across notifications, webhooks, email, and content.</td><td><a href="administration.md">administration.md</a></td></tr><tr><td><strong>Reference</strong></td><td>Troubleshooting, FAQs, support links, and a glossary of marketplace terms.</td><td><a href="reference.md">reference.md</a></td></tr></tbody></table>

## What the marketplace gives you

- **Multi-gateway connections.** Register a connection to any supported gateway (Apigee, Apigee X, Kong, MuleSoft, AWS API Gateway, Azure API Management, IBM API Connect, Tyk, APISIX, Aelix). The marketplace reads APIs and Products from the connected gateway without relocating them.
- **Documentation and repository sources.** Pull API specifications from spec registries (SwaggerHub, Postman) and source-control systems (GitHub, Bitbucket, Azure Repos) for APIs not yet behind a runtime gateway.
- **Synchronised API and Product catalog.** Maintain a unified catalog populated from every connected source. Synchronisation is automatic and incremental; gateway-side changes propagate on the next cycle.
- **Consumer-facing metadata enrichment.** Complete each API and Product with the title, overview, long-form documentation, logo, categories, and tags consumers see in the storefront.
- **API governance scoring.** Score every API specification against configurable linting rulesets and surface the score, severity-ranked violations, and remediation guidance on a dedicated Governance Report.
- **Read-only Plan, quota, and rate-limit visibility.** Display the Plan, quota window, and rate-limit ceiling that the connected gateway enforces, so consumers see accurate operational terms before they subscribe.
- **Subscription lifecycle management.** Review pending subscription requests, approve or reject them, and move existing subscriptions through Active, Suspended, and Revoked states.
- **Consumer application management.** Inspect every application a consumer has registered, the credentials issued to it, and the subscriptions linked to it.
- **Provider analytics.** Read time-series traffic data per API, per Organisation, and per consumer, filtered by time range and HTTP status code, and export the underlying data for offline analysis.
- **MCP server and API GPT integration.** Register Model Context Protocol servers that expose a curated subset of APIs to AI agents, and configure the API GPT chat assistant that consumers reach through the storefront.
- **System notifications and webhooks.** Post organisation-wide announcements to every user's inbox and emit outbound webhook deliveries on every subscription, governance, API, and Product event for downstream automation.
- **Identity and access management.** Configure SAML 2.0 single sign-on against an external identity provider, manage built-in and custom roles, organise members into teams, and scope visibility per team.
- **Storefront branding and theming.** Apply per-Organisation branding (logo, favicon, accent colour, theme) and register additional domains for multi-brand deployments.

{% hint style="success" %}
**New here?** Start with [Getting started](getting-started.md). Ten minutes covers sign-in, the dashboard, the four sidebar groups, and the UI conventions you see on every page from there on.
{% endhint %}

## Architecture at a glance

![Figure 1-1. The Marketplace architecture: consumer-side surfaces and AI agents, the marketplace storefront and provider console, the connected API gateways and their backend services. Solid arrows show consumer traffic; dotted arrows show the synchronisation flow that populates the marketplace catalog.](.gitbook/assets/screenshots/provider/architecture.png)

The marketplace sits between the consumer-facing storefront and the runtime gateways. Provider work happens in the console at `/admin/...`. Consumer work happens in the public catalog at the root URL. AI agents reach a curated slice of APIs through the MCP server. Every gateway, every spec source, every notification target is configured once and surfaces in the same console.

## How this guide is organised

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Getting started</strong></td><td>Sign in, learn the dashboard, walk the four sidebar groups, recognise the UI patterns.</td><td><a href="getting-started.md">getting-started.md</a></td></tr><tr><td><strong>Provider workflow</strong></td><td>Connect a gateway, import APIs, review governance, publish, onboard consumers, monitor.</td><td><a href="provider-workflow.md">provider-workflow.md</a></td></tr><tr><td><strong>Administration</strong></td><td>People and roles, sign-in and branding, day-to-day operations.</td><td><a href="administration.md">administration.md</a></td></tr><tr><td><strong>Reference</strong></td><td>Troubleshooting, FAQs, additional resources, glossary.</td><td><a href="reference.md">reference.md</a></td></tr></tbody></table>

## Deployment models

Marketplace ships in three deployment models: **hosted SaaS**, **private cloud** (deployed into your own AWS, Azure, or GCP account), and **on-premises** (deployed into your data centre). The provider experience is identical across all three. The deployment model only affects the URL operators reach the console at and who owns the underlying infrastructure. All three models support SAML 2.0 sign-on, role-based access control, multi-tenancy by Organisation, and per-tenant white-labelled branding.

## Audience and prerequisites

This guide is written for the **API Provider** and **Portal Admin** roles. You are responsible for connecting gateways, importing APIs, completing consumer-facing metadata, configuring governance and visibility, approving subscription requests, monitoring usage, and operating administrative surfaces. Prior operational experience with at least one supported gateway product is assumed. Detailed marketplace knowledge is not: the chapters introduce each surface in the order an operator typically encounters it.

Topics outside the API Provider scope (Organisation creation, platform-wide tenancy administration, identity-provider configuration, deep theme customisation) are referenced where relevant and pointed at the corresponding administrator surfaces or companion guides.
