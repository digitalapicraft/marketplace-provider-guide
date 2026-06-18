---
icon: box-archive
---

An API Product bundles one or more APIs into a single subscription unit: one key, one quota window, one rate-limit ceiling, one row on the consumer dashboard. A Plan attaches to the Product and carries the operational terms the gateway enforces at runtime, which is how many calls per period, how fast they can arrive, and what happens at the cap. Products and their Plans are read in from the connected gateway, so the gateway is the system of record for which APIs sit inside a bundle, what the Plan is called, what the quota figure is, what the period is, and what the rate-limit ceiling is. The marketplace mirrors those values for consumers to read, then asks you to author the consumer-facing copy that sits beside them.

![Figure. The Manage API Products list with synced Products and the per-row Edit action.](.gitbook/assets/screenshots/provider/admin-manage-api-products.png)

The Plan, quota window, rate-limit ceiling, and the set of bundled APIs are authored on the gateway and synced into the marketplace **read-only**. The gateway is the only system that can enforce these terms on a real call. Editing a quota, rate limit, or Plan tier on the marketplace edit form appears to save but does not reach the gateway, and the next sync overwrites the local change with whatever the gateway reports. To change a Plan term, change it on the gateway and re-sync from **Manage API Sources**.

{% hint style="info" %}
**Note:** The rule of thumb. If a value affects how the gateway accepts or rejects a runtime call (quota, period, rate limit, which APIs are in the bundle, the Plan tier name), change it on the gateway. If a value affects how a consumer reads the catalog tile and detail page (title, overview, logo, categories, tags, visibility, moderation state), change it on the marketplace.
{% endhint %}

## What you see

**Manage API Products** is the catalog's Product control surface. Every Product the marketplace knows about is listed here regardless of which gateway it came from. The page opens with the **Live** tab selected; a second **Scheduled** tab sits beside it and lists Products waiting on a future publish timer.

Read the columns left to right. The default order surfaces identity first, source second, Plan terms third, lifecycle last:

- **Title**: the consumer-facing Product name. A new synced row shows the gateway-supplied name until you replace it on the edit form. Marketplace-controlled. Sort this column to scan a long catalog quickly. Once a logo is uploaded it renders as a small thumbnail beside the title.
- **Status**: a coloured pill reading **Draft** (grey), **Needs Review** (blue), **Published** (green), or **Archived** (amber). Marketplace-controlled. State changes happen on the edit form, never inline on the list.
- **API Source**: the gateway connection the Product was synced from. In a multi-gateway portal this tells you which gateway team owns the bundle. Filter by it to focus on one gateway at a time.
- **Plan**: the synced Plan name attached to the Product. Read-only; the gateway is the system of record.
- **Quota**: the quota figure the gateway will enforce. Read-only; the gateway is the system of record.
- **Edit action**: opens the edit form, which exposes the read-only gateway fields for review and the editable marketplace fields for authoring.

Filters above the table are **Status**, **API Source**, and **Visibility**. Each filter is recorded as a URL parameter, so a filtered view can be bookmarked or shared. Pagination preserves filter and sort state.

The per-row action menu carries **Edit**, **View on consumer catalog**, and **Open in gateway**. The last entry deep-links to the bundle on the source gateway's admin console and is enabled only for sourced Products; manual-create Products without a source have it disabled. There is deliberately no **Delete** action: removing a Product uses the **Archived** state, not deletion, so subscribers keep their history.

## The edit form: read-only versus editable fields

The edit form mixes fields the gateway owns with fields the marketplace owns. Gateway-owned fields appear in the upper portion of the form and on the right rail under the bundle metadata; they render greyed out, carry a synced-from-gateway indicator, or are surfaced as plain text instead of input controls. Marketplace-owned fields render as normal inputs.

**Marketplace-owned (you author these):**

- **Title**: text (required). The consumer-facing display name. The gateway-supplied name often carries a version suffix or internal codename; replace it with a name a subscriber recognises.
- **Overview**: rich text (optional). The blurb on the Product detail page. Pick **Markdown** from the **Text format** dropdown when you need lists or links.
- **Documentation**: rich text (optional). Longer reference content such as getting-started steps, sample request and response payloads, error codes, and a support contact. Two to four paragraphs is typical.
- **Logo**: image upload (optional). A square PNG or SVG, 256x256 or larger, under 500 KB, transparent background where possible. It anchors the catalog tile.
- **Categories** and **Tags**: taxonomy references (optional). The values that surface the Product to audience filters. Reuse the values from the underlying APIs so the consumer's filter experience stays consistent.
- **Visibility**: radio (required). **Org Level**, **Internal**, or **Public** (see the table below).
- **Status**: select (required). The moderation state, one of **Draft**, **Needs Review**, **Published**, or **Archived**.
- **Publish on / Unpublish on**: datetime (optional). The scheduled go-live and retirement times on the right-rail **Publishing options** panel.

