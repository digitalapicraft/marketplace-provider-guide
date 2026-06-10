---
icon: network-wired
---

A gateway connection registers a supported gateway so the marketplace can read its APIs and Products. It is a read-only metadata sync, not a proxy: your APIs stay on the gateway. Supported gateways are Apigee Edge, Apigee X, Kong, MuleSoft, AWS API Gateway, Azure, IBM API Connect, Tyk, APISIX, and Aelix.

![Figure. The Add Apigee X Connection form, with credentials masked.](.gitbook/assets/screenshots/provider/admin-apim-connection-add-apigeex.png)

## Configure

1. From the left sidebar, expand **API MANAGEMENT**, then click **Manage API Sources**.
2. In the Add New Source panel, under **API Gateways**, click the tile for your gateway product. The gateway-specific Add Connection form opens.
3. Enter a **Connection Name** identifiable by your team, and an optional **Description**.
4. Enter the gateway's admin or control-plane **Endpoint** (not the runtime endpoint consumers call) and its tenant or workspace identifier.
5. Enter the credential the gateway requires. Field labels differ by product: Apigee X takes a service-account JSON key and API Hub region; Apigee Edge, a username and password; Kong, an admin API key; AWS, an IAM access key pair and region; Azure, a service-principal client ID and secret.
6. Click **Test connection** and wait for the green confirmation banner. A red banner names the failing step (endpoint, authentication, or region): correct that field and retest.
7. Click **Save**. The connection appears under Existing API Sources with row-level **Import APIs**, **Edit**, and **Delete Connection** actions.

{% hint style="success" %}
**Result:** The gateway is connected and ready to import APIs into the catalog.
**Tip:** Run the same gateway in multiple environments or regions? Add a separate connection for each. Per-environment connections give cleaner audit trails and simpler credential rotation.
{% endhint %}

To change a connection later, click **Edit** on its row, paste any rotated credential into the blanked secret field, re-test, and save. To remove it, click **Delete Connection** and confirm. Deleting unlinks every API imported from that connection, so check Manage APIs for live subscriptions first.