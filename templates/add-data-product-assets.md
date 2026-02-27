# Template: `add-data-product-assets`

[Back to Command Reference](../purview-import.md#add-data-product-assets)

Associate data assets with data products. The data products must already exist. You can identify data products by ID directly, or by name + governance domain for automatic lookup.

The data asset can be identified in one of three ways (provide exactly one per row):

1. **`dataassetid`** — the Atlas GUID of the asset.
2. **`collectionname` + `tablename`** — the collection and table name; the tool resolves the GUID.
3. **`fqn`** — the fully qualified name of the asset; the tool queries the data map to resolve the GUID.

Generate this template with:

```
purview-import add-data-product-assets --template > dp_assets.csv
```

## Template

```csv
dataproductid,dataproductbusinessdomain,dataproductname,dataassetid,fqn,collectionname,tablename
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| dataproductid | No* | The data product ID (GUID). If blank, lookup uses name + domain. |
| dataproductbusinessdomain | No* | The governance domain of the data product. Used for lookup when ID is blank. |
| dataproductname | No* | The name of the data product. Used for lookup when ID is blank. |
| dataassetid | No** | The Atlas GUID of the data asset. Must be a valid GUID. |
| fqn | No** | The fully qualified name of the asset. The tool queries the data map to resolve the GUID. |
| collectionname | No** | The collection containing the table. Used with `tablename`. |
| tablename | No** | The table/asset name within the collection. Used with `collectionname`. |

\* Provide either the `dataproductid`, or both `dataproductname` and `dataproductbusinessdomain`.

\*\* Provide one of three asset identification methods: `dataassetid` alone; `fqn` alone; or `collectionname` + `tablename`.
