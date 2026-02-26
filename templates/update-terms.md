# Template: `update-terms`

[Back to Command Reference](../purview-import.md#update-terms)

Update existing glossary terms in the Microsoft Purview instance. The CSV structure is the same as `add-terms`, but the `id` column is required so the tool knows which term to update.

The recommended workflow is to use `list-terms --file terms.csv` to export existing terms, modify the file, and pass it back to this command.

**NOTE**: This will not change the term's governance domain.

Generate this template with:

```
purview-import update-terms --template > terms.csv
```

## Template

```csv
businessdomain,id,term,description,status,parent,owners,experts,acronyms,resources
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| businessdomain | Yes | The governance domain name or ID the term belongs to. |
| id | Yes | The term ID (GUID). Required for identifying which term to update. |
| term | Yes | The name of the glossary term. |
| description | No | A description of the glossary term. |
| status | Yes | `Published`, `Draft`, or `Expired` |
| parent | No | The name of a parent term (for hierarchical terms). |
| owners | No | Comma-separated list of owners. Supports key/value format. |
| experts | No | Comma-separated list of experts. Supports key/value format. |
| acronyms | No | Comma-separated list of acronyms for the term. |
| resources | No | Key/value pairs where key is the resource name and value is the URL. |
