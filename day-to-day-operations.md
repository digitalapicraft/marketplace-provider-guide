---
icon: briefcase
---

Daily operations on the marketplace cover content updates (pages and articles), system-wide announcements, governance rule tuning, webhook delivery monitoring, and email-template customisation. This chapter walks through each surface end to end. Most of these tasks live under **SETTINGS** or **ADMINISTRATION** in the left sidebar, the two grey section headers below the API-management links. None of them require code or developer support. An Org Admin or Portal Admin completes every task in this chapter from the browser.

You will learn:

- How to edit, add, and replace the standing pages and media that make up your storefront.
- How to post a system notification that reaches every signed-in user, scoped to severity and audience.
- How to tune governance linting rules and override the ruleset for a single Organisation.
- How to register webhooks, monitor their delivery logs, and customise email templates.
- How to review moderated content, schedule a deprecation notice, and keep URL aliases and redirects clean.
- How to control domain registration and read your personal notifications inbox.

Allow ~90 minutes to walk through this chapter end to end. Individual tasks take 5 to 15 minutes each.

![Figure 13-0. The Configuration index where most operational surfaces in this chapter are grouped.](.gitbook/assets/screenshots/provider/admin-config.png)

The Configuration index in Figure 13-0 organises every operational surface under named groups: **People**, **Content authoring**, **Search and metadata**, **System**, **Web services**, and **Development**. Each link in this chapter starts from one of those groups, so keep this page bookmarked while you work through the tasks below.

## Managing static pages and content

Your storefront ships with a small set of standing pages: About, Help, Privacy, Terms, and a homepage. These are the pages a consumer sees before signing in, and the pages that hold any policy or onboarding copy you publish. The content surfaces sit under **Content** in the left sidebar.

The **Content** overview is the single list of every content item in the portal, across Pages, CMS items, and Media. Open it from **Administration** > **CMS** when you want to find an item by title regardless of which type it belongs to.

![Figure 13-19. The Content overview listing every content item with Title, Content type, Author, Status, and Updated columns.](.gitbook/assets/screenshots/provider/admin-content.png)

The numbered callouts in Figure 13-19 are:

1. **Content** heading. The page heading. The **Overview** and **Scheduled content** tabs below switch between the full list and the queue of future state changes.
2. **Title / Type / Published status** filters. Combine them to find, for example, every Documentation item that is still in Draft.
3. **Content type** column. Tells you whether a row is a Page, a Documentation item, a Guide, or another type.
4. **Status** column. Whether the item is Published or unpublished.
5. **Updated** column. Sortable column showing when the item was last saved. Sort descending to see today's edits first.
6. **Operations** menu. Per-row menu where you open the item for **Edit** or view its revisions.

#### Edit the public storefront pages

Edit a storefront page when copy is out of date, when legal asks for a Privacy or Terms revision, or when you want to revise the homepage hero before a launch.

#### Before you start

- **Decide who reviews the change.** Pages publish immediately on save. If you need a second pair of eyes, draft the change first and only flip the publish state once review is done.
- **Have the new copy ready in plain text.** Pasting from a word processor often drags styling that breaks the page layout. Paste into a plain-text editor first, then into the page body.
- **Know the page's URL alias.** If you change the alias, update any internal links that point at the old path.

To edit a page:

1. From the left sidebar, expand **Content** and click **Pages**.
2. The **Pages** list loads. Each row is one page with its title, status, author, and last-changed timestamp.
3. Use the **Title** filter at the top of the list to find the page you want, or sort by **Updated** to find the most recently edited one.
4. Click the page title to open it, then click **Edit**.
5. Update the copy in the body editor.
6. To change the publish state, toggle the **Status** field between **Published** and **Unpublished**.
7. Click **Save**.

![Figure 13-1. The Pages list showing every standing page with status, author, and last-changed columns.](.gitbook/assets/screenshots/provider/admin-content-pages.png)

The numbered callouts in Figure 13-1 are:

1. **Pages** title. The page heading. Each row below is one storefront page.
2. **Title** filter. Free-text filter that scopes the list to pages whose title contains your text.
3. **Status** filter. Scopes the list to **Published** or **Unpublished** rows.
4. **Add page** button. Top-right button that opens the new-page editor covered in the next task.
5. **Operations** menu. Per-row menu where you open the page for **Edit**, view its revision log, or remove it.

{% hint style="success" %}
**Result:** The page is updated. If status is **Published**, the new copy is live for every visitor on the next page load.
{% endhint %}

{% hint style="info" %}
**Note:** The Pages list includes both published and unpublished entries. Use the **Status** filter at the top of the list to scope to one or the other.
{% endhint %}

{% hint style="success" %}
**Tip:** Before saving a major copy change to a high-traffic page (the homepage, Pricing, Privacy), set the status to **Unpublished**, save, preview the page logged in as an admin, then re-publish once you are satisfied.
{% endhint %}

Verify:

1. Reload the page in a private browser window and confirm the new copy renders.
2. Confirm the **Status** column on the Pages list reflects the value you saved.
3. Test any links or images in the body to confirm they resolve to the right destinations.

#### Add a new page

Add a new page when you need a new standing surface: a launch announcement, a partner-program landing page, a new policy document, or a help topic that does not fit the existing structure.

#### Before you start

- **Decide on a URL alias.** Use a short, descriptive slug like `/partner-programme` rather than `/page-12`. The alias becomes the public URL.
- **Decide whether the page goes in the main menu.** A page can exist without being linked from any menu. That is useful for landing pages you only link from email campaigns.
- **Know the parent page if the page belongs in a hierarchy.** Help and documentation pages typically nest under a parent.

To add a page:

1. From the left sidebar, expand **Content** and click **Pages**.
2. Click **Add page** in the top-right of the **Pages** list.
3. The page editor opens. Enter the page title in the **Title** field.
4. Enter a URL alias in the **URL alias** field (for example `/partner-programme`).
5. Enter a short summary in the **Description** field. This is the meta description used by search engines and in some link previews.
6. Build the body using the page editor on the left of the canvas.
7. Open the **SEO settings** panel from the top toolbar to set the SEO title and meta description if they should differ from the page values.
8. Click **Preview** in the top-right to see the page rendered as a visitor would see it.
9. When you are happy with the result, save and publish from the top-right action menu.

![Figure 13-2. The Add page editor with title, URL alias, description, and the visual block builder.](.gitbook/assets/screenshots/provider/admin-content-pages-add.png)

The numbered callouts in Figure 13-2 are:

1. **Title** field. The page heading visitors see at the top of the rendered page and on browser tabs.
2. **URL alias** field. The public path under which the page is served. Use a short, descriptive slug like `/partner-programme`.
3. **Description** field. Short summary used as the meta description for search engines and link previews.
4. **Body** area. The visual block area where you compose the page. Drag blocks (heading, text, image, callout) onto the canvas.
5. **SEO settings** panel. Side panel for SEO title and meta description if they should differ from the page values.
6. **Save / Preview** actions. Top-right actions. **Preview** renders the page as a visitor would see it; **Save** commits your changes.

{% hint style="success" %}
**Result:** The new page is live at the alias you entered. If you added it to a menu, the link appears in the navigation on the next page load.
{% endhint %}

{% hint style="info" %}
**Note:** The page editor is a visual builder. Each block (heading, body, image, callout, columns) is dragged onto the canvas from the block panel on the left. You can rearrange blocks at any time without losing content.
{% endhint %}

{% hint style="success" %}
**Tip:** Save an unpublished version first, share the preview URL with a reviewer, and only publish once they approve. This is faster than building, publishing, and rolling back if something is wrong.
{% endhint %}

Verify:

1. Open the new alias in a private browser window and confirm the page renders.
2. Confirm the page appears in the Pages list with the expected status.
3. If you added the page to a menu, confirm the link appears in the navigation.

#### Manage media (images, files)

