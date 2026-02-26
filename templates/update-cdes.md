# Template: `update-cdes`

[Back to Command Reference](../purview-import.md#update-cdes)

Update existing critical data elements in the Microsoft Purview instance. The CSV structure is the same as `add-cdes`, but the `id` column is required so the tool knows which CDE to update.

The recommended workflow is to use `list-cdes --file cdes.csv` to export existing CDEs, modify the file, and pass it back to this command.

Generate this template with:

```
purview-import update-cdes --template > cdes.csv
```

## Template

```csv
businessdomain,id,name,description,status,datatype,owners
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| businessdomain | Yes | The governance domain name or ID. |
| id | Yes | The CDE ID (GUID). Required for identifying which CDE to update. |
| name | Yes | The name of the critical data element. |
| description | No | A description of the critical data element. |
| status | Yes | `Published`, `Draft`, or `Expired` |
| datatype | Yes | `Number`, `Text`, or `DateTime` |
| owners | No | Comma-separated list of owners. Supports key/value format. |
