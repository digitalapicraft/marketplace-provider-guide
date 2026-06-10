---
icon: bell-concierge
---

Review pending subscription requests for your Products and move each one through its lifecycle. This is your daily triage surface: every time a consumer registers an app and clicks **Subscribe**, a row lands here for a decision. It pairs with [Consumer apps & credentials](feat-apps-and-credentials.md#consumer-apps-and-credentials), where you inspect the app before you approve.

![Figure. The Manage Product Subscriptions queue with per-Product rows across multiple states.](.gitbook/assets/screenshots/provider/admin-portal-product-subscriptions.png)

## What you review

The queue is a flat list of requests; the marketplace does not enforce an approval policy or auto-expire pending rows, so triage cadence is on you. Each row carries the context you need to decide:

- **Status**: *Pending*, *Active*, *Suspended*, or *Revoked*. The available row actions depend on this value.
- **App**: the consumer-side app the subscription belongs to. Click it to open the app detail page and inspect credentials and the registering user.
- **Consumer**: the user who registered the app; click through to their profile and email for outreach.
- **Product / Plan**: the API Product and plan tier the request is against. Most Products ship a single plan.
- **Created**: request timestamp in the portal timezone. Sort ascending so the oldest requests rise to the top.
- **Notes** (Approve): optional text that surfaces in the consumer's notification email.
- **Reason** (Reject / Suspend / Revoke): written to both the consumer's email and the audit log.

## Subscription states

- **Pending**: intent recorded, no key on the gateway, no calls succeed.
- **Active**: approved; a key is registered on the gateway and quota and rate-limit counters apply.
- **Suspended**: paused; the key and call history are preserved and the gateway returns `401`. Quota counters freeze in place.
- **Revoked**: ended; the key is destroyed on the gateway and the row stays for audit. Resuming means the consumer re-subscribes and you re-approve.

## Configure

1. Expand **API MANAGEMENT** in the sidebar, then click **Manage Product Subscriptions**.
2. Filter **Status** to *Pending* and sort by **Created** ascending. Filter state is recorded in the URL (`?status=pending`), so you can bookmark the morning view.
3. Click a row's **App** name to inspect the consumer's app, registering user, and requested Product before deciding.
4. To approve, open the row's **Actions** menu, click **Approve**, add optional **Notes** for the consumer's email, then click **Confirm**. The status flips to *Active* and the gateway accepts calls with the new key.
5. To reject a pending request, click **Reject Subscription**, fill the **Reason** field with one to three actionable sentences, then **Confirm**. The status moves to *Revoked* and the reason reaches the consumer.
6. To pause a live subscription, click **Suspend**, enter a **Reason**, and **Confirm**. Click **Reactivate** later to resume on the same key.
7. To end a subscription, click **Revoke Access**, enter a **Reason**, and **Confirm**. The gateway destroys the key and the row settles under the *Revoked* filter.

![Figure. The Processed Requests view, listing each approved and rejected subscription with its recorded disposition.](.gitbook/assets/screenshots/provider/subscriptions-processed.png)

Once you act on a request it leaves the pending list and lands on the **Processed Requests** view, which keeps every approval (*Active*) and rejection (*Revoked*) with the disposition you assigned and the actions that remain available.

## Verify

- Confirm the queue row's Status changes (Pending to Active, Suspended, or Revoked) without a page-reload error.
- Open the app's **Credentials** tab after an approval and confirm a new key row sits at the top with the current timestamp.
- Ask the consumer to retry a call: an approved key returns `2xx`; a suspended or revoked key returns `401`.
- Open the **Audit** tab on the subscription detail page and confirm the actor, the state change, and the **Reason** text were recorded.

{% hint style="success" %}
**Tip:** Reject and Revoke are final for a row. Prefer Suspend when there is any chance of resuming, since it keeps the consumer's deployed key valid.
**Result:** Every request has a clear disposition, and each subscription sits in the correct state at the gateway with the consumer notified.
{% endhint %}