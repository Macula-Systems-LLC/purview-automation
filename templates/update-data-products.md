# Template: `update-data-products`

[Back to Command Reference](../purview-import.md#update-data-products)

Update existing data products in the Microsoft Purview instance. The CSV structure is the same as `add-data-products`, but the `id` column is required so the tool knows which data product to update.

The recommended workflow is to use `list-data-products --file products.csv` to export existing data products, modify the file, and pass it back to this command.

Generate this template with:

```
purview-import update-data-products --template > data_products.csv
```

## Template

```csv
businessdomain,id,name,description,status,dptype,businessuse,updatefrequency,endorsed,owners,assetids,audience
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| businessdomain | Yes | The governance domain name or ID. |
| id | Yes | The data product ID (GUID). Required for identifying which product to update. |
| name | Yes | The name of the data product. |
| description | No | A description of the data product. |
| status | Yes | `Published`, `Draft`, or `Expired` |
| dptype | Yes | `Dataset`, `MasterDataAndReferenceData`, `BusinessSystemOrApplication`, `ModelTypes`, `DashboardsOrReports`, or `Operational` |
| businessuse | No | Description of the business use case for this data product. |
| updatefrequency | No | `Daily`, `Weekly`, `Monthly`, `Quarterly`, `Yearly`, or blank. |
| endorsed | No | `true` or `false`. Whether the data product is endorsed. |
| owners | No | Comma-separated list of owners. Supports key/value format. |
| assetids | No | Comma-separated list of data asset GUIDs to associate. |
| audience | No | The intended audience for the data product. |
