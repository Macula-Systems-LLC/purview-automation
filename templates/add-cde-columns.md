# Template: `add-cde-columns`

[Back to Command Reference](../purview-import.md#add-cde-columns)

Associate data asset columns with critical data elements. The CDEs must already exist. Each row links one column from a data asset to a CDE.

The column's parent data asset can be identified in one of three ways (provide exactly one per row):

1. **`assetid` + `columnname`** — the Atlas GUID of the table asset plus the column name.
2. **`collectionname` + `tablename` + `columnname`** — the collection, table, and column name; the tool resolves the asset GUID.
3. **`fqn`** — the fully qualified name of the column; the tool queries the data map to resolve both the column and its parent asset.

Generate this template with:

```
purview-import add-cde-columns --template > cde_columns.csv
```

## Template

```csv
businessdomain,id,cdename,assetid,fqn,collectionname,tablename,columnname
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| businessdomain | Yes | The governance domain name or ID of the CDE. |
| id | No* | The CDE ID (GUID). If blank, lookup uses cdename + domain. |
| cdename | No* | The name of the CDE. Used for lookup when ID is blank. |
| assetid | No** | The data asset GUID (table-level) that contains the column. Must be a valid GUID. |
| fqn | No** | The fully qualified name of the column. Takes precedence — the tool resolves the column and its parent asset from the data map. |
| collectionname | No** | The collection containing the table. Used with `tablename`. |
| tablename | No** | The table/asset name within the collection. Used with `collectionname`. |
| columnname | No** | The column name within the asset. Required when using `assetid` or `collectionname`/`tablename`; derived automatically when using `fqn`. |

\* Provide either the `id`, or the `cdename` + `businessdomain` for the CDE.

\*\* Provide one of three asset identification methods: `fqn` alone; `assetid` + `columnname`; or `collectionname` + `tablename` + `columnname`.
