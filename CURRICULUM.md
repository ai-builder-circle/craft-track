# The Craft Track

**A 16-week mentored programme: building production APIs by hand.**

Student: Kananelo Mofokeng
Domain: FreelanceForge (quoting, invoicing, payments for SA freelancers)
Primary language: Java 21 → transfer test in Python/FastAPI
Outcome: two repositories that read as evidence of judgement, not tutorial completion.

---

## 0. The Contract

This is the part that makes the rest work. Read it twice.

### What I do

- Write specifications, acceptance criteria, and failing-test descriptions **in prose**.
- Ask questions during review rather than supplying fixes.
- Explain concepts, mechanisms, and history.
- Attack your API and tell you nothing except that it fell over.
- Refuse to advance you past a phase gate until you can defend the work verbally.

### What I do not do

- Write any code that lands in `src/`. Not a controller, not a value object, not a config file.
- Give you a snippet longer than five lines, and never one containing domain logic.
- Tell you the answer when the struggle *is* the lesson. I will tell you when the struggle is just missing information — those are different, and I'll name which one you're in.

### What you do

- Write 100% of production code, unassisted, from your own notes and the primary sources.
- Keep a `LEARNING.md` journal. Every entry: what I tried, what broke, what I now believe, what I'm still unsure about.
- Every commit compiles and passes tests. Your `git log` is the transcript of your thinking — treat it as a deliverable.
- Come to review with a *position*, not a question. "I put authorization in the application layer because X" beats "where does authorization go?"

### Honesty clause

If you look something up, that's research — good. If you paste something you don't understand, that's debt, and it compounds. Flag it in the journal. I will find it in review anyway, and it's cheaper if you say it first.

---

## 1. Why this domain and this language

**FreelanceForge** is your canonical domain across five previous repos, so you already speak the language — and Domain-Driven Design is nearly impossible in a domain you don't know. It also has genuinely hard invariants: money arithmetic, 15% VAT with rounding traps, quote expiry, state transitions, deposit thresholds. A todo app has none of these, which is why todo apps teach nothing about DDD.

**Java 21** because the type system will *enforce* the things you're learning — immutable value objects, sealed hierarchies for state, records, no accidental nulls if you're disciplined. DDD's entire canon is written in Java and C#. It's also the dominant enterprise language in SA banking and insurance, which is where the jobs are.

**Python/FastAPI at the end** as the transfer test. If you can only do DDD where the compiler holds your hand, you learned Java. If you can do it in Python, you learned modelling.

You may override either choice. Say so now, not in week six.

---

## 2. Repository shape

**Repo 1 — `http-by-hand`** (Phase 1, deliberately disposable)
A working HTTP API with no web framework. Proof you know what Spring hides.

**Repo 2 — `freelanceforge-api`** (Phases 2–5, the flagship)

```
freelanceforge-api/
├── README.md              ← a technical essay, not a setup guide
├── docs/
│   ├── glossary.md        ← the ubiquitous language
│   ├── threat-model.md
│   └── decisions/         ← ADR-001, ADR-002, ...
├── src/main/java/.../
│   ├── domain/            ← ZERO framework imports. ZERO database imports.
│   ├── application/       ← use cases, commands, transactions
│   └── infrastructure/    ← HTTP, persistence, security adapters
└── src/test/java/...
```

Tag each phase: `v0.1-http`, `v0.2-domain`, `v0.3-persistence`, `v0.4-secure`, `v1.0`.

The single most valuable artefact you will produce is a `domain/` package that compiles and tests in under a second with no database, no framework, and no mocks. Very few junior candidates can show that. It's the whole game.

---

## 3. Weekly rhythm

| Day | Work |
|---|---|
| Mon | Concept + primary source reading. Answer the week's reading question in writing. |
| Tue–Thu | Build against the week's acceptance criteria. |
| Fri | Review with me. Socratic — expect to be interrogated, not corrected. |
| Sat | Drill / kata. Small, timed, constrained. |
| Sun | Write: one ADR or one journal entry. Non-negotiable. |

Roughly 12–15 hours a week. This runs alongside WeThinkCode, not instead of it.

---

## Phase 0 — Diagnostic (before Week 1)

Three tasks. No preparation, no research. I need to see where you actually are, not where your CV says you are.

