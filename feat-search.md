---
icon: magnifying-glass
---

Two surfaces under **Configuration** > **Search and metadata** control how storefront content is found by URL. URL aliases map machine-generated paths such as `/node/688` to readable paths such as `/api/payment-service`, so consumers see a clean address and search engines index a meaningful URL. Redirect rules forward old paths to new ones, so links from emails, search engines, or external blogs keep working when content moves or is renamed. A third tab, Fix 404 pages, records every request that failed so you can turn a broken inbound link into a working redirect in one click.

![Figure. The URL aliases list, with the public alias on the left, the system path on the right, and an Edit action per row.](.gitbook/assets/screenshots/provider/admin-config-search-path.png)

## What you see

The **URL aliases** page is a table of every readable path the marketplace serves, with a free-text filter at the top and an add button in the corner. Each row pairs one public path with one internal path:

- **URL aliases heading**: the page title. Below it sits the alias table and the controls described here.
- **Filter aliases**: a free-text filter that scopes the list to rows whose alias or system path contains your text. Type a fragment such as `payment` to find the alias for the payment API without paging through the full list.
- **Alias column**: the readable public path consumers see in their address bar, such as `/api/payment-service`. This is the field you edit.
- **System path column**: the internal path the alias resolves to, such as `/node/688`. Read-only. Change the alias, not this column.
- **Operations**: a per-row **Edit** action that opens the alias form. **Delete** removes the alias and returns the page to its raw system path.
- **Add URL alias**: a top-right action that opens a blank alias form for a path the auto-generator did not produce.

The **Redirect** page sits behind a tab strip. The first tab, **URL Redirects**, lists every active forward. The second, **Fix 404 pages**, lists requests that returned a 404. Each redirect row carries the old path, the destination, the status code, and an edit action.

![Figure. The Redirect list with From and To paths, the status code, and a sub-tab for fixing 404 pages.](.gitbook/assets/screenshots/provider/admin-config-search-redirect.png)

## Alias form fields

When you edit or add a URL alias, the form exposes these fields:

- **Alias**: text (required). The readable public path consumers see, such as `/api/payment-service`. Start it with a leading slash. Keep it lowercase and hyphen-separated.
- **System path**: read-only. The internal path the alias resolves to, such as `/node/688`. Shown for reference only on the edit form.

## Redirect form fields

When you add or edit a redirect, the form exposes these fields:

- **From**: text (required). The old path the marketplace receives a request for, such as `/old-payments-api`. Enter it without the domain.
- **To**: text (required). The destination the request forwards to. Internal paths such as `/api/payment-service` and external URLs such as `https://docs.example.com` both work.
- **Status code**: select (required). The HTTP status returned to the client. `301 (Moved Permanently)` for renamed or permanently moved content so search engines update their index; `302 (Found)` for a temporary move. Default is `301`.

## Edit a URL alias

Edit an alias when a renamed page should keep a clean public URL, or when an auto-generated alias has drifted from the page title.

1. From the left sidebar, expand **Configuration**, then **Search and metadata**, and click **URL aliases**.
2. Use **Filter aliases** to find the alias by its public path or by the system path it points at.
3. Click **Edit** in the row's **Operations** column.
4. Update the **Alias** field to the new public path, such as `/api/payment-service`.
5. Click **Save**.

{% hint style="success" %}
**Tip:** When you rename a page, change its alias here and add a redirect from the old alias to the new one, covered below. That preserves every inbound link to the old path.
{% endhint %}

## Add a redirect for a moved or renamed URL

Add a redirect when an inbound path needs to land somewhere else: when you renamed an API listing, deleted an old marketing page, or restructured the documentation tree. The marketplace forwards the request to the new path with a 301 status by default.

1. From the left sidebar, expand **Configuration**, then **Search and metadata**, and click **Redirect**.
2. Click **+ Add redirect** in the top-right.
3. Enter the old path in the **From** field, such as `/old-payments-api`.
4. Enter the new path or external URL in the **To** field, such as `/api/payment-service`.
5. Choose a **Status code**: `301 (Moved Permanently)` for renamed content, `302 (Found)` for a temporary move.
6. Click **Save**.

{% hint style="info" %}
**Note:** A long redirect chain (A forwards to B forwards to C) slows the request and confuses search-engine indexers. When you redirect a path, point it directly at the final destination, not at another redirect.
{% endhint %}

## Convert a 404 into a redirect

The marketplace records every request that returned a 404 in the **Fix 404 pages** tab next to **URL Redirects**. Skim this tab weekly to catch broken inbound links from search engines, social posts, or external blogs before they hurt discovery.

1. Open **Configuration** > **Search and metadata** > **Redirect**.
2. Click the **Fix 404 pages** tab.
3. Each row shows the path that returned a 404 and how many times it was hit. Sort by **Count** descending to surface the highest-impact misses.
4. Click **Add redirect** on the row.
5. The **From** field is pre-filled with the failed path. Enter the **To** path and pick a status code.
6. Click **Save**.

{% hint style="success" %}
**Tip:** A spike of 404s on one path often means a page was renamed without a redirect, or an external site is linking to the wrong path. Trace the referrer first to confirm the right destination, then point each redirect straight at the final target so requests resolve in one hop.
{% endhint %}

## Verify

- Open the **From** path in a private browser window and confirm the browser lands on the **To** destination.
- Confirm the response status matches the code you picked, `301` or `302`.
- Confirm there is no chain. The redirect lands on the final destination in one hop, not on another redirect.
- After editing an alias, open the new public path and confirm the page renders at the clean URL.
- Return to the **Fix 404 pages** tab after a converted row has been hit again and confirm the request no longer lands there.

{% hint style="success" %}
**Result:** Consumers reach content at the clean alias, and any request to a retired path forwards to its replacement in a single hop.
{% endhint %}

## Related

- [Content and pages](feat-content-and-pages.md)
- [Custom domains](feat-custom-domains.md)
- [Provider analytics](feat-provider-analytics.md)