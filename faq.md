# Frequently Asked Questions

We continue to add to the list of FAQ's so please check back again if you're question isn't captured or answer found in the help documentation. 

1. [**What is the availability of the tool?**](#what-is-the-availability-of-the-tool)
2. [**How do users access the tool?**](#how-do-users-access-the-tool)
3. [**What can the tool support, especially concerning glossary, meta model, and old concepts?**](#what-can-the-tool-support-especially-concerning-glossary-meta-model-and-old-concepts)
4. [**Does the tool create governance domains, data products, or other constructs if they don't exist, or do they need to be pre-created?**](#does-macula-purview-automate-create-governance-domains-data-products-or-other-constructs-if-they-dont-exist-or-do-they-need-to-be-pre-constructed)
5. [**What kind of permissions does the tool require, and how is it managed particularly regarding app registration and credentials?**](#what-kind-of-permissions-does-the-tool-require-and-how-is-it-managed-in-particularly-regarding-app-registration-and-credentials)
6. [**How are managed attributes handled by the tool?**](#how-are-managed-attributes-handled-by-the-tool)
7. [**Is there a rollback option if something goes wrong during migration?**](#is-there-a-rollback-option-if-something-goes-wrong-during-migration)
8. [**How will the tool handle security and audit requirements, particularly regarding data export/import?**](#how-will-the-tool-handle-security-and-audit-requirements-particularly-regarding-data-exportimport)
9. [**Can the tool handle hierarchical glossaries from the old system?**](#can-the-tool-handle-hierarchical-glossaries-from-the-old-system)
10. [**How does the tool handle error validation, especially when data products or assets are not created as expected?**](#how-does-the-tool-handle-error-validation-especially-when-data-products-or-assets-are-not-created-as-expected)
11. [**What is the process for adding business domain owners and ensuring proper permissions?**](#what-is-the-process-for-adding-business-domain-owners-and-ensuring-proper-permissions)
12. **[Do the templates require the headers when running add-* commands](#do-the-templates-require-the-headers-when-running-add--commands)**
13. **[How do I grant access for adding business concepts to existing Governance Domains?](#how-do-i-grant-access-for-adding-business-concepts-to-existing-governance-domains)**



## What is the availability of the tool? 

Macula Purview Automate V1 will be publicly available on 9/12/2024 for free to customers.  

## How do users access the tool?

Please visit https://www.maculasys.com/microsoft-purview to learn about Macula Purview Automate and access a link to download the solution.

## What can the tool support, especially concerning glossary, meta model, and old concepts?

Please visit https://github.com/Macula-Systems-LLC/purview-automation to view solution help documentation and examples on how to initially configure and use the migrate and import utilities.  Macula Purview Automate supports migrating Glossaries, Terms, and Assets into Microsoft Purview related concepts.  It also facilitates the creation of Governance Domains, Data Products, OKR's, Critical Data Elements from earlier concepts or brand new.  

## Does Macula Purview Automate create governance domains, data products, or other constructs if they don't exist, or do they need to be pre-constructed?

Macula Purview Automate provides the flexibility to create new governance domains when migrating glossaries and glossary terms.  Governance Domains can be created from existing glossaries or user specified.  Similarly, Data Products can be created from existing Assets or new when importing existing data assets.

## What kind of permissions does the tool require, and how is it managed, in particularly regarding app registration and credentials?

The current version uses a registered application and security principal that has been granted Purview permissions to perform the necessary operations. Please see help documentation regarding "credentials" to learn more and how to set up prior to migrating or importing data.

## How are managed attributes handled by the tool?

As of the time of this version release, Managed Attributes is still under development and a prioritized feature in an upcoming release.

## Is there a rollback option if something goes wrong during migration?

There is no rollback option.  However, safeguards have been added to the solution to validate data imports and desired relationships.  In addition, when executing a command, users are presented with a draft-view of the to-be modifications before committing their changes.  Migration and import does not have to be an "all or nothing" event.  The tool supports small-batch execution which can be helpful when developing a transition plan.

## How will the tool handle security and audit requirements, particularly regarding data export/import?

Each operation is captured identifying who made the modifications and what modifications where applied.

## Can the tool handle hierarchical glossaries from the old system?

Yes.  Import templates support parent-child relationships for many former and new concepts.

## How does the tool handle error validation, especially when data products or assets are not created as expected?

Prior to committing changes, users are presented the to-be modifications for their review which include any warnings or conflicts that have been identified.

## What is the process for adding governance domain owners and ensuring proper permissions?

Users have the option when migrating Glossaries and Terms to retain their same ownership or assign them to different owners.  If creating new Governance Domains, new owners are assigned at that time.

## Do the templates require the headers when running add-* commands?

Yes, the templates, when filled out require that the header row is present for using as input to importing data using the purview-import tool

## How do i grant access for adding business concepts to existing Governance Domains?

If you wish to add terms, or other business concepts to an existing business domain, the principal user must be assigned to the following roles within the governance domain itself.

- Governance Domain Owners
- Governance Domain Readers
- Data Steward
