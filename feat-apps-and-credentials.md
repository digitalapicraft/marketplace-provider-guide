---
icon: key
---

Inspect the apps a consumer registered, the credentials issued against each app, and the subscriptions linked to it. Use this surface to vet an app before approving, and to rotate a key when it is suspected leaked.

![Figure. The Manage Apps list showing consumer apps, owners, and subscription counts.](.gitbook/assets/screenshots/provider/admin-portal-manage-apps.png)

## Configure

1. Expand **API MANAGEMENT** in the sidebar, then click **Manage Apps**. The list shows every app with its **Owner**, **Organisation**, and **Subscriptions** count.
2. Click an **App Name** to open its detail page. Four tabs appear: **Overview**, **Credentials**, **Subscriptions**, and **Activity**.
3. On **Overview**, confirm the registering user, the app description, and any redirect URLs match the requested Product and point at the consumer's real domain.
4. Open the **Credentials** tab to read each issued key by type, prefix, issue date, status, and **Last used** timestamp. Providers see only the prefix, never the full key.
5. To rotate, click **Rotate** on the credential row, fill the **Reason** field, then **Confirm**. The marketplace issues a new key, revokes the old one on the gateway, and emails the consumer the new key location.
6. Open the **Subscriptions** tab to see every Product the app is subscribed to, one row per subscription, with its plan, state, approval date, and last-call timestamp.

{% hint style="success" %}
**Result:** You have a full view of the app, its credentials, and its linked subscriptions, and any compromised key is replaced atomically.
**Tip:** Rotation invalidates the old key immediately. Coordinate with the consumer and plan it outside their peak hours.
{% endhint %}

### Key columns

1. **App Name.** The name the consumer gave at registration. Click to open the detail page.
2. **Subscriptions.** Count of active and pending subscriptions tied to this app. Zero usually means the consumer never subscribed to a Product.
3. **Actions.** Per-row menu carrying View, Edit, and Delete.