The media library is where every image, PDF, video, and downloadable file the portal serves is stored. Pages reference media items rather than holding the binary directly. That means you can replace an image once and every page that references it picks up the new version.

To upload a new media item:

1. From the left sidebar, expand **Content** and click **Media**.
2. The **Media** list loads. Use the **Type** filter to scope to images, documents, video, or audio. Use the **Status** filter to scope to published or archived items.
3. Click **Add media** to upload a single file, or **Bulk upload** to drop in many files at once.
4. Drag the file onto the upload target, or click to choose a file from your computer.
5. Enter a **Name** for the media item. The name is the label you and your team see in the library. Visitors only see the file itself.
6. Add alt text for images. This is the description screen readers announce and what search engines index.
7. Click **Save**.

To replace an existing media item without breaking inbound references:

1. Open the **Media** list and find the item by name.
2. Click the row's edit link.
3. In the file field, click **Remove**, then upload the new file.
4. Click **Save**.

![Figure 13-3. The Media library list view with type, status, and used-in columns.](.gitbook/assets/screenshots/provider/admin-content-media.png)

The numbered callouts in Figure 13-3 are:

1. **Media** heading. The page heading. Each row is one media item: an image, document, video, or audio file.
2. **Name** filter. Free-text filter that scopes the list to items whose name contains your text.
3. **Type** filter. Scopes the list to images, documents, video, or audio.
4. **Status** filter. Scopes the list to published or archived items.
5. **Add media** button. Top-right action that opens the single-file upload form.
6. **Bulk upload** action. Drop-zone action for adding many files in one pass.

For a tile-style view that shows thumbnails alongside metadata, switch to the grid view shown in Figure 13-4.

![Figure 13-4. The Media library grid view, useful when you want thumbnails of every image at a glance.](.gitbook/assets/screenshots/provider/admin-content-media-grid.png)

The numbered callouts in Figure 13-4 are:

1. **Grid view toggle**. Switches the default list into a thumbnail grid. The same filters apply.
2. **Thumbnail tile**. Each tile shows the media preview, the item name, and a quick-edit menu.
3. **Edit** action. Opens the media item editor where you replace the file or update alt text without changing the URL.

{% hint style="success" %}
**Result:** Every page that references the media item now serves the new file on the next page load. The URL stays the same.
{% endhint %}

{% hint style="warning" %}
**Caution:** Deleting a media item breaks every reference to it. If a page used the deleted item, that area of the page renders empty. Always replace before deleting, or check the **Used in** column before removing anything.
{% endhint %}

Verify:

1. Confirm the new media item appears in the **Media** list with the correct name, type, and alt text.
2. Reference the item from a test page and confirm it renders with the expected URL.
3. After a replacement, confirm every page in the **Used in** column now serves the replacement file.

#### Configure taxonomy (categories, tags)

Taxonomy is how you label content for filtering and discovery. Every category and tag a consumer sees on the API catalogue, the Documentation library, the Guides library, and the Blog comes from a taxonomy vocabulary. The marketplace ships with seven vocabularies you can edit:

- **Api Environment**. Production, Sandbox, and Staging labels for an API listing.
- **Api Filters**. Top-level discovery facets shown on the public catalogue.
- **Api Tags**. Free-form tags attached to APIs and API Products.
- **Blog Category**. Category labels for blog articles.
- **Doc Category**. Category labels for Documentation pages.
- **Guide Category**. Category labels for Guides.
- **Tags**. Site-wide tags shared across content types.

To add a new term to a vocabulary:

1. From the left sidebar, expand **Structure** and click **Taxonomy**.
2. The **Taxonomy** list shows the seven vocabularies. Find the one you want to extend.
3. Click **List terms** on that row to see existing terms, or click **Add term** to jump straight to the term editor.
4. Enter a **Name** for the term. This is the label consumers see.
5. Enter a short **Description**. The description appears as hover text on some surfaces.
6. Click **Save**.

![Figure 13-17. The Taxonomy management page listing every vocabulary with its description and a List terms action per row.](.gitbook/assets/screenshots/provider/admin-structure-taxonomy.png)

The numbered callouts in Figure 13-17 are:

1. **Taxonomy** heading. The page heading. Each row below is one vocabulary the marketplace ships with.
2. **Vocabulary name** column. The name of the vocabulary (for example **API Filters** or **Blog category**). This is the label your team works with, not the consumer-facing term.
3. **Description** column. A short description of what the vocabulary labels and where its terms appear.
4. **Operations** menu. Per-row menu. **List terms** opens the term list for that vocabulary; the dropdown adds a term or edits the vocabulary itself.

{% hint style="success" %}
**Result:** The new term is available immediately. When a Provider tags an API or Product with the term, the consumer-facing filter list picks it up on the next refresh.
{% endhint %}

{% hint style="success" %}
**Tip:** Keep vocabularies tight. A discovery filter list with 40 categories is harder to use than a list with 8. Audit each vocabulary every quarter and fold rarely-used terms into broader ones.
{% endhint %}

#### Manage navigation menus

The marketplace builds its sidebars, footers, and section navigation from named menus. Edit a menu when you want to add a link to a new page, reorder items, or hide a link without redeploying code.

To edit a menu:

1. From the left sidebar, expand **Structure** and click **Menus**.
2. The **Menus** list shows every menu the portal ships with: the **Main navigation**, the **Footer** and **Legal links**, the **Org Admin Menu** and **Portal Admin Menu** sidebars, and the **Social links** row.
3. Click **Edit menu** on the row you want to change.
4. On the menu editor, drag rows to reorder them, toggle a row's **Enabled** checkbox to show or hide it, or click **Add link** to add a new entry.
5. Click **Save**.

![Figure 13-18. The Menus management page listing every navigation menu with its description and an Edit menu action per row.](.gitbook/assets/screenshots/provider/admin-structure-menu.png)

The numbered callouts in Figure 13-18 are:

1. **Menus** heading. The page heading. Each row is one navigation menu.
2. **Title** column. The menu name (for example **Main navigation**, **Portal Admin Menu**, or **Footer**).
3. **Description** column. A short note on where the menu renders and what it controls.
4. **Operations** menu. Per-row menu. **Edit menu** opens the link editor where you add, reorder, and disable items.
5. **+ Add menu** button. Top-right action that creates a new menu when you need navigation the shipped set does not cover.

{% hint style="success" %}
**Result:** The reordered or toggled menu renders its new shape on the next page load. No deployment is needed.
{% endhint %}

{% hint style="warning" %}
**Caution:** Disabling a link in the **Portal Admin Menu** or **Org Admin Menu** hides the surface from everyone who relies on that sidebar, not just yourself. Confirm no one needs the link before you hide it.
{% endhint %}

## Posting system-wide announcements

System notifications are the marketplace's broadcast channel. Use them when you need every signed-in user (Providers, Org Admins, Consumers) to see a message regardless of which page they are on.

#### Post a system notification

Post a system notification when you need to announce planned downtime, a major feature release, a service disruption, a deprecation, or anything else that affects the whole portal audience.

#### Before you start

- **Decide on the severity.** **Info** is for routine announcements. **Warning** is for something users should plan around (planned maintenance, breaking change in 30 days). **Critical** is for active incidents.
- **Decide on the audience.** A notification can target every signed-in user, only Providers, only Consumers, or only members of a specific Organisation. Pick the narrowest audience that covers everyone who needs to know.
- **Draft the message and a short title.** The title appears in the in-app inbox. The body appears on the notification detail view. Keep both short. A long title gets truncated.
- **Decide whether the notification has a start and end date.** A planned-maintenance notification should appear two weeks before the work and disappear once the work is complete.

To post a system notification:

