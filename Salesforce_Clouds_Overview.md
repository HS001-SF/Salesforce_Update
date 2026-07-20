# A Tour Through the Clouds
### *What Salesforce actually is, why it's split into "clouds," and how each one fits into the bigger picture — including Data Cloud and Agentforce, the AI layer tying it all together.*

---

## So... what is Salesforce, really?

Most people hear "Salesforce" and think of a single piece of software. In reality, Salesforce is closer to an ecosystem — a shared platform with a family of specialized products, each one built to run a different part of a business. Sales teams, support teams, marketers, and store operators all end up looking at the same underlying customer record, but through a tool tailored to their job.

Salesforce calls each of these tailored products a **"cloud."** Historically that meant Sales Cloud, Service Cloud, Marketing Cloud, and so on. In the last couple of years the branding has shifted: Salesforce now markets most of its AI-powered capabilities under the umbrella name **Agentforce**, and many of the old cloud names have been folded into that story (for example, Commerce Cloud is increasingly referred to as "Agentforce Commerce"). The underlying products and what they do, however, are largely the same — this report uses the classic cloud names since they're still the easiest way to understand the map.

**Why does this matter?** Because pricing, implementation, and hiring in the Salesforce world are all organized around clouds. A company doesn't "buy Salesforce" — it buys Sales Cloud seats, or a Service Cloud contract, or a Marketing Cloud tier. Knowing the map helps you understand what you're actually paying for.

---

## The Six Core Clouds

Almost every Salesforce deployment starts with some combination of these six. They cover the day-to-day functions most companies need regardless of industry.

### 1. Sales Cloud
The original product and still the flagship. It manages leads, opportunities, pipelines, and forecasting. Sales reps track every interaction with a prospect in one place, while managers get visibility into pipeline health. In its current form, Sales Cloud leans heavily on AI features (branded Einstein and Agentforce) that surface deal-risk alerts, suggest next steps, and can automatically draft follow-up emails.

### 2. Service Cloud
The customer-support counterpart to Sales Cloud. It powers help desks and contact centers — case management, omnichannel routing (chat, email, phone, social), a knowledge base, and self-service portals. AI agents built on Service Cloud can now resolve routine tickets on their own and hand off only the complex cases to a human.

### 3. Marketing Cloud
Handles email, SMS, push notifications, journey building, and campaign analytics. It lets marketers design multi-step customer journeys (e.g., "if a customer abandons a cart, wait 24 hours, then send a discount email") and personalize content at scale using customer data.

### 4. Commerce Cloud
An e-commerce platform for building and running online storefronts — product catalogs, checkout, promotions, and order management across web, mobile, and in some cases physical retail. It's designed to plug into the same customer data used by Sales, Service, and Marketing so a shopper's history follows them across channels.

### 5. Experience Cloud
Used to build branded portals, communities, and websites for customers, partners, or employees — think self-service help centers, partner-deal-registration portals, or dealer networks — all backed by live Salesforce data rather than a static website.

### 6. Analytics (Tableau)
Salesforce's business-intelligence layer, built around Tableau (acquired in 2019) alongside native reporting and dashboard tools. It turns the raw data sitting across the other clouds into dashboards, forecasts, and ad-hoc exploration for decision-makers.

---

## The Layer That Ties It Together — and the Specialists

### Data Cloud (a.k.a. Data 360)
For years, each cloud kept its own version of "the customer," which meant a support rep and a marketer could be looking at slightly different pictures of the same person. Data Cloud (recently rebranded in places as Data 360) was built to fix that: it pulls in data from every source — CRM records, website behavior, mobile app activity, support tickets, even outside systems — and stitches it into a single, continuously updated customer profile.

That unification matters more than ever because it's the fuel for Salesforce's AI. Without clean, connected data, an AI agent can't reason well about a customer, so Data Cloud has effectively become the foundation the rest of the platform sits on.

### Agentforce — the AI layer
Agentforce is Salesforce's brand for autonomous AI agents that work across every cloud: an agent might independently answer a support ticket in Service Cloud, qualify a lead in Sales Cloud, or recommend products in Commerce Cloud. Unlike the earlier generation of Einstein features, which mostly made suggestions for a human to accept, Agentforce agents are designed to complete multi-step tasks on their own within guardrails a company sets. It launched in 2024 and expanded quickly through 2025 into what Salesforce now calls **Agentforce 360**, spanning nearly the whole product line.

### Industry & specialty clouds
Beyond the six core clouds, Salesforce sells a long tail of industry-specific products that add pre-built data models and workflows for regulated or specialized sectors:

- **Financial Services Cloud** — built for banks, wealth managers, and insurers; adds household and account relationship modeling.
- **Health Cloud** — patient relationship management, care coordination, and appointment workflows for healthcare providers and payers.
- **Manufacturing Cloud** — aligns sales agreements with demand forecasts and gives manufacturers a unified view of account performance.
- **Nonprofit Cloud** — donor management, fundraising, and program/case management for nonprofits.
- **Revenue Cloud** — combines CPQ (configure-price-quote), billing, and subscription management into one revenue lifecycle tool.
- **Public Sector, Education, Consumer Goods, Automotive, Media, and Energy & Utilities clouds** — each tailored with industry-specific objects and processes.

Most companies never touch these; they exist for organizations whose workflows don't fit neatly into the generic six.

---

## Which Clouds Actually Matter to You?

The temptation with Salesforce is to assume more clouds equal more capability. In practice, the opposite is often true — every additional cloud is another license cost, another system to configure, and another team that needs training. A leaner deployment that's actually used beats a comprehensive one that isn't.

### A simple way to decide

| If your priority is... | Start with... |
|---|---|
| Growing pipeline & closing deals faster | Sales Cloud |
| Reducing support ticket resolution time | Service Cloud |
| Running multi-channel campaigns | Marketing Cloud |
| Selling products directly online | Commerce Cloud |
| Building a self-service portal or community | Experience Cloud |
| Unifying customer data before adding AI | Data Cloud |
| Operating in a regulated industry | The matching industry cloud |

### The takeaway

Salesforce's "clouds" are best understood as a modular toolkit rather than one monolithic product. Six core clouds — Sales, Service, Marketing, Commerce, Experience, and Analytics — cover the functions most businesses need. Data Cloud now sits underneath as the shared source of truth, and Agentforce is the AI layer that acts on that data across every cloud. Layered on top are industry-specific clouds for businesses with specialized workflows.

The right approach for most organizations is to start with the one or two clouds tied to the most pressing business problem, get real adoption there, and expand deliberately — rather than trying to light up the whole platform at once.

---

*This overview reflects Salesforce's product lineup and branding as of mid-2026; Salesforce updates naming and packaging frequently, so it's worth checking current documentation before making purchasing decisions.*
