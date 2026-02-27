# Purview Bulk Import

Tool for importing, exporting, and managing entities in Microsoft Purview. This includes governance domains, glossary terms, data products, critical data elements (CDEs), OKRs, data assets, custom attributes, and more.

This tool is built for Windows and is a command line application.

## Getting Help

To view the help and list of all available commands, run:

```
purview-import -h
```

To view help for a specific command, run the command with the `-h` parameter:

```
purview-import list-assets -h
```

This will display the optional parameters and a description about what they are used for.

## Common Conventions

### The `--file` Parameter

Most commands have a `--file` parameter that indicates the file to be imported or exported depending on the direction the data is flowing based on the command. Export/list commands write output to the file; add/update commands read input from the file.

### The `--dry-run` Parameter

All "add-\*" and "update-\*" commands have a `--dry-run` parameter, which **defaults to true**. This means that the input will be initially validated, however no modifications will be made to the system. This is intentional as a safety mechanism. When you are ready to run the command and make changes to the destination system, use the flag `--dry-run false` to cause the new data to be sent to the destination system.

### The `--enums` Parameter

All "add-\*" commands also have a `--enums` parameter. When the command is run with the `--enums` parameter, the console will list the allowed values for any import document columns that have a restricted subset of possible values. For example:

```
purview-import add-terms --enums
```

### The `--template` Parameter

All "add-\*" commands have a `--template` parameter. When run, it outputs the CSV (or JSON, depending on the command) template headers directly to the console. You can pipe this to a file to create a blank import template:

```
purview-import add-terms --template > new_terms.csv
```

### Key/Value Pair Formatting in CSV Columns

Some CSV columns accept structured data as key/value pairs within a single cell. Format these values using a semicolon (`;`) to separate each key from its value, and a comma (`,`) to separate multiple pairs. For example:

```
"user@domain.com;Interim data steward,group-name;data owner group,principal-name;service principal that owns the term"
```

This represents three key/value pairs. Ensure there are no spaces around the delimiters unless they are part of the actual key or value.

The columns that use this format are **owners**, **experts**, and **resources**. Resources is mandatory to have the key/value pair, where the value is a URL to link to the resource. Owners and experts use this format optionally — if there is no semicolon and subsequent value, the user is added as the owner or expert with a default description.

---

## Commands

Commands are listed below in the same order they appear in the help output.

