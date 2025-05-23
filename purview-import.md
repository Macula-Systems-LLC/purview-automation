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
- [lookup-user](#lookup-user)
- [list-assets](#list-assets)
- [list-governance-domains](#list-governance-domains)
- [delete-governance-domain](#delete-governance-domain)
- [add-governance-domains](#add-governance-domains)
- [add-governance-domain-owners](#add-governance-domain-owners)
- [list-terms](#list-terms)
- [add-terms](#add-terms)
- [delete-terms](#delete-terms)
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




## lookup-user

Lookup a user in MS Entra Graph API by email or object id

This is a utility to determine a user based on their object id or their email address.  It just returns the json string that represents the user in Entra, but can be very useful when needing to determine the owner/steward/expert when the api only returns the guid object id.

This can be run with the `--user` parameter that could be the Entra object id (guid) or the users email address.

*Parameters:*

> `--user`
>
> The user identifier, such as an email address or a entra object id for a user

*Examples:*

`purview-import lookup-user --user 21e8c3e5-5776-4b5b-8cbf-881e8eb5ff3f`

`purview-import lookup-user --user bob@test.com`

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
> The csv file to write the asset list to, but in the format that is ready for import as data products.

> `--ignore-azp` or `-i`
>
> Ignore azure purview api calls, and only query the MS Purview api for assets

> `--tables-only` or `-t`
>
> Only list SQL Azure tables

> `--filter-type <FILTER>`
>
> Filter by asset type, where the asset type matches the string entirely

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
> The file to write the csv output to

*Examples:*

`purview-import list-governance-domains --file governance_domains.csv`



------

## delete-governance-domain

Delete a governance domain in the Microsoft Purview instance.  The domain cannot have any child entities such as glossary terms or data products or OKR's. You can specify the Domain Name or the Domain Id

*Parameters:*

> `-d <Governance Domain>`

*Examples:*

`purview-import delete-governance-domain -d "My Governance Domain"`

`purview-import delete-governance-domain -d 7417a2b8-7fe4-4307-b6b2-a36706ca4fd3`

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
> The csv file that contains the data to import

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
> The file to write the csv output to

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

## delete-terms

Delete all glossary terms within a specified governance domain in the Microsoft Purview instance

This currently can only delete ALL terms from the governance domain.

*Parameters:*

> `--governance-domain "The Domain Name"`
>
> The governance domain that the terms should be deleted from

*Examples:*

`purview-import delete-terms --governance-domain "The Domain Name"`

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

Add critical data elements to the Microsoft Purview instance using the csv template as input

*Parameters:*

> `--template`
>
> This writes the csv template headers directly to the console. It can be piped to a csv file using the > filename.csv
>

> `--enums`
>
> List enums for use in the add cdes creation template

> `--file <FILE>`
>
> The csv file that contains the data to import

> `--dry-run false`
>
> specifying whether a command is a dry run or not, the default is true, and must be specified as false explicitly when the command is intended to import data

*Enums:*

[Status](#status)

[Data Types](#data-types)

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

Add data products to the Microsoft Purview instance using the csv template as input. This template is unique, in that it has a column for assetsids (that does NOT require a value), which can be a comma seperated list of guid's that represent data assets that will be associated with the data products immediately during the creation using this command. Saving the step of creating the data product and then associating the assets after the fact during a subsequent step. If no asset id's are specified, the data product is still created as normal.

*See [List Data Assets](#list-assets) for a way to export data assets in this format for easy data-asset/data-product mapping*

*Parameters:*

> `--template`
>
> This writes the csv template headers directly to the console. It can be piped to a csv file using the > filename.csv
>

> `--enums`
>
> List enums for use in the add data products creation template

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
> List enums for use in the add data product terms relationship creation template

> `--file <FILE>`
>
> The csv file that contains the data to import

> `--dry-run false`
>
> specifying whether a command is a dry run or not, the default is true, and must be specified as false explicitly when the command is intended to import data

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
> The file to write the csv output to

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

`purview-import list-key-results--file allkeyresults.csv`



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

`purview-import list-okr-dp--file allokrdataproducts.csv`



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
