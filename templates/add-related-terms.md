# Template: `add-related-terms`

[Back to Command Reference](../purview-import.md#add-related-terms)

Create Related or Synonym relationships between glossary terms. Each row defines one directional relationship. The `relatedtermdomain` column is only required when the related term is in a different governance domain than the source term; if left blank, both terms are assumed to be in the same domain.

Generate this template with:

```
purview-import add-related-terms --template > related_terms.csv
```

## Template

```csv
termdomain,term,relatedtermdomain,relatedterm,relationshiptype,description
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| termdomain | Yes | The governance domain of the source term. |
| term | Yes | The name of the source term. Must exist in the specified domain. |
| relatedtermdomain | No | The governance domain of the related term. If blank, defaults to the same domain as `termdomain`. |
| relatedterm | Yes | The name of the related term. Must exist in the specified (or inferred) domain. |
| relationshiptype | Yes | The type of relationship: `Related` or `Synonym`. |
| description | No | Optional description for the relationship. |