- [credentials](#credentials)
- [lookup-user](#lookup-user)
- [search-principals](#search-principals)
- [deactivate-product](#deactivate-product)
- [list-glossaries](#list-glossaries)
- [delete-glossary](#delete-glossary)
- [list-term-templates](#list-term-templates)
- [export-term-template-values](#export-term-template-values)
- [list-assets](#list-assets)
- [asset-by-id](#asset-by-id)
- [asset-delete](#asset-delete)
- [list-asset-collections](#list-asset-collections)
- [create-dataset-definition](#create-dataset-definition)
- [update-dataset-definition](#update-dataset-definition)
- [create-dataset-entities](#create-dataset-entities)
- [update-dataset-entities](#update-dataset-entities)
- [create-asset-relationship](#create-asset-relationship)
- [create-managed-attribute-enum](#create-managed-attribute-enum)
- [create-managed-attributes](#create-managed-attributes)
- [list-governance-domains](#list-governance-domains)
- [add-governance-domains](#add-governance-domains)
- [delete-governance-domain](#delete-governance-domain)
- [update-governance-domain-status](#update-governance-domain-status)
- [add-governance-domain-owners](#add-governance-domain-owners)
- [add-domain-roles](#add-domain-roles)
- [list-terms](#list-terms)
- [add-terms](#add-terms)
- [update-terms](#update-terms)
- [delete-terms](#delete-terms)
- [update-term-status](#update-term-status)
- [list-data-products](#list-data-products)
- [add-data-products](#add-data-products)
- [update-data-products](#update-data-products)
- [add-data-product-terms](#add-data-product-terms)
- [add-data-product-assets](#add-data-product-assets)
- [list-cdes](#list-cdes)
- [add-cdes](#add-cdes)
- [update-cdes](#update-cdes)
- [add-cde-terms](#add-cde-terms)
- [add-cde-columns](#add-cde-columns)
- [list-okrs](#list-okrs)
- [add-okrs](#add-okrs)
- [list-key-results](#list-key-results)
- [add-key-results](#add-key-results)
- [list-okr-dp](#list-okr-dp)
- [add-okr-dp](#add-okr-dp)
- [list-custom-attributes](#list-custom-attributes)
- [add-custom-attributes](#add-custom-attributes)

---

## Credential & Utility Commands

### credentials

Set the credentials for the migration tool. This is required before any other commands can be run. You will need an Azure App Registration with permissions to the Purview and Graph APIs.

More detailed instructions for this command can be found [here](credentials.md).

*Parameters:*

> `--client-id`
>
> The client id (GUID) to use for authentication. Must be a valid GUID. See: https://learn.microsoft.com/en-us/purview/tutorial-using-rest-apis

> `--client-secret`
>
> The client secret to use for authentication. See: https://learn.microsoft.com/en-us/purview/tutorial-using-rest-apis

> `--tenant-id`
>
> The Azure Tenant Id (GUID) to use for integration. Must be a valid GUID.

*Examples:*

```
purview-import credentials --client-id d711c4b8-a919-4fe6-81e7-213ecfb46d4a --client-secret 5ecb67170d084474a355b3c0a834b14a --tenant-id d94627ed-d46d-407c-bd61-cd8077013034
```

------

### lookup-user

Lookup a user in MS Entra Graph API by email address or Entra Object ID. Returns the JSON representation of the user from Entra. Useful when you need to determine the owner/steward/expert identity and the API only returns the GUID object id.

*Parameters:*

> `-u, --user`
>
> The user identifier — either an email address or an Entra Object ID (GUID).

*Examples:*

```
purview-import lookup-user --user 21e8c3e5-5776-4b5b-8cbf-881e8eb5ff3f
purview-import lookup-user --user bob@test.com
```

------

### search-principals

Search for a user, group, or service principal in MS Entra using a search string. Returns matching principals with their Object IDs. Useful for finding the correct identifier when you only know part of a name or email.

*Parameters:*

> `-s, --search`
>
> The search term, such as part of an email address or name for a user, service principal, or group.

*Examples:*

```
purview-import search-principals -s john
purview-import search-principals --search "finance team"
```

------

### deactivate-product

Deactivate the product and remove the license from this machine.

*Parameters:*

> `-h, --help`
>
> Prints help information

*Examples:*

```
purview-import deactivate-product
```

------

## Classic Glossary Commands

These commands work with the classic Azure Purview glossary (Atlas-based). They are primarily used for migration scenarios where terms need to be exported from the classic glossary and imported into the new Microsoft Purview governance domains.

### list-glossaries

List all glossaries and their terms from the classic Azure Purview instance. Supports filtering by glossary name, term name, or parent term. Can output as CSV or generate an import file compatible with the `add-terms` command for migration purposes.

*Parameters:*

> `--filter-glossaries`
>
> Comma separated list of source glossary names to filter. If not provided, all glossaries will be listed.

> `--filter-terms`
>
> Comma separated list of source term names to filter. If not provided, all terms will be listed. This will respect the `--filter-glossaries` option if it is specified and only use terms from the glossaries that match the filter.

> `--filter-by-parent`
>
> Filter for terms whose parent term matches the specified term name.

> `--csv`
>
> If set, the output will be in CSV format (output to stdout, pipe to a file).

> `--create-import-file`
>
> If set, a CSV file will be created with the specified name that matches the `add-terms` template format, ready for import.

> `-d, --single-governance-domain`
>
> When creating the import file, the governance domain to migrate the terms to. If not specified, the glossary name will be used for the governance domain name.

> `-r, --remove-parent`
>
> Remove the original term's parent when creating the import file.

> `--use-glossary-contacts-when-empty`
>
> When the term has no contacts, use the glossary contacts as the owners of the term instead.

*Examples:*

```
purview-import list-glossaries
```

```
purview-import list-glossaries --csv > glossarylist.csv
```

```
purview-import list-glossaries --filter-glossaries "my glossary" --filter-terms "my term,other term"
```

```
purview-import list-glossaries --filter-by-parent "My Parent Term"
```

```
purview-import list-glossaries --filter-glossaries "my glossary" --filter-by-parent "My Parent Term"
```

```
purview-import list-glossaries --create-import-file glossary-terms.csv
```

```
purview-import list-glossaries --create-import-file glossary-terms.csv -d "Target Domain" --remove-parent
```

```
purview-import list-glossaries --create-import-file glossary-terms.csv --use-glossary-contacts-when-empty
```

------

### delete-glossary

Delete a glossary from the classic Azure Purview instance. Optionally delete all terms in the glossary before deleting the glossary itself.

*Parameters:*

> `--glossary`
>
> The name of the glossary to delete.

> `--delete-terms`
>
> If set to true, all terms will be deleted from the glossary before the glossary itself is deleted.

*Examples:*

```
purview-import delete-glossary --glossary glossary1
```

```
purview-import delete-glossary --glossary glossary1 --delete-terms true
```

------

### list-term-templates

List all term templates in the classic Azure Purview instance. Term templates define custom attributes that can be applied to glossary terms. Optionally export to a file.

*Parameters:*

> `-f, --file`
>
> The file to write the term templates to.

*Examples:*

```
purview-import list-term-templates
```

```
purview-import list-term-templates --file templates.csv
```

------

### export-term-template-values

Export all custom term template attribute values from terms in the classic Azure Purview instance. Supports filtering by glossary or term name. The results can be saved to a CSV file using the `--file` option.

*Parameters:*

> `--filter-glossaries`
>
> Comma separated list of source glossary names to filter. If not provided, all glossaries will be listed.

> `--filter-terms`
>
> Comma separated list of source term names to filter. If not provided, all terms will be listed. This will respect the `--filter-glossaries` option if it is specified and only use terms from the glossaries that match the filter.

> `--file`
>
> If set, a CSV file will be created with the specified name containing the term template values.

*Examples:*

```
purview-import export-term-template-values --file termtemplatevalues.csv
```

```
purview-import export-term-template-values --filter-glossaries "my glossary" --file termtemplatevalues.csv
```

```
purview-import export-term-template-values --filter-terms "term1,term2" --file filtered_values.csv
```

------

## Data Asset Commands

These commands work with data assets in both the classic Azure Purview (Atlas) catalog and the new Microsoft Purview Data Governance API. They allow you to list, inspect, delete, and manage assets and their relationships.

### list-assets

List all data assets from both Azure Purview (Atlas) and Microsoft Purview Data Governance APIs. Server-side filters reduce what is retrieved from the APIs. A client-side search filter (`--search`) can further filter the merged results across name, description, type, FQN, and collection.

The exported lists respect all filtering parameters. Assets can be exported as a standard CSV or in the format expected by `add-data-products` for creating data products directly from an asset list.

*Parameters:*

> `-s, --search`
>
> Free-text search across asset name, description, type, FQN, and collection. Applied client-side after all assets are retrieved.

> `-f, --file`
>
> The CSV file to write the asset list to. Includes a set of common columns.

> `--data-product-file`
>
> The CSV file to write the asset list to, but in the format that is ready for import as data products using the `add-data-products` command.

> `-i, --ignore-azp`
>
> Skip Azure Purview (Atlas) API calls. Only retrieve assets from the Microsoft Purview Data Governance API.

> `-t, --tables-only`
>
> Server-side filter: only retrieve table assets (ObjectType=Tables).

> `--files-only`
>
> Server-side filter: only retrieve file assets (ObjectType=Files).

> `--filter-type`
>
> Server-side filter by specific entity type. The asset type must match the string entirely. Examples: `azure_sql_table`, `databricks_table`, `powerbi_dataset`.

> `--filter-collection`
>
> Server-side filter by collection friendly name.

*Examples:*

```
purview-import list-assets
```

```
purview-import list-assets --file all_assets.csv
```

```
purview-import list-assets --data-product-file all_assets_as_data_product_import.csv
```

```
purview-import list-assets --filter-type powerbi_dataset --data-product-file powerbi_data_products.csv
```

```
purview-import list-assets -t --data-product-file sql_tables_as_data_products.csv
```

```
purview-import list-assets -i --search "sales" --file sales_assets.csv
```

```
purview-import list-assets --filter-collection "Finance" --file finance_assets.csv
```

------

### asset-by-id

Retrieve a single asset/entity from the Azure Purview (Atlas) catalog by its GUID. Displays formatted asset details by default, or raw JSON with `--raw`. Useful for inspecting an asset's full metadata, relationships, and attributes.

*Parameters:*

> `-g, --guid`
>
> The GUID of the asset to retrieve. Must be a valid GUID format.

> `--raw`
>
> Output raw JSON without formatting. Useful for piping to other processes or `jq`.

> `--min-ext-info`
>
> Return minimal information for referred entities. Reduces response size.

> `--ignore-relationships`
>
> Ignore relationship attributes in the response.

*Examples:*

```
purview-import asset-by-id --guid 3f2504e0-4f89-11d3-9a0c-0305e82c3301
```

```
purview-import asset-by-id -g 3f2504e0-4f89-11d3-9a0c-0305e82c3301 --raw
```

```
purview-import asset-by-id -g 3f2504e0-4f89-11d3-9a0c-0305e82c3301 --min-ext-info --ignore-relationships
```

------

### asset-delete

Delete an asset from the Azure Purview (Atlas) catalog by its GUID. In interactive mode (default), the command displays the asset details and prompts for confirmation before deleting. Use `--silent` with `--dry-run false` for scripted or automated deletion.

*Parameters:*

> `-g, --guid`
>
> The GUID of the asset to delete. Must be a valid GUID format.

> `-s, --silent`
>
> Skip the confirmation prompt. Requires `--dry-run false` to actually delete.

> `--dry-run` (Default: true)
>
> Validate the asset without deleting it. Set to `false` to perform the actual delete.

*Examples:*

```
# Interactive mode — shows asset details and prompts for confirmation
purview-import asset-delete -g 3f2504e0-4f89-11d3-9a0c-0305e82c3301
```

```
# Dry run — validates the asset exists but does not delete
purview-import asset-delete -g 3f2504e0-4f89-11d3-9a0c-0305e82c3301 --dry-run true
```

```
# Silent delete — no prompt, actually deletes
purview-import asset-delete -g 3f2504e0-4f89-11d3-9a0c-0305e82c3301 --silent --dry-run false
```

------

### list-asset-collections

List all collections in the Azure Purview instance in a hierarchical tree format. Supports free-text search across collection name, friendly name, and description. Matching collections and their ancestors are shown when searching.

*Parameters:*

> `-s, --search`
>
> Free-text search across collection name, friendly name, and description.

> `-f, --file`
>
> Write the output to a CSV file.

> `--csv`
>
> Output CSV to stdout for piping to another process.

*Examples:*

```
purview-import list-asset-collections
```

```
purview-import list-asset-collections -s "Finance"
```

```
purview-import list-asset-collections --file collections.csv
```

```
purview-import list-asset-collections --csv > collections.csv
```

------

### create-dataset-definition

Create custom dataset type definitions in the Azure Purview (Atlas) instance using a JSON file. Type definitions define the schema for custom entity types (e.g., custom tables and columns). Use `--template` to generate a sample JSON file showing the required structure.

*Parameters:*

> `--template`
>
> Output a sample JSON template for creating custom dataset type definitions. See [Template Example](templates/create-dataset-definition.md).

> `-f, --file`
>
> The JSON file that contains the custom dataset type definitions to create.

> `--dry-run` (Default: true)
>
> Validate the JSON file without creating the type definitions. Set to `false` to create.

*Examples:*

```
purview-import create-dataset-definition --template > dataset_definition.json
```

```
purview-import create-dataset-definition --file dataset_definition.json
```

```
purview-import create-dataset-definition --file dataset_definition.json --dry-run false
```

------

### update-dataset-definition

Update existing custom dataset type definitions in the Azure Purview (Atlas) instance using a JSON file. Uses PUT to update existing type definitions. Use `--template` to generate a sample JSON file.

*Parameters:*

> `--template`
>
> Output a sample JSON template for updating custom dataset type definitions. See [Template Example](templates/update-dataset-definition.md).

> `-f, --file`
>
> The JSON file that contains the custom dataset type definitions to update.

> `--dry-run` (Default: true)
>
> Validate the JSON file without updating the type definitions. Set to `false` to update.

*Examples:*

```
purview-import update-dataset-definition --template > dataset_definition.json
```

```
purview-import update-dataset-definition --file dataset_definition.json
```

```
purview-import update-dataset-definition --file dataset_definition.json --dry-run false
```

------

### create-dataset-entities

Create custom dataset entities (tables and columns) in the Azure Purview (Atlas) instance using a JSON file. Entities are instances of the custom type definitions created with `create-dataset-definition`. Supports `businessAttributes` for managed attribute values. Use `--collection-id` to assign entities to a specific collection.

*Parameters:*

> `--template`
>
> Output a sample JSON template for creating custom dataset entities. See [Template Example](templates/create-dataset-entities.md).

> `-f, --file`
>
> The JSON file that contains the custom dataset entities to create.

> `-c, --collection-id`
>
> The collection ID (assigned identifier) to assign the entities to. If not specified, entities will be created in the root collection.

> `--dry-run` (Default: true)
>
> Validate the JSON file without creating the entities. Set to `false` to create.

*Examples:*

```
purview-import create-dataset-entities --template > dataset_entities.json
```

```
purview-import create-dataset-entities --file dataset_entities.json
```

```
purview-import create-dataset-entities --file dataset_entities.json -c my-collection-id --dry-run false
```

------

### update-dataset-entities

Update existing custom dataset entities (tables and columns) in the Azure Purview (Atlas) instance using a JSON file. Entities in the file must include their real GUIDs (not placeholder negative IDs). Use `--collection-id` to reassign entities to a different collection.

*Parameters:*

> `--template`
>
> Output a sample JSON template for updating custom dataset entities. See [Template Example](templates/update-dataset-entities.md).

> `-f, --file`
>
> The JSON file that contains the custom dataset entities to update.

> `-c, --collection-id`
>
> The collection ID (assigned identifier) to assign the entities to. If not specified, entities remain in their current collection.

> `--dry-run` (Default: true)
>
> Validate the JSON file without updating the entities. Set to `false` to update.

*Examples:*

```
purview-import update-dataset-entities --template > dataset_entities.json
```

```
purview-import update-dataset-entities --file dataset_entities.json
```

```
purview-import update-dataset-entities --file dataset_entities.json -c new-collection-id --dry-run false
```

------

### create-asset-relationship

Create a parent/child (container/contained) relationship between assets in the Azure Purview (Atlas) catalog. You can create a single relationship by specifying `--parent` and `--child` GUIDs directly, or create relationships in bulk using a CSV file.

*Parameters:*

> `--template`
>
> Output CSV template headers to stdout (for piping to a file). See [Template Example](templates/create-asset-relationship.md).

> `-p, --parent`
>
> The GUID of the parent/container asset in the relationship.

> `-c, --child`
>
> The GUID of the child/contained asset in the relationship.

> `-f, --file`
>
> Path to a CSV file containing parent,child GUID pairs for bulk relationship creation.

> `--dry-run` (Default: true)
>
> Validate the relationship without creating it. Set to `false` to create.

*Examples:*

```
# Single relationship
purview-import create-asset-relationship --parent aaa-bbb-ccc --child ddd-eee-fff --dry-run false
```

```
# Bulk from CSV
purview-import create-asset-relationship --template > relationships.csv
purview-import create-asset-relationship --file relationships.csv
purview-import create-asset-relationship --file relationships.csv --dry-run false
```

------

### create-managed-attribute-enum

Create enum definitions for managed attribute choice/multi-choice fields in the Azure Purview (Atlas) instance. You can create a single enum inline by providing `--name` and `--choices`, or create multiple enums from a JSON file.

*Parameters:*

> `--template`
>
> Output a sample JSON template for creating enum definitions via file. See [Template Example](templates/create-managed-attribute-enum.md).

> `-n, --name`
>
> The name of the enum definition to create (e.g., `ATTRIBUTE_ENUM_CUSTOMNAME`).

> `-c, --choices`
>
> Comma-separated list of enum choices (e.g., `"choice1,choice2,choice3"`).

> `-d, --description`
>
> Optional description for the enum definition.

> `-f, --file`
>
> A JSON file containing enum definitions in the `{ "enumDefs": [...] }` format. Supports multiple enums in one file.

> `--dry-run` (Default: true)
>
> Validate the input without creating the enum. Set to `false` to create.

*Examples:*

```
# Single enum inline
purview-import create-managed-attribute-enum --name ATTRIBUTE_ENUM_STATUS --choices "Active,Inactive,Deprecated" --dry-run false
```

```
# From JSON file
purview-import create-managed-attribute-enum --template > enums.json
purview-import create-managed-attribute-enum --file enums.json
purview-import create-managed-attribute-enum --file enums.json --dry-run false
```

------

### create-managed-attributes

Create managed attribute group definitions in the Azure Purview (Atlas) instance using a JSON file. Managed attributes support the following types: string, int, boolean, date, choice, and multi-choice. Choice and multi-choice types require enum definitions created with `create-managed-attribute-enum`.

*Parameters:*

> `--template`
>
> Output a sample JSON template for creating managed attribute definitions. See [Template Example](templates/create-managed-attributes.md).

> `-f, --file`
>
> The JSON file that contains the managed attribute definitions to create.

> `--dry-run` (Default: true)
>
> Validate the JSON file without creating the managed attributes. Set to `false` to create.

*Examples:*

```
purview-import create-managed-attributes --template > managed_attributes.json
```

```
purview-import create-managed-attributes --file managed_attributes.json
```

```
purview-import create-managed-attributes --file managed_attributes.json --dry-run false
```

------

## Governance Domain Commands

### list-governance-domains

List all governance domains in the Microsoft Purview instance. Displays to screen or exports to a CSV file.

*Parameters:*

> `-f, --file`
>
> The CSV file to write the governance domains to.

*Examples:*

```
purview-import list-governance-domains
```

```
purview-import list-governance-domains --file governance_domains.csv
```

------

### add-governance-domains

Add governance domains to the Microsoft Purview instance from a CSV file. Use `--template` to generate the CSV headers. The `id` column in the template is used by `list-governance-domains` only — leave it blank when adding new domains.

By default, owners specified in the import file are added to ALL roles for the governance domain. Use `--owner-only` to add them only to the owner role.

*Parameters:*

> `--template`
>
> Output the CSV template headers to the console. Pipe to a file using `> filename.csv`. See [Template Example](templates/add-governance-domains.md).

> `--enums`
>
> List enums for use in the governance domain creation template.

> `-f, --file`
>
> The CSV file that contains the governance domain data to import.

> `--owner-only`
>
> Only add owners to the governance domain owner role, not to all roles for the governance domain.

> `--dry-run` (Default: true)
>
> Do not actually create the governance domains. Set to `false` to import.

*Enums:*

> **Status**: `Published`, `Draft`, `Expired`
>
> **Governance Domain Type**: `FunctionalUnit`, `LineOfBusiness`, `DataDomain`, `Regulatory`, `Project`

*Examples:*

```
purview-import add-governance-domains --template > new_governance_domains.csv
```

```
purview-import add-governance-domains --enums
```

```
purview-import add-governance-domains --file new_governance_domains.csv
```

```
purview-import add-governance-domains --file new_governance_domains.csv --dry-run false
```

```
purview-import add-governance-domains --file new_governance_domains.csv --owner-only --dry-run false
```

------

### delete-governance-domain

Delete a governance domain in the Microsoft Purview instance. The domain cannot have any child entities such as glossary terms, data products, or OKRs. You can specify the domain by name or by ID (GUID). Prompts for confirmation unless `--silent` is used.

*Parameters:*

> `-d, --governance-domain`
>
> The governance domain to delete. Can be the domain name or the domain ID (GUID).

> `-s, --silent`
>
> Do not prompt for confirmation, silent delete.

*Examples:*

```
purview-import delete-governance-domain -d "My Governance Domain"
```

```
purview-import delete-governance-domain -d 7417a2b8-7fe4-4307-b6b2-a36706ca4fd3
```

```
purview-import delete-governance-domain -d "My Governance Domain" --silent
```

------

### update-governance-domain-status

Update the status of a governance domain in the Microsoft Purview instance. You can specify the domain by name or GUID, and the new status which must be Draft, Published, or Expired.

*Parameters:*

> `-d, --governance-domain`
>
> The governance domain to modify. Can be the domain name or GUID.

> `-s, --status`
>
> The new governance domain status. Must be one of: `Draft`, `Published`, or `Expired`.

> `--dry-run` (Default: true)
>
> Do not actually update the domain. Set to `false` to apply the change.

*Examples:*

```
purview-import update-governance-domain-status -s Published -d "My Governance Domain"
```

```
purview-import update-governance-domain-status -s Draft -d 7417a2b8-7fe4-4307-b6b2-a36706ca4fd3
```

```
purview-import update-governance-domain-status --status Published --governance-domain "My Governance Domain" --dry-run false
```

------

### add-governance-domain-owners

Add owners to an existing governance domain in the Microsoft Purview instance. Accepts a comma-separated list of Entra Object IDs (GUIDs) or email addresses.

By default this adds the user to ALL roles for the governance domain. This behavior is intended to save the tedious task of the user having to add themselves to all of the individual roles. The `--owner-only` flag restricts the assignment to only the owner role.

This does not use an import file — it is an ad-hoc tool to quickly add a user as owner of a governance domain.

*Parameters:*

> `-d, --governance-domain-id`
>
> The governance domain ID (GUID) to add owners to.

> `-o, --owners`
>
> A comma separated list of owner Object IDs (GUIDs) or email addresses to add as owners.

> `--owner-only`
>
> Only add owners to the governance domain owner role, not to all roles for the governance domain.

*Examples:*

```
purview-import add-governance-domain-owners -d 5d32b66f-809b-4199-a790-3dc3045c4f48 --owners test@emaildomain.com,296f90df-3cd5-4ec2-8de9-24b262e9286f
```

```
purview-import add-governance-domain-owners -d 5d32b66f-809b-4199-a790-3dc3045c4f48 --owners test@emaildomain.com --owner-only
```

------

### add-domain-roles

Assign users or groups to governance domain roles (e.g., Data Owner, Data Steward) from a CSV file. Use `--template` to generate the CSV headers and `--enums` to see valid role values.

*Parameters:*

> `--template`
>
> Output the CSV template headers to the console. Pipe to a file using `> filename.csv`. See [Template Example](templates/add-domain-roles.md).

> `--enums`
>
> List enums for valid role values.

> `-f, --file`
>
> The CSV file that contains the role assignments to create.

> `--dry-run` (Default: true)
>
> Do not actually create the role assignments. Set to `false` to import.

*Examples:*

```
purview-import add-domain-roles --template > add_to_roles.csv
```

```
purview-import add-domain-roles --enums
```

```
purview-import add-domain-roles --file add_to_roles.csv
```

```
purview-import add-domain-roles --file add_to_roles.csv --dry-run false
```

------

## Glossary Term Commands

These commands manage glossary terms in the new Microsoft Purview governance domains (not the classic Atlas glossary).

### list-terms

List all glossary terms in the Microsoft Purview instance. Supports filtering by governance domain and optionally includes custom attribute values in the export.

*Parameters:*

> `-f, --file`
>
> The file to write the CSV glossary term output to.

> `-g, --governance-domain`
>
> Filter by governance domain. Accepts the domain name or ID (GUID).

> `-a, --attributes`
>
> Include the terms' custom attributes in the output.

*Examples:*

```
purview-import list-terms
```

```
purview-import list-terms --file allterms.csv
```

```
purview-import list-terms -g "Finance Domain" --file finance_terms.csv
```

```
purview-import list-terms --file allterms_with_attributes.csv --attributes
```

------

### add-terms

Add glossary terms to the Microsoft Purview instance from a CSV file. Use `--template` to generate the CSV headers.

*Parameters:*

> `--template`
>
> Output the CSV template headers to the console. Pipe to a file using `> filename.csv`. See [Template Example](templates/add-terms.md).

> `--enums`
>
> List enums for use in the add terms template.

> `-f, --file`
>
> The CSV file that contains the glossary terms to create.

> `--dry-run` (Default: true)
>
> Do not actually create the terms. Set to `false` to import.

*Enums:*

> **Status**: `Published`, `Draft`, `Expired`

*Examples:*

```
purview-import add-terms --template > new_terms.csv
```

```
purview-import add-terms --enums
```

```
purview-import add-terms --file new_terms.csv
```

```
purview-import add-terms --file new_terms.csv --dry-run false
```

------

### update-terms

Update existing glossary terms in the Microsoft Purview instance from a CSV file. Use the `list-terms` command to export terms, modify the file, and pass it back to this command.

**NOTE**: This will not change the term's governance domain. To move a term to a different governance domain, you must delete and re-add the term.

*Parameters:*

> `--template`
>
> Output the CSV template headers to the console. Pipe to a file using `> filename.csv`. See [Template Example](templates/update-terms.md).

> `--enums`
>
> List enums for use in the glossary terms update template.

> `-f, --file`
>
> The CSV file that contains the glossary terms to update.

> `--dry-run` (Default: true)
>
> Do not actually update the glossary terms. Set to `false` to apply updates.

*Examples:*

```
purview-import update-terms --template > terms_template.csv
```

```
purview-import update-terms --enums
```

```
purview-import update-terms --file terms.csv
```

```
purview-import update-terms --file terms.csv --dry-run false
```

------

### delete-terms

Delete ALL glossary terms in a governance domain. This will also delete the term relationships — specifically, related terms, synonyms, data product relationships, critical data element relationships, data asset relationships, and critical data column relationships.

Prompts for confirmation unless `--silent` is used. Specify the governance domain by name or ID.

*Parameters:*

> `-d, --governance-domain`
>
> The governance domain to delete ALL glossary terms from. Accepts domain name or ID.

> `-s, --silent`
>
> When true, will not prompt for confirmation before deleting.

*Examples:*

```
purview-import delete-terms --governance-domain "The Domain Name"
```

```
purview-import delete-terms -d "The Domain Name" --silent
```

------

### add-related-terms

Create Related or Synonym relationships between glossary terms in the Microsoft Purview instance from a CSV file. Each row defines a relationship between two terms. The `relatedtermdomain` column is only required when the related term lives in a different governance domain than the source term; if left blank, the tool assumes both terms are in the same domain.

*Parameters:*

> `--template`
>
> Output the CSV template headers to the console. Pipe to a file using `> filename.csv`. See [Template Example](templates/add-related-terms.md).

> `--enums`
>
> List valid enum values for relationship types.
>
> **Relationship Type**: `Related`, `Synonym`

> `-f, --file`
>
> The CSV file that contains the term relationships to create.

> `--dry-run` (Default: true)
>
> Validate the CSV file without creating the relationships. Set to `false` to import.

*Examples:*

```
purview-import add-related-terms --template > related_terms.csv
```

```
purview-import add-related-terms --enums
```

```
purview-import add-related-terms --file related_terms.csv
```

```
purview-import add-related-terms --file related_terms.csv --dry-run false
```

------

### update-term-status

Update the status of one or many glossary terms within a governance domain. You can specify the domain by name or GUID, and the new status which must be Draft, Published, or Expired. Optionally limit the update to specific terms using `--terms`. If no terms are specified, all terms in the governance domain will be updated.

*Parameters:*

> `-d, --governance-domain`
>
> The governance domain containing the terms. Can be the domain name or GUID.

> `-s, --status`
>
> The new term status. Must be one of: `Draft`, `Published`, or `Expired`.

> `-t, --terms`
>
> Term(s) to limit the update to. If there is more than one, use a comma separated list. If omitted, all terms in the domain are updated.

> `--dry-run` (Default: true)
>
> Do not actually update the terms. Set to `false` to apply the change.

*Examples:*

```
purview-import update-term-status -s Published -d "My Governance Domain" --dry-run false
```

```
purview-import update-term-status -s Draft -d 7417a2b8-7fe4-4307-b6b2-a36706ca4fd3 -t "MyTerm,MyTerm2"
```

```
purview-import update-term-status --status Published --governance-domain "My Governance Domain" --terms "MyTerm,MyTerm2" --dry-run false
```

------

## Data Product Commands

### list-data-products

List all data products in the Microsoft Purview instance. Supports filtering by governance domain and optionally includes custom attribute values in the export.

*Parameters:*

> `-f, --file`
>
> The file to write the CSV output to.

> `-g, --governance-domain`
>
> Filter by governance domain. Accepts the domain name or ID (GUID).

> `-a, --attributes`
>
> Include the data products' custom attributes in the output.

*Examples:*

```
purview-import list-data-products
```

```
purview-import list-data-products --file alldataproducts.csv
```

```
purview-import list-data-products -g "Finance Domain" --file finance_products.csv
```

```
purview-import list-data-products --file all_with_attrs.csv --attributes
```

------

### add-data-products

Add data products to the Microsoft Purview instance from a CSV file. Use `--template` to generate the CSV headers.

*Parameters:*

> `--template`
>
> Output the CSV template headers to the console. Pipe to a file using `> filename.csv`. See [Template Example](templates/add-data-products.md).

> `--enums`
>
> List enums for use in the data products creation template.

> `-f, --file`
>
> The CSV file that contains the data products to create.

> `--dry-run` (Default: true)
>
> Do not actually create the data products. Set to `false` to import.

*Enums:*

> **Status**: `Published`, `Draft`, `Expired`
>
> **Data Product Type**: `Dataset`, `MasterDataAndReferenceData`, `BusinessSystemOrApplication`, `ModelTypes`, `DashboardsOrReports`, `Operational`
>
> **Update Frequency**: *(blank)*, `Daily`, `Weekly`, `Monthly`, `Quarterly`, `Yearly`

*Examples:*

```
purview-import add-data-products --template > new_dataproducts.csv
```

```
purview-import add-data-products --enums
```

```
purview-import add-data-products --file new_dataproducts.csv
```

```
purview-import add-data-products --file new_dataproducts.csv --dry-run false
```

------

### update-data-products

Update existing data products in the Microsoft Purview instance from a CSV file. Use `list-data-products` to export data products, modify the file, and pass it back to this command.

*Parameters:*

> `--template`
>
> Output the CSV template headers to the console. Pipe to a file using `> filename.csv`. Alternatively, use the `list-data-products` command and modify that file. See [Template Example](templates/update-data-products.md).

> `--enums`
>
> List enums for use in the data product update template.

> `-f, --file`
>
> The CSV file that contains the data products to update.

> `--dry-run` (Default: true)
>
> Do not actually update the data products. Set to `false` to apply updates.

*Examples:*

```
purview-import update-data-products --template > dp_template.csv
```

```
purview-import update-data-products --enums
```

```
purview-import update-data-products --file updated_dataproducts.csv
```

```
purview-import update-data-products --file updated_dataproducts.csv --dry-run false
```

------

### add-data-product-terms

Associate glossary terms with data products in the Microsoft Purview instance from a CSV file. Both the data products and the terms must already exist. You can provide entity IDs directly, or use name + governance domain for automatic lookup.

*Parameters:*

> `--template`
>
> Output the CSV template headers to the console. Pipe to a file using `> filename.csv`. See [Template Example](templates/add-data-product-terms.md).

> `--enums`
>
> List enums for use in the data product term relationship creation template.

> `-f, --file`
>
> The CSV file that contains the data product and term associations to create.

> `--dry-run` (Default: true)
>
> Do not actually create the relationships. Set to `false` to import.

*Examples:*

```
purview-import add-data-product-terms --template > dp_terms.csv
```

```
purview-import add-data-product-terms --file dp_terms.csv
```

```
purview-import add-data-product-terms --file dp_terms.csv --dry-run false
```

------

### add-data-product-assets

Associate data assets with data products in the Microsoft Purview instance from a CSV file. The data products must already exist. Each row identifies the data product and a data asset to associate with it.

The data asset can be identified in one of three ways (provide exactly one per row):

1. **`dataassetid`** — the Atlas GUID of the asset.
2. **`collectionname` + `tablename`** — the collection and table name; the tool resolves the GUID.
3. **`fqn`** — the fully qualified name of the asset; the tool queries the data map to resolve the GUID.

*Parameters:*

> `--template`
>
> Output the CSV template headers to the console. Pipe to a file using `> filename.csv`. See [Template Example](templates/add-data-product-assets.md).

> `--enums`
>
> List enums for use in the data product asset relationship creation template.

> `-f, --file`
>
> The CSV file that contains the data product and asset associations to create.

> `--dry-run` (Default: true)
>
> Do not actually create the relationships. Set to `false` to import.

*Examples:*

```
purview-import add-data-product-assets --template > dp_assets.csv
```

```
purview-import add-data-product-assets --file dp_assets.csv
```

```
purview-import add-data-product-assets --file dp_assets.csv --dry-run false
```

------

## Critical Data Element (CDE) Commands

### list-cdes

List all critical data elements in the Microsoft Purview instance.

*Parameters:*

> `-f, --file`
>
> The file to write the CSV output to.

*Examples:*

```
purview-import list-cdes
```

```
purview-import list-cdes --file allcdes.csv
```

------

### add-cdes

Add critical data elements to the Microsoft Purview instance from a CSV file. Use `--template` to generate the CSV headers.

*Parameters:*

> `--template`
>
> Output the CSV template headers to the console. Pipe to a file using `> filename.csv`. See [Template Example](templates/add-cdes.md).

> `--enums`
>
> List enums for use in the critical data elements creation template.

> `-f, --file`
>
> The CSV file that contains the critical data elements to create.

> `--dry-run` (Default: true)
>
> Do not actually create the critical data elements. Set to `false` to import.

*Enums:*

> **Status**: `Published`, `Draft`, `Expired`
>
> **Data Types (CDE's)**: `Number`, `Text`, `DateTime`

*Examples:*

```
purview-import add-cdes --template > new_cdes.csv
```

```
purview-import add-cdes --enums
```

```
purview-import add-cdes --file new_cdes.csv
```

```
purview-import add-cdes --file new_cdes.csv --dry-run false
```

------

### update-cdes

Update existing critical data elements in the Microsoft Purview instance from a CSV file. Use `list-cdes` to export CDEs, modify the file, and pass it back to this command.

*Parameters:*

> `--template`
>
> Output the CSV template headers to the console. Pipe to a file using `> filename.csv`. Alternatively, use the `list-cdes` command and modify that file. See [Template Example](templates/update-cdes.md).

> `--enums`
>
> List enums for use in the critical data elements update template.

> `-f, --file`
>
> The CSV file that contains the critical data elements to update.

> `--dry-run` (Default: true)
>
> Do not actually update the critical data elements. Set to `false` to apply updates.

*Examples:*

```
purview-import update-cdes --template > cde_template.csv
```

```
purview-import update-cdes --enums
```

```
purview-import update-cdes --file updated_cdes.csv
```

```
purview-import update-cdes --file updated_cdes.csv --dry-run false
```

------

### add-cde-terms

Associate glossary terms with critical data elements in the Microsoft Purview instance from a CSV file. The CDEs and terms must already exist. You can provide entity IDs directly, or use name + governance domain for automatic lookup.

*Parameters:*

> `--template`
>
> Output the CSV template headers to the console. Pipe to a file using `> filename.csv`. See [Template Example](templates/add-cde-terms.md).

> `--enums`
>
> List enums for use in the CDE term relationship creation template.

> `-f, --file`
>
> The CSV file that contains the CDE and term associations to create.

> `--dry-run` (Default: true)
>
> Do not actually create the CDE term relationships. Set to `false` to import.

*Examples:*

```
purview-import add-cde-terms --template > cde_terms.csv
```

```
purview-import add-cde-terms --file cde_terms.csv
```

```
purview-import add-cde-terms --file cde_terms.csv --dry-run false
```

------

### add-cde-columns

Associate data asset columns with critical data elements in the Microsoft Purview instance from a CSV file. The CDEs must already exist. Each row links one column from a data asset to a CDE.

The column's parent data asset can be identified in one of three ways (provide exactly one per row):

1. **`assetid` + `columnname`** — the Atlas GUID of the table asset plus the column name.
2. **`collectionname` + `tablename` + `columnname`** — the collection, table, and column name; the tool resolves the asset GUID.
3. **`fqn`** — the fully qualified name of the column; the tool queries the data map to resolve both the column and its parent asset.

*Parameters:*

> `--template`
>
> Output the CSV template headers to the console. Pipe to a file using `> filename.csv`. See [Template Example](templates/add-cde-columns.md).

> `--enums`
>
> List enums for use in the CDE columns creation template.

> `-f, --file`
>
> The CSV file that contains the CDE and column associations to create.

> `--dry-run` (Default: true)
>
> Do not actually create the associations. Set to `false` to import.

*Examples:*

```
purview-import add-cde-columns --template > cde_columns.csv
```

```
purview-import add-cde-columns --file cde_columns.csv
```

```
purview-import add-cde-columns --file cde_columns.csv --dry-run false
```

------

## OKR (Objectives & Key Results) Commands

### list-okrs

List all OKRs (Objectives and Key Results) in the Microsoft Purview instance.

*Parameters:*

> `-f, --file`
>
> The file to write the CSV OKR output to.

*Examples:*

```
purview-import list-okrs
```

```
purview-import list-okrs --file allokrs.csv
```

------

### add-okrs

Add OKRs to the Microsoft Purview instance from a CSV file. Use `--template` to generate the CSV headers.

*Parameters:*

> `--template`
>
> Output the CSV template headers to the console. Pipe to a file using `> filename.csv`. See [Template Example](templates/add-okrs.md).

> `--enums`
>
> List enums for use in the OKR creation template.

> `-f, --file`
>
> The CSV file that contains the OKRs to create.

> `--dry-run` (Default: true)
>
> Do not actually create the OKRs. Set to `false` to import.

*Enums:*

> **Status**: `Published`, `Draft`, `Expired`

*Examples:*

```
purview-import add-okrs --template > new_okrs.csv
```

```
purview-import add-okrs --enums
```

```
purview-import add-okrs --file new_okrs.csv
```

```
purview-import add-okrs --file new_okrs.csv --dry-run false
```

------

### list-key-results

List all OKR Key Results in the Microsoft Purview instance.

*Parameters:*

> `-f, --file`
>
> The file to write the CSV output to.

*Examples:*

```
purview-import list-key-results
```

```
purview-import list-key-results --file allkeyresults.csv
```

------

### add-key-results

Add OKR Key Results to the Microsoft Purview instance from a CSV file. Use `--template` to generate the CSV headers.

*Parameters:*

> `--template`
>
> Output the CSV template headers to the console. Pipe to a file using `> filename.csv`. See [Template Example](templates/add-key-results.md).

> `--enums`
>
> List enums for use in the add key results creation template.

> `-f, --file`
>
> The CSV file that contains the key results to create.

> `--dry-run` (Default: true)
>
> Do not actually create the key results. Set to `false` to import.

*Enums:*

> **Status (Key Results)**: `NotTrack`, `OnTrack`, `Behind`, `AtRisk`

*Examples:*

```
purview-import add-key-results --template > newkeyresults.csv
```

```
purview-import add-key-results --enums
```

```
purview-import add-key-results --file newkeyresults.csv
```

```
purview-import add-key-results --file newkeyresults.csv --dry-run false
```

------

### list-okr-dp

List all Data Products associated with OKRs in the Microsoft Purview instance.

*Parameters:*

> `-f, --file`
>
> The file to write the CSV output to.

*Examples:*

```
purview-import list-okr-dp
```

```
purview-import list-okr-dp --file allokrdataproducts.csv
```

------

### add-okr-dp

Associate Data Products with OKRs in the Microsoft Purview instance from a CSV file.

*Parameters:*

> `--template`
>
> Output the CSV template headers to the console. Pipe to a file using `> filename.csv`. See [Template Example](templates/add-okr-dp.md).

> `--enums`
>
> List enums for use in the OKR/data product relationships template.

> `-f, --file`
>
> The CSV file that contains the OKR and data product associations to create.

> `--dry-run` (Default: true)
>
> Do not actually create the associations. Set to `false` to import.

*Examples:*

```
purview-import add-okr-dp --template > okr_dp.csv
```

```
purview-import add-okr-dp --file okr_dp.csv
```

```
purview-import add-okr-dp --file okr_dp.csv --dry-run false
```

------

## Custom Attribute Commands

### list-custom-attributes

List all custom attribute definitions in the Microsoft Purview instance. Optionally export to a file.

*Parameters:*

> `-f, --file`
>
> The file to write the custom attributes to.

*Examples:*

```
purview-import list-custom-attributes
```

```
purview-import list-custom-attributes --file custom_attributes.csv
```

------

### add-custom-attributes

Add custom attributes to the Microsoft Purview instance from a CSV file. Use `-g/--group` to specify which attribute group the custom attributes should belong to.

*Parameters:*

> `--template`
>
> Output the CSV template headers to the console. Pipe to a file using `> filename.csv`. See [Template Example](templates/add-custom-attributes.md).

> `--enums`
>
> List enums for use in the custom attributes creation template.

> `-g, --group`
>
> The attribute group that the custom attributes should be in.

> `-f, --file`
>
> The CSV file that contains the custom attributes to create.

> `--dry-run` (Default: true)
>
> Do not actually create the custom attributes. Set to `false` to import.

*Examples:*

```
purview-import add-custom-attributes --template > new_attributes.csv
```

```
purview-import add-custom-attributes --enums
```

```
purview-import add-custom-attributes -g "My Attribute Group" --file new_attributes.csv
```

```
purview-import add-custom-attributes -g "My Attribute Group" --file new_attributes.csv --dry-run false
```

------

## Enum Reference

The following enums are used across multiple commands. Use `--enums` on any add/update command to see which enums apply to that specific command.

#### Status

- `Published`
- `Draft`
- `Expired`

#### Governance Domain Type

- `FunctionalUnit`
- `LineOfBusiness`
- `DataDomain`
- `Regulatory`
- `Project`

#### Data Types (CDE's)

- `Number`
- `Text`
- `DateTime`

#### Data Product Type

- `Dataset`
- `MasterDataAndReferenceData`
- `BusinessSystemOrApplication`
- `ModelTypes`
- `DashboardsOrReports`
- `Operational`

#### Update Frequency

- (blank)
- `Daily`
- `Weekly`
- `Monthly`
- `Quarterly`
- `Yearly`

#### Status Key Results

- `NotTrack`
- `OnTrack`
- `Behind`
- `AtRisk`

------

## Typical Workflow Examples

### Migrating Classic Glossary Terms to Governance Domains

This is a common scenario when moving from the classic Azure Purview glossary to the new Microsoft Purview governance domains.

```bash
# 1. Set up credentials
purview-import credentials --client-id <CLIENT_ID> --client-secret <SECRET> --tenant-id <TENANT_ID>
```

```bash
# 2. List existing glossaries and export as an import file
purview-import list-glossaries --create-import-file migration_terms.csv
```

```bash
# 3. (Optional) Target a single governance domain instead of using glossary names
purview-import list-glossaries --create-import-file migration_terms.csv -d "Enterprise Data Domain"
```

```bash
# 4. Create the governance domain if it doesn't exist
purview-import add-governance-domains --template > domains.csv
# ... edit domains.csv ...
purview-import add-governance-domains --file domains.csv --dry-run false
```

```bash
# 5. Dry run the term import to validate
purview-import add-terms --file migration_terms.csv
```

```bash
# 6. Import for real
purview-import add-terms --file migration_terms.csv --dry-run false
```

```bash
# 7. Publish the terms
purview-import update-term-status -s Published -d "Enterprise Data Domain" --dry-run false
```

### Setting Up a New Governance Domain from Scratch

```bash
# 1. Create the governance domain
purview-import add-governance-domains --template > domain.csv
# ... fill in domain.csv ...
purview-import add-governance-domains --file domain.csv --dry-run false
```

```bash
# 2. Add owners
purview-import add-governance-domain-owners -d <DOMAIN_GUID> --owners admin@company.com
```

```bash
# 3. Add role assignments
purview-import add-domain-roles --template > roles.csv
# ... fill in roles.csv ...
purview-import add-domain-roles --file roles.csv --dry-run false
```

```bash
# 4. Add glossary terms
purview-import add-terms --template > terms.csv
# ... fill in terms.csv ...
purview-import add-terms --file terms.csv --dry-run false
```

```bash
# 5. Add data products
purview-import add-data-products --template > products.csv
# ... fill in products.csv ...
purview-import add-data-products --file products.csv --dry-run false
```

```bash
# 6. Link terms to data products
purview-import add-data-product-terms --template > dp_terms.csv
# ... fill in dp_terms.csv ...
purview-import add-data-product-terms --file dp_terms.csv --dry-run false
```

### Creating Data Products from Existing Assets

```bash
# 1. List all assets and export in data product format
purview-import list-assets --data-product-file assets_as_products.csv
```

```bash
# 2. Filter to just SQL tables
purview-import list-assets -t --data-product-file sql_products.csv
```

```bash
# 3. Edit the CSV to set governance domains, descriptions, etc.
# 4. Import as data products
purview-import add-data-products --file sql_products.csv --dry-run false
```

### Working with Custom Datasets (Atlas)

```bash
# 1. Create the type definition
purview-import create-dataset-definition --template > type_def.json
# ... edit type_def.json ...
purview-import create-dataset-definition --file type_def.json --dry-run false
```

```bash
# 2. Create enum definitions for managed attributes (if needed)
purview-import create-managed-attribute-enum --name ATTRIBUTE_ENUM_STATUS --choices "Active,Inactive,Deprecated" --dry-run false
```

```bash
# 3. Create managed attribute groups
purview-import create-managed-attributes --template > managed_attrs.json
# ... edit managed_attrs.json ...
purview-import create-managed-attributes --file managed_attrs.json --dry-run false
```

```bash
# 4. Create entities using the type definition
purview-import create-dataset-entities --template > entities.json
# ... edit entities.json ...
purview-import create-dataset-entities --file entities.json -c my-collection --dry-run false
```

```bash
# 5. Create parent/child relationships between entities
purview-import create-asset-relationship --template > relationships.csv
# ... edit relationships.csv ...
purview-import create-asset-relationship --file relationships.csv --dry-run false
```
