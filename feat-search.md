---
icon: magnifying-glass
---

URL aliases and redirects control how storefront content is found by URL. Aliases map machine paths like `/node/688` to readable paths like `/api/payment-service`. Redirects forward old paths to new ones so inbound links keep working when content moves or is renamed.

![Figure. The URL aliases list, with the public alias, the system path, and an Edit action per row.](.gitbook/assets/screenshots/provider/admin-config-search-path.png)

## Configure

1. From the left sidebar, expand **Configuration**, then **Search and metadata**, and click **URL aliases**.
2. Use **Filter aliases** to find an alias by its public path or by the system path it points at.
3. Click **Edit** in the row's **Operations** column, update the **Alias** field to the new public path, and click **Save**.
4. To forward an old path, return to **Search and metadata** and click **Redirect**.
5. Click **+ Add redirect**, then enter the old path in **From** and the new path or external URL in **To**.
6. Choose a **Status code**: `301 (Moved Permanently)` for renamed content, `302 (Found)` for temporary moves.
7. Click **Save**.

{% hint style="success" %}
**Result:** Consumers reach content at the clean alias, and any request to a retired path forwards to its replacement.
**Tip:** When you rename a page, update its alias AND add a redirect from the old path so every inbound link survives.
{% endhint %}

## Fix 404 pages

The **Fix 404 pages** tab next to **URL Redirects** lists every path that returned a 404, with a hit count. Sort by count to surface the highest-impact misses, click **Add redirect** on a row to pre-fill the **From** field, set the **To** path and status code, and save. Point each redirect straight at the final destination so requests resolve in one hop.