---
icon: bullhorn
---

Publishing completes the consumer-facing metadata, sets the visibility scope, and moves the moderation state from Draft to Published so the API appears in the storefront. A gateway-imported API arrives with only an OpenAPI title and version; publishing turns that bare node into a discoverable catalog tile. Use it once the API is content-complete and you are ready for consumers to find it. It pairs with API Products and plans, which add the subscribe and quota controls publishing alone does not.

![Figure. The Overview section of the API edit form where the catalog blurb is written.](.gitbook/assets/screenshots/provider/node-add-apis-section-overview.png)

## What you configure

The edit form groups the consumer-facing metadata into fieldsets, with the publish controls in the right rail.

- **Overview**: rich-text blurb of three to five sentences. Drives the catalog tile snippet and the Overview tab on the detail page.
- **Documentation**: long-form rich text for authentication, base URL, key endpoints, and a runnable example. Renders verbatim on the Documentation tab.
- **Text format**: per editor, Content (plain rich text), Markdown (raw Markdown paste), or Email. Overview and Documentation set their format independently.
- **Logo**: square image upload, PNG, JPG, or SVG up to 5 MB, 256x256 or larger. The tile crops it to a 64x64 thumbnail.
- **Domain**: multi-select business categories (for example Banking, Health) that drive the catalog left-rail grouping.
- **API Tags**: comma-separated discovery keywords that power the catalog filter chips.
- **Visibility**: radio group fixing the audience scope (see Options).
- **Moderation state**: the Change to control that takes the API live, plus an optional Revision log message stored with the revision.

## Options

- **Visibility scope.** **Org Level** keeps the API to your organisation, with an optional Teams picker to narrow to named teams. **Internal** shows the tile to any signed-in account. **Public** renders it to anonymous visitors on the discovery page; reserve it for content-reviewed production APIs.
- **Moderation states.** Draft hides the tile from consumers; Published renders it. Some workflows also expose Needs Review and Archived. Archiving is a permanent retirement that preserves the node and its revision history.
- **Scheduled publish.** Instead of going live on save, queue a future transition. The marketplace cron flips the state to Published when the scheduled moment arrives.

## Configure

1. From the sidebar, open **API MANAGEMENT > Manage APIs**, find the Draft API, and click **Edit**.
2. Complete the **Overview** and the **Documentation** long-form page. Set each **Text format** to Markdown when pasting Markdown.
3. Upload a square **Logo** that stays legible at 64x64.
4. Set the **Domain** categories and comma-separated **API Tags** that consumers will filter on.
5. In the **Visibility** radio group, pick the scope. If you select Org Level, optionally choose one or more **Teams** to narrow further.
6. In the right-rail **Moderation** panel, set **Change to** to **Published**, and type an optional **Revision log message** such as *Initial publish, v1.0*.
7. To launch later, expand **Publishing options**, set the **Publish on** date and time, set **Publish state** to **Published**, and leave Moderation at **Draft**.
8. Click **Save**.

## Verify

- Confirm the **Status** column on Manage APIs reads **Published** for the API.
- Open an incognito window, go to `<your-portal-domain>/api-discovery`, and confirm the tile renders with the title, logo, and Overview snippet for an anonymous visitor.
- Click each Domain and Tag filter chip and confirm the API surfaces under the expected category.
- Click the tile and confirm the detail page renders the Overview, Documentation, and API Specification tabs.
- For a scheduled publish, confirm the row stays Draft until the scheduled time, then flips to Published on the next cron cycle.

{% hint style="info" %}
**Note:** Publishing does not make the API subscribable on its own. Subscriptions and quotas live on API Products; wrap the API in a Product to expose a Subscribe button.
{% endhint %}

{% hint style="success" %}
**Result:** The Status column reads **Published** and the API renders in the storefront for the audience the Visibility scope allows.
{% endhint %}