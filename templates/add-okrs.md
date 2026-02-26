# Template: `add-okrs`

[Back to Command Reference](../purview-import.md#add-okrs)

Add OKRs (Objectives and Key Results) to a governance domain in the Microsoft Purview instance. Each row creates one OKR objective.

Generate this template with:

```
purview-import add-okrs --template > okrs.csv
```

## Template

```csv
businessdomain,id,name,status,targetdate,owners
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| businessdomain | Yes | The governance domain name or ID. |
| id | No | Leave blank for new OKRs. Used by list-okrs for exports. |
| name | Yes | The name of the OKR objective. |
| status | Yes | `Published`, `Draft`, or `Expired` |
| targetdate | No | Target completion date in `YYYY-MM-DD` format. |
| owners | No | Comma-separated list of owners. Supports key/value format. |
