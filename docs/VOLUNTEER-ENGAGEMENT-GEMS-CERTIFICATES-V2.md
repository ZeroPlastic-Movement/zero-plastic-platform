# Volunteer Engagement V2 — Gems and Self-Service E-Certificates

Status: **Product proposal for founder review — not implemented**  
Updated: 2026-11-02  
Scope: ZeroPlastic volunteer app (engagement) + web management platform (configuration and approvals) + shared backend.  
Supersedes nothing in the existing marks/rank implementation until reviewed.

## 1. Product principle

The **volunteer app** is for volunteers to discover opportunities, take part, upload evidence, celebrate progress and download certificates. All catalogue configuration, operational work, approvals, exception handling and reporting live in the **web management platform**. Both use a shared API and database.

Protect three distinct concepts:
1. **Volunteer marks/points**: existing formal volunteer contribution metric. Preserve current rank names and thresholds from the existing deployed application; reported ranks include Hero Volunteer, Superhero Volunteer, Legendary Volunteer and Titan Volunteer. Do **not** replace these with the placeholder UserTier enum currently in this repository.
2. **Gems**: complementary recognition for verified kindness, initiative, cross-campus collaboration, learning support and SDG-related positive actions, including non-ZeroPlastic actions. Not redeemable for volunteer marks by default.
3. **Certificates**: self-service evidence of volunteer service, based on verified **volunteer marks and actual project participation**, never on Gems alone.

**Ethics / community design:** Gems reflect documented positive actions, **not** a person's moral worth or character. Never publicly label low-gem users as bad people. Avoid coercion, public shaming, manipulative scarcity or always-on leaderboards.

## 2. Product naming (selected working name)

| Candidate | Tone | Notes |
| --- | --- | --- |
| **Kindness Gems** | clear, warm | Recommended working name; understandable beyond ZeroPlastic |
| **Impact Gems** | action / results | Good if SDG action is predominant; may be confused with volunteer marks |
| **Good Deed Gems** | approachable | Explicit about individual deeds |
| **Humanity Gems** | aspirational | Broader than volunteering; potentially moralising |
| **Spark Gems** | youthful / playful | Needs short explanation |
| **Ripple Gems** | community impact | Distinctive and works for team actions |

**Founder decision (2026-11-02): Kindness Gems is the selected working name.** Use this consistently throughout the mobile app, web administration, API descriptions and product documentation. Alternate names below are historical brainstorm options, not competing product names. Example: “Collect Gems by showing up for others.” Gems are evidence of actions, not a personality score.

## 3. Example Gem catalogue — proposals, not final awards

The web administrator can create, change, retire and categorise activities; configure evidence, base rewards, eligibility, limits and verification. Examples for Sri Lankan university settings:

| Category | Example deed | Proposed base Gems | Evidence |
| --- | --- | ---: | --- |
| Cross-campus friendship | Join a genuine exchange / peer meetup with another university | 1 | group photo + campus/partner |
| Cross-campus collaboration | Visit another university and actively support its project | 5 | event photo + host club confirmation |
| Learning support | Tutor a peer in a subject or digital skill | 2 | consensual photo / learning artifact + short description |
| School education | Lead a school awareness session independently | 5 | school/session photo + basic session details |
| Inclusive community | Help run an accessible event for peers with disabilities | 4 | event photo + organiser confirmation |
| Everyday kindness | Assist a person or community group with a practical need | 1 | privacy-safe photo / alternative proof |
| SDG contribution | Conduct a documented small local SDG-related initiative | 5 | before/after or activity photo + outcome |
| Peer mentorship | Mentor a first-year student over an agreed period | 3 | approved progress notes + consent-safe evidence |
| Community learning | Run an open study / career-skills workshop | 3 | event group photo + attendance description |
| Campus collaboration | Co-organise an inter-university event | 5 | event photos + participating club record |

**Illustrative only.** Avoid rewarding photographing vulnerable people, unsolicited charity encounters or performative acts. Subjects must consent; allow privacy-preserving evidence when photographs would be unsafe, especially with children.

## 4. Team awards and volunteer marks

