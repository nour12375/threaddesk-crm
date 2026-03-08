Here we go — now the actors (aka user roles / personas / system actors in the old-school UML / use-case sense, but adapted to Scrum-style user stories).
As a grizzled architect who's designed dozens of multi-tenant SaaS systems (including inbox-heavy CRMs back when "microservices" was still called "SOA done right"), I always start by nailing down the concrete actors before flooding the backlog with stories. This avoids the classic mistake of building features nobody actually uses.
ThreadDesk is a lightweight, inbox-first B2B CRM MVP targeting small-to-medium sales-driven teams (agencies, consultants, technical services, energy suppliers, etc.). So the actor set is deliberately narrow — no 15-role enterprise bloat like Salesforce. We focus on who really touches the system daily in a 5–25 person org.
Primary Human Actors (Real Users)

Sales Representative (or "Sales Rep", "Closer", "Account Executive")
The heavy daily user — lives in email, turns threads into deals/quotes/invoices.
Needs fast inbox → deal flow, AI summaries, one-click quote gen, Customer 360 view.
Pain: drowning in threads, forgetting follow-ups, manual data entry.
Most user stories revolve around this persona.

Sales Manager / Team Lead (or "Sales Director" in slightly bigger teams)
Oversees pipeline, assigns leads/deals, checks team activity.
Needs visibility into all open deals, thread summaries across team, basic reporting (MVP = timeline + list views).
Often the one who connects the mailbox and invites users.

Administrator / Company Owner / Founder (frequently the same person in SMB)
Configures the tenant: connects IMAP, imports product catalog (CSV/XLSX), invites users, sets basic roles/permissions.
Cares deeply about data isolation, easy Docker spin-up for demos, no vendor lock-in.
Often doubles as Sales Manager in very small teams.

Billing / Operations Person (sometimes the same admin or sales rep wearing multiple hats)
Focuses on quote → invoice transition, marks invoices paid (manual in MVP).
Less frequent user, but critical for revenue close.


Secondary / Supporting Actors

Team Member / Collaborator (lightweight viewer / commenter)
Can view Customer 360 timeline, add notes/tasks, but limited write access (no deal creation, no quote gen).
Useful for support folks, junior reps, or partners who need context without full control.


System / External Actors (non-human)

IMAP Email Server — source of truth for incoming threads (polling actor).
AI Engine (LLM wrapper) — produces thread summaries + action items in strict JSON.
NATS Event Bus — internal actor for pub/sub between microservices (deal created → quote possible, etc.).
MinIO / File Storage — actor that persists PDFs (quotes/invoices), imported CSVs, attachments.
PostgreSQL instances — per-service data persistence with tenant isolation.

Mapping Actors to Key User Stories (Quick Cross-Reference)

Sales Rep → almost everything: connect mailbox (shared), auto-create contacts, read AI summaries, create deals, build quotes, generate invoices, view Customer 360.
Sales Manager / Admin → invite users, assign roles, import product catalog, oversee timelines/pipeline.
Billing Person → convert quote → invoice, update payment status.
Admin / Owner → full tenant setup, Docker deploy, multi-tenant security guarantees.

Quick Actor Cheat-Sheet Table (old-school style)

ActorFrequencyMain Goals / ValueTypical Team Size FitPermissions LevelSales RepresentativeDailyTurn emails → revenue (deals, quotes, invoices) fastAllHigh (CRUD on own + shared)Sales ManagerDaily–WeeklyPipeline visibility, team coaching, avoid lost threads5+ peopleHigh + view-allAdministrator / OwnerSetup + occasionalTenant config, product import, user invites, demo-abilityAll (often 1 person)Full (god-mode)Billing / OpsWeeklyInvoice generation & trackingOptionalMediumTeam MemberOccasionalRead context, add notes/tasks10+ peopleRead + limited write
This set keeps the MVP lean: 3–4 real roles max in the first release (most tools start with Sales Rep + Admin, then add Manager).
Next logical step in a real project:

Define very coarse permissions matrix (who can see whose deals? Can reps see all threads or only assigned?)
Write 2–3 concrete acceptance criteria per major story from the previous list, scoped to these actors
Decide if we fake multi-tenancy first (single DB + tenant_id) or go full separate schemas from day 1

What do you want to attack now — permissions model, detailed AC for a specific actor, or slice the stories by role / MoSCoW? Or maybe name these personas with fake but realistic names like we do in workshops (e.g. "Ahmed the Hustling Sales Rep", "Leila the Overworked Founder")? Just tell me.