---
icon: list-check
---

Governance rules are the linting rulesets that produce each API's governance score. Tune them when your team's API style guide changes, when a rule generates false positives, or when a new compliance requirement arrives. This is the configuration side of [API governance](feat-api-governance.md): that page reads the scores and findings, this one sets the rules and thresholds that produce them.

![Figure. API Governance Settings with score deductions, publish threshold, and rule-set tabs.](.gitbook/assets/screenshots/provider/admin-config-apim-api-linting.png)

## What you configure

The page splits into two areas: the score-deduction controls and publish threshold at the top, and the per-rule tabs below. The settings you set are:

- **Error deduction**: points removed from an API's score for each Error-severity violation. Set high so a single Error has visible impact on the score.
- **Warning deduction**: points removed per Warning-severity violation. Lower than Error.
- **Info deduction**: points removed per Info-severity violation. A small impact for advisory findings.
- **Hint deduction**: points removed per Hint-severity violation. The smallest impact, used for stylistic suggestions.
- **Publish threshold**: a slider from 0 to 100. An API scoring below this value cannot be published until a Provider clears the violations or an admin overrides the gate.
- **Rule tabs**: the rules grouped into three pillars, **Security**, **Style**, and **Documentation**. Each tab lists the rules in that pillar with the current severity and an enable/disable toggle per row.

## Options

Each rule carries one of four severities, ordered by score impact: **Error** (highest), **Warning**, **Info**, then **Hint** (lowest). Rules group by pillar: Security covers authentication, transport, and data exposure; Style covers naming, casing, and response shape; Documentation covers descriptions, examples, and summaries.

## Configure

1. From the left sidebar, expand **SETTINGS** and click **API Governance Settings**.
2. Set the **Error**, **Warning**, **Info**, and **Hint** deduction fields to control how many points each severity removes from a score.
3. Drag the **Publish threshold** slider to set the minimum score an API needs before it can be published.
4. Click the **Security**, **Style**, or **Documentation** tab for the pillar that holds the rule you want.
5. Find the rule in the list. Each row shows the name, current severity, and an enable/disable toggle.
6. Change the severity from the row dropdown (**Error**, **Warning**, **Info**, or **Hint**), or flip the **Enabled** toggle off to retire a rule.
7. Click **Save** at the bottom of the page.
8. Re-scan an API to apply the change. Existing scan results are not re-scored automatically.

## Per-Organisation override

A default ruleset applies to every Organisation. To run a different standard for one Organisation, open its settings page from the **Organisations** list, open **Governance overrides**, toggle on **Use custom ruleset**, adjust the rules that should differ, and save. Every rule you leave unchanged falls through to the portal default, so future portal-default tightening still reaches the un-overridden rules for that Organisation. Use this for a legacy API exemption, a partner-imposed schema, or one Organisation running a tighter standard than the rest of the portal.

## Verify

- Reload the page and confirm the rule's severity and Enabled state hold the values you saved.
- Re-scan a known API and confirm the new severity is reflected in its score.
- Confirm any API whose score now falls below the **Publish threshold** is gated from publishing.
- After an override, re-scan an API owned by that Organisation and confirm it reflects the custom ruleset, while an API in another Organisation scores unchanged.

{% hint style="success" %}
**Tip:** When you tighten a rule, re-run the governance scan on every published API the next morning. You will see which APIs newly fail the gate and need attention.
**Result:** Every governance scan that runs after you save evaluates APIs against the updated ruleset and threshold.
{% endhint %}