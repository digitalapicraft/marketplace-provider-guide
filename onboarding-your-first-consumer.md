---
icon: user-plus
---

Once your API Product is published, the next event in the marketplace is a subscription request. A consumer signs in to the portal, locates your Product, registers an app, and clicks **Subscribe**. From that moment, a row appears in your queue and a clock starts ticking on your end. This chapter walks the full lifecycle: where pending requests land, how to inspect the consumer's app before you approve, what an approval actually changes on the gateway, how to manage and rotate the credentials the consumer holds, how to read the first calls in Provider Analytics, and how to suspend or revoke access when the relationship needs to pause or end.

You will learn:

- How a subscription moves between Pending, Active, Suspended, and Revoked, and which actor triggers each transition.
- How to read the Manage Product Subscriptions queue, including every column, filter, and row action.
- How to inspect a consumer's app and the credentials registered against it.
- How to approve a subscription with optional notes, reject one with a clear rationale, and rotate a consumer's credential when the key is suspected leaked.
- How to suspend a subscription to pause traffic without destroying the key, and when to escalate to a full revoke.
- How to look up an active subscription's traffic in Provider Analytics and confirm the first calls landed cleanly.

Allow ~45 minutes to work through the chapter end to end on a fresh subscription.

## Understanding the subscription lifecycle

Before you act on the queue, hold a clear picture of the four states and who moves a subscription between them. The marketplace exposes the state machine in three places: the **Status** column on Manage Product Subscriptions, the **Status** pill on the subscription detail page, and the audit log entry attached to every transition.

#### Read the state machine

Use this task once, on first read, to anchor every later action against a clear mental model.

A subscription occupies exactly one of four states at any time:

- **Pending.** The consumer has clicked **Subscribe** on the storefront. The marketplace has recorded the intent and added the row to your queue. No API key is registered on the gateway and no calls succeed.
- **Active.** You have approved the request. The marketplace has registered a key on the gateway, sent the consumer a notification, and the gateway accepts calls authenticated with that key. Quota and rate-limit counters apply.
- **Suspended.** You have paused the subscription. The key is preserved, the call history is preserved, and the gateway returns `401` for any call authenticated with the key. Quota counters freeze where they are.
- **Revoked.** You have ended the subscription. The marketplace destroys the key on the gateway and the row stays on the queue with state **Revoked** for audit. To resume the relationship, the consumer must re-subscribe and you must re-approve.

Five transitions move a subscription between states:

1. **Pending to Active.** Triggered by you (API Provider or Portal Admin) when you click **Approve**. The marketplace provisions the key on the gateway, sets state to *Active*, and queues a notification email to the consumer.
2. **Pending to Revoked.** Triggered by you when you click **Reject Subscription**. The marketplace records the rejection reason, sends a rejection email, and the queue row settles into *Revoked*.
3. **Active to Suspended.** Triggered by you when you click the suspend action. The gateway begins rejecting calls authenticated by the key.
4. **Suspended to Active.** Triggered by you when you click the reactivate action. The gateway resumes accepting calls without any change to the key.
5. **Active or Suspended to Revoked.** Triggered by you when you click **Revoke Access**. The gateway destroys the key. The row stays for audit; the relationship is final.

{% hint style="info" %}
**Note:** Two list views show subscriptions: **Manage API Subscriptions** at `/admin/portal/subscriptions` (per-API rows) and **Manage Product Subscriptions** at `/admin/portal/product-subscriptions` (per-Product rows). Use the Product view as the default. The API view is only useful when a consumer subscribed to an API outside of any Product, which is rare for marketplaces that lead with Products.
{% endhint %}

{% hint style="success" %}
**Tip:** Treat the audit log as the source of truth for "who did what". Every transition above writes an entry with the actor's username, the target subscription, the previous state, the new state, and any **Reason** text. The log is reachable from the subscription detail page under the **Audit** tab.
{% endhint %}

{% hint style="warning" %}
**Caution:** The marketplace does not auto-expire pending requests. A request sitting in *Pending* for six months is still actionable. Build a regular review cadence into your operations so consumers do not lose interest while waiting.
{% endhint %}

## Reviewing the subscription approval queue

The approval queue is the single page where every new request lands. Most provider mornings start here.

#### Open the Manage Product Subscriptions queue

Use this task as your first stop each working day, and any time you receive a subscription notification email.

#### Before you start