1. From the left sidebar, expand **Administration** and click **System Notifications**.
2. The **System Notifications** list loads. Each row is one notification with its title, type, status, and scheduled date.
3. Click **Add notification** in the top-right.
4. Enter the title in the **Title** field. Keep it under 60 characters.
5. Enter the body in the message editor. Plain text plus a single link works best for skim-reading.
6. Choose a severity from the **Type** dropdown: **Info**, **Warning**, or **Critical**.
7. Choose an audience from the **Audience** scope control. Defaults to all signed-in users.
8. If the notification is time-bounded, set a **Start date** and an **End date**. Outside that window the notification is hidden automatically.
9. Set the **Status** to **Published** when you are ready for the notification to go live, or **Draft** to save without publishing.
10. Click **Save**.

![Figure 13-5. The System Notifications list with filters, bulk actions, and the Add notification entry point.](.gitbook/assets/screenshots/provider/admin-notifications-system-list.png)

The numbered callouts in Figure 13-5 are:

1. **System Notifications** heading. The page heading. Each row in the list below is one notification.
2. **Keywords** filter. Free-text search across notification titles and bodies.
3. **Type** filter. Filter by severity (Info, Warning, Critical).
4. **Status** filter. Filter by publication state (Published, Draft, Archived).
5. **Scheduled** filter. Shows only notifications with a future start date.
6. **Action** selector. Bulk action selector for the rows you tick: Publish, Unpublish, Archive, or Delete.
7. **Add notification** button. Opens the create form covered in step 3 above.

{% hint style="success" %}
**Result:** The notification appears in the in-app inbox of every user in the audience. Critical notifications also surface as a banner across the top of every page.
{% endhint %}

{% hint style="info" %}
**Note:** Critical notifications override per-user notification preferences and always appear in the in-app inbox. Use this severity sparingly. A noisy banner trains users to ignore the channel.
{% endhint %}

{% hint style="success" %}
**Tip:** For planned maintenance, post a Warning two weeks ahead and a Critical with a one-hour banner on the day of the work. Archive both once the work is complete to keep the inbox clean.
{% endhint %}

Verify:

1. Sign in as a member of the targeted audience and confirm the notification appears in the in-app inbox.
2. For Critical severity, confirm the banner appears across the top of every page until dismissed.
3. Confirm the row in **System Notifications** shows status **Published** and the right scheduled window.

#### Edit or archive an existing notification

Edit a notification when the maintenance window slips, when copy needs tightening after legal review, or when an incident severity escalates from Warning to Critical mid-flight. Archive a notification once the event it announced has passed so it stops cluttering the inbox.

To edit a notification:

1. Open **Administration** > **System Notifications**.
2. Find the notification by title or filter the list by **Type** and **Status**.
3. Click the title to open the detail view, then click **Edit**.
4. Adjust the title, body, severity, audience, or schedule as needed.
5. Click **Save**. The change applies to the in-app inbox immediately.

To archive in bulk:

1. Tick the checkbox on every row you want to archive.
2. From the **Action** dropdown at the top of the list, choose **Archive**.
3. Click **Apply to selected items**.

{% hint style="success" %}
**Tip:** When you escalate a Warning to Critical, keep the original Warning notification archived rather than deleted. The audit trail of how an incident communicated is useful in a post-incident review.
{% endhint %}

## Tuning API governance

The Provider Analytics chapter showed the output of governance: the score every API gets and the breakdown of failed rules. The page covered here is where the rules themselves live. Tune these rules when your team's API style guide changes, when a rule is generating false positives, or when a new compliance requirement arrives.

#### Open the linting rules configuration

Open the linting rules surface when you want to see which rules are enforced, change a rule's severity, disable a rule, or set a different publish threshold.

To open the linting rules:

1. From the left sidebar, expand **SETTINGS** and click **API Governance Settings**.
2. The **API Governance Settings** page loads. The page is split into two areas: the score-deduction controls at the top, and three tabs for the rule sets (**Security**, **Style**, **Documentation**).

![Figure 13-6. The API Governance Settings page with score deductions, publish threshold, and rule-set tabs.](.gitbook/assets/screenshots/provider/admin-config-apim-api-linting.png)

The numbered callouts in Figure 13-6 are:

1. **API Governance Settings** heading. The page heading.
2. **Error deduction** field. How many points an Error-severity rule violation removes from an API's governance score. Defaults to a high number so a single Error has visible impact.
3. **Warning deduction** field. How many points a Warning-severity violation removes. Lower than Error.
4. **Info deduction** field. How many points an Info-severity violation removes. The smallest of the three impacts on score.
5. **Hint deduction** field. How many points a Hint-severity violation removes. The smallest score impact, used for stylistic suggestions.
6. **Publish threshold** slider. A slider from 0 to 100. APIs scoring below this threshold cannot be published until a Provider corrects the violations or an admin overrides the gate.
7. **Security / Style / Documentation tabs**. Each tab holds a list of rules belonging to that pillar. Click a tab to see the rules in that group.

#### Enable, disable, or adjust the severity of a rule

Enable, disable, or re-classify a rule when the default severity does not match your team's style guide, when a rule is producing too many false positives, or when you want a rule that is off by default to start enforcing.

#### Before you start

- **Know which pillar the rule belongs to.** Security rules cover authentication, transport, and data exposure. Style rules cover naming, casing, and response shape. Documentation rules cover descriptions, examples, and summaries.
- **Talk to the team before disabling a rule.** A disabled rule stays off until someone re-enables it. Make sure everyone agrees on the trade-off.

To adjust a rule:

1. From the left sidebar, expand **SETTINGS** and click **API Governance Settings**.
2. Click the tab for the pillar the rule belongs to (**Security**, **Style**, or **Documentation**).
3. Find the rule in the list. Each row shows the rule name, the current severity, and an enable/disable toggle.
4. To change the severity, click the severity dropdown on that row and choose **Error**, **Warning**, **Info**, or **Hint**.
5. To turn the rule off, flip the **Enabled** toggle to off.
6. Click **Save** at the bottom of the page.

{% hint style="success" %}
**Result:** The change applies to every governance scan that runs after you save. Existing scan results are not retroactively re-scored. Re-scan an API to see the new outcome.
{% endhint %}

{% hint style="success" %}
**Tip:** When you tighten a rule (move from Warning to Error, or enable a rule that was off), re-run the governance scan on every published API the next morning. You will see which APIs newly fail the gate and need attention.
{% endhint %}

Verify:

1. Reload the page and confirm the rule's severity and Enabled state hold the values you set.
2. Re-scan a known API and confirm the new severity is reflected in the score.
3. Confirm any API whose score now falls below the **Publish threshold** is gated from publishing.

#### Create a custom ruleset for one Organisation

A default ruleset applies to every Organisation in the marketplace. An Org-scoped override applies a different ruleset only to APIs owned by that Organisation. This is useful when one Organisation has an exemption (a legacy API, a partner-imposed schema), or when one Organisation runs a tighter standard than the rest of the portal.

To override the ruleset for one Organisation:

1. Open the Organisation's settings page from the **Organisations** list under **ORGANISATION** in the left sidebar.
2. Open the **Governance overrides** section.
3. Toggle on **Use custom ruleset**.
4. Adjust severity and enabled state for any rule you want to differ from the portal default.
5. Click **Save**.

{% hint style="success" %}
**Result:** APIs owned by that Organisation are now scored against the custom ruleset. APIs owned by every other Organisation continue to score against the portal default.
{% endhint %}

{% hint style="info" %}
**Note:** Override only what you need to. Every rule you do not override falls through to the portal default. Future portal-default tightening still applies to that Organisation for the un-overridden rules.
{% endhint %}

Verify:

1. Re-scan an API owned by the overriding Organisation and confirm the score reflects the custom ruleset.
2. Re-scan an API owned by another Organisation and confirm the score is unchanged.
3. Toggle off **Use custom ruleset** and confirm the next scan reverts to the portal default.

## Configuring webhook deliveries

