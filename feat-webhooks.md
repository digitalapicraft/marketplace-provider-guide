---
icon: bolt
---

Webhooks are the marketplace's outbound notification channel for systems other than email. When something happens (a subscription is created, an API is published, a governance scan completes), the marketplace POSTs a signed JSON payload to a URL you control. Use webhooks to drive Slack messages, ticketing-system tickets, internal dashboards, or other downstream automations in near-real-time.

![Figure. The Webhook configuration list with name, URL, status, and last-delivery columns.](.gitbook/assets/screenshots/provider/admin-config-services-webhook.png)

## What you configure

A webhook registration ties a receiver URL to the events it cares about and the secret used to sign each payload:

- **Label**: an internal name that helps you find the webhook later, for example "Slack #api-ops".
- **Type**: set to *Outgoing*. Outgoing webhooks POST new events to the configured URL.
- **Content Type**: the body format. Leave it on `application/json` unless the receiver needs another format.
- **Secret**: a long random string the marketplace uses to sign every payload. The receiver verifies the signature before acting. Treat it like a password and do not reuse it across webhooks.
- **Token**: an optional value the receiver checks on the incoming hook as a second factor alongside the signature.
- **URL**: the receiver endpoint, entered under **Outgoing Webhook Settings**. It must accept JSON, return a 2xx within 10 seconds, and use HTTPS in production.
- **Events**: the subscriptions, ticked in the same panel. Subscribe only to what the receiver needs.
- **Status**: *Active* to enable delivery, *Inactive* to register without firing.

![Figure. The Add webhook form with Label, Type, Content Type, Secret, and Token fields above the Outgoing Webhook Settings section.](.gitbook/assets/screenshots/provider/admin-config-services-webhook-add.png)

## Configure

1. Expand **SETTINGS** in the sidebar, then click **Webhook**.
2. Click **Add webhook** in the top-right.
3. Enter a **Label**, set **Type** to *Outgoing*, and leave **Content Type** on `application/json`.
4. Paste a long random string into **Secret**, and optionally a **Token**.
5. Open **Outgoing Webhook Settings**, enter the receiver **URL** (HTTPS in production), and tick the events to subscribe to.
6. Set **Status** to *Active* and click **Save**.
7. Open the row's **Operations** menu, click **Send test**, and confirm the receiver returns a `2xx`.

## Options

The events you can subscribe to span the platform's main activity:

- **Subscription events**: Subscription Created, Subscription Approved.
- **Governance events**: Governance Scan Complete.
- **API events**: API Published.
- **Product / membership events**: Product changes and Member Invited.

## Verify

- Confirm the row appears in the **Webhook** list with status *Active*.
- Click **Send test** and confirm the receiver acknowledges the synthetic event with a `2xx`.
- Trigger a real subscribed event and confirm the delivery shows up in the **Deliveries** tab, where each row records the response code and timestamp. Use **Retry** on any failed delivery.

{% hint style="info" %}
**Note:** A delivery times out after 10 seconds. Have the receiver acknowledge quickly and queue the work asynchronously, or the delivery fails and retries.
{% endhint %}

{% hint style="success" %}
**Result:** Every subscribed event fires a signed JSON POST to the receiver URL, and each attempt is logged under the webhook's Deliveries tab.
{% endhint %}