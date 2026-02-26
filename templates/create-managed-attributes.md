# Template: `create-managed-attributes`

[Back to Command Reference](../purview-import.md#create-managed-attributes)

Create managed attribute group definitions in the Azure Purview (Atlas) instance. Managed attributes let you add custom metadata fields to assets. Supported types: `string`, `int`, `boolean`, `date`, `choice` (single-select), and `multi-choice` (multi-select).

Choice and multi-choice types reference enum definitions created with `create-managed-attribute-enum`. The `typeName` for choice attributes should match the enum name. For multi-choice, wrap it in `array<>` (e.g., `array<ATTRIBUTE_ENUM_YOURCHOICES>`).

The `applicableEntityTypes` option controls which asset types the attribute can be applied to.

Generate this template with:

```
purview-import create-managed-attributes --template > managed_attributes.json
```

## Template

```json
{
  "businessMetadataDefs": [
    {
      "category": "BUSINESS_METADATA",
      "name": "MyAttributeGroup",
      "description": "A group of managed attributes for custom metadata.",
      "attributeDefs": [
        {
          "name": "text_attr",
          "typeName": "string",
          "description": "A free-text attribute.",
          "isOptional": true,
          "cardinality": "SINGLE",
          "isUnique": false,
          "isIndexable": true,
          "options": {
            "maxStrLength": "500",
            "applicableEntityTypes": "[\"DataSet\",\"azure_sql_table\"]"
          }
        },
        {
          "name": "number_attr",
          "typeName": "int",
          "description": "A numeric attribute.",
          "isOptional": true,
          "cardinality": "SINGLE",
          "isUnique": false,
          "isIndexable": true,
          "options": {
            "applicableEntityTypes": "[\"DataSet\"]"
          }
        },
        {
          "name": "boolean_attr",
          "typeName": "boolean",
          "description": "A true/false attribute.",
          "isOptional": true,
          "cardinality": "SINGLE",
          "isUnique": false,
          "isIndexable": true,
          "options": {
            "applicableEntityTypes": "[\"DataSet\"]"
          }
        },
        {
          "name": "date_attr",
          "typeName": "date",
          "description": "A date attribute.",
          "isOptional": true,
          "cardinality": "SINGLE",
          "isUnique": false,
          "isIndexable": true,
          "options": {
            "applicableEntityTypes": "[\"DataSet\"]"
          }
        },
        {
          "name": "choice_attr",
          "typeName": "ATTRIBUTE_ENUM_YOURCHOICES",
          "description": "A single-choice attribute. Create the enum first with create-managed-attribute-enum.",
          "isOptional": true,
          "cardinality": "SINGLE",
          "isUnique": false,
          "isIndexable": true,
          "options": {
            "applicableEntityTypes": "[\"DataSet\"]"
          }
        },
        {
          "name": "multi_choice_attr",
          "typeName": "array<ATTRIBUTE_ENUM_YOURCHOICES>",
          "description": "A multi-choice attribute. Create the enum first with create-managed-attribute-enum.",
          "isOptional": true,
          "cardinality": "SINGLE",
          "isUnique": false,
          "isIndexable": true,
          "options": {
            "applicableEntityTypes": "[\"DataSet\"]"
          }
        }
      ]
    }
  ]
}
```
