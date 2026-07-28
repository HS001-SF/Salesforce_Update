# Dynamic Reports using Lightning Web Components (LWC)

## Overview
Build a reusable reporting solution using LWC and Apex.

## Features
- Dynamic Object Selection
- Dynamic Field Selection
- Dynamic SOQL
- Dynamic Filters
- Lightning Datatable
- Sorting
- Pagination
- CSV Export
- CRUD/FLS Security

## Architecture
Dynamic Reports LWC -> Apex Controller -> Dynamic SOQL -> Salesforce Data -> Lightning Datatable

## Flow
1. Select Object
2. Load Fields
3. Select Columns
4. Apply Filters
5. Generate SOQL
6. Display Data
7. Export CSV

## Apex Methods
- getObjects()
- getFields(objectName)
- getReportData(objectName, fields, filters)

Example SOQL:
SELECT Name, Industry, Phone FROM Account WHERE Industry='Banking' LIMIT 500;

## Security
- CRUD/FLS Validation
- Prevent SOQL Injection

## Future Enhancements
- Save Reports
- Dashboard Charts
- PDF/Excel Export
- Scheduled Reports
- Aggregate Reports

## Technology Stack
- LWC
- Apex
- SOQL
- Lightning Datatable
- JavaScript
