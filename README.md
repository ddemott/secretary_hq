# Secretary HQ

> ### ⚠️ This repository contains no source code — and that is deliberate.
>
> Secretary HQ is a commercial, proprietary product. This repository exists so
> that the project can be shown, discussed, and evaluated **without publishing
> the implementation**. What you see here is documentation only: what the
> product does, how it is put together, and the engineering standards it holds
> itself to.
>
> The source code is closed and remains the exclusive property of the author.
> It is intentionally withheld to protect copyright and commercial interest —
> not because there is nothing to show, but because there is a great deal to
> show and it is not free to take.
>
> **Interested in the code, a live demo, a technical walkthrough, or licensing?**
> Open an issue or get in touch — happy to talk.

---

**AI receptionist for service businesses.** Answers calls 24/7, books appointments, answers policy questions, captures leads and messages, and logs every interaction — call summaries, transcripts, outcomes, and analytics — to the owner's dashboard. No missed calls, no hold music, no voicemail.

Built for auto & vehicle, beauty & personal care, home services, fitness, food & beverage, and professional-services businesses. **30 business types across 6 categories**, each with its own vocabulary ("Bays / Technicians" for a tire shop, "Chairs / Stylists" for a salon, "Vans / Plumbers" for a plumbing service).

HIPAA verticals (medical, dental, chiropractic, optometry, veterinary) are permanently out of scope by design.