Webhooks are the marketplace's outbound notification channel for systems other than email. When something interesting happens (a Subscription is created, an API is published, a governance scan completes), the marketplace POSTs a JSON payload to a URL you control. Use webhooks to drive Slack notifications, ticketing-system tickets, internal dashboards, or downstream automations.

#### Register a webhook endpoint

Register a webhook when you want a system other than the marketplace to react to a marketplace event in near-real-time.

#### Before you start

- **Have the receiver URL.** This is the URL the marketplace POSTs to. The receiver must accept JSON, return a 2xx within 10 seconds, and be reachable from the marketplace's egress IP range.
- **Have a shared secret.** The marketplace signs every payload with a header derived from this secret. The receiver verifies the signature before acting on the payload. Generate a long random string and do not reuse a secret across webhooks.
- **Decide which events to subscribe to.** Subscribe only to the events the receiver actually needs. Common subscriptions are Subscription Created, Subscription Approved, API Published, Governance Scan Complete, and Member Invited.

To register a webhook:

1. From the left sidebar, expand **SETTINGS** and click **Webhook**.
2. The **Webhook** list loads. Each row is one webhook with its name, target URL, status, and last-delivery timestamp.
3. Click **Add webhook** in the top-right.
4. Enter a **Name** that will help you find this webhook later, for example "Slack #api-ops".
5. Enter the receiver URL in the **URL** field. Use HTTPS in production.
6. Paste the shared secret into the **Secret** field. The marketplace uses it to sign every payload. Treat it like a password.
7. Tick the events to subscribe to in the **Events** list.
8. Set **Status** to **Active**.
9. Click **Save**.
10. Trigger a test event from the row's **Send test** action and check that the receiver got the payload.

![Figure 13-7. The Webhook configuration list with name, URL, status, and last-delivery columns.](.gitbook/assets/screenshots/provider/admin-config-services-webhook.png)

The numbered callouts in Figure 13-7 are:

1. **Webhook** heading. The page heading. Each row is one registered receiver.
2. **Name** column. The internal label you gave the webhook (for example "Slack #api-ops").
3. **URL** column. The receiver endpoint the marketplace POSTs to. Use HTTPS in production.
4. **Status** column. **Active** receivers get every event they subscribe to. **Inactive** receivers are skipped.
5. **Add webhook** button. Top-right action that opens the registration form covered in step 3 above.
6. **Operations** menu. Per-row menu where you open the **Deliveries** tab, send a test event, edit, or remove the webhook.

<details>

<summary><strong>All fields on the Add webhook form</strong></summary>

| Field | Type | Required | What to enter |
|---|---|---|---|
| Name | Text | Yes | Internal label that helps the team identify the webhook in the list. |
| URL | URL | Yes | Receiver endpoint. Use HTTPS in production. Must accept JSON and respond within 10 seconds. |
| Secret | Text | Yes | Long random string used to sign every payload. Store it in the receiver and verify on inbound. |
| Events | Multi-select | Yes | One or more event names. Common picks are Subscription Created, Subscription Approved, API Published, Governance Scan Complete, Member Invited. |
| Status | Select | Yes | **Active** to enable delivery, **Inactive** to register without firing. |
| Description | Textarea | No | Free text describing what this receiver does. Useful when other admins audit the list. |
| Headers | Key/value | No | Extra HTTP headers (for example a static `X-Source: marketplace` tag) appended to every delivery. |

</details>

![Figure 13-20. The Add webhook form with the Label, Type, Content Type, Secret, and Token fields above the Outgoing Webhook Settings section.](.gitbook/assets/screenshots/provider/admin-config-services-webhook-add.png)

The numbered callouts in Figure 13-20 are:

1. **Label** field. A recognisable name for the webhook so you can find it in the list later (for example "Slack #api-ops").
2. **Type** field. Set to **Outgoing**. Outgoing webhooks POST new events to the configured URL.
3. **Content Type** field. The body format the marketplace sends. Leave it on `application/json` unless the receiver needs something else.
4. **Secret** field. The shared string the receiver uses to verify the inbound signature. Treat it like a password and do not reuse it across webhooks.
5. **Token** field. An optional token the receiver checks on the incoming hook. Use it as a second factor alongside the signature.
6. **Outgoing Webhook Settings** section. The collapsible panel where you enter the target URL and pick the events to subscribe to.

{% hint style="success" %}
**Result:** From this point on, every event you ticked fires a POST to the receiver URL with a signed JSON body.
{% endhint %}

{% hint style="info" %}
**Note:** A webhook delivery times out after 10 seconds. If the receiver is slow, the delivery fails and is retried per the retry policy. Make sure the receiver acknowledges quickly and queues the work asynchronously.
{% endhint %}

Verify:

1. Confirm the row appears in the **Webhook** list with status **Active**.
2. Click **Send test** and confirm the receiver acknowledges the test event with a `2xx` response.
3. Trigger a real subscribed event and confirm the delivery shows up in the **Deliveries** tab.

#### Send a test delivery

Send a test delivery to confirm a newly registered receiver is reachable, that the signature header is being verified correctly, and that the JSON payload shape matches what the receiver expects. Use this on every new webhook before relying on it in production.

To send a test:

1. Open **SETTINGS** > **Webhook**.
2. Find the webhook row and open its **Operations** menu.
3. Click **Send test**. The marketplace fires a single POST with a synthetic payload tagged `test: true`.
4. Watch the modal for the response code and elapsed time.
5. Open the **Deliveries** tab to inspect the request and response bodies in full.

{% hint style="success" %}
**Tip:** Tag synthetic deliveries in your receiver logs by checking the `X-Marketplace-Test` header. That stops a test event from appearing in your real downstream metrics.
{% endhint %}

#### Inspect webhook delivery logs

Inspect delivery logs when a downstream system reports it stopped receiving notifications, when you want to confirm a specific event was delivered, or when you want to retry a failed delivery.

To read the delivery log for a webhook:

1. From the left sidebar, expand **SETTINGS** and click **Webhook**.
2. Click the webhook's name to open its detail view.
3. Open the **Deliveries** tab. Each row is one delivery attempt with the event type, target URL, response code, and timestamp.
4. Click a row to expand the request and response bodies, headers, and any error message.
5. To retry a failed delivery, click **Retry** on that row.

Common response-code patterns:

- **2xx**. Delivered successfully. No action needed.
- **3xx**. The receiver redirected. The marketplace does not follow redirects. Update the target URL in the webhook to the final destination.
- **4xx (other than 401)**. The receiver rejected the payload. Look at the response body in the expanded row to see why.
- **401**. The receiver rejected the signature. Confirm the shared secret on both ends matches.
- **5xx**. The receiver errored out. Check the receiver's logs.
- **Timeout**. The receiver did not respond within 10 seconds. Move the work to an async queue on the receiver.

{% hint style="warning" %}
**Caution:** Disabling a webhook does not retry failed deliveries that arrived while it was disabled. Investigate failures, fix the receiver, and only then re-enable. If the gap matters, replay the missed events from the marketplace audit log once the receiver is healthy.
{% endhint %}

{% hint style="success" %}
**Tip:** Add a Slack webhook that subscribes to delivery-failure events for every other webhook. You will hear about a broken receiver in your team chat before someone asks why their automation stopped working.
{% endhint %}

#### Rotate a webhook secret

Rotate a webhook secret on a regular cadence, when a team member with access leaves the organisation, or when you suspect the secret has been exposed in a log or screenshot.

To rotate:

1. Generate a new random string in your password manager.
2. Open **SETTINGS** > **Webhook** and click the webhook name.
3. Click **Edit**, paste the new value into the **Secret** field, and click **Save**.
4. Update the receiver to verify against the new secret.
5. Send a test delivery and confirm the receiver returns a `2xx`.

{% hint style="info" %}
**Note:** The marketplace uses the new secret on the very next outbound delivery. There is no overlap window where both secrets are accepted, so coordinate the rotation with the receiver team.
{% endhint %}

