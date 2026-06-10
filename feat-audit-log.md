---
icon: clipboard-list
---

The Marketplace Audit Log is the chronological record of every consequential action in the portal: content created and updated, members added, APIs published, and system events. Use it to answer who changed what and when, to investigate an unexpected state change, or to satisfy a compliance review.

![Figure. The Marketplace Audit Log with actor, entity, and operation filters above the entry list.](.gitbook/assets/screenshots/provider/admin-reports-marketplace-audit.png)

## Review

1. From the left sidebar, expand **Administration** and click **Audit Log**.
2. The log loads with the most recent entries first. Each row records the time, the user, the organisation, the entity, the operation, and the source IP.
3. Narrow the list with the **Organisation**, **Entity type**, **Content type**, and **Operation** filters. Combine them to find, for example, every `create` on an API entity within one organisation.
4. Set a **From** and **To** date to scope the log to a window.
5. Click **Filter** to apply, or **Reset** to clear every filter.
6. Click a row to open the entry and see the full detail of the change.
7. Click **Export CSV** in the top-right to download the filtered window for an offline review.

{% hint style="success" %}
**Result:** You have a filtered, time-ordered record of who did what across the portal, exportable for review.
**Tip:** When a system reports an unexpected state, filter to the affected entity and the hour around the incident. The operation and user columns usually point straight at the change that caused it.
{% endhint %}