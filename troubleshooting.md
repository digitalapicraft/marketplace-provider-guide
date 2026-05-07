| Gateway connection test fails with "endpoint unreachable" | The admin URL is wrong, points at the runtime endpoint, or your network blocks outbound calls to the gateway. | Open the connection in **Edit**, replace the URL with the gateway's admin endpoint (not the runtime endpoint), and click **Test connection** again. |
| API import returns 0 APIs | The connected service account or API key lacks read permission, or the gateway has no APIs in the chosen scope (organisation, region, project). | Confirm the credential has the gateway's reader role, then re-run **Import APIs** from **Existing API Sources**. |
| Governance score won't update after edit | The score reflects the last completed scan; an edit to the spec doesn't trigger a new scan automatically. | Open the Governance Report and click **Run scan** for the API, or click **Scan All** to rescan every API. |
| Subscription stuck **Pending** | No Provider has approved the request, or the API Product's plan requires approval and your role doesn't include the approval permission. | Open **Subscriptions**, filter by **Pending**, and click **Approve** on the row; ask your Org Admin to grant approval permissions if the action button is hidden. |
| Consumer reports 401s after approval | The consumer hasn't pulled their App's API key, or they're calling with the old key from before they re-subscribed. | Ask the consumer to open their App, copy the current key from the **Credentials** section, and replace the key in their client code. |
| Provider Analytics page shows no data | The connected gateway hasn't reported metrics yet, or the time-range filter excludes the period when traffic ran. | Widen the time range to **Last 30 days**, confirm the gateway's metrics export is configured, and reload the page. |
| MCP server register fails with "transport unsupported" | The transport you selected (SSE, stdio, HTTP) isn't supported by the marketplace's current MCP runtime. | Re-open the form and choose a supported transport (currently HTTP and SSE), then click **Save**. |
| Can't see **Manage API Sources** in the sidebar | Your account holds only the **API Consumer** role; **Manage API Sources** is hidden for non-providers. | Ask your Org Admin to grant your account the **API Provider** role and sign back in. |
| System notification not appearing for users | The notification was saved as **Draft**, or its **Visible from** date is in the future. | Open **System Notifications**, edit the notification, set status to **Published**, and confirm the **Visible from** date is now or in the past. |
| API Product **Publish** button greyed out | The API Product has no APIs attached, no plan, or its **Visibility** isn't set. | Open the API Product, attach at least one API, attach a plan with quota and rate-limit values, choose a **Visibility**, and the button enables. |
| Webhook deliveries failing | The receiving endpoint returned a non-2xx status, or your webhook secret rotated and the receiver is rejecting the signature. | Open **SETTINGS > Webhooks**, view the failure log, fix the receiving endpoint, and click **Replay** on the failed deliveries. |
| Toast says "Save failed: required field" but no field is highlighted | The required field is on a tab or accordion section you haven't opened. | Scroll to the top of the form, switch to the **Advanced** or **Plans** tab, and look for a tab title with a red dot indicating an error. |

### Gateway connection test fails with "endpoint unreachable"

**Symptom.** You completed the Add Connection form, clicked Test connection, and the marketplace returned a red banner with the message "endpoint unreachable" or a similar network-level error. The Save button still works, but no APIs will import.

**Likely cause.** Three causes account for almost every case. First, the URL entered is your gateway's runtime endpoint (the one consumers call) rather than the admin endpoint (the management API). Second, the URL is correct but the marketplace's egress IP is blocked by your gateway's firewall or VPC controls. Third, the URL is correct but uses a hostname that does not resolve on the public internet.

**Resolution.**

1. Re-read the Before you start section of Chapter 3.2 and confirm the admin endpoint is in use, not the runtime endpoint.
2. Edit the connection and replace the URL.
3. Click Test connection. If it still fails, open a terminal and run a `curl` against the same URL from a public host to confirm the URL resolves and responds.
4. If the URL works from a public host but the marketplace test fails, contact your network team about allowing the marketplace egress IP through your gateway's firewall.

