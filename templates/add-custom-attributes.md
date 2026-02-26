# Template: `add-custom-attributes`

[Back to Command Reference](../purview-import.md#add-custom-attributes)

Add custom attributes to the Microsoft Purview instance. Custom attributes extend the metadata schema for governance entities like glossary terms and data products. Use `-g/--group` on the command line to specify which attribute group the attributes belong to.

Generate this template with:

```
purview-import add-custom-attributes --template > custom_attributes.csv
```

## Template

```csv
attributename,description,required,type,choices
```

## Column Reference

| Column | Required | Description |
|--------|----------|-------------|
| attributename | Yes | The name of the custom attribute. |
| description | No | A description of the custom attribute. |
| required | No | `yes` or `no`. Whether the attribute is required when filling in metadata. |
| type | Yes | The data type: `string`, `int`, `boolean`, `date`, `choice`, or `multi-choice`. |
| choices | No | Semicolon-separated list of choices (only for `choice` or `multi-choice` types, e.g., `Choice1;Choice2;Choice3`). |
