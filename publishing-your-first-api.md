Up to this point your API has lived inside the marketplace as a draft. The gateway connection feeds bytes in, the governance pass confirms the API is safe to expose, but no one outside your team can see it yet. This chapter takes you the last mile: complete the consumer-facing metadata, choose the audience the API is visible to, transition the moderation state from draft to published, and verify the entry appears in the public catalog.

You will learn:

- How to complete the consumer-facing metadata fields — Title, Overview, Documentation, Domain, Tags, and logo — that appear on the catalog tile and detail page.
- How to set **Visibility** so the API is shown to the right audience: Public, Internal, or Org Level.
- How to transition the moderation state from Needs Review to Published, with optional revision logging.
- How to verify the published API renders for an anonymous visitor on the public catalog.
- How to schedule a publish for a future date and time.

Allow ~30 minutes per API for the first publish, including content review.

## Editing metadata before you publish

Imported APIs arrive with whatever the gateway provided: an OpenAPI title, a version number, and very little else. Before publishing, complete the fields that consumers read — the description, the logo, the categories. This is the work that converts a row in your gateway into a product entry on your portal.

#### Edit an imported API's metadata

Use this task when an API has been imported and governance-checked but still reads like a machine-generated stub.

#### Before you start

- **Prepare a one-paragraph product description.** Consumers skim. Two or three sentences that state what the API does, what data it returns, and who it is for outperform two pages of reference detail. The reference detail belongs in the OpenAPI spec.
- **Prepare a square logo file (PNG or SVG, 256×256 or larger).** The catalog tile crops to a square. A wordmark with significant horizontal whitespace renders poorly next to other tiles.
- **Agree on a category taxonomy with your team first.** Domain values such as Banking, Health, T1, t2 appear in the public catalog filter. Inconsistent categorisation renders the filter ineffective within ten APIs.

To edit an API's consumer-facing metadata:

1. From the left sidebar, select **API MANAGEMENT**, then **Manage APIs**. The list shows every API across every connected source, with a Status column and an edit action per row.
2. Locate the row for the API to publish. Narrow the list with the Title search, the API Source filter, or the Domain filter at the top of the page.
3. Click the edit action on that row. The marketplace opens the API edit form — the same form as Create API, pre-populated.
4. In the **Title** field, replace the imported title with the consumer-facing name. The imported title often contains a version suffix or an internal codename inappropriate for the public page.
5. Complete **Overview**. Select Markdown in the Text format dropdown for headings, lists, or links. Two short paragraphs is the recommended length.
6. Complete **Documentation**. This is the long-form page that opens when a consumer clicks through from the catalog tile. Walk through authentication, base URL, the three or four most-used endpoints, and a single curl example. Link out to your full reference for the rest.
7. Set **Domain** by selecting one or more values from the multi-select. Choose from Banking, Health, T1, t2, or whatever taxonomy your portal admin has configured.
8. Set **API Tags** with comma-separated keywords consumers might search for — `accounts`, `payments`, `KYC`, etc.
9. Upload your square **logo** image to the file field below the documentation editor. The uploaded image becomes the catalog tile thumbnail.
10. Leave the **Status** dropdown in the right rail at Needs Review for now. Transition it to Published in Chapter 6.3 once the metadata is complete and visibility has been set.
11. Click **Save**. The API remains in draft state but the metadata is committed.

![Figure 6-1. The Manage APIs list with the Status column and per-row edit action.](.gitbook/assets/screenshots/provider/admin-manage-apis.png)

The numbered callouts in Figure 6-1 are:

1. **Title search** — Free-text search at the top of the list. Enter any fragment of the API name; the list filters as you type.
2. **API Source filter** — Restricts the list to APIs imported from one gateway connection. Useful when your portal aggregates several gateways.
3. **Domain filter** — Restricts the list by the discovery filter taxonomy you set in Chapter 4. Use this when you have hundreds of APIs and want to see only one functional area.
4. **Status column** — The current moderation state for each API. Needs Review indicates the API is a draft and not visible to consumers; Published indicates it is live in the catalog. Change this on the edit form, not from the list.
5. **Edit action** — Opens the API edit form pre-populated with the current values. This is where you complete the consumer-facing metadata before changing the status.
6. **Add new** — Creates a brand-new API node from scratch rather than from an imported gateway entry. Ignore this for the publish flow; you imported your API in Chapter 4.

> **Result:** Your API has a human-readable name, an Overview, Documentation, a logo, a domain, and tags. It is still in draft. Consumers who land on `/api-discovery` cannot see it yet.

> **Tip:** Treat the Overview as the back-cover blurb on a book and the Documentation as the first chapter. Marketplace consumer surveys indicate readers decide whether to subscribe within twenty seconds of opening the tile.

> **Note:** The marketplace stores every save as a revision. If the description is damaged, roll back from the revision history without losing the API spec.

#### Verify