**Gateway-owned (read-only, shown for review):**

- **Plan** name and tier, **Quota** (calls per window), **Rate limit** (per-second or per-minute ceiling), **Burst** and **Overage policy** where present, and the **Bundled APIs** list. All render as plain text.

| Element | Owner | Where to change it |
|---|---|---|
| Product structure (which APIs are bundled) | Gateway | On the gateway, then re-sync |
| Plan name and Plan tier | Gateway | On the gateway, then re-sync |
| Quota (calls per period) and quota period | Gateway | On the gateway, then re-sync |
| Rate limit, burst, overage | Gateway | On the gateway, then re-sync |
| Title, Overview, Documentation | Marketplace | Marketplace edit form |
| Logo, Categories, Tags | Marketplace | Marketplace edit form |
| Visibility, Status, Publish on | Marketplace | Marketplace edit form |

{% hint style="warning" %}
**Caution:** Editing a quota, rate-limit, or Plan-tier value on the marketplace appears to save but does not reach the gateway. The next sync overwrites the local edit with the value the gateway reports.
{% endhint %}

## Sync a Product from a connected gateway

Use this when a new Product has been published on the gateway, or when an existing Product's APIs, Plan name, quota, or rate limit have changed on the gateway and the catalog needs to catch up. A sync is read-only against your gateway: the marketplace pulls metadata; it does not push changes back.

1. From the left sidebar, choose **Manage API Sources**.
2. Confirm the gateway shows status **Connected**. A red status means no Product can sync until the connection is repaired.
3. Locate the row for the gateway connection that owns the Product, then click **Import Products**. The marketplace pulls the current Product list through the same adapter that imports APIs.
4. Wait for the sync banner to confirm completion. New Products appear in **Manage API Products** with their Plan, quota, and rate-limit fields populated from the gateway.
5. Open **Manage API Products** to confirm the Product is visible and its Plan column reflects the gateway state.

{% hint style="success" %}
**Tip:** Schedule a recurring sync if your gateway team ships Product or Plan changes often. The cadence setting lives on the connection edit form on Manage API Sources.
{% endhint %}

## Review a Plan's read-only terms

Use this on every Product you review. The Plan section is the part consumers read first when they decide whether to subscribe, so every figure shown here needs to match what the gateway will enforce on a real call. The Plan card sits on the right rail, below the bundle metadata and above the **Status** panel.

1. Open the Product on the edit form.
2. Read the **Plan name**: a mirror of the gateway Plan title, for example *Free*, *Starter*, *Pro*, or *Enterprise*.
3. Read the **Quota**: two values rendered as one phrase, for example *10,000 calls per day*. The integer is the cap; the period is the rolling window the gateway resets the counter on (*per minute*, *per hour*, *per day*, *per month*).
4. Read the **Rate limit**: the per-second or per-minute ceiling the gateway applies to bursts inside the quota window, for example *50 calls per second*. A rate limit lower than the quota means the gateway smooths traffic across the period.
5. Read the **Burst** field if present: the short-lived overage permitted above the rate-limit ceiling before the gateway returns `429`.
6. Read the **Overage policy** if present: the gateway behaviour at the cap (*reject*, *queue*, or *throttle*).
7. Read the **Bundled APIs** list. One row means a single-API Product, a thin wrapper around one API with a Plan attached. Two or more rows mean a bundle, where one key gives access to every API under the same Plan, quota, and rate limit. Click any title to open the API on Manage APIs and confirm it is itself published.

{% hint style="info" %}
**Note:** A Product carries exactly one Plan in this marketplace. Multi-tier Plans (Free + Starter + Pro on the same bundle) are modelled as one Product per tier on the gateway, each synced as a separate marketplace Product.
{% endhint %}

{% hint style="warning" %}
**Caution:** A consumer report of "429 before reaching the quota" is almost always a rate-limit hit, not a quota hit. Confirm the rate-limit ceiling first, then ask the consumer about their burst pattern.
{% endhint %}

## Enrich the consumer-facing metadata

Use this once a Product has synced and you want consumers to find, read, and decide on it. Confirm the underlying APIs are published first: a Product whose APIs are still in Draft will surface to consumers but reject calls.