## Customising email templates

Every transactional email the marketplace sends (subscription approvals, password recovery, member invitations, API deprecation notices) comes from a template. The portal ships with sensible defaults. Most organisations customise at least the subject lines and the From-name to match their brand voice.

![Figure 13-8. The Email templates list, grouped by category, with one row per transactional template.](.gitbook/assets/screenshots/provider/admin-structure-email-templates-templates.png)

The numbered callouts in Figure 13-8 are:

1. **Email type** column. The template name as it appears in the list. Templates are grouped into collapsible categories (**API Deprecation**, **General**, **MFA**, **Organisation**, **User**) so you can find one quickly.
2. **Machine name** column. The internal identifier the marketplace uses to fire the template on the matching event. Read-only. Useful when an integrator asks you which template handles a specific event.
3. **Subject line** column. Open the template to see and edit the subject. The subject is the only field you commonly change when matching brand voice. It appears as the email's **Subject** header.
4. **Edit template** action. Per-row action that opens the template editor. From the editor you change the subject, the body, and the variables substituted at send time.

![Figure 13-9. The email transport configuration where you set the From address, From name, and SMTP credentials used for every outbound email.](.gitbook/assets/screenshots/provider/admin-config-system-symfony-mailer-lite-transport.png)

The numbered callouts in Figure 13-9 are:

1. **Transport name** field. The label that identifies this transport in the email-template editor.
2. **DSN** field. The full transport string (for example `smtp://user:pass@host:port`). The marketplace uses it to dispatch every outbound email.
3. **From address** field. The sender address that appears in every email's **From** header. Match this to a mailbox your team monitors for bounces and replies.
4. **From name** field. The display name that pairs with the From address. Typically your portal or company name.
5. **Save configuration** button. Persists the transport. New outbound emails use it immediately. Queued emails are not retroactively rewritten.

#### Configure the outbound email transport

Configure the transport before you customise any template. The transport carries every outbound email out of the marketplace, so a misconfigured DSN means nothing ships and your users see silent failures.

#### Before you start

- **Have the SMTP credentials.** Username, password, host, and port. Most managed providers (SendGrid, Postmark, Mailgun, SES) issue a per-environment credential.
- **Pick a monitored From address.** Bounces and out-of-office replies go to whatever address you set. A `no-reply@` mailbox that nobody watches loses real bounces.
- **Decide on TLS.** Production transports should require TLS. Add `?encryption=tls` to the DSN if your provider does not negotiate it by default.

To configure the transport:

1. From the left sidebar, expand **Configuration** > **System** and click **Symfony Mailer Lite transport**.
2. Enter a friendly **Transport name** (for example "Production SMTP").
3. Paste the full **DSN** string. The exact format depends on your provider. SMTP DSNs look like `smtp://user:pass@host:port`.
4. Enter the **From address** and **From name** that appear on every outbound email.
5. Click **Save configuration**.
6. Trigger a low-impact email (for example, invite yourself to a test Organisation) and confirm it arrives.

{% hint style="success" %}
**Result:** Every outbound email now flows through the configured transport with the From address and name you set.
{% endhint %}

{% hint style="warning" %}
**Caution:** A misconfigured DSN does not throw an obvious error in the UI. Always send a test email after saving and watch your inbox.
{% endhint %}

{% hint style="success" %}
**Tip:** Keep DSN credentials in your secrets manager and paste them into the DSN field only at configuration time. The DSN field stores the credentials, so anyone with admin access can read them back from the form.
{% endhint %}

#### Set the SMTP server credentials

The SMTP settings page holds the raw server host, port, and encryption that the transport dials out through. Configure it when your mail provider gives you SMTP credentials rather than a single DSN string, or when you want to set a backup server and TLS behaviour explicitly.

#### Before you start

- **Have the SMTP host and port.** The default port is 25. If your network blocks it, port 587 or 465 usually works. Most managed providers document the port they expect.
- **Know the encryption your provider requires.** Production providers require TLS or SSL. Pick the matching option rather than leaving it on the default.
- **Have a backup server if your provider offers one.** The marketplace falls back to the backup server when the primary cannot be reached.

To set the SMTP credentials:

1. From the left sidebar, expand **Settings** and click **SMTP Settings**.
2. Enter the **SMTP server** host (for example `smtp.gmail.com`).
3. Enter a **SMTP backup server** if your provider offers one. This is optional. The marketplace tries it only when the primary is unreachable.
4. Enter the **SMTP port**. The default is 25. Use 587 or 465 if 25 is blocked.
5. Choose the encryption under **Use encrypted protocol** (for example **Use TLS**) to match what your provider requires.
6. Set **Enable TLS encryption automatically** to **On** so the marketplace negotiates TLS where the server supports it.
7. Click **Save configuration** in the top-right.
8. Trigger a low-impact email and confirm it arrives.

![Figure 13-21. The SMTP Authentication Support page with the server host, backup server, port, and encryption controls.](.gitbook/assets/screenshots/provider/admin-config-system-smtp.png)

The numbered callouts in Figure 13-21 are:

1. **SMTP server** field. The host of your outgoing SMTP server (for example `smtp.gmail.com`).
2. **SMTP backup server** field. Optional secondary host the marketplace tries when the primary cannot be reached.
3. **SMTP port** field. The port the marketplace connects on. Defaults to 25. Use 587 or 465 if 25 is blocked.
4. **Use encrypted protocol** selector. The transport encryption (for example **Use TLS**) for servers that require SSL or TLS.
5. **Enable TLS encryption automatically** toggle. Set to **On** to negotiate TLS automatically where the server supports it.
6. **Save configuration** button. Persists the SMTP settings. New outbound emails dial out through these values immediately.

{% hint style="success" %}
**Result:** Outbound email now connects through the SMTP host, port, and encryption you set.
{% endhint %}

{% hint style="warning" %}
**Caution:** A wrong port or encryption mode fails silently in the UI. Always send a test email after saving and confirm it lands.
{% endhint %}

#### Edit a system email template

Edit a system email template when the default copy does not match your tone, when legal asks for specific footer copy, when you want to change which fields are shown, or when you want to translate templates for a non-English audience.

#### Before you start

- **Know the trigger event.** Each template fires on a specific event. Editing the wrong template means your change never goes out.
- **Know which variables the template exposes.** Variables like `[user:display-name]`, `[site:name]`, and `[org:name]` are substituted at send time. The template editor lists every variable available to that template.
- **Have a test recipient.** Test changes against your own email address before rolling them out broadly.

The templates that ship with the portal include:

- **Default**. The fallback template used when no event-specific template applies.
- **Member Invite**. Sent when a Provider or Org Admin invites a colleague.
- **User Activation**. Sent when a new user account is activated.
- **User Password Recovery**. Sent when a user requests a password reset.
- **User Created (No Approval)**. Sent when a self-signup user is created without admin review.
- **User Created (Awaiting Approval)**. Sent to the user when their signup needs admin review.
- **User Created (Approval Admin)**. Sent to the admin who needs to approve a new signup.
- **User Created By Admin**. Sent when an admin creates an account directly.
- **User Blocked**. Sent when an account is blocked.
- **User Cancelled**. Sent when an account is cancelled.
- **User Cancellation Confirmation**. Confirmation to the user that their cancellation has been processed.
- **MFA Email OTP**. Sent when a user logs in and email-based multi-factor authentication is required.
- **API Deprecation Upcoming**. Sent ahead of an API's deprecation date.
- **API Deprecation Deprecated**. Sent on the deprecation date.
- **API Deprecation Delete Notice**. Sent before a deprecated API is fully removed.

To edit a template:

1. From the left sidebar, expand **SETTINGS** and click **Email templates**.
2. The **Email templates** list loads. Find the template you want by name.
3. Click the template name to open the editor.
4. Update the **Subject** field if you want a different subject line.
5. Update the **Body** in the rich-text editor. Insert variables from the variable list on the right of the editor.
6. Click **Preview** to see the rendered email with placeholder values substituted.
7. Click **Save**.

