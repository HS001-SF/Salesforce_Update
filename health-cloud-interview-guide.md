# Salesforce Health Cloud – Interview Guide (5 YOE Developer Level)

## 1. Foundational Concept: What is Health Cloud, Really?

Health Cloud is **Sales Cloud + Service Cloud + a healthcare-specific data model + industry features**, built on the same Salesforce platform (Apex, LWC, Flow, Lightning, standard sharing/security engine). Interviewers will expect you to know it's not a separate platform — it's an overlay of:

- **Standard Salesforce Core** (Accounts, Contacts, Cases, Person Accounts)
- **Healthcare Data Model (HDM)** – FHIR-inspired custom & standard objects
- **Industry-specific features**: Care Management, Utilization Management, Provider Network Management, Payer/Provider capabilities, Home Health, Intelligent Document Automation

A common trap question: *"Is Health Cloud a separate org/product?"* — No, it's a managed package + licensing layer on core Salesforce, so all your normal Apex/Trigger/LWC/Flow/Integration knowledge applies directly.

---

## 2. Core Data Model — Must Know Cold

### Person Accounts
- Health Cloud runs on **Person Accounts** (Account + Contact merged record) — enabled at org creation, can't be turned off later.
- Know the implications: sharing rules apply at Account level, page layouts differ, reports/list views need "Person Account" record type awareness.

### Key Objects (be ready to explain relationships)
| Object | Purpose |
|---|---|
| `CarePlan` | Central plan of care for a patient |
| `CarePlanTemplate` | Reusable template (goals, problems, tasks) |
| `CareplanProblem` (Problem) | Clinical problem/diagnosis linked to plan |
| `CareplanGoal` (Goal) | Goal tied to a problem |
| `CarePlanActivity`/Intervention | Task/intervention for a goal |
| `CareBarrier` | Obstacle preventing goal achievement |
| `CareTeam` / `CareTeamMember` | Team assigned to a patient, with roles |
| `CareRequest` / `CareRequestExtension` | Utilization Management request wrapper |
| `AuthorizationForm` / `CoverageBenefit` | Payer-side UM objects |
| `EHRPatient`/Clinical objects: `ClinicalEncounter`, `Condition`, `Medication`, `AllergyIntolerance`, `Immunization`, `Procedure` | FHIR-aligned clinical data, mostly read via integration |
| `HealthCareProvider` / `HealthCareFacility` | Provider Network Management |
| `ContactContactRelation` / `AccountContactRelation` | Actionable Relationship Center (ARC) — patient's relationships (caregiver, PCP, family) |

Interviewers love asking: **"How do CarePlan, Problem, Goal, Barrier, and Intervention relate?"**
Answer: CarePlan → has Problems → each Problem has Goals → each Goal has Interventions (tasks) and can have Barriers blocking it. This hierarchy is a classic diagram to sketch on a whiteboard.

### Newer Care Program Model (post ~2022 releases)
- `CareProgram`, `CareProgramEnrollee`, `CareProgramTeam`, `CareProgramProduct`
- Represents structured programs (e.g., diabetes management program) a patient enrolls in — distinct from ad-hoc Care Plans.
- Be ready to explain **CareProgramEnrollee vs Contact vs Account** — enrollee is the join between a program and a person account.

---

## 3. Utilization Management (UM)
Common in payer-side interviews:
- `CareRequest` → represents a prior-authorization/service request
- Decision workflow: Intake → Clinical Review → Decision (Approve/Deny/Pend) → Notification
- Uses **Omniscript / FlexCards** heavily (from the Vlocity/Industries acquisition) for UI-driven intake
- Know **DataRaptors, Integration Procedures, OmniScript** if the role touches Industries Cloud tooling — very commonly paired with Health Cloud in payer implementations

---

## 4. Provider Network Management / Provider Search
- `HealthcareProvider`, `HealthcareProviderSpecialty`, `HealthCareFacility`, Network objects
- Provider Search component: used by members/patients to find in-network doctors
- Questions to expect: how do you model a provider who works at multiple facilities and multiple networks? (Many-to-many junction objects)

---

## 5. Clinical / EHR Data & FHIR
- Health Cloud's clinical objects map closely to **FHIR resources** (Condition, MedicationStatement, AllergyIntolerance, Encounter, Immunization, Procedure, Observation).
- **Admin Console / Data model doesn't invent clinical data** — it's typically populated via integration from EHR systems (Epic, Cerner) using **HL7v2, FHIR APIs, or MuleSoft**.
- Salesforce's **FHIR Adapter / Health Cloud FHIR standard connector** and **MuleSoft Accelerator for Healthcare** are common integration touchpoints.
- Know the term **"Patient Timeline"** — a UI component that shows clinical, encounter, and care data chronologically. Built on top of these clinical objects.

Expect a question like: *"How would you bring in patient allergy data from an external EHR?"*
Answer shape: middleware (MuleSoft or custom) transforms HL7/FHIR payload → maps to `AllergyIntolerance` object via REST/Bulk API or Platform Events → triggers/Flow do post-processing (alerts, care plan updates).

---

## 6. Actionable Relationship Center (ARC)
- Visual, force-directed graph component showing relationships between a patient and other records (caregivers, providers, household members).
- Built on `ContactContactRelation`/`AccountContactRelation` with `RelationshipType` and reciprocal roles.
- Common interview ask: explain how a **reciprocal relationship** (e.g., "Spouse of" / "Spouse of") is modeled without duplicate records — via a single junction record with two role fields.

---

## 7. Social Determinants of Health (SDOH)
- Objects/features to capture non-clinical factors (housing, food security, transportation) affecting health outcomes.
- Often modeled via **Assessments** (see below) plus custom objects, feeding Care Plan barriers/goals.

