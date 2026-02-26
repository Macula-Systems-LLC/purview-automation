# Template: `add-okr-dp`

[Back to Command Reference](../purview-import.md#add-okr-dp)

Associate data products with OKRs in the Microsoft Purview instance. Both entities must already exist and are identified by name and governance domain.

Generate this template with:

```
purview-import add-okr-dp --template > okr_dp.csv
```

## Template

```csv
okrbusinessdomain,okrname,dataproductbusinessdomain,dataproductname
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| okrbusinessdomain | Yes | The governance domain name or ID of the OKR. |
| okrname | Yes | The name of the OKR objective. |
| dataproductbusinessdomain | Yes | The governance domain name or ID of the data product. |
| dataproductname | Yes | The name of the data product. |
