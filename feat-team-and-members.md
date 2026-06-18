---
icon: user-group
---

Running the marketplace is a team activity. You bring in colleagues to import APIs, review governance, design Products and plans, approve consumers, and watch traffic. Everyone you work with lives inside one **Organisation**, the unit of multi-tenancy on the marketplace. Inside that Organisation, **Members** are the individual users who sign in, and **Teams** group those members into smaller crews so a Payments squad sees Payments APIs and a Cards squad sees Cards APIs. This page covers the Members list, the invite flow, the team surfaces, and the member lifecycle from Pending to Suspended.

![Figure. The Organisation Members list with role, team, status, and last sign-in per row.](.gitbook/assets/screenshots/provider/admin-organisations-members.png)

## What you see

The **Organisation Members** page lists every user linked to your Organisation, their role, their team membership, their status, and their last sign-in. From this one page you invite new members, change a member's role or team, suspend a member, or revoke them. Reach it from the **ORGANISATION** group in the left sidebar by clicking **Members**.

The table carries these columns, left to right:

- **Name.** The member's full name from their profile. Click a name to open the member detail page.
- **Email.** The address notifications are mailed to. The email is the unique identifier across Organisations, so the same person can be a member of more than one Organisation under one email.
- **Role(s).** The roles assigned to the member, rendered as comma-separated badges. A member can hold more than one role; the effective permission set is the union.
- **Teams.** The teams the member belongs to inside the Organisation. Empty when the member is on no team yet.
- **Status.** Pending (invite sent, not accepted), Active (accepted and signed in at least once), or Suspended (sign-in blocked, data preserved).
- **Last sign-in.** The most recent sign-in timestamp. Useful for spotting dormant members before a seat-count audit.
- **Edit / Remove.** Row actions. **Edit** opens the member's detail form where you change role and team; **Remove** revokes the member, with a confirmation prompt.

Three filter dropdowns sit above the table. **Status** narrows to Pending, Active, or Suspended. **Role** lists every built-in and custom role, so filtering by Org Admin is how you find the colleagues who can change settings. **Team** lists every team, the quickest way to see who works on a product line. Pagination sits below the table at 25 rows per page by default, with a per-page selector of 10, 25, 50, or 100. The URL captures every filter and sort selection, so a filtered view can be bookmarked. The empty state reads *No members match the current filters*.

A sibling page, **Organisation Teams** (also under the ORGANISATION group), lists every team with its member, scoped-API, and scoped-Product counts.

## Invite-member form fields

- **Email**: text, required. The work email the invitation is mailed to. Must be a valid address and cannot match an existing member of this Organisation. For a new user, this email becomes their unique identifier across every Organisation.
- **Role**: select, required. The role the colleague holds on first sign-in. Built-in roles appear first, then any custom roles. Defaults to the Organisation's default role when left unset.
- **Also add to team (optional)**: single-select, optional. Lists every team in the Organisation, so the colleague lands with the right scope on first sign-in. Leave blank to add them later.
- **Welcome message**: free text up to 500 characters, optional. Shown in the invitation email above the standard sign-in link. Useful for a line such as *"Hi Sam, this is the marketplace where we publish Payments APIs."*

## Create-team form fields

- **Name**: text, required, max 100 characters. The team's display name in every visibility picker and badge.
- **Description**: text, optional, max 255 characters. A one-line note for the Teams list.
- **Initial members**: multi-select, optional. Lists existing Organisation members so you can populate the team in one shot.

## Invite a colleague to your organisation

1. From the **ORGANISATION** group in the sidebar, click **Members** to open **Organisation Members**.
2. Click **Invite member** at the top right.
3. Enter the colleague's work **Email**. The form rejects an address already in this Organisation.
4. Pick a **Role**. Choose the smallest role that lets the colleague do their job.
5. Optional. Pick a team in **Also add to team** to scope the colleague on first sign-in.
6. Optional. Add a **Welcome message**.
7. Click **Send invitation**. A Pending row appears in the list and the colleague receives an email.

{% hint style="info" %}
**Note:** Inviting a colleague does not create a login from scratch. It links a new or existing account to your Organisation under the role you picked. If your Organisation uses single sign-on, the colleague signs in through your identity provider and the marketplace links the account on first sign-in.
{% endhint %}

{% hint style="success" %}
**Tip:** Send invites in small batches that share the same role and team rather than one large mixed batch. The form takes one email per submission, and homogeneous batches keep the audit log readable.
{% endhint %}

## Resend or revoke a pending invitation

