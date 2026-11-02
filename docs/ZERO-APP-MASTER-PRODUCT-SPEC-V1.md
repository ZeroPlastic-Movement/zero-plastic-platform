# ZERO APP — Master Product Requirements and UI/UX Blueprint
Version: 1.0 | Date: 2026-11-02
Status: **Consolidated design baseline for founder review**; requirements listed as CONFIRMED come from project decisions, others labelled DESIGN DEFAULT and should not be presented as approved business rules.
References: ZERO_PLASTIC_MODULAR_DEV_ROADMAP.md, ZERO_PLASTIC_EXECUTIVE_SUMMARY.md, backend/prisma/schema.prisma, docs/design/ZERO-APP-MOBILE-ONBOARDING-APPROVED.md, docs/VOLUNTEER-ENGAGEMENT-GEMS-CERTIFICATES-V2.md.

## 1. Product purpose and identity — CONFIRMED
- **Zero App** is one free national volunteering app. ZeroPlastic Movement and Volunteers Academy contribute projects; partners including corporations are linked to projects. No corporate subscriptions, client tenants, separate corporate applications or corporate access codes.
- Every user is a **volunteer** with one lifelong account. The app is for discovering, joining and attending projects, collecting verified marks and Kindness Gems, tracking impact, and downloading certificates.
- The **web management platform** owns all operational work: clubs, projects, district approvals, member management, evidence exceptions, Gem quest definitions and approvals, reports and settings.
- Projects and verified outcomes are tagged to relevant Sustainable Development Goals (SDGs); do not present platform contributions as official national UN indicator statistics.
- Formal volunteer marks/ranks (Hero, Superhero, Legendary, Titan among existing tiers) are separate from **Kindness Quest**, where approved positive deeds earn **Kindness Gems**. Do not overwrite actual deployed rank thresholds with speculative examples.
- `Kindness Quest` is **final game name**; `Kindness Gems` is its reward unit. `Kindness League` is an optional FUTURE tournament name, not confirmed.

## 2. Organisational structure — CONFIRMED except noted
- National directors oversee all districts, projects and chapter performance. District directors oversee their assigned districts and approve chapter projects. Chapter executive committees manage their own chapter's volunteers and projects and approve their members' Kindness Quest claims.
- Chapter types may include university, school, higher education institute, postgraduate institute, corporate-linked chapter (if established), and district/community club. Partners linked to projects need not be chapters.
- Every registered volunteer has a primary club connection. **DESIGN DEFAULT:** fall back to their district community club if no institutional club exists; distinguish auto-assigned vs verified membership.
- Registration should not require officer approval or student ID-card photo upload in MVP. For a student, collect **university registration number as text** for future verification; keep confidential and do not badge it verified automatically.
- Membership category is just a matching hint (study, work, independently volunteer), not a user class restricting participation. School-age people need appropriate age safeguarding.

## 3. Mobile onboarding and discovery — APPROVED 11-screen reference
1. Splash — Zero App; 2. Skippable vision; 3. Skippable benefits; 4. Skippable SDG introduction; 5. Explore as guest / Create account; 6. Account basics; 7. 'Tell us about yourself' (study/work/independent); 8. Institution, district, student number if applicable and suggested club; 9. Optional SDG interests; 10. Success with honest membership status; 11. Project-first Home.
- Guest visitors browse published eligible projects; login is required for joining/checking in.
- Project discovery across Sri Lanka: text search; district/location/radius, date/time, SDG, in-person/virtual, organising club and nearest/soonest sorting. Location permission is requested when needed (near me or check-in), not at installation.
- Separate **Explore Projects** (all available public projects) from **My Volunteering** (Upcoming registrations, Ongoing activities, Past participation/verified records). Show cancelled projects to affected registrants as cancelled.
- Mobile navigation DESIGN DEFAULT: Home, Explore, My Projects, Kindness Quest, Profile. Certificates and impact inside Profile; GPS check-in opens from relevant project, not QR.

