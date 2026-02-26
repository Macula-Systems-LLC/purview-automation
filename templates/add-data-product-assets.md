# Template: `add-data-product-assets`

[Back to Command Reference](../purview-import.md#add-data-product-assets)

Associate data assets with data products. The data products must already exist. You can identify data products by ID directly, or by name + governance domain for automatic lookup.

Generate this template with:

```
purview-import add-data-product-assets --template > dp_assets.csv
```

## Template

```csv
dataproductid,dataproductbusinessdomain,dataproductname,dataassetid
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| dataproductid | No* | The data product ID (GUID). If blank, lookup uses name + domain. |
| dataproductbusinessdomain | No* | The governance domain of the data product. Used for lookup when ID is blank. |
| dataproductname | No* | The name of the data product. Used for lookup when ID is blank. |
| dataassetid | Yes | The data asset GUID to associate with the data product. |

\* Provide either the ID, or both the name and business domain for the data product.
