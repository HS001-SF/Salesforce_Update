# <a name="x75a4ffc2abd2b9c4b6a06ecbc7c9bb8295438fa"></a>SAP to Salesforce Inbound Integration – Patient Contact Synchronization

## <a name="overview"></a>Overview

This integration will enable automatic synchronization of patient contact records from SAP to Salesforce.

Whenever a new patient record is created in the SAP system, the corresponding patient/contact record will also be created automatically in Salesforce.

---

# <a name="business_requirement"></a>Business Requirement

The healthcare organization manages patient information in SAP.

To maintain synchronized patient data across systems, Salesforce should automatically receive and create patient contact records whenever a new patient is created in SAP.

---

# <a name="integration_flow"></a>Integration Flow

SAP System\
`   `↓\
REST API Request\
`   `↓\
Salesforce Inbound REST API\
`   `↓\
Contact/Patient Record Creation

---

# <a name="proposed_solution"></a>Proposed Solution

## <a name="integration_type"></a>Integration Type

Inbound REST API Integration

## <a name="source_system"></a>Source System

SAP

## <a name="target_system"></a>Target System

Salesforce Health Cloud / Salesforce CRM

## <a name="communication_method"></a>Communication Method

REST API with JSON Payload

---

# <a name="salesforce_solution_design"></a>Salesforce Solution Design

## <a name="apex_rest_api"></a>Apex REST API

A custom Apex REST API endpoint will be developed in Salesforce to receive patient data from SAP.

The API will:

- Receive patient information
- Validate required fields
- Create patient/contact records in Salesforce
- Return success or error response to SAP

---

# Data Flow Process

![](Aspose.Words.2ce7e0b3-70a8-420e-9b2f-4307a7438fc7.001.png)

# <a name="sample_patient_data"></a>Sample Patient Data

| SAP Field     | Salesforce Field    |
| :------------ | :------------------ |
| Patient ID    | External Patient ID |
| First Name    | FirstName           |
| Last Name     | LastName            |
| Phone Number  | Phone               |
| Email         | Email               |
| Date of Birth | Birthdate           |
| Gender        | Gender              |
| Address       | Mailing Address     |

---

# <a name="proposed_salesforce_api_endpoint"></a>Proposed Salesforce API Endpoint

/services/apexrest/patient

Example:

https://your-domain.my.salesforce.com/services/apexrest/patient

---

# <a name="sample_json_payload"></a>Sample JSON Payload

{\
`  `"patientId": "PAT1001",\
`  `"firstName": "Test",\
`  `"lastName": "Con",\
`  `"phone": "9999999999",\
`  `"email": "testn@con.com",\
`  `"gender": "Male"\
}

---

# <a name="security_authentication"></a>Security & Authentication

Recommended authentication approach:

- OAuth 2.0 Authentication
- Salesforce Connected App
- Secure API Communication

---

# <a name="error_handling"></a>Error Handling

The solution will support:

- Validation handling
- Error responses
- Duplicate prevention
- API exception handling
- Logging for failed transactions

---

# <a name="duplicate_prevention"></a>Duplicate Prevention

Salesforce will use:

- Patient ID OR
- External ID

to avoid duplicate patient record creation.

Recommended approach:

- Upsert operation using External Patient ID

---

# <a name="future_enhancements"></a>Future Enhancements

Possible future improvements:

- Patient update synchronization
- Appointment synchronization
- Medical record integration
- Real-time bi-directional integration
- Middleware integration (MuleSoft/SAP CPI)

---

# <a name="benefits"></a>Benefits

- Real-time patient data synchronization
- Reduced manual data entry
- Improved data accuracy
- Faster patient onboarding
- Better operational efficiency
- Centralized patient management

---

# <a name="technical_components"></a>Technical Components

| Component              | Technology              |
| :--------------------- | :---------------------- |
| Integration Type       | REST API                |
| Salesforce Development | Apex REST Service       |
| Data Format            | JSON                    |
| Authentication         | OAuth 2.0               |
| Target Object          | Contact / Patient       |
| Error Handling         | Apex Exception Handling |

---

# Configure Scoping Rules for Custom Object Record Visibility

## Overview

