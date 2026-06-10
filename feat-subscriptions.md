---
icon: bell-concierge
---

Review pending subscription requests for your Products and move each subscription through its lifecycle: Active, Suspended, or Revoked. Use this surface as your daily triage point whenever a consumer subscribes.

![Figure. The Manage Product Subscriptions queue with per-Product rows across multiple states.](.gitbook/assets/screenshots/provider/admin-portal-product-subscriptions.png)

## Configure

1. Expand **API MANAGEMENT** in the sidebar, then click **Manage Product Subscriptions**.
2. Filter **Status** to *Pending* and sort by **Created** ascending so the oldest requests rise to the top.
3. Click a row's **App** name to inspect the consumer's app, registering user, and requested Product before deciding.
4. To approve, open the row's **Actions** menu, click **Approve**, add optional **Notes** for the consumer's email, then click **Confirm**. The status flips to *Active* and the gateway accepts calls with the new key.
5. To reject, click **Reject Subscription**, fill the **Reason** field with one to three actionable sentences, then **Confirm**. The status moves to *Revoked* and the reason reaches the consumer.
6. To pause a live subscription, click **Suspend**, enter a **Reason**, and **Confirm**. The gateway returns `401` while the key and call history are preserved. Click **Reactivate** to resume.
7. To end a subscription, click **Revoke Access**, enter a **Reason**, and **Confirm**. The gateway destroys the key and the row stays under the *Revoked* filter for audit.

{% hint style="success" %}
**Result:** Every request has a clear disposition, and each subscription sits in the correct state at the gateway.
**Tip:** Reject and Revoke are final for a row. Prefer Suspend when there is any chance of resuming.
{% endhint %}

### Key columns

1. **Status.** *Pending*, *Active*, *Suspended*, or *Revoked*. The available actions depend on this value.
2. **App.** The consumer-side app the subscription belongs to. Click it to open the app detail page.
3. **Actions.** Per-row menu carrying Approve, Reject Subscription, Suspend, Reactivate, and Revoke Access.