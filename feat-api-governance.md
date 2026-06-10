---
icon: shield-halved
---

The marketplace scores every API spec against a configurable set of linting rules and produces a value between 0 and 100. The Governance Report shows the score, severity-ranked violations, and remediation guidance, so you can fix findings before consumers see the API.

![Figure. The API Governance Report dashboard with summary panels and the Scanned APIs table.](.gitbook/assets/screenshots/provider/admin-api-gov-report.png)

## Configure

1. From the left sidebar, expand **API MANAGEMENT**, then click **API Governance Report**.
2. Read the score breakdown across the summary panels: **Score Distribution** (catalog-wide histogram), **Category Averages** (mean score per Security, Documentation, Naming, Operations), and **Top Violated Rules** (the five rules triggered most often).
3. In the **Scanned APIs** table, sort by **Score** ascending to surface the worst offenders, then click a title to open its drilldown.
4. In the per-API **Findings** table, sort by **Severity** descending and work the Errors first. Click a **Path** value to copy the JSON pointer to the spec location.
5. Click a **Rule** name to open the side panel and read what it checks, why it matters, and the compliant example under **How to fix it**.
6. Edit the spec at the path shown, apply the fix, and save.
7. Click **Re-run scan** on the drilldown to re-score the single API and confirm the finding clears.

{% hint style="success" %}
**Result:** A single-screen view of governance health, plus a per-API path from finding to cleared score.
**Tip:** Use **Scan All** on the report to re-score the whole catalog after a ruleset change or a large import.
{% endhint %}

## Callouts

1. **Score Distribution.** Histogram of catalog-wide scores by band.
2. **Top Violated Rules.** The fix at the top usually moves the most scores.
3. **Scan All.** Re-scans every API against the current ruleset.
4. **Scanned APIs table.** Sortable per-API list with score and severity counts.