## 4. Project management: web-owned — CONFIRMED
- A permitted chapter executive member creates a project belonging to their chapter; chapter association is server-derived from permissions, not arbitrary selection.
- Project form: title, description, programme owner (ZeroPlastic/Volunteers Academy where relevant), type (in-person/virtual), date/time/time zone, capacity, organising committee, category, SDGs, partners, estimated budget, location and allowed geofence radius for in-person.
- **In-person:** required map pin/latitude/longitude; default geofence **1 km**, configurable per project. Volunteer check-in **and** check-out must rely on device GPS and server-side radius verification, never QR codes. Record location accuracy and timestamps; optionally flag suspicious spoofing/impossible travel; exceptions only through audited authorised web action.
- **Virtual:** NO physical location, NO GPS attendance. Ordinary online participants get **zero formal volunteer marks**, even if they join a meeting. Verified organising committee members may receive marks under established committee rules. Separate Kindness Quest claims may be allowed and locally reviewed.
- **Approval hierarchy:** chapter creates/sends → district director Approves / Rejects (with reason) / Requests changes. A chapter never approves its own project. District director sees all assigned chapters; national directors have nationwide visibility and a controlled override facility (DESIGN DEFAULT).
- **States:** DRAFT, PENDING_APPROVAL, CHANGES_REQUESTED, REJECTED, APPROVED, CANCELLED, COMPLETED. Upcoming/Ongoing/Past are derived time-based user-facing views, not substitute approval states. Approved projects can be published and joined; changes to date/location/type/geofence/capacity after approval require reapproval (DESIGN DEFAULT); avoid destructive deletion of participation history.
- Chapter can edit drafts and returned projects, request cancellation with a reason; authorised chapter may cancel a future own approved project with notification and audit, with escalation to district for serious exceptions (DESIGN DEFAULT). Rejected project can be revised and resubmitted.
- **Completion Phase 1:** chapter clicks Mark Completed → web opens EXISTING Monday.com completion form → officer confirms they submitted it → record confirmation actor/timestamp and mark `COMPLETED_REPORT_SELF_DECLARED` (or status COMPLETED + reportVerificationStatus). Do not claim Monday submission was technically verified. Store a configurable **actual** form URL when provided; never invent it. Report collects 5 photos, participant count, finance/budget, waste weight for cleanups, awareness attendance where relevant, SDGs, narrative/outcomes and social caption. Actual integration/validation later phase.
- Record verified GPS participation independently from self-declared completion report, preventing duplicate counting and false marks.

## 5. Volunteer marks, profiles, safety and certificates — CONFIRMED
- Formal volunteer marks are based on eligible verified in-person attendance and applicable verified organiser contributions for virtual projects. Actual existing awards/rank thresholds must be migrated/validated, not guessed.
- Each club officer can view chapter volunteers, non-sensitive profiles and authorised activity history. District directors view assigned chapters/district, national directors nationwide, subject to field-level private data controls.
- **Suspend chapter membership** is distinct from **ban/suspend whole app account**; serious account bans require elevated authority, reason, appeal and audit. Chapter restrictions must not erase existing certificates or historic legitimate participation.
- Volunteer profile exposes personal marks/rank, projects, hours, verified SDG contributions, Kindness Gems, certificate access; private IDs and sensitive evidence not visible to unrelated users.
- At **500 verified formal volunteer marks** a volunteer can generate/re-download their **PDF e-certificate automatically**, with name, first and last verified attended project dates, count of unique verified attended projects, issue date, serial and verification QR/URL. No printing and no routine manual approval. Gems do not count. Reissues snapshot changes; web handles exceptions/revocations.