1. After saving, reload the edit form and confirm Title, Overview, Documentation, Domain, and Tags retain your values.
2. Open the API detail page and confirm the uploaded logo renders next to the title.
3. Confirm the **Status** column on Manage APIs still reads Needs Review — the publish step has not happened yet.

## Choosing who can see the API

Visibility determines which audience the API tile is rendered for. You can change visibility at any time, but each change has consequences for subscribers, so set it deliberately the first time.

#### Set visibility

Use this before publishing to control whether the API is shown to anonymous visitors, all logged-in users, or only members of your own organisation.

#### Before you start

- **Identify which audience pays for or consumes this API.** Public APIs are commercial or developer-relations APIs. Internal APIs are partner APIs behind portal login. Org Level APIs are private to your team and never appear in the consumer catalog.
- **Confirm the API does not leak data through descriptions.** Endpoint paths or examples in the Overview can expose schema details that you would not show externally. Public APIs require a content-review pass; internal APIs are looser.

To set visibility:

1. On the API edit form, scroll to the visibility radio group. Three options are shown:
2. Select one of:
   - **Public — visible to everyone including anonymous visitors.** Anyone, signed in or not, can find the tile in `/api-discovery`. Select this for APIs intended to drive sign-ups.
   - **Internal — visible to all logged-in users.** The tile is shown to anyone with a marketplace account, regardless of organisation. Select this for APIs shared with partners but not indexed by search engines.
   - **Org Level — visible only to members of this organisation.** The tile is shown only to users in your organisation. Select this for APIs your engineering team consumes internally and that have no place in the consumer catalog.
3. If you select Org Level, the **Teams** picker becomes relevant. Select one or more teams (for example, `QA1`, `TestingTeam`) to limit visibility further. Leave all teams unselected to make the API visible to every member of your organisation.
4. Click **Save**. The visibility takes effect on the next page render — there is no separate confirm step.

The numbered callouts on the visibility section of the edit form are:

1. **Org Level — visible only to members of this organisation** — The narrowest scope. The API does not appear in the public catalog and never appears for users outside your organisation. Pair with the Teams picker for finer-grained access.
2. **Internal — visible to all logged-in users** — Mid scope. Useful for partner programs where every consumer must sign in but public discovery is not desired.
3. **Public — visible to everyone including anonymous visitors** — Widest scope. The tile is shown on `/api-discovery` to signed-out visitors and is eligible for indexing by your sitemap. Select this only after the content has been reviewed.

> **Caution:** Changing an already-published API from Public to Internal or Org Level does not revoke existing subscriptions. Consumers who already subscribed continue to call the API and see it in their app dashboard. To stop calls, also revoke their subscription — see Chapter 8.6.

> **Note:** The Domain — visible only to members of this organisation label is the canonical label; the Org Level prefix is what the radio group displays. Use whichever the screenshot shows when writing internal docs.

> **Tip:** Default new APIs to Internal while your portal is small. Promote them to Public in batches once descriptions and rate limits are confirmed correct.

#### Verify

1. Reload the edit form and confirm the Visibility radio remains on the option you selected.
2. If you selected Org Level, confirm the Teams picker reflects your selection (or remains empty for organisation-wide visibility).
3. Confirm the API detail page renders for an account in the intended audience (and not for an account outside it).

## Publishing the API

Publishing is one click. Everything that matters happens before the click and after the click — what you put in the metadata, and what appears in the catalog. The click itself is a state transition on the moderation field.

#### Move an API from Draft to Published

Use this task when the metadata is complete, the visibility is set, and you are ready for consumers to find the API.

#### Before you start

- **Re-read the Overview and Documentation as if you were a consumer.** Typos, broken links, and unhelpful copy persist once they are public.
- **Confirm the linting score in Chapter 5 is acceptable.** Publishing an API with critical lint failures pushes consumer-visible problems into production.
- **Decide whether to attach the API to an API Product.** Plans live on Products, not APIs. If subscribers are required, Chapter 7 is also necessary.

To move an API from Draft to Published:

1. From the left sidebar, select **Manage APIs**.
2. Locate your API in the list. Confirm the **Status** column reads **Needs Review**.
3. Click the **Edit** action on the row.
4. Scroll to the right rail of the edit form and find the **Status** panel.
5. Under **Change to**, select **Published** from the dropdown.
6. (Optional) Enter a short note in **Revision log message** — for example, *Initial publish, v1*. The note is stored with the revision and helps later retrieval of the change.
7. (Optional) To schedule the publish for later instead of immediately, expand the **Publishing options** panel further down the right rail and set a **Publish on** date and time. Leave it blank for an immediate publish.
8. Click **Save**.

The publish controls live in the right rail of the edit form. From top to bottom the panel contains:

