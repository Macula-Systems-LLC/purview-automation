# Template: `update-dataset-definition`

[Back to Command Reference](../purview-import.md#update-dataset-definition)

Update existing custom dataset type definitions in the Azure Purview (Atlas) instance. The JSON structure is the same as `create-dataset-definition`. Uses PUT to replace the existing type definitions.

Generate this template with:

```
purview-import update-dataset-definition --template > dataset_definition.json
```

## Template

```json
{
  "entityDefs": [
    {
      "category": "ENTITY",
      "name": "macula_table",
      "typeVersion": "1.0",
      "superTypes": ["DataSet", "Purview_Table"],
      "attributeDefs": [
        {
          "name": "sourceSystem",
          "typeName": "string",
          "isOptional": true
        },
        {
          "name": "refreshFrequency",
          "typeName": "string",
          "isOptional": true
        },
        {
          "name": "hasPII",
          "typeName": "boolean",
          "isOptional": true
        }
      ],
      "options": {
        "schemaElementsAttribute": "columns"
      }
    },
    {
      "category": "ENTITY",
      "name": "macula_column",
      "typeVersion": "1.0",
      "superTypes": ["DataSet"],
      "attributeDefs": [
        {
          "name": "dataType",
          "typeName": "string",
          "isOptional": true
        },
        {
          "name": "businessName",
          "typeName": "string",
          "isOptional": true
        },
        {
          "name": "isPII",
          "typeName": "boolean",
          "isOptional": true
        },
        {
          "name": "format",
          "typeName": "string",
          "isOptional": true
        }
      ],
      "options": {
        "schemaAttributes": "[\"dataType\",\"businessName\",\"isJasonColumn\",\"format\"]"
      }
    }
  ],
  "relationshipDefs": [
    {
      "category": "RELATIONSHIP",
      "name": "macula_table_columns",
      "typeVersion": "1.0",
      "relationshipCategory": "COMPOSITION",
      "endDef1": {
        "type": "macula_table",
        "name": "columns",
        "isContainer": true,
        "cardinality": "SET"
      },
      "endDef2": {
        "type": "macula_column",
        "name": "table",
        "isContainer": false,
        "cardinality": "SINGLE"
      }
    }
  ]
}
```
