# Template: `add-cde-terms`

[Back to Command Reference](../purview-import.md#add-cde-terms)

Associate glossary terms with critical data elements. Both entities must already exist. You can identify them by ID directly, or by name + governance domain for automatic lookup.

If you provide an ID, the name and business domain columns for that entity are ignored. If you leave the ID blank, the tool looks up the entity using the name and business domain columns.

Generate this template with:

```
purview-import add-cde-terms --template > cde_terms.csv
```

## Template

```csv
cdeid,cdebusinessdomain,cdename,termid,termbusinessdomain,termname
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| cdeid | No* | The CDE ID (GUID). If blank, lookup uses name + domain. |
| cdebusinessdomain | No* | The governance domain of the CDE. Used for lookup when ID is blank. |
| cdename | No* | The name of the CDE. Used for lookup when ID is blank. |
| termid | No* | The glossary term ID (GUID). If blank, lookup uses name + domain. |
| termbusinessdomain | No* | The governance domain of the term. Used for lookup when ID is blank. |
| termname | No* | The name of the glossary term. Used for lookup when ID is blank. |

\* Provide either the ID, or both the name and business domain for each entity.