1. From memory, write down every HTTP status code you'd use in a CRUD API and the exact condition for each. Then write the ones you're unsure about in a separate list.
2. In 200 words: what is the difference between an entity and a value object, and why does it matter?
3. Explain how you would prevent user A from reading user B's invoice. Give me your actual mechanism, not "check permissions."

Send all three as prose. I'll calibrate the plan from your answers.

---

## Phase 1 — HTTP and the Wire (Weeks 1–3)

**Goal:** never be confused by a framework again.

### Week 1 — The protocol
Build an HTTP server on `com.sun.net.httpserver`. For one day, drop lower: parse a raw request line and headers off a `ServerSocket` by hand so you feel the shape of the protocol.

- Request line, headers, body framing (`Content-Length` vs chunked)
- Method dispatch and a routing table you wrote yourself
- Why HTTP is stateless and what that costs you later

**Reading:** RFC 9110 §9 (Methods) and §15 (Status Codes). Fielding's dissertation, Chapter 5 only.
**Reading question:** Fielding never says "JSON over HTTP." What does he actually require, and which of his constraints does your API violate?

### Week 2 — Representations and errors
- JSON serialisation without a library (you've done this — now do it well: nulls, nested objects, escaping)
- Content negotiation: `Accept`, `Content-Type`, 406, 415
- Status codes as a **decision tree**, not a lookup table
- Errors as RFC 9457 Problem Details, consistently, everywhere

**Drill:** Write the decision tree that takes you from "something went wrong" to a specific 4xx. Defend every branch.

### Week 3 — The properties that separate professionals from tutorial-followers
- Safety and idempotency: why `DELETE` is idempotent but not safe; why `POST` isn't; how to make it so with an `Idempotency-Key`
- Conditional requests: `ETag`, `If-Match`, `If-None-Match`
- Caching semantics: `Cache-Control`, validators, when a 304 is correct
- `PUT` vs `PATCH`, and JSON Merge Patch (RFC 7386)
- Pagination: offset vs cursor, and why offset breaks under concurrent writes

**Deliverable (`http-by-hand`, tag `v0.1-http`):** in-memory CRUD API, no framework, hand-written test client, README justifying every status code chosen.

**Phase gate — viva:** Two clients `PUT` the same resource simultaneously. Walk me through exactly what happens, and how `If-Match` fixes it. Hold this answer. It comes back in Week 9.

---

## Phase 2 — The Domain Model (Weeks 4–7)

**Goal:** the model is the system. The database is a detail. The API is a detail.

### Week 4 — Language before code
Almost no code this week. This is the week most people skip and it's the week that decides whether you actually learn DDD.

- Solo event storming: map FreelanceForge as a timeline of domain events
- Extract the ubiquitous language into `docs/glossary.md`
- Identify candidate bounded contexts: does "Client" mean the same thing in Quoting as in Payments? (It doesn't. That's the lesson.)

**Reading:** Evans, *Domain-Driven Design*, Ch. 1–2. Khononov, *Learning Domain-Driven Design*, Ch. 1–3.
**Reading question:** Give me three terms from freelancing where you and a freelancer would mean different things.

### Week 5 — Value objects: making illegal states unrepresentable
Build these, immutable, self-validating, with correct equality:

- `Money` — refuses to add ZAR to USD, at compile time or construction time
- `VatRate`, `LineTotal`, `EmailAddress`, `QuoteNumber`, `DateRange`

**The trap I want you to fall into:** compute VAT per line item and round, then sum. Then compute the sum and round once. Reconcile the difference with what SARS actually expects. Write an ADR on which you chose.

**Drill:** find every place a primitive `String` or `BigDecimal` is doing a domain job and replace it. Count them. That number is your "primitive obsession" score.

### Week 6 — Entities, aggregates, invariants
The hardest week in the programme.

- Identity vs equality; entity lifecycle
- Aggregate = consistency boundary = transaction boundary
- Vernon's rules: model true invariants inside the boundary, design small aggregates, reference other aggregates **by identity**, update others eventually
- `Quote` as an aggregate root: line items inside, `Client` referenced by `ClientId` only
- State machine: `DRAFT → SENT → ACCEPTED | DECLINED | EXPIRED`, enforced by the model, not by an `if` in a controller

**Invariants to enforce:** a sent quote cannot be edited; an expired quote cannot be accepted; total always equals sum of lines plus VAT; a quote must have at least one line item to be sent.

**Reading:** Vernon, *Effective Aggregate Design* (three-part paper). Fowler, "Anemic Domain Model."
**Reading question:** Why is `Client` not inside the `Quote` aggregate? Give me two independent reasons.

### Week 7 — Services, events, ports
- Domain services (logic that belongs to no single entity) vs application services (orchestration)
- Domain events: `QuoteAccepted`, `QuoteExpired` — raised by the aggregate, dispatched outside it
- Repositories as **domain-owned collection interfaces**; the interface lives in `domain/`, the implementation does not
- Hexagonal architecture: ports and adapters, dependency direction always inward

**Reading:** Cockburn on Hexagonal Architecture. Vernon, *DDD Distilled*, Ch. 6–8.

**Deliverable (tag `v0.2-domain`):** `domain/` package with zero framework and zero persistence imports, full test suite running in under one second with no mocks and no database.

**Phase gate — viva:** Defend your aggregate boundaries. I will propose a feature designed to break them. Your job is to tell me whether it breaks the model or the model was wrong.

---

## Phase 3 — Persistence and the Application Layer (Weeks 8–10)

### Week 8 — The impedance mismatch
- Implement repositories with **plain JDBC first**. Feel the mapping cost. Write it down.
- Then introduce JPA/Hibernate and articulate precisely what it bought you and what it cost you
- Schema migrations with Flyway from day one
- The rule: the ORM never leaks into `domain/`. If you're annotating your aggregate, stop and write an ADR arguing for it.

### Week 9 — Transactions and consistency
- Transaction boundary = aggregate boundary. One aggregate per transaction.
- Isolation levels; the lost update problem demonstrated with two real sessions
- Optimistic locking with a version column → **and now connect it back to Week 3's `If-Match`**. They are the same idea at two altitudes. This is the week the programme clicks.
- Eventual consistency between aggregates via domain events

**Reading:** RFC 9110 §13 (Conditional Requests) again, alongside your ORM's optimistic locking docs. Read them as one topic.

### Week 10 — Use cases and the API contract
- Application services as use cases: `AcceptQuote`, `IssueInvoice`
- Commands in, DTOs out. **Your API contract is not your domain model** — if you serialise the aggregate directly, every refactor becomes a breaking change
- Integration tests with Testcontainers against real PostgreSQL
- Test pyramid: fast domain tests, slower use-case tests, few end-to-end

**Deliverable (tag `v0.3-persistence`):** full CRUD over real Postgres, migrations versioned, domain still framework-free, three-tier test suite green in CI.

**Phase gate — viva:** Delete PostgreSQL from the project and swap in an in-memory repository. If that takes more than an hour, your dependencies point the wrong way.

---

## Phase 4 — Security (Weeks 11–14)

### Week 11 — Threat model first
You've done STRIDE before. Do it on your own system this time, when you have something real to lose.

- Assets, trust boundaries, data flows
- STRIDE per boundary
- OWASP API Security Top 10 (2023) used as a **test checklist**, not a reading list

**Deliverable:** `docs/threat-model.md`, with each identified threat mapped to a mitigation or an accepted risk.

### Week 12 — Authentication
- Password storage: Argon2id, parameter selection, why salts, why a general-purpose hash is malpractice
- Sessions vs tokens: the actual trade-off — revocation and statefulness — not the blog-post version
- **Build session-based auth first.** It's simpler and revocable. Only then add JWT.
- JWT properly: `alg` confusion, `none`, signature verification, issuer and audience validation, expiry, key rotation, and the revocation problem it hands you
- Where OAuth 2.0 / OIDC actually applies, and why you don't need it here

**Reading:** RFC 8725 (JWT Best Current Practices). OWASP Authentication and Password Storage Cheat Sheets.
**Reading question:** You've just learned a user's token was stolen. Walk me through your response under session auth, then under stateless JWT. Which system do you want at 2am?

### Week 13 — Authorization
Where most tutorials die and most production breaches happen.

- Broken Object Level Authorization (BOLA/IDOR) is the #1 API vulnerability. Build a test suite that proves user A cannot read, update, or delete user B's quotes — by ID guessing, by nested route, by bulk endpoint, by filter parameter.
- Where does authz belong? Not the controller alone. Argue it in an ADR.
- RBAC vs ABAC, and why "admin" is not a design
- Tenant scoping enforced at the repository layer, so forgetting it is impossible rather than merely discouraged

### Week 14 — Hardening
- Input validation at the edge vs invariant enforcement in the domain — both, different jobs, and knowing which is which
- Rate limiting and its relationship to your idempotency work in Week 3
- Secrets: environment, rotation, never in git, and what to do when it *is* in git
- TLS, HSTS, CORS explained precisely (CORS is not a security control for your API — know why)
- Audit logging: what to log, what never to log
- Dependency and secret scanning wired into CI

**Red-team day:** I hand you an attack list. You prove each one fails, with a named test. Any attack that succeeds costs you the phase gate.

**Deliverable (tag `v0.4-secure`):** threat model, security ADRs, and an authorization test suite that reads like a specification.

---

## Phase 5 — Prove It (Weeks 15–16)

### Week 15 — A second bounded context
Add `Payments` (or your burial-society context, if you want the harder version).

- Context mapping: how do Quoting and Payments relate? Partnership, customer-supplier, or anti-corruption layer?
- Integrate via **domain events**, never a shared table
- Even an in-process event bus teaches the lesson; distribution is a deployment detail, not a design one

### Week 16 — The transfer test and the shop window
1. Reimplement one vertical slice — one aggregate, one use case, one endpoint — in Python/FastAPI, **from your own notes only**, without opening the Java code. If you can, you own the concepts. If you can't, you learned a framework, and we find out where the gap is.
2. Rewrite `README.md` as a technical essay: the domain, the modelling decisions, the trade-offs, what you'd do differently.
3. Record a 10-minute walkthrough. Explaining it out loud is where you discover what you don't understand.

---

## Anti-patterns I will fail you for

- Anemic domain model — entities that are bags of getters and setters with logic in services
- Serialising aggregates directly as API responses
- Validation living only in the controller
- Repositories returning DTOs, or accepting SQL from callers
- A `utils` or `helpers` package (it means you gave up on naming a concept)
- JWT used as a session because the tutorial did it
- `@Transactional` sprayed across the codebase
- Tests that mock the thing they're testing
- Any commit message that is just "fix" or "update"

---

## Primary sources

I can't browse or verify these from here, and I'd rather you check editions and links yourself than trust my recall — treat this as a starting point to verify, not a bibliography.

**Domain modelling**
- Eric Evans, *Domain-Driven Design* (2003) — read Part II and IV; Part III is heavy going first time
- Vaughn Vernon, *Domain-Driven Design Distilled* (2016) — start here, it's short
- Vaughn Vernon, *Effective Aggregate Design*, three-part paper — the single most useful thing on aggregates
- Vlad Khononov, *Learning Domain-Driven Design* (O'Reilly, 2021) — the most approachable modern treatment
- Scott Wlaschin, *Domain Modeling Made Functional* — F#, but the best writing anywhere on making illegal states unrepresentable
- Martin Fowler: "Anemic Domain Model," "Bounded Context," "DDD Aggregate"

**HTTP and API design**
- RFC 9110 — HTTP Semantics (the one to actually read)
- RFC 9457 — Problem Details for HTTP APIs
- RFC 7386 — JSON Merge Patch
- Roy Fielding, *Architectural Styles and the Design of Network-based Software Architectures* (2000), Ch. 5

**Security**
- OWASP API Security Top 10 (2023)
- OWASP Application Security Verification Standard (ASVS)
- OWASP Cheat Sheet Series — Password Storage, Authentication, Authorization, JWT
- RFC 8725 — JWT Best Current Practices
- Aaron Parecki, *OAuth 2.0 Simplified*

**Architecture and operations**
- Alistair Cockburn, "Hexagonal Architecture"
- Michael Nygard, *Release It!* (2nd ed.)
- Testcontainers documentation

---

## Phase gates summary

| Gate | You must be able to |
|---|---|
| 1 | Explain concurrent-write conflict resolution over HTTP without hand-waving |
| 2 | Defend your aggregate boundaries against a feature designed to break them |
| 3 | Swap the database out in under an hour |
| 4 | Survive a red-team pass with a named test per attack |
| 5 | Rebuild a slice in another language from your own notes |

No gate, no advance. Time is not the currency here — demonstrated understanding is.
