---
icon: code
---

The Marketplace Admin API is the headless control plane for the marketplace: everything an administrator does in the portal console can be driven programmatically. Use it to discover and configure API sources, browse and import gateway inventories, manage the API and product catalogue, run the approval queues, administer users and organisations, and publish to the developer portal.

{% hint style="info" %}
**Note:** The base URL for every endpoint is `https://<your-portal-domain>/api/admin/v1`. The API is versioned in the path; this page documents version 1 of the surface (API document version 2.0.0).
{% endhint %}

## Authentication and authorisation

The API authenticates with Firebase Authentication ID tokens. Exchange a portal administrator's email and password for a token at `POST /auth/token`, then send it on every request:

```bash
curl -s https://<portal>/api/admin/v1/auth/token \
  -H 'Content-Type: application/json' \
  -d '{"email": "<admin email>", "password": "<password>"}'

curl -s https://<portal>/api/admin/v1/apis \
  -H 'Authorization: Bearer <id token>'
```

Tokens are short lived; exchange the refresh token at `POST /auth/token/refresh` without re-sending credentials. The token establishes identity only. Every endpoint additionally requires the baseline portal permission `access marketplace admin api`, plus the per-domain permission listed in the tables below, so an operator grants exactly the surface each integration needs.

{% hint style="success" %}
**Tip:** Token issuance is disabled by default. An operator enables it by setting the Firebase Web API key in the Admin API settings. A separate, default-off configuration toggle allows named users to call the API with their portal session instead of a token, intended for local development and break-glass access; every use of that bypass is logged.
{% endhint %}

## Request and response conventions

Every endpoint shares one contract, so a client written for one resource works for all of them:

* **Envelope.** Successful responses return `{"data": ...}`, with collections adding `meta` (pagination counts) and `links` (first, previous, next, last).
* **List contract.** Collections accept `page` (1-based), `limit` (default 50), `sort`, and `filter[field]=value` query members. Filter and sort keys are validated against each endpoint's allow-list; unknown keys are rejected rather than ignored.
* **Errors.** Failures are RFC 7807 `application/problem+json` documents with `type`, `title`, `status`, and `detail` members.
* **Bulk operations.** Bulk endpoints return HTTP 200 with a per-item report; inspect `meta.failed` for the items that did not succeed.
* **Long-running work.** Imports and syncs that take time return HTTP 202 with a `job_id`; poll `GET /jobs/{id}` for status and progress.
* **Writes.** Creation returns HTTP 201 with a `Location` header; deletion returns HTTP 204.

## Endpoints

All paths below are relative to `/api/admin/v1`. The permission column lists what is required in addition to the baseline `access marketplace admin api` permission.

### Authentication

Token issuance is the only unauthenticated surface, and it is disabled until an operator configures a Firebase Web API key. Issuance is rate limited to 10 attempts per IP in a 5 minute window, failure responses are deliberately uniform so accounts cannot be enumerated, and credentials are never logged.

<details>

<summary>Authentication: 2 endpoints</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| POST | `/auth/token` | Exchanges an email + password for a Firebase ID token. The only unauthenticated endpoint. | none (public by design) |
| POST | `/auth/token/refresh` | Exchanges a refresh token for a fresh ID token, without re-sending credentials. | none (public by design) |

</details>

### API description

The API describes itself. The OpenAPI 3.1 document is generated from the live route definitions, so it only advertises endpoints this portal can actually serve, and it prunes endpoints whose backing module is not installed.

<details>

<summary>API description: 1 endpoint</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| GET | `/openapi.json` | Returns the OpenAPI 3.1 description of this API, unwrapped. | baseline only |

</details>

### Source types

Source types are the connector catalogue: each entry describes one supported API source with its capability map and the connection field descriptors a client needs to build a connection form dynamically.

<details>

<summary>Source types: 2 endpoints</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| GET | `/sources` | Lists every supported API source type with its capability map and feature-flag state. | `administer gateway sync` |
| GET | `/sources/{source}` | Returns one source type including the connection-field descriptors a client builds the credentials object from. | `administer gateway sync` |

</details>

### Gateway connections and imports