{% hint style="success" %}
**Result:** The next outbound email of that type uses the new template. Subject and body changes apply on the next outbound email. Queued emails that have not yet sent are not retroactively rewritten.
{% endhint %}

{% hint style="success" %}
**Tip:** Test a template change by triggering the corresponding event for a test user before rolling it out broadly. For example, edit the Member Invite template, then invite yourself to a test Organisation and confirm the email looks right.
{% endhint %}

{% hint style="warning" %}
**Caution:** A broken variable name (a typo in `[user:display-name]`) renders as the literal string in the outgoing email. Always send a test email to yourself after editing.
{% endhint %}

Verify:

1. Trigger the corresponding event for a test recipient and confirm the email arrives with the new subject and body.
2. Confirm every variable in the body resolves to the right value. No literal `[user:display-name]` strings remain.
3. Confirm the From-name and From-address match what you configured on the transport.

## Reviewing moderated content

Editorial workflow puts a review gate between a content author saving a draft and that draft going public. The Moderated content view is where reviewers see every item waiting on them, every recently transitioned item, and every scheduled publish, across Pages, CMS items, and Media, in one queue.

To open the moderation queue:

1. From the left sidebar, expand **Content** and click the **Moderated content** tab.
2. Use the **Content type** filter to narrow the queue to one type (for example, **Page** or **Article**) when you only review one kind.
3. Use the **Moderation state** filter to scope to **Draft**, **Needs review**, **Published**, or **Archived**.
4. Click a title to open the item, review the changes, then transition the state from the editor.

![Figure 13-10. The Moderated content queue showing every item with a non-published state, plus filters for Title, Content type, and Moderation state.](.gitbook/assets/screenshots/provider/admin-content-moderated.png)

The numbered callouts in Figure 13-10 are:

1. **Moderated content** heading. The page heading. The tab strip below switches between the **Content** list (everything), **Pages** and **Media** sub-lists, **Moderated content** (this view), and the **Scheduled content** queue.
2. **Title / Content type / Moderation state** filters. Filters at the top of the queue. Combine them to find, for example, every Page in **Needs review** state.
3. **Updated** column. Sortable column showing when the draft was last saved. Sort descending to see what came in today.
4. **Operations** menu. Per-row action that opens the item for review. From the editor you transition the state and add a revision message that the original author sees.

{% hint style="success" %}
**Result:** You see one ranked queue of every item that wants review, regardless of which content type it belongs to. Approving an item from here is the same as opening it directly from its content list and changing its state.
{% endhint %}

{% hint style="success" %}
**Tip:** Sort by **Updated** descending and skim the top of the queue every morning. Drafts that sit longer than a week often go stale. Chase the author or close the item.
{% endhint %}

#### Set a moderation policy on a content type

Set a moderation policy when you want a specific content type (for example, blog articles or partner-program pages) to require review before going live. The default workflow allows direct publish; switching to a moderated workflow gates publication behind the queue above.

To set a moderation policy:

1. From the left sidebar, expand **Configuration** > **Workflow** and click **Workflows**.
2. Click **Edit** on the **Editorial** workflow.
3. Under **This workflow applies to**, tick the content types you want gated.
4. Save the workflow. Existing items in those types stay in their current state. New items default to **Draft** and must transition through **Needs review** before they reach **Published**.

{% hint style="info" %}
**Note:** Changing a workflow does not retroactively re-moderate published items. Items already in **Published** stay there until someone explicitly transitions them.
{% endhint %}

## Using the App Builder for consumer demos

App Builder is a low-code surface where you build small demonstration Apps on top of the APIs in your catalogue. Use it to prove an integration pattern, give a partner a runnable starter, or seed a Workspace that consumers can clone for their own Apps.

To open App Builder:

1. From the left sidebar under **API MANAGEMENT**, click **App Builder**.
2. The landing surface loads. The left rail summarises the **API Catalog** the builder is wired to and lists **TEMPLATES** and **Workspaces** you can open or extend.
3. Pick a template tile, choose **Workspaces** > **+ New** to start a blank workspace, or type a goal into the prompt box on the right and let the assistant draft a starting point.

![Figure 13-11. The App Builder landing page with the API Catalog summary on the left, template tiles, and the assistant prompt on the right.](.gitbook/assets/screenshots/provider/app-builder.png)

The numbered callouts in Figure 13-11 are:

1. **API Catalog** panel. A summary of the APIs and endpoints the builder can call, with live counts. Click **Sync** to refresh the inventory after you publish a new API.
2. **TEMPLATES** list. Pre-built starter Apps (for example, **Customer Support System**) you clone as the basis for a new build. Templates package an API selection, a UI layout, and routing wiring.
3. **Shared Workspaces** list. Workspaces shared with you or your team. Open one to keep working on a build that someone else started.
4. **Workspaces / + New**. Your own workspaces. Click **+ New** to create a blank workspace.
5. **Prompt box**. Type what you want to build (for example, "a dashboard that lists active subscriptions"), pick a model, and the builder drafts a starting App you then refine.
6. **Discover / Ideate / Build / Visualize** phases. The four phases of the builder along the bottom of the prompt area. Move between phases as you turn an idea into a runnable App.

{% hint style="success" %}
**Result:** You have a workspace where you compose an App from APIs in your catalogue. Publish the workspace and consumers see it as a clonable starter on their own dashboard.
{% endhint %}

{% hint style="info" %}
**Note:** App Builder runs against your published APIs. If a partner needs a demo of an API that is still in draft, publish it to a private Plan first so the builder can call it.
{% endhint %}

## Configuring search and URL paths

Two surfaces under **Configuration** > **Search and metadata** control how content is found by URL. **URL aliases** map machine-generated paths (like `/node/688`) to readable paths (like `/api/payment-service`). **Redirect** rules forward old paths to new ones so links from emails, search engines, or external sites keep working when something moves or is renamed.

#### Edit a URL alias

Edit an alias when a renamed page should keep a clean public URL or when an auto-generated alias drifted from the page title.

To edit a URL alias:

1. From the left sidebar, expand **Configuration**, then **Search and metadata**, and click **URL aliases**.
2. Use **Filter aliases** to find the alias by its public path or by the system path it points at.
3. Click **Edit** in the row's **Operations** column.
4. Update the **Alias** field to the new public path (for example `/api/payment-service`).
5. Click **Save**.

![Figure 13-12. The URL aliases list, with the alias on the left, the system path on the right, and an Edit action per row.](.gitbook/assets/screenshots/provider/admin-config-search-path.png)

The numbered callouts in Figure 13-12 are:

1. **URL aliases** heading. The page heading. Each row maps one public path to one system path.
2. **Filter aliases** input. Free-text filter that scopes the list to rows whose alias contains your text.
3. **Alias** column. The readable public path consumers see in their address bar.
4. **System path** column. The internal path the alias resolves to. Read-only. Change the alias, not this column.
5. **Add URL alias** button. Top-right action that opens the new-alias form when you want a path that the auto-generator did not produce.

{% hint style="success" %}
**Tip:** When you rename a page, change its alias here AND add a redirect from the old alias to the new one (covered in the next task). That preserves every inbound link to the old path.
{% endhint %}

#### Add a redirect for a moved or renamed URL

Add a redirect when an inbound path needs to land somewhere else. For example, when you renamed an API listing, deleted an old marketing page, or restructured the documentation tree. The marketplace forwards the request to the new path with a 301 status by default.

To add a redirect:

1. From the left sidebar, expand **Configuration**, then **Search and metadata**, and click **Redirect**.
2. Click **+ Add redirect** in the top-right.
3. Enter the old path in the **From** field (for example `/old-payments-api`).
4. Enter the new path or external URL in the **To** field (for example `/api/payment-service`).
5. Choose a **Status code**: `301 (Moved Permanently)` for renamed content, `302 (Found)` for temporary redirects.
6. Click **Save**.

