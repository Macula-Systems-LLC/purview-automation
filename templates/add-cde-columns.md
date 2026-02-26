# Template: `add-cde-columns`

[Back to Command Reference](../purview-import.md#add-cde-columns)

Associate data asset columns with critical data elements. The CDEs must already exist. Each row links one column from a data asset to a CDE.

Generate this template with:

```
purview-import add-cde-columns --template > cde_columns.csv
```

## Template

```csv
businessdomain,id,cdename,assetid,columnname
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| businessdomain | Yes | The governance domain name or ID of the CDE. |
| id | No* | The CDE ID (GUID). If blank, lookup uses cdename + domain. |
| cdename | No* | The name of the CDE. Used for lookup when ID is blank. |
| assetid | Yes | The data asset GUID that contains the column. |
| columnname | Yes | The name of the column within the asset. |

\* Provide either the ID, or the cdename for the CDE.
