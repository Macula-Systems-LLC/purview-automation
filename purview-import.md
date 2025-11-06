# Purview Bulk Import

Tool for importing and exporting lists of entities for Microsoft Purview

This tool is built for Windows and is a command line application.

To view the help, run the command

`purview-import -h`

to view help for a specific command, run the command with the -h parameter

`purview-import list-assets -h`

this will display the optional parameters and a description about what they are used for

Most commands have a --file parameter and that indicates the file to be imported or exported depending on the direction the data is flowing based on the command.

All "add-*" commands have a --dry-run parameter, which defaults to true.  This means that the input will be initially validated, however no modifications will be made to the system.  This is intentional as a safety mechanism.  When you are ready to run the command and make changes to the destination system, use the flag `--dry-run false` to cause the new data to be sent to the destination system.

In addition, all "add-*" commands also have a `--enums` parameter, when the command is run with the `--enums` parameter, the console will list the allowed values for any import document columns that have a restricted subset of possible values. for example: `purview-import add-terms --enums`



Commands:

- [credentials](#credentials)
- [deactivate-product](#deactivate-product) ⚡
- [lookup-user](#lookup-user)
- [search-principals](#search-principals) ⚡
- [list-glossaries](#list-glossaries) ⚡
- [delete-glossary](#delete-glossary) ⚡
- [export-term-template-values](#export-term-template-values) ⚡
- [list-assets](#list-assets)
- [list-governance-domains](#list-governance-domains)
- [delete-governance-domain](#delete-governance-domain)
- [update-governance-domain-status](#update-governance-domain-status) ⚡
- [add-governance-domains](#add-governance-domains)
- [add-domain-roles](#add-domain-roles) ⚡
- [add-governance-domain-owners](#add-governance-domain-owners)
- [list-terms](#list-terms)
- [add-terms](#add-terms)
- [update-terms](#update-terms) ⚡
- [delete-terms](#delete-terms)
- [update-term-status](#update-term-status) ⚡
- [list-cdes](#list-cdes)
- [add-cdes](#add-cdes)
- [list-data-products](#list-data-products)
- [add-data-products](#add-data-products)
- [add-data-product-terms](#add-data-product-terms)
- [add-data-product-assets](#add-data-product-assets)
- [add-cde-columns](#add-cde-columns)
- [list-okrs](#list-okrs)
- [add-okrs](#add-okrs)
- [list-key-results](#list-key-results)
- [add-key-results](#add-key-results)
- [list-okr-dp](#list-okr-dp)
- [add-okr-dp](#add-okr-dp)
- [list-custom-attributes](#list-custom-attributes) ⚡
- [add-custom-attributes](#add-custom-attributes) ⚡


------




## credentials

Set the credentials for the migration tool

There are specific required parameters for this command, and this is required before any of the other commands can be run.

More detailed instructions for this command can be found [here](credentials.md).

*Parameters:*

> `--client-id`
>
> The client id to use for authentication. See: https://learn.microsoft.com/en-us/purview/tutorial-using-rest-apis

> `--client-secret`
>
> The client secret to use for authentication. See: https://learn.microsoft.com/en-us/purview/tutorial-using-rest-apis

> `--azure-purview-url`
>
> The Azure Purview URL to use for integration. In your azure portal (portal.azure.com), go to your Microsoft Purview Account (not in the Purview Application itself) and select Settings > Properties.  Look for the Atlas endpoint.

> `--tenant-id`
>
> The Azure Tenant Id (GUID) to use for integration

*Example:*

`purview-import credentials --client-id d711c4b8-a919-4fe6-81e7-213ecfb46d4a --client-secret`
`5ecb67170d084474a355b3c0a834b14a --tenant-id d94627ed-d46d-407c-bd61-cd8077013034 --azure-purview-url`
`https://myapp-purview.purview.azure.com`

------

⚡ **NEW COMMAND** ⚡

## deactivate-product

Deactivate the product and remove the license from this machine

*Parameters:*

> `-h, --help`
>
> Prints help information

*Examples:*

`purview-import deactivate-product`

------




## lookup-user

Lookup a user in MS Entra Graph API by email or object id

This is a utility to determine a user based on their object id or their email address.  It just returns the json string that represents the user in Entra, but can be very useful when needing to determine the owner/steward/expert when the api only returns the guid object id.

This can be run with the `--user` parameter that could be the Entra object id (guid) or the users email address.

*Parameters:*

> `-u, --user`
>
> The user identifier, such as an email address or a entra object id for a user

*Examples:*

`purview-import lookup-user --user 21e8c3e5-5776-4b5b-8cbf-881e8eb5ff3f`

`purview-import lookup-user --user bob@test.com`

------

⚡ **NEW COMMAND** ⚡

## search-principals

Lookup a user, group or service principal using a search string

*Parameters:*

> `-s, --search`
>
> The search term, such as part of an email address or name for a user, service principal or group

*Examples:*

`purview-import search-principals -s john`

`purview-import search-principals --search john`

------

⚡ **NEW COMMAND** ⚡

## list-glossaries

List all glossaries in the source Azure Purview instance

*Parameters:*

> `--filter-glossaries`
>
> Comma seperated list of source glossaries to filter. If not provided, all glossaries will be listed

> `--filter-terms`
>
> Comma seperated list of source terms to filter. If not provided, all terms will be listed. This will respect the --filter-glossaries option if it is specified and only use terms from the glossaries that match the filter

> `--filter-by-parent`
>
> Filter for terms whose parent term matches the specified term name

> `--csv`
>
> If set, the output will be in CSV format

> `--create-import-file`
>
> If set, a csv file will be created with the specified name that matches the purview-import add-terms template

> `-d, --single-governance-domain`
>
> When creating the import file, the governance domain to migrate the terms to. If not specified, the glossary name will be used for the Governance Domain name

> `-r, --remove-parent`
>
> Remove the original Term's parent when creating the import file

> `--use-glossary-contacts-when-empty`
>
> When the term has no contacts, use the glossary contacts as the owners of the term instead

*Examples:*

`purview-import list-glossaries`

`purview-import list-glossaries --csv > glossarylist.csv`

`purview-import list-glossaries --filter-glossaries "my glossary" --filter-terms "my term,other term"`

`purview-import list-glossaries --filter-by-parent "My Parent Term"`

`purview-import list-glossaries --filter-glossaries "my glossary" --filter-by-parent "My Parent Term"`

`purview-import list-glossaries --create-import-file glossary-terms.csv`

------

⚡ **NEW COMMAND** ⚡

## delete-glossary

Delete glossaries from the source Azure Purview instance

*Parameters:*

> `--glossary`
>
> The glossary to delete

> `--delete-terms`
>
> If set, all terms will be deleted from the glossary before deletion

*Examples:*

`purview-import delete-glossary --glossary glossary1`

`purview-import delete-glossary --glossary glossary1 --delete-terms true`

------

⚡ **NEW COMMAND** ⚡

## export-term-template-values

List all custom term template values in the source Azure Purview instance terms. The results can be saved to a csv file using the --file option

*Parameters:*

> `--filter-glossaries`
>
> Comma seperated list of source glossaries to filter. If not provided, all glossaries will be listed

> `--filter-terms`
>
> Comma seperated list of source terms to filter. If not provided, all terms will be listed. This will respect the --filter-glossaries option if it is specified and only use terms from the glossaries that match the filter

> `--file`
>
> If set, a csv file will be created with the specified name with the term template values

*Examples:*

`purview-import export-term-template-values --file termtemplatevalues.csv`

------




## list-assets

List all data assets in both old and new purview instances.

This will list assets to the screen and can be filtered in a few different ways. In addition it can be saved in 2 different formats as well.  It can be exported as a csv that lists basic pertinent information about the assets, and can also be exported in the format that add-data-products expects as input. This is helpful for creating multiple data products directly from an asset list.

The exported lists will all respect the filtering using the different parameters.

Parameters:

> `--file <FILE>`
>
> The csv file to write the asset list to, this includes a set of common columns.

> `--data-product-file <FILE>`
>
> The csv file to write the asset list to, but in the format that is ready for import as data products. The data product formatted csv file to write the asset list to

> `--ignore-azp` or `-i`
>
> Ignore azure purview api calls, and only query the MS Purview api for assets

> `--tables-only` or `-t`
>
> Only list SQL Azure tables

> `--files-only`
>
> Only list File data sources

> `--filter-type <FILTER>`
>
> Filter by asset type, where the asset type matches the string entirely. Filter by asset type: azure_sql_table, databricks_table, powerbi_dataset, etc

> `--filter-collection <FILTER>`
>
> Filter by collection name, where the collection name matches a collection in the domain

Examples:

`purview-import list-assets --file all_assets.csv`

`purview-import list-assets --data-product-file all_assets_as_data_product_import.csv`

`purview-import list-assets --filter-type powerbi_dataset --data-product-file all_assets_as_data_product_import.csv`

`purview-import list-assets -t --data-product-file all_assets_as_data_product_import.csv`

------




## list-governance-domains

List all governance domains in the Microsoft Purview instance.

*Parameters:*

> `--file <FILE>`
>
> The csv file to write the governance domains to

*Examples:*

`purview-import list-governance-domains --file governance_domains.csv`



------

## delete-governance-domain

Delete a governance domain in the Microsoft Purview instance.  The domain cannot have any child entities such as glossary terms or data products or OKR's. You can specify the Domain Name or the Domain Id

*Parameters:*

> `-d <Governance Domain>` or `--governance-domain <DOMAIN>`

> ⚡ `-s, --silent` ⚡
>
> ⚡ Do not confirm, silent delete ⚡

*Examples:*

`purview-import delete-governance-domain -d "My Governance Domain"`

`purview-import delete-governance-domain -d 7417a2b8-7fe4-4307-b6b2-a36706ca4fd3`

------

⚡ **NEW COMMAND** ⚡

## update-governance-domain-status

Update a governance domain status in the Microsoft Purview instance. You can specify the Domain Name or the Domain guid, and the new status which must be Draft, Published, or Expired

*Parameters:*

> `-d, --governance-domain <n>`
>
> The governance domain to modify status. Can be name or guid

> `-s, --status <STATUS>`
>
> The new governance domain status. Can be Draft, Published, or Expired

> `--dry-run` (Default: True)
>
> Do not actually update the domain

*Examples:*

`purview-import update-governance-domain-status -s Published -d "My Governance Domain"`

`purview-import update-governance-domain-status -s Draft -d 7417a2b8-7fe4-4307-b6b2-a36706ca4fd3`

`purview-import update-governance-domain-status --status Published --governance-domain "My Governance Domain"`

------




## add-governance-domains

Add governance domains to the Microsoft Purview instance using the template as input.

*Parameters:*

> `--template`
>
> This writes the csv template headers directly to the console. It can be piped to a csv file using the > filename.csv
>
> The id column is there for the list-governance-domains only, leave it blank for the add-governance-domains command

> `--enums`
>
> List enums for use in the governance domain creation template

> `--file <FILE>`
>
> The csv file that contains the governance domain data to import

> ⚡ `--owner-only` ⚡
>
> ⚡ Only add owners to the governance domain owner role, not to all roles for the governance domain ⚡

> `--dry-run false`
>
> specifying whether a command is a dry run or not, the default is true, and must be specified as false explicitly when the command is intended to import data

*Enums:*

[Status](#status)

[governance Domain type](#governance-domain-type)

*Examples:*

`purview-import add-governance-domains --template`

`purview-import add-governance-domains --enums`

`purview-import add-governance-domains --file new_governance_domains.csv`

`purview-import add-governance-domains --file new_governance_domains.csv --dry-run false`



------

⚡ **NEW COMMAND** ⚡

## add-domain-roles

Add governance domain role users/groups

*Parameters:*

> `--template`
>
> Create a template for governance domains. pipe to a file using > to save the template

> `-d, --governance-domain-id <GOVERNANCE_DOMAIN_ID>`
>
> Specify the domain id to add the principals to

> `-f, --file <FILE>`
>
> The file that contains the governance domains to create

> `--dry-run` (Default: True)
>
> Do not actually create the governance domains

*Examples:*

`purview-import add-domain-roles --template`

`purview-import add-domain-roles --enums`

`purview-import add-domain-roles --file add_to_roles.csv`

`purview-import add-domain-roles --file add_to_roles.csv --dry-run false`

------




## add-governance-domain-owners

Adds owners to an existing governance domain in the Microsoft Purview instance.  By default this adds the user to ALL roles for the governance domain. This behavior is intended to save the tedious task of adding themself to all of the individual roles.

This does not use an import file, but is just an ad-hoc tool to add a user to be the owner of a governance domain. This helps for giving access to all aspects of the governance domain.

The `--owner-only` flag indicates that the user should not be added to ALL the roles, just the owner role and still has the option of manually assigning themselves roles, but the additional roles are not assigned automatically.

*Parameters:*

> `-d` or `--governance-domain-id <GOVERNANCE_DOMAIN_ID>`
>
> Specify the governance domain guid to add owners to

> `-o` or `--owners <OWNERS>`
>
> A comma seperated list of owner object id's (guid's) or email addresses to add as owners

> `--owner-only`
>
> Only add owners to the governance domain owner role, not to all roles for the governance domain

*Examples:*

`purview-import add-governance-domain-owners -d 5d32b66f-809b-4199-a790-3dc3045c4f48 --owners
test@emaildomain.com,296f90df-3cd5-4ec2-8de9-24b262e9286f`

------




## list-terms

List all glossary terms in the Microsoft Purview instance

*Parameters:*

> `--file <FILE>`
>
> The file to write the csv glossary term output to

> ⚡ `-g, --governance-domain <GOVERNANCE_DOMAIN>` ⚡
>
> ⚡ The governance domain to filter by. Name or Id ⚡

*Examples:*

`purview-import list-terms`

`purview-import list-terms --file allterms.csv`

------




## add-terms

Add glossary terms to the Microsoft Purview instance using the csv template as input.

*Parameters:*

> `--template`
>
> This writes the csv template headers directly to the console. It can be piped to a csv file using the > filename.csv
>

> `--enums`
>
> List enums for use in the add terms creation template

> `--file <FILE>`
>
> The csv file that contains the data to import

> `--dry-run false`
>
> specifying whether a command is a dry run or not, the default is true, and must be specified as false explicitly when the command is intended to import data

[Status](#status)

*Examples:*

`purview-import add-terms --file new_terms.csv`

`purview-import add-terms --file new_terms.csv --dry-run false`

------

⚡ **NEW COMMAND** ⚡

## update-terms

Update glossary terms in the Microsoft Purview instance. Use the list-terms command to get a list of terms to modify. **NOTE**: this will not change the term's governance domain. To do that, you must delete and re-add the term

*Parameters:*

> `--template`
>
> Create a template for glossary terms. pipe to a file using > to save the template. Leave the id column empty

> `--enums`
>
> List enums for use in the glossary terms creation template

> `-f, --file <FILE>`
>
> The file that contains the glossary terms to create

> `--dry-run` (Default: True)
>
> Do not actually create the glossary terms

*Examples:*

`purview-import update-terms --template`

`purview-import update-terms --enums`

`purview-import update-terms --file terms.csv`

`purview-import update-terms --file terms.csv --dry-run false`

------




## delete-terms

Delete ALL terms in a governance domain.   This will delete the term relationships. Specifically, related terms, synonyms, data product relationships, critical data element relationships, data asset relationships, and critical data column relationships.

*Parameters:*

> `-d, --governance-domain <DOMAIN>`
>
> The governance domain to delete ALL glossary terms from

> ⚡ `-s, --silent` ⚡
>
> ⚡ When true it will not prompt for confirmation to delete domains ⚡

*Examples:*

`purview-import delete-terms --governance-domain "The Domain Name"`

`purview-import delete-terms --d "The Domain Name"`

------

⚡ **NEW COMMAND** ⚡

## update-term-status

Update the status of a term or many terms. You can specify the Domain Name or the Domain guid that the terms exist in, and the new status which must be Draft, Published, or Expired. You may also specify a term or a comma seperated list of terms to limit the update to. If no terms are specified, all terms in the governance domain will be updated

*Parameters:*

> `-d, --governance-domain <n>`
>
> The governance domain to modify status. Can be name or guid

> `-s, --status <STATUS>`
>
> The new term status. Can be Draft, Published, or Expired

> `-t, --terms <TERMS>`
>
> Term(s) to limit the update to. If there is more than one, use a comma separated list

> `--dry-run` (Default: True)
>
> Do not actually update the terms

*Examples:*

`purview-import update-term-status -s Published -d "My Governance Domain"`

`purview-import update-term-status -s Draft -d 7417a2b8-7fe4-4307-b6b2-a36706ca4fd3 -t "MyTerm,MyTerm2"`

`purview-import update-term-status --status Published --governance-domain "My Governance Domain" --terms "MyTerm,MyTerm2"`

------




## list-cdes

List all critical data elements in the Microsoft Purview instance

*Parameters:*

> `--file <FILE>`
>
> The file to write the csv output to

*Examples:*

`purview-import list-cdes`

`purview-import list-cdes --file allcdes.csv`

------




## add-cdes

Add critical data elements to the Microsoft Purview instance using the template as input.

*Parameters:*

> `--template`
>
> This writes the csv template headers directly to the console. It can be piped to a csv file using the > filename.csv
>

> `--enums`
>
> List enums for use in the critical data elements creation template

> `--file <FILE>`
>
> The csv file that contains the data to import

> `--dry-run false`
>
> specifying whether a command is a dry run or not, the default is true, and must be specified as false explicitly when the command is intended to import data

*Enums:*

[Status](#status)

[Data Types (CDE's)](#data-types-cdes)

*Examples:*

`purview-import add-cdes --file new_cdes.csv`

`purview-import add-cdes --file new_cdes.csv --dry-run false`

------




## list-data-products

List all data products in the Microsoft Purview instance

*Parameters:*

> `--file <FILE>`
>
> The file to write the csv output to

*Examples:*

`purview-import list-data-products`

`purview-import list-data-products --file alldataproducts.csv`

------




## add-data-products

Add data products to the Microsoft Purview instance using the csv template as input.

*Parameters:*

> `--template`
>
> This writes the csv template headers directly to the console. It can be piped to a csv file using the > filename.csv
>

> `--enums`
>
> List enums for use in the data products creation template

> `--file <FILE>`
>
> The csv file that contains the data to import

> `--dry-run false`
>
> specifying whether a command is a dry run or not, the default is true, and must be specified as false explicitly when the command is intended to import data

*Enums:*

[Status](#status)

[Data Product Type](#data-product-type)

[Update Frequency](#update-frequency)

*Examples:*

`purview-import add-data-products --file new_dataproducts.csv`

`purview-import add-data-products --file new_dataproducts.csv --dry-run false`

------




## add-data-product-terms

Add data product terms relationship to data products to the Microsoft Purview instance.  This requires the data products and the terms are already created.  This will associate them and are specified by name and governance domain.

*Parameters:*

> `--template`
>
> This writes the csv template headers directly to the console. It can be piped to a csv file using the > filename.csv
>

> `--enums`
>
> List enums for use in the add data product terms relationship creation template ⚡ List enums for use in the data products term relationship creation template ⚡

> `--file <FILE>`
>
> The csv file that contains the data to import ⚡ The file that contains the data products and terms to create ⚡

> `--dry-run false`
>
> specifying whether a command is a dry run or not, the default is true, and must be specified as false explicitly when the command is intended to import data ⚡ Do not actually create the data product relationships (Default: True) ⚡

*Examples:*

`purview-import add-data-product-terms --file new_dataproducttermss.csv`

`purview-import add-data-product-terms --file new_dataproducttermss.csv --dry-run false`

------




## add-data-product-assets

Add data asset relationships to data products in the Microsoft Purview instance. The data products need to exist already, and the data asset must be specified by a guid identifier, this supports two different id's depending on how the asset was created.

*Parameters:*

> `--template`
>
> This writes the csv template headers directly to the console. It can be piped to a csv file using the > filename.csv
>

> `--enums`
>
> List enums for use in the add data product asset relationship creation template

> `--file <FILE>`
>
> The csv file that contains the data to import 

> `--dry-run false`
>
> specifying whether a command is a dry run or not, the default is true, and must be specified as false explicitly when the command is intended to import data

*Examples:*

`purview-import add-data-product-assets --file new_dataproductassets.csv`

`purview-import add-data-product-assets --file new_dataproductassets.csv --dry-run false`

------




## add-cde-columns

Add data asset column relationships to the CDE's in the Microsoft Purview instance.  The CDE must already exist.

*Parameters:*

> `--template`
>
> This writes the csv template headers directly to the console. It can be piped to a csv file using the > filename.csv
>

> `--enums`
>
> List enums for use in the add cde columns creation template 

> `--file <FILE>`
>
> The csv file that contains the data to import

> `--dry-run false`
>
> specifying whether a command is a dry run or not, the default is true, and must be specified as false explicitly when the command is intended to import data

*Examples:*

`purview-import add-cde-columns --file new_cdecolumns.csv`

`purview-import add-cde-columns --file new_cdecolumns.csv --dry-run false`

------




## list-okrs

List all OKRs in the Microsoft Purview instance

*Parameters:*

> `--file <FILE>`
>
> The file to write the csv OKR output to

*Examples:*

`purview-import list-okrs`

`purview-import list-okrs --file allokrs.csv`



------



## add-okrs

Add okrs to the Microsoft Purview instance using the csv template as input.

*Parameters:*

> `--template`
>
> This writes the csv template headers directly to the console. It can be piped to a csv file using the > filename.csv
>

> `--enums`
>
> List enums for use in the add okrs creation template

> `--file <FILE>`
>
> The csv file that contains the data to import

> `--dry-run false`
>
> specifying whether a command is a dry run or not, the default is true, and must be specified as false explicitly when the command is intended to import data

*Enums:*

[Status](#status)

*Examples:*

`purview-import add-okrs --file new_dataproducts.csv`

`purview-import add-okrs --file new_dataproducts.csv --dry-run false`

------

## list-key-results

List all  OKR Key Results in the Microsoft Purview instance

*Parameters:*

> `--file <FILE>`
>
> The file to write the csv output to

*Examples:*

`purview-import list-key-results`

`purview-import list-key-results --file allkeyresults.csv`



------



## add-key-results

Add OKR Key Results to the Microsoft Purview instance using the csv template as input.

*Parameters:*

> `--template`
>
> This writes the csv template headers directly to the console. It can be piped to a csv file using the > filename.csv
>

> `--enums`
>
> List enums for use in the add key results creation template

> `--file <FILE>`
>
> The csv file that contains the data to import

> `--dry-run false`
>
> specifying whether a command is a dry run or not, the default is true, and must be specified as false explicitly when the command is intended to import data

*Enums:*

[Status](#status-key-results)

*Examples:*

`purview-import add-key-results --file newkeyresults.csv`

`purview-import add-keyresults --file newkeyresults.csv --dry-run false`

------

## list-okr-dp

List all Data Products associated with OKRs

*Parameters:*

> `--file <FILE>`
>
> The file to write the csv output to

*Examples:*

`purview-import list-okr-dp`

`purview-import list-okr-dp --file allokrdataproducts.csv`



------



## add-okr-dp

Add Data Product / OKR Relationships to the Microsoft Purview instance using the csv template as input.

*Parameters:*

> `--template`
>
> This writes the csv template headers directly to the console. It can be piped to a csv file using the > filename.csv
>

> `--enums`
>
> List enums for use in the okr/data product relationships template

> `--file <FILE>`
>
> The csv file that contains the data to import

> `--dry-run false`
>
> specifying whether a command is a dry run or not, the default is true, and must be specified as false explicitly when the command is intended to import data

*Examples:*

`purview-import add-okr-dp --file newokrdataproducts.csv`

`purview-import add-okr-dp --file newokrdataproducts.csv --dry-run false`

------

⚡ **NEW COMMAND** ⚡

## list-custom-attributes

List all of the custom attributes the Microsoft Purview instance

*Parameters:*

> `-h, --help`
>
> Prints help information

> `-f, --file <FILE>`
>
> The file to write the attributes to

*Examples:*

`purview-import list-custom-attributes`

------

⚡ **NEW COMMAND** ⚡

## add-custom-attributes

Add custom attributes to the Microsoft Purview instance

*Parameters:*

> `--template`
>
> Create a template for adding critical data elements. pipe to a file using > to save the template. Leave the id column empty

> `--enums`
>
> List enums for use in the critical data elements creation template

> `-g, --group`
>
> The attribute group that the custom attributes should be in

> `-f, --file <FILE>`
>
> The file that contains the critical data elements to create

> `--dry-run` (Default: True)
>
> Do not actually create the critical data elements

*Examples:*

`purview-import add-custom-attributes --template`

`purview-import add-custom-attributes --enums`

`purview-import add-custom-attributes --file new_attributes.csv`

`purview-import add-custom-attributes --file new_attributes.csv --dry-run false`

------



## Enums for commands

#### Status

- Published
- Draft
- Expired

#### Governance Domain Type

- FunctionalUnit
- LineOfBusiness
- DataDomain
- Regulatory
- Project

#### Data Types (CDE's)

- Number
- Text
- DateTime

#### Data Product Type

- Dataset
- MasterDataAndReferenceData
- BusinessSystemOrApplication
- ModelTypes
- DashboardsOrReports
- Operational

#### Update Frequency

- (blank)
- Daily
- Weekly
- Monthly
- Quarterly
- Yearly

#### Status Key Results

- NotTrack
- OnTrack
- Behind
- AtRisk