## 6. Kindness Quest — CONFIRMED core, DESIGN DEFAULT details
- National directors have **Kindness Quest Management** section in the web admin: create/edit/publish/pause/retire configurable quest definitions, Gem pool, evidence, claimant cap, eligible chapters, repeat/cooldown limits, optional SDGs and categories.
- Sample quests: introduce a genuine new member (verified sign-up and first confirmed participation before reward — DESIGN DEFAULT anti-fraud rule), teach a peer, support another campus, organise an awareness session, mentor a new volunteer, cross-university SDG collaboration.
- A volunteer claims a quest in mobile, adds outcome and normally photographs; permit privacy-safe evidence alternatives (minors, sensitive scenarios), and can confirm up to **three** team members total. The preset total Gem pool is **shared** by verified teammates, not multiplied; fixed-point ledger avoids rounding discrepancies.
- Approvals by authorised **chapter executive committee only** (no routine district/national reapproval). Officers cannot approve their own/team claim. Reject/request changes with reasons; immutable award/reversal ledger and abuse audit.
- Kindness Quest ≠ automatic formal volunteer marks. An activity might qualify independently under verified formal project rules.
- Optional future seasonal **Kindness League** competitions; no public moral ranking or humiliating low-gem messaging. Kindness Gems represent documented action, not human worth.

## 7. SDG contribution data — CONFIRMED objective; implementation model DESIGN DEFAULT
- Each project has SDG tags tied to real outputs. One activity record may contribute to multiple SDGs but national totals must not double count the same output.
- Store typed verifiable metrics: volunteers, volunteer hours, kilograms of waste, people reached, beneficiaries where measured, finance contributions, photos/report evidence, dates, geography, verification state.
- ZeroPlastic projects predominantly relate to SDGs **12, 13, 14, 15**, with youth development mapped to appropriate goals only when a specific activity supports the claim. Volunteers Academy can run projects across all 17.
- Reports by project, club, district, organisation/programme, SDG, timeframe, verification status; explicitly separate verified platform metrics from claims from unverified Monday submission.

## 8. Screen inventory and UI responsibilities — DESIGN BASELINE
### Mobile
M01 Splash; M02-04 onboarding (vision/benefits/SDGs); M05 Entry; M06 Register/login; M07 Background; M08 Club match; M09 SDG interests; M10 Success; M11 Home/Explore; M12 All Projects (filters/map/list); M13 Project Detail; M14 Join status; M15 My Projects (Upcoming/Ongoing/History); M16 GPS Check-in; M17 GPS Check-out; M18 Kindness Quest home; M19 Quest details; M20 Submit claim/photos/team; M21 My claims/status; M22 Profile/marks/rank; M23 My Impact/SDG history; M24 Certificates (eligibility/download); M25 Notifications; M26 Settings/profile edit.
### Web admin
W01 Login/role context; W02 Chapter dashboard; W03 Chapter volunteers directory; W04 Volunteer detail/suspend chapter membership; W05 Chapter project list/filter; W06 Create/edit draft; W07 Submit/track district approval; W08 Project details/attendance; W09 Cancel/changes dialog; W10 Completion → Monday form confirmation; W11 District dashboard; W12 District project approval queue; W13 Review/approve/reject/request changes; W14 District chapters + volunteers; W15 National dashboard; W16 Nationwide projects/chapters/volunteers + filters; W17 Kindness Quest catalogue/definition editor; W18 Chapter Kindness claim inbox/review; W19 Gem ledger/audit; W20 Certificates/templates/exceptions; W21 Reports/SDG contribution; W22 Roles/settings/partner directory.
All screens require loading, empty, unauthorized, validation, success/error and small-screen states where applicable. UI is design reference, not completed code.

## 9. Minimal engineering plan (preserve current repo)
The current `backend/prisma/schema.prisma` contains auth/profile/RBAC foundation and **does not yet contain** Clubs, Projects, attendance, Gems or Certificates. The `ENHANCED-PRISMA-SCHEMA-V2.md` is a proposal, not the live migration.
1. Extend existing user roles with scoped chapter/district/national permissions; create Club, Membership and partner-project association tables.
2. Project lifecycle and district approval API + minimal web screens.
3. Volunteer discovery and GPS attendance mobile/API, formal mark ledger.
4. Chapter user management, personal profile/history and certificate self-service.
5. National Kindness Quest catalogue, chapter approval, evidence storage, Gem ledger and mobile screens.
6. SDG dashboards and Monday form integration improvements when ready.
Use focused tests for access boundaries, GPS, virtual mark exclusion, double-award prevention and certification threshold. No unnecessary enterprise rollout machinery for this MVP.

