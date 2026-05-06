# Introduction

Marketplace is an enterprise API marketplace and operations console. It serves three roles for the organisation that deploys it.

As a **public storefront**, the marketplace renders a branded developer catalog through which external and internal consumers discover APIs, read interactive documentation, request subscriptions, and manage the credentials issued to their applications. As a **multi-gateway control plane**, it integrates with the major commercial and open-source API gateways — Apigee, Apigee X, Kong, MuleSoft, AWS API Gateway, Azure API Management, IBM API Connect, Tyk, APISIX, and Aelix — so a single console covers every gateway regardless of where the APIs run. As a **governance and operations console**, it scores every API against configurable linting rulesets, routes subscription approvals through a defined workflow, surfaces time-series provider analytics, exposes an MCP server and API GPT layer for AI-agent integrations, and emits webhooks and email notifications on every operational event.

The product is delivered as a hosted SaaS service. It supports SAML 2.0 single sign-on, role-based access control with built-in and custom roles, multi-tenancy by Organisation, and per-tenant white-labelled branding (logo, theme, accent colour, and custom domain). Operational state is kept consistent with the connected gateways through periodic synchronisation; gateway-side configuration (Plans, quotas, rate limits, bundled APIs) is treated as read-only on the marketplace and is the gateway's responsibility.

![Figure 1-1. The Marketplace architecture: consumer-side surfaces and AI agents, the marketplace storefront and provider console, the connected API gateways and their backend services. Solid arrows show consumer traffic; dotted arrows show the synchronisation flow that populates the marketplace catalog from the gateways.](screenshots/provider/architecture.png)

- **Multi-gateway connections** — Register a connection to any supported gateway (Apigee, Apigee X, Kong, MuleSoft, AWS API Gateway, Azure API Management, IBM API Connect, Tyk, APISIX, Aelix). The marketplace reads APIs and Products from the connected gateway without relocating them.
- **Documentation and repository sources** — Pull API specifications from spec registries (SwaggerHub, Postman) and source-control systems (GitHub, Bitbucket, Azure Repos) for APIs not yet behind a runtime gateway.
- **Synchronised API and Product catalog** — Maintain a unified catalog populated from every connected source. Synchronisation is automatic and incremental; gateway-side changes propagate on the next cycle.
- **Consumer-facing metadata enrichment** — Complete each API and Product with the title, overview, long-form documentation, logo, categories, and tags consumers see in the storefront.
- **API governance scoring** — Score every API specification against configurable linting rulesets and surface the score, severity-ranked violations, and remediation guidance on a dedicated Governance Report.
- **Read-only Plan, quota, and rate-limit visibility** — Display the Plan, quota window, and rate-limit ceiling that the connected gateway enforces, so consumers see accurate operational terms before they subscribe.
- **Subscription lifecycle management** — Review pending subscription requests, approve or reject them, and move existing subscriptions through Active, Suspended, and Revoked states.
- **Consumer application management** — Inspect every application a consumer has registered, the credentials issued to it, and the subscriptions linked to it.
- **Provider analytics** — Read time-series traffic data per API, per Organisation, and per consumer, filtered by time range and HTTP status code, and export the underlying data for offline analysis.
- **MCP server and API GPT integration** — Register Model Context Protocol servers that expose a curated subset of APIs to AI agents, and configure the API GPT chat assistant that consumers reach through the storefront.
- **System notifications and webhooks** — Post organisation-wide announcements to every user's inbox and emit outbound webhook deliveries on every subscription, governance, API, and Product event for downstream automation.
- **Identity and access management** — Configure SAML 2.0 single sign-on against an external identity provider, manage built-in and custom roles, organise members into teams, and scope visibility per team.
- **Storefront branding and theming** — Apply per-Organisation branding (logo, favicon, accent colour, theme) and register additional domains for multi-brand deployments.

This guide is written for the **API Provider** and **Portal Admin** roles in the Marketplace deployment. The reader is responsible for connecting the organisation's API gateways, importing APIs into the marketplace, completing the consumer-facing metadata, configuring governance and visibility, approving subscription requests, monitoring usage, and operating the day-to-day administrative surfaces (system notifications, webhooks, email templates, scheduled content). The reader is assumed to be familiar with API gateway concepts in general and to have prior operational experience with at least one supported gateway product. Detailed knowledge of the marketplace itself — its sidebar layout, governance scoring model, subscription lifecycle, and synchronisation behaviour — is not assumed; the chapters that follow introduce each surface in the order an operator typically encounters them during initial onboarding and routine operation.

The chapters are sequenced for an end-to-end onboarding read on first encounter, then function as task-oriented reference material thereafter. Topics that fall outside the API Provider scope — Organisation creation, platform-wide tenancy administration, identity-provider configuration, deep theme customisation — are referenced where relevant and pointed at the corresponding administrator surfaces or companion guides.

- **Introduction** — States the audience, scope, and overall structure of the guide and lists the typographical conventions used throughout.
- **Getting started** — Covers initial sign-in, the Portal Admin dashboard, the left-sidebar groups, and the common UI patterns shared across every administrative surface.
- **Settings & administration** — Describes the personal-scope settings available to every user, including profile, notification preferences, default landing page, session management, and personal API tokens.
- **Troubleshooting** — Lists common operational symptoms together with their cause and resolution. Use the symptom-cause-fix table for fast lookup; the detailed entries that follow include reproduction notes and full diagnostic procedures.
- **FAQs** — Answers questions raised most frequently during onboarding and routine operation, including topics on gateway limits, credential rotation, data retention, and feature behaviour.
- **Glossary** — Defines every product-specific and protocol-specific term used in the guide.

- **Connecting your first gateway** — Register a Gateway Connection against a supported gateway product and verify the credentials so the marketplace can read APIs and Products from it.
- **Importing your first API** — Trigger an automatic import from a connected gateway, confirm the imported API appears in **Manage APIs**, and verify the governance scan ran.
- **Reviewing API governance** — Open the Governance Report, interpret the score breakdown, drill into the top violated rules, and re-run the scan after a remediation pass.
- **Publishing your first API** — Complete the consumer-facing metadata, set the visibility scope, transition the moderation state from Draft to Published, and verify the catalog rendering for an anonymous visitor.
- **Reviewing API Products and Plans** — Review API Products and Plans synchronised from the connected gateway, complete the consumer-facing description and logo, set visibility, and publish. Plan, quota, and rate-limit values are read-only and authored on the gateway side.
- **Onboarding your first consumer** — Review pending subscription requests, approve them, and observe the consumer's first calls in Provider Analytics.
- **Monitoring usage** — Read the Provider Analytics dashboard, filter by time range and HTTP status code, and export the underlying time-series data.
- **Exposing APIs to AI agents** — Register an MCP server, configure the API GPT chat assistant, and test that an AI agent can discover and call the exposed APIs.
- **Managing your team** — Manage Members, Teams, and Roles within an Organisation, and post organisation-wide announcements to every user's inbox.
- **Configuring access and storefront branding** — Configure sign-in methods, set up SAML 2.0 single sign-on, brand the storefront with logo, favicon, and theme, and adjust Organisation-wide preferences.
- **Day-to-day operations** — Operate the static-page editor, the media library, the taxonomy management surfaces, the linting-rule configuration, the webhook delivery pipeline, the email-template editor, the search-path and redirect configuration, and the additional-domain registration page.
