---
icon: box-archive
---

An API Product bundles one or more APIs into a single subscription unit: one key, one quota window, one rate-limit ceiling, one row on the consumer dashboard. A Plan attaches to the Product and carries the operational terms the gateway enforces at runtime. Use this surface after a gateway sync to review the synced Plan terms and author the consumer-facing copy that sits beside them, then publish the Product to the catalog.

![Figure. The Manage API Products list with synced Products and the per-row Edit action.](.gitbook/assets/screenshots/provider/admin-manage-api-products.png)

The Plan, quota window, rate-limit ceiling, and the set of bundled APIs are authored on the gateway and synced into the marketplace **read-only**. The gateway is the system of record and the only system that can enforce these terms on a real call. Editing a quota, rate limit, or Plan tier on the marketplace edit form appears to save but does not reach the gateway, and the next sync overwrites the local change with whatever the gateway reports. To change a Plan term, change it on the gateway and re-sync from **Manage API Sources**.

## What you configure

On the marketplace side you author only the consumer-facing presentation. The runtime terms come from the gateway.

**Marketplace-owned (you author these):**

- **Title**: the consumer-facing display name. A synced Product pre-fills the gateway name, which often carries a version suffix or internal codename; replace it with a name a subscriber recognises.
- **Overview**: the rich-text blurb on the Product detail page. Pick **Markdown** from the Text format dropdown when you need lists or links.
- **Documentation**: longer reference content such as getting-started steps, sample payloads, error codes, and a support contact.
- **Logo**: a square PNG or SVG, 256x256 or larger, under 500 KB, transparent background where possible. It anchors the catalog tile.
- **Categories** and **Tags**: the taxonomy that surfaces the Product to audience filters. Reuse the values from the underlying APIs.
- **Visibility**: **Org Level**, **Internal**, or **Public** (see Options).
- **Status**: the moderation state, one of **Draft**, **Needs Review**, **Published**, or **Archived**.

**Gateway-owned (read-only, shown for review):**

- **Plan** name and tier, **Quota** (calls per window), **Rate limit** (per-second or per-minute ceiling), **Burst** and overage policy where present, and the **Bundled APIs** list. All render as plain text, not input controls.

## Configure

1. Open **Manage API Products** under **API MANAGEMENT** in the left sidebar. Each row shows Title, Status, API Source, Plan, and Quota. Filter by **API Source** to scope to one gateway.
2. Click the **Edit** action on a synced Product. The form opens with gateway metadata pre-filled.
3. Review the read-only Plan card on the right rail: Plan name, Quota, Rate limit, and Bundled APIs. Confirm the figures match what the gateway will enforce before you advertise them.
4. Replace the **Title** with a consumer-facing name, then complete the **Overview** and **Documentation** fields.
5. Upload a square **Logo**, then set **Categories** and **Tags** to match the underlying APIs.
6. Set the **Visibility** radio. If you choose **Org Level**, use the Teams picker to limit access further.
7. On the right-rail **Status** panel, select **Published** under **Change to**, add a revision note, and click **Save**.

## Options

- **Visibility**: **Org Level** (this organisation only, optionally narrowed by team), **Internal** (any logged-in user), or **Public** (everyone, including anonymous visitors).
- **Quota window**: the gateway reports the period as *per minute*, *per hour*, *per day*, or *per month*; the marketplace renders it verbatim.
- **Scheduled publication**: expand the **Publishing options** panel and set **Publish on** (and optionally **Unpublish on**) to queue a future go-live. Scheduled Products appear on the **Scheduled** tab until the timer fires.

## Verify

- Confirm the **Status** column on **Manage API Products** reads **Published** for the Product.
- Open an incognito window and confirm the Product tile renders for the configured audience, showing the title, logo, bundled APIs, the synced Plan name, quota, rate limit, and a **Subscribe** action.
- Reload the edit form and confirm your Title, Overview, Logo, Categories, and Tags survived the save while the Plan and quota fields still read as plain text.

{% hint style="warning" %}
**Caution:** Publishing a Product whose underlying APIs are still in Draft surfaces a tile that consumers can see but cannot call. Publish the bundled APIs first, then the Product.
**Result:** The Product is live in the catalog with authored copy beside its gateway-synced Plan terms, and the first subscription request lands in your queue.
{% endhint %}