---
icon: list-check
---

Governance rules are the linting rules that produce each API's governance score. Tune them when your team's API style guide changes, when a rule generates false positives, or when a new compliance requirement arrives. This is the configuration side of [API governance](feat-api-governance.md): that page reads the scores and findings, this one decides which rules run.

![Figure. API Governance Settings, with the scoring summary and the three rule-category links.](.gitbook/assets/screenshots/provider/admin-config-apim-api-linting.png)

## How scoring works

The overall score is a simple average of three category scores, each out of 100:

**Overall score = (Security + Style + Documentation) / 3**

You do not set point values by hand. You decide which rules are active in each category, and the category score reflects how an API does against its enabled rules. Each finding a rule raises carries a severity of **Error**, **Warning**, or **Info**.

## What you configure

The settings page opens on an **Overview** tab and has one tab per category. The three categories are:

- **Security (OWASP).** The OWASP API Security Top 10 (2023) rules for authentication, authorization, and data protection. The tab header shows how many are on, for example *11 of 31 rules enabled*.
- **Style Guidelines.** API style conventions: URL patterns, HTTP methods, and response structures. For example *10 of 18 rules enabled*.
- **Documentation.** Completeness rules for descriptions, examples, and metadata. For example *10 of 13 rules enabled*.

Inside a category tab, each rule row shows the rule name, its severity, and an **Enabled** toggle. The **Scoring Configuration** section on the Overview tab controls how the category averages combine into the overall score.

## Configure

1. From the left sidebar, expand **SETTINGS** and click **API Governance Settings**.
2. On the **Overview** tab, read the scoring summary and open **Scoring Configuration** if you need to adjust how the categories combine.
3. Click the **Security (OWASP)**, **Style Guidelines**, or **Documentation** category to open its rule list.
4. Find the rule you want. Each row shows the name, severity, and an **Enabled** toggle.
5. Turn a rule on or off with its toggle. Disabling a rule stops it from raising findings and from affecting that category's score.
6. Click **Save configuration** at the top of the page.
7. Re-scan an API to apply the change. Existing scan results are not re-scored automatically; use **Scan All** on the Governance Report to re-score the catalog.

## Verify

- Reload the page and confirm each category's enabled-rule count matches what you saved.
- Re-scan a known API and confirm a rule you disabled no longer appears in its findings, and that the category score moves as expected.
- Run **Scan All** after a ruleset change so every published API is re-scored against the new configuration.

{% hint style="info" %}
**Note:** Changing the ruleset does not retroactively re-score APIs and does not block any API from publishing. Treat the score as the quality bar your API guild agrees on, then re-scan to apply a change.
**Result:** Every governance scan that runs after you save evaluates APIs against the rules you left enabled, and the average of the three category scores reflects the updated ruleset.
{% endhint %}