Connections are validated against the upstream gateway before they are saved, and can be re-tested at any time. Remote inventory endpoints list what the gateway currently exposes, with each row marked as already imported or not. Imports run inline for single items and as pollable jobs for bulk work.

<details>

<summary>Gateway connections and imports: 16 endpoints</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| GET | `/gateways/connections` | Lists gateway connections visible to the caller. | `administer gateway sync` |
| POST | `/gateways/connections` | Creates a gateway connection after the plugin confirms it can reach the upstream. | `administer gateway sync` |
| POST | `/gateways/connections/test` | Validates a set of credentials against a gateway plugin without saving anything. | `administer gateway sync` |
| DELETE | `/gateways/connections/{gateway_connection}` | Deletes a gateway connection. | `administer gateway sync` |
| GET | `/gateways/connections/{gateway_connection}` | Returns one gateway connection. | `administer gateway sync` |
| PUT | `/gateways/connections/{gateway_connection}` | Replaces a gateway connection after the plugin re-confirms reachability. Full credentials required , no partial updates. | `administer gateway sync` |
| GET | `/gateways/connections/{gateway_connection}/apis` | Lists APIs imported from one specific gateway connection. | `access content` |
| POST | `/gateways/connections/{gateway_connection}/apis/import` | Fetches ONE API from the gateway by api_id and persists it inline. Same code path as the async import queue, run synchronously so the caller gets the resulting node back without polling. | `administer gateway sync` |
| POST | `/gateways/connections/{gateway_connection}/apis/import/all` | Synchronously imports all APIs from the gateway connection (or a supplied subset via api_ids[]). Runs the same logic as the admin-UI "Import All" batch but inline, returning a per-API result report without polling. | `administer gateway sync` |
| POST | `/gateways/connections/{gateway_connection}/jobs` | Triggers an async job against a gateway connection (currently import-apis; more kinds land as follow-up). | `administer gateway sync` |
| GET | `/gateways/connections/{gateway_connection}/products` | Lists API products imported from one specific gateway connection. | `access content` |
| POST | `/gateways/connections/{gateway_connection}/products/import` | Fetches ONE product from the gateway by product_id and persists it inline. Auto-imports any associated APIs that are not already present, matching the queue worker. | `administer gateway sync` |
| POST | `/gateways/connections/{gateway_connection}/products/import/all` | Synchronously imports all products from the gateway connection (or a supplied subset via product_ids[]). Auto-imports associated APIs. Returns a per-product result report without polling. | `administer gateway sync` |
| GET | `/gateways/connections/{gateway_connection}/remote/apis` | Lists the APIs currently exposed by the gateway behind a connection, each marked imported/node_id. Filters: search, api_id, name, import_status=new\|existing; ?refresh=1 bypasses the five-minute upstream cache. | `administer gateway sync` |
| GET | `/gateways/connections/{gateway_connection}/remote/products` | Lists the API products currently exposed by the gateway behind a connection, each marked imported/node_id. | `administer gateway sync` |
| POST | `/gateways/connections/{gateway_connection}/test` | Re-verifies a stored connection against its gateway without re-supplying credentials. Persists nothing. | `administer gateway sync` |

</details>

### API and product catalogue

The catalogue covers every API and API product on the portal, whether imported from a connection or created natively. Deleting an API revokes its active and pending subscriptions; deleting a product also disassociates its linked APIs.

<details>

