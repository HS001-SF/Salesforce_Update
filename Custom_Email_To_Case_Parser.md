# Custom Email-to-Case & Account Matching Integration

## Overview
This document outlines the setup and Apex implementation for a custom Email Service in Salesforce. This solution bypasses standard Email-to-Case to parse highly structured email bodies, map custom and standard Case fields, and automatically link or create associated Account records based on the email payload.

---

## 1. Required Email Template Structure

The external system or user sending the email must adhere to a strict `Key: Value` format. 
**Important:** The `Description:` tag must always remain at the very end of the email to properly capture multi-line text.

```text
Account Name: Acme Corporation
Product: GC1040
Type: Problem
Case Reason: Installation
Web Name: Jane Smith
Web Email: jane.smith@example.com
Case Origin: Email
Subject: System Outage
Description: 
This is a detailed description.
It can span multiple lines.
```

---

## 2. Apex Implementation (Inbound Email Handler)

This class parses the email body, handles automated Gmail verification emails, extracts the specific values, matches/creates the Account, and inserts the Case.

```java
global class StructuredEmailToCaseHandler implements Messaging.InboundEmailHandler {
    
    global Messaging.InboundEmailResult handleInboundEmail(Messaging.InboundEmail email, Messaging.InboundEnvelope envelope) {
        Messaging.InboundEmailResult result = new Messaging.InboundEmailResult();
        
        try {
            String plainTextBody = email.plainTextBody;
            
            if (String.isBlank(plainTextBody)) {
                result.success = false;
                result.message = 'Email body is empty.';
                return result;
            }
            
            Case newCase = new Case();
            
            // =========================================================
            // Gmail Verification Catcher
            // =========================================================
            if (email.subject != null && email.subject.containsIgnoreCase('Forwarding Confirmation')) {
                newCase.Subject = email.subject;
                newCase.Description = plainTextBody; 
                newCase.Origin = 'Email';
                
                insert newCase;
                result.success = true;
                return result; 
            }
            
            // =========================================================
            // Standard Parsing Logic
            // =========================================================
            List<String> emailLines = plainTextBody.split('\n');
            Boolean isDescription = false;
            String parsedDescription = '';
            String parsedAccountName = ''; 
            
            for (String line : emailLines) {
                line = line.trim();
                
                if (isDescription) {
                    parsedDescription += line + '\n';
                    continue; 
                }
                
                // Extract Account Name
                if (line.startsWithIgnoreCase('Account Name:')) {
                    parsedAccountName = line.substringAfter(':').trim();
                }
                // Extract Custom / Standard Case Fields
                else if (line.startsWithIgnoreCase('Product:')) {
                    newCase.Product__c = line.substringAfter(':').trim(); // Ensure API name matches your org
                }
                else if (line.startsWithIgnoreCase('Type:')) {
                    newCase.Type = line.substringAfter(':').trim();
                }
                else if (line.startsWithIgnoreCase('Case Reason:')) {
                    newCase.Reason = line.substringAfter(':').trim(); 
                }
                else if (line.startsWithIgnoreCase('Web Name:')) {
                    newCase.SuppliedName = line.substringAfter(':').trim();
                } 
                else if (line.startsWithIgnoreCase('Web Email:')) {
                    newCase.SuppliedEmail = line.substringAfter(':').trim();
                } 
                else if (line.startsWithIgnoreCase('Case Origin:')) {
                    newCase.Origin = line.substringAfter(':').trim();
                } 
                else if (line.startsWithIgnoreCase('Subject:')) {
                    newCase.Subject = line.substringAfter(':').trim();
                } 
                else if (line.startsWithIgnoreCase('Description:')) {
                    isDescription = true; 
                    String inlineDesc = line.substringAfter(':').trim();
                    if (String.isNotBlank(inlineDesc)) {
                        parsedDescription += inlineDesc + '\n';
                    }
                }
            }
            
            // Fallbacks for missing fields
            newCase.Description = String.isNotBlank(parsedDescription) ? parsedDescription.trim() : plainTextBody; 
            newCase.Origin = String.isBlank(newCase.Origin) ? 'Email' : newCase.Origin;
            newCase.Subject = String.isBlank(newCase.Subject) ? email.subject : newCase.Subject;
            
            // =========================================================
            // Account Matching & Creation Logic
            // =========================================================
            if (String.isNotBlank(parsedAccountName)) {
                List<Account> existingAccounts = [SELECT Id FROM Account WHERE Name = :parsedAccountName LIMIT 1];
                
                if (!existingAccounts.isEmpty()) {
                    newCase.AccountId = existingAccounts[0].Id;
                } else {
                    Account newAcc = new Account(Name = parsedAccountName);
                    insert newAcc;
                    newCase.AccountId = newAcc.Id;
                }
            }
            
            // Insert the Case
            insert newCase;
            result.success = true;
            
            } catch (Exception e) {
                System.debug('Error in StructuredEmailToCaseHandler: ' + e.getMessage());
                result.success = false;
                result.message = 'An error occurred: ' + e.getMessage();
            }
            
            return result;
        }
    }
```

---

## 3. Configuration & Gotchas

*   **Accept Email From:** In the Email Service configuration within Salesforce, ensure the "Accept Email From" field is blank during initial setup and testing to prevent Salesforce from silently dropping unauthorized emails.
*   **Context User Permissions:** The Context User assigned to the Email Address must have 'Create' access to the `Account` and `Case` objects, as well as access to all referenced fields.
*   **Duplicate Rules:** If strict Account matching/duplicate rules are active, the `insert newAcc;` statement may fail. Adjust matching rules or handle DML exceptions accordingly.
*   **Picklist Values:** Parsed text for picklist fields (e.g., `Type`, `Case Reason`) must precisely match the spelling of active values in the Salesforce org.
