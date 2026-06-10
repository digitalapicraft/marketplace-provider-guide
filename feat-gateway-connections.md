---
icon: network-wired
---

A gateway connection registers a supported gateway so the marketplace can read its APIs and Products. It is a one-time setup under **Manage API Sources** that you reuse for every later import. It performs a read-only metadata sync, not a proxy: your APIs stay on the gateway, and the marketplace never sits in the request path.

![Figure. The Manage API Sources page showing the API Gateways tiles.](.gitbook/assets/screenshots/provider/admin-apim-connections.png)

## What you configure

Each gateway opens its own Add Connection form, but the shape is consistent: a label, an admin endpoint, a tenant identifier, and a credential. The fields you fill in are:

- **Connection Name.** A team-visible label that appears on every imported API row. Required, up to 128 characters. Use an environment or region prefix, such as `prod-eu-apigeex`.
- **Description.** Optional free text for organisational context, such as the owning team or credential renewal cycle.
- **Admin / control-plane endpoint.** The gateway's management URL, not the runtime endpoint consumers call. APISIX adds a separate **Runtime URL** so imported routes match the correct data plane.
- **Tenant / workspace identifier.** Scopes the connection to one slice of the gateway: the GCP Project ID for Apigee X, the Organization and Environment for Apigee Edge, or the Service Name, Resource Group, and Subscription ID for Azure.
- **Environment / region.** Where applicable, the region or environment to read from. Apigee X selects an API Hub region; AWS selects an IAM region; Apigee Edge names an environment such as `prod` or `test`.
- **Credential.** The secret the gateway authenticates with, stored encrypted at rest. The form blanks it on edit so you paste a fresh value to rotate. Shapes range from a service-account JSON key to a username and password, an admin API key, an IAM access key pair, or a service-principal client ID and secret.

## Options

Every gateway is a peer source, and you can add any number of connections, including several instances of the same product. Supported gateway types and their credential models:

- **Apigee X.** GCP service-account JSON key plus API Hub region.
- **Apigee Edge.** Username and password against an organisation and environment.
- **Kong.** Admin API key against the Admin API URL, with an optional Enterprise workspace.
- **MuleSoft.** Anypoint username and password, or a connected-app client ID and secret.
- **AWS API Gateway.** IAM access key pair scoped to one region; optional assume-role.
- **Azure APIM.** Service-principal client ID and secret, or a SAS token.
- **IBM API Connect.** Toolkit OAuth credential, IBM API key, provider organisation, and catalog.
- **Tyk.** Dashboard or Gateway API key, selectable by API Mode.
- **APISIX.** Admin API key against the Admin API and Runtime URLs.
- **Aelix.** Email and password of a control-plane user.

## Configure

1. From the left sidebar, expand **API MANAGEMENT**, then click **Manage API Sources**.
2. In the Add New Source panel, under **API Gateways**, click the tile for your gateway product. The gateway-specific Add Connection form opens.
3. Enter a **Connection Name** and an optional **Description**.
4. Enter the admin or control-plane **Endpoint** and the tenant or workspace identifier for your gateway.
5. Enter the credential the gateway requires, plus any environment or region selection.
6. Click **Test connection** and wait for the green confirmation banner. A red banner names the failing step (endpoint, authentication, or region): correct that field and retest.
7. Click **Save**. The connection appears under Existing API Sources with row-level **Import APIs**, **Edit**, and **Delete Connection** actions.

## Verify

- The **Test connection** check returns a green confirmation banner naming the endpoint and tenant the marketplace authenticated against.
- The saved connection appears under **Existing API Sources** with your connection name and an **Import APIs** action.
- Run **Import APIs** on the row: the gateway's APIs become selectable, confirming the metadata sync works end to end.

{% hint style="warning" %}
**Caution:** A connection that fails the test remains saveable but cannot import APIs. Re-test as soon as you can rather than leaving a half-configured row in Existing API Sources.
{% endhint %}

{% hint style="success" %}
**Result:** The gateway is connected and ready to import APIs into the catalog.
{% endhint %}

To change a connection later, click **Edit** on its row, paste any rotated credential into the blanked secret field, re-test, and save. To remove it, click **Delete Connection** and confirm. Deleting unlinks every API imported from that connection, so check Manage APIs for live subscriptions first.