### API import returns 0 APIs

**Symptom.** You opened Existing API Sources, clicked Import APIs on a connected gateway, and the import completed with the message "0 APIs imported". No new rows appeared in Manage APIs.

**Likely cause.** The credential supplied authenticates against the gateway but lacks the role needed to list APIs. On Apigee X, the required role is Apigee API Reader. On Kong, the admin token must have read access to the relevant workspace. On AWS, the IAM user needs `apigateway:GET` on the region's resources. Less commonly, the gateway is healthy but the chosen scope (organisation, region, project) genuinely contains no APIs.

**Resolution.**

1. Open the gateway's IAM console and confirm the credential's role.
2. Open Existing API Sources in the marketplace and click Edit on the connection.
3. Click **Test connection** to re-authenticate. The test passes whether the credential can read APIs or not, so this step alone does not diagnose the role problem — but it confirms the credential is at least valid.
4. Replace the credential with one that holds the gateway's reader role.
5. Click Save, then Import APIs again.

### Governance score does not update after edit

**Symptom.** You edited an API's spec, fixed several governance violations, and reloaded the Governance Report. The score is unchanged.

**Likely cause.** Governance scoring is event-driven, not synchronous. The score on the report is the score from the last completed scan. Editing the spec does not trigger a new scan automatically, so the score reflects the pre-edit state until a rescan is requested.

**Resolution.**

1. Open the Governance Report under API MANAGEMENT > API Governance Report.
2. Locate the row for the edited API.
3. Click **Run scan** on that row to rescan only this API, or click **Scan All** at the top of the page to rescan every API.
4. Wait for the scan to complete (typically a few seconds per API).
5. Reload the page. The score reflects the post-edit state.

### Subscription stuck Pending

**Symptom.** A consumer requested a subscription days ago. The subscription row in Subscriptions still shows status Pending and the consumer reports that they cannot call the API.

**Likely cause.** Pending indicates a Provider must approve before the subscription becomes Active. Either nobody has reviewed the request yet, or the API Product's plan was created with Auto-approve disabled and your Organisation's notification preferences hid the approval prompt.

**Resolution.**

1. Open **ADMINISTRATION > Subscriptions**.
2. Filter the list by **Status: Pending**.
3. Open the subscription row and review the request.
4. Click **Approve** to move it to Active, or **Reject** with a reason if it should not be granted.
5. To prevent the same backlog forming again, open the API Product's plan and either enable Auto-approve or update your notification preferences so Subscription requests reach you in real time.

### Consumer reports 401s after approval

**Symptom.** You approved a subscription. The consumer reports their first calls to the API now return 401 Unauthorized.

**Likely cause.** The consumer's App holds the API key. After approving a subscription, the App's keys may have changed (a new key was issued for the new subscription, or an existing key was scoped to include it). The consumer is calling with their old key.

**Resolution.**

1. Ask the consumer to open their App in the marketplace.
2. Have them open the Credentials section and copy the current API key.
3. Have them replace the key in their client code or environment variable.
4. Have them retry the call. If it still returns 401, confirm the App's subscription to your API Product is Active (not Suspended) and that the call is hitting the runtime endpoint, not the admin endpoint.

### Provider Analytics page shows no data

**Symptom.** You opened ADMINISTRATION > Provider Analytics. The chart area is empty and the table reads "No data for the selected range."

**Likely cause.** Three causes are common. First, the time range defaults to Last 24 hours and your traffic ran outside that window. Second, the connected gateway has not been configured to export metrics back to the marketplace. Third, the API Product has no Active subscriptions, so no traffic is being attributed to it.

**Resolution.**

1. Widen the time range to Last 30 days using the date filter at the top of the page.
2. If the chart is still empty, open the gateway's metrics-export configuration and confirm the marketplace's analytics endpoint is registered as a destination.
3. Open ADMINISTRATION > Subscriptions and confirm at least one subscription is Active.
4. Wait up to 15 minutes for the next metrics-export cycle, then reload.

