# Template: `create-dataset-entities`

[Back to Command Reference](../purview-import.md#create-dataset-entities)

Create custom dataset entities (tables and columns) in the Azure Purview (Atlas) instance. Entities are instances of the type definitions created with `create-dataset-definition`.

Key points about this template:

- Use negative GUIDs (e.g., `"-1"`, `"-11"`) as temporary placeholders. Atlas will assign real GUIDs on creation.
- The `relationshipAttributes` section links columns to their parent table using these temporary GUIDs.
- The `businessAttributes` section is optional and lets you set managed attribute values.
- The `contacts` section is optional and lets you assign Owner and Expert contacts by their Entra Object ID.

Generate this template with:

```
purview-import create-dataset-entities --template > dataset_entities.json
```

## Template

```json
{
  "entities": [
    {
      "typeName": "macula_table",
      "attributes": {
        "qualifiedName": "mac.table1",
        "name": "mac.table1",
        "description": "Customer orders table from the legacy ERP system containing order history and customer relationships.",
        "sourceSystem": "Legacy ERP",
        "refreshFrequency": "Daily",
        "hasPII": true
      },
      "businessAttributes": {
        "YourAttributeGroupName": {
          "your_attribute_1": "table-level value",
          "your_attribute_2": "table-level value 2"
        }
      },
      "contacts": {
        "Owner": [
          { "id": "00000000-0000-0000-0000-000000000001", "info": "Data Team Lead" }
        ],
        "Expert": [
          { "id": "00000000-0000-0000-0000-000000000002", "info": "Domain Expert" },
          { "id": "00000000-0000-0000-0000-000000000003" }
        ]
      },
      "relationshipAttributes": {
        "columns": [
          { "guid": "-11", "typeName": "macula_column" },
          { "guid": "-12", "typeName": "macula_column" }
        ]
      },
      "guid": "-1"
    },
    {
      "typeName": "macula_column",
      "attributes": {
        "qualifiedName": "mac.table1#customer_id",
        "name": "customer_id",
        "description": "Unique identifier for each customer in UUID format.",
        "dataType": "string",
        "businessName": "Customer Identifier",
        "isPII": true,
        "format": "UUID"
      },
      "businessAttributes": {
        "YourAttributeGroupName": {
          "your_attribute_1": "column-level value",
          "your_attribute_2": "column-level value 2"
        }
      },
      "guid": "-11",
      "relationshipAttributes": {
        "table": {
          "guid": "-1",
          "typeName": "macula_table"
        }
      }
    },
    {
      "typeName": "macula_column",
      "attributes": {
        "qualifiedName": "mac.table1#order_count",
        "name": "order_count",
        "description": "Total number of orders placed by the customer.",
        "dataType": "int",
        "businessName": "Total Orders",
        "isPII": false,
        "format": "integer"
      },
      "guid": "-12",
      "relationshipAttributes": {
        "table": {
          "guid": "-1",
          "typeName": "macula_table"
        }
      }
    }
  ]
}
```
