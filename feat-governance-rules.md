---
icon: list-check
---

Governance rules are the linting rulesets that produce each API's governance score. Tune them when your API style guide changes, a rule generates false positives, or a new compliance requirement arrives.

![Figure. API Governance Settings with score deductions, publish threshold, and rule-set tabs.](.gitbook/assets/screenshots/provider/admin-config-apim-api-linting.png)

## Configure

1. From the left sidebar, expand **SETTINGS** and click **API Governance Settings**.
2. Set the **Error**, **Warning**, **Info**, and **Hint** deduction fields to control how many points each severity removes from a score.
3. Drag the **Publish threshold** slider to set the minimum score an API needs before it can be published.
4. Click the **Security**, **Style**, or **Documentation** tab for the pillar that holds the rule you want.
5. Find the rule in the list. Each row shows the name, current severity, and an enable/disable toggle.
6. Change the severity from the row dropdown (**Error**, **Warning**, **Info**, or **Hint**), or flip the **Enabled** toggle off to retire a rule.
7. Click **Save** at the bottom of the page.
8. Re-scan an API to apply the change. Existing scan results are not re-scored automatically.

{% hint style="success" %}
**Result:** Every governance scan that runs after you save evaluates APIs against the updated ruleset and threshold.
**Tip:** After tightening a rule, re-run the scan on every published API to see which ones newly fail the gate.
{% endhint %}

## Per-Organisation override

A default ruleset applies to every Organisation. To run a different standard for one Organisation, open its settings page from the **Organisations** list, open **Governance overrides**, toggle on **Use custom ruleset**, adjust the rules that should differ, and save. Every rule you leave unchanged falls through to the portal default.