- **Confirm your portal sends email notifications.** If the SMTP transport at `/admin/config/system/symfony-mailer-lite-transport` is not configured, you will not receive an email when a request lands. Without email, the queue must be checked manually each day.
- **Define your approval policy.** Write down the rules your team applies before opening the queue: which Products auto-approve, which require procurement sign-off, which require a security review. The marketplace does not enforce policy for you; the queue is a flat list.
- **Have a kickoff email template ready.** Approval triggers a transactional email from the marketplace, but the transactional email is generic. Most teams send a personalised follow-up alongside.

To open the queue:

1. From the left sidebar, expand **API MANAGEMENT**, then click **Manage Product Subscriptions**. The page opens at `/admin/portal/product-subscriptions`.
2. The default view shows every subscription regardless of state, sorted by request timestamp descending. The newest requests sit at the top.
3. Read the column headers from left to right: **App**, **Consumer**, **Product**, **Plan**, **Status**, **Created**, and **Actions**.
4. Scroll the page to confirm pagination sits below the table at 25 rows per page by default.

![Figure 8-1. The Manage Product Subscriptions list with per-Product subscription rows across multiple states.](.gitbook/assets/screenshots/provider/admin-portal-product-subscriptions.png)

The numbered callouts in Figure 8-1 are:

1. **App column.** The name of the consumer-side app the subscription belongs to. Click the app name to open its detail page in Manage Apps, where the credentials and registering user are visible.
2. **Consumer column.** The marketplace user who registered the app. Click to open their profile and see their other apps, their organisation, and their contact email.
3. **Product column.** The API Product the subscription is against. Click to jump to the Product detail in Manage API Products.
4. **Plan column.** The plan tier on the Product. Most Products ship with a single plan, in which case the value matches the Product name.
5. **Status column.** *Pending*, *Active*, *Suspended*, or *Revoked*. The set of available actions on the row depends on which value sits here.
6. **Created column.** Timestamp of the original subscription request, in the portal's configured timezone.
7. **Actions menu.** Per-row dropdown carrying **Approve**, **Reject Subscription**, **Suspend**, **Reactivate**, and **Revoke Access**. The visible entries depend on the current Status.

{% hint style="success" %}
**Result:** The full queue is on screen with enough context per row to triage which to act on first.
{% endhint %}

{% hint style="success" %}
**Tip:** Sort by **Created** ascending so the oldest requests rise to the top. Consumers waiting more than a few business days lose interest. A queue that triages oldest-first naturally prevents the worst case.
{% endhint %}

{% hint style="info" %}
**Note:** The status filter is sticky in the URL. Once you select *Pending*, the URL records `?status=pending` and a bookmark returns to the same filtered view next time you open it.
{% endhint %}

#### Filter the queue by status, Product, or app

Use this task to narrow a busy queue down to the slice you intend to action this morning.

To filter:

1. On the queue page, locate the filter row above the table. Three filters sit there: **Status**, **Product**, and **App**.
2. From the **Status** dropdown, pick *Pending* to see only requests awaiting decision, *Active* to see live subscriptions, *Suspended* to see paused ones, or *Revoked* to audit ended ones. Leave at *All* to see everything.
3. From the **Product** dropdown, pick a single API Product to focus on subscriptions to one Product line. Useful when one Product is launching and the queue has dozens of related rows.
4. In the **App** search box, type part of an app name to filter by app. The match is partial and case-insensitive.
5. Read the URL after applying filters. The marketplace records each filter as a URL parameter (`?status=pending&product=42`), so you can bookmark or share a filtered view.
6. To clear filters, click **Reset** above the filter row. The list returns to All / All / empty.

{% hint style="success" %}
**Tip:** Bookmark the URL for `?status=pending` and pin it. The morning queue review then takes one click rather than two clicks and a dropdown.
{% endhint %}

{% hint style="info" %}
**Note:** Pagination preserves filter state. Moving to page two does not drop your Status or Product selection.
{% endhint %}

#### Review the pending queue and triage

Use this task to walk the list of pending requests and assign each one a disposition: approve now, approve after a check, reject, or hold for clarification.

To triage:

1. From the queue, apply the *Pending* status filter so only actionable rows remain.
2. Sort by **Created** ascending so the oldest is at the top.
3. For each row, click the **App** name to open the consumer's app detail page in a new tab. The detail page is covered in the next section.
4. Read the registering user, the app description, the requested Product, and the request timestamp. Decide whether the request fits your policy.
5. Return to the queue tab. Use the **Actions** menu on the row to **Approve**, **Reject Subscription**, or leave it alone for now.
6. For requests that need clarification before a decision, leave the row in *Pending* and email the consumer directly. The queue keeps the row visible until you act.

