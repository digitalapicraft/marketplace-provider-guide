---
icon: file-code
---

Documentation sources pull OpenAPI specs from spec registries (SwaggerHub, Postman) and source control (GitHub, Bitbucket, Azure Repos). Use them for APIs that are not yet behind a runtime gateway. Imported specs appear in Manage APIs like gateway-sourced APIs, but with no marketplace proxy in front: requests go to the server URL the spec declares.

![Figure. The Add SwaggerHub Connection form.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-swaggerhub.png)

## Configure

1. From the left sidebar, expand **API MANAGEMENT**, then click **Manage API Sources**.
2. In the top-right corner, click **Manage Documentation Sources**. The visible tile group switches from API Gateways to API Collections / Repositories.
3. Click the source tile you need. SwaggerHub is active today; Postman, GitHub, Bitbucket, and Azure Repos are marked Coming Soon, with forms visible but imports disabled.
4. Enter a **Connection Name** identifiable by your team, and an optional **Description**.
5. Provide the source location and a read-only credential. SwaggerHub takes an **Owner**, an optional **Organization**, and an **API Key**. Repository sources take a **Repository URL**, an access token or app password, and a **Branch**, with OpenAPI or Swagger format checkboxes.
6. Click **Test connection** where exposed, then click **Save**. The source appears under Existing API Sources on the Manage Documentation Sources view.

{% hint style="success" %}
**Result:** A documentation source is registered alongside your gateway connections, ready to import specs in Manage APIs.
**Tip:** Scope the token to read access only, and prefer a workspace- or repository-scoped token over a user-scoped one to limit exposure.
{% endhint %}

Manage Documentation Sources is a view toggle on the same page, not a separate navigation. Click **Manage API Gateways** in the same top-right slot to return to the gateway tiles. To put a managed proxy in front of a spec-only API later, also publish it through a gateway connection.