- A Gem activity may be performed solo or in a registered team of **up to 3 people** (proposed firm maximum based on founder instruction). A 5-person illustration must not become a permissible team size without a later explicit rule change.
- A catalogue activity has a total **Gem pool**. When approved, divide the pool equally among verified team members; do **not** award the full pool to each teammate.
- Store precise fractional allocations (e.g. 5 Gems / 3 = 1.666...) using fixed-point integer units in the ledger; display rounded values without causing accounting discrepancies. Alternatively require pools divisible by eligible team size if a whole-number display is selected later.
- Example: school session worth **5 total Gems**: one approved person = 5; two = 2.5 each; three = approximately 1.67 each, with the exact ledger total remaining 5.
- Existing **volunteer marks** are awarded using the existing project/mark rules. If a school session also qualifies for 10 volunteer marks, that is a separate award process; it does **not** automatically mean 10 marks per teammate or that marks and Gems exchange 1:1. Preserve existing mark thresholds until verified.
- A single documented real-world activity may have both a project participation record and a Gem claim, linked to avoid inconsistent or duplicated proof.
- Prevent repeated farming: catalogue-defined claim frequency/cooldowns, sensible daily/period caps, no self-approval, no approving your own teammate's claim, duplication checks and visible award history.

## 5. Volunteer app flow

1. Open **Gems** tab: balance, earned categories, recent verified deeds, a friendly optional progress visual and suggested activities.
2. Browse **Gem Quests** (catalogue entries; call them “Quests” only in UX if preferred).
3. Choose quest, enter activity date/place, short reflection/outcome, optional participating campus/organisation.
4. Add required **photo evidence** (at least one for ordinary cases), with consent/privacy guidance. For minors, sensitive or inaccessible settings, allow approved privacy-safe alternative evidence rather than requiring identifiable faces.
5. Submit alone or invite up to two teammate accounts. Each teammate confirms participation; only confirmed teammates may receive Gems.
6. Status: Draft / Submitted / Needs changes / Approved / Rejected.
7. University club executive committee approver reviews evidence **on the web** and approves or rejects; an approved award updates Gems automatically and notifies teammates. No district/national secondary approval in normal workflow.
8. Volunteer can see award source, amount and status; corrections are logged, never silently overwrite historical awards.

## 6. University/chapter approval and permissions

- Claims route to the **owning university chapter's authorised executive committee members**. Other eligible chapter types may be configured subsequently.
- Approvals stop at the university chapter; national/district managers retain **read-only oversight, audit and exceptional abuse-reversal privileges**, not a routine second approval queue.
- A submitter cannot approve their own claim; an approver cannot approve a claim in which they are a teammate or beneficiary. Route to another authorised officer (or exception administrator when there is no independent officer).
- For **cross-university** claims, owning chapter approves; optionally attach a host club acknowledgment, not a required second approval.
- If an applicant is not linked to a qualifying chapter, handle via a configured independent approver; do not default to instant auto-approval.
- All award decisions record actor, time, evidence references, catalogue rule version, reason and ledger transaction.

## 7. Self-service volunteer e-certificate

**Eligibility:** a volunteer becomes eligible when their verified existing **volunteer marks/points total reaches 500**. Gems do not count toward that threshold unless the founder changes the rule explicitly. Determine whether 500 is lifetime verified balance or another current-app metric before implementation; use lifetime verified marks as the proposed default.

**Volunteer app:** “Certificates” shows eligibility; at 500+ points show **Generate E-Certificate**. No staff handling, no human approval for an otherwise eligible request, no printed-certificate workflow.

**PDF content:**
- ZeroPlastic branding and formal certificate title (e.g. “Certificate of Volunteer Service”).
- Volunteer legal/preferred name as stored and verified in profile.
- First **verified attended project date** through last **verified attended project date** (not signup date; exclude cancelled/no-show/unverified projects).
- Total number of distinct **verified attended projects** during that service period.
- Issue date, unique certificate ID, QR/URL for public authenticity verification, appropriate authorised digital signature or approved signature artwork.
- Optional verified volunteer points and rank **only if explicitly approved**; do not add Gem judgments or moral labels.

**Request behavior:**
- Available any time *after* threshold is reached. Clicking generates a downloadable PDF automatically from the current eligible participation snapshot.
- Repeated requests do not trigger staff. Default: reuse the existing PDF for an identical snapshot; issue a new serial/version only when verified service data has changed or the user explicitly requests a reissue. Prevent needless duplicate issuance.
- Verification endpoint reveals minimal public fields (name, certificate ID, issue date, status and service period), not private profile or participation evidence.
- Preserve certificate snapshots, issuance history, verification validity, and revocation/correction support on web.
- If zero verified project attendances but >=500 points, do not invent project dates: show an exception for admin review, not an inaccurate certificate.
- **Digital PDF only; printed request excluded.**

## 8. Web management requirements

