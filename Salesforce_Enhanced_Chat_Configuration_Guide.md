# Salesforce Enhanced Chat (Messaging for In-App and Web) - Configuration Guide

## Objective
Configure Salesforce Enhanced Chat to allow customers to communicate with service agents through Omni-Channel.

## Prerequisites
- Service Cloud
- Omni-Channel enabled
- Service Console
- Appropriate user permissions
- Digital Engagement / Messaging feature (if required)

## Configuration Steps

### 1. Configure User Access
- Create/update a Permission Set.
- Grant Messaging and Omni-Channel permissions.
- Assign it to service agents.

### 2. Enable Omni-Channel
- Enable Omni-Channel.
- Create Presence Status.
- Create Presence Configuration.
- Create Queue.
- Create Routing Configuration.
- Create Service Channel.
- Add Omni-Channel utility to Service Console.

### 3. Configure Omni Flow
Use Salesforce standard Omni Flow:
- Chats Routed to Agents
- Chats Routed to Agents with the Right Skills

Verify the flow is activated and the Route Work element references the correct Service Channel, Queue, and Routing Configuration.
Create a custom Omni Flow only for custom routing requirements.

### 4. Create Messaging Channel
- Create an Enhanced Chat channel.
- Select Queue.
- Select Routing Configuration.
- Select Omni Flow.

### 5. Configure Deployment
- Branding
- Pre-chat form
- Business hours
- Publish deployment

### 6. Testing
- Start a chat.
- Verify Messaging Session creation.
- Verify routing to the available agent.

## Process Flow

```text
Customer Starts Chat
      |
      v
Enhanced Chat Widget
      |
      v
Messaging Session
      |
      v
Omni Flow
      |
      v
Route Work
      |
      v
Queue
      |
      v
Omni-Channel
      |
      v
Available Agent
```

## Benefits
- Automatic routing
- Skill-based routing
- Improved agent productivity
- Better customer experience