![Figure 13-13. The Redirect list with the From and To paths, status code, and a sub-tab for fixing 404 pages.](.gitbook/assets/screenshots/provider/admin-config-search-redirect.png)

The numbered callouts in Figure 13-13 are:

1. **URL Redirects / Fix 404 pages** tabs. Tab strip. The first tab lists every active redirect. The second lists requests that returned 404 so you can convert them into redirects in one click.
2. **From** column. The old path the marketplace receives a request for.
3. **To** column. The destination the marketplace forwards the request to. Internal paths and external URLs both work.
4. **Status code** column. The HTTP status returned to the client. Use `301` for permanent moves so search engines update their index.
5. **Edit** action. Per-row action that opens the redirect for editing. Use it to update the **To** path when content moves a second time.
6. **+ Add redirect** button. Top-right action that opens the new-redirect form covered in step 2 above.

{% hint style="info" %}
**Note:** A long redirect chain (A redirects to B redirects to C) slows the request and confuses search-engine indexers. When you redirect a path, point it directly at the final destination, not at another redirect.
{% endhint %}

Verify:

1. Open the **From** path in a private browser window and confirm the browser lands on the **To** destination.
2. Confirm the response status matches the code you picked (`301` or `302`).
3. Confirm there is no chain. The redirect lands on the final destination in one hop.

#### Convert a 404 into a redirect from the Fix 404 pages tab

The marketplace records every request that returned a 404 in the **Fix 404 pages** tab next to **URL Redirects**. Skim this tab weekly to catch broken inbound links from search engines, social posts, or external blogs before they impact discovery.

To convert a 404 into a redirect:

1. Open **Configuration** > **Search and metadata** > **Redirect**.
2. Click the **Fix 404 pages** tab.
3. Each row shows the path that 404'd and how many times it was hit. Sort by **Count** descending to surface the highest-impact misses.
4. Click **Add redirect** on the row.
5. The **From** field is pre-filled with the 404'd path. Enter the **To** path and pick a status code.
6. Click **Save**.

{% hint style="success" %}
**Tip:** A spike of 404s on one path often means a page was renamed without a redirect or that an external site is linking to the wrong path. Trace the referrer before you fix the redirect to confirm the right destination.
{% endhint %}

## Registering an additional domain

Some organisations run more than one branded portal from one marketplace tenancy. For example, a public portal at `developer.example.com` and a partner portal at `partners.example.com`. Domain registration is where you control which email domains are allowed, or blocked, when users sign up to any of those portals.

To configure domain registration rules:

1. From the left sidebar, expand **Configuration**, then **System**, and click **Domain registration**.
2. Choose a **Restriction Type**: **Allow only domains listed below to register** for a closed allow-list, or **Prevent domains listed below from registering** for a deny-list.
3. Enter one domain per line in the **Email domains** field. Wildcards work. `*.example.com` matches any subdomain.
4. Enter the message a blocked user sees in the **Error message** field.
5. Click **Save configuration** in the top-right.

![Figure 13-14. The Domain registration form with the Restriction Type radio buttons, the Email domains list, and the Error message field.](.gitbook/assets/screenshots/provider/admin-config-system-domain-register.png)

The numbered callouts in Figure 13-14 are:

1. **Restriction Type** radios. Radio choice between an allow-list and a deny-list. Pick one mode. You cannot mix.
2. **Email domains** field. One domain per line. Wildcards (`*.example.com`) match any subdomain. Use this list for both allow-list and deny-list modes.
3. **Error message** field. The message shown to a user whose email domain is not permitted. Keep it actionable. Point them at a contact form or a partner manager.
4. **Save configuration** button. Top-right action that persists the rules. New signup attempts are evaluated against the new list immediately. Existing users are not retroactively blocked.

{% hint style="success" %}
**Tip:** Use the deny-list mode (with consumer mailbox domains like `gmail.com`) for a partner portal that should only admit corporate addresses. Use the allow-list mode when you have a fixed roster of partner companies.
{% endhint %}

{% hint style="warning" %}
**Caution:** A typo in **Email domains** locks out every user from that domain on the next signup attempt. Verify the list with a test address before saving.
{% endhint %}

## Reading and acting on inbox notifications

The bell icon in the top-right of every page opens your personal notifications inbox. Every notification the marketplace sends you (subscription approvals or rejections, governance scan completions, system announcements, webhook delivery failures, member invitations) accumulates here even when you missed the email.

To open and filter the inbox:

1. Click the bell icon in the top-right of the page header. The inbox loads as a full-page view.
2. Use the **Type** dropdown to scope to one event type (for example, **Subscription** or **Governance**).
3. Use the **Status** dropdown to scope to **Read** or **Unread** entries.
4. Set a **From** and **To** date to scope to a window. That is useful when you are looking for a specific event you remember happening last week.
5. Click **Filter** to apply, or **Reset** to clear.
6. Click any row to open the notification detail. Opening a row marks it read.

![Figure 13-15. The personal notifications inbox with Type, Status, and date filters above the entry list.](.gitbook/assets/screenshots/provider/notifications-inbox.png)

The numbered callouts in Figure 13-15 are:

1. **Type** dropdown. Scopes the inbox to one event family (subscription events, governance scans, system announcements, webhook delivery failures, member invitations).
2. **Status** dropdown. Scopes to **Read** or **Unread** entries. Default is **Any**.
3. **From / To** date inputs. Narrow the inbox to events between two dates. Both are optional.
4. **Filter / Reset** buttons. **Filter** applies your selections. **Reset** clears every filter and shows the full inbox.
5. **Notification list**. Each row is one notification. Click to open the detail view. Opening a row marks it read.

{% hint style="success" %}
**Result:** You see your notifications history. The badge on the bell icon clears when every entry is read.
{% endhint %}

{% hint style="success" %}
**Tip:** When a downstream automation reports it stopped working, check the inbox for a webhook-delivery-failure entry. The detail view links to the webhook's delivery log covered earlier in this chapter.
{% endhint %}

## Managing portal features

Feature Management is the master switchboard for the portal. Each toggle turns a whole capability on or off for your tenancy without a code deployment. Use it to roll a feature out gradually, to disable a capability your organisation does not use, or to gate an in-progress feature until it is ready.

#### Enable or disable a portal feature

Open Feature Management when you want to turn a capability on for the first time, retire one your team no longer uses, or stage a rollout where one feature group goes live ahead of another.

#### Before you start

- **Know what the feature controls.** Toggling a feature off hides every surface that depends on it for every user, not just yourself. Confirm no one relies on it before you switch it off.
- **Group the change.** Features are organised into groups (User Management, API Engagement, Portal Workflows). Expand the group to see the individual toggles and the count of features it holds.

To toggle a feature:

1. From the left sidebar, expand **Settings** and click **Manage Features**.
2. The **Feature Management** page loads with the feature groups collapsed. Each group shows a count of the features it holds.
3. Click a group (for example **API Engagement**) to expand it and see the individual toggles.
4. Flip the toggle on or off for the feature you want to change.
5. The change saves on toggle. No separate save step is required.

![Figure 13-23. The Feature Management page with the User Management, API Engagement, and Portal Workflows groups, each showing a feature count.](.gitbook/assets/screenshots/provider/admin-portal-features.png)

The numbered callouts in Figure 13-23 are:

1. **Feature Management** heading. The page heading. The line below it confirms you are enabling or disabling features for your portal.
2. **User Management** group. The collapsible group holding user-facing toggles. The pill on the right shows how many features it contains.
3. **API Engagement** group. The collapsible group holding API-discovery and engagement toggles.
4. **Portal Workflows** group. The collapsible group holding workflow toggles such as moderation and approval gates.
5. **Group expander**. The chevron on each row. Click it to expand the group and reveal the individual on/off toggles.

