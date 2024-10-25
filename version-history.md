# V 1.0.6.0

Release Date: 10/25/2024

## New features

- Added Acronyms and Resource Links to operation `purview-migrate list-glossaries --create-import-file` operation
- Optimized adding Owners
- Added progression/status indicator for `purview-migrate list-glossaries` command

## Issues Resolved

- Resolved issue with listing duplicate Terms in purview-migrate list-glossaries
- Resolved issue with +1000 term paging 
- Resolved paging issue when assigning Ownership - paging issue occurring
- Resolved paging issue when assigning Assets to Product - paging issue occurring with numerous assets 
- Resolved migration error thrown caused by duplicate Term listing 

Release Date: 10/22/2024

## New features

- acronyms and resource links supported when importing terms to a governance domain (purview-import)
- acronyms and resource links moved when migrating terms (purview-migrate)

## Issues Resolved

- fix error when using purview-migrate caused by having 2 terms in the Azure Purview that are the same, even when they are in separate glossaries

# V 1.0.4.0

Release Date: 10/7/2024

## New features

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

## New features

- ability to list-glossaries and create file ready to import using add-terms
- Signed with EV certificate, ensuring integrity and authenticity, reducing warnings.

---

# V 0.6.0
Release Date: 9/19/2024

## New features

- User friendly names for parent/owners when listing governance domains
- Delete glossaries/terms in previous version (drain old system)
- Delete all terms in a governance domain (like a bulk undo for migrating terms)
- allow email address for --add-owners parameter when migrating terms

## Issues Resolved

- Error messaging improvements

---

# V 0.5.0
Release Date: 9/16/2024

## New features

- Audit log for bulk import
- Added OKR import
- Added Key Results import
- Renamed 'Business Domain' to 'Governance Domain' matching GA labels
- Added verbose logging option

## Issues Resolved

- Escaped formatting for list glossaries
- Resolved inclusion of data product owner when adding owner-only permissions

