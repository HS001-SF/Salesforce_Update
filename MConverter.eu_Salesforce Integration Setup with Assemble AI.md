# Salesforce Integration Setup with Assemble AI

## Overview

This document explains the step-by-step process to connect Salesforce with Assemble AI using OAuth authentication and a Salesforce Connected App.

The integration allows Assemble AI to securely access Salesforce APIs and interact with Salesforce data.

------------------------------------------------------------------------

# Objective

The purpose of this setup is to establish a secure connection between:

- Salesforce
- Assemble AI Workspace

Once connected, Assemble AI can:

- Access Salesforce data
- Read and create records
- Integrate AI workflows with Salesforce
- Enable automation and AI-powered use cases

------------------------------------------------------------------------

# Integration Architecture

Assemble AI\
↓\
OAuth Authentication\
↓\
Salesforce Connected App\
↓\
Salesforce APIs

------------------------------------------------------------------------

# Step 1 --- Create Connected App in Salesforce {#step-1-create-connected-app-in-salesforce}

Navigate to:

Setup\
→ App Manager\
→ New Connected App

------------------------------------------------------------------------

# Step 2 --- Enter Basic Information {#step-2-enter-basic-information}

Fill in the following details:

| Field              | Value                   |
|--------------------|-------------------------|
| Connected App Name | Assemble AI Integration |
| API Name           | Auto Generated          |
| Contact Email      | Your Email Address      |

------------------------------------------------------------------------

# Step 3 --- Enable OAuth Settings {#step-3-enable-oauth-settings}

Enable the option:

✔ Enable OAuth Settings

------------------------------------------------------------------------

# Step 4 --- Add Callback URL {#step-4-add-callback-url}

Use the following callback URL provided by Assemble AI:

https://app.assemble.ai/oauth/callback

------------------------------------------------------------------------

# Step 5 --- Add OAuth Scopes {#step-5-add-oauth-scopes}

Add the following OAuth scopes:

- Full Access (full)
- Perform requests on your behalf at any time (refresh_token, offline_access)

These permissions allow Assemble AI to securely communicate with Salesforce APIs.

------------------------------------------------------------------------

# Step 6 --- Save Connected App {#step-6-save-connected-app}

Click:

Save

After saving, wait approximately 5--10 minutes for the Connected App to become active.

------------------------------------------------------------------------

# Step 7 --- Copy Consumer Key and Consumer Secret {#step-7-copy-consumer-key-and-consumer-secret}

Open the Connected App and copy the following values:

- Consumer Key
- Consumer Secret

These credentials will be used inside Assemble AI.

------------------------------------------------------------------------

# Step 8 --- Open Assemble AI Workspace {#step-8-open-assemble-ai-workspace}

Login to Assemble AI Workspace.

Navigate to the Salesforce Integration section.

------------------------------------------------------------------------

# Step 9 --- Configure Salesforce Connection in Assemble AI {#step-9-configure-salesforce-connection-in-assemble-ai}

Enter the following details:

| Field         | Value                      |
|---------------|----------------------------|
| Client ID     | Salesforce Consumer Key    |
| Client Secret | Salesforce Consumer Secret |
| Instance URL  | Salesforce Org URL         |

Example Salesforce Instance URL:

https://yourorg.my.salesforce.com

For Sandbox:

https://yourorg--uat.sandbox.my.salesforce.com

------------------------------------------------------------------------

# Step 10 --- Authorize Salesforce {#step-10-authorize-salesforce}

Click:

Connect Salesforce

You will be redirected to the Salesforce login page.

Login using Salesforce credentials and click:

Allow Access

------------------------------------------------------------------------

# Step 11 --- Verify Integration {#step-11-verify-integration}

Once authorization is successful:

- Salesforce org will connect with Assemble AI
- Assemble AI can access Salesforce APIs
- Integration setup will be completed successfully

------------------------------------------------------------------------

# Security Best Practices

Recommended best practices:

- Use a dedicated Integration User
- Avoid using System Administrator accounts
- Provide only required object permissions
- Keep Consumer Secret secure
- Use OAuth authentication for secure communication

------------------------------------------------------------------------

# Final Outcome

After successful setup:

✔ Salesforce and Assemble AI will be connected\
✔ OAuth authentication will be enabled\
✔ Assemble AI can securely interact with Salesforce data\
✔ APIs and automation workflows can be executed successfully

------------------------------------------------------------------------