This document describes the implementation approach for configuring Salesforce Scoping Rules on custom objects to control record visibility based on business requirements and user-specific access conditions.

Scoping Rules help improve user experience by displaying only relevant records to users in list views, searches, and Salesforce UI components while reducing unnecessary data exposure.

# Business Requirement

The organization requires controlled visibility for custom object records so that users can only access records relevant to their assigned responsibilities, departments, business units, or operational regions.

The objective is to:

- Improve data visibility management
- Reduce unnecessary record exposure
- Enhance user experience
- Support secure and business-specific record filtering

# Objective

Implement Salesforce Scoping Rules for custom objects to:

- Filter visible records for users
- Display only relevant records
- Improve search and list view experience
- Support business-specific record visibility requirements

# Solution Overview

Salesforce Scoping Rules will be configured for the target custom object based on predefined business conditions such as:

- User Role
- Profile
- Department
- Region
- Record Ownership
- Business Unit

The configured rules will automatically filter records visible to users across Salesforce UI components.

# Implementation Flow

text id="y8k2mp" User Login ↓ Scoping Rule Evaluation ↓ Business Condition Validation ↓ Filtered Record Visibility ↓ User Views Relevant Records Only

# Solution Design

## Salesforce Feature Used

- Scoping Rules

## Target Area

- Custom Object Record Visibility

## Visibility Scope

- List Views
- Search Results
- Related Record Access
- Salesforce UI Record Display

# Step-by-Step Implementation

## Step 1 - Identify Target Custom Object

Identify the custom object where Scoping Rules need to be implemented.

Example:

- Patient Records
- Case Records
- Service Requests
- Claims Object

# Step 2 - Gather Business Conditions

Collect business requirements for record filtering.

Examples:

- Users should only see records from their assigned region.
- Users should only access department-specific records.
- Managers should view records for their business unit.

# Step 3 - Navigate to Scoping Rules Setup

In Salesforce:

text id="v3m9zx" Setup → Scoping Rules

# Step 4 - Create New Scoping Rule

Create a new Scoping Rule for the target custom object.

Provide:

- Rule Name
- Object Name
- Description

# Step 5 - Configure Rule Conditions

Define filtering conditions based on business requirements.

Example Conditions:

- Region = Current User Region
- Department = User Department
- Owner = Current User

# Step 6 - Assign Users

Specify:

- Profiles
- Roles
- Permission Sets
- User Groups

who should be affected by the rule.

# Step 7 - Activate Scoping Rule

Activate the configured rule after validation.

# Step 8 - Validate User Visibility

Test record visibility for:

- Different user roles
- Different profiles
- Different departments
- Different business units

Validate:

- List Views
- Search Results
- Record Access

# Example Business Scenario

## Scenario

The organization has regional operations:

- North Region
- South Region
- East Region
- West Region

Requirement: Users should only see records belonging to their assigned region.

## Solution

A Scoping Rule is configured to filter records where:

- Record Region = Logged-in User Region

Result: Users only see records relevant to their region.

# Benefits

- Improved user experience
- Reduced unnecessary data visibility
- Better operational efficiency
- Simplified record navigation
- Enhanced data governance
- Business-specific visibility control

# Testing Scenarios

| Scenario                    | Expected Result                    |
| --------------------------- | ---------------------------------- |
| User opens list view        | Only scoped records visible        |
| User searches records       | Filtered records displayed         |
| Different region user login | Only region-specific records shown |
| Unauthorized access attempt | Restricted records not visible     |

# Security Considerations

- Scoping Rules do not replace Salesforce sharing model.
- Existing profile permissions and sharing settings still apply.
- Scoping Rules provide additional visibility filtering.

# Dependencies

- Custom object availability
- User role configuration
- Profile setup
- Business rule confirmation
- Required fields for filtering

# Acceptance Criteria

- Scoping Rule configured successfully.
- Users can only view relevant records.
- Record filtering works in list views and searches.
- Visibility behavior validated successfully.
- Business requirements fulfilled successfully.

# Conclusion

The implementation of Salesforce Scoping Rules for custom objects will improve record visibility management and provide users with a cleaner and more relevant data access experience while supporting business-specific visibility requirements and operational efficiency.
