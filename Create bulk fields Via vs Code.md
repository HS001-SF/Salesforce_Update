# Creating Multiple Custom Fields in Salesforce Using VS Code

## Overview

This document explains how to create multiple custom fields in the same Salesforce org by using an existing custom field as a template and deploying the modified metadata through Visual Studio Code (VS Code) with Salesforce CLI.

## Prerequisites

- Visual Studio Code
- Salesforce Extension Pack
- Salesforce CLI
- Authorized Salesforce org
- Existing custom field to use as a template

## Steps

### 1. Retrieve an Existing Custom Field

Retrieve the custom field metadata from the Salesforce org.

Example:

```
force-app/main/default/objects/Account/fields/Customer_ID__c.field-meta.xml
```

### 2. Duplicate the Field Metadata File

Create a copy of the existing field metadata file.

Example:

```
Customer_ID__c.field-meta.xml
```

Rename it to:

```
Customer_Code__c.field-meta.xml
```

> **Note:** The filename must match the new field API name.

### 3. Update the Metadata

Modify the following elements in the copied file:

#### Before

```xml
<CustomField xmlns="http://soap.sforce.com/2006/04/metadata">
    <fullName>Account.Customer_ID__c</fullName>
    <label>Customer ID</label>
    <type>Text</type>
    <length>50</length>
</CustomField>
```

#### After

```xml
<CustomField xmlns="http://soap.sforce.com/2006/04/metadata">
    <fullName>Account.Customer_Code__c</fullName>
    <label>Customer Code</label>
    <type>Text</type>
    <length>50</length>
</CustomField>
```

Update at least:

- `fullName` (API Name)
- `label`

Modify any additional properties if required.

### 4. Deploy the Metadata

Deploy the new field using VS Code.

You can:

- Right-click the file and select **SFDX: Deploy Source to Org**, or
- Use Salesforce CLI:

```bash
sf project deploy start --source-dir force-app/main/default/objects/Account/fields/Customer_Code__c.field-meta.xml
```

## Expected Result

If the deployment is successful:

- A new custom field is created.
- The original field remains unchanged.
- The new field appears on the specified object.

## Important Notes

- The API name (`fullName`) must be unique.
- The metadata filename must match the API name.
- The label does not have to be unique, but unique labels are recommended.
- Ensure all metadata is valid before deployment.
- Existing field dependencies are **not** copied automatically. Update references if necessary.

## Example Directory Structure

```
force-app/
└── main/
    └── default/
        └── objects/
            └── Account/
                └── fields/
                    ├── Customer_ID__c.field-meta.xml
                    └── Customer_Code__c.field-meta.xml
```

## Summary

| Action | Required |
|---------|----------|
| Copy existing field metadata | ✅ |
| Rename metadata file | ✅ |
| Update `fullName` | ✅ |
| Update `label` | Recommended |
| Deploy using VS Code or Salesforce CLI | ✅ |

## Outcome

By using an existing field as a template and modifying its metadata, multiple custom fields can be created efficiently within the same Salesforce org without manually creating each field through the Salesforce UI.