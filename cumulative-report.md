# What I've Been Up To — A Catch-Up Dispatch

*A cumulative update covering the last few days of work across Salesforce, client projects, and learning.*

---

## Digging Into a Gap in Salesforce Sales Cloud

One of the things I've been spending real time on lately is understanding where Salesforce Sales Cloud *doesn't* do what you'd expect it to — natively, out of the box.

The one that caught my attention: **you can't convert multiple leads at once in Salesforce**. That's it. You pick a lead, click Convert, fill in the details, and move on. One at a time. For teams running high-volume outbound or cleaning up a messy funnel after a campaign, this is genuinely painful. There is an app on the AppExchange that handles this, but it's not built-in, and honestly — I think there's room to do something better or at least complementary to it.

So that's what I've started working on. The current phase is pure architecture — how would this actually work? What does the data model look like? How do you handle the edge cases around existing Accounts and Contacts when converting in bulk? What safeguards do you need so you're not accidentally creating duplicates or orphaned records? These are the questions I'm thinking through right now, and it's been a good deep-dive into how Salesforce's lead conversion process works under the hood.

The build hasn't started yet — I'm still in design mode — but the architecture is taking shape.

---

## Client Work: The Assemble.ai × Salesforce Proposal

On the client side, I put together a detailed technical proposal for **Assemble.ai**, an AI coding platform, evaluating how well it handles real Salesforce development work.

The core ask from their team was clear: they're not interested in "build a demo" use cases. They want to see AI make *actual changes* to a live Salesforce org — triggers firing, LWC components deployed on record pages, records being created and updated, integrations actually syncing data. That framing shaped the whole proposal.

I structured the evaluation around **13 real-world Salesforce use cases**, organized across three tiers:

**Tier 1** covers core single-object work — things like a roll-up that native Salesforce can't do (because Opportunity→Account is a lookup, not a master-detail), an "Account at a glance" record page card for sales reps, and a validation guardrail on Case saves. These are the baseline — any serious AI platform should handle them.

**Tier 2** is where it gets more interesting: multi-object scenarios, async patterns, and light integrations. A lead deduplication panel with real merging logic, a nightly Batch Apex job for large-scale data hygiene, a customer onboarding wizard that creates four linked records in a single transaction with proper rollback, an inbound REST endpoint for external systems, and a Screen Flow backed by invocable Apex for refund eligibility.

**Tier 3** is the enterprise-grade stuff — the things that separate production-ready code from code that works in a sandbox. Async chains (Trigger → Queueable) for auto-generating Orders and Assets when Opportunities close, a CPQ-style quote line editor with a live pricing engine, the Salesforce ⇄ HubSpot bi-directional contact sync (which is specifically what Assemble.ai asked about), Platform Events for async decoupling, and a large-data-volume batch job with chaining and a Finalizer for resilience.

Each use case gets graded on hard gates (does it deploy cleanly? do tests pass? no governor limit exceptions?) and scored dimensions like bulkification, security, test quality, LWC hygiene, and architecture. We're also bringing an in-house debug log analyzer to the table — so when we say something uses 60% of the SOQL ceiling at 200 records, that's a reading from the log, not a gut feeling. The goal is measurement, not anecdote.

It was a solid piece of work to put together, and I think the tiered structure gives Assemble.ai genuinely useful signal about where their platform performs well and where it needs hardening.

---

## Getting into Agentforce

Alongside everything else, I've started digging into **Agentforce** — Salesforce's AI agent platform — and honestly, it's one of the more exciting things I've touched in a while. The short version: Agentforce lets you build autonomous AI agents that live inside your Salesforce org and can actually *do things*, not just answer questions.

The way I'd describe it to someone unfamiliar: think of it as giving your Salesforce org a brain that can reason, make decisions, and take action — without a human clicking through screens to make it happen. An agent can be triggered by an event, figure out what needs to happen next, call tools or flows to do it, and handle the whole thing end to end.

**What makes it different from a chatbot or a flow**

This was my first real question when I started — how is this different from Einstein Bots or just a complex Flow? The answer comes down to *reasoning*. A Flow follows a path you design. A bot follows a script. An Agentforce agent takes a goal, breaks it down into steps, decides which tools to use, and adapts if something doesn't work the first time. It's closer to how a person operates than how automation traditionally works.

**The building blocks**

There are a few core concepts I've been working through:

- **Agents** — the top-level entity. You define what the agent is for, what it's allowed to do, and what its personality/tone should be when it's interacting with users.
- **Topics** — scoped areas of responsibility. An agent can have multiple topics (e.g., "Handle billing questions", "Escalate support cases"), and it routes to the right one based on the conversation.
- **Actions** — the things an agent can actually do. These can be Flows, Apex, prompt templates, or connections to external systems. This is where Agentforce connects to real Salesforce functionality.
- **Data grounding** — agents can pull context from records, knowledge bases, and other sources in your org so their responses and decisions are based on actual data, not just what the LLM knows in general.

**Where I think this gets genuinely interesting**

The piece I keep coming back to is how Agentforce sits on top of everything else in Salesforce. The Apex I know, the Flows I know, the data model I know — all of that becomes *callable* by an agent. It's not a separate system you build from scratch. It's a layer of intelligence on top of infrastructure that already exists.

That opens up use cases that feel pretty different from traditional Salesforce automation. An agent that reviews new leads coming in and decides which ones need immediate follow-up. An agent that handles routine customer queries by pulling from Knowledge and case history, and only escalates when it genuinely can't resolve something. An agent running through a process checklist when an Opportunity closes and making sure every downstream step actually happens.

I'm still in learning mode — going through the documentation, understanding how agent testing works, figuring out what the failure modes look like — but the foundation is there and I can already see where this connects to the other work I'm doing. The multiple lead converter, for instance, could eventually have an agent layer on top of it to handle routing decisions intelligently rather than just mechanically converting everything in a batch.

More to come here as this develops.

---

## What's Next

Two things are running in parallel from here:

- **Multiple Lead Converter** — moving from architecture to early build. The design work is close to done; the next step is starting to actually write code.
- **Agentforce** — continuing to learn and experiment, and eventually figuring out how it connects to the other Salesforce work I'm doing.

More updates soon.