---

## 8. Assessments / Questionnaires
- Health Cloud has a built-in **Assessment framework**: `AssessmentTask`, question/answer/questionnaire objects, and admin-configurable scoring.
- Used for screenings (depression screening, fall-risk, SDOH surveys).
- Know that this predates and partially overlaps with core Salesforce **Surveys** — interviewers may probe on when to use one vs the other.

---

## 9. Security & Compliance (Very Commonly Asked)
- **HIPAA** isn't a Salesforce feature — it's an operational/contractual responsibility, but Salesforce provides tools to help:
  - **Shield Platform Encryption** for PHI at rest
  - **Field Audit Trail** for tracking historical changes to sensitive fields
  - **Restriction Rules / Scoping Rules** to limit visibility beyond standard sharing (useful for care team-based visibility instead of role hierarchy)
  - **Einstein Trust Layer** if Einstein/AI features touch PHI
- Sharing model nuance: Health Cloud often uses **Care Team-based sharing** (via Apex sharing or `CareTeamMember` driven Apex-managed sharing) rather than pure role hierarchy, since a patient's data should only be visible to their actual care team.
- Be ready to discuss **why row-level security via role hierarchy is often insufficient in healthcare** (a doctor should see only their patients, not all patients under their manager).

---

## 10. Licensing (Interviewers Test This to See Real Project Exposure)
- Health Cloud requires specific **Permission Set Licenses (PSLs)**: e.g., Health Cloud Platform, Care Management, Utilization Management add-ons are often separately licensed.
- Know that features can be **"grayed out"/inaccessible** even with correct profile/perm sets if the underlying PSL isn't assigned — a classic real-world debugging scenario to mention.

---

## 11. Integration Patterns You Should Be Able to Discuss
- **MuleSoft Accelerator for Healthcare** (most common enterprise pattern)
- **HL7v2 ↔ FHIR transformation** middleware
- **Platform Events** for near-real-time clinical updates
- **Bulk API 2.0** for large historical data loads (patient records at scale)
- **External Objects / Salesforce Connect** if EHR data is federated rather than replicated
- Governor limit awareness for high-volume patient data (bulkification, avoiding SOQL/DML in loops — standard but always tested)

---

## 12. Apex/LWC Considerations Specific to Health Cloud
- Many HC objects are part of **managed packages** — you often can't add triggers directly on Salesforce-managed objects; instead use **Flow, Process Builder (legacy), or Apex trigger via allowed extension points / Trigger Framework on standard objects** where supported.
- FlexCards & OmniScripts (if Industries/Vlocity tooling is present) instead of pure LWC in payer implementations — know the difference between **pure Health Cloud** (LWC/Aura based) vs **Health Cloud + Industries/OmniStudio** (FlexCard/OmniScript based) implementations, since this changes your dev approach entirely.
- Patient Timeline and Provider Search are **pre-built LWC components** — you configure/extend them rather than building from scratch.

---

## 13. Recent/Advanced Features (Good to Mention, Shows You're Current)
- **Home Health** management (scheduling caregivers/visits)
- **Intelligent Document Automation** (IDA) — OCR/AI extraction from healthcare documents (referrals, intake forms) into structured objects
- **Einstein for Health Cloud** — generative AI summarization of care plans/patient history (verify latest capabilities against current docs, this evolves fast)
- **Provider Timeline / Payer enhancements** for claims and coverage visibility

---

## 14. Likely System-Design / Scenario Questions at 5 YOE

1. *"Design a data model for a patient with multiple chronic conditions managed by different care teams."* → CarePlan per condition or one CarePlan with multiple Problems; discuss sharing implications.
2. *"How do you ensure a nurse only sees patients assigned to her care team, not the whole hospital?"* → CareTeamMember-driven Apex sharing / Restriction Rules.
3. *"How would you sync patient demographic updates from an EHR without creating duplicates?"* → Upsert via External ID, Matching Rules/Duplicate Rules, MDM considerations.
4. *"How do you handle a spike of 500K clinical records nightly from an EHR feed?"* → Bulk API 2.0, batch Apex, Platform Events for downstream real-time needs, staggered processing to avoid governor limits.
5. *"Difference between Care Plan and Care Program?"* → Care Plan = individualized clinical plan; Care Program = structured enrollment-based program (e.g., a disease management program) with its own team/product model.
6. *"How does Health Cloud handle FHIR compliance?"* → Object model alignment + FHIR adapter/APIs for interoperability, not a replacement for a full FHIR server.

---

## 15. Quick Study Checklist Before the Interview
- [ ] Draw the CarePlan → Problem → Goal → Intervention/Barrier hierarchy from memory
- [ ] Explain CareTeam & CareTeamMember sharing model
- [ ] Explain Person Account implications on data model & reporting
- [ ] Differentiate Care Plan vs Care Program vs Care Program Enrollee
- [ ] Explain at least one integration pattern end-to-end (EHR → Salesforce)
- [ ] Know PSL/licensing gotchas (feature invisible ≠ bug, could be missing license)
- [ ] Be able to name FHIR resources mapped in Health Cloud (Condition, Medication, AllergyIntolerance, Encounter)
- [ ] Understand where Industries/OmniStudio (FlexCards/OmniScripts) fits vs plain LWC
- [ ] Have one real (or realistic hypothetical) project story ready: what you built, what object model you designed, what security model you used, what integration you handled

---

*Note: Health Cloud releases new features 3x/year (Spring/Summer/Winter). For anything version-specific (latest UM enhancements, newest AI features), it's worth a quick check of current Salesforce Health Cloud release notes before the interview, since capabilities here move fast.*
