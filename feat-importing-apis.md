---
icon: file-import
---

Bring APIs into the catalog two ways: an automatic import that reads a connected gateway and creates one node per spec, or the manual Create API wizard for APIs that live outside any gateway. Most teams seed the catalog with an import, then add manual entries as needed.

![Figure. Manage APIs list with imported APIs at the top, sorted by Last updated.](.gitbook/assets/screenshots/provider/admin-manage-apis.png)

## Configure

1. Confirm the connection passed **Test connection** first. A failed connection cannot list APIs.
2. From **Manage API Sources**, find the connection under **Existing API Sources** and click **Import APIs** on its row.
3. Wait for the marketplace to enumerate the gateway. Large gateways take 30 seconds or more.
4. Review the list. Each row shows the API title, version, environment, and a checkbox. Deselect any rows to exclude; the header checkbox toggles all.
5. Pick a default **Visibility** for the batch. **Org Level** is the safe first-import choice. Internal and Public are the other options.
6. Click **Import selected**. The marketplace creates one node per row and redirects to **Manage APIs**.
7. Verify the result: sort by **Last updated** descending, confirm the new APIs sit at the top, and check each carries the connection name in the **API Source** column.

{% hint style="success" %}
**Result:** Every selected API exists as a catalog node, linked to its source gateway and ready for governance, plans, and publication.
**Tip:** Imported APIs land in Draft. Consumers do not see them until you publish them in [Publishing your first API](#publishing-your-first-api).
{% endhint %}

## Create an API by hand

For APIs not behind a gateway, open the **Create APIs** wizard from **Create > APIs** (or **Content > APIs > Add API**). The single-page form has four fieldsets: **Basic Identity** (Title, Version, Revision, Environment, Tags), **Discovery Filters** (Domain, API Resources, Spotlight), **Narrative Content** (Overview, Documentation, Logo), and the **Specification Editor** (upload, paste, or import an OpenAPI 2.0 or 3.0 document, then Validate). Set Visibility and Moderation state in the footer, then **Save**.