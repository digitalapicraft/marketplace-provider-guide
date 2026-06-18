---
icon: list-check
---

Governance rules are the linting rules that produce each API's governance score. The **API Governance Settings** page is where you turn rules on, turn them off, and re-weight their severity to match your organisation's API standards. Tune them when your team's style guide changes, when a rule generates false positives, or when a new compliance requirement arrives. This is the configuration side of [API governance](feat-api-governance.md): that page reads the scores and findings, this one decides which rules run.

![Figure. API Governance Settings, with the scoring summary and the three rule-category tabs.](.gitbook/assets/screenshots/provider/admin-config-apim-api-linting.png)

## How scoring works

The overall score is the average of three category scores, each marked out of 100:

**Overall score = (Security + Style Guidelines + Documentation) / 3**

You do not set point values by hand. You decide which rules are active in each category, and the category score reflects how an API does against the rules left enabled in that category. Each finding a rule raises carries a severity of **Error**, **Warning**, or **Info**, and severity decides how heavily a violation weights the category score: an Error costs far more than an Info.

The averaged score lands in one of four bands on the report:

- **Excellent**: 80 and above.
- **Good**: 60 to 79.
- **Fair**: 40 to 59.
- **Poor**: below 40.

{% hint style="info" %}
**Note:** The score is a policy bar, not a hard publish gate. Disabling a rule lifts the score of every API that was violating it; enabling one drops the score of every API that violates the new rule. Neither action blocks publishing. Agree the bar with your API guild, then re-scan to apply a change.
{% endhint %}

## What you configure

The settings page opens on an **Overview** tab and adds one tab per category. The three categories are fixed:

- **Security (OWASP).** The OWASP API Security Top 10 (2023) rules for authentication, authorization, transport security, and data protection. The tab header shows how many are active, for example *11 of 31 rules enabled*.
- **Style Guidelines.** API style conventions: URL patterns, HTTP methods, casing, and response structures. For example *10 of 18 rules enabled*.
- **Documentation.** Completeness rules for descriptions, examples, summaries, and the top-level info metadata. For example *10 of 13 rules enabled*.

Inside a category tab, each rule is one row showing:

- **Rule name and description**: the rule name plus a one-line plain-text description of what triggers a violation.
- **Severity**: an **Error**, **Warning**, or **Info** value, set per rule. This drives the score weight applied to any violation.
- **Enabled toggle**: the on-off switch. A disabled rule does not run on the next scan, raises no findings, and stops affecting that category's score.

The **Overview** tab also carries a **Scoring Configuration** section that controls how the three category averages combine into the overall score, and the **Save configuration** button that persists every change on the page.

### What each category checks

| Category | What it checks | Example rules |
|---|---|---|
| Security (OWASP) | Authentication declarations, scope definitions, transport security, data exposure. | `security-defined`, `securitySchemes-present`, `oauth2-scopes-required`. |
| Style Guidelines | Casing on paths, parameters, and schema properties; HTTP method and response-shape conventions. | `path-kebab-case`, `parameter-camelCase`, `schema-property-camelCase`. |
| Documentation | Presence of descriptions, examples, summaries, and the top-level info block. | `info-contact`, `operation-description`, `parameter-description`, `tag-description`. |

## Configure rules

1. From the left sidebar, expand **SETTINGS**, then click **API Governance Settings**. The page opens on the **Overview** tab.
2. Read the scoring summary. Open **Scoring Configuration** if you need to adjust how the three categories combine into the overall score.
3. Click the **Security (OWASP)**, **Style Guidelines**, or **Documentation** tab to open that category's rule list.
4. Find the rule you want to change. Each row shows the name, description, severity, and the **Enabled** toggle.
5. To disable a rule, click its **Enabled** toggle off. The row dims, and the rule skips every API on the next scan.
6. To enable a previously disabled rule, click the toggle on. The row brightens, and the rule fires on the next scan.
7. To re-weight a rule, pick a new value from its **Severity** control: **Error**, **Warning**, or **Info**.
8. Click **Save configuration**. A confirmation banner appears.
9. Re-scan to apply the change. Existing results are not re-scored automatically.

{% hint style="warning" %}
**Caution:** When tightening a rule (raising severity or enabling a previously disabled one), notify API teams before you run **Scan All**. Otherwise scores drop overnight without explanation and the team spends the next morning asking what changed.
{% endhint %}

## Adjust severity for a noisy rule

Use this when a rule fires across many APIs but the impact does not warrant treating each finding as an Error.

1. On the rule's category tab, open its **Severity** control.
2. Lower the value, for example Error to Warning, or Warning to Info.
3. Click **Save configuration**.
4. Run **Scan All** from the Governance Report and read the Score Distribution histogram. It should shift right as the rule contributes less weight.

{% hint style="success" %}
**Tip:** Reserve **Error** for security and contract correctness, and use **Info** for stylistic rules such as casing or summary presence. Lowering severity is not the same as disabling: the finding still appears in the drilldown, it costs less score. A common starting profile is a handful of Errors, a band of Warnings, and the rest Info.
{% endhint %}

## Roll back a rule change

Use this when a recent change has caused unintended score drops that need reversing.

1. On **API Governance Settings**, open the tab for the rule you changed.
2. Reverse the change: flip the toggle back, or pick the prior severity.
3. Click **Save configuration**.
4. Run **Scan All** to refresh scores against the restored ruleset.

{% hint style="warning" %}
**Caution:** The marketplace does not version the ruleset, so roll back is manual. Keep a changelog of rule edits outside the marketplace (a team wiki or shared doc) so a reverse is always a copy-paste away.
{% endhint %}

## Re-scan after a ruleset change

Saving the configuration changes the policy, but scores update only when a scan runs against the new rules.

1. Save your rule changes on **API Governance Settings**.
2. Open the **API Governance Report** from the sidebar.
3. Click **Scan All** to re-score the whole catalog against the new ruleset, or open a single API's drilldown and click **Re-run scan** to re-score one API.

The full re-scan flow, including the progress indicator and the run summary, is covered in [API governance](feat-api-governance.md).

{% hint style="warning" %}
**Caution:** Disabling rules to game the score does not improve API quality. Use the ruleset to encode your real standards. If a rule fires repeatedly across many APIs, the right fix is almost always in the specs, not in the rule.
{% endhint %}

## Verify

- Reload the page and confirm each category's enabled-rule count matches what you saved (for example *11 of 31 rules enabled*).
- Confirm the **Scoring Configuration** reflects any change you made to how the categories combine.
- Re-scan a known API and confirm a rule you disabled no longer appears in its findings, and that the category score moves as expected.
- Run **Scan All** after the change so every API is re-scored against the new configuration, then spot-check the Category Averages panel on the report.

## Related

- [API governance](feat-api-governance.md) reads the scores and findings the ruleset produces, and covers the full Scan All flow.
- [Publishing APIs](feat-publishing-apis.md) moves an API from Draft to Published once its score meets the bar this ruleset defines.
- [API Products & Plans](feat-products-and-plans.md) groups scored APIs into a subscribable Product.

{% hint style="success" %}
**Result:** Every governance scan that runs after you save evaluates APIs against the rules you left enabled, at the severities you set, and the average of the three category scores reflects the updated ruleset.
{% endhint %}