{% hint style="warning" %}
**Caution:** A *Pending* row that you neither approve nor reject still counts against the consumer's experience. The marketplace shows the consumer the same "Pending" badge on their My Subscriptions page for six months as it does for six minutes. Set a service-level expectation with your team (for example, decide within two business days) and stick to it.
{% endhint %}

#### Verify

1. Confirm the **Manage Product Subscriptions** page loads with a non-empty queue when at least one consumer has subscribed.
2. Confirm the *Pending* filter hides every Active, Suspended, and Revoked row.
3. Click one row's **App** name and confirm the consumer's app detail page opens with the registering user and app description visible.
4. Confirm the URL preserves the filter state after navigating to page two.

## Inspecting the consumer's app and credentials

Approval is not the only action you take in this chapter; inspection comes first. Before you grant access, look at the app the consumer registered, the user who registered it, and the credentials it holds. Manage Apps is the surface for all three.

#### Open the Manage Apps list

Use this task to see every consumer app the marketplace knows about, regardless of subscription state.

To open the list:

1. From the left sidebar, expand **API MANAGEMENT**, then click **Manage Apps**. The page opens at `/admin/portal/manage-apps`.
2. The default view shows every app in the marketplace, sorted by **Created** descending.
3. Read the columns: **App Name**, **Owner**, **Organisation**, **Subscriptions**, **Created**, and **Actions**.
4. Scroll the table to confirm pagination sits below at 25 rows per page.

![Figure 8-2. The Manage Apps list showing consumer-side apps, their owners, and their subscription counts.](.gitbook/assets/screenshots/provider/admin-portal-manage-apps.png)

The numbered callouts in Figure 8-2 are:

1. **App Name column.** The name the consumer gave their app at registration. Click to open the app detail page, where credentials and subscriptions surface.
2. **Owner column.** The marketplace user who registered the app. Click to open their profile.
3. **Organisation column.** The organisation the owner belongs to. Useful for grouping apps by tenant when several consumers from the same organisation each register their own.
4. **Subscriptions column.** Count of active and pending subscriptions tied to this app. A high count is normal for mature apps; a count of zero usually means the consumer registered the app but never subscribed to a Product.
5. **Created column.** When the app was registered on the portal. The most recent apps sit at the top by default.
6. **Actions menu.** Per-row dropdown carrying **View**, **Edit**, and **Delete**. Edit and Delete are typically reserved for Portal Admin; API Providers usually act via the subscription queue instead.

{% hint style="success" %}
**Tip:** Filter the list by **Organisation** when triaging requests from a single tenant. A procurement-heavy approval flow often runs one organisation at a time.
{% endhint %}

{% hint style="info" %}
**Note:** Manage Apps shows apps; it does not show subscriptions inline. To see the subscription state for an app, either click into the app detail page or read the queue at Manage Product Subscriptions.
{% endhint %}

#### Inspect a consumer's app

Use this task before approving any subscription request to confirm the app is legitimate, the description matches the requested Product, and the registered redirect URLs (if any) point at the consumer's real infrastructure.

To inspect:

1. From the subscription queue or from Manage Apps, click the **App Name** to open its detail page.
2. The detail page has four tabs: **Overview**, **Credentials**, **Subscriptions**, and **Activity**.
3. On the **Overview** tab, read the registering user, the organisation, the app description, the registered redirect URLs, and any contact email the consumer provided.
4. Confirm the description matches the requested Product. A vague description against a high-value Product is a flag for a follow-up question.
5. Read the redirect URLs. They should point at the consumer's domain. A localhost URL is acceptable during development but not for a Product slated for production traffic.

The Overview tab gathers the registering user, the organisation, the app description, the registered redirect URLs, and any contact email into a single read-only panel.

{% hint style="info" %}
**Note:** The marketplace shows the registering user as the *primary contact* for the app. Organisations with several developers often have one user register the app on behalf of the team; treat the registering user as a starting point for outreach, not the only person involved.
{% endhint %}

{% hint style="success" %}
**Tip:** Cross-check the registering user's email domain against the organisation's domain. A mismatch usually means the user signed up with a personal address; ask for a corporate address before approving high-value subscriptions.
{% endhint %}

#### Inspect the credentials registered against an app

Use this task to see what credentials the marketplace has provisioned for an app, when they were issued, and which keys are still in use.

To inspect credentials:

1. On the app detail page, click the **Credentials** tab.
2. Read the credentials table. Each row shows the credential type (typically API key), the credential prefix (the first few characters of the key, never the full key), the issue date, the expiry date if set, and the current status (Active, Suspended, Revoked).
3. For each Active credential, confirm the consumer has only one key per subscription. Multiple Active credentials per subscription is unusual and usually means a previous rotation was not completed cleanly.
4. Read the **Last used** timestamp on each credential. A credential issued months ago with no Last used is either a credential the consumer never integrated or a credential they have abandoned in favour of another.

The Credentials tab lists each issued key with its type, prefix, issue date, optional expiry, status, and last-used timestamp, one row per credential.

{% hint style="warning" %}
**Caution:** The full credential value is shown to the consumer exactly once, immediately after issue, on their My Apps page on the portal. Providers see only the prefix. If a consumer reports a lost key, the path forward is rotation, not retrieval; nobody can recover the original.
{% endhint %}

{% hint style="info" %}
**Note:** A credential's Last used value updates from gateway analytics on a poll interval typically between one and five minutes. A fresh credential with an empty Last used after thirty minutes is a signal the consumer has not yet integrated, not that the credential is broken.
{% endhint %}

#### Review the app's subscriptions tab

Use this task to see every Product the app is subscribed to, in any state.

To review:

1. On the app detail page, click the **Subscriptions** tab.
2. Read the per-row table: **Product**, **Plan**, **Status**, **Approved**, **Last call**.
3. *Pending* rows here are the same requests you see on the main queue. Approving from the app detail page and approving from the queue have identical effects.
4. *Active* rows show the **Last call** column populated once the consumer has called the API at least once.
5. *Revoked* and *Suspended* rows stay visible for audit. A row that flips to *Revoked* does not disappear from the tab; it remains for the historical record.

The Subscriptions tab lists every Product the app is subscribed to, one row per subscription, with its plan, current state, approval date, and last-call timestamp.

{% hint style="success" %}
**Tip:** Use the Subscriptions tab as a sanity check before approving a high-value Product. If the same app already has three Active subscriptions to your Products with no traffic, the consumer may be staging requests faster than their integration can absorb them.
{% endhint %}

#### Verify

1. Confirm the **App Name** link from the subscription queue opens the app detail page with all four tabs visible.
2. Confirm the **Credentials** tab shows at least one credential row for any app with an Active subscription.
3. Confirm the **Subscriptions** tab lists the same subscriptions you can find on Manage Product Subscriptions when filtered by that app name.

## Approving and rejecting subscriptions

Approval is the moment the relationship goes live. Rejection is the moment you close it before it starts. Both actions are reversible only by re-subscribing, so take the decision deliberately.

#### Approve a subscription

Use this task to grant a consumer access to your Product after the queue triage and the app inspection have both passed.

#### Before you start

- **Confirm the consumer's identity matches your records.** For paid Products this is a procurement check; for free tiers the email domain on the registering user often suffices.
- **Confirm the gateway has capacity.** A single approval is fine. When approving ten subscriptions at once, monitor the gateway's connection metrics, since the marketplace registers each key synchronously against the gateway.
- **Decide whether to send notes.** The approval dialog accepts an optional **Notes** field that surfaces in the consumer's notification email. Use it to point the consumer at your kickoff documentation or to flag any per-consumer constraints.

To approve a subscription:

1. From **Manage Product Subscriptions**, filter to *Pending* and locate the row to approve.
2. Open the **Actions** menu on the row and click **Approve**. A confirmation dialog opens.
3. Read the dialog summary: the consumer's app name, the Product, the plan, and the requested quota.
4. Optionally fill the **Notes** field with text the consumer should see in the notification email. Two to three sentences is the right length.
5. Click **Confirm**. The dialog closes, the row's Status flips to *Active*, and a confirmation toast appears at the top of the page.
6. Behind the scenes, the marketplace provisions the API key on the gateway, registers the key against the consumer's app, and queues the notification email. The full sequence typically completes in under fifteen seconds.

The approval dialog summarises the consumer's app, the Product, the plan, and the requested quota, and offers an optional **Notes** field whose contents surface in the consumer's notification email.

Once you act on a request, it leaves the pending list and lands on the **Processed Requests** view, where every approved and rejected subscription is kept with the disposition you assigned.

![Figure 8-3. The Product Subscriptions page on the Processed Requests view, listing each approved and rejected subscription with its disposition.](.gitbook/assets/screenshots/provider/subscriptions-processed.png)

The numbered callouts in Figure 8-3 are:

1. **Processed Requests tab.** The view that holds every request you have already actioned. Switch here after approving or rejecting to confirm the row recorded the disposition you intended.
2. **App and Consumer columns.** The consumer-side app and the user who registered it, carried over from the pending row so you can trace a processed decision back to its requester.
3. **Product and Plan columns.** The API Product and plan tier the subscription was against.
4. **Status column.** The recorded disposition: *Active* for an approval, *Revoked* for a rejection. This is the post-decision state, not the original pending state.
5. **Actions menu.** Per-row actions that remain available after the decision, such as **Suspend** or **Revoke Access** on an approved row.

{% hint style="success" %}
**Result:** The subscription is *Active*. The gateway accepts calls authenticated with the new key. The consumer sees the key on their My Apps page and receives an email containing the key location and a link to your Product documentation.
{% endhint %}

{% hint style="info" %}
**Note:** The full API key is shown to the consumer on their My Apps page on the portal. Providers see only the prefix in the credentials table; the marketplace deliberately never echoes the full value back to the issuing surface.
{% endhint %}

{% hint style="success" %}
**Tip:** If your plan has a long quota period, for example monthly, approve subscriptions early in the period so the consumer receives a full month of calls. Approving three days before a quota renewal looks ungenerous.
{% endhint %}

#### Verify

1. Confirm the queue row's Status changes from *Pending* to *Active* without a page reload error.
2. Open the app detail page's **Credentials** tab and confirm a new credential row sits at the top with the current timestamp.
3. Confirm the consumer's My Apps page shows the new subscription as *Active* (ask the consumer to refresh, or sign in as a test consumer to check).
4. Wait five minutes, then open the **Last call** column on the subscription detail page; once the consumer makes their first call, the value populates.

#### Reject a pending subscription with a rationale

Use this task when the request fails your policy and you do not intend to onboard the consumer at all.

#### Before you start

- **Have a clear, business-respectful rationale.** The rejection reason is sent verbatim to the consumer's notification email and stored in the audit log. Write it as if the consumer's procurement team will read it, because they likely will.
- **Confirm rejection is the right call.** If the request is borderline, prefer leaving it in *Pending* and emailing the consumer to gather more context. Rejection ends the subscription request lifecycle; the consumer must resubmit to try again.

To reject:

1. From **Manage Product Subscriptions**, filter to *Pending* and locate the row.
2. Open the **Actions** menu on the row and click **Reject Subscription**. A confirmation dialog opens with a **Reason** text area.
3. Fill the **Reason** field with one to three sentences. Be specific enough that the consumer can act on the feedback. "Insufficient procurement detail. Please re-subscribe once your procurement contact has confirmed the contract reference." is a better rejection than "Rejected."
4. Click **Confirm**. The dialog closes, the row's Status moves to *Revoked*, and the consumer receives a notification email containing the Reason text.
5. The row remains in the queue under the *Revoked* filter for audit. You can reopen the row to read the Reason at any later point.

{% hint style="info" %}
**Note:** Reject Subscription is only available on rows currently in *Pending*. To end an *Active* or *Suspended* subscription, use **Revoke Access** instead, which is covered later in this chapter.
{% endhint %}

{% hint style="warning" %}
**Caution:** Rejection is final for this row. If the consumer responds with the missing detail and you decide to onboard them after all, they must subscribe again from scratch; you cannot reanimate a Revoked row.
{% endhint %}

{% hint style="success" %}
**Tip:** A rejected consumer is often a future consumer. Keep the rejection email professional and signal the path forward. Many consumers come back two weeks later with the missing information.
{% endhint %}

#### Verify

1. Confirm the row's Status moves from *Pending* to *Revoked* immediately after confirming the dialog.
2. Confirm the rejection email arrives in the consumer's inbox containing the Reason text you entered.
3. Open the audit log entry for the rejection and confirm the **Reason** is captured against your username and the action timestamp.

## Managing credentials on an active subscription

Once a subscription is active, the credential it holds is a long-lived object. Credentials get leaked, compromised, or simply rotated for hygiene. The marketplace exposes one path for credential management: rotation, which issues a fresh key and revokes the prior one in a single atomic move.

#### Rotate a consumer credential

Use this task when a credential is suspected leaked, when a consumer has reported the key compromised, or when your policy requires periodic rotation regardless of incident.

#### Before you start

- **Coordinate with the consumer.** Rotation invalidates the old key the moment the new one is provisioned. The consumer must swap the key in their app within the rotation window, or calls will fail with `401`.
- **Decide the rotation window.** Some teams rotate immediately, some pre-stage a new key alongside the old one for a few minutes. The marketplace's default behaviour is immediate.
- **Document the reason.** The rotation dialog accepts a **Reason** field. Suspected leak, scheduled rotation, security policy: the reason surfaces in the consumer's notification email and the audit log.