<summary>API and product catalogue: 10 endpoints</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| GET | `/apis` | Lists every API on the portal , imported from any connection, plus custom. `filter[source]=custom\|imported\|<connection_id>` narrows. | `access content` |
| POST | `/apis` | Creates an API. Omit `connection_id` for a portal-native (custom) row; supply it (plus `api_id`, `uuid`, `revision` as available) to wire the row to a specific gateway connection. | `create apis content` |
| DELETE | `/apis/{node}` | Deletes an API node. Revokes all active/pending subscriptions for this API only , other apps are unaffected. | `delete any apis content` |
| GET | `/apis/{node}` | Returns one API with source metadata. | `access content` |
| PATCH | `/apis/{node}` | Updates editable fields on an API (title, version, overview, documentation, organisation). | `edit any apis content` |
| GET | `/products` | Lists every API product on the portal , imported from any connection, plus custom. `filter[source]=custom\|imported\|<connection_id>` narrows. | `access content` |
| POST | `/products` | Creates an API product. Omit `connection_id` for a portal-native (custom) row; supply it (plus `product_id`, `gateway_type` as available) to wire the row to a specific gateway connection. | `create api_products content` |
| DELETE | `/products/{node}` | Deletes an API product. Disassociates linked APIs, revokes all active/pending subscriptions for this product only, then deletes the node. | `delete any api_products content` |
| GET | `/products/{node}` | Returns one API product with source metadata. | `access content` |
| PATCH | `/products/{node}` | Updates editable fields on an API product (title, description, associated_apis, quota, organisation). | `edit any api_products content` |

</details>

### Publishing and syndication

Bulk operations over the catalogue: apply moderation transitions, set visibility (organisation, internal, or public), and manage cross-organisation portal syndication. Bulk endpoints return HTTP 200 with a per-item report; check meta.failed.

<details>

<summary>Publishing and syndication: 4 endpoints</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| GET | `/publishing/portals` | Lists the portal organisations syndication can target, with their domains. | `syndicate apis to portals` |
| POST | `/publishing/portals` | Adds/removes/sets cross-org portal syndication on APIs/products. Super-administrators only; product-implied publications are preserved and reported. | `syndicate apis to portals` |
| POST | `/publishing/transitions` | Applies one moderation transition to many APIs/products. Per-item report at HTTP 200; check meta.failed. | `access content` |
| POST | `/publishing/visibility` | Sets org/internal/public visibility on many APIs/products. Leaving org clears team restrictions, as the admin UI does. | `access content` |

</details>

### Content and moderation

Editorial control over portal content: list what awaits a decision, inspect the transitions available to the caller, apply a transition, or publish and unpublish content that is not under editorial moderation.

<details>

<summary>Content and moderation: 7 endpoints</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| GET | `/content/moderated` | Lists content awaiting an editorial decision across all bundles. | baseline only |
| GET | `/content/{bundle}` | Lists nodes of one content bundle. | baseline only |
| GET | `/content/{bundle}/{node}` | Returns one node with the transitions available to the caller. | baseline only |
| POST | `/content/{bundle}/{node}/publish` | Publishes a node that is not under editorial moderation. | baseline only |
| GET | `/content/{bundle}/{node}/transitions` | Lists the moderation transitions the caller may apply to a node. | baseline only |
| POST | `/content/{bundle}/{node}/transitions/{transition}` | Applies a moderation transition to a node. | baseline only |
| POST | `/content/{bundle}/{node}/unpublish` | Unpublishes a node that is not under editorial moderation. | baseline only |

</details>

### Subscriptions

The approval queue for API and product subscriptions. Rejections and revocations require a reason, which is recorded and included in the notification to the consumer.

<details>

<summary>Subscriptions: 6 endpoints</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| GET | `/subscriptions` | Lists API and product subscriptions, pending first. | `administer apim subscriptions` |
| GET | `/subscriptions/{subscription}` | Returns a single subscription. | `administer apim subscriptions` |
| GET | `/subscriptions/{subscription}/analytics` | Returns per-subscription analytics reports from the upstream gateway. | `view api analytics` |
| POST | `/subscriptions/{subscription}/approve` | Approves a pending subscription request. | `administer apim subscriptions` |
| POST | `/subscriptions/{subscription}/reject` | Rejects a pending subscription request. Requires a reason. | `administer apim subscriptions` |
| POST | `/subscriptions/{subscription}/revoke` | Revokes an active subscription. Requires a reason. | `administer apim subscriptions` |

</details>

### Organisations, members and teams

Organisation administration: list organisations, manage members and their group roles, invite people by email, and inspect teams.

<details>

