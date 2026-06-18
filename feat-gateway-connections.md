---
icon: network-wired
---

A gateway connection registers a supported gateway so the marketplace can read its APIs and Products. You set it up once under **Manage API Sources** and reuse it for every later import. The connection is a read-only metadata sync, not a proxy: your APIs stay on the gateway, and the marketplace never sits in the request path. Add a connection when you are ready to bring an existing gateway's catalog into the marketplace, before you run your first import.

![The Manage API Sources page showing the API Gateways tiles.](.gitbook/assets/screenshots/provider/admin-apim-connections.png)

## What you see

**Manage API Sources** opens at `/admin/apim/connections`. The page has a heading at the top, an **Add New Source** panel below it, and an **Existing API Sources** list further down once you have saved at least one connection.

The Add New Source panel groups its tiles under two headings:

- **API Gateways.** Runtime gateways. The visible tiles are **Aelix**, **Apigee Edge**, **ApigeeX**, **APISIX**, **AWS API Gateway**, **Azure**, **IBM API Connect**, **Kong**, **MuleSoft**, **Tyk**, and **WSO2** (marked *Coming Soon*).
- **API Collections / Repositories.** Spec-only and design-time sources (SwaggerHub, Postman, Azure Repos, GitHub, Bitbucket). These are documented separately in [Documentation and repository sources](feat-documentation-sources.md).

Each gateway tile carries a **+ Add Connection** action. Clicking the action (or the tile body) opens that gateway's Add Connection form at `/admin/apim/connection/add/<gateway>`. A top-right **Manage Documentation Sources** button toggles the view to the spec/repository tiles; you do not need it for gateway connections.

Once saved, each connection becomes a row under Existing API Sources with row-level **Import APIs**, **Edit**, and **Delete Connection** actions. The connection name you choose appears on every imported API row in the **API Source** column on Manage APIs.

Every gateway is treated as a peer source. You can add any number of connections, including several instances of the same product. A separate connection per Apigee X organisation, or per AWS region, is a common pattern.

## Connection form fields

Each gateway opens its own form, but the shape is consistent: a label, an optional description, an admin or control-plane endpoint, a tenant or workspace identifier, a credential, and (where relevant) an environment or region. The fields common to most forms:

- **Connection Name**: text (required, max 128). A team-visible label that appears on every imported API row. Use an environment or region prefix such as `prod-eu-apigeex`.
- **Description**: text (optional). Free text for organisational context, such as the owning team or the credential renewal cycle.
- **Admin / control-plane endpoint**: text (required, max 128). The gateway's management URL, not the runtime endpoint consumers call.
- **Tenant / workspace identifier**: varies (required where shown). Scopes the connection to one slice of the gateway.
- **Environment / region**: dropdown or text (varies). Where the connection reads from.
- **Credential**: varies (required). The secret the gateway authenticates with, stored encrypted at rest. Edit forms blank the secret so you paste a fresh value to rotate.

The remaining fields differ per product. The task subsections below give the full field set for each supported gateway.

## Add an Apigee X connection

Use this when your APIs run on Apigee X, the Google Cloud Apigee product. Before you start, obtain your GCP project ID, identify the API Hub region you provisioned, and create a service account with the Apigee Reader role (do not reuse a personal credential or an Owner-level key).

1. From the left sidebar, expand **API MANAGEMENT**, then click **Manage API Sources**.
2. In the Add New Source panel, under API Gateways, click the **Apigee X** tile. The form opens at `/admin/apim/connection/add/apigeex`.
3. Complete the fields:
   - **Connection Name**: text (required, max 128). Label identifiable by your team.
   - **Description**: text (optional). Scope of this connection.
   - **ApigeeX Endpoint**: text (required, max 128). The admin endpoint, typically starting `https://apigee.googleapis.com/`. Not the runtime endpoint.
   - **Organization / GCP Project ID**: text (required, max 128). The GCP project hosting Apigee X. Case-sensitive: match the Google Cloud console exactly.
   - **API Hub Location**: dropdown (required). The region where API Hub is deployed; options span North America, Europe, Asia, and Australia.
   - **GCP Service Account Key**: text (required). Paste the full JSON key contents including the outer braces. Stored encrypted.
4. Click **Test connection** and wait for the green confirmation banner.
5. Click **Save**. The connection appears under Existing API Sources.

![The Add Apigee X Connection form, with credentials masked.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-apigeex.png)

{% hint style="info" %}
**Note:** Saving the connection does not import any APIs. They remain in Apigee X until you run [Importing APIs](feat-importing-apis.md).
**Caution:** The service account key grants read access to every API in the project. Never paste a key carrying Owner or Editor roles.
**Tip:** Store the JSON key in a secrets store such as Google Secret Manager or HashiCorp Vault, and rotate both stores in the same change window.
{% endhint %}

