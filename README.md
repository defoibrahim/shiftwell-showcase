<div align="center">

# Shiftwell

**Multi-tenant HR platform for UAE and GCC shift teams: Core HR, self-serve signup with live AED pricing, a two-factor operator console, and an offline-first Flutter time-and-attendance app.**

![Status](https://img.shields.io/badge/status-live-2ea44f?style=flat-square)
![Platform](https://img.shields.io/badge/platform-web%20%C2%B7%20mobile%20web%20%C2%B7%20Flutter%20app-3A5BD9?style=flat-square)
![NestJS](https://img.shields.io/badge/API-NestJS%2010-E0234E?style=flat-square&logo=nestjs&logoColor=white)
![Next.js](https://img.shields.io/badge/web-Next.js%2014-000000?style=flat-square&logo=nextdotjs&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL%2016-row--level%20security-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Prisma](https://img.shields.io/badge/ORM-Prisma-2D3748?style=flat-square&logo=prisma&logoColor=white)
![Flutter](https://img.shields.io/badge/mobile-Flutter%203.41%20%C2%B7%20GetX-02569B?style=flat-square&logo=flutter&logoColor=white)

[Live site](https://shiftwell.everbuild.cloud) · [Console sign-in](https://app.shiftwell.everbuild.cloud) · [Portfolio](https://portfolio.everbuild.cloud) · [LinkedIn](https://linkedin.com/in/dafallah-ibrahim)

</div>

> [!NOTE]
> This is a showcase repository. The source code is private because Shiftwell is company work, built at **Sudan Tech Solutions (STS)** and shared here with permission. This repo covers what the product does, how it is built and what I did on it. I'm happy to walk through the code in an interview.

<p align="center">
  <img src="docs/screenshots/landing-hero.png" width="100%" alt="Shiftwell landing page hero: 'Everyone knows where they're working.' with a phone mock-up showing a geofenced clock-in button and a list of today's shift statuses">
</p>

<p align="center">
  <img src="docs/screenshots/mobile-landing-hero.png" width="30%" alt="Landing page hero on a phone, 390 px wide">
  &nbsp;
  <img src="docs/screenshots/mobile-pricing.png" width="30%" alt="Pricing section on a phone: Core HR base plan at AED 8 per employee per month">
  &nbsp;
  <img src="docs/screenshots/mobile-employee-portal-sign-in.png" width="30%" alt="Employee payslip portal sign-in on a phone: company code, employee code and PIN">
</p>

<p align="center">
  <img src="docs/screenshots/landing-pricing-aed.png" width="100%" alt="Pricing section: Core HR at AED 8 per employee per month, plus Attendance, Leave, Payroll and Shifts add-ons, with 'Needs Leave' and 'Needs Attendance' dependency badges">
</p>

<p align="center">
  <img src="docs/screenshots/checkout-plan-builder.png" width="100%" alt="Self-serve checkout: Payroll and Shifts are selected, Leave and Attendance were added automatically as their dependencies, and the order summary shows AED 1,250.00 per month for 50 employees">
</p>

<p align="center">
  <img src="docs/screenshots/console-sign-in.png" width="49%" alt="Admin console sign-in with company code, work email and password">
  <img src="docs/screenshots/operator-sign-in-2fa.png" width="49%" alt="Operator console sign-in on its own host, with email, password and a six-digit authenticator code">
</p>

<p align="center">
  <img src="docs/screenshots/landing-modules.png" width="100%" alt="Landing page feature grid: shift scheduling, roster and swaps, clock in and out, timesheets, payroll, leave and people">
</p>

<p align="center">
  <img src="docs/screenshots/landing-mobile-app.webp" width="100%" alt="Dark landing section about the attendance app, with a fanned stack of phone mock-ups for roster, leave, payslip and clock-in screens">
</p>

<p align="center">
  <img src="docs/screenshots/landing-admin-tour.png" width="49%" alt="Interactive admin tour on the landing page: the shift templates screen">
  <img src="docs/screenshots/landing-admin-tour-payroll.png" width="49%" alt="Interactive admin tour on the landing page: the pay run screen with sample employees">
</p>

<sub>The screenshots above are real captures of the live sites (the mobile app has its own section below). The phone stack and the admin tour are interactive mock-ups on the marketing page, filled with illustrative sample data shipped with the marketing site (URLs shown inside the mock-ups are illustrative too). They show the planned modules and are not captures of the signed-in console. No real company or employee data appears anywhere.</sub>

## Features

**Core HR, in the admin console**
- Employee records with sites, departments and positions. Employees are never hard-deleted: they are marked terminated, so their history is kept.
- Salary structures versioned by date, so a raise closes one version and opens the next and past figures can be reproduced.
- Identity documents with uploaded scans, and a dashboard card for documents that are about to expire.
- Bank details whose form comes from the employee's country (IBAN in the UAE, account number and IFSC in India). Only the owner and finance roles can see or edit them.
- Employee import and export by CSV.
- Console users with the roles owner, HR admin, finance and manager. Managers see only their own departments. Colleagues join by emailed invitation, and there is self-service password reset.

**Employee portal**
- A phone-first sign-in with company code, employee code and PIN. HR issues the first PIN, and the employee must change it before seeing anything else.
- Revoking access or terminating an employee closes any open session immediately.

**Self-serve signup and billing**
- The landing page reads plans, add-ons and prices live from the API, priced in AED per employee per month.
- At checkout the buyer chooses modules and head-count. Dependencies are added automatically (Shifts needs Attendance, Payroll needs Leave), and the server prices the order again rather than trusting the page.
- The company is created with exactly the modules that were bought, and the owner gets an email link to set a password.
- Entitlements gate features, navigation and limits (employees, sites, console users). A cancelled company is locked out of everything except sign-in and the renewal screen.

**Operator console** (a separate host)
- Sign-in needs a password and a TOTP authenticator code, and each code works only once. Operators are invited from the CLI with a one-time setup link and a QR code.
- Operators manage the plan and add-on catalogue and turn the payments requirement on or off. If a price changes after a company has bought it, the change is saved as a new version (`core_v1` → `core_v2`), so existing customers keep the terms they bought.

**Planned modules.** The landing page sells Attendance, Shifts, Leave and Payroll (with the UAE WPS salary file). In the API they exist as isolated module scaffolds behind the entitlement system. Work on the leave schema had started by the last commit. Payroll is designed but not yet implemented (see [docs/architecture.md](docs/architecture.md)).

## Mobile app (Flutter)

**Workforce Mobile** is the employee-facing app: an offline-first time-and-attendance client for shift workers. It is a separate private repository, written in Flutter.

<p align="center">
  <img src="docs/screenshots/mobile-app/cover-trio.png" width="100%" alt="Three framed phones: the on-shift punch screen with its progress ring in the centre, the punch history with Missed, Late and overtime flags on the left, and the green offline punch-in confirmation on the right">
</p>

<p align="center">
  <img src="docs/screenshots/mobile-app/framed/01-login.png" width="31%" alt="Sign-in screen in a phone frame: empty Employee ID and PIN fields, branded with Falcon Facilities and Marina Site B">
  <img src="docs/screenshots/mobile-app/framed/02-punch-ready.png" width="31%" alt="Punch tab before the shift: 'Inside worksite, Marina Site B, 40 m radius', a large 'Hold to punch in' button and today's 06:00 to 14:00 shift card">
  <img src="docs/screenshots/mobile-app/framed/03-punch-hold-to-confirm.png" width="31%" alt="Hold-to-punch: a progress ring fills around the button while it reads 'Keep holding…'">
</p>
<p align="center">
  <img src="docs/screenshots/mobile-app/framed/04-punched-in-confirmation.png" width="31%" alt="Full-screen green 'Punched in' confirmation: 05:57 at Marina Site B, recorded on this phone, syncs when there is signal">
  <img src="docs/screenshots/mobile-app/framed/05-on-shift-progress.png" width="31%" alt="On shift: 'Punch out' button with a green shift-progress ring and elapsed timer, worked 2h 44m of 8h scheduled, and recent activity">
  <img src="docs/screenshots/mobile-app/framed/06-punch-history.png" width="31%" alt="Punch history by day, with Missed, Late 14m, OT 2h 33m and rest-day rows">
</p>
<p align="center">
  <img src="docs/screenshots/mobile-app/framed/07-profile.png" width="31%" alt="'Me' tab: identity header, account rows, notifications, location permission, theme, language, security and help">
  <img src="docs/screenshots/mobile-app/framed/08-profile-settings.png" width="31%" alt="Profile scrolled down to sign out and the app version">
  <img src="docs/screenshots/mobile-app/framed/12-profile-dark-theme.png" width="31%" alt="Profile screen in the dark theme">
</p>
<p align="center">
  <img src="docs/screenshots/mobile-app/framed/11-punch-arabic-rtl.png" width="31%" alt="Punch screen in Arabic, on shift, with Arabic-Indic numerals">
  <img src="docs/screenshots/mobile-app/framed/10-profile-arabic-rtl.png" width="31%" alt="Profile screen in Arabic">
</p>

<sub>These screens are rendered from the Flutter codebase in a phone viewport (390 × 844 at 3×). The app was built for Flutter web on GitHub Actions with its mock data source and driven by headless Chrome. They are not native Android or iOS captures. Each capture is shown inside a drawn phone frame; the 9:41 status bar, Dynamic Island and home indicator belong to the frame, not to the app. The employee, site, supervisor and company shown ("Rashid Alam", EMP-40921, Falcon Facilities, Marina Site B) are the app's own fictional fixtures. The clock was fixed at 05:57 and 08:42 Dubai time to show the before-shift and on-shift states.</sub>

**Store-style previews**

<p align="center">
  <img src="docs/screenshots/mobile-app/card-1-punch.png" width="31%" alt="Store-style preview card: 'Hold to punch in, even offline', geofence check first, then it syncs when there is signal, over the hold-to-punch screen">
  <img src="docs/screenshots/mobile-app/card-2-history.png" width="31%" alt="Store-style preview card: 'Late, missed and overtime at a glance', every day's punches with clear flags, over the punch history screen">
</p>

**Features**
- Sign-in with Employee ID and PIN, optional biometric unlock (`local_auth`), and handling for account lockout and expired sessions. Punches keep queuing and syncing even if the session expires.
- Hold-to-punch with a geofence check against the assigned site. It tells apart inside the zone, outside the zone, weak GPS, location turned off and no assigned site.
- An offline punch queue. Each punch is synced, pending, rejected or not sent, and carries a ULID idempotency key from a hand-written generator, so a retried punch cannot be recorded twice.
- A live shift-progress ring with an elapsed timer, plus today's worked, scheduled and break totals.
- Punch history built from daily records, with late, missed punch, overtime, absent, out-of-zone and left-early flags.
- A profile hub with light and dark themes, English and Arabic with full right-to-left layout, location-permission status, notifications, security and an HR help sheet.
- Five tabs: Punch, Roster, Leave, Pay and Me. Roster, Leave and Pay, along with the day-detail, correction, payslip, notification, personal-info and employment screens, are placeholders for now.

**Stack**
- Flutter 3.41 and Dart 3.11. **GetX** handles routing, bindings, route middleware and reactive state.
- get_it + injectable for dependency injection, with the mock or remote data source chosen at build time (`--dart-define=DATA_SOURCE`).
- Clean architecture per feature (domain, data and presentation layers), with dartz `Either` results instead of thrown errors.
- dio behind a single API-client wrapper (only sign-in uses it so far), plus flutter_secure_storage, shared_preferences, permission_handler, geolocator and connectivity_plus.
- flutter_screenutil, skeletonizer, pull_to_refresh, google_nav_bar, Lucide icons, a bundled Manrope font, and gen_l10n for English and Arabic.
- About 21k lines of Dart in `lib/`, 155 test files and about 450 golden images. The app's CI checks that generated code is current, then runs the formatter, `flutter analyze --fatal-infos` and the tests. Design is documented in a spec, a brief and ADRs.

**How it connects to the backend.** Not yet, and on purpose. The app's spec defers the API contract to a later "remote contract" document, and the domain layer stays transport-neutral: no entity, use case or repository names an HTTP concept. Each repository has an in-memory fixture implementation today, covering the profile, a fixed site with a 40 m geofence, a 06:00 to 14:00 shift, punch history and a fake location service. Remote implementations will be registered once the contract with the Shiftwell API exists, and they will be selected at build time without touching the domain or UI. The one network call today is sign-in, which goes to the public dummyjson.com test service as a stand-in. Background sync (workmanager) and a local SQLite store (drift) are declared for that phase but not used yet. The visual design follows the Shiftwell brand tokens from the landing site.

## Architecture

```mermaid
flowchart TB
  subgraph Public
    L["Landing site<br/>Next.js 14 · Bun<br/>shiftwell.everbuild.cloud"]
  end
  subgraph Phone["Employee phone"]
    MA["Workforce Mobile<br/>Flutter · GetX<br/>offline punch queue"]
  end
  subgraph App["app.shiftwell.everbuild.cloud"]
    N1["nginx · TLS"]
    W["Next.js web<br/>admin console + /portal"]
    A["NestJS API<br/>modular monolith"]
  end
  subgraph Ops["Operator host"]
    N2["nginx · TLS<br/>/ops and operator API only"]
  end
  DB[("PostgreSQL 16<br/>row-level security<br/>+ pg-boss jobs")]
  RP["Country rule packs<br/>ae.json · in.json · sd.json"]
  M["Mailtrap<br/>invites · resets"]
  GH["GitHub Actions<br/>lint · typecheck · unit + e2e · images"]
  DJ["dummyjson.com<br/>sign-in stand-in"]

  L -- "GET catalogue / POST checkout (CORS: /api/signup only)" --> A
  N1 --> W
  N1 --> A
  N2 --> W
  N2 --> A
  W -- "typed client generated from OpenAPI" --> A
  A -- "app_user role, SET LOCAL app.tenant_id" --> DB
  A --> RP
  A --> M
  GH -- "SSH: deploy.sh, health check, rollback" --> App
  MA -. "planned: remote data sources once the API contract is defined" .-> N1
  MA -- "sign-in only, for now" --> DJ
```

The dotted arrow is planned work. The mobile app runs on in-memory fixtures today and is not yet connected to the API (see [Mobile app](#mobile-app-flutter)).

The API is one deployable with walls inside it. `core/` holds tenancy, identity, org, billing, signup and operator. `modules/` holds attendance, shifts, leave and payroll, kept as islands. `bridges/` is the only place two modules meet. Lint enforces these boundaries, so a cross-module import fails the build. There's more depth in [docs/architecture.md](docs/architecture.md).

### Key engineering decisions

- **Tenant isolation in three layers.** Every tenant table has `tenantId` as the first column of every index. PostgreSQL row-level security is turned on and forced on every tenant table. Each request runs in a transaction that sets the tenant with `SET LOCAL`, never plain `SET`, so a pooled connection cannot carry one company's data into another company's request. The API connects as a role that owns nothing. Foreign keys are composite on tenant and id, because Postgres runs its integrity checks with RLS bypassed. A dedicated e2e suite proves the isolation, and it refuses to run as a role that could bypass RLS.
- **Auth that fails closed.** The global guards run in a fixed order: JWT, then audience, then roles, then subscription. After them, an interceptor (not middleware) sets the tenant from the verified token. A route without a declared audience is rejected. Refresh tokens rotate within a family, and replaying a used one revokes the whole family. Access is re-read from the database on every request. Sign-in errors never reveal whether an account exists, and per-account lockout works alongside per-IP rate limits.
- **Money is integer fils.** No floats or decimal strings. The money helpers split amounts so the parts always add up exactly to the total, because a one-fil discrepancy gets a WPS bank file rejected.
- **Country rules are data.** Identity document types, bank-account fields and formats, and salary-component tags come from per-country JSON rule packs. Supporting a new country means adding a file, not a schema change or an `if (country === 'AE')`.
- **The OpenAPI document is the contract.** The console's API types are generated from it, and CI fails if they are stale. An e2e test fails on any route without a summary, a response schema or correct security, and on any money field that isn't an integer.
- **Deploys are automatic and safe to repeat.** Every push to `main` runs lint, type checks, unit tests, e2e tests against PostgreSQL 16 and the production image builds, then deploys over SSH. The deploy script sets the server up on its first run, and later runs check health and roll back an unhealthy release. Because a rollback reverts code but not the schema, migrations are additive only.

## Tech stack

| Layer | Tech |
| --- | --- |
| API | NestJS 10, TypeScript, Prisma (pg adapter), class-validator, zod, Swagger/OpenAPI |
| Database | PostgreSQL 16 with hand-written RLS policies, exclusion constraints (`btree_gist`) and triggers |
| Background jobs | pg-boss, which runs inside Postgres (no Redis) |
| Auth | JWT access tokens + rotating refresh tokens in httpOnly cookies, argon2, TOTP for operators |
| Admin console and portal | Next.js 14 (App Router), React 18, Tailwind CSS, Radix UI primitives |
| Landing and checkout | Next.js 14, Tailwind, shadcn/ui, Bun, schema.org structured data |
| Mobile app | Flutter 3.41 / Dart 3.11, GetX (routing, DI bindings, state), get_it + injectable, dartz, dio, geolocator, permission_handler, local_auth, flutter_secure_storage, gen_l10n (English + Arabic RTL) |
| Mail | Mailtrap (invitations, password resets, operator setup) |
| Infra | Docker Compose, nginx + certbot, Caddy for local HTTPS, GitHub Actions CI/CD to a VPS |
| Quality | ESLint with architecture-boundary rules, Jest unit tests and e2e suites, generated API types checked in CI |

## My role

I worked on Shiftwell at **Sudan Tech Solutions (STS)**. I built the landing site, checkout and Flutter mobile app on my own, and led the API work after the initial import:

- **Flutter mobile app (Workforce Mobile):** I'm the only author. I wrote all 38 commits in its repository (12 August to 16 September 2026), covering the architecture, the design system, punch with geofencing, the offline queue, attendance history, auth and the profile hub, plus the tests, golden images, CI and the spec and ADR docs.
- **Landing site and checkout:** I wrote all 17 commits in its repository (September to October 2026), covering the design build, SEO and structured data, the contact form, live catalogue pricing, self-serve checkout and the VPS deploy pipeline.
- **API and console:** the organisation's repository starts with a single import commit of the existing codebase, made by a teammate. Of the 26 commits after it, I wrote 25 (17 changes plus 8 merges, 29 Sep to 1 Oct 2026). They add self-serve signup, the operator console with two-factor sign-in and emailed invitations, the free plan, landing checkout support, automatic deploys to the VPS, and a series of security and correctness fixes. A teammate added the first leave-module schema on 8 Oct 2026, and the leave module is being built by a teammate on a separate branch.
- I wrote the AI/engineering guide (AGENTS.md) and the deploy runbook, and kept the architecture guide and roadmap in line with the code as features landed.

## Status

Live. The landing site went up in September 2026, and the app has been deployed automatically since 30 September 2026:
- Landing site: [shiftwell.everbuild.cloud](https://shiftwell.everbuild.cloud)
- Admin console and employee portal: [app.shiftwell.everbuild.cloud](https://app.shiftwell.everbuild.cloud), where you sign in with a company code
- Operator console: on its own TOTP-protected host, for platform staff only

Core HR, the employee portal, billing, signup and the operator console are in production. The Flutter mobile app is in development: the punch, history, auth and profile flows work on mock data, and connecting them to the API is the next step. Attendance, shifts, leave and payroll are next on the roadmap. The checkout currently runs with payments switched off, so a company is created at signup, until a real payment provider is integrated.

---

<sub>© Sudan Tech Solutions (STS). Shared with permission. Source code is private.</sub>
