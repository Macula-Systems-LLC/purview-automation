# Template: `add-data-product-terms`

[Back to Command Reference](../purview-import.md#add-data-product-terms)

Associate glossary terms with data products. Both entities must already exist. You can identify them by ID directly, or by name + governance domain for automatic lookup.

If you provide an ID, the name and business domain columns for that entity are ignored. If you leave the ID blank, the tool looks up the entity using the name and business domain columns.

Generate this template with:

```
purview-import add-data-product-terms --template > dp_terms.csv
```

## Template

```csv
dataproductid,dataproductbusinessdomain,dataproductname,termid,termbusinessdomain,termname
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| dataproductid | No* | The data product ID (GUID). If blank, lookup uses name + domain. |
| dataproductbusinessdomain | No* | The governance domain of the data product. Used for lookup when ID is blank. |
| dataproductname | No* | The name of the data product. Used for lookup when ID is blank. |
| termid | No* | The glossary term ID (GUID). If blank, lookup uses name + domain. |
| termbusinessdomain | No* | The governance domain of the term. Used for lookup when ID is blank. |
| termname | No* | The name of the glossary term. Used for lookup when ID is blank. |

\* Provide either the ID, or both the name and business domain for each entity.