## Add an Apigee Edge connection

Use this when your APIs run on Apigee Edge, the on-premises and cloud Apigee product distinct from Apigee X. Apigee Edge authenticates with a username and password against your organisation.

1. From the left sidebar, select **Manage API Sources**, then click the **Apigee Edge** tile.
2. Complete the fields:
   - **Connection Name**: text (required, max 128).
   - **Description**: text (optional).
   - **Endpoint URL**: text (required, max 128). The management API endpoint, typically `https://api.enterprise.apigee.com/`.
   - **Organization**: text (required, max 128). Your Apigee Edge organisation name.
   - **Environment**: text (required, max 128). The environment to read from, for example `prod` or `test`.
   - **Username**: text (required, max 128). A user with read access to the organisation.
   - **Password**: text (required, max 128). The password for that user.
3. Click **Test connection**, then **Save**.

![The Add Apigee Edge Connection form.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-apigee.png)

{% hint style="info" %}
**Note:** Add a separate connection per environment if you manage `dev`, `test`, and `prod` from the same organisation.
**Caution:** Personal credentials disappear when the owning user leaves. Always use a dedicated service account with the read-only Org User role.
{% endhint %}

## Add a Kong connection

Use this when your APIs run on Kong Gateway (Kong Enterprise or open-source Kong). Kong authenticates with an admin API key against the Admin API URL.

1. From the left sidebar, select **Manage API Sources**, then click the **Kong** tile.
2. Complete the fields:
   - **Connection Name**: text (required, max 128).
   - **Description**: text (optional).
   - **Admin API URL**: text (required, max 128). The Kong Admin API URL, not the proxy URL, typically `https://kong-admin.example.com:8001`.
   - **Workspace**: text (optional, max 128). A Kong Enterprise workspace name. Leave blank for open-source or the default workspace.
   - **Admin API Key**: text (required, max 128). A Kong RBAC token or admin key with read access to services and routes.
3. Click **Test connection**, then **Save**.

![The Add Kong Connection form.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-kong.png)

{% hint style="success" %}
**Tip:** When Kong sits behind a private load balancer, expose the Admin API on a separate hostname from the proxy and point the marketplace at the Admin API hostname.
{% endhint %}

## Add a MuleSoft connection

Use this when your APIs run on MuleSoft Anypoint Platform. MuleSoft accepts either a username and password or a connected-app client ID and secret.

1. From the left sidebar, select **Manage API Sources**, then click the **MuleSoft** tile.
2. Complete the fields:
   - **Connection Name**: text (required, max 128).
   - **Description**: text (optional).
   - **Anypoint Platform Base URL**: text (required, max 128). Typically `https://anypoint.mulesoft.com`.
   - **Business Group (Organization) ID**: text (required, max 128). The business group your APIs are registered under.
   - **Environment ID**: text (required, max 128). The environment to read from.
   - **Authentication**: choose **Username & Password** (enter **Username** and **Password**) or **Client ID & Secret** (enter **Client ID** and **Client Secret**).
3. Click **Test connection**, then **Save**.

![The Add MuleSoft Connection form.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-mulesoft.png)

{% hint style="success" %}
**Tip:** Prefer **Client ID & Secret** for service integrations: a connected app is auditable and revocable independently of any one user.
**Note:** Business groups and environments are independent identifiers. Pick the IDs in pairs to match the catalog slice you want.
{% endhint %}

## Add an AWS API Gateway connection

Use this when your APIs run on AWS API Gateway. AWS authenticates with an IAM access key pair scoped to a single region. Add one connection per region.

1. From the left sidebar, select **Manage API Sources**, then click the **AWS API Gateway** tile.
2. Complete the fields:
   - **Connection Name**: text (required, max 128). Include the region, for example "Production us-east-1".
   - **Description**: text (optional).
   - **AWS Region**: dropdown (required). Lists every commercial, GovCloud, and China region.
   - **Access Key ID**: text (required, max 128). For a user or role with read access to API Gateway.
   - **Secret Access Key**: text (required, max 128). The matching secret.
   - **Use Assume Role**: checkbox (optional). Have the marketplace assume an IAM role rather than using the access key directly.
   - **Enable AWS Cognito Authentication**: checkbox (optional). Honour Cognito authorisers when proxying calls to APIs behind a Cognito user pool.
3. Click **Test connection**, then **Save**.

![The Add AWS API Gateway Connection form.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-aws.png)

{% hint style="info" %}
**Note:** AWS API Gateway is regional. A connection bound to `us-east-1` does not see APIs in `eu-west-1`.
**Caution:** Use a dedicated IAM user or role with a read-only API Gateway policy. Never paste root-account or administrator credentials.
**Tip:** Combine **Use Assume Role** with an SSO-issued temporary credential when long-lived access keys are disallowed.
{% endhint %}