<summary>Organisations, members and teams: 7 endpoints</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| GET | `/organisations` | Lists organisations. | `administer marketplace organisations` |
| GET | `/organisations/{group}` | Returns one organisation. | `administer marketplace organisations` |
| GET | `/organisations/{group}/members` | Lists an organisation's members. | `manage org members` |
| POST | `/organisations/{group}/members/invite` | Invites one or more people to an organisation by email. | `manage org members` |
| DELETE | `/organisations/{group}/members/{relationship}` | Removes a member from an organisation, leaving their account intact. | `manage org members` |
| GET | `/organisations/{group}/teams` | Lists an organisation's teams. | `manage org teams` |
| GET | `/organisations/{group}/teams/{team}/members` | Lists a team's members. | `manage org teams` |

</details>

### Partner approvals

Partner registrations awaiting approval, per organisation. Approval sends the activation email; rejection requires a reason and can optionally delete the account.

<details>

<summary>Partner approvals: 3 endpoints</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| GET | `/organisations/{group}/approvals` | Lists partner registrations awaiting approval. | `manage org members` |
| POST | `/organisations/{group}/approvals/{relationship}/approve` | Approves a pending partner registration and sends the approval email. | `manage org members` |
| POST | `/organisations/{group}/approvals/{relationship}/reject` | Rejects a pending partner registration. Requires a reason; account deletion is opt-in. | `manage org members` |

</details>

### Organisation roles and permissions

Custom organisation roles and their permission grants. Predefined roles cannot be edited or deleted; attempts are refused with HTTP 409.

<details>

<summary>Organisation roles and permissions: 10 endpoints</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| GET | `/organisations/permissions` | Lists every ORGANISATION permission a custom org role may grant, grouped by category. Call before creating an org role. | `manage org members` |
| DELETE | `/organisations/{group}/members/{relationship}/custom-roles/{role}` | Removes a custom-role assignment from a member. | `manage org members` |
| POST | `/organisations/{group}/members/{relationship}/custom-roles/{role}` | Assigns a custom role to a member. | `manage org members` |
| GET | `/organisations/{group}/members/{relationship}/roles` | Returns a member's directly-assigned roles, team-inherited roles, and the union of permissions they effectively hold. | `manage org members` |
| PATCH | `/organisations/{group}/members/{relationship}/roles` | Replaces the group roles held by a member. | `manage org members` |
| GET | `/organisations/{group}/roles` | Lists an organisation's custom roles and the available permission keys. | `manage org members` |
| POST | `/organisations/{group}/roles` | Creates a custom role in an organisation. | `manage org members` |
| DELETE | `/organisations/{group}/roles/{role}` | Deletes a custom role. Predefined roles are refused with 409. | `manage org members` |
| GET | `/organisations/{group}/roles/{role}` | Returns one custom role. | `manage org members` |
| PATCH | `/organisations/{group}/roles/{role}` | Updates a custom role. Predefined roles are refused with 409. | `manage org members` |

</details>

### Users and site roles

Portal account administration: list and inspect accounts, work the pending registration queue, block and unblock, and inspect site-wide roles and the permissions they grant.

<details>

<summary>Users and site roles: 12 endpoints</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| GET | `/permissions` | Lists every site-wide permission grouped by the module that declares it, including Drupal's restrict-access flag. | `administer permissions` |
| GET | `/roles` | Lists every site-wide user role with the permissions it grants, in admin-UI weight order. | `administer permissions` |
| GET | `/roles/{user_role}` | Returns one site-wide role with its permissions resolved to labels. | `administer permissions` |
| GET | `/users` | Lists portal accounts. | `administer users` |
| GET | `/users/pending` | Lists accounts awaiting approval. | `administer users` |
| GET | `/users/{user}` | Returns one account. | `administer users` |
| POST | `/users/{user}/approve` | Approves a pending registration and sends the activation email. | `administer users` |
| POST | `/users/{user}/block` | Blocks an account. | `administer users` |
| DELETE | `/users/{user}/firebase-identity` | Severs the Firebase identity link for an account. | `administer marketplace admin api` |
| POST | `/users/{user}/reject` | Rejects a pending registration. Requires a reason. | `administer users` |
| GET | `/users/{user}/roles` | Returns one account's roles and the union of site-wide permissions it effectively holds. | `administer permissions` |
| POST | `/users/{user}/unblock` | Reactivates a blocked account without sending a notification. | `administer users` |

