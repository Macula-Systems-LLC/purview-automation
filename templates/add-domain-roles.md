# Template: `add-domain-roles`

[Back to Command Reference](../purview-import.md#add-domain-roles)

Assign users or groups to governance domain roles. Each row specifies a principal (user or group) and which roles to assign by placing an `x` in the corresponding column. Leave a role column blank to skip that role.

Generate this template with:

```
purview-import add-domain-roles --template > domain_roles.csv
```

## Template

```csv
principal,domainowner,domainreader,dataproductowner,datasteward,localcatalogreader,dataqualitysteward,dataqualityreader,dataqualitymetadatareader,dataprofilesteward,dataprofilereader
```

## Column Reference

| Column | Description |
|--------|-------------|
| principal | Entra Object ID or email address of the user or group. |
| domainowner | Place `x` to assign the Domain Owner role. |
| domainreader | Place `x` to assign the Domain Reader role. |
| dataproductowner | Place `x` to assign the Data Product Owner role. |
| datasteward | Place `x` to assign the Data Steward role. |
| localcatalogreader | Place `x` to assign the Local Catalog Reader role. |
| dataqualitysteward | Place `x` to assign the Data Quality Steward role. |
| dataqualityreader | Place `x` to assign the Data Quality Reader role. |
| dataqualitymetadatareader | Place `x` to assign the Data Quality Metadata Reader role. |
| dataprofilesteward | Place `x` to assign the Data Profile Steward role. |
| dataprofilereader | Place `x` to assign the Data Profile Reader role. |