1. From **Manage API Products**, click the **Edit** action on the row.
2. Replace the **Title** with the consumer-facing name a subscriber would recognise.
3. Complete the **Overview** with two or three sentences stating what the bundle does, what data it returns, and which tier it targets. Pick **Markdown** when you need lists or links.
4. Complete the **Documentation** with longer reference content: getting-started steps, sample payloads, error codes, support contact.
5. Upload a square **Logo** below the Documentation editor and wait for the upload to finish.
6. Set the **Categories** and **Tags** to match the underlying APIs.
7. Set the **Visibility** radio. If you choose **Org Level**, use the Teams picker to narrow access to named teams.
8. Leave **Status** at **Draft** for now, then click **Save**.

| Visibility option | Audience |
|---|---|
| **Org Level** | Members of this organisation only; the narrowest scope. Optionally narrowed further with the Teams picker. |
| **Internal** | All logged-in users regardless of organisation; useful for partner programmes. |
| **Public** | Everyone, including anonymous visitors; the widest scope and eligible for indexing. |

{% hint style="info" %}
**Note:** Edits to consumer-facing fields survive the next sync. The marketplace overwrites only the gateway-controlled fields and preserves title, overview, documentation, logo, categories, tags, visibility, and status.
{% endhint %}

{% hint style="success" %}
**Tip:** Treat the Overview as a back-cover blurb. Consumers decide whether to subscribe within twenty seconds of opening the tile, so one strong sentence outperforms long copy.
{% endhint %}

## Publish a Product

Publishing flips the marketplace-side state that decides whether the consumer catalog renders the tile. It does not touch the gateway. Confirm every bundled API is published and the gateway-reported Plan figures match what you intend to advertise before you publish.

1. From **Manage API Products**, locate the Product and click **Edit**.
2. On the right rail, find the **Status** panel.
3. Under **Change to**, select **Published**.
4. Optionally enter a short **Revision log message**, for example *Initial release, Pro tier*. The note is stored with the revision.
5. Click **Save**.

To schedule a future go-live instead, expand the **Publishing options** panel, set **Publish on** to the target date and time (in the marketplace timezone), optionally set **Unpublish on** to retire the Product automatically, and click **Save**. The Product appears on the **Scheduled** tab until the timer fires.

{% hint style="warning" %}
**Caution:** Publishing a Product whose underlying APIs are still in Draft surfaces a tile that consumers can see but cannot call. Publish the bundled APIs first, then the Product.
{% endhint %}

{% hint style="info" %}
**Note:** Every Status transition is recorded in the moderation log, reachable from the global **Moderated content** admin page under **Content**. Filter by **Type: API Product** to scope it.
{% endhint %}

## Handle stale and removed Products

A row is flagged *stale* when any gateway-controlled field (bundle name, attached APIs, Plan name, quota, rate limit) has changed on the gateway since the last sync. A row is flagged *removed* when the Product has been deleted on the gateway; the marketplace keeps the row as a tombstone to protect existing subscribers.

1. On **Manage API Products**, look for the *stale* or *removed* badge alongside the **Status** column.
2. From **Manage API Sources**, click **Import Products** on the affected connection and wait for the sync banner.
3. Re-open the row and confirm the Plan, quota, rate limit, bundled APIs, and Title now match the gateway.
4. If the gateway removed the Product, open the edit form, set **Status** to **Archived**, add a Revision log message, and click **Save**. Archived Products are hidden from the consumer catalog but remain visible to existing subscribers as read-only history.

{% hint style="warning" %}
**Caution:** Do not delete a Product to handle a removed gateway-side Product. Deletion erases revision history and breaks subscriber dashboards. **Archived** is the correct end state. Archiving does not revoke subscriber access; the gateway honours existing keys until you revoke them.
{% endhint %}

## Verify

- Confirm the **Status** column on **Manage API Products** reads **Published** for the Product.
- Open an incognito window and confirm the Product tile renders for the configured audience, showing the title, logo, bundled APIs, the synced Plan name, quota, rate limit, and a **Subscribe** action.
- Reload the edit form and confirm your Title, Overview, Logo, Categories, and Tags survived the save while the Plan and quota fields still read as plain text.
- After a re-sync, confirm any *stale* badge has cleared and the gateway-controlled fields match the gateway state.

## Related

- [Publishing APIs](feat-publishing-apis.md): publish the underlying APIs before you publish the Product that bundles them.
- [Subscriptions](feat-subscriptions.md): the first subscription request lands in your queue once a Product is published.
- [Consumer apps & credentials](feat-apps-and-credentials.md): inspect the app and the issued key behind each subscription.
- [Gateway connections](feat-gateway-connections.md): change Plan terms on the gateway and adjust the sync cadence.