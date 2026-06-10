---
icon: shield-halved
---

The marketplace scores every API spec against a configurable set of linting rules and produces a value between 0 and 100. The Governance Report shows that score, severity-ranked violations, and remediation guidance, so you fix findings before consumers ever see the API. Governance sits between Draft and Published: treat the score as the quality gate, with a target of 80 or higher for production APIs and 90 or higher for public ones.

![Figure. The API Governance Report dashboard with summary panels and the Scanned APIs table.](.gitbook/assets/screenshots/provider/admin-api-gov-report.png)

## What you read

The report and its per-API drilldown surface everything you need to triage and remediate:

- **Score (0 to 100).** A single number per API, color-banded green (80+), amber (50 to 79), and red (below 50). Violations are weighted by severity, so one Error costs more than several Warnings. APIs not yet scanned read *N/A*.
- **Severity-ranked violations.** Every finding carries a severity: **Error** (red, severity 1), **Warning** (amber, severity 2), or **Info** (grey, severity 3). The per-API Severity Summary counts each band so you clear Errors first.
- **Per-rule breakdown.** Each finding names the **Rule** that fired, the **Path** (JSON pointer to the offending spec location), and a one-line **Message** fix hint. Repeated rows for one rule usually point to a single root cause.
- **Catalog summary panels.** **Score Distribution** (histogram across score bands), **Category Averages** (mean score per Security, Documentation, Naming, Operations), and **Top Violated Rules** (the five rules triggered most often). The top of that last list is where one fix moves the most scores.
- **Remediation guidance.** Clicking a rule name opens a side panel with what the rule checks, why it matters, and a copy-ready compliant example under **How to fix it**.

## Use the report

1. From the left sidebar, expand **API MANAGEMENT**, then click **API Governance Report**.
2. Read the three summary panels to decide focus: the lowest Category Average is the priority, and the top entry in Top Violated Rules is the fix that moves the most scores.
3. In the **Scanned APIs** table, sort by **Score** ascending to surface the worst offenders, then click a title to open its drilldown.
4. In the per-API **Findings** table, sort by **Severity** descending and work the Errors first. Click a **Path** value to copy the JSON pointer to the spec location.
5. Click a **Rule** name to open the side panel and read the compliant example under **How to fix it**.
6. Edit the spec at the named path, apply the fix, and save (push through your gateway release process if the spec lives there).
7. Click **Re-run scan** on the drilldown to re-score that one API and confirm the finding clears.
8. Use **Scan All** on the report to re-score the whole catalog after a ruleset change or a large import.

![Figure. The Bulk API Governance Scan page with the per-API progress indicator.](.gitbook/assets/screenshots/provider/admin-api-gov-report-scan-all.png)

## Verify

- After **Re-run scan**, confirm the Score header refreshes and the targeted finding no longer appears in the Findings table.
- Confirm the score moved in the expected direction and the violation count for the fixed rule drops.
- After **Scan All**, confirm the progress indicator advances row by row, the page returns to the report with refreshed panels, and the spot-checked API's Last scanned timestamp matches the run.

## Options

The ruleset is fully configurable, not fixed. Rules are grouped into four categories (Security, Documentation, Naming, Operations), and each can be enabled, disabled, or re-weighted between Error, Warning, and Info severity. Tuning the ruleset is a separate task: see [Governance rules](feat-governance-rules.md). Re-scanning happens automatically when a spec changes; use **Scan All** for ruleset changes, first-time imports, or recovery from suspected drift.

{% hint style="info" %}
**Note:** The marketplace does not block publication on score. The bar is policy agreed with your API guild, not a hard gate in code.
**Result:** Each API has a current score, a severity-ranked finding list, and a clear path from violation to cleared score.
{% endhint %}