{% hint style="success" %}
**Result:** The capability is on or off across the portal immediately. Surfaces that depend on a disabled feature stop rendering for every user.
{% endhint %}

{% hint style="warning" %}
**Caution:** Disabling a feature that other surfaces depend on can hide functionality your team relies on day to day. Toggle one feature at a time and confirm the affected surfaces still behave as expected before moving on.
{% endhint %}

## Reviewing the marketplace audit log

The Marketplace Audit Log is the chronological record of every consequential action in the portal: content created and updated, members added, APIs published, and system events. Use it to answer "who changed this and when", to investigate an unexpected state change, or to satisfy a compliance review.

#### Read and filter the audit log

Open the audit log when you need to trace a specific change, confirm who performed an action, or export a window of activity for a review.

To read the audit log:

1. From the left sidebar, expand **Administration** and click **Audit Log**.
2. The **Marketplace Audit Log** loads with the most recent entries first. Each row records the time, the user, the organisation, the entity, the operation, and the source IP.
3. Use the **Organisation**, **Entity type**, **Content type**, and **Operation** filters to narrow the list. Combine them to find, for example, every `create` operation on an API entity within one organisation.
4. Set a **From** and **To** date to scope the log to a window.
5. Click **Filter** to apply, or **Reset** to clear.
6. Click **Export CSV** in the top-right to download the filtered window for an offline review.

![Figure 13-24. The Marketplace Audit Log with Organisation, Entity type, Content type, and Operation filters above the chronological entry list.](.gitbook/assets/screenshots/provider/admin-reports-marketplace-audit.png)

The numbered callouts in Figure 13-24 are:

1. **Marketplace Audit Log** heading. The page heading. The list below is the chronological activity record.
2. **Organisation / Entity type / Content type / Operation** filters. Combine them to scope the log to a specific actor or action.
3. **From / To** date inputs. Narrow the log to events between two dates.
4. **Filter / Reset** buttons. **Filter** applies your selections. **Reset** clears every filter.
5. **Operation** column. The action recorded for each row, tagged **create** or **update** so you can scan for a class of change at a glance.
6. **Export CSV** button. Top-right action that downloads the currently filtered window for an offline or compliance review.

{% hint style="success" %}
**Result:** You see a filtered, time-ordered record of who did what across the portal, exportable for review.
{% endhint %}

{% hint style="success" %}
**Tip:** When a downstream system reports an unexpected state, filter the audit log to the affected entity and the hour around the incident. The operation and user columns usually point straight at the change that caused it.
{% endhint %}

## AI agent surfaces

The two **AI agent** surfaces in the sidebar, **API GPT** and **MCP Servers**, are covered end-to-end in the chapter on exposing APIs to AI agents. API GPT is the chat-style assistant your team and consumers query against the API catalogue. MCP Servers is where you register and manage Model Context Protocol endpoints. The screenshots and walkthroughs there cover the full setup; this chapter does not duplicate them.

## Scheduling future state changes

Some changes need to be announced ahead of time, take effect on a specific date, and require their own audit trail. The marketplace handles this through scheduled-state content, most prominently the deprecation flow.

#### Schedule a content publication

Schedule a content publication when an announcement, a policy update, or a new partner-program page needs to appear at a specific date and time. The marketplace flips the moderation state from **Draft** to **Published** automatically at that moment.

To schedule a publication:

1. Open the content item from **Content** > **Pages** (or **Articles**, or any content type that supports scheduling).
2. Open the **Scheduling** sidebar panel.
3. Enter a **Publish on** date and time.
4. Optionally enter an **Unpublish on** date and time for content that should retire on its own.
5. Save the item with status **Draft**. The marketplace publishes it at the scheduled time.

![Figure 13-16. The Scheduled content queue showing every notice and content item with a future publish or transition date.](.gitbook/assets/screenshots/provider/admin-content-scheduled.png)

The numbered callouts in Figure 13-16 are:

1. **Scheduled** heading. The page heading. Each row is one content item with a future state change pending.
2. **Title** column. The content title. Click to open the item and adjust dates or copy.
3. **Content type** column. Tells you whether the row is a deprecation notice, a scheduled page publish, or another scheduled item.
4. **Publish on** column. The date the scheduled state change fires. Edit the item to change this date.
5. **Status** column. Whether the schedule is **Pending** (waiting to fire) or has already run.

{% hint style="success" %}
**Tip:** The Scheduled queue is the single place to audit every future state change in the portal. Skim it every Monday morning to catch a scheduled publish that was set up for the wrong week.
{% endhint %}

#### Schedule an API deprecation notice

Schedule a deprecation notice when an API is going out of service and you want consumers, App owners, and integrators to see a clear timeline of what is happening and when.

#### Before you start

- **Pick the deprecation date.** This is the date the API stops accepting new traffic. Existing consumers continue to call until the delete date.
- **Pick the delete date.** This is the date the API is fully removed. Most teams set this 30 to 90 days after the deprecation date.
- **Decide on a recommended replacement.** If consumers should migrate to a different API, link to it in the notice body.

To schedule a deprecation notice:

1. From the left sidebar, expand **Structure** and click **Upcoming API Deprecation Notice**.
2. Click **Add notice** in the top-right.
3. Choose the API from the API picker.
4. Enter the **Deprecation date**, the date the API stops accepting new traffic.
5. Enter the **Delete date**, the date the API is fully removed.
6. Write the **Notice body**. Explain why the API is going out of service and what consumers should do. Link to the replacement API if there is one.
7. Click **Save**.

![Figure 13-22. The Upcoming API Deprecation Notice configuration under Structure, where you define the deprecation notice entity for an API.](.gitbook/assets/screenshots/provider/admin-structure-deprecation-notice.png)

The numbered callouts in Figure 13-22 are:

1. **Upcoming API Deprecation Notice** heading. The page heading, reached from **Structure**. This is the settings surface for the deprecation notice entity type.
2. **Settings panel**. The body of the form where you define the notice. Add a notice from here to attach a deprecation timeline to an API.
3. **Save** button. Top-right action that persists the notice and arms the scheduled deprecation flow.

{% hint style="success" %}
**Result:** The notice is scheduled. From this point on:

- Every consumer who has subscribed to the API sees a deprecation banner on their App dashboard and on the API's catalogue page.
- The **API Deprecation Upcoming** email goes out to subscribed consumers on a cadence ahead of the deprecation date.
- On the deprecation date, the **API Deprecation Deprecated** email goes out and the API moves to a Deprecated state in the catalogue.
- On the delete date, the **API Deprecation Delete Notice** email goes out and the API is fully removed.
{% endhint %}

{% hint style="info" %}
**Note:** Editing or cancelling a scheduled deprecation is supported up until the deprecation date fires. After that the state change has happened. You cannot un-deprecate by editing the notice. Publish the API again as a new version instead.
{% endhint %}

{% hint style="success" %}
**Tip:** Pair a deprecation notice with a system notification (covered earlier in this chapter) on the day the deprecation fires. The system notification gives you a portal-wide banner. The deprecation notice gives subscribed consumers their per-API context.
{% endhint %}

Verify:

1. Confirm the notice appears in the **Scheduled** queue with status **Pending** and the dates you entered.
2. Sign in as a subscribed consumer and confirm the deprecation banner appears on the API's catalogue page and on their App dashboard.
3. Confirm the **API Deprecation Upcoming** email lands in the test consumer's inbox on the cadence you expect.

## Next steps

- See the chapter on Monitoring usage. Watch the analytics dashboard for traffic dips after a deprecation banner goes live.
- See the chapter on Managing your team. Delegate ongoing operations work (moderation, notifications, governance overrides) to colleagues with the right roles.
- See the chapter on Configuring access and storefront branding. Match new email templates and notification copy to the brand voice you set in the previous chapter.