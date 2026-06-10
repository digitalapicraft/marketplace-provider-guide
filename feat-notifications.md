---
icon: bullhorn
---

Post an organisation-wide announcement to the in-app inbox of every signed-in user. Use it for planned downtime, a major release, a service disruption, or a deprecation that affects the whole portal audience.

![Figure. The System Notifications list with filters, bulk actions, and the Add notification entry point.](.gitbook/assets/screenshots/provider/admin-notifications-system-list.png)

## Configure

1. Expand **Administration** in the sidebar, then click **System Notifications**.
2. Click **Add notification** in the top-right.
3. Enter a **Title** under 60 characters. It appears in the in-app inbox.
4. Write the body in the message editor. Plain text plus one link reads best.
5. Choose a severity from **Type**: *Info* for routine news, *Warning* for something to plan around, *Critical* for an active incident.
6. Set the **Audience** scope: all signed-in users, only Providers, only Consumers, or one Organisation. Pick the narrowest audience that covers everyone who needs to know.
7. For a time-bounded notice, set a **Start date** and **End date**. Outside that window it hides automatically.
8. Set **Status** to *Published* to go live, or *Draft* to save without publishing, then click **Save**.

{% hint style="success" %}
**Result:** The notification reaches the inbox of every user in the audience. Critical notifications also surface as a banner across the top of every page.
**Tip:** For planned maintenance, post a Warning two weeks ahead and a Critical banner on the day. Archive both once the work is done to keep inboxes clean.
{% endhint %}

### Key controls

1. **Type filter.** Scopes the list by severity (Info, Warning, Critical).
2. **Action selector.** Bulk-publishes, unpublishes, archives, or deletes the rows you tick.
3. **Add notification.** Opens the create form.