# Macula Purview Automate

This is a suite of tools to help migrate your data from the Azure Purview portal into the new Microsoft Purview portal, as well as assist in any bulk loading of data into the new Microsoft Purview portal.

### Purview Migrate:

A streamlined tool for migrating  glossaries and glossary terms from Azure Purview Classic, ensuring that  your data is accurately and efficiently integrated into Microsoft  Purview’s comprehensive governance framework. 

- Create Governance Domains from Glossaries
- Migrate classic Glossary Terms to new Terms
- List and Export classic Terms
- Export Term Templates
- Export Assets
- Assign Domain and Term Owners

### Purview Import:

A robust import utility that provides the greatest flexibility supporting the creation and management of governance domains, data products, data  assets and more. This tool ensures that your data governance practices  are scalable, secure, and fully aligned with Microsoft Purview’s  advanced capabilities.

- Import Governance Domains
- Import Glossary Terms
- Import Data Products
- Associate Glossary Terms
- Load and Assign Assets to Data Products
- Create and Assign Critical Data Elements
- Assign Owners (all objects)



### Additional features

- **Secure** - The Macula Purview Automate solution runs using a service principal for authentication and authorization. You will need some assistance from  your Entra Admin to help register a temporary application and service  principal that is  assigned permissions on your Microsoft Purview  portal.
- **Traceable** - The Macula Purview Automate solution keeps a localized record of all modifications to track changes to your Purview portal that can be used for auditing purposes. 
- **Valid** - Macula Purview Automate helps ensure your data is modeled and accurately imported.
- **Pre-Commits** - Review your modifications before committing. 



[Available as a download from Maculas website here](https://www.maculasys.com/microsoft-purview)



For more detailed information:

[Credentials](./credentials.md)

[Purview Migrate](./purview-migrate.md)

[Purview Import](./purview-import.md)