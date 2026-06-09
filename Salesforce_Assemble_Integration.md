Salesforce ↔ Assemble.ai
Integration Setup Documentation
Prepared by: Ayush Makkar | Organization: iMark Infotech | Date: June 2026
1. Overview
This document outlines the step-by-step process followed to establish a connection between the Assemble.ai agent platform and the iMark Infotech Salesforce organization. The integration enables Assemble.ai agents to perform CRUD operations on Salesforce objects via the Salesforce REST API.

2. Platform Details
Field	Details
Assemble.ai Workspace	imark-technologies
Salesforce Org URL	https://imarkinfotech2.my.salesforce.com
Salesforce Admin User	ayush.makkar@imarkinfotech.com
Connected App Name	Assemble AI
Integration Type	Salesforce REST API via OAuth 2.0 Client Credentials
Skill Used	Salesforce SKILL.md (pre-loaded in Assemble.ai)

3. Prerequisites
•	Admin access to Salesforce org
•	Admin access to Assemble.ai workspace
•	Salesforce External Client App Manager enabled
•	Assemble.ai Salesforce skill already loaded (confirmed in Settings → Skills)

4. Step-by-Step Integration Process
Step 1: Create External Client App in Salesforce
1.	Navigate to Salesforce Setup → search “external” → External Client Apps → External Client App Manager
2.	Click “New” to create a new External Client App
3.	Fill in Basic Information:
•	External Client App Name: Assemble AI
•	API Name: Assemble_AI (auto-populated)
•	Contact Email: ayush.makkar@imarkinfotech.com
•	Distribution State: Local
4.	Expand “API (Enable OAuth Settings)” section and check “Enable OAuth”
5.	Set Callback URL: https://app.assemble.ai/oauth/callback
6.	Add OAuth Scopes – move the following to Selected:
•	Full access (full)
•	Manage user data via APIs (api)
•	Access the Identity URL service (id, profile, email, address, phone)
7.	Under Flow Enablement, enable:
•	Enable Client Credentials Flow
•	Enable Authorization Code and Credentials Flow
8.	Click “Create”

Step 2: Configure OAuth Policies
9.	After creation, the app opens on the Policies tab
10.	Under “OAuth Flows and External Client App Enhancements”, enable Client Credentials Flow
11.	In the “Run As” field, write username of your salesforce org not the email
12.	Leave Start Page as “None” (not needed for server-to-server API integration)
13.	Click “Save”

Step 3: Retrieve Consumer Key and Secret
14.	Navigate to the “Settings” tab of the Assemble AI connected app
15.	Copy the Consumer Key and Consumer Secret
⚠️  The Consumer Secret is only shown once at creation. Store it securely immediately.

Step 4: Store Credentials in Assemble.ai Environment
Navigate to Assemble.ai → Settings → Environment → + New Secret and add the following secrets one by one:

Secret Key	Value
SF_CLIENT_ID	Consumer Key from Salesforce Connected App
SF_CLIENT_SECRET	Consumer Secret from Salesforce Connected App
SF_USERNAME	ayush.makkar@imarkinfotech.com
SF_PASSWORD	Salesforce Password (see Section 5 — Hurdle)
SF_INSTANCE_URL	https://imarkinfotech2.my.salesforce.com

5. Hurdle Encountered: Security Token Not Available
⚠️  Issue Encountered
During the standard Salesforce Username-Password OAuth flow, a Security Token is required to be appended to the password (e.g., Password + SecurityToken). The Security Token is normally retrieved from: Salesforce Profile → Settings → My Personal Information → Reset My Security Token.
Root Cause
The “Reset My Security Token” option was not visible in the left sidebar under My Personal Information. This is a known Salesforce behavior that occurs when:
•	The Salesforce org has My Domain enabled with SSO (Single Sign-On) configured
•	IP restrictions are set to “Enforce login from trusted IP ranges only” at the org level
•	The user’s profile does not include the option due to enhanced security policies

Resolution
Since the Security Token option was not available, the SF_PASSWORD secret was set using only the Salesforce login password (without a security token appended). This works because:
•	The Salesforce org uses My Domain, which typically relaxes the security token requirement for trusted IPs
•	The OAuth 2.0 Client Credentials Flow (used by Assemble.ai) does not rely on username-password authentication at runtime — it uses the Consumer Key and Secret instead

⚠️  If authentication issues arise in future, consider resetting the security token via Salesforce Admin tools, or switching fully to Client Credentials Flow which does not require a password at all.

6. Verification
Once all 5 environment secrets are saved in Assemble.ai, the integration can be tested by running an agent task such as:
“List the 5 most recent Accounts from Salesforce”
The Salesforce skill loaded in Assemble.ai will automatically authenticate using the stored credentials and return results from the Salesforce org.

7. Summary
Component	Status	Notes
Salesforce Connected App	✅ Created	Assemble AI app in External Client App Manager
OAuth Scopes	✅ Configured	Full, API, Identity scopes
Consumer Key & Secret	✅ Retrieved	Stored securely in Assemble.ai Environment
Security Token	⚠️ Not Available	SSO/My Domain hides reset option; password used alone
Assemble.ai Environment Secrets	✅ Configured	All 5 secrets added successfully


End of Document
