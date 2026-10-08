# Roadmap

**Status:** `draft`, awaiting owner review.

There are two kinds of work:

- **Discovery stages (S0–S12)** produce *decisions*, not deliverables, in the spirit of `/wayfinder`. Each is worked in one or more sessions with `grilling` + `domain-modeling`.
- **Implementation milestones (M1…)** produce *working software*. They are defined only in S12 and executed via `/to-spec` → `/to-tickets` → `/implement`.

Supporting items:

- **Research (R*)**: AFK, run with `/research` by a background agent. Output goes to `docs/foundation/research/`, citing primary sources.
- **Spikes (SP*)**: throwaway code that answers one question, run with `/prototype` or a minimal technical test. Its verdict is recorded and the code kept on a `prototype/<name>` branch.

No dates. Sessions per stage are rough sizing only.

## Overview

| ID | Stage | Status | Depends on | Size |
| --- | --- | --- | --- | --- |
| S0 | Process and documentation setup | in_progress | none | 1 session |
| S1 | Business framing and constraints | not_started | S0 | 1 |
| S2 | Domain language and tenancy structure | not_started | S1 | 1–2 |
| S3 | First-release scope and product boundaries | not_started | S1, S2 | 1–2 |
| S4 | People, relationships, and identity | not_started | S2, S3 | 1–2 |
| S5 | Operations: groups, enrollment, attendance, meals | not_started | S4 | 1–2 |
| S6 | Billing, payments, and platform fee | not_started | S4, S5, R1, R2 | 2–3 |
| S7 | Communication | not_started | S4, S5, R4 | 1–2 |
| S8 | Roles, permissions, and access scope | not_started | S4–S7 | 1 |
| S9 | Privacy, security, and compliance foundation | not_started | S8, R1 | 1 |
| S10 | Application boundaries and technology stack | not_started | S3, S7, S8, S9, R3–R5, SP1 | 1–2 |
| S11 | Data model and RLS design | not_started | S4–S10 | 1–2 |
| S12 | Implementation milestones | not_started | S3, S11 | 1 |

```
S0 → S1 → S2 → S3 → S4 → S5 ─┬─→ S6 ─┐
                  │      │        └─→ S7 ─┼─→ S8 → S9 → S10 → S11 → S12
                  │      └────────────────┘
  R1 legal ───────────────────────→ S6, S9
  R2 payments ────────────────────→ S6
  R3 auth+RLS path, R5 mobile ────→ S10 (+ SP1)
  R4 chat build vs buy ───────────→ S7, S10
```

S6 and S7 are independent of each other, so they can run in either order. The research items start as soon as their trigger stage (listed below) is accepted.

## Why this order

1. **Framing before everything (S1).** The jurisdiction decides legal obligations, payment providers, fiscal receipts, and even whether login is by phone or email. A pilot customer is the only way to *validate* scope. Owner capacity caps how ambitious the first release can be.
2. **Language before scope (S2 → S3).** We cannot scope "payments per location" while "account" still means three things. S2 is deliberately **wide and shallow**: structural terms and the tenant boundary only.
3. **Scope before deep models (S3 → S4–S7).** Modeling every capability in depth would waste a solo founder's time. After S3, the deep dives cover first-release capabilities in depth. Each deferred capability gets one line: the constraint it puts on today's design, or "no constraint, deferred".
4. **Identity before operations, billing, and chat (S4).** All three hang off *who is related to which child, in which role, at which location*. Chat membership and bill visibility are derived from these relationships.
5. **Operations before billing and chat (S5 → S6, S7).** Billing consumes enrollment plans, absences, and meal orders. Chat membership follows group enrollment and staff assignment history.
6. **Access after the domain (S8).** A permission matrix needs the resources and relationships to exist. S4 already fixes the *primitives* (scope types, relationship kinds), so S5–S7 can reason about visibility without waiting.
7. **Security after access, stack after security (S9 → S10).** The stack has to satisfy the security requirements, such as "the database access path enforces RLS". Choosing it first would let the tool dictate the rules.
8. **Data and RLS after the stack (S11).** Policy mechanics depend on the database and auth choice. Business predicates (from S8) do not, so they are written first.
9. **Milestones last (S12).** Only now can the first milestone be specified with business rules, access rules, and acceptance criteria.

**Feedback loops are expected.** S4–S7 may show that S3 scope is too big or too small. When that happens, S3 is marked `needs_review`, never silently edited.

---

## S0: Process and documentation setup

