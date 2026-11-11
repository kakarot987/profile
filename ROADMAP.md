# Java Backend Contracting & Consulting Roadmap

> **Owner:** [@kakarot987](https://github.com/kakarot987)  
> **Starting point:** 6 years of professional Java development experience  
> **Roadmap created:** 11 November 2026  
> **Portfolio improvement deadline:** 20 November 2026  
> **Contract transition begins:** 21 November 2026  
> **Target role:** Senior Java Backend Engineer / Distributed Systems Contractor  
> **Long-term direction:** Independent Java Backend & Distributed Systems Consultant

## Mission

Move from full-time Java development to contract-based work **without unnecessary financial risk**. Build a portfolio that demonstrates **correctness, system design, low-level design, reliability, tests, and delivery**, then use that evidence for recruiter conversations, interviews, and client acquisition.

**Guiding principles**

1. **Quality over quantity.** A handful of demonstrably working repositories beats dozens of unverified demos.
2. **Source code over README claims.** Test important behavior; separate implemented features from planned work.
3. **System design + low-level design.** Show both distributed architecture decisions and clean object-oriented abstractions.
4. **Evidence over buzzwords.** Include reproducible instructions, passing tests, CI links, screenshots/demos, and failure cases.
5. **Contract first, resign second.** Keep employment until an acceptable signed contract and financial runway are in place.

---

## Progress dashboard

| Milestone | Target date | Definition of done | Status |
| --- | --- | --- | --- |
| Resolve critical code/security/portfolio issues | **13 Nov 2026** | Priority issues triaged and important fixes tested | ⬜ Not started |
| Complete 3 system-design case studies | **18 Nov 2026** | Each has HLD, key flows, failure cases, design tradeoffs | ⬜ Not started |
| Showcase 3–5 credible LLD pattern demos | **19 Nov 2026** | Correct code, meaningful tests, understandable docs | ⬜ Not started |
| Final portfolio quality gate | **20 Nov 2026** | Clean build, credible README, pinned repos, evidence | ⬜ Not started |
| Fresh portfolio review and positioning | **21–30 Nov 2026** | Top projects selected, resume and LinkedIn updated | ⬜ Not started |
| Begin outreach and interviews | **1–20 Dec 2026** | Consistent applications, referrals, recruiter contacts | ⬜ Not started |
| Negotiate qualified offers | **21 Dec 2026 – 20 Jan 2027** | Assess bill rates, scope, term, payment risk | ⬜ Not started |
| Decide on safe employment transition | **21 Jan – 20 Feb 2027** | Signed contract + runway + viable start date | ⬜ Not started |

> Update status manually as work completes. Dates are targets, **not promises**; prioritize correctness if a fix takes longer.

---

# Phase 1 — Portfolio engineering sprint (11–20 November 2026)

## A. Cross-repository quality and security (P0)

- [ ] Review all showcased repos for **committed `.env` files, passwords, API keys, tokens and sensitive data**.
- [ ] If a real secret was exposed, **rotate/revoke it** and address Git history as appropriate; adding `.gitignore` alone is insufficient.
- [ ] Remove tracked generated files/caches such as `.gradle/`, `target/`, `build/`, and IDE-specific outputs where they do not belong. Add suitable `.gitignore` rules.
- [ ] Verify each flagship repo builds and runs from a **clean clone** on the documented branch.
- [ ] Fix broken links, directory names, nonexistent commands, and incorrect port/environment instructions.
- [ ] Add or verify unit tests for core logic and **integration tests for major failure paths**.
- [ ] Make CI run appropriate checks; verify actual results rather than claiming success from the presence of a workflow file.
- [ ] Document authentic guarantees, shortcomings, prerequisites, and future work. Avoid unsupported terms such as “production-ready” or claimed performance figures.
- [ ] Ensure demo services and endpoints are appropriately local/sandboxed; never present mock payment, simulated trading, or AI hypotheses as production outputs.

### Concrete issues previously observed

- [ ] **Adapter:** In [`PaymentAdapterFactory`](https://github.com/kakarot987/AdapterPatternNodePlusReact), `getAvailableProviders()` uses `getAdapter("__init__")` for map initialization, which can throw `IllegalArgumentException`; initialize the map safely and test the provider-list endpoint.
- [ ] **Adapter:** Inspect tracked `bff/.env` for secrets; remove or replace it with a sanitized `.env.example`. Remove tracked Gradle caches.
- [ ] **Singleton:** In [`DatabaseConnection`](https://github.com/kakarot987/singleton-pattern), the double-checked-locking `instance` lacks `volatile`; correct the implementation and misleading “Bill Pugh” description. Clarify that one shared JDBC connection is **not** an appropriate production substitute for a connection pool.
- [ ] **Proxy:** In [`ReportServiceProxy`](https://github.com/kakarot987/proxy-design-pattern), `requestCount > 20` creates a lifetime request cap; implement a defined rate-limit window or explicitly document the demo behavior. Bound/expire cache entries and add concurrency tests.
- [ ] **Classic Builder:** Remove compiled `target/` files committed in [`classic-builder-design-pattern`](https://github.com/kakarot987/classic-builder-design-pattern).
- [ ] **Swiggy Clone:** Check Compose paths: `restaurant-partner` is referenced while a repository directory is named `restuarent-partner` (verify/fix in code and docs).
- [ ] **Spring AI:** Its ChatClient README describes simulated responses. Verify real provider integration before describing this as a functioning live-AI solution.
- [ ] **Trading App:** Validate the *current* source and tests; previous architecture notes describe an outdated persistence/publishing issue, whereas newer source implements a PostgreSQL outbox pattern. Update documentation according to verified behavior.

## B. High-level system design: build 3 flagship case studies (P0)

Create `docs/SYSTEM_DESIGN.md` (or an equivalent discoverable path) for the top 3. Each should have:

- [ ] Problem statement, **functional requirements**, **non-functional requirements**, assumptions and scope.
- [ ] Clear **HLD diagram** and ownership boundaries (services, databases, queues, clients).
- [ ] One **sequence diagram** for the most important end-to-end workflow.
- [ ] APIs, data model and event/message contracts.
- [ ] Design decisions with alternatives and tradeoffs (e.g., sync vs async; SQL vs NoSQL).
- [ ] Failure scenarios: crashes, broker outages, duplicate events, timeouts, retries, poison records.
- [ ] Consistency, idempotency, recovery and observability guarantees **as implemented**.
- [ ] Scaling options and bottlenecks, without invented benchmarks.
- [ ] Relevant tests or executable demo proving the critical path.
- [ ] Explicit **implemented / simulated / planned / known limitation** sections.

### Flagship HLD candidates

#### 1. [Swiggy Clone](https://github.com/kakarot987/swiggy-clone) — Distributed order lifecycle

- [ ] Map customer order → restaurant acceptance → delivery-agent events.
- [ ] Explain service boundaries and who owns authoritative order state.
- [ ] Document Kafka topics, producer/consumer behavior, order-state transitions and message headers.
- [ ] Examine **database-save-then-Kafka-publish** failure window and design a durable recovery strategy (e.g., transactional outbox, as justified).
- [ ] Document idempotency, duplicate delivery, retry policies, dead-letter/quarantine handling and out-of-order events.
- [ ] Validate Docker Compose paths, local startup, and an executable order-flow demonstration.

#### 2. [Trading App](https://github.com/kakarot987/trading-app) — Real-time event processing

- [ ] Diagram exchange/market ingestion → Kafka → trade processor → PostgreSQL/outbox → Kafka → WebSocket.
- [ ] Verify current `TradeConsumer`, `TradePersistenceService`, `OutboxPublisher`, and `TradeQuarantineService` behavior.
- [ ] Run relevant Testcontainers and integration tests; record actual passing results.
- [ ] Verify duplicate trade replay, DB outage, Kafka outage, restart with pending outbox, and poison-event behavior.
- [ ] Document **at-least-once delivery**, identity-based deduplication, Redis cache limitations and outbox scaling constraints.
- [ ] Record honest measurements only after reproducible load tests.

#### 3. [Amazon Clone Backend](https://github.com/kakarot987/Amazon_Clone_BckEnd) — Distributed commerce consistency

- [ ] Document API gateway, auth, service boundaries, MongoDB/MySQL, RabbitMQ and operational tools.
- [ ] Explain transactional outbox + consumer inbox/deduplication with sequence diagrams.
- [ ] Describe reservation/checkout consistency guarantees and remaining saga/reconciliation gaps.
- [ ] Confirm CI/test and local demo behavior; retain clear mock-payment/shipping caveats.
- [ ] Show how an engineer detects and investigates stuck operations.

### Additional HLD candidates (P1; select only if they outperform the above)

- [ ] [Gyaandaan](https://github.com/kakarot987/gyaandaan): student, teacher, course-assignment services and Eureka; explain boundaries, discovery and failure handling.
- [ ] [TracePilot AI](https://github.com/kakarot987/tracepilot-ai): Java/Spring AI workflow execution, evidence traceability, leases, bounded tools, safety and review; distinguish local MVP from production readiness.
- [ ] [Doot](https://github.com/kakarot987/doot): OAuth2/JWT login flows, identity-provider integration, trust boundaries and token handling.
- [ ] [MyCafeteria](https://github.com/kakarot987/myCafeteria) / [RecipeQueen](https://github.com/kakarot987/recipeQueen): supporting frontend/backend application architecture; do not prioritize over distributed-systems case studies.

## C. Low-level design: 9 design-pattern repositories (P0/P1/P2)

**Goal:** Demonstrate **why** a pattern is useful, not just where its classes are located. For the top 3–5 pattern demos, add a small UML/class diagram, before/after explanation, use cases, tests, and a working demo.

| Priority | Repo | What to verify or improve |
| --- | --- | --- |
| **P0** | [Adapter](https://github.com/kakarot987/AdapterPatternNodePlusReact) | Fix provider initialization; test Stripe/PayPal/Razorpay adapters, BFF mapping, error cases; label mock provider SDKs clearly |
| **P0** | [Singleton](https://github.com/kakarot987/singleton-pattern) | Fix `volatile`/thread-safety issue, align names with patterns; compare eager, lazy, DCL, holder and enum singletons |
| **P0** | [Proxy](https://github.com/kakarot987/proxy-design-pattern) | Rate-limit semantics, bounded cache, expiration, concurrency tests; explain proxy vs decorator |
| **P1** | [Factory Method](https://github.com/kakarot987/factory-design-pattern) | Test factory registration, unknown channels, extensibility; explain when factory is preferable to `switch` |
| **P1** | [Abstract Factory](https://github.com/kakarot987/abstract-factory-pattern) | Test product-family compatibility, injected factory lookup; contrast with Factory Method |
| **P1** | [Decorator](https://github.com/kakarot987/decorator-design-pattern) | Test decorator ordering, invalid add-ons, composition; use `BigDecimal`/minor units if modeling money |
| **P1** | [Facade](https://github.com/kakarot987/Facade-design-pattern) | Test scene orchestration, subsystem failures and ordering; explain facade vs orchestration in distributed systems |
| **P2** | [Classic Builder](https://github.com/kakarot987/classic-builder-design-pattern) | Validate required/optional fields, immutability; remove tracked build outputs |
| **P2** | [Generic Builder](https://github.com/kakarot987/generic-builder-patter) | Clarify that the pizza implementation is a concrete builder, not a generic Java type-safe builder; improve naming/docs |

**Checklist for each selected LLD showcase**

- [ ] Brief problem statement and “without this pattern” example.
- [ ] Pattern roles mapped to real classes.
- [ ] Diagram or short code walkthrough.
- [ ] Unit tests for behavior, invalid inputs, and extensibility.
- [ ] Clean local run instructions and expected output.
- [ ] Explanation of tradeoffs and when **not** to use the pattern.

## D. Supporting specialist repositories (P1)

These can show breadth but **do not all need individual rewrites before 20 November**.

- [ ] [GraphQL Pagination](https://github.com/kakarot987/graphQL-pagination-strategies): demonstrate cursor vs offset pagination and tests for stable ordering.
- [ ] [Spring Data + jOOQ + Liquibase](https://github.com/kakarot987/Spring-Data-Jooq-Liquibase-Mysql) and [Liquibase jOOQ Spring Boot](https://github.com/kakarot987/Liquibase-Jooq-Spring-Boot): add schema-change examples, query examples and migration verification; differentiate overlapping repos.
- [ ] [Spring Boot ELK](https://github.com/kakarot987/spring-boot-elk): demonstrate correlation IDs, structured logging and useful incident queries.
- [ ] [Spring Boot Terraform](https://github.com/kakarot987/spring-boot-terraform): validate actual deployment instructions and infrastructure-security defaults; never imply all cloud targets were deployed if unverified.
- [ ] [Spring Boot CI/CD](https://github.com/kakarot987/spring-boot-cicd): distinguish pipeline **templates/configurations** from workflows confirmed running against an application.
- [ ] [Spring Boot Containerization](https://github.com/kakarot987/spring-boot-containerization), [MVC Docker Demo](https://github.com/kakarot987/mvc-docker-demo), [GCP Cloud Lab](https://github.com/kakarot987/gcp-cloud-engineering-lab): retain as cloud/operational supporting evidence.
- [ ] [Spring WebFlux Reactive API](https://github.com/kakarot987/spring-webflux-reactive-api-demo) and [Spring API Gateway](https://github.com/kakarot987/spring-api-gateway): add concise practical use cases, tests and verified startup instructions if showcased.
- [ ] [Spring AI](https://github.com/kakarot987/spring-ai): verify live model integration instead of relying on simulated response demos.

## E. GitHub profile and presentation (P0)

- [ ] Rewrite [profile README](https://github.com/kakarot987/kakarot987) for **Senior Java Backend / Distributed Systems Contracting**.
- [ ] Replace generic skills inventory with: **who I help, what I build, evidence, how to contact me**.
- [ ] Fix or add valid LinkedIn/contact links (avoid publishing personal information you don't want exposed).
- [ ] Pin **4–6 repos** across distributed systems, system design, LLD, and deployment/AI.
- [ ] Create a top-level portfolio navigation table grouping repositories into **HLD**, **LLD**, **cloud/DevOps**, **AI**, **other experiments**.
- [ ] Add diagrams and 1–2 short recorded or repeatable demos of core project functionality.
- [ ] Verify every repo claimed as “showcase” has setup commands that work.
- [ ] Keep learning exercises visible if useful, but clearly label them as exercises rather than commercial products.

### Proposed pinned portfolio (subject to the November 21 re-review)

1. **Amazon Clone Backend** — reliability and distributed commerce.
2. **Trading App** — streaming/event processing and persistence.
3. **Swiggy Clone** — domain workflows across services.
4. **TracePilot AI** — evidence-grounded AI-assisted backend workflows.
5. **Adapter Pattern** — LLD / integration boundaries.
6. **Terraform or ELK** — deployment / observability evidence.

> A pin is earned by a working, testable, explainable artifact—not by repo size or popularity. Change the lineup if the code review warrants it.

---

# Phase 2 — Contract-market preparation (21–30 November 2026)

## Positioning

**Suggested headline:**

> Senior Java Backend Engineer | 6 Years | Spring Boot, Microservices & Distributed Systems | Open to Contract Opportunities

**Primary service positioning:** Java backend engineering, service modernization, asynchronous/event-driven systems, reliability and integration work.  
**Secondary specialties:** Cloud/containerization, observability, Spring AI integration (after verification).

### Checklist

- [ ] Re-review the latest GitHub source and CI outcomes; choose the **best 3–5 client-facing examples** based on quality, not README length.
- [ ] Prepare a **1–2 page contract-focused resume** emphasizing professional impact and measurable production outcomes (no fabricated numbers).
- [ ] Update LinkedIn headline, About and featured projects; enable suitable contract visibility.
- [ ] Write a **60-second introduction** explaining experience, strongest backend problems solved, and preferred contract work.
- [ ] Prepare concise stories about design decisions, failures, debugging, migrations and cross-team communication from **real employment experience** (respect confidentiality).
- [ ] Choose initial contract target: **6–12 month Java backend engagement**, direct client or reliable agency.
- [ ] Build an initial list of suitable recruiters, staffing firms, previous colleagues, and engineering leaders.
- [ ] Research current contract pricing using **actual offers**, not only illustrative multipliers or unverified online estimates.

---

# Phase 3 — Applications, referrals & interviews (1–20 December 2026)

## Pipeline

- [ ] Apply to **10–15 well-matched roles per week** (quality applications, not bulk spam).
- [ ] Search for: `Senior Java Contract Developer`, `Spring Boot Contractor`, `Java Microservices Contract`, `Backend Engineer Contract`, `Contract-to-Hire Java`.
- [ ] Explore LinkedIn, Naukri, specialized staffing firms, professional referrals, and suitable global contract marketplaces.
- [ ] Track each application: company, source, client vs agency, rate, term, location/time zone, recruiter, status, next follow-up.
- [ ] Contact former colleagues and engineering managers for warm referrals without disclosing confidential client information.
- [ ] Follow up professionally on active conversations and record interview feedback.

## Interview plan

- [ ] **Java:** collections, concurrency, JVM/GC basics, streams, exception design, Java 17/21 features.
- [ ] **Spring:** Boot, transactions, Security, REST contracts, validation, testing and observability.
- [ ] **Data:** SQL indexing, transactions, locking, isolation, migrations, cache invalidation.
- [ ] **Distributed systems:** Kafka offsets, consumer groups, retries, ordering, outbox/inbox, idempotency, consistency and failure recovery.
- [ ] **System design:** design an order workflow, event-ingestion platform, notification service, API gateway and audit system.
- [ ] **LLD:** defend Adapter vs Strategy, Factory Method vs Abstract Factory, Proxy vs Decorator, Builder, Facade and Singleton lifecycle issues.
- [ ] **Live portfolio demo:** walk through architecture, a real failure case, the code that addresses it, and test evidence.
- [ ] **Consulting:** requirement clarification, estimating scope, communicating tradeoffs, handling change requests, and managing delivery risk.

---

# Phase 4 — Offers, negotiation & safe transition (21 December 2026 – 20 February 2027)

## Evaluate contract opportunities

- [ ] Clarify whether the arrangement is fixed-term employment, agency contractor, contract-to-hire, or independent B2B consulting.
- [ ] Confirm rate, **billable hours/days**, expected monthly revenue, non-billable time, payment cycle, currency, fees, and tax treatment.
- [ ] Check term, renewal likelihood, early termination clause, mutual notice period and any payment on termination.
- [ ] Review ownership of work/IP, confidentiality, non-solicitation, liability caps, insurance, substitution, exclusivity and scope-change procedure.
- [ ] Validate the identity/payment history of any direct client or agency; use appropriate written contracts and invoicing.
- [ ] Compare **total expected net value** against current CTC, benefits, paid leave, provident fund, insurance, equipment costs and gaps between engagements.
- [ ] Keep a buffer of roughly **6 months of essential expenses** (adjust upward for dependents, EMIs or uncertain work).
- [ ] For India-based independent consulting, discuss GST, applicable tax regime, invoicing and advance tax with a qualified chartered accountant. Do not assume every software activity qualifies for the same presumptive-tax rules.
- [ ] Check current employment notice period, restrictions and conflicts before undertaking paid side work or resigning.

### Pricing worksheet (illustrative, not market data)

- Current CTC: `₹_______ / year`
- Minimum required annual personal take-home: `₹_______`
- Business, insurance and other incremental annual costs: `₹_______`
- Expected billable months per year: `_______` (conservatively fewer than 12 for independent contracting)
- Minimum monthly **invoiced** rate required: `₹_______`
- Desired monthly invoiced rate: `₹_______`
- Actual rates validated from qualified opportunities: `₹_______`

> An earlier planning heuristic was **1.5–1.8× current CTC on an annualized basis**. Treat this only as a scenario to evaluate, **not** a promise of the Indian or international contracting market. Compute net economics and compare real offers before deciding.

## Resignation decision gate

**Do not resign just because the GitHub portfolio looks strong.** Only consider resignation after:

- [ ] A suitable contract is **signed** by authorized parties.
- [ ] Scope, hours, start date, location/time-zone expectations and payment terms are clear.
- [ ] Termination and liability exposure are acceptable.
- [ ] Notice-period timing works without risking the new engagement.
- [ ] Financial runway, health insurance, taxes and household obligations are addressed.
- [ ] The expected financial and career benefits justify the tradeoffs versus full-time work.

---

# Reusable case-study template

Create `docs/SYSTEM_DESIGN.md` for flagship projects with this outline:

```markdown
# <Project> — System Design Case Study

## 1. Problem and requirements
- Users, use cases, scope, assumptions
- Functional and non-functional requirements

## 2. High-level architecture
- Architecture diagram
- Service/data ownership, infrastructure and dependencies

## 3. Key end-to-end flow
- Sequence diagram
- API/event contracts and state transitions

## 4. Data and consistency
- Database decisions, transactions, deduplication and recovery

## 5. Reliability and security
- Failure cases, retry/backoff, DLQ/quarantine, observability
- Authentication/authorization and threat boundaries

## 6. Scale and tradeoffs
- Bottlenecks, alternatives, limitations, measured benchmarks (if any)

## 7. Demonstration and tests
- Reproducible setup, scripts, test/CI links, expected output

## 8. Implementation status
- Implemented and verified
- Implemented but not yet verified
- Simulated/mock
- Planned
- Known gaps
```

# Weekly review questions

1. Which *verified engineering problem* can I now explain better than last week?
2. Which project has stronger evidence: working code, tests, failure handling, and useful docs?
3. Can a reviewer clone and run the project without relying on me?
4. Does my GitHub reflect the problems **clients pay to solve**?
5. Am I making progress toward qualified contract conversations, rather than only adding projects?

---

## Notes on evidence and scope

This roadmap consolidates review observations from **11 November 2026**. Reviews sampled repository trees, READMEs and selected source files; **they were not a full security audit or verified end-to-end test of every project**. Some README claims may lag current source. Re-check implementation and CI before calling an item fixed or completed.

**Final objective:** By 20 November, aim for **3 defensible HLD case studies**, **3–5 tested LLD demos**, **4–6 credible pinned repos**, and **one compelling profile**, then focus on **recruiter outreach, contract interviews and a safe career transition**.