To rotate:

1. Open the consumer's app detail page (from the queue's **App** column or from Manage Apps).
2. Click the **Credentials** tab.
3. Locate the credential row to rotate. The row shows the credential prefix, the issue date, and the current status.
4. Click the **Rotate** action on the row. A confirmation dialog opens with a **Reason** field.
5. Fill the **Reason** field with the rotation rationale. One or two sentences is enough.
6. Click **Confirm**. The marketplace issues a new credential, revokes the old credential on the gateway, and queues a notification email containing the new key location for the consumer.

The rotate dialog carries a **Reason** field for the rotation rationale, which surfaces in the consumer's notification email and the audit log.

{% hint style="success" %}
**Result:** The old credential is *Revoked* on the gateway. A new *Active* credential row sits at the top of the credentials table. The consumer sees the new key on their My Apps page and receives an email with the new key location.
{% endhint %}

{% hint style="warning" %}
**Caution:** Rotation invalidates the old key immediately on the gateway. Any in-flight call authenticated with the old key may complete or may return `401` depending on gateway timing. Any new call with the old key returns `401`. Plan the rotation outside the consumer's peak hours where possible.
{% endhint %}

{% hint style="info" %}
**Note:** The consumer must copy the new key from their My Apps page; the marketplace does not include the full key value in the notification email for security reasons. The email points the consumer at the My Apps URL.
{% endhint %}

{% hint style="success" %}
**Tip:** For incident response (a leaked key reported by a consumer), rotate first, then communicate. The five minutes you save on the rotation can be the five minutes the abuser would have used. The audit log preserves the chain for post-incident review.
{% endhint %}

#### Verify

1. Confirm a new credential row appears at the top of the Credentials tab with the current timestamp.
2. Confirm the prior credential row's status is now *Revoked*.
3. Ask the consumer to update their app and retry a call; confirm the call succeeds with the new key and returns `401` with the old key.
4. Open the audit log entry for the rotation and confirm the **Reason** text is captured against your username.

## Suspending and revoking subscriptions

Sometimes the relationship needs to pause. Sometimes it needs to end. Suspend pauses; revoke ends. Both actions take effect immediately at the gateway.

#### Suspend an active subscription

Use this task when the consumer is in arrears, repeatedly violating the rate limit, or otherwise needs their traffic paused while an issue is resolved. Suspension is reversible.

#### Before you start

- **Confirm the consumer has been warned.** Suspending without notice generates support tickets within minutes. Send the consumer an email at least one working day ahead.
- **Confirm suspension is the right step.** Suspension preserves the key and the call history. If the relationship is over, use revoke instead.
- **Prepare a reason.** The suspend dialog requires a **Reason** field. The reason surfaces in the consumer's notification email and the audit log.

To suspend:

1. From **Manage Product Subscriptions**, locate the row for the active subscription.
2. Open the **Actions** menu and click **Suspend**. A confirmation dialog opens.
3. Fill the **Reason** field with the rationale, for example *Payment overdue, pending resolution by 2026-06-15* or *Rate-limit violation, pending review*.
4. Click **Confirm**. The row's Status moves to *Suspended* and the gateway begins rejecting calls authenticated by the key.
5. The consumer's My Subscriptions page on the portal reflects the new state. The notification email reaches the consumer's primary inbox.

{% hint style="success" %}
**Result:** The gateway returns `401` for any call authenticated with the suspended key. The key itself is preserved. The call history, quota counter, and subscription detail page remain intact.
{% endhint %}

{% hint style="info" %}
**Note:** Suspension does not credit unused quota. When the subscription returns to *Active*, the quota counter resumes from where it stopped, not from zero.
{% endhint %}

{% hint style="success" %}
**Tip:** Suspending mid-period is friendlier to the consumer than revoking. If the dispute resolves, the consumer's integration continues to use the same key they already deployed.
{% endhint %}

#### Reactivate a suspended subscription

Use this task once the underlying issue (payment, rate-limit dispute, governance review) has been resolved.

To reactivate:

1. From the queue, filter to *Suspended* and locate the row.
2. Open the **Actions** menu and click **Reactivate**.
3. A confirmation dialog opens. Optionally fill a **Notes** field; the notes surface in the consumer's notification email.
4. Click **Confirm**. The row's Status returns to *Active* and the gateway resumes accepting calls authenticated by the same key.

