An **API Product** bundles one or more APIs into a single subscription unit — one key, one quota, one rate limit, one row on the consumer dashboard. A **Plan** attaches to a Product and carries the operational terms: how many calls per period, how fast they can arrive, what happens at the cap. On this marketplace, API Products and their Plans are synchronised from the connected gateway and are not authored here. The gateway is the system of record for bundle structure, Plan name, quota, and rate limit; the marketplace mirrors those values for consumers to read but does not author them.

You will learn:

- How to trigger a Product sync from a connected gateway and locate new Products in **Manage API Products**.
- How to recognise which fields on the edit form are gateway-owned (read-only) and which are marketplace-owned (editable).
- How to author the consumer-facing metadata the marketplace owns — Title, Overview, Documentation, logo, categories, tags.
- How to set Visibility, transition the moderation state to Published, and schedule a future publish.
- How to recognise stale or removed Products and reconcile them with the gateway state.

Allow ~30 minutes per Product for the first sync-and-enrich cycle, plus 5 minutes per re-sync.

## How Products and Plans reach the marketplace

Each gateway implements its own version of the bundle idea — Apigee Products bundle proxies, Kong groups Services into bundles, AWS API Gateway uses Usage Plans, MuleSoft uses API Groups. Each also carries a Plan attached to that bundle that defines the runtime quota and rate-limit numbers the gateway will enforce on every call. The marketplace normalises all of these into a single **API Products** list on the Provider side, with the Plan and its numbers shown alongside the Product they apply to.

Because the gateway is the only system that can enforce the quota and rate-limit at runtime, the marketplace deliberately treats those values as read-only mirrors. A figure shown on the marketplace catalog that did not match what the gateway actually enforces would mislead consumers. The next sync rewrites every gateway-controlled field to whatever the gateway currently reports.

The rule of thumb is straightforward. If a value affects how the gateway accepts or rejects a runtime call — quota, period, rate limit, which APIs are in the bundle, the Plan tier name — change it in the gateway. If a value affects how a consumer reads the catalog tile and detail page — title, overview, logo, categories, tags, visibility, moderation state — change it on the marketplace.

#### Trigger a sync from a connected gateway

Use this task when a new Product has been published on the gateway and you want it to appear on the marketplace, or when an existing Product's APIs, Plan, quota, or rate limit have changed on the gateway and you need the marketplace catalog to catch up.

#### Before you start

- **Confirm the gateway connection is healthy.** From the left sidebar, choose **Manage API Sources** and check that your gateway shows status **Connected**. A red status here means no Product can sync until the connection is repaired (Chapter 3).
- **Know which gateway issued the Product.** If your portal has connections to several gateways, sync each one separately. The marketplace does not auto-deduplicate Products that happen to share a name across gateways.
- **Allow time for the gateway to settle.** A Product or Plan change made on the gateway in the last few seconds may not yet be reachable through the gateway's admin API. Wait a minute before triggering a sync after a gateway-side change.

To trigger a Product sync:

1. From the left sidebar, choose **Manage API Sources**.
2. Locate the row for the gateway connection that owns the Product. The list shows the connection name, the gateway type, and the last sync timestamp.
3. Click the **Import Products** action on that row. The marketplace pulls the current Product list from the gateway through the same adapter that imports APIs.
4. Wait for the sync banner to confirm completion. The list refreshes; new Products appear in **Manage API Products** with their Plan, quota, and rate-limit fields populated from the gateway.
5. Open **Manage API Products** to confirm the new Product is visible and that its Plan column reflects the gateway state.

> **Result:** The Product is on the marketplace. Its Plan, quota, rate-limit, and bundled-API fields are filled in from the gateway. The consumer-facing fields — title, description, logo, categories, visibility, status — are still empty and waiting for you to author them.

> **Note:** A sync is read-only against your gateway. The marketplace pulls metadata; it does not push changes back. To rename a Product, change the APIs inside it, raise a quota, or relax a rate limit, change the value on the gateway and re-sync.

> **Tip:** Schedule a recurring sync if your gateway team ships Product or Plan changes often. The cadence setting lives on the connection edit form covered in Chapter 3.

#### Verify

1. Confirm the sync banner reports completion without error.
2. Open Manage API Products and confirm the new or updated Product is listed.
3. On the Product row, confirm the **Plan**, **Quota**, and **API Source** columns reflect the gateway state.

## Reviewing a synced Product

