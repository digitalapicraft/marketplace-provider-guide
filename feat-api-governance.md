---
icon: shield-halved
---

The marketplace runs every API spec through a configurable set of linting rules and produces a score between 0 and 100. The **API Governance Report** turns that score into action: it shows the catalog-wide picture, the per-API breakdown, the exact JSON path each violation fires on, and copy-ready guidance for fixing it. Use it after any import, after a ruleset change, and as the standing item in a weekly governance review, so findings are cleared before consumers ever see the API.

Governance sits between Draft and Published. The score is the quality bar your API guild agrees on, not a hard gate in code: the marketplace will let you publish a low-scoring API, so the discipline is to fix findings first. Aim for 80 or higher on production APIs and 90 or higher on public ones.

![Figure. The API Governance Report dashboard with summary panels and the Scanned APIs table.](.gitbook/assets/screenshots/provider/admin-api-gov-report.png)

## How the score works

Every API earns one overall score, computed as the average of three category scores, each marked out of 100:

**Overall score = (Security + Style Guidelines + Documentation) / 3**

The report bands the result so you can read health at a glance:

- **Excellent**: 80 and above.
- **Good**: 60 to 79.
- **Fair**: 40 to 59.
- **Poor**: below 40.

Inside each category, individual rules raise findings, and every finding carries a severity of **Error**, **Warning**, or **Info**. Errors weigh the most and Info the least, so the same number of findings can produce very different scores. An API that has never been scanned reads *N/A* until its first run. The category split and the rule toggles that drive these scores are configured on a separate page: see [Governance rules](feat-governance-rules.md).

## What you see

The report has two levels. The top-level dashboard summarises the whole catalog; clicking any API opens a per-API drilldown with the exact findings.

### Catalog dashboard

- **Score Distribution panel.** A histogram counting how many APIs fall in each band. A heavy left tail means many APIs need attention; a right-skewed shape means the catalog is broadly healthy.
- **Category Averages panel.** One row per category (Security, Style Guidelines, Documentation) with the mean score across every scanned API. The lowest category is your priority for the week.
- **Top Violated Rules panel.** The rules triggered most often across the catalog, with a violation count next to each. The rule at the top of this list is usually the single fix that moves the most scores.
- **Scan All button.** Re-scans every API against the current ruleset. Use it after a ruleset change or a large import.
- **Scanned APIs table.** One row per API, sortable by any column.

The **Scanned APIs** table carries these columns:

- **API Title**: the catalog title. Click it to open the drilldown.
- **Score**: a value between 0 and 100, colour-banded by the bands above; *N/A* if no scan has run.
- **Errors**: count of severity Error findings. This is the highest-signal triage cue.
- **Warnings**: count of severity Warning findings.
- **Info**: count of severity Info findings, mostly style and consistency cues.
- **Last scanned**: timestamp of the most recent scan. Sort descending to see the freshest runs.
- **View report**: per-row link into the drilldown.

### Per-API drilldown

![Figure. A per-API governance drilldown with the score header, severity summary, and findings table.](.gitbook/assets/screenshots/provider/admin-api-gov-report-540.png)

- **Score header.** The current score with its colour band, the last-scanned timestamp, and the spec version that was scanned.
- **Severity Summary.** Coloured badges counting Error, Warning, and Info findings. The three counts add up to the total rows in the Findings table.
- **Findings table.** One row per violation, with these columns:
  - **Rule**: the name of the rule that fired. Click it to open the rule reference side panel.
  - **Severity**: an Error, Warning, or Info badge, colour-coded so Errors stand out.
  - **Path**: the JSON pointer to the offending location in the spec. Click to copy it to your clipboard.
  - **Message**: a one-line fix hint, for example *Add a 4xx response definition to this operation*.
- **Rule reference side panel.** Clicking a Rule name opens a panel with **What this rule checks**, **Why it matters**, and **How to fix it** (a short, compliant spec fragment you can copy), plus **Where it fired** with the offending snippet.
- **Scan history panel.** Below the Findings table, the last runs with date, score, and severity counts, so you can read the trend release over release.

