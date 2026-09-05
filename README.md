<h1 align="center">FitZone — Gym Management System</h1>

<p align="center">
  A full-stack gym management platform built as a graduation project.<br>
  Spring Boot 3 REST API + Flutter (mobile &amp; web) client — 7 role-based portals, real-time chat,
  QR check-in, online payments, finance &amp; payroll, Arabic/English localization.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Spring%20Boot-3.5-6DB33F?logo=springboot&logoColor=white" alt="Spring Boot 3.5">
  <img src="https://img.shields.io/badge/Java-21-007396?logo=openjdk&logoColor=white" alt="Java 21">
  <img src="https://img.shields.io/badge/Flutter-3.x-02569B?logo=flutter&logoColor=white" alt="Flutter 3">
  <img src="https://img.shields.io/badge/PostgreSQL-17-4169E1?logo=postgresql&logoColor=white" alt="PostgreSQL 17">
  <img src="https://img.shields.io/badge/Flyway-77%20migrations-CC0200?logo=flyway&logoColor=white" alt="Flyway">
  <img src="https://img.shields.io/badge/Load%20tested-1000%20VUs%20%C2%B7%200%25%20failures-success" alt="Load tested">
</p>

> **Showcase repository.** This repo exists to present the project — architecture, features and
> engineering decisions. Secrets, environment files and raw test artifacts are intentionally excluded.

---

## Table of Contents

