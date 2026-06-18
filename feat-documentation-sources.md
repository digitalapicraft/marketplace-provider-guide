---
icon: file-code
---

Documentation sources pull OpenAPI specs from spec registries and source-control repositories. Use them for an API whose spec lives in a registry or repo but that is not yet behind a runtime gateway. The flow mirrors gateway connections: select the source, enter a read-only credential, and save. Imported specs land in Manage APIs like gateway-sourced APIs, with one difference: there is no marketplace proxy in front, so requests go to whatever server URL the spec declares.

![The Add SwaggerHub Connection form.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-swaggerhub.png)

## What you see

Documentation sources live on the same page as gateway connections, at `/admin/apim/connections`. They are a view toggle, not a separate navigation:

1. Open **Manage API Sources** from the **API MANAGEMENT** sidebar group. The page opens with the **API Gateways** tile group active.
2. Click **Manage Documentation Sources** in the top-right corner. The URL stays at `/admin/apim/connections` but the visible tile group switches to **API Collections / Repositories**: the spec-only and design-time sources.
3. The inverse button, **Manage API Gateways**, appears in the same top-right slot to switch back.

The tiles in the documentation-sources group are **SwaggerHub** (active today), **Postman**, **Azure Repos**, **GitHub**, and **Bitbucket** (all marked *Coming Soon* today). The active tile's action reads **Configure & Import**; Coming Soon tiles show their form but their import action is disabled.

Saved sources appear under **Existing API Sources** on the Manage Documentation Sources view, each with **Edit** and (where exposed) **Test connection** actions. The connection name appears on imported API rows in the **API Source** column on Manage APIs.

Two families of source are supported. Registries hold finished specs; source control holds specs alongside code:

- **Spec registries.** SwaggerHub (active) and Postman (Coming Soon).
- **Source control.** GitHub, Bitbucket, and Azure Repos (all Coming Soon).

## Source form fields

The forms share the gateway shape (a label, a location, and a credential) with a few source-specific fields. Common fields across the sources:

- **Connection Name**: text (required, max 128). A team-visible label that appears on imported API rows.
- **Description**: text (optional). Free text describing the scope of the connection.
- **Source location**: varies. SwaggerHub takes an **Owner** and optional **Organization**; repository sources take a **Repository URL**.
- **Credential / token**: varies (required). A read-scope secret, stored encrypted. SwaggerHub takes an **API Key**; Postman a **Postman API Key**; GitHub a personal access token; Bitbucket a username and app password; Azure Repos a PAT with Code (Read) scope.
- **Branch**: dropdown (repository sources). The branch to read specs from; the dropdown populates after a valid URL and token are entered.
- **Format checkboxes**: **OpenAPI**, plus **Swagger** on Azure Repos. Tell the marketplace which spec versions to discover and parse.

The per-source field sets follow.

## Add a SwaggerHub connection

Use this when an API spec is hosted in SwaggerHub. SwaggerHub authenticates with an API key issued to your SwaggerHub user, and lists the APIs owned by a single user and optionally an organisation that user belongs to. Before you start, generate a read-access API key from your SwaggerHub profile under API Keys, and identify the owner (typically your SwaggerHub login).

1. From the left sidebar, select **Manage API Sources**.
2. Click **Manage Documentation Sources** to switch the visible tile group.
3. Click the **SwaggerHub** tile. The form opens at `/admin/apim/connection/add/swaggerhub`.
4. Complete the fields:
   - **Connection Name**: text (required, max 128).
   - **Description**: text (optional).
   - **API Key**: text (required, max 128). Your SwaggerHub API key with read scope.
   - **Organization**: text (optional, max 128). The SwaggerHub organisation name, for example `my-org`. Leave blank for personal accounts.
   - **Owner**: text (required, max 128). The SwaggerHub user whose APIs to import, for example `my-user`.
5. Click **Test connection** to confirm the marketplace can list APIs, then **Save**. The source appears under Existing API Sources on the Manage Documentation Sources view.

![The Add SwaggerHub Connection form.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-swaggerhub.png)

{% hint style="info" %}
**Note:** SwaggerHub APIs surface as spec-only entries. Consumers can read the document and try requests, but the request hits whichever server URL the spec declares, with no marketplace proxy in front.
**Tip:** If one SwaggerHub user owns APIs across several organisations, add a separate connection per organisation. Each connection scopes its imports to the Organization plus Owner combination.
{% endhint %}

## Add a Postman connection

Use this when an API spec is held in a Postman workspace. Postman authenticates with a Postman API key.

1. From the left sidebar, select **Manage API Sources**.
2. Click **Manage Documentation Sources**, then the **Postman** tile.
3. Complete the fields:
   - **Connection Name**: text (required, max 128).
   - **Description**: text (optional).
   - **Postman API Key**: text (required, max 128). A Postman API key with read access to the workspace.
4. Click **Save**.

![The Add Postman Connection form.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-postman.png)

