# Template: `create-asset-relationship`

[Back to Command Reference](../purview-import.md#create-asset-relationship)

Create parent/child (container/contained) relationships between assets in the Azure Purview (Atlas) catalog. Each row defines one relationship by specifying the parent (container) and child (contained) asset GUIDs.

Generate this template with:

```
purview-import create-asset-relationship --template > relationships.csv
```

## Template

```csv
parent,child
550e8400-e29b-41d4-a716-446655440000,660e8400-e29b-41d4-a716-446655440001
```