{% hint style="success" %}
**Result:** Calls authenticated with the original key succeed again. The quota counter resumes from where it froze at suspension.
{% endhint %}

{% hint style="info" %}
**Note:** Reactivation does not issue a new key; the consumer continues to use the same credential they held before suspension. No re-deployment is required on the consumer side.
{% endhint %}

#### Revoke a subscription

Use this task when the relationship is over and the consumer should no longer be able to call the API. Revocation is final.

#### Before you start

- **Confirm the action is irreversible at the row level.** A revoked subscription cannot be reactivated. The consumer must subscribe again from scratch and you must approve again from scratch.
- **Confirm the consumer has been notified.** A revocation that lands in a consumer's inbox without warning produces escalations to your support team. Coordinate ahead.
- **Capture a clear reason.** The revoke dialog requires a **Reason** field. The reason surfaces in the consumer's notification email and the audit log, and is the explanation the consumer's procurement team will see months later.

{% hint style="warning" %}
**Caution:** Revocation destroys the API key on the gateway. In-flight calls at the exact moment of revoke may complete; every new call returns `401`. The action cannot be undone. Use suspend if there is any chance of resumption.
{% endhint %}

To revoke:

1. From **Manage Product Subscriptions**, locate the row for the active or suspended subscription.
2. Open the **Actions** menu and click **Revoke Access**. A confirmation dialog opens with a **Reason** text area.
3. Fill the **Reason** field with one to three sentences explaining why. Be specific. "Contract end-date reached. Please re-subscribe under the new contract reference if the relationship is renewed." is a better revocation reason than "End of contract."
4. Click **Confirm**. The row's Status moves to *Revoked*, the gateway destroys the key, and the consumer receives a notification email.

The revoke dialog requires a **Reason** text area before it will confirm, and the reason it captures is written to both the consumer's notification email and the audit log.

{% hint style="success" %}
**Result:** The consumer's calls return `401` from the moment the revoke completes. The key is destroyed; the audit log records who acted and why. The row stays on the queue under the *Revoked* filter for audit.
{% endhint %}

{% hint style="info" %}
**Note:** A revoked subscription does not free quota that was already counted in the current period. Quota counters are bound to the subscription instance, not the Product; once a subscription is revoked, its counters are sealed in the audit record.
{% endhint %}

{% hint style="success" %}
**Tip:** Use revoke sparingly. Most consumer disputes resolve faster when the consumer knows they can resume. Suspend buys time; revoke ends the conversation. Reserve revoke for end-of-contract or clear policy breach.
{% endhint %}

#### Verify

1. Confirm the row's Status changes to *Suspended* or *Revoked* immediately after confirming the dialog.
2. Ask the consumer to retry a call and confirm the gateway returns `401`.
3. Open the audit log entry for the action and confirm the **Reason** text is captured against your username.
4. For a suspended subscription, reactivate it in a test environment and confirm calls resume with the same key.

## Watching the consumer's first calls in Provider Analytics

The fastest signal that approval worked is the consumer's first call. It typically appears within minutes and gives you concrete evidence the integration is healthy.

#### Look up an active subscription's traffic

Use this task immediately after approval to confirm the gateway and the marketplace are in sync, and then again every few days for the first week of the relationship.

#### Before you start

- **Open the consumer's subscription detail page.** This is where the per-subscription call counts roll up.
- **Treat timing as indicative, not guaranteed.** Some consumers integrate the same day; some take a fortnight. The first call landing within ten minutes of approval is a good signal that the consumer's integration path is healthy; absence of calls is not necessarily a problem.

To look up traffic:

1. From **Manage Product Subscriptions**, click into the subscription row to open the detail page.
2. The detail page shows the call count for the current period, the all-time call count, the **Last call** timestamp, and a recent-responses panel listing the most recent HTTP response codes.
3. Refresh the page after five minutes. If the count is still zero, that is acceptable; most consumers do not call within five minutes. The relevant signal is the first non-zero value.
4. If the count is non-zero and the recent response codes are `2xx`, the integration is live and healthy. Move on; you will revisit usage in the aggregate Provider Analytics page (covered in the Monitoring usage chapter).
5. If the count is non-zero and the response codes are `401` or `403`, the consumer's key is not being sent correctly. Reach out and ask them to double-check the header name and value.
6. If the response codes are `5xx`, the API itself is failing. Check the gateway logs and your upstream service before assuming the consumer is at fault.

The subscription detail page gathers the current-period call count, the all-time count, the last-call timestamp, and a recent-responses panel of the most recent HTTP status codes into one place.

