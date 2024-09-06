# Purview Migrate

Tool for migrating Glossaries and Terms for Azure Purview into Microsoft Purview

This tool is built for windows and is a command line application.

To view the help, run the command

`purview-migrate -h`

to view help for a specific command, run the command with the -h parameter

`purview-migrate list-glossar-h`

this will display the optional parameters and a description about what they are used for



This will help with the migration directly from Azure Purview Glossaries and Terms into Microsoft Purview.

The main scenarios are:

- Migrate Glossaries into Governance Domains
- Migrate Glossaries into Governance Domains and migrate all Terms under each/any Glossary into the Domain
- Migrate a subset of terms/glossaries into Governance Domains and Terms (using filtering)
- Migrate terms into a newly specified Governance Domain (terms can be filtered so not ALL go into the GD)

When mapping both Glossaries and Terms, the experts and stewards can be brought along as owners of the Governance Domain and/or Terms respectively.  In addition, new owners can be specified during the migration.

One other feature this tool has, is to be able to export custom term templates along with the terms they are associated with and their values for later export into Microsoft Purview.

The migrate-terms command has a `--dry-run` parameter, which defaults to true.  This means that the actions that would normally take place will be written to the screen, however no modifications will be made to the system.  This is intentional as a safety mechanism.  When you are ready to run the command and make changes to the destination system, use the flag `--dry-run false` to cause the new data to be sent to the Microsoft Purview instance.



*Duplicate terms per Governance Domain will not be created.*



Commands:

- [credentials](#credentials)
- [list-glossaries](#list-glossaries)
- [list-governance-domains](#list-governance-domains)
- [export-term-template-values](#export-term-template-values)
- [migrate-terms](#migrate-terms)

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




## list-glossaries

List all glossaries, and the terms in an Azure Purview instance

This is a utility to get a quick view into the source Purview system and see what glossaries and terms are available for migration. In addition, this is a good command to test connectivity to the source system after running the [credentials](#credentials) command.

*Parameters:*

> There are no additional parameters for this command

*Examples:*

`purview-migrate list-glossaries`

------




## list-governance-domains

List all Governance Domains in the destination Microsoft Purview instance

This is a utility to get a quick view into the destination Purview system and see what governance domains already exist before choosing to migrate terms and glossaries. In addition, this is a good command to test connectivity to the destination system after running the [credentials](#credentials) command.

*Parameters:*

> There are no additional parameters for this command

*Examples:*

`purview-migrate list-governance-domains`

------



## export-term-template-values

List all term templates, and their associated terms with the corresponding values in the Azure Purview instance

The output of this command is a csv list. use the > to pipe the results to a csv file.

This will archive data and have it ready for import into the Microsoft Purview system to supplement the terms themselves with the additional fields/metadata.

*Parameters:*

> There are no additional parameters for this command

*Examples:*

`purview-migrate export-term-template-values`

------



## migrate-terms

This is a powerful command and does the work when you want to migrate the Terms and optionally the Glossaries from the Azure Purview instance into the Microsoft Purview instance.

The output of this command is a csv list. use the > to pipe the results to a csv file.

This will archive data and have it ready for import into the Microsoft Purview system to supplement the terms themselves with the additional fields/metadata.

Either `--map-glossaries` OR `--single-governance-domain` is required to specify the location of the terms when they are migrated into the destination system.

*Parameters:*

> `--map-glossaries` 
>
> This parameter is a flag that indicates that the existing source Glossaries should be mapped into the destination system as Governance Domains. If used with the --filter-glossaries command, it can limit which glossaries get migrated.

> `--single-governance-domain <DOMAINNAME>` 
>
> *Example:*
>
> `--single-governance-domain BusinessTerms`
>
> This parameter specifies that all terms migrating during this command will go into a single Governance Domain, that is specified with this parameter.

> `--filter-glossaries <FILTER>`
>
> *Example:* 
>
> `--filter-glossaries Finance,Operations,IT,Shipping` 
>
> This parameter takes a comma seperated list, and limits which glossaries are selected for the migration. If this parameter is specified, ONLY glossaries that match one of the values will be used.

> `--filter-terms <FILTER>`
>
> *Example:*
>
> `--filter-terms User,Address,Person,Employee`
>
> This parameter takes a comma seperated list, and limits which terms are selected for the migration. If this parameter is specified, ONLY terms that match one of the values will be used, this filter is used in conjunction with the filter-glossaries parameter, so if filter-glossaries is specified, only terms in those glossaries will be considered for this filter. If the terms have spaces, you must quote the entire comma seperated list.
>
> If you filter terms and do NOT filter glossaries, in conjunction with the --map-glossaries parameter, ALL glossaries will be created in the downstream system, but will not have any terms unless the terms match this --filter-terms criteria.

> `--governance-domain-type <TYPE>`
>
> *Example:* 
>
> `--governance-domain-type DataDomain` 
>
> The type of governance domain(s) to create as they are created.  If not provided, FunctionalUnit will be used. Options are: FunctionalUnit, LineOfBusiness, DataDomain, Regulatory, Project.

> `--governance-domain-status <STATUS>`
>
> *Example:* 
>
> `--governance-domain-status Draft` 
>
> The status of business domain when created.  If not provided, Draft will be used. Options are: Draft, Published, Expired

> `--map-stewards`
>
> *Example:* 
>
> `--map-stewards` 
>
> If set, the stewards of the glossaries will be mapped to be owners of the governance domains. (Only upon creation of new governance domains)

> `--map-experts`
>
> *Example:* 
>
> `--map-experts` 
>
> If set, the experts of the glossaries will be mapped to be owners of the governance domains. (Only upon creation of new governance domains)

> `--map-term-stewards`
>
> *Example:* 
>
> `--map-term-stewards` 
>
> If set, the stewards of the terms will be mapped to the owners of the terms.

> `--map-term-experts`
>
> *Example:* 
>
> `--map-term-experts` 
>
> If set, the experts of the terms will be mapped to the owners of the terms.

> `--add-owners`
>
> *Example:* 
>
> `--add-owners user@domain.com,user@domain.com,56cbc2e3-d037-4800-b0ed-e32e01209c19` 
>
> Comma seperated list of ObjectId's and/or email addresses (if Entra permission User.ReadAll is assigned to the user) that represent users that will be added as owners of the governance domain(s) created. (Only upon creation of new governance domains)

> `--owner-only`
>
> *Example:* 
>
> `--owner-only` 
>
> Only add owners to the governance domain owner role, not to all roles for the governance domain.

> `--map-gd-only`
>
> *Example:* 
>
> `--map-gd-only` 
>
> Do not map terms, only map governance domains.

> `--dry-run`
>
> *Example:* 
>
> `--dry-run false` 
>
> specifying whether the command is a dry run or not, the default is true, and must be specified as false explicitly when the command is intended to import data



*Examples:*

`purview-migrate migrate-terms --filter-glossaries "glossary1,glossary2" --filter-terms "term1,term2" --map-glossaries --business-domain-type FunctionalUnit --business-domain-status Draft --map-stewards --map-experts --add-owners "f135749f-eed0-4cb9-1141-9f6a574212c0,3abe053d-c0c3-4c5a-807c-4d7d448e2532" --map-term-stewards --map-term-experts --dry-run`

`purview-migrate migrate-terms --filter-glossaries "glossary1,glossary2" --filter-terms "term1,term2" --single-business-domain "Destination Business Domain" --business-domain-type FunctionalUnit --business-domain-status Draft --map-stewards --map-experts --add-owners "user@domain.com,user2@domain.com,3abe053d-c0c3-4c5a-807c-4d7d448e2532"`

------