{% hint style="info" %}
**Note:** The Score column reflects the most recent successful scan. If a scan fails partway through (for example, the spec became unparseable mid-edit), the column keeps the prior score with a small warning icon until the next successful run.
{% endhint %}

## Read and triage the report

1. From the left sidebar, expand **API MANAGEMENT**, then click **API Governance Report**.
2. Read the three summary panels in order. The lowest **Category Average** tells you which area to focus on; the top entry in **Top Violated Rules** tells you which single fix moves the most scores.
3. In the **Score Distribution** histogram, check the shape. A left-heavy tail means a remediation push is overdue.
4. In the **Scanned APIs** table, sort by **Score** ascending to surface the worst offenders, then by **Errors** descending to find the APIs carrying the heaviest violations.
5. Click an API title to open its drilldown.

{% hint style="success" %}
**Tip:** Treat **Top Violated Rules** as the weekly standup agenda. Fixing the rule at the top of that list moves the Score Distribution histogram more visibly than addressing one low-scoring API at a time.
{% endhint %}

## Drill into a finding and remediate it

1. From the **Scanned APIs** table, click the row for the target API to open its drilldown.
2. Read the **Severity Summary** and sort the **Findings** table by **Severity** descending so Errors come first.
3. Click a **Rule** name to open the side panel. Read **What this rule checks** and **Why it matters**, then copy the fragment under **How to fix it**.
4. Click the **Path** value to copy the JSON pointer, then paste it into your editor's go-to-path tool to land on the exact spec location.
5. Apply the fix in the spec source. If the spec lives in a connected gateway, push the change through your normal release process; if it lives in the marketplace, edit it on the API detail page.
6. Batch several edits, then save the spec once.

{% hint style="success" %}
**Tip:** Group findings by Rule before you start. Rows that share a Rule but differ only by Path usually trace to a single root cause, for example one missing default 4xx response repeated across every operation.
{% endhint %}

## Re-run a scan

After remediation, refresh the score so the report reflects your work.

To re-scan a single API:

1. On the per-API drilldown, click **Re-run scan**. A running indicator appears; most specs finish within a minute.
2. When the scan completes, the **Score header** refreshes and the **Findings** table re-renders.
3. Confirm the targeted finding no longer appears and the score moved in the expected direction.

To re-scan the whole catalog:

1. On the **API Governance Report**, click **Scan All**.
2. Read the confirmation panel: it lists how many APIs are queued and an estimated duration.
3. Click **Start scan**. A per-API progress indicator advances through Queued, Running, and Done.
4. When every row reaches Done, the page returns to the report with refreshed panels.

![Figure. The Bulk API Governance Scan page with the per-API progress indicator.](.gitbook/assets/screenshots/provider/admin-api-gov-report-scan-all.png)

{% hint style="info" %}
**Note:** Per-API scans run automatically when a spec is re-imported or edited. **Scan All** is the right choice after a ruleset change, a first-time import, or recovery from suspected drift.
{% endhint %}

{% hint style="warning" %}
**Caution:** Avoid running **Scan All** during a heavy import. The two operations contend for the same scanner workers, and the import can stall.
{% endhint %}

## Verify

- After **Re-run scan**, confirm the **Score header** refreshes, the targeted finding is gone, and the score moved as expected.
- Confirm the violation count for the fixed rule drops the next time you read **Top Violated Rules**.
- After **Scan All**, confirm the progress indicator advances row by row, the page returns to the report with refreshed panels, and a spot-checked API's **Last scanned** timestamp matches the run.
- Confirm the **Severity Summary** counts on the drilldown add up to the total rows in the **Findings** table.

## Related

- [Governance rules](feat-governance-rules.md) configures which rules run, their severities, and how the three categories combine into the score.
- [Publishing APIs](feat-publishing-apis.md) takes a remediated API from Draft to Published once its score meets your bar.
- [API Products & Plans](feat-products-and-plans.md) groups governed APIs into a Product so consumers can subscribe through a Plan.
- [Importing APIs](feat-importing-apis.md) is the return route when a finding traces back to a missing field on the source spec.

{% hint style="success" %}
**Result:** Each API has a current score, a severity-ranked finding list with exact spec paths, and a clear path from violation to cleared score, refreshed catalog-wide whenever you run Scan All.
{% endhint %}