## Add an Azure API Management connection

Use this when your APIs run on Azure API Management. Azure authenticates with a service-principal client ID and secret, with SAS-token authentication available for environments that disallow OAuth.

1. From the left sidebar, select **Manage API Sources**, then click the **Azure** tile.
2. Complete the fields:
   - **Connection Name**: text (required, max 128).
   - **Description**: text (optional).
   - **Service Name**: text (required, max 128). The API Management instance name as it appears in the Azure portal.
   - **Resource Group**: text (required, max 128). The resource group containing the instance.
   - **Subscription ID**: text (required, max 128). The Azure subscription GUID.
   - **Management API URL**: text (required, max 128). Typically `https://management.azure.com`.
   - **Authentication Type**: choose **OAuth** (default) or **SAS Token**.
   - **Tenant ID**: text (required, max 128). The Azure AD tenant GUID for OAuth.
   - **Client ID**: text (required, max 128). The service-principal client ID.
   - **Client Secret**: text (required, max 128). The service-principal client secret.
3. Click **Test connection**, then **Save**.

![The Add Azure API Management Connection form.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-azure.png)

{% hint style="success" %}
**Tip:** Create a service principal scoped to the resource group with the *API Management Service Reader* role. Avoid subscription-wide Contributor rights.
**Caution:** Client secrets expire. Set a reminder for the expiry date and rotate before the next scheduled import, or no APIs flow until you re-credential.
{% endhint %}

## Add an IBM API Connect connection

Use this when your APIs run on IBM API Connect. It requires a toolkit OAuth credential, an IBM API key, and a provider organisation. The API key alone is insufficient.

1. From the left sidebar, select **Manage API Sources**, then click the **IBM API Connect** tile.
2. Complete the fields:
   - **Connection Name**: text (required, max 128).
   - **Description**: text (optional).
   - **Toolkit Endpoint**: text (required, max 128). The toolkit (platform) endpoint URL.
   - **Toolkit Client ID**: text (required, max 128). OAuth client ID for toolkit access.
   - **Toolkit Client Secret**: text (required, max 128). The matching OAuth secret.
   - **IBM API Key**: text (required, max 128). Your IBM Cloud API key.
   - **Provider Organisation Name**: text (required, max 128). The provider organisation that owns your APIs.
   - **Catalog Name**: text (required, max 128). The catalog within the provider organisation.
   - **Import Level**: choose **Catalog** (every API in the catalog) or **Space** (a single space within the catalog).
   - **Space Name**: text (required only when Import Level is Space, max 128).
   - **User Registry Name**: text (required, max 128). The registry the toolkit credentials belong to.
3. Click **Test connection**, then **Save**.

![The Add IBM API Connect Connection form.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-ibm-apic.png)

{% hint style="info" %}
**Note:** The provider organisation is distinct from the catalog. One deployment can host multiple provider organisations, each with multiple catalogs.
**Tip:** Use **Import Level: Space** when API ownership is assigned to spaces inside a shared catalog, for a clean per-team audit trail.
{% endhint %}

## Add a Tyk connection

Use this when your APIs run on Tyk. Tyk supports two modes: Tyk Pro/Cloud (Dashboard API) and Tyk open-source (Gateway API). The marketplace defaults to Dashboard mode.

1. From the left sidebar, select **Manage API Sources**, then click the **Tyk** tile.
2. Complete the fields:
   - **Connection Name**: text (required, max 128).
   - **Description**: text (optional).
   - **API Mode**: choose **Tyk Dashboard API (Pro/Cloud)** (default) or **Tyk Gateway API (Open Source)**.
   - **Dashboard URL**: text (required, max 128). The Tyk Dashboard URL, or the Gateway API URL in Gateway mode.
   - **Dashboard API Key**: text (required, max 128). A dashboard or gateway API key with read access.
3. Click **Test connection**, then **Save**.

![The Add Tyk Connection form.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-tyk.png)

{% hint style="info" %}
**Note:** Tyk Cloud users find the Dashboard API key under **Users > Edit Profile > Tyk Dashboard API Access Credentials**.
**Tip:** Use Gateway mode for installs with no Dashboard, such as a Kubernetes-only deployment where Operator manages routes via CRDs.
{% endhint %}

## Add an APISIX connection

Use this when your APIs run on Apache APISIX. APISIX requires both the Admin API base URL (control plane) and the Runtime URL (data plane), so imported routes match the correct runtime.

1. From the left sidebar, select **Manage API Sources**, then click the **APISIX** tile.
2. Complete the fields:
   - **Connection Name**: text (required, max 128).
   - **Description**: text (optional).
   - **Admin API Base URL**: text (required, max 128). The Admin API URL, typically on port `9180`.
   - **Runtime URL**: text (required, max 128). The data-plane URL where consumer traffic terminates.
   - **Admin API Key**: text (required, max 128). The admin key configured in `config.yaml`.
