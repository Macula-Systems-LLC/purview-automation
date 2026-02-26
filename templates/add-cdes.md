# Template: `add-cdes`

[Back to Command Reference](../purview-import.md#add-cdes)

Add critical data elements to a governance domain in the Microsoft Purview instance. Each row creates one CDE.

Generate this template with:

```
purview-import add-cdes --template > cdes.csv
```

## Template

```csv
businessdomain,id,name,description,status,datatype,owners
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| businessdomain | Yes | The governance domain name or ID. |
| id | No | Leave blank for new CDEs. Used by list-cdes for exports. |
| name | Yes | The name of the critical data element. |
| description | No | A description of the critical data element. |
| status | Yes | `Published`, `Draft`, or `Expired` |
| datatype | Yes | `Number`, `Text`, or `DateTime` |
| owners | No | Comma-separated list of owners. Supports key/value format. |
