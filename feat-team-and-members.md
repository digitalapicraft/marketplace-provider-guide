---
icon: user-group
---

Members are the individual users who sign in and act on behalf of your Organisation: importing APIs, reviewing governance, designing Products, approving consumers, and watching traffic. Teams group those members into smaller crews so each product line sees only its own Org-Level APIs and Products. Use this surface to invite colleagues, set their role on the way in, and scope what each crew sees.

![Figure. The Organisation Members list with role, team, status, and last sign-in per row.](.gitbook/assets/screenshots/provider/admin-organisations-members.png)

## What you configure

The Members list shows every user linked to your Organisation; the invite form and the team pages are where you set them up. The fields you fill in are:

- **Email.** Required, the work address the invitation is mailed to. Must be a valid address and cannot match an existing member of this Organisation. For a new user, the same email becomes their unique identifier across every Organisation.
- **Role.** Required, the role the colleague holds on first sign-in. Built-in roles appear first, then any custom roles. Defaults to the Organisation's default role when left unset. Pick the smallest role that fits the job.
- **Also add to team.** Optional single-select of every team in the Organisation, so the colleague lands with the right scope on first sign-in. Leave blank to add them later.
- **Welcome message.** Optional free text up to 500 characters, shown in the invitation email above the standard sign-in link.
- **Team name and description.** When you create a team, a name (max 100 characters) used in every visibility picker, plus an optional one-line description (max 255 characters) for the Teams list. An **Initial members** multi-select populates the team in one shot.

Each member row carries a **Status** of Pending (invite sent, not accepted), Active (accepted and signed in at least once), or Suspended (sign-in blocked, data preserved), plus their roles, teams, and last sign-in.

## Configure

1. Expand the **ORGANISATION** group in the sidebar, then click **Members** to open the **Organisation Members** list.
2. Click **Invite member** at the top right.
3. Enter the colleague's work **Email**. The form rejects an address already in this Organisation.
4. Pick a **Role**, optionally pick a team in **Also add to team**, and optionally add a **Welcome message**.
5. Click **Send invitation**. A Pending row appears in the list.
6. To create a Team, click **Teams** in the ORGANISATION group, click **Add team**, enter a **Name** and **Description**, optionally pick **Initial members**, then **Save**.
7. On the team detail page, click **Add members**, pick Organisation members from the dropdown, and **Save**.
8. To scope a resource, open an API or Product, set **Visibility = Org Level**, pick the team in the **Teams** picker that appears, and **Save**.

## Verify

- The invitee shows up in **Organisation Members** with **Status = Pending**, and ask them to confirm the invitation email arrived.
- After they accept, the row flips to **Active** with the role and team you set.
- The new team appears in **Organisation Teams**, and its name shows in the **Teams** picker on an API or Product Visibility form.
- Sign in as a team member and confirm Org-Level resources scoped to the team are visible; sign in as a non-member and confirm they are not.

## Options

- **Member states.** Pending, Active, and Suspended. Suspend blocks sign-in while keeping the record; revoke removes the member entirely and releases the seat.
- **SAML-provisioned users.** Do not use the invite form. On first sign-in through your identity provider, with a matching verified domain, they land as **Active** with the default role, with no Pending state.
- **Team membership.** Additive: a member sees the union of every team's scoped resources, and can belong to more than one team. Scoping only applies at **Org Level**; Public APIs are visible to everyone and Internal APIs to every Org member, both ignoring team scope.

![Figure. The Organisation Teams list with member, scoped-API, and scoped-Product counts per team.](.gitbook/assets/screenshots/provider/admin-organisations-teams.png)

{% hint style="success" %}
**Tip:** Invite in small batches that share the same role and team so the audit log stays readable.
{% endhint %}

{% hint style="success" %}
**Result:** Colleagues land with the right role and team on first sign-in, and each team sees only the APIs and Products scoped to it.
{% endhint %}