# Template: `add-data-products`

[Back to Command Reference](../purview-import.md#add-data-products)

Add data products to a governance domain in the Microsoft Purview instance. Each row creates one data product.

Generate this template with:

```
purview-import add-data-products --template > data_products.csv
```

## Template

```csv
businessdomain,id,name,description,status,dptype,businessuse,updatefrequency,endorsed,owners,assetids,audience
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| businessdomain | Yes | The governance domain name or ID. |
| id | No | Leave blank for new data products. Used by list-data-products for exports. |
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