{% hint style="info" %}
**Note:** The Postman tile is marked **Coming Soon**. The form is visible but imports are disabled pending a future release. The fields are documented so your onboarding plan accounts for them.
**Tip:** Pre-fill the connection now. The saved values become active when the tile leaves Coming Soon, so no rework is required.
{% endhint %}

## Add a GitHub connection

Use this when an OpenAPI spec is held in a GitHub repository. GitHub authenticates with a personal access token (or fine-grained token) that has read access to repository contents.

1. From the left sidebar, select **Manage API Sources**.
2. Click **Manage Documentation Sources**, then the **GitHub** tile.
3. Complete the fields:
   - **Connection Name**: text (required, max 128). For example "My GitHub Connection".
   - **Repository URL**: text (required, max 128). The full repository URL, for example `https://github.com/my-org/my-api-repo`.
   - **GitHub Access Token**: text (required, max 128). A token with read access to repository contents.
   - **Branch**: dropdown. Populates after a valid URL and token are entered; selects the branch to read specs from.
   - **OpenAPI**: checkbox. Enables OpenAPI document discovery in the repository.
4. Click **Save**.

![The Add GitHub Connection form.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-github.png)

{% hint style="info" %}
**Note:** The GitHub tile is marked **Coming Soon**. The form is visible but imports are disabled pending a future release.
**Tip:** Use a fine-grained token scoped to the one repository the marketplace needs. Classic PATs grant broad access and are harder to audit.
{% endhint %}

## Add a Bitbucket connection

Use this when an OpenAPI spec is held in a Bitbucket repository. Bitbucket authenticates with a username and an app password scoped to repository read access.

1. From the left sidebar, select **Manage API Sources**.
2. Click **Manage Documentation Sources**, then the **Bitbucket** tile.
3. Complete the fields:
   - **Connection Name**: text (required, max 128). For example "My Bitbucket Connection".
   - **Repository URL**: text (required, max 128). The full repository URL, for example `https://bitbucket.org/my-workspace/my-api-repo`.
   - **Username**: text (required, max 128). Your Bitbucket username.
   - **App Password**: text (required, max 128). An app password with repository read scope.
   - **Branch**: dropdown. Populates after a valid URL and credentials are entered.
   - **OpenAPI**: checkbox. Enables OpenAPI document discovery.
4. Click **Save**.

![The Add Bitbucket Connection form.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-bitbucket.png)

{% hint style="info" %}
**Note:** The Bitbucket tile is marked **Coming Soon**. The form is visible but imports are disabled pending a future release.
**Tip:** Create the app password under a dedicated service user account so the integration does not break when a team member rotates roles.
{% endhint %}

## Add an Azure Repos connection

Use this when an OpenAPI or Swagger spec is held in an Azure Repos repository. Azure Repos authenticates with a personal access token scoped to Code (Read).

1. From the left sidebar, select **Manage API Sources**.
2. Click **Manage Documentation Sources**, then the **Azure Repos** tile.
3. Complete the fields:
   - **Connection Name**: text (required, max 128).
   - **Description**: text (optional).
   - **Repository URL**: text (required, max 128). The Azure Repos repository URL.
   - **Access Token**: text (required, max 128). An Azure DevOps PAT with Code (Read) scope.
   - **OpenAPI**: checkbox. Read OpenAPI 3.x documents.
   - **Swagger**: checkbox. Read Swagger 2.0 documents.
   - **Branch**: dropdown. Click **Fetch Branches** to load the list, then select the branch.
4. Click **Save**.

![The Add Azure Repos Connection form.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-azurerepo.png)

{% hint style="info" %}
**Note:** The Azure Repos tile is marked **Coming Soon**. The form is visible but imports are disabled pending a future release.
**Tip:** Tick both **OpenAPI** and **Swagger** when the repository holds a mix of versions. The marketplace inspects each file at import time and routes 3.x and 2.0 documents through the appropriate parser.
{% endhint %}

## Verify

- The visible tile group has switched to **API Collections / Repositories** after you click Manage Documentation Sources.
- The saved source appears under **Existing API Sources** on the Manage Documentation Sources view.
- The **Test connection** check (where exposed) returns a green confirmation banner.
- Opening the connection in **Edit** mode shows the credential field blanked while the source location fields retain their values.

{% hint style="success" %}
**Tip:** Scope the token to read access only, and prefer a workspace- or repository-scoped token over a user-scoped one to limit exposure.
**Result:** A documentation source is registered alongside your gateway connections, ready to import specs in Manage APIs.
{% endhint %}

## Related

- [Gateway connections](feat-gateway-connections.md): register a runtime gateway and put a managed proxy in front of an API.
- [Importing APIs](feat-importing-apis.md): pull a registered source's specs into the catalog as draft nodes.
- [API governance](feat-api-governance.md): read the linting score each imported spec runs through before you publish.
- [Publishing APIs](feat-publishing-apis.md): make a spec-only API live in the consumer catalog.