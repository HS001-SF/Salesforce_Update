I started the day by reviewing a client interview recording as a reference for learning. This helped me understand how real-world client interactions are handled, the type of questions discussed during requirement gathering, and the overall communication approach followed in project discussions. Observing these conversations provided valuable insights into client expectations and industry best practices.

I also spent time working on Salesforce Trailhead challenges to strengthen my platform knowledge and improve my hands-on skills. Completing these challenges allowed me to earn additional Trailhead points while reinforcing my understanding of various Salesforce features and functionalities through practical exercises.

In addition, I dedicated time to studying the fundamentals of Salesforce Education Cloud. I explored its core purpose, key features, and how it helps educational institutions manage student relationships, recruitment processes and admissions.

Overall, the day was productive and focused on continuous learning, practical skill development, and expanding my knowledge of Salesforce products and solutions.



# End of Day Report
**Date:** June 9, 2026

---

Today marked a productive step forward in expanding my technical capabilities, with a focused effort on exploring and implementing AI-driven tools within the Salesforce ecosystem.

The day began with initiating structured learning around **Agentforce**, Salesforce's native AI agent platform. This involved diving into the foundational concepts of how autonomous agents operate within the Salesforce environment — understanding their architecture, capabilities, and real-world applications in enterprise workflows.

To reinforce this learning, I completed several **Trailhead modules** related to Agentforce, earning the associated points along the way. The courses covered the core building blocks of Agentforce and provided hands-on exercises that helped solidify the theoretical concepts into practical understanding.

On the integration front, a significant milestone was achieved today — successfully **connecting my Salesforce Developer org with Assemble.ai**. This integration sets the foundation for leveraging AI agents to automate and operate tasks across enterprise systems directly from within the Salesforce environment, opening up possibilities for intelligent workflow automation going forward.

Overall, today's work was centered around building a stronger understanding of the AI-agent landscape within Salesforce, while taking a concrete step toward an integrated, agent-driven setup.

---

# End of Day Report

**Date:** July 7, 2026
**Prepared by:** Aniket
**Role:** Salesforce Developer, iMark Infotech Pvt. Ltd.

---

## Summary

Dedicated the day to revising and deepening knowledge of **Salesforce Integration fundamentals**, covering the full stack of concepts from basic API theory through to Salesforce-specific API types. This builds a strong foundation for understanding how Salesforce communicates with external systems and vice versa.

---

## Work Completed

### 1. API Fundamentals

- Revised the concept of an **API (Application Programming Interface)** — how it acts as a contract between two systems, defining how requests should be made and what responses to expect, without either side needing to know the other's internal implementation.

### 2. REST Architecture

- Studied **REST (Representational State Transfer)** as an architectural style for building APIs — stateless, resource-based, and operating over HTTP.
- Understood why REST has become the dominant API style for modern web and cloud integrations due to its simplicity and scalability.

### 3. HTTP Protocol

- Revised how **HTTP (HyperText Transfer Protocol)** works as the communication layer underlying REST APIs.
- Covered the client-server model: how a client initiates a request and a server returns a response over HTTP.

### 4. Request & Response Structure

- Studied the anatomy of an HTTP **Request** (method, URL, headers, body) and **Response** (status code, headers, body).
- Understood how these two structures form the complete cycle of a single API interaction.

### 5. HTTP Methods

- Revised the core HTTP methods used in REST APIs:
  - **GET** — retrieve a resource
  - **POST** — create a new resource
  - **PUT / PATCH** — update an existing resource (full vs. partial)
  - **DELETE** — remove a resource
- Understood how these map to CRUD operations in data systems like Salesforce.

### 6. Headers

- Studied HTTP **Headers** as metadata attached to requests and responses — covering common headers like `Content-Type`, `Authorization`, `Accept`, and how they control behavior on both ends of the API call.

### 7. Body

- Revised the **Request/Response Body** — the payload of data sent or received in an API call, typically formatted as JSON or XML.

### 8. JSON

- Revised **JSON (JavaScript Object Notation)** as the standard data format for REST API communication — its structure (objects, arrays, key-value pairs), and why it is preferred over XML for its readability and lightweight nature.
- Reinforced how Salesforce APIs return and accept data in JSON format.

### 9. HTTP Status Codes

