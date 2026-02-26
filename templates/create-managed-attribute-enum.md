# Template: `create-managed-attribute-enum`

[Back to Command Reference](../purview-import.md#create-managed-attribute-enum)

Create enum definitions for managed attribute choice and multi-choice fields in the Azure Purview (Atlas) instance. Each enum defines a set of allowed values that can be referenced by managed attribute definitions created with `create-managed-attributes`.

The `enumDefs` array can contain multiple enum definitions. Each definition has a `name`, optional `description`, and an `elementDefs` array of values with ordinals.

Generate this template with:

```
purview-import create-managed-attribute-enum --template > enums.json
```

## Template

```json
{
  "enumDefs": [
    {
      "name": "ATTRIBUTE_ENUM_STATUS",
      "description": "Status values for assets.",
      "elementDefs": [
        { "value": "Active", "ordinal": 0 },
        { "value": "Inactive", "ordinal": 1 },
        { "value": "Pending", "ordinal": 2 }
      ]
    },
    {
      "name": "ATTRIBUTE_ENUM_REGION",
      "description": "Geographic regions.",
      "elementDefs": [
        { "value": "US", "ordinal": 0 },
        { "value": "EU", "ordinal": 1 },
        { "value": "APAC", "ordinal": 2 }
      ]
    }
  ]
}
```
