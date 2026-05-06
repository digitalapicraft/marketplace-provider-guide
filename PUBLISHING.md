# Publishing this guide to GitBook

This folder is a GitBook-compatible Git repository. The contents are produced
from the master Markdown source by `scripts/build_gitbook.py`. Re-run that
script after every doc update to refresh the tree.

## One-time setup

1. **Generate a GitBook personal access token.**
   - Visit https://app.gitbook.com/account/developer.
   - Click **New token**, give it a name (for example `marketplace-docs-publisher`),
     and copy the value.
   - Either pass it on every script invocation with `--token <value>`, or export
     it once: `export GITBOOK_TOKEN=<value>`.

2. **Push this folder to a Git remote.**
   GitBook pulls content from a Git URL. Push the contents of this folder
   (the folder containing `SUMMARY.md`, `README.md`, `.gitbook.yaml`, the
   per-page `*.md` files, and the `.gitbook/assets/` directory) to a Git
   repository GitBook can reach. GitHub, GitLab, and Bitbucket are all
   supported. Public repositories work without auth; private repositories
   need a Git access token passed via `--git-username` and `--git-token`.

   ```bash
   cd delivery/gitbook
   git init
   git checkout -b main
   git add .
   git commit -m "Initial publish: Marketplace User Guide for API Providers"
   git remote add origin git@github.com:your-org/marketplace-docs.git
   git push -u origin main
   ```

3. **Find your GitBook organization ID.**
   ```bash
   python3 scripts/publish_to_gitbook.py --list-orgs
   ```
   Pick the `id` of the organization you want the space to live under.

## First publish

Create a new GitBook space and trigger the import in one command:

```bash
python3 scripts/publish_to_gitbook.py \
    --org <orgId> \
    --new \
    --space-title "Marketplace Provider Guide" \
    --git-url https://github.com/your-org/marketplace-docs \
    --git-ref main
```

The script prints the new `spaceId`. Save it — you'll use it for re-imports.

For a public repo, that's all. For a private repo, add `--git-username
<your-github-username> --git-token <github-personal-access-token>`.

## Subsequent updates

After every doc-source update:

```bash
# 1. Rebuild the GitBook tree from the master MD
python3 scripts/build_gitbook.py delivery/gitbook

# 2. Commit and push to your Git remote
cd delivery/gitbook
git add .
git commit -m "docs: update <what changed>"
git push

# 3. Trigger GitBook to re-import
python3 scripts/publish_to_gitbook.py \
    --space <spaceId> \
    --git-url https://github.com/your-org/marketplace-docs \
    --git-ref main
```

GitBook pulls the latest commit on the named branch. Imports complete in
under a minute for a guide of this size.

## Verifying the import

After the import, open `https://app.gitbook.com/spaces/<spaceId>` in your
browser. You should see:

- **18 pages** organised under the 5 sections in `SUMMARY.md`:
  Introduction, Getting started, Provider workflow, Administration, Reference.
- The **Glossary** rendered as a definition list.
- All **70 figures** loading from the `.gitbook/assets/screenshots/provider/`
  folder.
- **Internal cross-references** resolving — `[Title](page-slug.md#anchor)`
  links jump to the right page and section.

If a screenshot does not appear, check that `.gitbook/assets/screenshots/`
was committed to the Git repo. (GitHub may have ignored the leading `.gitbook/`
folder if a stale `.gitignore` was applied; verify with `git ls-files
.gitbook/`.)

## Optional: publishing to a public site

A GitBook **space** is the editable workspace; a GitBook **site** is the
public, indexed, branded surface customers see. To create one:

1. In the GitBook UI, open the space you just created.
2. Click **Settings → Publishing → Publish**.
3. Pick a sub-domain (`yourorg.gitbook.io/marketplace-provider-guide`) or
   wire up a custom domain (`docs.your-portal.com`).

The site updates automatically on every space change.

## What's in this folder

| Path | Purpose |
|---|---|
| `.gitbook.yaml` | Tells GitBook which file is the README and which is the SUMMARY. |
| `README.md` | Landing page (the Introduction content). |
| `SUMMARY.md` | The navigation tree. Reorder by editing this file. |
| `<chapter>.md` | One file per published page. Edit in your IDE or in GitBook's web editor (changes flow back via Git Sync). |
| `.gitbook/assets/screenshots/` | All figures referenced from the chapters. |
| `.gitignore` | Excludes editor noise (`.DS_Store`, `node_modules/`, etc.). |

## Supported flows for ongoing edits

- **Edit in your IDE**, push to Git, the GitBook script (or a CI job that
  calls the same `/git/import` endpoint) refreshes the space.
- **Edit in the GitBook web editor**. When you save, GitBook can push back
  to the Git remote (configure under **Settings → Git Sync**), keeping the
  Markdown source authoritative.