- Studied the key **HTTP Status Code** categories:
  - **2xx** — Success (200 OK, 201 Created, 204 No Content)
  - **4xx** — Client errors (400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found)
  - **5xx** — Server errors (500 Internal Server Error)
- Understanding status codes is critical for building reliable error handling in integration code.

### 10. REST vs SOAP

- Compared **REST and SOAP** as the two dominant API paradigms:
  - SOAP uses XML, has strict standards (WSDL), and is more suited to enterprise/legacy systems.
  - REST is lightweight, flexible, uses JSON, and is the modern standard.
- Noted that Salesforce supports both, and understanding when each is used matters for integration design decisions.

### 11. Salesforce APIs

- Studied the key **Salesforce API types** and their use cases:
  - **REST API** — standard CRUD operations on Salesforce records via HTTP/JSON
  - **SOAP API** — XML-based, used in enterprise integrations and legacy systems
  - **Bulk API** — designed for high-volume data operations (insert, update, delete, query on large datasets)
  - **Streaming API** — push-based notifications for real-time data changes (PushTopic, Platform Events)
  - **Metadata API** — for deploying and retrieving org configuration/metadata
  - **Composite API** — batching multiple REST API requests into a single HTTP call for efficiency
  - **Connect API (Chatter API)** — for accessing Salesforce community/social features

---

# End of Day Report

**Date:** July 14, 2026
**Prepared by:** Aniket
**Role:** Salesforce Developer, iMark Infotech Pvt. Ltd.

---

## Summary

Continued progressing through the Salesforce Integration learning track, covering two key modules today: **Middleware platforms** used in enterprise integration architectures, and **Asynchronous Integration** patterns native to Salesforce. These two areas together form the backbone of how Salesforce integrates with external systems at scale.

---

Studied the role of **middleware** in enterprise integration — middleware sits between Salesforce and external systems, handling data transformation, routing, orchestration, and protocol translation so that individual systems don't need to be directly coupled to each other.

Covered the following platforms:

- **MuleSoft** — Salesforce's own integration platform (acquired 2018). Uses an API-led connectivity model with reusable APIs organized into System, Process, and Experience layers. The industry standard for Salesforce-centric enterprise integrations.
- **Boomi (Dell Boomi)** — a cloud-native iPaaS (Integration Platform as a Service) known for its low-code drag-and-drop interface and wide connector library. Often compared directly to MuleSoft for mid-market integrations.
- **Informatica** — strong in data integration and ETL (Extract, Transform, Load) scenarios, particularly for large-scale data migration and master data management (MDM) alongside Salesforce.
- **Jitterbit** — a mid-market iPaaS focused on speed of deployment, with pre-built Salesforce connectors and a visual design studio. Popular for smaller teams needing quick integration setup.
- **Azure Logic Apps** — Microsoft's serverless integration service, useful when the wider tech stack is Azure-based. Offers event-driven workflows and native connectors to both Microsoft services and Salesforce.

Key takeaway: the choice of middleware depends on factors like org size, existing tech stack, volume of data, and whether the requirement is real-time API orchestration (MuleSoft) vs. batch data sync (Informatica) vs. lightweight event-driven workflows (Azure Logic Apps).

---

Studied Salesforce-native **asynchronous patterns** — used when operations are too large, long-running, or external-dependency-heavy to run synchronously within Salesforce governor limits.

#### Apex Asynchronous Patterns

- **Future Methods (`@future`)** — annotated Apex methods that run in a separate thread after the current transaction completes. Used for callouts from triggers and simple async operations. Limitations: no chaining, no monitoring, limited parameter types.
- **Queueable Apex** — an improved version of Future methods. Supports chaining (a Queueable can enqueue another), accepts complex object types as parameters, and provides a Job ID for monitoring via `AsyncApexJob`. Preferred over `@future` for most async use cases.
- **Batch Apex** — designed for processing large volumes of records (up to 50 million) by breaking them into configurable chunks (`Database.Batchable` interface with `start`, `execute`, `finish` methods). Can implement `Database.Stateful` to maintain state across batches and `Database.AllowsCallouts` for external HTTP calls per batch.

#### Event-Driven Asynchronous Patterns

