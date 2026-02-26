# Template: `add-key-results`

[Back to Command Reference](../purview-import.md#add-key-results)

Add key results to existing OKRs in the Microsoft Purview instance. Each row creates one key result linked to an OKR by name and governance domain.

Generate this template with:

```
purview-import add-key-results --template > key_results.csv
```

## Template

```csv
businessdomain,okrname,id,name,status,progress,goal,max
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| businessdomain | Yes | The governance domain name or ID of the parent OKR. |
| okrname | Yes | The name of the parent OKR this key result belongs to. |
| id | No | Leave blank for new key results. Used by list-key-results for exports. |
| name | Yes | The name of the key result. |
| status | Yes | `NotTrack`, `OnTrack`, `Behind`, or `AtRisk` |
| progress | No | Current progress value (numeric). |
| goal | No | Target goal value (numeric). |
| max | No | Maximum possible value (numeric). |
