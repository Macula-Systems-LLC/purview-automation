# Template: `update-dataset-entities`

[Back to Command Reference](../purview-import.md#update-dataset-entities)

Update existing custom dataset entities (tables and columns) in the Azure Purview (Atlas) instance. Unlike create, entities here must use their real GUIDs (not placeholder negative IDs).

Key differences from `create-dataset-entities`:

- The `guid` field must contain the real GUID assigned by Atlas during creation.
- The `relationshipAttributes` section should be omitted — relationships are not modified during updates.
- The `businessAttributes` section is optional and lets you update managed attribute values.

Generate this template with:

```
purview-import update-dataset-entities --template > dataset_entities.json
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
        "description": "Updated description for the customer orders table.",
        "sourceSystem": "New ERP",
        "refreshFrequency": "Hourly",
        "hasPII": true
      },
      "businessAttributes": {
        "YourAttributeGroupName": {
          "your_attribute_1": "updated table-level value",
          "your_attribute_2": "updated table-level value 2"
        }
      },
      "contacts": {
        "Owner": [
          { "id": "00000000-0000-0000-0000-000000000001", "info": "Data Team Lead" }
        ],
        "Expert": [
          { "id": "00000000-0000-0000-0000-000000000002", "info": "Domain Expert" }
        ]
      }
    },
    {
      "typeName": "macula_column",
      "attributes": {
        "qualifiedName": "mac.table1#customer_id",
        "name": "customer_id",
        "description": "Updated description for the customer identifier column.",
        "dataType": "string",
        "businessName": "Customer Identifier",
        "isPII": true,
        "format": "UUID"
      },
      "businessAttributes": {
        "YourAttributeGroupName": {
          "your_attribute_1": "updated column-level value"
        }
      }
    }
  ]
}
```