</details>

### Feature management

Platform feature toggles and per-organisation overrides, mirroring the Feature Management console.

<details>

<summary>Feature management: 6 endpoints</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| GET | `/features` | Lists every portal feature and its platform-level state. | `manage portal features` |
| POST | `/features/sync` | Reconciles the feature registry with the installed plugins. | `manage portal features` |
| GET | `/features/{feature_key}` | Returns one feature and its platform-level state. | `manage portal features` |
| PATCH | `/features/{feature_key}` | Enables, disables or reconfigures a feature platform-wide. | `manage portal features` |
| GET | `/organisations/{gid}/features` | Lists the effective feature state for one organisation. | `manage portal features` |
| PATCH | `/organisations/{gid}/features/{feature_key}` | Sets or clears an organisation-level override for a feature. | `manage portal features` |

</details>

### Governance reports

Read-only access to API governance results: the latest linting scores across published APIs and the scoring history per API.

<details>

<summary>Governance reports: 3 endpoints</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| GET | `/governance` | Lists the latest linting scores across published APIs. | `view api governance` |
| GET | `/governance/{node}` | Returns the latest linting result for one API. | `view api governance` |
| GET | `/governance/{node}/history` | Returns the recent linting runs for one API. | `view api governance` |

</details>

### API health

The API status surface: dashboard rows with status counts, the health record per API, and an on-demand re-check that is queued immediately.

<details>

<summary>API health: 3 endpoints</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| GET | `/health` | Lists the health dashboard rows with status counts. | `view api health dashboard` |
| GET | `/health/{node}` | Returns the health record for one API. | `view api health dashboard` |
| POST | `/health/{node}/check` | Queues an immediate health check for one API. | `refresh api health` |

</details>

### Audit events

The portal audit log: filterable event listing, aggregate statistics, and a streaming export as NDJSON or CSV.

<details>

<summary>Audit events: 5 endpoints</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| GET | `/events` | Lists portal events, newest first. | `view marketplace audit log` |
| GET | `/events/export` | Streams the matched events as NDJSON or CSV. | `view marketplace audit log` |
| GET | `/events/stats` | Returns aggregate event counts by operation, entity type and day. | `view marketplace audit log` |
| GET | `/events/types` | Lists the operations, entity types, bundles and modules present in the log. | `view marketplace audit log` |
| GET | `/events/{event}` | Returns a single event by ID. | `view marketplace audit log` |

</details>

### Notifications

Read access to site-wide notifications and a per-type summary of the matched set.

<details>

<summary>Notifications: 3 endpoints</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| GET | `/notifications` | Lists site-wide notifications. | `administer marketplace notifications` |
| GET | `/notifications/summary` | Counts notifications by type and audience for the matched set. | `administer marketplace notifications` |
| GET | `/notifications/{notification}` | Returns one notification. | `administer marketplace notifications` |

</details>

### Jobs

Asynchronous work started elsewhere in the API (bulk gateway imports, user syncs) is tracked here. Poll a job for status and progress, or request cancellation; cancelling a finished job is a no-op.

<details>

<summary>Jobs: 3 endpoints</summary>

| Method | Path | Purpose | Additional permission |
|---|---|---|---|
| GET | `/jobs` | Lists admin-API jobs (gateway imports, user syncs). | `administer gateway sync` |
| DELETE | `/jobs/{job}` | Requests cancellation of a queued or running job. Idempotent for terminal jobs. | `administer gateway sync` |
| GET | `/jobs/{job}` | Returns a job's current status and progress. | `administer gateway sync` |

</details>

## Discovering the full schema

Field-level request and response schemas for every endpoint are published by the API itself: `GET /openapi.json` returns the complete OpenAPI 3.1 document, generated from the live route definitions of your portal. Feed it to any OpenAPI tool to generate clients, run contract tests, or browse the schemas interactively. Because the document is derived from the running portal, it always matches the endpoints your installation actually serves.

{% hint style="success" %}
**Result:** A client integration can be built end to end from this page plus the live OpenAPI document: authenticate, read the conventions once, and every one of the 103 endpoints follows them.
{% endhint %}