- **Purpose:** agree on how we work so every later session is consistent.
- **Decisions:** doc structure (D-001); stage process (D-002); grilling question format (D-003); where tracking lives (D-004); handoff location (D-005).
- **Inputs:** kickoff brief, installed skills, repo conventions (`CLAUDE.md`, `docs/agents/`).
- **Main questions:** see D-001–D-005 in `decisions.md`.
- **Deliverables:** this folder (README, product-context, roadmap, decisions, open-questions, handoff).
- **Done when:** owner accepts the roadmap and D-001–D-005 (or amends them).
- **Downstream:** every stage.
- **Belongs later:** everything about the product.

## S1: Business framing and constraints

- **Purpose:** fix the facts and constraints that every later decision depends on.
- **Decisions:** launch jurisdiction(s), currency, UI languages; target segment at launch (single-location vs networks); pilot customer(s) and their availability; the top pains to solve and what customers use today; owner capacity and infra budget; success criteria for the first release; what the monetization hypothesis must prove first (T1), not final pricing.
- **Inputs:** product-context.
- **Main questions:**
  - Which country do you launch in, and do you expect another within 2 years?
  - Is there a specific kindergarten (or network) that will use the first release? How involved will they be?
  - What do they use now (spreadsheets, messenger groups, accounting software, another CRM)? What do they hate most?
  - Is the wedge **operations + billing** or **parent communication**? What makes a business switch?
  - How many hours per week can you give, and what monthly infra spend is acceptable before revenue?
  - What would make the first release a success: N paying locations, payments processed, owner hours saved?
  - Does 1% on recorded payments survive T1? (Answer the framing now; mechanics go to S6.)
- **Deliverables:** `stages/S01-business-framing.md`, decisions, updated open questions. Triggers R1 and R2.
- **Done when:** jurisdiction, pilot status, capacity, ranked pains, and success criteria are accepted.
- **Downstream to consider:** branded apps and multi-country can change identity and localization decisions. Record only the constraint.
- **Belongs later:** features (S3), technology (S10).

## S2: Domain language and tenancy structure

- **Purpose:** one vocabulary and an explicit structure of the business world.
- **Decisions:** canonical terms and relationships (with cardinalities and examples) for platform, organization/tenant, location, brand, payment recipient (legal entity or sole proprietor), bank/settlement account, network, group, child, person vs user account, staff member, guardian. Also: the tenant boundary; whether a network can span organizations (T6); who owns a brand (T5); whether a location can change organization or recipient over time.
- **Inputs:** S1.
- **Main questions:**
  - Is the tenant exactly one organization? What happens when a business sells one location to another business?
  - "Two organizations share a brand": franchise, family members, or a former network split? Who controls the brand?
  - Can one payment recipient (sole proprietor) receive money for locations of two different organizations?
  - Does "network administrator" mean the administrator of a multi-location organization, or a role across organizations?
  - Is a bank account owned by a recipient, and can a location change account mid-year? (History matters for reports.)
- **Deliverables:** `/GLOSSARY.md` created; `stages/S02-domain-structure.md` with a relationship diagram and scenarios; ADR for the tenant boundary.
- **Done when:** every term in product-context maps to a canonical term or an `_Avoid_` entry; tenant boundary and cardinalities accepted with worked scenarios.
- **Downstream to consider:** branded apps (T5), consolidated reports (T6), recipient routing (S6), RLS tenant key (S11).
- **Belongs later:** roles and permissions (S8), people relationships in depth (S4).

## S3: First-release scope and product boundaries

- **Purpose:** decide what the first release is, what it is not, and what it must not preclude.
- **Decisions:** first-release capability list; explicit non-goals; which surfaces ship (web CRM, mobile app, or PWA first); which roles use it; manual vs automated workflows; data import needs; a "must not preclude" constraint list for deferred capabilities.
- **Inputs:** S1 (pains, pilot, capacity), S2 (vocabulary).
- **Main questions:**
  - Can the first release ship with manual payment recording only, and online payments later?
  - Are meals in the first release, and if so, included-in-tuition only?
  - What level of chat: group announcements only, or full two-way chat with media?
  - Does staff need the mobile app in the first release, or is the web CRM enough for them?
  - What must be imported from the pilot's current tools on day one?
- **Deliverables:** `stages/S03-scope.md` with a capability map tagged first release / next / later / out.
- **Done when:** first-release list and non-goals accepted; every deferred capability has its constraint or "no constraint".
- **Downstream to consider:** all of S4–S12.
- **Belongs later:** how each capability works (S4–S7).

## S4: People, relationships, and identity

- **Purpose:** model people, their relationships to children and businesses, and how they log in.
- **Decisions:**
  - Person vs user account; child records without login.
  - Guardian vs emergency contact vs authorized pickup adult. Who can consent, receive bills, or see the child.
  - Payer (responsible party) for billing.
  - Guardians with children in several organizations, and staff working across locations or organizations (T4).
  - Login identifier (phone or email, per S1).
  - Invitation and onboarding, conceptually.
  - Lifecycle: enrollment and withdrawal at the person level; guardian added or removed; restricted guardian (court order); staff leaving.
  - The **primitives** of access: scope types and relationship kinds.
