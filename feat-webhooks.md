---
icon: bolt
---

Emit outbound webhook deliveries when subscription, governance, API, and Product events fire, so downstream systems can react in near-real-time. Use webhooks to drive Slack messages, tickets, dashboards, or other automations.

![Figure. The Add webhook form with Label, Type, Content Type, Secret, and Token fields above the Outgoing Webhook Settings section.](.gitbook/assets/screenshots/provider/admin-config-services-webhook-add.png)

## Configure

1. Expand **SETTINGS** in the sidebar, then click **Webhook**.
2. Click **Add webhook** in the top-right.
3. Enter a **Label** that helps you find this webhook later, for example "Slack #api-ops".
4. Set **Type** to *Outgoing* and leave **Content Type** on `application/json` unless the receiver needs another format.
5. Paste a long random string into **Secret**. The marketplace signs every payload with it. Treat it like a password and do not reuse it.
6. Open **Outgoing Webhook Settings**, enter the receiver **URL** (HTTPS in production), and tick the events to subscribe to.
7. Set **Status** to *Active* and click **Save**.
8. Open the row's **Operations** menu, click **Send test**, and confirm the receiver returns a `2xx`.

{% hint style="success" %}
**Result:** Every subscribed event fires a signed JSON POST to the receiver URL. Deliveries appear under the webhook's **Deliveries** tab.
**Tip:** A delivery times out after 10 seconds. Have the receiver acknowledge quickly and queue the work asynchronously.
{% endhint %}

### Key fields

1. **URL.** The receiver endpoint. Must accept JSON and respond within 10 seconds.
2. **Secret.** Signs every payload. The receiver verifies the signature before acting.
3. **Events.** One or more event names. Subscribe only to what the receiver needs.