**Gem catalogue:** create/update/retire quests, descriptions, categories, Gem pool, max team size (global cap 3), frequency, evidence settings, chapter eligibility and published date.

**Chapter approvals:** local executive committee inbox with claim details, photos, teammate confirmations, approve/reject/needs-changes, conflict checks and batch views.

**Oversight:** trends by campus/chapter, suspicious claim patterns, audit trail, award reversal with explanation, storage/privacy settings, no publicly displayed “good-person score”.

**Certificates:** configure 500-mark eligibility threshold, template, signature, verification URL; review exceptional records, revoke/correct certificates, report downloads/issuance. Routine certificates auto-generate without review.

## 9. Minimal shared backend/data design (additive)

Do **not** replace existing auth/user/profile/role tables with the older speculative `ENHANCED-PRISMA-SCHEMA-V2.md`; verify against `backend/prisma/schema.prisma` first.

Proposed future entities:
- `Chapter`, `ChapterMembership`, scoped `RoleAssignment` / executive officer permissions.
- `GemQuestDefinition` (configurable catalogue and version, eligibility, pool, limits, evidence).
- `GemClaim` (owner chapter, creator, date, text, state, quest version).
- `GemClaimMember` (confirmed teammates; max 3 and uniqueness rules).
- `GemEvidence` (storage reference, consent/sensitivity metadata; not public by default).
- `GemApproval` (local decision, actor and audit details).
- `GemLedgerEntry` (immutable award/reversal, integer fixed-point Gem units).
- `VolunteerMarkLedger` / existing mark records (reuse actual implementation; do not create competing balances).
- `ProjectParticipation` (verified attendance, dates, uniqueness).
- `VolunteerCertificate` (PDF object, verified snapshot, serial, issue date, status).
- `CertificateVerification` (token/slug and public minimum disclosure).

Use object storage with size/compression and upload limits for photos. Store evidence privately. On the app, compressed JPEGs and resumable upload when feasible; do not place images in the relational database.

## 10. Updated lightweight delivery order

1. **Requirements and rules first**: verify current deployed volunteer rank thresholds, existing formal marks and certificate branding. Confirm Gem name, catalogue, award split precision and any privacy exceptions.
2. **Shared data/permissions**: finish chapters and chapter-scoped officer rights; add verified attendance and formal mark source-of-truth. Keep existing backend foundations.
3. **Minimum web operations**: chapter/project administration and attendance verification so actual activity data exists before mobile launch.
4. **Mobile volunteer MVP**: discovery, participation, check-in, points/rank display.
5. **Gems vertical slice**: web quest config and chapter approval + app quest list, evidence upload, team confirmation and ledger.
6. **Certificate vertical slice**: web template setup and exception controls + app auto-generation/download and public verification.
7. Engagement polish: friendly collectible gallery, optional personal milestones, restrained notifications, inter-university discovery and accessibility; no exploitative streak pressure or moral scoring.

Keep the MVP focused: one quest catalogue, one university approval inbox, one Gem ledger, one certificate template. Add more advanced dashboards only after real volunteer feedback.

## 11. Open product decisions (do not silently assume these are final)

1. **Resolved:** Kindness Gems selected as the working product name. Keep the user-facing activity term **Gem Quests** pending UX testing.
2. Exact legacy volunteer marks/rank thresholds and which marks count toward 500.
3. The **max 3 teammates** rule vs the hypothetical 5-person illustration (recommended: enforce 3).
4. Fractional Gems display (recommended: 2 decimals; underlying exact fixed-point allocations).
5. Whether all users can see Gem balances or only volunteers and authorised officers (recommended: volunteer-controlled visibility; no negative moral implications).
6. Handling of Gem photo exceptions when children, vulnerable people or private acts of kindness are involved.
7. Certificate signatory/title and public QR verification fields.
8. Whether project dates must span only qualifying verified projects; recommended yes.
9. How to handle cross-campus projects with shared ownership without two approval layers.

## 12. Acceptance checks

- Volunteer submits solo and 2/3-person claims with photos, chapter officer approves locally, balance updates exactly once.
- Non-independent or self-approval blocked server-side; duplicate submissions/replayed approval blocked.
- No district approval in ordinary successful path.
- Marks/ranks do not change from a Gem-only award.
- 499 verified marks: no certificate generation; 500: auto PDF.
- Certificate uses real verified first/last project dates and distinct verified project count.
- Identical re-download returns same PDF rather than endlessly minting identifiers.
- Revoked certificate no longer validates.
- Private evidence never appears in public certificate verification.
- All primary administrative actions are web-based.