- **Inputs:** S2, S3.
- **Main questions (scenarios):** divorced parents where one is restricted; a grandmother who picks up but is not a guardian; a parent with children in two different businesses; an educator at two locations; a guardian who never installs the app; a child who withdraws with debt outstanding.
- **Deliverables:** glossary updates; `stages/S04-people-identity.md` with relationship model, lifecycle rules, scenarios; possibly an ADR for platform-level identity.
- **Done when:** all scenarios have accepted answers; access primitives accepted.
- **Downstream to consider:** chat membership and history access (S7); bill and payment visibility (S6, S8); RLS predicates (S11); branded apps sharing one identity.
- **Belongs later:** the full permission matrix (S8), the auth provider (S10).

## S5: Operations: groups, enrollment, attendance, meals

- **Purpose:** model the day-to-day kindergarten operation over time.
- **Decisions:** group lifecycle; enrollment of a child in a location and group over time (effective dating); attendance plan (full- or half-day) and where it lives; planned vs actual attendance; absence kinds, who reports them, and reporting deadlines; staff assignment and substitution; how history is kept; meals in first-release depth (plan vs order, cancellation deadlines), tied to absence reporting.
- **Inputs:** S4, S3.
- **Main questions:** a child moves group mid-month; switches from full to half day on the 15th; parent reports illness at 9:30 after the meal cut-off; a substitute educator for a week; a group is split or merged in September.
- **Deliverables:** `stages/S05-operations.md` with lifecycle rules and scenarios; glossary updates.
- **Done when:** lifecycle rules and scenario outcomes accepted; billing-relevant facts (what S6 consumes) listed explicitly.
- **Downstream to consider:** billing inputs (S6); chat membership on group change (S7); offline attendance marking (S10).
- **Belongs later:** prices and money (S6).

## S6: Billing, payments, and platform fee

- **Purpose:** model money: what is owed, why, to whom, what was paid, and what the platform earns.
- **Decisions:**
  - Billing unit (child or family) and billing period.
  - Price lists and historical pricing.
  - Charge lines with a calculation trace.
  - Allowance policies (vacation, illness): configurable vs a fixed set.
  - Recalculation timing (prepaid month with credit next month, or post-paid).
  - Invoices or statements to guardians.
  - Payment states: manually recorded, provider-confirmed, bank-settled.
  - Allocating payments to charges; debts, refunds, corrections.
  - Routing to recipient and account per location and service (T7).
  - Consolidated reporting dimensions.
  - Platform fee: base, who pays, refunds, manual-payment invoicing, historical imports (T1, T2).
- **Inputs:** S4, S5, R1 (fiscal and legal), R2 (providers).
- **Main questions (worked examples):** sick for 6 working days in March under a "more than 5 consecutive days" rule; price rise in January for new contracts only; half cash, then the rest online; a refund after withdrawal; meals payable to a different recipient than tuition.
- **Deliverables:** `stages/S06-billing.md`; glossary updates; ADR(s), for example "ledger-style charges, never edited"; SP3 logic prototype.
- **Done when:** worked examples produce accepted outcomes; payment-state model and fee model accepted; first-release billing subset accepted.
- **Downstream to consider:** financial RLS (S8, S11); provider choice (S10); accounting exports later.
- **Belongs later:** the provider integration specifics (S10, milestones).

## S7: Communication

- **Purpose:** define chat behaviour precisely enough to build or buy.
- **Decisions:** conversation types (group, private educator–guardian, administrator–guardian, staff-only?); whether membership is derived from relationships or managed by hand; whether a private conversation covers the child's guardians or one guardian; history visibility on join; what happens on leaving; editing, deleting, moderation; media types, size limits, processing; retention and storage cost; delivery, unread state, push, quiet hours; the first-release chat subset; build vs buy direction (R4).
- **Inputs:** S4, S5, S3, R4.
- **Main questions (scenarios):** a child moves group (do guardians lose access to old group history?); an educator leaves the business; a restricted guardian; the father wants a private thread without the mother; an administrator needs to read a private thread after a complaint.
- **Deliverables:** `stages/S07-communication.md`; glossary updates.
- **Done when:** membership and history rules accepted with scenarios; first-release feature list accepted; build/buy direction drafted for S10.
- **Downstream to consider:** media access control and storage RLS (S9, S11); push infra (S10); branded apps (notification sender identity).
- **Belongs later:** vendor or implementation choice (S10).

## S8: Roles, permissions, and access scope