Live at **[secretaryhq.com](https://www.secretaryhq.com)**.

---

## Feature Inventory

### Voice & Calls

- Answers inbound calls 24/7 with a low-latency, streaming, human-like voice
- Identifies callers on arrival and matches them to existing customer records
- Greets callers by name when recognized; captures new callers into the address book automatically
- Owner-editable spoken greeting, AI disclosure line, and services menu — callers hear what the business can do before they speak
- Per-tenant AI persona: assistant name, voice, and five style flags (soft, cheerful, formal, warm, concise)
- **Question-tree call architecture** — the call's required information is *data*, not prompt text. Host code owns all call state across 10 statuses, computes liveness through branching choices, and discards answers stranded on a branch the caller abandoned
- **Goodbye gate** — the agent structurally cannot end a call while any selected item is unresolved. A stated goal cannot be forgotten
- Answers questions at any point mid-call without losing its place in the intake
- Anti-double-write guards: a completed action refuses to repeat, missing arguments are backfilled from recorded answers, and two consecutive tool failures convert the flow to "take a message" instead of retrying forever
- Honest dead-air handling — hold lines are spoken by a runtime timer and may name a lookup only while a tool is genuinely in flight
- Handles rejections, off-topic questions, and policy queries naturally; never speaks raw errors
- Records full call transcripts and generates AI post-call summaries
- Per-call outcome tracking (booked / transferred / message / no action) with a back-link to the appointment the call created
- Call-end context recapture — the stored customer snapshot reflects what the call produced, not what was true before it
- Stale-session reaper so every call ends with a completed record even if the worker dies mid-call
- Live cold transfer to a human via SIP REFER *(built; awaiting live different-carrier verification)*
- Phone-verification (OTP) flow bound to the individual call, so a proof of possession can never be replayed on a later call

### Appointment Booking

- Books appointments atomically — shift coverage, staff skills, resource availability, and timeslot occupancy checked in a single database transaction
- Race-safe under concurrent calls via GiST exclusion constraints on `(resource_id, time-range)` and `(employee_id, time-range)` — the find-then-insert race surfaces as a clean error, never a double-booking
- DST-safe and timezone-aware; cross-midnight and wrap-around night shifts handled correctly
- Customer-led: asks the caller's preferred time first, widens the window only if nothing fits, never imposes a slot
- Specific error codes drive the spoken response — the caller hears *why* a time didn't work, not a generic apology
- Service-to-employee and service-to-resource skill mapping enforced as the authoritative gate
- **Semantic service matching** — the caller's own words are embedded and matched against the service catalog by meaning (pgvector), so nobody has to say a catalog name out loud. Low-confidence matches fall through to the tenant default rather than silently booking the wrong thing
- 15-minute increment and duration validation; per-tenant booking buffers
- Owner-defined booking mechanics spoken verbatim after every successful booking — what actually happens at the booked time, in the owner's words
- Cancel and reschedule by phone
- Quick-book panel for walk-ins

### Knowledge Base

- Two-layer knowledge: live database lookups for facts (pricing, hours, availability, catalog) plus RAG over uploaded documents for policies
- Document upload — PDF, TXT, DOC, DOCX, MD — chunked, embedded, and retrieved by pgvector cosine similarity
- 40-pair policy Q&A seeded per business; owners extend, edit, and reword from the dashboard
- **Website-scan onboarding** — scans the owner's existing site and proposes knowledge-base entries for review
- **Knowledge suggestions queue** — scanned or inferred Q&A pairs land as suggestions with confidence and source URL; the owner accepts or rejects
- **"Explain this answer" RAG debugger** — shows which knowledge chunks the AI retrieves for a question, their similarity scores, and which ones would actually reach the model in production, so an owner can see *why* the AI answers the way it does
- Graceful spoken fallback whenever retrieval cannot answer

### Messages & Leads

- Takes a message when booking isn't what the caller wants, and emails it to the owner
- Pages the owner for urgent calls
- Job-inquiry capture for staffing calls — records the role in the caller's own words, and distinguishes the agency that called from the client where the work is
- A caller who only left a message still becomes a customer record
- Job inquiries link to the meeting they booked, and stamp a summary into that appointment
- One inquiry per call, enforced at the database level, so tool retries converge on a single row

### Scheduler & Dashboard

- Staff swimlane scheduler: 24-hour view, zoom, employee day focus with utilization stats
- Resource columns alongside staff lanes
- Appointment list view, calendar sub-view, and detail panel
- Week view and today's schedule on the home screen
- **Utilization heatmap** — weekday × hour grid of how much staffed time is actually booked
- Coverage-gap indicators across scheduler, services list, skill map, and setup wizard
- Interactive 3-column skill relationship map (employee → service → resource) plus a skill matrix
- 8 UI themes: light, dark, midnight, nord, sunset, forest, high-contrast, solarized
- Per-business vocabulary applied throughout the UI, resolved through a 3-tier fallback (tenant override → business template → generic)
- Mobile-responsive layouts
- First-run tour and a setup-progress pill that tracks what's still missing
- Keyboard shortcuts, focus trapping, confirm-modals on destructive actions, toast notifications
- URL-synced tab state so reloads, deep links, and browser back/forward all work

### CRM & Customer Records

- Full customer profiles with appointment history, call summaries, transcripts, and internal notes
- Searchable customer list
- Customer preferences stored durably and recalled on the next call ("prefers Maria", "last service: oil change")
- Square CRM bidirectional sync (customers + appointments), with timestamp-based merge and version tracking
- Google Calendar and Outlook Calendar OAuth integration — appointments auto-sync on create, update, delete, and cancel
- CRM connect / disconnect, sync-status counts, and sync-map cleanup

### Analytics & Reporting

- Top-line stats: calls, appointments, customers — volume, today, this week — plus recent activity
- Call analytics: call volume over time, call → booking conversion, caller abandonment, busiest hours, return rate, no-show patterns
- **"Why callers reached out"** — the agent classifies every non-booking call (no availability / wrong service / price / message / info) so the owner sees the *reason*, not just the count
- Coverage analysis (staffing versus demand)
- Per-call AI cost ledger — LLM tokens, STT audio seconds, and TTS characters recorded per call with an estimated cost

### Business Setup

- 7-step onboarding wizard: repeatable, re-enterable, with live coverage feedback
- Solo-business fast path for one-person businesses
- Service, employee, resource, and skill management; employee ↔ service and service ↔ resource assignment views
- Weekly schedule grid that fans out into a 4-week schedule, with a copy-week button for forward extension, plus shift overrides
- Dry-run coverage check before commit — the wizard shows what the schedule would look like before anything is written
- Industry templates preview per business type
- **Go-live panel** — phone number search, purchase, and routing, or call-forwarding setup, resumable from either the wizard or the Phone Assistant page

### Reminders & Communications

- Appointment reminder and confirmation scheduling via SMS and email, with caller-chosen custom lead times
- Consent-gated — only contacts who have opted in receive messages; opt-out keyword handling
- 60-second polling scheduler, batching due reminders per tick, with retry policy and rate limiting
- Database-level idempotency: at most one scheduled reminder per appointment per type
- Delivery-status webhooks recorded per message; sent-communications history view
- **Self-service links** — token-gated cancel and reschedule pages that need no login
- *SMS sending is behind a feature flag pending per-tenant 10DLC brand registration. The agent is told plainly that it cannot text, so it never promises a message it has no means to send.*

### Data, Audit & Recovery

- **Full data export** (GDPR Art. 15 access / Art. 20 portability, CCPA right-to-know) — machine-readable JSON dump of everything a business owns, with credentials and security tokens deliberately excluded
- Spreadsheet-shaped CSV exports for customers, appointments, and calls, with RFC-4180 escaping and a formula-injection guard
- **Owner-facing audit log** — who changed which booking, price, or staff member, and when, recorded by a database-level trigger on appointments, customers, resources, services, and employees
- **Version history and field-level restore** — per-record version timeline, restore individual fields from a past version, and copy fields between records
- Soft-delete everywhere, with a deleted-records panel and restore. Businesses are never hard-deleted by the app; purging is a deliberate manual maintenance act that dry-runs and prints its blast radius first
- Bulk-export routes are owner-gated; front-desk logins are rejected

### Accounts, Roles & Multi-Tenancy

- Row Level Security on every tenant table, with `FORCE ROW LEVEL SECURITY`
- Tenant isolation enforced in two layers: unauthenticated requests are rejected before any tenant is resolved, and an authenticated caller's supplied tenant must match their token
- JWT auth with short-lived sessions, bcrypt password hashing, auto-logout on 401, token refresh, password reset, password-rotation checks
- Role-based access: owner, admin, front-desk. Front-desk users see primary tabs only and are snapped back if they reach a restricted tab via a stale URL
- Team-member invitations
- Super-admin platform dashboard for cross-tenant management and support
- **Demo mode** — a public, one-click, fully isolated demo business with realistic seeded data, an expiring session, abuse rate limiting, and outbound SMS and CRM sync disabled

### Billing

- Stripe Checkout subscriptions with a subscription gate — unsubscribed businesses are blocked from paid features
- Webhook-driven subscription activation and access gating
- Payments for the *business's own customers* are deliberately out of scope — no PCI or payout liability; that stays with the owner's POS

### Platform & Operations

- Shallow liveness and deep readiness endpoints, the latter reporting database reachability and connection-pool saturation
- Authenticated Prometheus-style metrics endpoint: HTTP request counts and latency histograms, booking attempts by outcome, tool calls by outcome, sync dispatches, and error events
- Structured logging with per-request enrichment (service, environment, tenant, call id)
- Deadlock-prevention pool timeouts and fail-fast connection checkout
- Graceful shutdown that drains the pool
- Production environment validation — the server refuses to start without its required secrets
- On-demand simulation harness: health board, CI status, an end-to-end tool journey, an LLM tool-selection evaluation, a RAG accuracy evaluation, a billing check, and a browser-based voice call so the agent can be talked to without a phone
- A speech-synthesis verification command that opens the real audio socket and demands real audio bytes back — because "it compiles and the tests are green" is not the same as "it makes noise"

---

## Architecture

```
Inbound Call
    |
Carrier (PSTN + SIP trunk) --> LiveKit Cloud (SIP ingress)
                                          |
                                LiveKit Agent worker (Node)
                                — Deepgram Nova-3 (STT)
                                — OpenAI GPT-4.1-mini (LLM)
                                — Deepgram Aura (TTS, streaming)
                                          |
                                Fastify backend (agent tool API)
                                          |
                                PostgreSQL + pgvector (RLS multi-tenancy)
                                          |
                                Next.js 16 Dashboard
```

| Layer             | Tech                                                                                                                                                     |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Voice**         | Telnyx (carrier + SIP trunk), LiveKit Cloud (orchestrator), Deepgram Nova-3 (STT), OpenAI GPT-4.1-mini (voice LLM; 4o-mini for summaries/classification), Deepgram Aura (streaming TTS, per-business voice) |
| **Backend**       | Fastify 5.x, 29 route modules, JWT auth, Zod validation at every boundary, RLS via a per-request tenant-scoped client |
| **Frontend**      | Next.js 16 (App Router), React 19, Tailwind CSS 3.4, TypeScript, Lucide icons                                                                            |
| **Database**      | PostgreSQL + pgvector, 214 migrations, Row Level Security, atomic booking stored procedures with GiST exclusion constraints                               |
| **Agent runtime** | LiveKit Agents (Node). Call flow is a **question tree**: host-owned checklist, purpose-selected trees, goodbye gate, per-turn tool selection              |
| **Async**         | Inline in routes (post-call summaries, calendar sync) plus polling workers for reminders and stale-session reaping                                        |
| **Billing**       | Stripe Checkout, webhooks, subscription-gate middleware                                                                                                  |
| **Security**      | Helmet headers, rate limiting, CORS restriction, bcrypt, forced RLS                                                                                      |

---

## Engineering Notes

A few conventions this codebase holds itself to:

- **Every primary key is named `<table_singular>_id`**, never bare `id`, so joins are symmetric and `SELECT *` across joins stays unambiguous. Natural composite keys are preferred over surrogate UUIDs when the natural key is short and stable.
- **Tests cover happy *and* sad paths**, and every sad path carries 5W diagnostic context (Who, What, When, Where, Why) so a failure is debuggable from its output alone:

  ```typescript
  it('should reject country-code-only "+1"', () => {
    // WHO: inbound caller | WHAT: carrier sent only "+1" as phone
    // WHEN: partial caller ID before the full number resolves
    // WHERE: shared phone normalizer
    // WHY: a partial number must never become a customer record
    expect(normalizePhone('+1')).toBeNull();
  });
  ```

- **The database starts bare and ends bare.** The seed creates only what an empty business needs to exist; every test creates its own data and deletes it afterward. A failure is provably the code or the test, never a stray row from the last run.
- **Test it or delete it.** Code that cannot be exercised against a real external surface does not ship. Mocked-API tests prove the mock works, not the integration.
- **Measure before fixing.** One direct timing test beats hours of guessing about where latency lives.

---

## Scale

|                |                                                                             |
| -------------- | --------------------------------------------------------------------------- |
| **Phase**      | 13 — Production Readiness                                                   |
| **Voice AI**   | Live — inbound calls reach the agent; question-tree call flow in production  |
| **Tests**      | ~5,700 passing, zero skips, zero TypeScript errors |
| **E2E**        | 38 Playwright spec files                                                    |
| **Migrations** | 214                                                                         |
| **Business types** | 30, across 6 categories                                                 |

Merges to `main` are gated by branch protection behind four green CI jobs (backend, dashboard, agent, E2E).

---

## What's in this repository

| File                                 | Purpose                                                          |
| ------------------------------------ | ---------------------------------------------------------------- |
| [`README.md`](README.md)             | This file — what the product does and how it's built             |
| [`OWNER_GUIDE.md`](OWNER_GUIDE.md)   | Plain-language product tour written for business owners          |
| [`LICENSE`](LICENSE)                 | Proprietary commercial — all rights reserved, no license granted |

No source code, tests, migrations, configuration, or infrastructure definitions
are published here or anywhere else public.

---

## License

**Proprietary and commercial. All rights reserved.**

Secretary HQ is commercial software, licensed and not sold. Any use requires a
paid commercial license executed in writing with the copyright holder.

The documentation in this repository is published for evaluation and discussion
only. The Secretary HQ software, its source code, database schema, prompts, and
design are proprietary works and are **not** licensed for copying, duplication,
reuse, redistribution, derivative works, ML training, or use of any kind. No
license — express or implied — is granted by the presence of this repository.

See [`LICENSE`](LICENSE) for full terms.