### MCP server register fails with "transport unsupported"

**Symptom.** You opened ADMINISTRATION > MCP Servers, completed the form, clicked Save, and the form returned a red banner with "transport unsupported".

**Likely cause.** A transport was selected that the marketplace's MCP runtime does not yet support. The currently supported transports are HTTP and SSE. The stdio transport is not supported because the marketplace runs MCP servers as remote processes, not as subprocesses of the user's agent.

**Resolution.**

1. Re-open the MCP Server form.
2. From the **Transport** dropdown, select HTTP or SSE.
3. Confirm the **Endpoint URL** matches the transport (HTTP servers expose a request endpoint; SSE servers expose a stream endpoint).
4. Click **Save**.

### Cannot see Manage API Sources in the sidebar

**Symptom.** You signed in but the API MANAGEMENT group in the left sidebar does not show Manage API Sources. Other members of your Organisation can see it.

**Likely cause.** The link is restricted to the API Provider, Org Admin, and Portal Admin roles. Your account holds only the API Consumer role.

**Resolution.**

1. Request that your Org Admin grant your account the API Provider role under ORGANISATION > Members.
2. Sign out and sign back in. Role changes take effect only on the next sign-in.
3. Confirm the API MANAGEMENT group now shows Manage API Sources, Manage APIs, Manage API Products, and API Governance Report.

### System notification not appearing for users

**Symptom.** You posted a system notification under SETTINGS > System Notifications and saved it. Users report they do not see it in their in-app inbox.

**Likely cause.** The notification was saved as Draft rather than Published, or its Visible from field is set to a future date so the notification has not activated yet.

**Resolution.**

1. Open SETTINGS > System Notifications.
2. Locate the notification in the list. Check its status column.
3. Click **Edit**.
4. Set status to Published and confirm Visible from is now or in the past.
5. Click **Save**. Users see the notification in their inbox on their next page load.

### API Product Publish button greyed out

**Symptom.** You opened your API Product and want to publish it. The Publish button at the top of the page is disabled and cannot be clicked.

**Likely cause.** The marketplace blocks publishing if the API Product is missing one of the required ingredients: at least one API, at least one plan with quota and rate-limit values set, and a Visibility value.

**Resolution.**

1. Scroll to the APIs section and confirm at least one API is attached. If not, click **Attach API** and add one.
2. Scroll to the Plans section and confirm at least one plan exists with Quota and Rate limit values. If not, click **Add plan** and complete those fields.
3. Scroll to the Visibility field and select Public, Authenticated, or Private.
4. The Publish button enables. Click it.

### Webhook deliveries failing

**Symptom.** You configured a webhook to receive subscription or governance events. Events occur in the marketplace but your receiver does not see them. The webhook log shows red Failed rows.

**Likely cause.** Either your receiver returned a non-2xx status (5xx network error, 4xx authentication failure), or the webhook signature secret rotated on either side and the receiver is rejecting requests with an invalid signature.

**Resolution.**

1. Open SETTINGS > Webhooks and click into the failing webhook.
2. View the Delivery log to see the response code and body the receiver returned.
3. If the response is 5xx, fix the receiver. If 401/403, re-confirm the signing secret matches between the marketplace and the receiver.
4. Click **Replay** on each failed delivery once the receiver is healthy.

### Toast says "Save failed: required field" but no field is highlighted

**Symptom.** You completed a long form, clicked Save, and the marketplace returned a red toast naming a required field. Scrolling the form, you cannot locate a field highlighted in red.

**Likely cause.** Long forms are split into tabs or collapsed accordion sections. The required field lives on a tab not yet opened, so its red border is not visible from the current tab. The tab title gains a red dot indicating the error, but it is small and easy to miss.

**Resolution.**

1. Scroll to the top of the form.
2. Look at the tab strip (or accordion list) and locate the tab with a red dot or red error count.
3. Click that tab. The field with the validation error is now visible with a red border.
4. Complete the field and click **Save**.