- **Platform Events** — Salesforce's publish/subscribe (pub/sub) messaging framework. A publisher fires an event (a Platform Event record), and any number of subscribers (Flows, Apex triggers, external systems) react to it independently and asynchronously. Decouples the producer from the consumer entirely.
- **Change Data Capture (CDC)** — automatically publishes change events whenever a Salesforce record is created, updated, deleted, or undeleted. External systems can subscribe to these events via the Event Bus to stay in sync with Salesforce data in near real-time — without polling the API.
- **Event Bus** — the underlying infrastructure that powers both Platform Events and CDC. Acts as the message broker, holding published events for up to 72 hours (retention window) so subscribers can replay missed events if needed. Supports both Salesforce-internal subscribers and external systems via CometD (Streaming API).

# End of Day Report

**Date:** July 15, 2026
**Prepared by:** Aniket
**Role:** Salesforce Developer, iMark Infotech Pvt. Ltd.

---

## Summary

Began learning **Salesforce Experience Cloud** from the ground up — starting with core concepts, site architecture, and then going deep on the sharing and security model, which is the most critical and commonly misunderstood part of Experience Cloud implementations.

---

## Work Completed

### 1. What is Experience Cloud

- Studied how Experience Cloud allows exposing a scoped subset of a Salesforce org — data, records, Flows, Knowledge articles — to external users who are not internal Salesforce users.
- Understood the three primary audience types:
  - **Customers** — self-service portals, case tracking, order history
  - **Partners** — deal registration, lead distribution, PRM-style dashboards
  - **Employees** — intranets, HR self-service portals
- Understood that a "site" is essentially a Force.com/Lightning app with its own URL, branding, and page layouts configured via Experience Builder.

### 2. Site Templates & Architecture

- Covered the available site templates:
  - **Customer Service** — for customer-facing support portals
  - **Partner Central** — for partner relationship management
  - **Build Your Own** — flexible template supporting both LWR and Aura runtimes
- Studied the difference between **LWR (Lightning Web Runtime)** — the modern CMS-driven standard — and the older **Aura-based** sites.
- Covered the key builder tools: **Experience Builder** (drag-and-drop page builder), **CMS Workspaces** for content management, and **Audience Targeting** for personalized content delivery.

### 3. Experience Cloud Sharing Model – Deep Dive

Focused heavily on how external users access data — this works differently from internal users and is the area most likely to cause issues in real implementations.

#### Sharing Sets
- Used with base-level external licenses (Customer Community).
- Works via an **Access Mapping**: a declarative rule that says "grant access to records where a lookup field on the record matches a field on the external user's Contact/Account."
- Example: Contact "Priya" logs into the portal. A Sharing Set says "grant Read/Write on Case where `Case.ContactId = User.ContactId`". Every Case tied to Priya is automatically visible to her as new Cases are created — no manual sharing needed.

#### Share Groups + Role Hierarchy
- Used with **Customer Community Plus** or **Partner Community** licenses, which support an external role hierarchy (unlike base Customer Community, which is flat).
- External users get Portal Roles (e.g. "Acme Corp - Manager", "Acme Corp - User") that roll up like an internal org chart.
- **Share Groups** bundle multiple portal roles together, allowing records relevant to an entire account team to be shared at once — useful for deal registrations or shared partner assets.

#### Guest User Access
- Governs unauthenticated (anonymous) visitors — public-facing pages like a product catalog or an unathenticated case submission form.
- Access is controlled purely by **object/field permissions on the Guest User profile** — OWD, role hierarchy, and standard sharing rules largely don't apply.
- Salesforce now defaults to **"Restrict guest user access" enabled**, meaning nothing is exposed unless explicitly granted on the Guest User profile.
- Common real-world issue: a public form fails silently because Guest User profile lacks Create access on the target object — a permissions issue, not a code issue.

#### Summary – Sharing Mechanism Comparison

| Mechanism | Who | Based On |
|---|---|---|
| Sharing Set | Customer Community (flat) | Record-to-user field matching |
| Share Group + Role Hierarchy | Community Plus / Partner | Portal roles, org-chart-like |
| Guest User | Anonymous visitors | Object/field permissions only, no hierarchy |

### 4. Connection to Existing Knowledge

- **Service Cloud Case work** → Customer Community customers viewing and updating their own Cases via Sharing Sets.
- **CPQ knowledge** → Partner Community users configuring quotes for their own deals using the role hierarchy model.

### 5. Licensing Overview

