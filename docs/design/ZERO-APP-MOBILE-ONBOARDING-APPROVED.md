# Zero App — Approved Mobile Onboarding Screens (v1)

**Approval:** Founder-approved visual design on 2026-11-02 (11-screen journey).  
**Status:** Approved UI/UX reference for development. This document captures the approved design and screen behavior.  
**Visual reference:** the final 11-screen concept board shared in the project conversation, titled `zero_app_volunteer_journey_board.png`. The artwork is **not yet committed as a binary asset**; add it under `docs/design/zero-app-volunteer-journey-board.png` before design implementation.

## Approved screen order

| # | Screen | Required behavior |
|---|---|---|
| 1 | Splash — Zero App | Fast brand entry, Get Started. |
| 2 | Onboarding — Vision | Introduce volunteering for Sri Lanka. Skippable. |
| 3 | Onboarding — Benefits | Discover projects, track contribution, join a community. Skippable. |
| 4 | Onboarding — SDGs | Introduce all 17 goals, with examples. Skippable. |
| 5 | Entry Options | **Explore Projects** (without account) or **Create Account**. |
| 6 | Account Setup | Minimal name and credential/contact setup; supported social sign-in only when configured. |
| 7 | Tell us about yourself | I study / I work / I volunteer independently. These are **matching hints, not different volunteer account types**. |
| 8 | Institution & Club | Search established institution/partner where relevant; select district; automatically suggest a matching existing club. University applicants enter **student registration number as text only**; **no ID-card photo/upload**. |
| 9 | SDG Interests | Select any of 17 SDGs; personalisation only, not project eligibility. Optional/changeable. |
| 10 | Registration Complete | Show club connection; do not falsely say a membership was verified if only suggested or self-declared. |
| 11 | Home — Discover Projects | Prominent search, location/date/time/SDG/type filters, nearby and national projects, bottom navigation. |

## Confirmed club rule

- Every registered volunteer belongs to a club. A known university/institute/school club is the first matching choice for students; a relevant partner club can be used where established; otherwise connect to a district community club.
- **Do not require university club officer approval or student identity document upload during MVP registration.** Students provide university registration number as text (protected private data) for possible future verification.
- Add a future **club membership verification** module, driven by authorised club officers, with bulk review and fraud/duplication controls. It must not block the current onboarding MVP.
- Treat institution/registration-number claims as **self-declared** until verified. Never display a verified badge based on registration number alone.
- Community volunteers (including elders, graduates, self-employed and people outside institutions) are welcome.
- Corporate employees are normal volunteers; corporate partner organisations associate with projects, without codes or paid accounts.

## Projects and attendance — constraints carried into screen development

- Projects belong to an organising club, and may list partner organisations.
- **In-person:** administrator picks geocoordinates and configurable radius (default **1 kilometre**). Attendance/check-in **and check-out are GPS-based only**, not QR scans. Verify distance server-side, with timestamp and location accuracy; allow audited authorised exceptions for GPS errors.
- **Virtual:** no project physical location/geofence. Only verified organising committee contribution can earn formal volunteer marks; ordinary virtual attendees **do not** earn volunteer marks. Kindness Gems may be earned under their separate quest approval rules.
- Filter projects by geography, date/time, SDG, project type and club. Public browsing does not require account; participation/check-in does.
- Keep volunteer marks/ranks distinct from **Kindness Gems** and use the shared 500-verified-mark threshold for digital self-service certificates.

## Visual and interaction guardrails

- Product name: **Zero App**. Nature-inspired, clean teal/green + light UI, generous spacing, rounded cards, approachable university-age photography; avoid clutter.
- The approved imagery is a **concept board**, not pixel-perfect production assets or real project data.
- No QR attendance feature on the home bottom navigation (the initial concept shows a generic check-in icon; the actual feature must open GPS check-in).
- A featured project, its date, named student, university and numerical samples in the mockup are **placeholders**, not seed data.
- Need proper empty/loading/error states, accessible labels and responsive layouts on each implementation screen.

## Future UI implementation method

Translate each numbered screen into a mobile component with state/route specifications. Before coding, review its states (browse as guest, authenticated, no nearby project, institution not found, club suggestion pending, failed GPS, successful attendance). Maintain this approved screen order unless the founder approves changes.

## Scope and repository status

This file stores the approved **requirements and screen inventory**. The PNG reference itself is available as a chat artifact and must be separately uploaded to GitHub; do not claim it is present in the repo until independently verified.
