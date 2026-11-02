# CLAUDE.md — Zero App Shared Project Context
Updated: 2026-11-02 | Repository: ZeroPlastic-Movement/zero-plastic-platform

## Read this first, in EVERY Claude Code session
This file is the **shared, version-controlled onboarding and handoff context** for all laptops and Claude sessions. Do not assume previous conversation context is available. On session start:
1. Confirm repo, branch, worktree, and `git status`.
2. Read **this file**, `docs/ZERO-APP-MASTER-PRODUCT-SPEC-V1.md`, and the relevant module specification/source files.
3. Inspect actual code and migrations before proposing work: planning Markdown is not proof of implementation.
4. Read `docs/SESSION-HANDOFF.md` for latest implementation progress, blockers and next action.
5. Restate the immediate task and make a small, module-scoped plan. Do not write or deploy unrelated changes.

**Source-of-truth precedence:** (1) current implementation and database migrations for WHAT EXISTS; (2) founder-confirmed rules in master specification for WHAT TO BUILD; (3) approved design specifications for UI; (4) legacy planning docs as reference only. In a conflict, flag it instead of silently choosing.

## Product
**Zero App** is one free national volunteer app for ZeroPlastic Movement, Volunteers Academy and individuals in partner-supported projects. **No corporate paid tiers, separate corporate app or access codes.** One volunteer account, one primary chapter/club connection, cross-chapter project participation. Two interfaces: **mobile for volunteer engagement** and **web for operations/administration**, backed by one API/database.

### Non-negotiable confirmed business rules
- Anyone can discover nationwide projects (guest browsing where appropriate); registration is lightweight, with study/work/independent background as a club-matching hint, not a separate volunteer account type.
- Institution/club and district are selected during onboarding; if a student selects a university, collect **student registration number as text**, **no ID card photo** and no officer approval at initial MVP registration. Future membership verification remains separate. Existing club associations must not falsely show “verified”.
- Chapters include universities, schools, institutes, community/district groups and other approved types. National admins create chapters. District director scope is assigned districts; national directors see nationwide.
- Chapter executives create, edit, submit and manage their projects. **District directors approve, reject or request changes** to chapter projects. Chapter cannot approve own project.
- **In-person**: project has latitude/longitude and configurable geofence (**1 km default**); check-in AND check-out are GPS-based with server verification, **never QR attendance**.
- **Virtual**: no geofence. Ordinary online attendees **do not earn formal volunteer marks**. Verified organising committee members may earn marks under existing committee rules.
- Chapter Project Completion in MVP launches the **existing Monday.com form** and records officer's self-declared submission/confirmation. Do not assert the external report was verified without integration; no new built-in five-photo/budget/report form yet. Form URL must be obtained, not invented.
- Formal volunteer marks and ranks (Hero, Superhero, Legendary, Titan, etc.) are **different from Kindness Gems**. Use existing mark/rank rules rather than made-up thresholds.
- **Kindness Quest** is the finalized gamification name; **Kindness Gems** the reward. National directors configure quests and Gem values through **web admin**; a chapter executive independently approves submitted claims from their chapter, with no routine district/national second approval. No self-approval. Up to **3 teammates**, shared Gem pool, evidence/consent rules, ledger preventing double-awards.
- Automatic **PDF volunteer e-certificate** is available at **500 verified formal volunteer marks** (not Gems), based on first/last verified attended project dates and count of distinct attended projects, unique serial/verification URL. No printing or routine staff approval.
- Chapter volunteer directory, chapter-level membership suspension, district oversight, nationwide director overview. Account-wide bans require elevated authority/auditing and must preserve legitimate historical records.
- Project discovery filters: district/location/date/time/SDG/project type/club; mobile shows Explore (all eligible), My Upcoming, My Ongoing, My History. Approval state and time-derived display state are separate.
- Project/SDG impact records must not double-count shared outputs or portray unofficial records as official national SDG indicators.
- **Kindness League** tournament is suggested, not an approved fixed feature/name.

## Approved UI/UX (design direction, NOT pixel-perfect code)
Founder approved: 11-screen mobile onboarding journey; Chapter Web Portal (8 screens); Chapter Creation & District Portal (6 screens); National Director Portal (6 screens). The chapter/district/national designs are approved in conversation but their generated PNG boards are not all committed to the repo; do not claim missing artwork is present.
- Mobile: guest discovery, streamlined account/profile/club, SDG interests, project-first home.
- Chapter web: dashboard, projects create/list/details/edit/cancel, volunteer directory/suspension, Kindness claims, Monday completion, chapter reports.
- District web: assigned chapters, project approval/reject/changes, reports.
- National web: create/manage chapters, nationwide oversight, national quest definitions, volunteer administration, SDG insights.
See `docs/design/ZERO-APP-MOBILE-ONBOARDING-APPROVED.md` and `docs/ZERO-APP-MASTER-PRODUCT-SPEC-V1.md`.

## Current repository reality (verify again each session)
- Existing source in `backend/`: Node.js 22, Express, TypeScript, Prisma/PostgreSQL; auth/role/user/profile and achievement foundation. `backend/prisma/schema.prisma` currently has about 10 models; **Club, Project, attendance, Gems and certificates are not yet in main** as of review.
- `docs/RESUME-WORK-GUIDE.md` reports historical 63 passing tests for M0.1–M0.3; that report is not proof of current test results. M0.4 chapter service and M0.5 integration were pending at review.
- The older `ENHANCED-PRISMA-SCHEMA-V2.md`, `M0.2-AUTH-COMPLETE.md`, and legacy roadmaps contain drafts/contradictions and are **not instructions to overwrite current schema**.
- Relevant files: `backend/prisma/schema.prisma`, `backend/src/app.ts`, `backend/src/services/`, `backend/src/routes/`, `backend/src/tests/`.

## Development style (founder-confirmed)
**Agile, modular, lightweight MVP; one usable vertical slice at a time.** Review a module's requirements → adjust UI → implement minimum backend+web/mobile → run focused tests and build → show outcome → refine → proceed. Changes to approved screens are allowed as real use cases emerge. Keep scope narrow, no unnecessary enterprise release approval systems or elaborate DevOps governance. **Never claim tests passed unless actually run.**

Suggested module order: existing authentication/profile check → chapter hierarchy/scoped permissions → chapter project creation/district approval/web → mobile discovery & GPS attendance → marks/rank history → Kindness Quest → self-service certificate → SDG reports and integrations. Dependencies can require small shifts; avoid a giant one-shot rewrite.

## Handoff protocol for the TWO laptops/sessions
At end of **each substantial coding session**, update `docs/SESSION-HANDOFF.md` with:
- UTC date/time; branch and latest commit SHA; which module; what truly changed;
- exact commands/tests run and results (or “not run”);
- migrations/database changes; any production deploy;
- unresolved blockers and the **single next recommended action**;
- changed decisions/spec references.
Commit and push the handoff alongside the code to the branch shared by the sessions. On switching machines, **pull the correct branch first** and check for local uncommitted changes. Do not work concurrently on the same files without reconciling git changes. Never put secrets, tokens, production personal data or .env contents in handoff.

For work started from the draft documentation branch `proposal/gems-and-self-service-certificates`, keep it distinct from `main`. Avoid assuming files in an unmerged PR are visible when a session checks out main.