1. From **Organisation Members**, apply the **Status = Pending** filter.
2. Locate the row for the colleague waiting on the invite.
3. Open the row action menu at the right of the row.
4. Click **Resend invitation** to generate a fresh token and dispatch a new email, or **Revoke invitation** to cancel it. Revoke asks you to confirm, then removes the row and invalidates the original token.

{% hint style="info" %}
**Note:** Invitation links expire after seven days and each token is single-use. If a colleague reports a *Token expired* error, or clicked the link on one device while their work account lives on another, resend rather than asking them to reuse the old link.
{% endhint %}

## Create a team and scope resources to it

1. From the **ORGANISATION** group, click **Teams** to open **Organisation Teams**.
2. Click **Add team**.
3. Enter a clear **Name** and a one-line **Description**. Optional. Pick **Initial members**.
4. Click **Save**. The page navigates to the new team's detail page.
5. On the detail page, click **Add members**, pick one or more Organisation members from the dropdown, then **Save**.
6. To scope a resource, open the API or Product, set **Visibility = Org Level**, pick the team in the **Teams** picker that appears, then **Save**.

{% hint style="info" %}
**Note:** Team scoping only applies at **Visibility = Org Level**. Public APIs are visible to everyone on the marketplace and Internal APIs to every member of the Organisation; both ignore team scope.
{% endhint %}

{% hint style="warning" %}
**Caution:** Removing a team from a published API or Product hides it from that team's members on their next page load. Existing subscriptions keep working and the API still responds, but members can no longer find the asset in the catalog or search. Tell affected teams before flipping team scope on a live asset.
{% endhint %}

## Change a member's role or suspend them

1. From **Organisation Members**, locate the member's row and click **Edit**.
2. To change responsibilities, pick a new role in the **Role(s)** multi-select. Hold cmd or ctrl to stack a second role.
3. To adjust scope, add or remove teams in the **Teams** multi-select.
4. To block sign-in without deleting the record, change **Status** to **Suspended** and add a brief **Suspension reason** for the audit log.
5. Click **Save**. The change takes effect on the member's next request.

{% hint style="warning" %}
**Caution:** Suspension does not revoke any API keys the member created. If you suspect credential compromise, also rotate the keys owned by the member from the **Manage Apps** page.
{% endhint %}

## Provision a SAML-authenticated user

SAML-authenticated users do not arrive through the invite form. When your Organisation has single sign-on configured, a user signing in through your identity provider with an email matching a verified domain lands as an **Active** member, not Pending, because the IdP has already authenticated them. The role assigned is the Organisation's **Default organisation role**, unless IdP attribute mapping supplies a role, in which case the IdP-supplied attribute wins. To widen the role afterward, filter Members by **Status = Active**, click **Edit** on the user's row, and pick a wider role in the **Role(s)** multi-select.

{% hint style="success" %}
**Tip:** Set the default to a narrow role such as API Consumer, then widen per user as the colleague's job is confirmed. Widening from Consumer to Provider is low risk; the reverse is harder to communicate. See [Single sign-on (SAML)](feat-single-sign-on.md) for the IdP setup.
{% endhint %}

## Verify

- The invitee shows up in **Organisation Members** with **Status = Pending**; ask them to confirm the invitation email arrived.
- After they accept, the row flips to **Active** with the role and team you set.
- The new team appears in **Organisation Teams**, and its name shows in the **Teams** picker on an API or Product Visibility form.
- Sign in as a team member and confirm Org-Level resources scoped to the team are visible; sign in as a non-member and confirm they are not.
- For a role change, ask the member to refresh and confirm the menu items they expected to gain or lose match the new role.

![Figure. The Organisation Teams list with member, scoped-API, and scoped-Product counts per team.](.gitbook/assets/screenshots/provider/admin-organisations-teams.png)

{% hint style="success" %}
**Tip:** Run a monthly Members review. Sort by **Last sign-in** ascending to surface dormant accounts, filter **Status = Pending** to find unaccepted invites, and filter **Role = Org Admin** to confirm only the right people hold admin. Suspend dormant accounts rather than removing them; a suspended member reactivates in one click, a revoked one must be re-invited.
{% endhint %}

{% hint style="success" %}
**Result:** Colleagues land with the right role and team on first sign-in, and each team sees only the APIs and Products scoped to it.
{% endhint %}

## Related

- [Roles & permissions](feat-roles-and-permissions.md) for the four built-in roles and the custom-role builder.
- [Single sign-on (SAML)](feat-single-sign-on.md) for federating sign-in against a corporate identity provider.
- [Branding & theming](feat-branding-and-theming.md) for per-Organisation storefront branding.
- [Custom domains](feat-custom-domains.md) for pointing a brand-owned domain at the storefront.