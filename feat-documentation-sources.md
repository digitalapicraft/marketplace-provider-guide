---
icon: file-code
---

Documentation sources pull OpenAPI specs from spec registries and source-control repositories. Use them for APIs whose spec lives in a registry or repo but that are not yet behind a runtime gateway. The flow mirrors gateway connections: select the source, enter a read-only credential, and save. Imported specs appear in Manage APIs like gateway-sourced APIs, but with no marketplace proxy in front: requests go to the server URL the spec declares.

![Figure. The Add SwaggerHub Connection form.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-swaggerhub.png)

## What you configure

The source forms share the gateway shape (a label, a location, and a credential) with a few source-specific fields:

- **Connection Name.** A team-visible label that appears on imported API rows. Required, up to 128 characters.
- **Description.** Optional free text describing the scope of the connection.
- **Source location.** What the connection reads from. SwaggerHub takes an **Owner** (the SwaggerHub user) and an optional **Organization**. Repository sources take a **Repository URL** pointing at the repo that holds the spec.
- **Credential / token.** A read-scope secret, stored encrypted. SwaggerHub takes an **API Key**; Postman takes a **Postman API Key**; GitHub takes a personal access token; Bitbucket takes a username and app password; Azure Repos takes a PAT with Code (Read) scope.
- **Branch or workspace.** For repository sources, the **Branch** to read specs from. The dropdown populates after a valid URL and token are entered (Azure Repos uses a **Fetch Branches** action). SwaggerHub scopes by Owner and Organization instead of a branch.
- **Sync options.** Repository sources expose format checkboxes (**OpenAPI**, and **Swagger** on Azure Repos) so the marketplace knows which spec versions to discover and parse.

## Options

Two families of source are supported. Registries hold finished specs; source control holds specs alongside code.

- **Spec registries.** **SwaggerHub** (active today) and **Postman** (Coming Soon).
- **Source control.** **GitHub**, **Bitbucket**, and **Azure Repos** (all Coming Soon).

Tiles marked Coming Soon show their form so you can plan onboarding, but imports stay disabled until the feature ships. Pre-filling those forms now saves rework later, since the saved values activate when the tile leaves Coming Soon.

## Configure

1. From the left sidebar, expand **API MANAGEMENT**, then click **Manage API Sources**.
2. In the top-right corner, click **Manage Documentation Sources**. The visible tile group switches from API Gateways to API Collections / Repositories.
3. Click the source tile you need. SwaggerHub is the active worked example.
4. Enter a **Connection Name** and an optional **Description**.
5. Provide the source location and a read-only credential: an Owner and API Key for SwaggerHub, or a Repository URL, token, and Branch for a repository source. Tick the format checkboxes that match your specs.
6. Click **Test connection** where exposed, then click **Save**. The source appears under Existing API Sources on the Manage Documentation Sources view.

## Verify

- The visible tile group has switched to **API Collections / Repositories** after you click Manage Documentation Sources.
- The saved source appears under **Existing API Sources** on the Manage Documentation Sources view.
- The **Test connection** check (where exposed) returns a green confirmation banner.
- Open the connection in **Edit** mode and confirm the credential field is blanked while the source location fields retain their values.

{% hint style="success" %}
**Tip:** Scope the token to read access only, and prefer a workspace- or repository-scoped token over a user-scoped one to limit exposure.
{% endhint %}

{% hint style="success" %}
**Result:** A documentation source is registered alongside your gateway connections, ready to import specs in Manage APIs.
{% endhint %}

Manage Documentation Sources is a view toggle on the same page, not a separate navigation. Click **Manage API Gateways** in the same top-right slot to return to the gateway tiles. To put a managed proxy in front of a spec-only API later, also publish it through a gateway connection.