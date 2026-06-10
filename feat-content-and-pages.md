---
icon: file-lines
---

The content surfaces under **Content** in the sidebar manage your storefront: standing pages (About, Help, Privacy, Terms, the homepage), articles, and the media library that holds every image, PDF, video, and file the portal serves. Edit content when copy is out of date, when legal asks for a Privacy or Terms revision, or when you launch a new page ahead of a release.

![Figure. The Pages list showing every standing page with status, author, and last-changed columns.](.gitbook/assets/screenshots/provider/admin-content-pages.png)

## What you configure

A page is composed in a visual block builder; media items are stored once and referenced by pages. The fields you fill in:

- **Title**: the heading visitors see at the top of the page and on the browser tab.
- **URL alias**: the public path, for example `/partner-programme`. Use a short, descriptive slug rather than `/node/12`.
- **Description**: the meta description used by search engines and link previews.
- **Body**: built by dragging blocks (heading, text, image, callout, columns) onto the canvas. Rearrange them at any time without losing content.
- **SEO settings**: an optional panel for an SEO title and meta description that differ from the page values.
- **Status**: *Published* or *Unpublished*. Pages publish immediately on save, so draft first if a change needs review.
- **Media: Name and alt text**: the library label and the description screen readers announce and search engines index.

## Configure

1. Expand **Content** in the sidebar, then click **Pages** (or **Media**).
2. To edit a page, find it with the **Title** filter, click the title, then **Edit**; update the body and save.
3. To add a page, click **Add page**, then enter a **Title**, a **URL alias**, and a **Description**.
4. Build the body with the visual block builder, dragging blocks onto the canvas.
5. To add an image or file, open **Media**, click **Add media** (or **Bulk upload** for many), upload, set a **Name** and alt text, and **Save**.
6. Click **Preview** to render the page as a visitor sees it, then **Save** to commit.

![Figure. The Add page editor with title, URL alias, description, and the visual block builder.](.gitbook/assets/screenshots/provider/admin-content-pages-add.png)

## Options

Content is organised by type and moves through an editorial workflow:

- **Content types**: Page, Article, Documentation, Guide, plus Media items, all visible in one Content overview.
- **Moderation states**: Draft, Needs review, Published, Archived. The **Moderated content** queue lists every item waiting on a reviewer.
- **Scheduling**: set a future publish state and the marketplace transitions the item at that time; the **Scheduled content** tab is the queue of pending changes.

![Figure. The Media library list with type, status, and used-in columns.](.gitbook/assets/screenshots/provider/admin-content-media.png)

## Verify

- Reload the page or alias in a private browser window and confirm the new copy renders.
- Confirm the **Status** column on the Pages list reflects what you saved.
- For media, confirm the item appears in the **Media** list with the correct name, type, and alt text, and that every page in the **Used in** column serves the current file.

{% hint style="warning" %}
**Caution:** Deleting a media item breaks every reference to it, leaving that area of a page empty. Always replace before deleting, or check the **Used in** column first.
{% endhint %}

{% hint style="success" %}
**Result:** The page or article is live at its alias and referenced media serves the current file. Published changes appear on the next page load.
{% endhint %}