Once a Product has synced, the work shifts from the gateway-facing screens to the **Manage API Products** list. Every Product the marketplace knows about is on this list, regardless of which gateway it came from. The list is also where you discover stale rows, archived rows, and Products waiting on consumer-facing copy.

#### Open a synced Product in the edit form

Use this task as the entry point for every other task in this chapter. The edit form is where read-only Plan fields are inspected and where the marketplace-owned consumer-facing fields are authored.

#### Before you start

- **Confirm the Product has synced from the gateway.** The row appears in Manage API Products only after [Trigger a sync from a connected gateway](#trigger-a-sync-from-a-connected-gateway) has run.
- **Know the gateway connection name.** Use the **API Source** filter to narrow the list when your portal has connections to several gateways.
- **Have your consumer-facing copy and logo to hand.** The edit form is where you paste both; switching back and forth is slower than entering them in one pass.

To open the edit form:

1. From the left sidebar, choose **API MANAGEMENT**, then **Manage API Products**. The list shows every synced Product with its title, the source gateway connection, the Plan name, the quota figure, and the current status.
2. Use the title search at the top of the list, or the **API Source** filter, to narrow the view to the Product you want.
3. Click the **Edit** action on the row. The marketplace opens the edit form pre-populated with the gateway-side metadata. Plan, quota, rate-limit, and bundled-API fields are visible but read-only; title, description, logo, categories, tags, visibility, and status are editable.

![Figure 7-1. The Manage API Products list with synced Products and the per-row Edit action.](.gitbook/assets/screenshots/provider/admin-manage-api-products.png)

The numbered callouts in Figure 7-1 are:

1. **Title column** — The consumer-facing Product name. New synced rows show the gateway-supplied name until you replace it on the edit form. Marketplace-controlled. Sort the list by this column to scan a long catalog quickly.
2. **Status column** — Shows **Draft**, **Needs Review**, **Published**, or **Archived**. Marketplace-controlled. State changes are made on the edit form, never inline on this list.
3. **API Source column** — Names the gateway connection the Product was synced from. In a multi-gateway portal this column tells you at a glance which gateway team owns the bundle. Filter the list by this column to focus on one gateway at a time.
4. **Plan column** — Shows the synced Plan name attached to the Product. Read-only on the marketplace; the gateway is the system of record.
5. **Quota column** — Shows the quota figure the gateway will enforce. Read-only on the marketplace; the gateway is the system of record.
6. **Edit action** — Opens the edit form. The form exposes the read-only gateway fields for review and the editable marketplace fields for authoring.

> **Note:** Two tabs sit above the list — the default tab shows live Products and a **Scheduled** tab shows Products with a future publish date set on the right rail of the edit form. See [Schedule a future publish date](#schedule-a-future-publish-date) for the scheduling mechanic.

#### Verify

1. Confirm the edit form opens with the gateway-supplied Title pre-populated.
2. Confirm the right rail shows the **Status** panel with the Product's current moderation state.
3. Confirm the read-only Plan, quota, and rate-limit fields render without input controls.

#### Recognise read-only versus editable fields

Use this task on the first Product you open. Spending a minute on the visual cues here saves a recurring source of confusion later.

The edit form mixes fields the gateway owns with fields the marketplace owns. The gateway-owned fields appear in the upper portion of the form and on the right rail under the bundle metadata; they render greyed out, are tagged with a synced-from-gateway indicator, or are surfaced as plain text instead of input controls. The marketplace-owned fields render as normal inputs — text boxes, rich-text editors, file pickers, dropdowns, radios.

A complete read/write split for a synced Product:

| Element | Owner | Where to change it |
|---|---|---|
| Product structure (which APIs are bundled) | Gateway | In the gateway, then re-sync |
| Plan name and Plan tier | Gateway | In the gateway, then re-sync |
| Quota (calls per period) | Gateway | In the gateway, then re-sync |
| Quota period (per minute, hour, day, month) | Gateway | In the gateway, then re-sync |
| Rate limit (calls per second or minute) | Gateway | In the gateway, then re-sync |
| Other Plan terms (burst, overage) | Gateway | In the gateway, then re-sync |
| **Title** (consumer-facing display name) | Marketplace | Marketplace edit form |
| **Description** / **Overview** / **Documentation** | Marketplace | Marketplace edit form |
| **Logo** image | Marketplace | Marketplace edit form |
| **Categories** and **Tags** | Marketplace | Marketplace edit form |
| **Visibility** (Public / Internal / Org Level) | Marketplace | Marketplace edit form |
| **Status** (Draft / Published / Archived) | Marketplace | Marketplace edit form |

> **Caution:** Editing a quota, rate-limit, or other Plan-tier value on the marketplace appears to save but does not reach the gateway. The next sync overwrites the local edit with the value the gateway reports. To change a Plan term, change it in the gateway and trigger a re-sync.

> **Note:** The marketplace deliberately keeps Plan terms read-only because the gateway is the only system that can enforce them at runtime. A quota figure in the marketplace catalog that did not match what the gateway enforces would mislead consumers into expecting headroom they do not have.

> **Tip:** When a consumer reports they are hitting a 429 well before the quota number shown on the catalog, the first thing to check is whether the marketplace is showing a stale figure. Trigger a sync from **Manage API Sources** to re-align before assuming a gateway misconfiguration.

## Enriching the consumer-facing metadata

The marketplace-owned fields are where Provider effort actually lives. They are the back-cover blurb a consumer reads before they subscribe — the title that appears in search results, the paragraph that explains what the bundle does, the longer reference content, the icon next to the tile, the filters that surface the bundle to the right audience.

#### Complete the Product description and logo

Use this task once a Product has synced from the gateway and you want consumers to be able to find, read, and decide on it.

#### Before you start

- **Confirm the underlying APIs are published.** A Product whose APIs are still in Draft will surface to consumers but reject calls. Publish the APIs first (Chapter 6) and only then enrich the Product.
- **Prepare the consumer-facing copy.** Two or three sentences for the Overview that state what the bundle does, what data it returns, and which audience tier it targets. Reserve detailed reference material for the Documentation field.
- **Prepare a square logo file (PNG or SVG, 256×256 or larger).** The Product tile crops to a square. A wide wordmark renders poorly next to other tiles.
- **Agree the category taxonomy with your team.** The same domain values you set on APIs in Chapter 6 reappear here as Product filters. Inconsistency between APIs and Products breaks the consumer's filter experience.

To enrich a synced Product:

1. From **Manage API Products**, click the **Edit** action on the row.
2. Replace the **Title** with the consumer-facing name a subscriber would recognise. The gateway-supplied name often contains a version suffix or an internal codename.
3. Complete the **Overview** field with two or three sentences. Choose **Markdown** from the **Text format** dropdown when you need lists or links.
4. Complete the **Documentation** field with longer reference content — getting-started steps, sample request and response payloads, error codes, support contact. Two to four paragraphs is typical.
5. Upload your square logo image to the **Logo** file field below the description.
6. Set the **Categories** and **Tags** fields. Use the same taxonomy you applied to the underlying APIs.
7. Leave the **Status** at **Draft** for now. Publish in a later task once visibility is set.
8. Click **Save**. The Product remains in draft state, but the consumer-facing metadata is committed.

> **Result:** The Product now has a human-readable title, an overview, longer reference documentation, a logo, and categories. The status is still **Draft**, so consumers cannot see it yet.

> **Note:** Edits to consumer-facing fields survive the next sync. The marketplace overwrites only the gateway-controlled fields — bundle name, attached APIs, Plan, quota, rate limit — and preserves the title, description, logo, categories, tags, visibility, and status.

> **Tip:** Treat the Overview as a back-cover blurb. Consumers decide whether to subscribe within twenty seconds of opening the tile. Shorter copy with one strong sentence outperforms long copy.

#### Verify

1. Reload the edit form and confirm Title, Overview, Documentation, Categories, and Tags retain your values.
2. Confirm the uploaded logo renders next to the Title on the Manage API Products list.
3. Confirm the **Status** column on Manage API Products still reads Draft — publication has not happened yet.

#### Set the visibility scope

Use this task before publishing the Product. Visibility decides which audience the Product tile is rendered for, and it is one of the marketplace-owned fields — not synced from the gateway.

The visibility radio group on the edit form is the same three-option control you saw on APIs in Chapter 6:

1. On the edit form for the synced Product, scroll to the **Visibility** radio group.
2. Select one of:
   - **Org Level — visible only to members of this organisation.** The narrowest scope. The tile never appears in the public catalog and is hidden from users outside your organisation. Pair with the Teams picker for finer-grained access.
   - **Internal — visible to all logged-in users.** Mid scope. The tile is shown to anyone with a marketplace account regardless of organisation. Useful for partner programs.
   - **Public — visible to everyone including anonymous visitors.** Widest scope. The tile is shown to signed-out visitors on the public catalog and is eligible for indexing.
3. If you select **Org Level**, scroll to the Teams picker and choose one or more teams (for example, **QA1**, **TestingTeam**) to limit visibility further. Leave all teams unselected to keep the Product visible to every member of your organisation.
4. Click **Save**.

> **Result:** The Product visibility is set. The Product does not become live until the moderation state changes in the next task.

> **Note:** Product-level visibility takes precedence over the visibility of the underlying APIs. A Public Product containing an Internal API will show the tile to anonymous visitors, but the **Subscribe** action prompts them to sign in before they can complete the subscription.

> **Tip:** Default new Products to **Internal** while your portal is small. Promote them to **Public** in batches once descriptions and Plan figures have been verified against the gateway.

#### Verify

1. Reload the edit form and confirm the Visibility radio remains on the option you selected.
2. If you selected Org Level, confirm the Teams picker reflects your team selection.
3. Open the Product detail page from an account in the intended audience and confirm the tile renders.

## Publishing a Product

Publishing a Product is one click on the Status panel — the same pattern you used for an API in Chapter 6. The publish action does not touch the gateway; it only flips the marketplace-side state that decides whether the consumer catalog renders the tile.

#### Move a Product from Draft to Published

Use this task once the Product has consumer-facing metadata and visibility set, and the gateway-side Plan reflects the terms you want consumers to see.

#### Before you start

- **Re-read the Overview as a consumer.** This is the page a subscriber lands on after clicking the tile.
- **Confirm every bundled API is published.** An unpublished API in a published Product is hazardous: consumers see the Product, subscribe, and receive 404s on their first call.
- **Confirm the gateway-reported Plan figures match what you intend to advertise.** If they do not, fix the gateway first and re-sync — do not publish.

To publish a Product:

1. From **Manage API Products**, locate your Product and click the **Edit** action.
2. On the edit form, scroll to the right rail and find the **Status** panel.
3. Under **Change to**, select **Published** from the dropdown.
4. (Optional) Enter a short note in **Revision log message** — for example, *Initial release, Pro tier*. The note is stored with the revision and helps later retrieval of the change.
5. Click **Save**.
6. Open an incognito window and navigate to your portal's API Products listing. Confirm the page renders the Product with the title, description, logo, bundled APIs, Plan name, quota, rate-limit, and a visible **Subscribe** action.

![Figure 7-2. An alternative view of the Manage API Products list, useful when verifying that a freshly published Product has appeared with the right Plan and Status.](.gitbook/assets/screenshots/provider/admin-manage-api-products-2.png)

> **Result:** The Product row in **Manage API Products** now shows **Status: Published**. The detail page renders for the selected audience. The first subscription request lands in your queue and begins the Chapter 8 onboarding flow.

> **Caution:** Publishing a Product whose underlying APIs are still in Draft will surface a Product to consumers that they cannot subscribe to. Publish APIs first, then the Product.

> **Note:** When the gateway-side Product changes, the next sync overwrites the gateway-controlled fields — bundle name, attached APIs, Plan, quota, rate limit. Marketplace edits to consumer-facing fields are preserved.

#### Verify

1. Confirm the **Status** column on Manage API Products reads Published for the Product.
2. Open an incognito window and confirm the Product tile renders for the configured audience on the consumer catalog.
3. Click the tile and confirm the detail page shows the bundled APIs, the Plan, the quota, the rate limit, and a **Subscribe** action.

#### Schedule a future publish date

Use this task when a Product is ready but should go live at a fixed point in the future — for example, an embargoed launch tied to an announcement.

#### Before you start

- **Confirm the marketplace timezone.** The schedule fields use the timezone of the marketplace, not your browser. Check your profile if uncertain.
- **Confirm the consumer-facing metadata is final.** The schedule will publish the Product as it stood at the moment the timer fires, including any unsaved changes you commit between now and then.
- **Coordinate with any announcement or press release.** A scheduled publish without an audience-facing announcement leaves the catalog tile to be discovered by chance.

To schedule a publish:

1. Open the Product on the edit form.
2. Expand the **Publishing options** panel on the right rail.
3. Set the **Publish on** field to the target date and time. The marketplace flips the Status to **Published** automatically when the schedule fires.
4. Click **Save**. The Product appears on the **Scheduled** tab of **Manage API Products** until the publish time arrives.

![Figure 7-3. The Scheduled tab of Manage API Products, showing Products with a future publish time.](.gitbook/assets/screenshots/provider/admin-manage-api-products-scheduled.png)

> **Result:** The Product is queued for an automatic publish at the time you set. Until then, it is hidden from the consumer catalog and visible only on the **Scheduled** tab.

> **Tip:** Pair a scheduled publish with an announcement on your portal's news or blog so consumers see the Product the same hour they receive the email.

#### Verify

1. Confirm the Product appears under the **Scheduled** tab of Manage API Products after **Save**.
2. Confirm the right rail of the edit form shows the Publish on date and time you set.
3. After the scheduled time passes, reload Manage API Products and confirm the Status has flipped to Published on the default tab.

## Handling stale and removed Products

The marketplace does not assume what to do when the gateway-side Product changes underneath a published catalog tile. Instead it flags the row and leaves the decision to you. Different gateway-side changes call for different responses, and an automated reaction would surprise both providers and consumers.

#### Recognise a stale Product

Use this task on every visit to **Manage API Products**. A stale row is the marketplace telling you the catalog and the gateway no longer agree.

A row is flagged stale when any of the gateway-controlled fields — bundle name, attached APIs, Plan name, quota, rate limit — has changed on the gateway since the last sync. A row is flagged removed when the Product has been deleted on the gateway entirely; the marketplace keeps the row as a tombstone to protect existing subscribers.

To recognise the indicators:

1. Open **Manage API Products**.
2. Look for the *stale* badge alongside the **Status** column. Sort by **Last sync** to surface the oldest rows first.
3. Look for the *removed* badge on rows where the gateway has deleted the underlying Product.
4. Click the row title to open the read-only summary, or **Edit** to open the form.

> **Note:** The marketplace will not auto-archive a removed Product. The decision to archive is yours, because the right response varies — for example, a temporary gateway-side delete during a refactor should not trigger consumer churn.

#### Re-sync after gateway-side changes

Use this task when a stale or removed badge appears, or when you know a gateway-side change has happened and want the catalog to reflect it without waiting for the scheduled sync.

To re-sync and reconcile:

1. From the left sidebar, choose **Manage API Sources**.
2. Locate the connection for the affected Product and click **Import Products**.
3. Wait for the sync banner to confirm completion.
4. Return to **Manage API Products** and re-open the affected row. Confirm the Plan, quota, rate-limit, bundled APIs, and Title now match the gateway state.
5. If the gateway has removed the Product, open the edit form, change the **Status** to **Archived**, add a Revision log message stating why, and click **Save**. Archived Products are hidden from the consumer catalog but remain visible to existing subscribers as read-only history.
6. Notify subscribers through your preferred channel — email, dashboard banner, or the portal's announcements feed.

> **Result:** The Product state on the marketplace matches the gateway. Active subscribers retain visibility into the change history; new consumers cannot discover an archived Product.

> **Caution:** Do not delete a Product on the marketplace as a way to handle a removed gateway-side Product. Deletion erases revision history and breaks subscriber dashboards. **Archived** is the correct end state.

> **Tip:** Schedule a recurring sync rather than relying on manual re-sync after every gateway change. The cadence is configured under the connection on **Manage API Sources** — see the Day-to-day operations chapter for the scheduling controls.

#### Verify

1. Confirm the *stale* badge has cleared from the Manage API Products row after the sync completes.
2. Open the edit form and confirm the gateway-controlled fields now match the gateway state.
3. For removed Products, confirm the Status now reads Archived and the row no longer appears on the consumer catalog.

## Next steps

- **[Onboarding your first consumer](onboarding-your-first-consumer.md#onboarding-your-first-consumer)** — With a published Product in the catalog, the next step is approving the first subscription and walking the consumer through obtaining a key.
- **[Monitoring usage](monitoring-usage.md#monitoring-usage)** — Once consumers begin calling, monitor latency, errors, and quota consumption from the analytics dashboards.
- **[Publishing your first API](publishing-your-first-api.md#publishing-your-first-api)** — Add additional APIs to the bundle by publishing them on the gateway and re-syncing the Product.
- **[Connecting your first gateway](connecting-your-first-gateway.md#connecting-your-first-gateway)** — If a Product needs Plan changes, change them in the gateway and re-sync; the connection edit form is also where you adjust sync cadence.
