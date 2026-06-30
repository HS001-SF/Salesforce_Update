# Weekly Work Report

**Period:** June 10 – June 29, 2026
**Prepared by:** Aniket
**Role:** Salesforce Developer, iMark Infotech Pvt. Ltd.

---

## 1. assemble.ai Evaluation – Client: Aliyan

Participated in testing **assemble.ai**, an AI agent platform, within a Salesforce org for our client Aliyan. Testing focused on Service Cloud and Marketing Cloud (simulated) use cases to evaluate the platform's ability to generate and execute production-grade automation and code.

- Ran structured test scenarios covering Service Cloud functionality and Marketing Cloud-style operations (Data Extension queries/upserts, subscriber updates, journey activation, etc.), using correct Marketing Cloud terminology in reporting.
- Documented results and prepared a formal **QA test report** for client submission, structured around the agreed success criteria.

## 2. SLA Countdown Component – Internal R&D

Identified and solved a common Salesforce Service Cloud problem on my own initiative.

**Problem:** Agents had no easy way to see how much SLA time remained on a Case, making prioritization difficult.

**Solution:** Built a live SLA countdown timer directly on the Case record.

**Benefit:** Agents can now see remaining SLA time at a glance, helping them prioritize urgent cases first.

### Implementation
- **Data model:** Added custom objects and custom fields to track SLA timing data on Cases.
- **Apex layer:** Built Apex classes and a trigger to automate SLA calculation and updates.
- **LWC component:** Developed a Lightning Web Component to display the countdown on the Case record page. It updates live and is working as expected in testing.

## 3. Learning – Salesforce Agentforce

Over the last two days, shifted focus to upskilling in **Salesforce Agentforce**, using a mix of resources:
- Claude (for concept clarification and guided learning)
- Trailhead modules and challenges
- YouTube tutorials

As a side benefit, completing Trailhead challenges also added to my **Trailhead points/badges**.

---

## Summary

| Area | Status |
|---|---|
| assemble.ai testing (Aliyan) | Completed, report submitted |
| SLA Countdown Component | Built and working |
| Agentforce learning | In progress |