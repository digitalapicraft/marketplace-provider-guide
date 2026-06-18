---
icon: key
---

Inspect the apps a consumer registered, the credentials issued against each app, and the subscriptions linked to it. Use this surface to vet an app before you approve it on the [Subscriptions](concept-publishing.md#subscriptions) queue, and to rotate a key when it is suspected leaked. Manage Apps shows every app the marketplace knows about, regardless of subscription state.

![Figure. The Manage Apps list showing consumer apps, owners, and subscription counts.](.gitbook/assets/screenshots/provider/admin-portal-manage-apps.png)

## What you inspect

The Manage Apps list gives you the roster; the app detail page gives you the depth across four tabs (**Overview**, **Credentials**, **Subscriptions**, **Activity**). These are the fields you read before granting or rotating access:

- **App Name**: the name the consumer gave at registration. Click to open the detail page.
- **Owner / Organisation**: the registering user and their tenant. Cross-check the user's email domain against the organisation domain before approving high-value access.
- **Subscriptions**: count of active and pending subscriptions tied to the app. Zero usually means the consumer registered an app but never subscribed.
- **Redirect URLs** (Overview): should point at the consumer's real domain; localhost is fine in development, not for production traffic.
- **Credential type / prefix**: typically an API key shown by its leading characters only. Providers never see the full value; the consumer sees it once, on their My Apps page.
- **Issue date / expiry**: when the key was provisioned and when it lapses, if an expiry is set.
- **Status**: *Active*, *Suspended*, or *Revoked* per credential.
- **Last used**: populated from gateway analytics on a one-to-five-minute poll. An empty value long after issue means the consumer has not integrated the key.

## Configure

1. Expand **API MANAGEMENT** in the sidebar, then click **Manage Apps**. The list shows every app with its **Owner**, **Organisation**, and **Subscriptions** count, newest first.
2. Click an **App Name** to open its detail page, then move through the four tabs.
3. On **Overview**, confirm the registering user, the app description, and any redirect URLs match the requested Product and point at the consumer's real domain.
4. On **Credentials**, read each issued key by type, prefix, issue date, status, and **Last used**. Confirm one Active key per subscription; multiple usually means a prior rotation did not complete.
5. To rotate, click **Rotate** on the credential row, fill the **Reason** field, then **Confirm**. The marketplace issues a new key, revokes the old one on the gateway atomically, and emails the consumer the new key location.
6. On **Subscriptions**, review every Product the app is subscribed to, one row per subscription, with its plan, state, approval date, and last-call timestamp. Approving from here is identical to approving from the queue.

## Verify

- Confirm the **App Name** link opens the detail page with all four tabs visible.
- After a rotation, confirm a new credential row sits at the top with the current timestamp, and the prior row's status reads *Revoked*.
- Ask the consumer to retry a call: the new key returns `2xx` and the old key returns `401`.
- Confirm the **Subscriptions** tab lists the same subscriptions you find on Manage Product Subscriptions when filtered by that app name.
- Open the audit log for the rotation and confirm the **Reason** is captured against your username.

{% hint style="warning" %}
**Caution:** Rotation invalidates the old key the moment the new one is provisioned. Coordinate with the consumer and plan it outside their peak hours so in-flight calls are not dropped.
**Result:** You have a full view of the app, its credentials, and its linked subscriptions, and any compromised key is replaced atomically with the consumer notified.
{% endhint %}