## 10. Decisions and limitations
**Founder-confirmed:** Zero App, free common volunteer identity, club connection with no student-card upload, project discovery first, district project approvals, GPS-only in-person attendance with configurable 1 km default, online participants do not receive volunteer marks, chapter Gem approvals, national quest management, Kindness Quest, Kindness Gems, 500-mark e-certificates, Monday external completion report for MVP.

**Defaults to validate before coding:** exact club membership fallback/verification states; suspension and ban authority; material edit/reapproval rules; cancellation authority, offline/GPS exceptions; 500-mark accounting source/rank thresholds; Kindness Quest referral award trigger; certificate signatories; Monday form URL and identifying project fields; evidence storage/retention; minors' safety.

No application source code, migrations, UI assets or deployments are modified by this document.

## 11. Critical operational requirement — mass GPS attendance (2026-11-02)
**Founder-confirmed real-world scenario:** Up to **400 volunteers** at one physical project may all check in within **1–2 minutes** at the start, and check out together at the end. The previous app crashed during this type of burst; prevention is a release-blocking priority for Zero App.

### Acceptance/performance targets (engineering proposal until validated)
- Load-test **400 distinct volunteers within 60 seconds**, including a sharp **50 requests/second** short burst, for both GPS check-in and GPS check-out. These are testing targets, not a claim that AWS Free Tier guarantees capacity.
- No API process crash, lost/duplicate check-ins, duplicate volunteer marks, or inconsistent checkout state. Successful requests must be durable in PostgreSQL.
- Target >=99% successful valid attendance requests during the scripted test and p95 <2 seconds under the defined pilot infrastructure; network failures can legitimately time out, so test client safe retry and eventual reconciliation. Report measured results, not assumed compliance.
- Test duplicate rapid taps, identical idempotency keys, concurrent repeated requests, low-accuracy GPS, out-of-geofence, late arrival, project start/end boundary, cellular disconnect/retry, and simultaneous project activity.

### Implementation design
- Single lightweight `POST /projects/:id/check-in` and `POST /projects/:id/check-out` paths: authenticate; check eligible in-person project/time; validate server-side geodesic radius against saved project coordinates and device-supplied accuracy; atomically write attendance to PostgreSQL and return authoritative result.
- GPS remains required; **no QR code** workaround. Keep location error/permission guidance in mobile and avoid repeated GPS polling of the API.
- Database uniqueness/transaction constraints guarantee an individual cannot create repeated active attendance records. Use idempotency keys for mobile retries. If server confirms an earlier submission but reply was lost, retry returns the existing record rather than a second credit.
- Do not put photo upload, leaderboards, full national analytics, certificate PDF generation or large mark recalculations in the synchronous critical attendance request. Post-processing may be handled asynchronously when needed.
- Provide visible pending state, disable repeat taps and permit safe bounded retry; only show definitive **Checked in/out** after a verified server response. Include clear recovery when the request status is unknown.
- Web chapter/district dashboard must show real attendance counts; avoid expensive global re-count on every request. Allow audited staff corrections for genuine mobile/GPS failures without weakening default GPS policy.
- Monitor error rate, response latency, DB connections, CPU/memory/CPU credits, retries and peak simultaneous check-ins during rehearsals.

### AWS deployment caution
A tiny/free-tier micro EC2 with PostgreSQL on the same host has **not** been proven adequate for these peak bursts, especially under burstable CPU-credit exhaustion. Benchmark the exact provisioned instance before any 400-person live event; upgrade instance size / separate DB if measured capacity fails. AWS T-family instances use CPU credits; performance can throttle or incur surplus-credit costs depending on mode. References: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/burstable-credits-baseline-concepts.html .

### Agile implementation gate
**Attendance module is not accepted until a measured 400-user check-in AND checkout simulation meets the durability and performance goals.** Keep a repeatable k6/Artillery or similar script in repo (with synthetic accounts and mockable GPS coordinates), test against a staging/pilot environment, document actual instance size and the test result in `docs/SESSION-HANDOFF.md`.
