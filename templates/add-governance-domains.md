# Template: `add-governance-domains`

[Back to Command Reference](../purview-import.md#add-governance-domains)

Add governance domains to the Microsoft Purview instance. Each row creates one governance domain. The `id` column is used by `list-governance-domains` only — leave it blank when adding new domains.

The `owners` column accepts a comma-separated list of Entra Object IDs or email addresses. See [Key/Value Pair Formatting](../purview-import.md#keyvalue-pair-formatting-in-csv-columns) for advanced owner formatting.

Generate this template with:

```
purview-import add-governance-domains --template > governance_domains.csv
```

## Template

```csv
id,parent,name,description,status,type,owners
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| id | No | Leave blank for new domains. Used by list-governance-domains for exports. |
| parent | No | Name of the parent governance domain, if this is a child domain. |
| name | Yes | The name of the governance domain. |
| description | No | A description of the governance domain. |
| status | Yes | `Published`, `Draft`, or `Expired` |
| type | Yes | `FunctionalUnit`, `LineOfBusiness`, `DataDomain`, `Regulatory`, or `Project` |
| owners | No | Comma-separated list of Entra Object IDs or email addresses. |
