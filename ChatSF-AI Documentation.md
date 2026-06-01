# ChatSF-AI Documentation

## 1. Introduction {#introduction}

### Project Name

ChatSF-AI

### Overview

ChatSF-AI is a custom AI-powered Salesforce solution that integrates Groq AI into Salesforce using Apex callouts and Lightning Web Components (LWC).

The application allows users to:

- Send prompts to AI
- Receive AI-generated responses inside Salesforce
- Store chat conversations in Salesforce custom objects

------------------------------------------------------------------------

# 2. Features {#features}

## AI Chat Integration

Users can ask questions directly from Salesforce UI and receive AI-generated responses.

## Chat History Storage

All user prompts and AI responses are stored in Salesforce for future reference.

## Real-Time AI Responses

The system communicates with Groq AI APIs and displays responses instantly.

------------------------------------------------------------------------

# 3. Technologies Used {#technologies-used}

| Technology                     | Purpose                  |
|--------------------------------|--------------------------|
| Salesforce Apex                | Backend Logic            |
| Lightning Web Components (LWC) | Frontend UI              |
| Groq AI API                    | AI Response Generation   |
| HTTP Callouts                  | External API Integration |
| Custom Object                  | Chat History Storage     |

------------------------------------------------------------------------

# 4. Application Flow {#application-flow}

## Process Flow

1.  User enters a message in ChatSF-AI
2.  LWC sends request to Apex Controller
3.  Apex sends HTTP callout to Groq AI
4.  Groq AI generates response
5.  Salesforce receives AI response
6.  Chat history is stored
7.  Response is displayed in UI

------------------------------------------------------------------------

# 5. Salesforce Configuration {#salesforce-configuration}

## Step 1: Create Custom Object

### Object Name

Chat History

### API Name

Chat_History\_\_c

### Required Fields

| Field Label  | API Name          | Type           |
|--------------|-------------------|----------------|
| User Message | User_Message\_\_c | Long Text Area |
| AI Response  | AI_Response\_\_c  | Long Text Area |

------------------------------------------------------------------------

# 6. Named Credential Setup {#named-credential-setup}

## Step 1

Go to: Setup → Named Credentials

## Step 2

Create New Named Credential

| Field | Value                |
|-------|----------------------|
| Label | Groq                 |
| Name  | Groq                 |
| URL   | https://api.groq.com |

------------------------------------------------------------------------

# 7. Apex Controller {#apex-controller}

## Class Name

GroqController

## Responsibilities

- Accept user messages
- Send API requests to Groq AI
- Receive AI responses
- Save chat history
- Return responses to LWC

------------------------------------------------------------------------

# 8. Lightning Web Component {#lightning-web-component}

## Component Features

- User chat interface
- Input text box
- Send button
- AI response display
- Chat history rendering

## Files Used

| File                 | Purpose                 |
|----------------------|-------------------------|
| chatsfAi.html        | UI Design               |
| chatsfAi.js          | Client-side Logic       |
| chatsfAi.css         | Styling                 |
| chatsfAi.js-meta.xml | Component Configuration |

# 10. Chat History Storage {#chat-history-storage-1}

Every interaction is stored in Salesforce automatically.

## Stored Information

- User Message
- AI Response
- Created Date

------------------------------------------------------------------------

# 11. How to Use ChatSF-AI {#how-to-use-chatsf-ai}

## Step 1

Open Salesforce App

## Step 2

Navigate to ChatSF-AI Component

## Step 3

Enter your message

Example:

- Explain Salesforce Flow
- What is Apex?
- Difference between Trigger and Flow

## Step 4

Click Send

## Step 5

View AI-generated response

![](media/image1.png){width="6.5in" height="2.576388888888889in"}

------------------------------------------------------------------------

# 12. Error Handling {#error-handling}

## Current Validations

- HTTP response validation
- API status code checks
- Empty input prevention

------------------------------------------------------------------------

# 13. Future Enhancements {#future-enhancements}

Planned future improvements:

- Salesforce org metadata queries
- User and object information retrieval
- Multi-chat conversations
- Streaming AI responses
- Voice support
- AI analytics dashboard

------------------------------------------------------------------------

# 14. Conclusion {#conclusion}

ChatSF-AI demonstrates how external AI services can be integrated into Salesforce using Apex callouts and Lightning Web Components. The solution provides a simple and effective AI chat experience directly inside Salesforce.
