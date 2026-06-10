---
icon: clipboard-list
---

The Marketplace Audit Log is the chronological record of every consequential action in the portal: content created and updated, members added, APIs published, and system events. Use it to answer who changed what and when, to investigate an unexpected state change, or to satisfy a compliance review. It is read-only and exportable, so it doubles as the evidence trail behind any review.

![Figure. The Marketplace Audit Log with actor, entity, and operation filters above the entry list.](.gitbook/assets/screenshots/provider/admin-reports-marketplace-audit.png)

## What each entry records

Every row captures one action across six columns:

- **Time**: the timestamp of the action. The list loads most-recent-first.
- **User**: the actor who performed the action.
- **Organisation**: the Organisation the actor and the affected entity belong to.
- **Entity**: the object that changed, such as an API, a Page, or a member.
- **Operation**: the action taken, tagged **create** or **update** so you can scan for a class of change at a glance.
- **Source IP**: the address the request came from, useful when confirming where an action originated.

## Filters

Narrow the log with the controls above the list, then combine them to isolate one actor or action:

- **Organisation**: scope to actions within one Organisation.
- **Entity type**: scope to one kind of object, such as an API or a member.
- **Content type**: scope to one content type for content-related changes.
- **Operation**: scope to **create** or **update**.
- **From / To dates**: bound the log to a window, useful for a specific incident or a review period.

After setting filters, click **Filter** to apply them or **Reset** to clear them and return to the full log.

## Review

1. From the left sidebar, expand **Administration** and click **Audit Log**.
2. The log loads with the most recent entries first. Each row records the time, the user, the organisation, the entity, the operation, and the source IP.
3. Narrow the list with the **Organisation**, **Entity type**, **Content type**, and **Operation** filters. Combine them to find, for example, every `create` on an API entity within one organisation.
4. Set a **From** and **To** date to scope the log to a window.
5. Click **Filter** to apply, or **Reset** to clear every filter.
6. Click a row to open the entry and see the full detail of the change.
7. Click **Export CSV** in the top-right to download the filtered window for an offline review.

## Verify

- After running a filter, confirm every row in the list matches the values you set (one Organisation, one operation, the chosen date window).
- Open a single entry and confirm the detail view names the actor, the entity, and the change.
- Click **Export CSV** and confirm the downloaded file contains the same filtered rows shown on screen.

{% hint style="success" %}
**Tip:** When a system reports an unexpected state, filter to the affected entity and the hour around the incident. The operation and user columns usually point straight at the change that caused it.
**Result:** You have a filtered, time-ordered record of who did what across the portal, exportable for review.
{% endhint %}