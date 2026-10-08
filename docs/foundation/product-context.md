# Product context

The owner's kickoff brief (2026-10-08), restated so later sessions do not need chat history. Items here are **stated inputs**, not accepted first-release scope. Scope is decided in S3. Terms here are informal until `GLOSSARY.md` fixes them in S2.

## Vision

A multi-tenant SaaS platform for independent private kindergarten businesses, from a single location to networks of roughly 40 locations.

Planned surfaces:

- A shared **web CRM** for owners, administrators, and staff.
- A shared **mobile app** for parents and staff, with role-appropriate functionality.
- Possibly **branded mobile apps** later.

## Structural concepts (relationships to be fixed in S2)

- Organization, location, brand, legal payment recipient, and bank account are **different concepts**.
- Some organizations may **share a brand** while operating independently.
- Within one organization, different locations may receive payments through **different legal entities or sole proprietors** and **different accounts**.
- An authorized network administrator needs a **consolidated financial overview**, broken down by location, recipient, and account.

## Users

- Platform administrators
- Organization owners and administrators
- Location administrators
- Educators and other staff
- Parents or legal guardians

Children have records **without login accounts**. A parent can have **multiple children**. A child has **at least two** authorized parents or guardians. Legal guardians, emergency contacts, and adults authorized to pick up a child must be distinguished.

## Capabilities under consideration (not all in the first release)

### Kindergarten operations

- Locations, groups, educators, children, guardian relationships.
- Full-day and half-day attendance plans.
- Planned attendance, actual attendance, absences.
- Group changes, staff replacements, historical records.

### Meals

- Meals included in tuition or charged separately.
- Possibly selecting individual meals (lunch without breakfast).
- Meal orders, changes, cancellation deadlines, billing consequences.

### Billing and recalculation

- Tuition, meal charges, additional services, payments, debts, refunds.
- Vacation or illness allowances with configurable eligibility and limits.
- Recalculation according to each kindergarten's policy.
- **Actual attendance is not automatically billable attendance.**
- Historical pricing preserved. Every bill explains how it was calculated.
- Online payments, manually recorded cash payments, bank transfers.
- Distinguish manually recorded payments, provider-confirmed payments, and confirmed bank settlements.
- Different payment recipients and accounts per location. Service-specific recipients (for example, meals paid to a different recipient) are to be investigated.

### Communication (stated as a core requirement)

- A group chat is created automatically when a children's group is created.
- Relevant educators and authorized parents get access.
- Text, photos, videos.
- Private educator–parent conversations. Administrator–parent conversations to be considered.
- To clarify: does a private conversation include one parent or both guardians; how membership changes; who sees earlier messages; what happens when a child or educator leaves a group.
- To account for: reliable delivery, upload failures, unread state, notifications, media access, retention, storage costs. First-release chat features to be decided.

## Monetization hypothesis (to validate)

- No fixed subscription. A **1% platform fee on recorded payments**.
- Online payments may allow automatic fee collection, depending on the provider.
- Cash and manually recorded payments roll up into a periodic invoice to the business.
- Still open: fee rates, caps, refunds, corrections, historical imports, and who pays the fee.
- Payment-provider fees are **not** platform revenue.

## Security requirements (stated)

- Tenant isolation and RLS from the first release.
- Access depends on roles, scope, and relationships to specific records.
- Children's data, chat media, and financial data need careful handling.
- Coverage: database queries, APIs, storage, realtime subscriptions, background jobs, caches, privileged operations.
- Verify that the chosen database access path actually enforces RLS.
- Legal requirements and payment-provider capabilities must be **verified, not guessed**.

## Owner background and constraints

- Experienced frontend developer: React, TypeScript. Familiar with Next.js, React Native/Expo, Supabase, Clerk, Tailwind, shadcn/ui. These are **candidates**, not decisions.
- Builds and operates alone at first, with limited personal time.
- Favour a maintainable foundation, low operational overhead, incremental delivery. Support growth without speculative abstractions.
- No delivery dates until scope and capacity are understood.

## Foundation goals

1. Consistent domain glossary
2. Clear product boundaries and a validated first-release scope
3. Conceptual domain model and lifecycle rules
4. Role, permission, and access-scope model
5. Privacy and security foundation
6. Application and backend boundaries
7. Justified stack per application and shared service
8. Database and RLS design derived from business rules
9. Ordered implementation milestones with dependencies and acceptance criteria
10. Durable documentation that lets a new agent continue without chat history

**Completion standard:** the first implementation milestone can be handed to another agent with clear scope, dependencies, business rules, access rules, and acceptance criteria.

---

## Agent observations (2026-10-08, not owner-confirmed)

### Tensions in the brief

| ID | Tension | Why it matters | Resolve in |
| --- | --- | --- | --- |
| T1 | "1% on recorded payments" while cash payments are recorded manually by the business | The business pays more the more honestly it records. That rewards under-recording, and the CRM's financial data then becomes unreliable for the business itself. | S1 (framing), S6 |
| T2 | Automatic fee collection on online payments, and different recipients per location | This needs split or marketplace payouts per recipient. It may make the platform a payment intermediary, which can require a licence or contracts depending on jurisdiction. | R2, R1, S6 |
| T3 | Chat is a "core requirement" (reliable delivery, media, push), but the owner is solo with limited time | Chat is the most operationally expensive capability, and its media storage costs scale with use, not with payments. | S3, S7, S10 (build vs buy) |
| T4 | Tenant isolation, yet a guardian may have children in two organizations and staff may work for several | A person's identity cannot belong to a single tenant. Login and the mobile app sit above the tenant boundary. | S2, S4 |
| T5 | Brands shared across independent organizations, plus future branded apps | One branded app could serve several tenants, so a brand cannot simply be an attribute of an organization. | S2 |
| T6 | A "network administrator" needs a consolidated view, and organizations operate independently | If a network can span several organizations, consolidated finance crosses the tenant boundary. | S2 |
| T7 | Service-specific recipients (meals to a different recipient) | One family's monthly charges may be payable to two recipients, which affects one-bill/one-payment UX and provider split support. | S6, R2 |

### Missing information

| Missing | Blocks |
| --- | --- |
| Jurisdiction(s), currency, UI languages | Legal research (R1), payments research (R2), identity (phone vs email login) |
| Pilot customer(s) and what they use today | Scope validation (S3) |
| Owner capacity (hours/week) and monthly infra budget | Scope size (S3), build vs buy (S10) |
| Scale per location (children, staff, groups, media volume) | Chat and storage costs, plan limits |
| Fiscal receipt obligations for cash and online payments | Billing and payments (S6) |
| Whether staff are employed by an organization or by a payment recipient | Identity, staff model (S4) |
| Staff devices (personal phones or a shared tablet per location; offline needs) | Attendance UX (S5), stack (S10) |
| Data migration and historical import from current tools | Scope (S3), monetization (historical imports) |
| The platform's own legal entity and the contract chain with businesses | Platform fee invoicing, data-processor role (R1, S9) |