3. Click **Test connection**, then **Save**.

![The Add APISIX Connection form.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-apisix.png)

{% hint style="success" %}
**Tip:** When the Admin API and Runtime are on different hosts, use the host the marketplace can reach for **Admin API Base URL** and the consumer-facing host for **Runtime URL**.
**Caution:** The default APISIX admin key in `config.yaml` is well-known. Rotate it to a strong value before exposing the Admin API to any external caller.
{% endhint %}

## Add an Aelix connection

Use this when your APIs run on Aelix. Aelix authenticates with the email and password of an Aelix control-plane user.

1. From the left sidebar, select **Manage API Sources**, then click the **Aelix** tile.
2. Complete the fields:
   - **Connection Name**: text (required, max 128).
   - **Description**: text (optional).
   - **Control Plane Endpoint URL**: text (required, max 128). The Aelix control-plane URL.
   - **Email**: text (required, max 254). An Aelix user with read access.
   - **Password**: text (required, max 128). The password for that user.
3. Click **Test connection**, then **Save**.

![The Add Aelix Connection form.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-aelix.png)

{% hint style="success" %}
**Tip:** Create a dedicated Aelix service user for the marketplace and grant read-only scopes only, so a stolen credential cannot mutate routes.
{% endhint %}

## Test a connection

Run a test whenever you create a connection or change its credentials. A successful test means the marketplace can reach the gateway's admin endpoint and authenticate.

1. On the Add Connection form (or its Edit equivalent), complete every required field.
2. Click **Test connection**, next to Save at the bottom of the form.
3. Wait for the result banner. Green means the marketplace authenticated. A red banner names the failing step, for example "endpoint unreachable", "authentication rejected", or "API Hub region returned empty".
4. If it fails, correct the field the error names. Common causes are an admin endpoint pointed at the runtime URL, a typo in the project ID, a service account lacking the Apigee Reader role, or a region mismatch.
5. Re-test until green, then click **Save**.

{% hint style="warning" %}
**Caution:** A connection that fails the test remains saveable but cannot import APIs. Imports against it fail from the gateway side. Re-test as soon as you can rather than leaving a half-configured row.
{% endhint %}

## Edit or revoke a connection

Use this to rotate credentials, point a connection at a new endpoint, or remove a gateway. Decide first whether to edit or delete: editing preserves the connection identity, the imported APIs, and any subscriptions; deleting removes them. Most credential rotations and endpoint moves should be edits. Before deleting, open Manage APIs, filter by the gateway, and check for live consumer subscriptions.

To edit:

1. From the left sidebar, select **Manage API Sources**.
2. Under Existing API Sources, find the connection's row and click **Edit**. The form opens with current values pre-filled, except secret fields, which are blanked for safety.
3. Update the fields you need. To rotate a credential, paste the new value into the blanked secret field.
4. Click **Test connection**, then **Save**.

To revoke:

1. Under Existing API Sources, find the connection's row and click **Delete Connection**.
2. Confirm in the dialog. The marketplace removes the connection record and unlinks every API imported from it.

![The Edit Connection form, pre-populated with current values (secret fields blanked).](.gitbook/assets/screenshots/provider/admin-apim-connection-16-edit-apigeex.png)

{% hint style="warning" %}
**Caution:** Deleting a connection that has imported APIs unlinks those APIs from their source. Any consumer apps subscribed to them lose access on the next request. Migrate consumers first, or warn them with the deprecation workflow.
**Note:** Editing the Connection Name is safe; it is only a label. Editing the admin endpoint or tenant identifier on a connection that already has imported APIs may strand those APIs. Re-import after a major endpoint change.
**Tip:** Treat credential rotation as a recurring calendar task. A 90-day cadence aligns with most enterprise security policies.
{% endhint %}

## Verify

- **Test connection** returns a green confirmation banner naming the endpoint and tenant the marketplace authenticated against.
- The saved connection appears under **Existing API Sources** with your connection name and an **Import APIs** action.
- After an edit, the row reflects your updated values and re-tests green.
- After a delete, the row is absent, and APIs sourced from it show an empty value in the **API Source** column on Manage APIs.

{% hint style="success" %}
**Result:** The gateway is connected and ready to import APIs into the catalog.
{% endhint %}

## Related

- [Documentation and repository sources](feat-documentation-sources.md): register spec-only and repository sources that are not behind a runtime gateway.
- [Importing APIs](feat-importing-apis.md): pull a connected gateway's APIs into the catalog, or create one by hand.
- [API governance](feat-api-governance.md): read the linting score each imported API runs through before you publish.
- [Publishing APIs](feat-publishing-apis.md): transition an API from Draft to Published once governance is acceptable.