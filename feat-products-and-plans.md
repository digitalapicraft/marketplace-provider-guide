---
icon: box-archive
---

An API Product bundles one or more APIs into a single subscription unit with one key. A Plan attaches to the Product and carries the quota and rate-limit terms consumers subscribe to. Plan, quota, and rate-limit values are synced read-only from the connected gateway, which authors and enforces them. You author the consumer-facing copy that sits beside them.

![Figure. The Manage API Products list with synced Products and the per-row Edit action.](.gitbook/assets/screenshots/provider/admin-manage-api-products.png)

## Configure

1. Open **Manage API Products** under **API MANAGEMENT** in the left sidebar. Each row shows Title, Status, API Source, Plan, and Quota.
2. Click the **Edit** action on a synced Product. The form opens with gateway metadata pre-filled.
3. Review the read-only Plan fields: Plan name, Quota, Rate limit, and the Bundled APIs list. These are synced from the gateway and cannot be edited here.
4. Replace the **Title** with a consumer-facing name, then complete the **Overview** and **Documentation** fields. Pick **Markdown** from the Text format dropdown when you need lists or links.
5. Upload a square **Logo** (PNG or SVG, 256x256 or larger), then set **Categories** and **Tags** to match the underlying APIs.
6. Set the **Visibility** radio: **Org Level**, **Internal**, or **Public**.
7. On the right-rail **Status** panel, select **Published** under **Change to**, add a revision note, and click **Save**.
8. Open an incognito window and confirm the Product tile renders with its Plan, quota, and a **Subscribe** action.

{% hint style="success" %}
**Result:** The Product is live in the catalog with authored copy beside its gateway-synced Plan terms.
**Tip:** To change a quota or rate limit, edit it on the gateway and re-sync from **Manage API Sources**. Editing those fields in the marketplace appears to save but is overwritten on the next sync.
{% endhint %}