{% hint style="success" %}
**Result:** You have visibility into first-call success and can identify integration problems while the consumer is still in onboarding, often before they raise a ticket.
{% endhint %}

{% hint style="info" %}
**Note:** Call counts roll up from the gateway on a poll interval typically between one and five minutes. A zero count immediately after approval is normal.
{% endhint %}

{% hint style="success" %}
**Tip:** Bookmark the subscription detail page for the first week of the relationship. Once usage is steady, switch to the aggregate Provider Analytics view, which is covered in the next chapter.
{% endhint %}

#### Cross-reference Provider Analytics

Use this task to confirm a single subscription's traffic against the aggregate Provider Analytics page, especially when investigating a consumer-reported issue.

To cross-reference:

1. Open Provider Analytics at `/admin/portal/analytics`.
2. Filter the analytics view by the consumer's app name or by the Product the subscription is against. The filters at the top of the page accept both.
3. Compare the time-series chart against the subscription detail page's call count for the same time window. The numbers should match within the poll interval.
4. A large divergence (the subscription page shows hundreds of calls, Provider Analytics shows zero) usually means the analytics ingestion is lagging. Wait five minutes and refresh.

{% hint style="info" %}
**Note:** Provider Analytics aggregates across all apps subscribed to a Product. The subscription detail page narrows to one subscription. Reading them together gives you both the macro view and the micro view.
{% endhint %}

{% hint style="success" %}
**Tip:** Filtering Provider Analytics by response code (`401`, `4xx`, `5xx`) surfaces consumers having integration trouble even when they have not raised a ticket. Build a weekly habit of scanning the `4xx` chart.
{% endhint %}

#### Verify

1. Confirm the call counter on the subscription detail page is non-zero within the first day of approval.
2. Confirm the most recent response codes are `2xx`, indicating a healthy round-trip.
3. If response codes are `4xx` or `5xx`, click into the recent-responses panel and reach out to the consumer with the specific error before they raise a ticket.
4. Confirm Provider Analytics shows matching numbers for the same time window when filtered to the same app.

## Communicating with the consumer alongside the platform notifications

The marketplace sends one transactional email per state transition. The transactional email is generic. Most teams supplement with a personal kickoff and use email for anything beyond a state change.

#### Send a kickoff message after approval

Use this task immediately after approving a subscription, to give the consumer a human contact and a clear next step.

#### Before you start

- **Prepare a kickoff template.** A consistent first email (welcome line, key location, documentation link, support address, escalation contact) saves re-typing and signals professionalism. Store the template in your team wiki.
- **Use the email on the consumer's profile.** Click through to the registering user from the subscription row to copy the email address.
- **Set up a shared inbox for consumer onboarding** (for example `developers@<your-domain>`). Personal inboxes lose messages when team members are on leave.

To send a kickoff:

1. From the subscription row, click the **Consumer** column to open the registering user's profile.
2. Copy the registered email address from the profile.
3. Open your mail client and send the kickoff email. Include the Product name, the API key location (the consumer's My Apps page on your portal), the Documentation link, and your support address.
4. Track the conversation in your team's CRM or ticketing system. The marketplace does not store consumer emails or threads.

{% hint style="success" %}
**Result:** The consumer has a real human contact alongside the platform notification, and a clear path to ask follow-up questions.
{% endhint %}

{% hint style="info" %}
**Note:** Approving a subscription automatically queues a notification email to the consumer's primary inbox. The transactional email contains the key location and a Product link; it does not contain a personal message. Send your own kickoff to add one.
{% endhint %}

{% hint style="success" %}
**Tip:** Cc your shared onboarding inbox on the kickoff email. The thread stays in the team's archive even if the original sender moves on.
{% endhint %}

#### Verify

1. Confirm the kickoff email arrived in the consumer's inbox by asking the consumer to acknowledge, or by sending a test message to a colleague's address first.
2. Confirm the email contains the Product name, the API key location, the Documentation link, and the support address.
3. Log the conversation in your CRM or ticketing tool with a link back to the subscription row.

## Next steps

- **Monitoring usage.** Once your first consumer is approved, the next chapter shows how to read the aggregate Provider Analytics dashboard for all incoming calls across all subscriptions.
- **Exposing APIs to AI agents.** When you are ready to widen access beyond human consumers, register an MCP server for the same API so that agent runtimes can discover and call it.
- **Managing your team.** Delegate subscription approvals to a colleague by inviting them to the Organisation with a suitable role, covered in the Managing your team chapter.