- Covered external user license types: **Customer Community**, **Customer Community Plus**, **Partner Community**, and guest user (no license).
- Noted that license type directly determines which sharing mechanism is available and whether external role hierarchy is supported.
# End of Day Report

**Date:** July 16, 2026
**Prepared by:** Aniket
**Role:** Salesforce Developer, iMark Infotech Pvt. Ltd.

---

## Summary

Split the day between hands-on R&D investigating a real Salesforce trigger conflict issue, and broadening general tech awareness across cloud infrastructure and modern data platforms relevant to the Salesforce ecosystem.

---

## Work Completed

### 1. R&D – Multiple Triggers on Same Object

Investigated a common but problematic Salesforce pattern: **two triggers existing on the same object and firing simultaneously**, which leads to unpredictable execution order, duplicate logic runs, and hard-to-debug side effects.

**The Problem:**
Salesforce does not guarantee the execution order of multiple triggers on the same object. When two triggers both fire on the same event (e.g., both `before insert`), they can conflict — running the same logic twice, overwriting each other's field updates, or causing governor limit breaches when combined DML/SOQL consumption adds up.

**Root Cause:**
Typically happens when triggers are added incrementally over time by different developers without a unified trigger strategy in place.

**Solution — One Trigger Per Object Pattern:**
The Salesforce best practice is to maintain a single trigger per object per event, which delegates all logic to an Apex handler class. This gives full control over execution order, makes testing easier, and prevents conflicts entirely.

```apex
// Single trigger on Account
trigger AccountTrigger on Account (before insert, before update, after insert, after update) {
    AccountTriggerHandler handler = new AccountTriggerHandler();
    if (Trigger.isBefore && Trigger.isInsert) handler.onBeforeInsert(Trigger.new);
    if (Trigger.isBefore && Trigger.isUpdate) handler.onBeforeUpdate(Trigger.new, Trigger.oldMap);
    if (Trigger.isAfter && Trigger.isInsert) handler.onAfterInsert(Trigger.new);
    if (Trigger.isAfter && Trigger.isUpdate) handler.onAfterUpdate(Trigger.new, Trigger.oldMap);
}
```

**Additional Considerations Explored:**
- Using a **TriggerHandler framework** (e.g. Kevin O'Hara's pattern) for even cleaner separation of concerns.
- Using a **static boolean flag** to prevent recursive trigger execution — a related issue that often surfaces alongside multi-trigger conflicts.
- Merging existing duplicate triggers into one as a refactoring task, carefully testing each handler method after consolidation.

---

### 2. General Tech Awareness – Cloud & Modern Data Platforms

Covered foundational awareness of technologies that frequently appear alongside Salesforce in enterprise architectures.

#### AWS (Amazon Web Services)
- Understood AWS as the leading cloud infrastructure platform — providing compute (EC2), storage (S3), serverless functions (Lambda), and managed databases (RDS, DynamoDB) as services.
- Relevance to Salesforce: AWS is commonly used to host middleware, integration layers, and data pipelines that connect to Salesforce via REST/SOAP APIs or event-driven patterns (Platform Events → AWS EventBridge, etc.).

#### Docker
- Understood Docker as a containerization platform — packaging an application and all its dependencies into a portable, self-contained "container" that runs consistently across any environment.
- Relevance to Salesforce: Used in CI/CD pipelines for Salesforce DevOps (e.g., containerized SFDX/SF CLI environments for automated deployments), and for hosting microservices that integrate with Salesforce.

#### Snowflake
- Understood Snowflake as a cloud-native data warehouse platform — designed for analytical workloads, storing and querying large volumes of structured data across multiple cloud providers.
- Relevance to Salesforce: A common pattern is syncing Salesforce data into Snowflake (via MuleSoft, Informatica, or Salesforce's own Data Cloud connectors) for reporting and BI that goes beyond Salesforce's native analytics capabilities.

#### Headless Salesforce
- Understood "Headless Salesforce" as an architectural pattern where Salesforce acts purely as a **backend data and logic layer** (APIs, automation, commerce engine) while the front-end UI is built on a completely separate stack (React, Next.js, mobile apps, etc.).
- Common in **Salesforce B2C Commerce (Headless Commerce)** and **Experience Cloud LWR sites**, where the presentation layer is decoupled from Salesforce's standard UI components.
- Key benefit: full front-end flexibility without being constrained to Lightning components or Experience Builder templates.