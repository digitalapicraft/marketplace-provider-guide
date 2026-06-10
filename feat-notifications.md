---
icon: bullhorn
---

System notifications are the marketplace's broadcast channel. Post one when every signed-in user (Providers, Org Admins, Consumers) needs to see a message regardless of which page they are on: planned downtime, a major release, a service disruption, or a deprecation that affects the whole portal audience. They complement webhooks and email by reaching users inside the app, even when they missed the email.

![Figure. The System Notifications list with filters, bulk actions, and the Add notification entry point.](.gitbook/assets/screenshots/provider/admin-notifications-system-list.png)

## What you configure

The Add notification form holds a handful of fields that control what the message says, who sees it, and when:

- **Title**: the headline shown in the in-app inbox. Keep it under 60 characters; a long title gets truncated.
- **Body**: the message shown on the notification detail view. Plain text plus a single link reads best for skim-reading.
- **Type**: the severity. *Info* for routine announcements, *Warning* for something to plan around (planned maintenance, a breaking change in 30 days), *Critical* for an active incident.
- **Audience**: the scope. All signed-in users, only Providers, only Consumers, or one Organisation. Defaults to all signed-in users. Pick the narrowest audience that covers everyone who needs to know.
- **Start date / End date**: an optional time window. Outside it, the notification is hidden automatically.
- **Status**: *Published* to go live, or *Draft* to save without publishing.

## Configure

1. Expand **Administration** in the sidebar, then click **System Notifications**.
2. Click **Add notification** in the top-right.
3. Enter a **Title** under 60 characters.
4. Write the **Body** in the message editor.
5. Choose a severity from **Type**: Info, Warning, or Critical.
6. Set the **Audience** scope to the narrowest group that covers everyone who needs the message.
7. For a time-bounded notice, set a **Start date** and an **End date**.
8. Set **Status** to *Published* to go live, or *Draft* to hold it, then click **Save**.

To edit later, find the notification by title or filter by **Type** and **Status**, open it, click **Edit**, adjust, and save. Changes apply to the inbox immediately. To retire several at once, tick the rows, choose **Archive** from the **Action** selector, and click **Apply to selected items**.

## Options

- **Severity**: Info, Warning, Critical. Critical overrides per-user notification preferences and always appears in the inbox, so reserve it for live incidents.
- **Audience scope**: all signed-in users, Providers only, Consumers only, or a single Organisation.
- **Bulk action**: Publish, Unpublish, Archive, or Delete the ticked rows.

## Verify

- Sign in as a member of the targeted audience and confirm the notification appears in the in-app inbox.
- For Critical severity, confirm the banner shows across the top of every page until dismissed.
- Confirm the row in **System Notifications** shows status *Published* and the scheduled window you set.

{% hint style="success" %}
**Tip:** For planned maintenance, post a Warning two weeks ahead and a Critical banner on the day of the work. Archive both once it is complete to keep inboxes clean.
{% endhint %}

{% hint style="success" %}
**Result:** The notification reaches the inbox of every user in the audience, and Critical notifications also surface as a top-of-page banner.
{% endhint %}