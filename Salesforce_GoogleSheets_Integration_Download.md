# Salesforce & Google Sheets Integration Architecture
## Real-Time Sync & Historical Data Handling

This document outlines the architectural approach and implementation steps for syncing Google Sheets data with Salesforce Accounts, encompassing both bulk historical uploads and real-time record creation.

---

## Phase 1: Historical Data Migration

For the pre-existing data within the Google Sheet, a bulk operation is the most efficient and robust approach.

1. **Export:** Download the existing Google Sheet as a `.csv` format.
2. **Data Preparation:** Ensure columns map cleanly to standard and custom Account fields (e.g., `Name`, `Phone`, `Website`, `BillingCity`).
3. **Execution:** Utilize the **Salesforce Data Loader** to execute an `Insert` operation. This establishes the baseline data volume while bypassing limits associated with API callouts for bulk historical data.

---

## Phase 2: Real-Time Integration (New Data)

To handle real-time Account creation upon the addition of new rows in Google Sheets, a custom integration using Google Apps Script and a custom Apex REST endpoint provides maximum control and scalability without relying on paid middleware.

### 1. Salesforce: Custom Apex REST Endpoint

A custom endpoint handles the incoming JSON payload from Google Sheets and performs the necessary DML operations.

```java
@RestResource(urlMapping='/GoogleSheetsIntegration/v1/Account/*')
global with sharing class GoogleSheetsAccountService {
    
    @HttpPost
    global static String createAccount(String accountName, String phone, String industry) {
        RestResponse res = RestContext.response;
        
        try {
            Account newAcc = new Account(
                Name = accountName,
                Phone = phone,
                Industry = industry,
                AccountSource = 'Google Sheets Integration' // Useful for reporting
            );
            
            insert newAcc;
            
            res.statusCode = 201;
            return 'Success: Account created with ID ' + newAcc.Id;
            
        } catch(DmlException e) {
            res.statusCode = 500;
            return 'Error: ' + e.getMessage();
        }
    }
}
```

### 2. Google Workspace: Apps Script Setup

Attach an Apps Script to the target Google Sheet utilizing an `onChange` or `onEdit` trigger to detect new rows, formulate the payload, and dispatch the HTTP POST request to Salesforce.

```javascript
function onEditTrigger(e) {
  const sheet = e.source.getActiveSheet();
  const range = e.range;
  
  // Basic validation to ensure we are only firing on new row additions
  if (range.getColumn() === 1 && e.value) { 
    const rowNum = range.getRow();
    const accountName = sheet.getRange(rowNum, 1).getValue();
    const phone = sheet.getRange(rowNum, 2).getValue();
    const industry = sheet.getRange(rowNum, 3).getValue();
    
    sendToSalesforce(accountName, phone, industry);
  }
}

function sendToSalesforce(name, phone, industry) {
  // Replace with your Salesforce access token retrieval logic
  const accessToken = 'YOUR_SALESFORCE_ACCESS_TOKEN'; 
  const endpoint = 'https://yourdomain.my.salesforce.com/services/apexrest/GoogleSheetsIntegration/v1/Account/';
  
  const payload = {
    "accountName": name,
    "phone": phone,
    "industry": industry
  };
  
  const options = {
    'method': 'post',
    'contentType': 'application/json',
    'headers': {
      'Authorization': 'Bearer ' + accessToken
    },
    'payload': JSON.stringify(payload)
  };
  
  try {
    UrlFetchApp.fetch(endpoint, options);
  } catch (error) {
    console.error('Error posting to Salesforce: ', error);
  }
}
```

---

## Phase 3: Reporting & Analytics

Once the real-time pipeline is operational, the data can be aggregated seamlessly within standard Salesforce reporting mechanisms.

1. **Report Type:** Utilize the standard **Accounts** report type.
2. **Filtering:** Filter the report using the specific identifier set during DML (e.g., `Account Source` = 'Google Sheets Integration').
3. **Dashboards:** Build dynamic dashboard components (donut charts for Industry breakdown, bar charts for Account creation volume over time) to monitor the ongoing data flow from the external sheet.