- **Status** panel — Shows the current state (**Draft**) and a **Change to** dropdown with the values **Needs Review**, **Published**, and **Archived**. Selecting **Published** here is the action that takes the API live.
- **Visibility** panel — The three radios you configured in [Choose who can see the API](#set-visibility). They remain editable until the API is published; after that, changing visibility takes effect immediately.
- **Revision log message** — Free-text label for the revision. Appears in the API's revision history alongside the timestamp and author.
- **Publishing options** panel — Optional schedule controls. **Publish on** sets a future date and time; the marketplace flips the state automatically when the schedule fires.
- **URL alias** panel — Auto-generates the public URL from the title by default. Override the alias here if a custom URL is required (typical when migrating from an older portal and preserving incoming links).

> **Result:** The API row in Manage APIs now shows Status: Published. The detail page is rendered for the selected audience. If visibility is Public, anonymous visitors can find the API at `/api-discovery`.

> **Note:** Publishing the API does not on its own make it subscribable. Subscriptions are managed at the API Product level. Continue with Chapter 7 to wrap your API in a Product and a plan, then with Chapter 8 to approve your first consumer.

> **Tip:** If you publish more than ten APIs in a session, sort the Manage APIs list by Status to identify any that have not yet been transitioned.

#### Verify

1. Confirm the Status column on Manage APIs reads Published for the API.
2. Open the API detail page and confirm the URL alias matches what is shown in the right rail.
3. Confirm the revision history lists your latest entry with the revision log message you typed.

## Verifying the public catalog

Trust but verify. The fastest way to confirm the publish worked is to view the page a consumer would see — not the admin view, not your own logged-in view, but a fresh anonymous session.

#### Verify the API appears in the public catalog

Use this immediately after every publish to confirm the change rendered as expected.

#### Before you start

- **Open a private or incognito window.** Your admin session caches the publish state and personalisation. An incognito window starts with no cookies and renders the page exactly as an anonymous visitor sees it.
- **Identify the public URL of your portal.** To verify a published API on the public catalog:

1. Open an incognito or private window.
2. Navigate to `<your-portal>/api-discovery`. 3. Confirm the catalog tile for your API is rendered. The tile should show the title, the logo, and the first line of the Overview.
4. If you set Domain values, click each domain filter to confirm your API appears under the correct category.
5. Click the tile. The detail page should render with your Overview, the Documentation, the API spec viewer, and a Subscribe button (if the API is wrapped in a Product — see Chapter 7).
6. Close the incognito window.

> **Result:** You have viewed exactly what a consumer sees. The publish is verified.

> **Note:** If the tile is not visible, the most common causes (in order) are: visibility is set to Internal or Org Level instead of Public; the moderation state is still Needs Review; the cache has not refreshed (wait one minute and reload). See [Troubleshooting catalog visibility](troubleshooting.md#troubleshooting) if none of the three apply.

> **Tip:** Bookmark `<your-portal>/api-discovery` in your incognito profile. Every publish ends with a one-click sanity check.

## Scheduling a release for later

Sometimes the publish needs to land at a specific time — a Monday-morning launch window, a coordinated press release, the start of a maintenance window. The marketplace supports a scheduled publish on the same edit form.

#### Schedule a release

Use this task when the API is ready now but the launch is later.

#### Before you start

- **Confirm your portal's timezone.** The schedule fields use the timezone of the marketplace, not your browser. Check your profile if uncertain.
- **Decide the target state for the API.** Almost always Published. Confirm before scheduling.

To schedule a future publish:

1. On the API edit form, scroll to the **publish_on** section.
2. Set **Publish on** date to the launch date.
3. Set **Publish on** time to the launch time.
4. Set **Publish state** to Published.
5. Leave **Status** field at Needs Review — the schedule will move the API to Published when it fires.
6. Click **Save**. The API row shows in Manage APIs / scheduled until the schedule runs, then moves to Manage APIs.

> **Result:** The API is queued for publishing. Edits are accepted up to the scheduled time; each save updates the queued version.

> **Caution:** The schedule cannot run if the marketplace cron is not running. If the launch date passes and the API remains in Needs Review, contact your portal administrator to check cron health before retrying manually.

#### Verify

1. Confirm the API row appears under the scheduled view of Manage APIs after **Save**.
2. Confirm the right-rail summary on the edit form shows the Publish on date and time you set.
3. After the scheduled time passes, reload Manage APIs and confirm the Status has flipped to Published.

## Next steps

- **[Reviewing API Products and Plans](reviewing-api-products-and-plans.md#reviewing-api-products-and-plans)** — Wrap your published API in a Product so consumers can subscribe through a Plan.
- **[Onboarding your first consumer](onboarding-your-first-consumer.md#onboarding-your-first-consumer)** — Once a Product is live, approve the first subscription and walk a consumer through obtaining a key.
- **[Reviewing API governance](reviewing-api-governance.md#reviewing-api-governance)** — Re-check the score after each publish; rule changes upstream can produce new findings on previously published APIs.
- **[Monitoring usage](monitoring-usage.md#monitoring-usage)** — After consumers begin calling, monitor latency, errors, and quota consumption from the analytics dashboards.