- **Purpose:** one access model covering every first-release resource and action.
- **Decisions:** role catalogue; scope types (platform, organization, location, group, relationship to a child); permission matrix (role × resource × action × scope); delegation (who can grant what); platform-admin support access (break-glass, audited); financial visibility; custom roles (probably not in the first release).
- **Inputs:** S2, S4–S7.
- **Main questions:** can a location administrator see another location's debts? Can an educator see a guardian's phone number? Can a platform admin read chats?
- **Deliverables:** `stages/S08-access.md` with the matrix and each rule written as a predicate over data.
- **Done when:** matrix accepted for all first-release resources; every rule expressible as a data predicate (input to S11).
- **Belongs later:** policy implementation (S11).

## S9: Privacy, security, and compliance foundation

- **Purpose:** decide the controls before choosing tools.
- **Decisions:**
  - Data classification (children's health info, photos and videos, financial data, contacts).
  - Legal basis and consent; the platform as data processor vs controller (R1).
  - Retention and deletion; data subject rights.
  - Audit log requirements.
  - Threat model across DB queries, APIs, storage, realtime, background jobs, caches, privileged operations.
  - Environments and secrets, backups and restore, data residency, minimum incident basics.
- **Inputs:** S8, R1.
- **Deliverables:** `stages/S09-security.md`: a control list, each with an owning stage or milestone; legal items marked verified or unverified.
- **Done when:** control list accepted; every legal claim either cited (R1) or listed as an open question.
- **Belongs later:** tool-specific configuration (S10, S11).

## S10: Application boundaries and technology stack

- **Purpose:** justify each piece of the system.
- **Decisions:** applications (web CRM, mobile app); backend shape (BaaS with database functions vs a separate API); background jobs; database host; auth provider (for example Supabase Auth vs Clerk, if one integrates with RLS); chat build vs buy; media storage and delivery; payment integration; push; monorepo; hosting; observability; how branded apps would be produced later.
- **Inputs:** S3, S7, S8, S9, R3, R4, R5, SP1.
- **Deliverables:** ADRs per lock-in choice; an architecture diagram in `stages/S10-architecture.md`.
- **Done when:** each component has an accepted choice and rationale; SP1 has **validated** RLS enforcement on the chosen access path.
- **Belongs later:** schema (S11).

## S11: Data model and RLS design

- **Purpose:** a logical schema and RLS policies derived from business rules. Not migrations.
- **Decisions:** first-release tables, keys, effective dating; tenant key strategy; RLS policy per table from S8 predicates; privileged operation paths (jobs, webhooks, admin); policy testing approach.
- **Inputs:** S4–S10.
- **Deliverables:** `stages/S11-data-rls.md`: entity list, policy spec, policy test matrix.
- **Done when:** every first-release access rule maps to a policy and at least one allow and one deny test case.

## S12: Implementation milestones

- **Purpose:** turn the foundation into an ordered build plan.
- **Decisions:** milestones, order, dependencies, acceptance criteria. The first milestone is specified well enough for `/to-spec`.
- **Inputs:** S3, S11 (and everything before).
- **Deliverables:** `milestones.md`; the first milestone's spec published via `/to-spec` → `/to-tickets` to GitHub issues.
- **Done when:** the completion standard in product-context is met.

---

## Research items (AFK)

| ID | Question | Triggered by | Feeds |
| --- | --- | --- | --- |
| R1 | Legal obligations in the launch jurisdiction: children's personal data, photos, processor/controller roles, data residency, fiscal receipts for cash and online payments, whether collecting a platform fee from payments needs a licence | S1 | S6, S9 |
| R2 | Payment providers in the launch jurisdiction: per-recipient merchant accounts, split payouts, automatic fee deduction, webhooks, refunds, settlement reports, onboarding of sole proprietors | S1 | S6, S10 |
| R3 | Auth and RLS enforcement paths: Supabase Auth vs Clerk third-party auth, which client and server paths enforce RLS (REST, realtime, storage, server-side ORM, edge functions), service-role risks | S3 | S10, SP1 |
| R4 | Chat build vs buy: managed chat SDKs vs Postgres with realtime; costs at pilot and at 40-location scale; media storage and transfer costs | S3 | S7, S10 |
| R5 | Mobile delivery: Expo push notifications, building several branded apps from one codebase, store account requirements per brand | S3 | S10 |

## Spikes

| ID | Question | Stage |
| --- | --- | --- |
| SP1 | Does the chosen access path enforce RLS end-to-end (queries, realtime, storage, jobs)? A minimal two-tenant test with allow and deny cases | S10 |
| SP2 | If chat is built: is delivery, unread state, and upload-retry reliable enough on mobile? | S7/S10 (only if needed) |
| SP3 | Recalculation logic prototype (`/prototype` LOGIC) driven through the S6 worked examples | S6 |

## Implementation milestones

Defined in S12. Not planned before then.