## 13. Refined product specification — 2026-11-02

### User-facing mental model
- **Volunteer Marks** measure verified formal service and determine the existing volunteer rank ladder. This remains unchanged.
- **Kindness Gems** celebrate **specific verified positive actions**, not a person's worth. The app calls earning opportunities **Gem Quests**, with categories such as Learn & Share, Care & Support, Campus Connections, Community Action, and SDG Challenges.
- The volunteer home page may show both totals side by side, always with distinct icons and descriptions. Never imply Gems can be purchased or exchanged for marks.
- Language guidelines: "You helped someone learn" or "You completed a kindness quest", not "You are better than students with fewer Gems". No forced public rankings or penalties for not participating.

### Quest lifecycle and configuration
- Each Kindness Gem quest definition includes: slug, title, plain-language action, category, pool value, maximum teammates (1 to 3), evidence rule, eligible chapters, claim frequency, effective dates, published/unpublished status, and reviewer guidance.
- Store the definition **version** used at submission time; later admin edits must not retroactively change submitted awards.
- A volunteer can claim an eligible quest after carrying out the real-world action; no automated award merely for opening or clicking a quest.
- Each claim records event date, chapter, description/outcome, evidence, confirmed teammates, review state and audit history.
- Verification is a single chapter-level decision by an authorised executive officer who is not an award beneficiary; the system credits the ledger only after approval.
- For a cross-campus action, record host campus and optional confirmation without requiring a second approval tier.

### Balanced rollout for university communities
Start with a modest quest catalogue of 10–15 real-world tasks. Use varied categories rather than incentivising only visible charity or environmental work. Claims should be easy enough to submit in a minute or two, but evidence and review should discourage spam. Protect beneficiaries' privacy and make sensitive-case evidence exceptions explicit.

### Technical reward integrity
- Award amounts are owned by the chapter-approved claim, not mutable directly by volunteers.
- The Gem ledger is append-only: an approved claim creates a single transaction per beneficiary; a correction adds reversing transactions with recorded reasons rather than editing history.
- Team membership must be confirmed before approval; the configured Gem pool is shared among up to three beneficiaries and never multiplied per member.
- Use fixed-point precision for fractional splits. Ensure total distributable units always equal the configured pool using deterministic remainder allocation and show rounded displays clearly.
- The same project may separately create volunteer marks **only through its existing marks rules**, with a link between source records to prevent double counting.

### Self-service certificates — precise eligibility and period
- Eligibility is based on at least 500 **verified formal volunteer marks**, excluding Kindness Gems, provisional marks and reverted awards.
- A valid issued certificate covers the first to last **verified attended project** in the records at generation time; count distinct attended project IDs, not event invites or Gem Quests.
- If service continues, the volunteer may regenerate when the verified attendance snapshot changes. No printed requests or ordinary staff approvals.
- The website verification URL/QR confirms the issued snapshot without exposing evidence photos, private contact details or detailed activity logs.
- The web platform controls the signing/template settings and handles exceptional corrections only.

### App and web navigation proposal
**Mobile:** Home | Activities | Kindness Gems | My Impact | Profile (certificate access via My Impact). Keep navigation compact and avoid management menus.

**Web:** Dashboard | Projects | Volunteers | Chapters | Kindness Gems (Quest Catalogue, Approval Inbox, Ledger & Audit) | Certificates (Template, Exceptions, Verification) | Reports | Settings.

### Revised MVP acceptance examples
1. Solo quest with configured 5 Gems: after one authorised university approval, award exactly 5; retrying approval awards zero additional Gems.
2. Three verified teammates share 5 Gems: exact total ledger allocation remains 5 even with rounded UI displays.
3. Executive officer who participated cannot approve that claim; another authorised officer must decide.
4. Rejected claim awards zero Gems and offers a reason; evidence is not visible publicly.
5. An eligible volunteer with 500 verified marks and multiple verified projects obtains a unique PDF without contacting staff; 499 verified marks cannot.
6. A student with 500 marks but no verified attended project is shown a data-quality exception; the certificate never invents participation dates.
7. The same PDF can be downloaded again; a changed verified attendance snapshot can produce a new version with independent verification.

### Next product decisions to confirm before implementation
The Kindness Gems name is settled as the **working name**. Remain open on visibility of Gem totals to other volunteers, exact chapter executive committee permissions, quest catalogue values and cooldowns, points precision display, evidence for private acts and minors, existing rank thresholds, and certificate signing authority. Do not silently treat these as final.
