# V 1.0.11.0

Release Date: 02/17/2025

## New Features

- Added option to pre-append tenet ID to Purview URL (see option in credentials.json)
- Removed the auto-heirarchy parents feature that limited the number of children. Now supporting multi-level by specifying parents/child levels using the A_B_C style underscore heirarchy

## Issues Resolved

- Removed API version from URL's where no longer supported


# V 1.0.11.0

Release Date: 01/09/2025

## New Features

- Allowing for duplicate terms with different parents to pass validation
- Removed the auto-heirarchy parents feature that limited the number of children. Now supporting multi-level by specifying parents/child levels using the A_B_C style underscore heirarchy
- Escaping output for migrating terms in console markup

## Issues Resolved

- Resolved issue caused by underlying preview API when getting policies for governance domains


# V 1.0.10.2

Release Date: 12/5/2024

## New Features

- Added an override flag option to ignore URL validation when creating credentials.
- Maintenance update to support recent api updates

## Issues Resolved

- Resolved issue occuring during duplicate term validation edge case


# V 1.0.9.0

Release Date: 11/25/2024

## New Features

- Added filter capability to the `purview-migrate list-glossaries` operation to limit glossaries and terms during list or creation of import files
- Added validation for all required properties during the creation of Data Products: owners, business use, and data product type
- Added filter capability when exporting term template values using  `purview-migrate export-term-template-values` by glossary and/or specific terms
- Added validation for the presence of Owner assignment during creation of governance domains and terms during bulk import operation
- Added allowance for duplicate term names within heirarchy so long as they have different parents

## Issues Resolved

- Resolved issue occuring for displaying special characters to screen when listing term template values



# V 1.0.8.0

Release Date: 11/07/2024

## New Features

- Cascade delete now supported to bulk delete terms 
- Allowance for the creation of Terms with duplicate names within the same Governance Domain
- Support added for explicit Term heirarchy specified in csv import files created using `purview-migrate list-glossaries --create-import-file filename.csv`
- Bulk update support added for Governance Domain status updates
- Bulk update support added for Terms status update

## Issues Resolved

- Resolved NEW issue occuring during asset `purview-import add-data-product-assets` causing operation to fail



# V 1.0.7.0

Release Date: 10/29/2024

## New Features

- Progression indicator added when exporting term template values using `purview-migrate export-term-template-values`
- Added Classic Term resource link to term during migration as reference to legacy term
- Optimized term validation checks to accelerate term creation time
- Added new filter for `purview-import list-assets` to support collections and asset types
- Added Glossary filter for `purview-migrate list-glossaries` command

## Issues Resolved

- Resolved issue occuring during `purview-import add-data-product-assets` causing operation to fail


# V 1.0.6.0

Release Date: 10/25/2024

## New Features

- Added Acronyms and Resource Links to operation `purview-migrate list-glossaries --create-import-file` operation
- Optimized adding Owners
- Added progression/status indicator for `purview-migrate list-glossaries` command

## Issues Resolved

- Resolved issue with listing duplicate Terms in purview-migrate list-glossaries
- Resolved issue with +1000 term paging 
- Resolved paging issue when assigning Ownership - paging issue occurring
- Resolved paging issue when assigning Assets to Product - paging issue occurring with numerous assets 
- Resolved migration error thrown caused by duplicate Term listing 

# V 1.0.5.0

Release Date: 10/22/2024

## New Features

- acronyms and resource links supported when importing terms to a governance domain (purview-import)
- acronyms and resource links moved when migrating terms (purview-migrate)

## Issues Resolved

- fix error when using purview-migrate caused by having 2 terms in the Azure Purview that are the same, even when they are in separate glossaries

# V 1.0.4.0

Release Date: 10/7/2024

## New Features

- delete terms and domains using purview-import

## Issues Resolved

- fixed cde-column template, was outputting the okr template when cde-column template was requested

# V 1.0.3.0

Release Date: 10/1/2024

## Issues Resolved

- fixed example for migrate-terms when specifying single-governance-domain, example showed single-business

# V 1.0.2.0

Release Date: 9/26/2024

## Issues Resolved

- fixed for complex term template value exports in `purview-migrate export-term-template-values`



# V 1.0.1.0
Release Date: 9/25/2024

## Issues Resolved

- Account for multiple selections during migration. When a term template with Multiple Choice datatype is used and the term has multiple items selected, the migrate-terms as well as the export-term-template-values command handle it appropriately

---

# V 1.0.0.0
Release Date: 9/23/2024

## New Features

- ability to list-glossaries and create file ready to import using add-terms
- Signed with EV certificate, ensuring integrity and authenticity, reducing warnings.

---

# V 0.6.0
Release Date: 9/19/2024

## New Features

- User friendly names for parent/owners when listing governance domains
- Delete glossaries/terms in previous version (drain old system)
- Delete all terms in a governance domain (like a bulk undo for migrating terms)
- allow email address for --add-owners parameter when migrating terms

## Issues Resolved

- Error messaging improvements

---

# V 0.5.0
Release Date: 9/16/2024

## New Features

- Audit log for bulk import
- Added OKR import
- Added Key Results import
- Renamed 'Business Domain' to 'Governance Domain' matching GA labels
- Added verbose logging option

## Issues Resolved

- Escaped formatting for list glossaries
- Resolved inclusion of data product owner when adding owner-only permissions

