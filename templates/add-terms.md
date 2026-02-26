# Template: `add-terms`

[Back to Command Reference](../purview-import.md#add-terms)

Add glossary terms to a governance domain in the Microsoft Purview instance. Each row creates one glossary term.

The `owners`, `experts`, and `resources` columns support key/value pair formatting. See [Key/Value Pair Formatting](../purview-import.md#keyvalue-pair-formatting-in-csv-columns) for details.

Generate this template with:

```
purview-import add-terms --template > terms.csv
```

## Template

```csv
businessdomain,id,term,description,status,parent,owners,experts,acronyms,resources
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| businessdomain | Yes | The governance domain name or ID to add the term to. |
| id | No | Leave blank for new terms. Used by list-terms for exports. |
| term | Yes | The name of the glossary term. |
| description | No | A description of the glossary term. |
| status | Yes | `Published`, `Draft`, or `Expired` |
| parent | No | The name of a parent term (for hierarchical terms). |
| owners | No | Comma-separated list of owners. Supports key/value format. |
| experts | No | Comma-separated list of experts. Supports key/value format. |
| acronyms | No | Comma-separated list of acronyms for the term. |
| resources | No | Key/value pairs where key is the resource name and value is the URL. |