- [Screenshots](#screenshots)
- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Roles &amp; Portals](#roles--portals)
- [Features](#features)
- [Architecture](#architecture)
- [Engineering Highlights](#engineering-highlights)
- [Load Testing](#load-testing)
- [Team](#team)

---

## Screenshots

All screenshots live in [`projectimages/`](projectimages).

### Authentication

| Login | Create account (with email verification) |
|:--:|:--:|
| <img src="projectimages/01-login.png" width="330"> | <img src="projectimages/02-register.png" width="330"> |

Language can be switched from the login screen itself, before signing in.

### Admin Portal

<p align="center"><img src="projectimages/03-admin-dashboard.png" width="900" alt="Admin dashboard"></p>

<p align="center"><i>Members vs. active subscriptions, monthly revenue and profit-by-type, with live badges on
pending shift records, vacations, subscriptions and enrollments.</i></p>

<p align="center"><img src="projectimages/04-admin-settings.png" width="900" alt="System settings"></p>

<p align="center"><i>System settings — gym profile, per-day opening hours, check-in and cancellation policies,
module feature toggles, and the subscription freeze policy.</i></p>

### Arabic (RTL)

<p align="center"><img src="projectimages/05-admin-dashboard-arabic.png" width="900" alt="Admin dashboard in Arabic, right-to-left"></p>

<p align="center"><i>The same dashboard in Arabic — the entire layout mirrors, sidebar included.</i></p>

### Reception Portal

| Reception home | Unified calendar |
|:--:|:--:|
| <img src="projectimages/06-reception-home.png" width="430"> | <img src="projectimages/07-reception-calendar.png" width="430"> |
| Member lookup, quick check-in / walk-in / new member / renew | Classes, courses, PT and health sessions on one calendar |

<p align="center"><img src="projectimages/08-reception-subscriptions.png" width="900" alt="Subscription management"></p>

<p align="center"><i>Subscription management — status and expiry-window filters, pending approvals, freeze requests,
one-click renewal.</i></p>

### Cashier Portal

| Sales dashboard | Point of sale |
|:--:|:--:|
| <img src="projectimages/09-cashier-dashboard.png" width="430"> | <img src="projectimages/10-cashier-pos.png" width="430"> |
| Today's revenue, transactions, top products | Category filters, live cart, discount, cash or card |

### Employee Portal — same screen, two form factors

| Web | Mobile |
|:--:|:--:|
| <img src="projectimages/11-employee-dashboard-web.png" width="560"> | <img src="projectimages/12-employee-dashboard-mobile.png" width="240"> |

Shift start/stop (admin-approved), hours this week, salary history and leave balance — one Flutter
codebase, laid out for each screen size.

### Trainer Portal

| No schedule set | Weekly availability set |
|:--:|:--:|
| <img src="projectimages/13-trainer-sessions-empty.png" width="330"> | <img src="projectimages/14-trainer-sessions.png" width="330"> |

Trainers publish daily availability; members book into it, and requests arrive under Pending → Confirmed → History.

### Member App

| Before subscribing | Membership plans | Payment choice |
|:--:|:--:|:--:|
| <img src="projectimages/15-member-home-inactive.png" width="270"> | <img src="projectimages/16-member-plans.png" width="270"> | <img src="projectimages/17-member-payment.png" width="270"> |

A member without an active plan gets a guided landing page; plans show exactly what they unlock, and
checkout offers **Stripe online payment (activates immediately)** or **cash at the gym (waits for admin approval)**.

| Home (active member) | Profile & freeze request | QR member card |
|:--:|:--:|:--:|
| <img src="projectimages/18-member-home-active.png" width="270"> | <img src="projectimages/19-member-profile-freeze.png" width="270"> | <img src="projectimages/20-member-qr.png" width="270"> |

Days-remaining bar, opening hours, check-in streak and today's plan · self-service freeze requests ·
a QR member card that **re-signs itself every 25 seconds**.

| Exercise library | Set logging |
|:--:|:--:|
| <img src="projectimages/21-member-exercises.png" width="330"> | <img src="projectimages/22-member-exercise-log.png" width="330"> |

Filter by muscle group and difficulty, then log reps and weight per set with full previous history.

### Chat

<p align="center"><img src="projectimages/23-chat-arabic.png" width="760" alt="Starting a new conversation, Arabic UI"></p>

<p align="center"><i>Starting a conversation — the contact list is filtered by role eligibility, with live presence dots.</i></p>

---

## Overview

FitZone digitizes the day-to-day operation of a gym end to end: membership sales and renewals,
subscription freezing, class and course scheduling with room-conflict detection, personal-training
bookings, nutrition and InBody health tracking, cafeteria point of sale, staff shifts, payroll and
expenses — plus a real-time chat and notification layer connecting members, trainers and staff.

One Flutter codebase serves **seven role-based portals** (mobile and web) against a single Spring Boot
REST + WebSocket API.

| | |
|---|---|
| Backend source files | ~460 Java classes |
| Frontend source files | ~235 Dart files |
| Database migrations | 77 Flyway versions |
| Roles supported | 8 (Admin, Receptionist, Cashier, Health Expert, Trainer, Employee, Member, Guardian) |
| Languages | English + Arabic (full RTL) |

---

## Tech Stack

**Backend**

| Concern | Technology |
|---|---|
| Framework | Spring Boot 3.5 (Java 21), Spring MVC |
| Persistence | Spring Data JPA / Hibernate, PostgreSQL 17 |
| Migrations | Flyway (V1 → V77) |
| Security | Spring Security, stateless JWT (jjwt), BCrypt |
| Real-time | Spring WebSocket (STOMP + SockJS) |
| Caching | Caffeine (`spring-boot-starter-cache`) |
| Resilience | Resilience4j, Bucket4j rate limiting |
| Payments | Stripe Java SDK |
| Media | Cloudinary + local upload storage |
| Reports / export | Apache POI, OpenPDF |
| Mapping / validation | MapStruct, Jakarta Bean Validation |
| API docs | springdoc-openapi (Swagger UI) |
| Ops | Spring Boot Actuator |

**Frontend**

| Concern | Technology |
|---|---|
| Framework | Flutter 3 (Android, iOS, Web) |
| State management | Provider (`ChangeNotifier`) |
| Routing | go_router |
| HTTP | Dio (JWT + forced-logout interceptors) |
| Real-time | `stomp_dart_client` |
| Charts | fl_chart |
| QR | `qr_flutter` (display) + `mobile_scanner` (scan) |
| Payments | `flutter_stripe` |
| Storage | `flutter_secure_storage`, `shared_preferences` |
| i18n | `flutter_localizations` + custom `context.tr()` translation layer |

---

## Roles & Portals

| Role | Platform | What they can do |
|---|---|---|
| **Admin** | Web + Mobile | Full control — members, staff, subscriptions, classes, courses, rooms, finance, payroll, expenses, system settings, audit log |
| **Receptionist** | Web | Check-ins, walk-ins, member lookup, subscriptions, broadcast announcements |
| **Cashier** | Web | Cafeteria point of sale, sales history, payments |
| **Health Expert** | Web + Mobile | InBody tests, nutrition plans, health sessions, health subscriptions |
| **Trainer** | Mobile | Sessions, assigned classes, workout plans, exercise library, class group chat |
| **Employee** | Web + Mobile | Shift check-in/out (admin-approved), salary records, vacation requests |
| **Member** | Mobile | Subscriptions, class &amp; course booking, workout plans, health tracking, QR check-in, chat |
| **Guardian** | — | Linked to a dependent member's account |

Users holding more than one role get an in-app **role switcher** instead of separate logins.

---

## Features

<details open>
<summary><b>Authentication &amp; security</b></summary>

- Stateless JWT auth with role-based access control across every endpoint
- Self-registration gated by **email OTP verification** (Gmail SMTP, async delivery)
- Forgot password → OTP → reset flow
- Account suspension enforced mid-session — the server pushes a forced logout to the client
- Per-endpoint rate limiting (Bucket4j) and a global exception handler with typed error responses
</details>

<details open>
<summary><b>Members &amp; subscriptions</b></summary>

- Member CRUD with photo upload, student membership type, guardian ↔ dependent linking
- Membership plans with per-member special discounts
- Freeze / unfreeze with unused-day reclaim and policy-driven freeze limits
- Automatic expiry reconciliation (midnight scheduler **and** on application startup)
- Expiry reminder notifications with a configurable lead time
- **Secure QR check-in** — backend-signed 30-second token, scanned at reception/admin
</details>

<details open>
<summary><b>Classes, courses &amp; training</b></summary>

- Gym classes with room, capacity and gender policy (Male / Female / Mixed) enforced server-side
- Recurring **courses** with per-session scheduling, attendance and enrollment approval
- **Cross-entity room conflict detection** — a class and a course session can never share a room/time
- Booking approval workflow, admin force-assign, archive / unarchive / bulk delete
- Personal-training bookings: `PENDING → CONFIRMED → COMPLETED / CANCELLED`
- Member-side schedule conflict checking before booking
- Workout plans and a shared exercise library, plus member-created custom exercises
</details>

<details open>
<summary><b>Health</b></summary>

- InBody test records, body-metric history and weight trends
- Health plans and health sessions assigned by a health expert
- Nutrition recommendations and tracked health issues
</details>

<details open>
<summary><b>Finance &amp; operations</b></summary>

- Revenue dashboard by source (memberships, classes, courses, PT, cafeteria) with charts
- **Expenses and payroll** tracking, with computed net profit
- Online payments via Stripe, with an admin approval step and **automatic refunds** when a paid class, course or health plan is cancelled
- Cafeteria catalog, cart → checkout, sales history
- Employee shifts with admin-approved check-in/out, shift schedules and payroll payments
- Vacation requests with automatic `ON_VACATION` status transitions
- Configurable **system settings**: gym profile, per-day operating hours, check-in policy, cancellation windows, feature toggles
</details>

<details open>
<summary><b>Communication</b></summary>

- Real-time chat over STOMP: member ↔ trainer DMs, admin ↔ staff DMs, and a group chat per class
- Online presence indicators and persisted message history
- Real-time notifications (WebSocket + dispatch scheduler), unread badges, broadcast announcements
</details>

<details open>
<summary><b>Cross-cutting</b></summary>

- **Full Arabic/English localization** with RTL layout support, switchable at runtime
- Audit logging of every administrative action (actor, entity, action, timestamp) with filters
- Pull-to-refresh across member/trainer screens; responsive dialogs down to phone widths
- Swagger/OpenAPI documentation for the entire API surface
</details>

---

## Architecture

```
              ┌──────────────────────────────────────────┐
              │       Flutter client (one codebase)      │
              │   Web portals · Android/iOS member app   │
              │   Provider · go_router · Dio · STOMP     │
              └──────────────┬───────────────────────────┘
                REST + JWT   │   WebSocket (STOMP)
              ┌──────────────┴───────────────────────────┐
              │            Spring Boot 3 API             │
              │   Controllers → Services → Repositories  │
              │   Security · Caffeine cache · Schedulers │
              └──────────────┬───────────────────────────┘
                             │  JPA / Hibernate
              ┌──────────────┴───────────────────────────┐
              │    PostgreSQL (schema `gym`) · Flyway    │
              └──────────────────────────────────────────┘
```

```
.
├── Springboot/                     # Spring Boot backend
│   ├── src/main/java/com/example/springboot/
│   │   ├── controllers/            # REST endpoints
│   │   ├── services/ + impl/       # business logic
│   │   ├── repositories/           # Spring Data JPA
│   │   ├── models/                 # JPA entities
│   │   ├── dtos/request|response/  # API contracts
│   │   ├── security/               # JWT filter, user details, role config
│   │   ├── config/                 # cache, websocket, async, OpenAPI
│   │   └── shared/                 # enums, exceptions, schedulers
│   └── src/main/resources/db/migration/    # 77 Flyway migrations
│
├── for_springboot/                 # Flutter frontend
│   └── lib/
│       ├── core/                   # network, router, theme, i18n, widgets
│       └── features/
│           ├── auth/ admin/ member/ trainer/ employee/
│           ├── reception/ cashier/ health_expert/
│           └── shared/             # chat, notifications, classes, courses
│
├── dbsql/                          # realistic demo dataset (run_all.sql)
└── projectimages/                  # screenshots used in this README
```

Every feature folder follows the same shape — `screens/`, `providers/`, `services/`, `models/` — so
adding a feature touches one predictable set of files on each side of the stack.

---

## Engineering Highlights

A few problems that were interesting to solve:

**Cross-entity room booking conflicts.** Classes store time as `OffsetDateTime`; course sessions store
`LocalDate` + `LocalTime`. Booking either one checks *both* tables, converting between the two
representations, so a room can never be double-booked from two different features.

**N+1 elimination on the hot path.** `GET /api/classes` originally issued ~8 extra queries per class
(chat conversation id, pending booking count, trainer-busy checks, lazy associations). It was rewritten
to batch every lookup — `JOIN FETCH` for trainer and room, a grouped count query for bookings, one
conversation-id lookup covering all classes, and one busy-check per distinct trainer.

**Caching that actually runs.** The cache annotations were silently inert until `@EnableCaching` was
added; Caffeine now backs the expensive admin aggregates.

**Mid-session enforcement.** Suspending a member takes effect immediately: the JWT filter rejects the
locked account and the Dio client's `onForcedLogout` hook drops the user back to the login screen.

**Reconciliation that survives downtime.** Subscription expiry runs on a midnight cron *and* at
application startup, so statuses stay correct even if the server was down when a subscription lapsed.

---

## Load Testing

The backend was load tested with **[Grafana k6](https://k6.io/)**, deliberately targeting the three most
expensive read endpoints in the system rather than light browse traffic — the goal was to prove the API
stays *correct*, not just fast, under heavy concurrency.

The three endpoints were not picked at random. A query audit of the codebase had already flagged them as
the worst read paths in the system, so the test was built to answer the follow-up question: **at 1,000
concurrent users, what do those known costs actually do — and does anything break?**

### Endpoints under test

| Endpoint | Why it is expensive |
|---|---|
| `GET /api/admin/finance` | ~18 uncached aggregate queries per call (6 payment tables × 2 time windows + 6 trend queries) |
| `GET /api/admin/stats` | 90+ queries on a cache miss; one shared cache key also exercises cache-stampede behaviour |
| `GET /api/classes` | Known N+1 pattern — roughly 8 extra queries for every class in the response |

Realistic data volume was seeded first (40–90 rows per activity table across subscriptions, payments,
check-ins, bookings, health records, cafeteria sales, chat and courses) so the aggregates had real work
to do.

### Load profile

A 13-stage ramp over 11 minutes: **50 → 150 → 250 → 500 → 750 → 1,000** concurrent virtual users, each
stage ramped and then held. Every VU logged in as an admin, then looped over the three heavy endpoints
with a short randomized pause between calls.

### Results

| Metric | Value |
|---|---|
| Peak concurrent virtual users | **1,000** |
| Total HTTP requests | 96,949 |
| Completed iterations | 31,972 |
| **HTTP failure rate** | **0.00%** (0 of 96,949) |
| **Checks passed** | **96,949 / 96,949 (100%)** |
| Throughput | 146.2 req/s |
| Median latency | 1.84 s |
| p90 / p95 latency | 4.54 s / 4.87 s |
| Min / max latency | 4 ms / 6.41 s |
| Data received / sent | 876 MB / 27 MB |
| Test duration | 11m 03s |

### Findings

- **Zero failed requests and zero failed checks** across all six load stages. Under saturation the
  backend queued and eventually served every request rather than rejecting or erroring on any of them.
- The only 12 interrupted iterations happened during graceful ramp-down as VUs were torn down; the
  three matching server-side log entries are client-disconnect exceptions, not application faults.
- Latency growth tracked the documented per-request query cost of these specific endpoints rather than
  any instability — light browse endpoints were an order of magnitude cheaper.
- Backend, database and the k6 generator all shared one machine, so these figures measure **query and
  application cost**, not a production capacity ceiling: the load generator competed for the same CPU.

### Fixed after the test

The audit named the problems, the load test priced them at scale, and two of the four were then fixed:

| Finding | Status |
|---|---|
| `GET /api/classes` N+1 (~8 queries per class: conversation id, booking count, trainer-busy, lazy loads) | ✅ Fixed — every per-class lookup batched into a fixed number of queries |
| Cache annotations inert (`@EnableCaching` missing) | ✅ Fixed — Caffeine caching now live |
| `/api/admin/stats` cache stampede on a single shared key | ⏳ Identified — recompute guard proposed |
| `/api/admin/finance` fully uncached (18 aggregates per call) | ⏳ Identified — short-lived cache proposed |

---

## Validation Rules

| Field | Rule |
|---|---|
| National ID | Exactly 9 digits, numbers only |
| Phone | Exactly 10 digits, numbers only |
| Email | Valid format; OTP-verified on self-registration |
| Password | Minimum 6 characters |
| Birth date | At least 10 years before today |
| Class / course capacity | Cannot exceed the assigned room's max capacity |

---

## Team

| Name | Role |
|---|---|
| **Muhammad Aljamal** | Team Leader — Full-Stack |
| **Ahmad Omaryeh** | Frontend |
| **Ahmad Abu Najeeb** | Frontend |

---

## License

Developed as a university graduation project and published for portfolio / academic purposes.
