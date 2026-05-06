# Glossary

Active
:   A subscription state. The consumer can call the API Product within the plan's quota and rate-limit settings.

Admin endpoint
:   The management URL of an API gateway, separate from the runtime endpoint that serves traffic. Required when adding a Gateway connection.

API Consumer
:   The Org-scoped role for users who discover APIs, subscribe to API Products, and call APIs from their Apps. Covered in a separate guide.

API GPT
:   The marketplace's chat assistant. Backed by an MCP server bundle, it answers consumer questions about APIs and helps construct sample calls.

API key
:   A credential issued to an App that authenticates calls to subscribed APIs. Rotatable from the App's settings.

API Product
:   A bundle of one or more APIs sold together under a single subscription. The unit a consumer subscribes to.

API Provider
:   The Org-scoped role responsible for adding API sources, importing APIs, designing API Products, and approving subscriptions. The audience of this guide.

API source
:   A registered location from which APIs are imported into the marketplace. Examples include API gateways (Apigee, Kong, AWS), spec registries (Postman, SwaggerHub), and source-control repositories (GitHub, Bitbucket).

API spec
:   An OpenAPI or Swagger document that describes an API's endpoints, request and response shapes, and authentication. The marketplace renders the spec as interactive documentation.

App
:   A consumer-side container for credentials. An App holds API keys and tracks which subscriptions belong to it. One consumer can have many Apps for different environments or projects.

Draft
:   An API or API Product state. Editable but not visible to consumers. Move to Published when ready.

Gateway connection
:   A specific API source of type 'API gateway'. Holds the gateway's admin endpoint URL and credentials so the marketplace can read and synchronise APIs from it.

Governance score
:   A 0-100 score per API that summarises how many linting rules pass relative to the total weighted by severity. Displayed on the governance report.

Linting rule
:   A check applied to an API spec, such as 'every operation must have a description' or 'security must be defined'. Rules are grouped into rulesets.

MCP server
:   A registered Model Context Protocol server that exposes a subset of marketplace APIs to AI agents. Configured under Portal > MCP Servers.

Member
:   A user who belongs to an Org. Members hold one or more roles within the Org.

Org
:   An organisation in the marketplace. Holds members, roles, branded settings, and the APIs and subscriptions owned by that organisation.

Org Admin
:   The Org-scoped role responsible for inviting members, assigning roles, configuring SSO, and branding the Org's storefront. Covered in a separate guide.

Pending
:   A subscription state. The consumer has requested access; a Provider must approve before the subscription becomes Active.

Plan
:   A pricing tier attached to an API Product, with quota and rate-limit settings. Sometimes called a tier in marketing copy.

Portal Admin
:   The platform-wide role with permissions across all Orgs. Used by the platform operator, not a typical Org user.

Published
:   An API or API Product state. Visible to consumers per the configured Visibility. Edits apply immediately.

Quota
:   An upper limit on the total number of calls a subscriber can make in a billing period. Counts reset at the start of each period.

Rate limit
:   An upper limit on the number of calls a subscriber can make per unit of time, typically per second or per minute. Limits the burst rate, separate from the period quota.

Revoked
:   A subscription state. Calls are blocked and the consumer cannot re-activate; they must request a new subscription.

Role
:   A named bundle of permissions. The default roles are API Provider, Org Admin, API Consumer, and Portal Admin.

Ruleset
:   A named collection of linting rules. The marketplace ships with default rulesets and lets you create your own at Configuration > APIM > API linting.

Subscription
:   A consumer's permission to call an API Product under a chosen plan. Subscriptions move through Pending, Active, Suspended, and Revoked states.

Suspended
:   A subscription state. Calls are blocked but the subscription record is retained. Reversible by re-approving.

System notification
:   A message posted by a Portal Admin that appears in every user's notification inbox. Used for outage announcements, release notes, and policy updates.

Visibility
:   An API or API Product setting that controls who can see it. Options are Public (anyone), Authenticated (any signed-in user), and Private (members of the Org only).

Webhook
:   An outbound HTTP callback the marketplace sends when subscription, API, or governance events occur. Configured under Portal > Webhooks.
