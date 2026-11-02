# Zero Plastic Platform
## Modular Development Roadmap - Solution Architect's Guide

**Version:** 1.0  
**Date:** July 2026  
**Author:** Solution Architect  
**Purpose:** Step-by-step module-by-module implementation with validation gates  
**Philosophy:** "Test one module completely before moving to the next. Minimize rework. Build stability first."

---

## TABLE OF CONTENTS

1. [Architecture Overview](#1-architecture-overview)
2. [Foundational Principles](#2-foundational-principles)
3. [Phase 1: Backend Foundation (Weeks 1-2)](#3-phase-1-backend-foundation-weeks-1-2)
4. [Phase 2: Mobile MVP (Weeks 3-8)](#4-phase-2-mobile-mvp-weeks-3-8)
5. [Phase 3: Web App Core (Weeks 9-14)](#5-phase-3-web-app-core-weeks-9-14)
6. [Phase 4: Web App Advanced (Weeks 15-20)](#6-phase-4-web-app-advanced-weeks-15-20)
7. [Module Dependencies Matrix](#7-module-dependencies-matrix)
8. [Validation & Testing Strategy](#8-validation--testing-strategy)
9. [Risk Mitigation](#9-risk-mitigation)
10. [Code Organization](#10-code-organization)

---

## 1. ARCHITECTURE OVERVIEW

### High-Level System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                   SINGLE BACKEND API                     │
│         Express.js + PostgreSQL + Prisma ORM             │
│                  (Shared by Mobile & Web)                │
└──────────────────────────┬──────────────────────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        ▼                  ▼                  ▼
   MOBILE APP         WEB APP             EXTERNAL
   (Flutter)          (React)            INTEGRATIONS
                                      (Twilio, Make.com)
```

### Module Hierarchy

```
BACKEND LAYERS (Foundation)
├── L0: Database & Migrations (Prisma)
├── L1: Core Services (Auth, Users, Projects)
├── L2: Business Logic (Marks, Approval Workflows)
├── L3: External Integrations (Twilio, Make.com)
└── L4: API Endpoints (Express routes)

MOBILE APP (Sits on top of Backend)
├── M1: Authentication Module (Register, Login)
├── M2: Project Discovery Module (Map, List, Search)
├── M3: Check-In/Out Module (GPS, Location)
├── M4: Achievement Module (Profile, Points, Badges)
├── M5: Notification Module (Push, SMS)
└── M6: Certificate Module (Display, Download)

WEB APP (Sits on top of Backend)
├── W1: Authentication Module (Login)
├── W2: Club Dashboard Module
├── W3: Project Management Module (Create, Edit, Submit)
├── W4: Approval Workflow Module
├── W5: Volunteer Management Module
├── W6: Mark Assignment Module
├── W7: Reporting Module
├── W8: Analytics Module
├── W9: CRM Module
├── W10: Team Management Module
├── W11: Complaint Management Module
└── W12: Admin Module
```

---

## 2. FOUNDATIONAL PRINCIPLES

### Core Philosophy

```
PRINCIPLE 1: BUILD FROM FOUNDATION UP
├─ Don't build mobile features that depend on incomplete backend
├─ Don't build web features that need unstable APIs
└─ Always ensure dependencies are 100% complete first

PRINCIPLE 2: ONE MODULE AT A TIME
├─ Complete one module 100%
├─ Test it thoroughly
├─ Validate it works
├─ Document it
└─ THEN move to next module

PRINCIPLE 3: TEST BEFORE MOVING ON
├─ Unit tests for all business logic
├─ Integration tests for API endpoints
├─ Manual testing for user flows
├─ Performance testing for critical paths
└─ NO "we'll fix it later" attitude

PRINCIPLE 4: MINIMIZE REWORK
├─ Clear module contracts/interfaces
├─ Comprehensive data validation
├─ Proper error handling
├─ API versioning from day 1
└─ Database migrations before code changes

PRINCIPLE 5: BUILD STABILITY FIRST
├─ Core authentication works perfectly
├─ Database is solid
├─ API foundation is bulletproof
├─ Then add features on top
└─ New features don't break old ones
```

### Module Readiness Checklist (Every Module)

```
BEFORE STARTING A MODULE:
- [ ] Dependencies identified and completed
- [ ] Module scope clearly defined
- [ ] Acceptance criteria written
- [ ] Test cases prepared
- [ ] API contracts finalized

DURING DEVELOPMENT:
- [ ] Code follows style guide
- [ ] Unit tests written (>80% coverage)
- [ ] Integration tests written
- [ ] Error handling comprehensive
- [ ] Database migrations versioned

BEFORE MARKING COMPLETE:
- [ ] All tests passing
- [ ] Code review approved
- [ ] Documentation updated
- [ ] Manual testing done
- [ ] Performance acceptable
- [ ] Deployed to staging
- [ ] Smoke tested in staging

BEFORE MOVING TO NEXT MODULE:
- [ ] All above complete
- [ ] Sign-off from architect
- [ ] Lessons learned documented
- [ ] Team confirms readiness
```

---

## 3. PHASE 1: BACKEND FOUNDATION (Weeks 1-2)

### Objective
Build the complete backend API foundation that both mobile and web apps will use.

### Why Do This First?
- Both mobile and web depend on backend API
- Getting backend right eliminates frontend surprises
- Database schema affects all downstream work
- Authentication module is dependency for everything

### Phase 1 Modules

#### **M0.1: Database Schema & Migrations**

**What to Build:**
```
Create complete Prisma schema with:
├─ Users table (volunteer, leader, admin)
├─ Profiles table (detailed user info)
├─ Clubs table
├─ Projects table
├─ Check-In/Out records
├─ Marks & Points
├─ Badges & Achievements
├─ Certificates
├─ Notifications
├─ Complaints
└─ Audit logs
```

**Acceptance Criteria:**
- [ ] All 12+ entities in schema
- [ ] All relationships correct
- [ ] All indexes created
- [ ] Migration scripts versioned
- [ ] Schema documented
- [ ] Can generate test data

**Testing Strategy:**
```
1. Create test database
2. Run migrations
3. Verify all tables exist
4. Verify all columns correct
5. Test relationships
6. Insert test data
7. Run sample queries
8. Verify performance
```

**Deliverables:**
- Prisma schema file
- Migration scripts (numbered)
- Entity relationship diagram (ERD)
- Data dictionary
- Sample test data SQL

**Dependencies:** None  
**Timeline:** 2-3 days  
**Risk:** Schema changes later = big rework. Get it right now.

---

#### **M0.2: Authentication Module (Core)**

**What to Build:**
```
Backend authentication system:
├─ JWT token generation
├─ Password hashing (bcrypt)
├─ Registration endpoint
├─ Login endpoint
├─ Token refresh endpoint
├─ Password reset flow
├─ Phone verification (Twilio)
└─ Session management
```

**Acceptance Criteria:**
- [ ] Register new user (phone + SMS verification)
- [ ] Login returns valid JWT
- [ ] Token expires correctly
- [ ] Refresh token works
- [ ] Password reset via email
- [ ] Password reset via SMS
- [ ] Invalid credentials rejected
- [ ] All tests passing

**Testing Strategy:**
```
UNIT TESTS:
├─ Password hashing
├─ JWT generation
├─ Token validation
└─ Expiry logic

INTEGRATION TESTS:
├─ Register → Login flow
├─ Token refresh
├─ Password reset
├─ SMS verification
└─ Invalid flows

MANUAL TESTING:
├─ Register a user
├─ Get SMS code
├─ Verify phone
├─ Login
├─ Check JWT in token
├─ Logout
└─ Try to login with wrong password
```

**Deliverables:**
- Auth service class
- Auth middleware
- Auth routes
- JWT config
- Tests
- API documentation

**Dependencies:** M0.1 (Database)  
**Timeline:** 3-4 days  
**Risk:** Poor auth = security issue. Test thoroughly.

---

#### **M0.3: Core User Service**

**What to Build:**
```
User management service:
├─ Create user profile
├─ Get user by ID
├─ Get user by email/phone
├─ Update user profile
├─ Get user role & permissions
├─ User existence check
└─ User status (active/banned/etc)
```

**Acceptance Criteria:**
- [ ] Create user with all fields
- [ ] Retrieve user by ID
- [ ] Retrieve user by email
- [ ] Retrieve user by phone
- [ ] Update profile fields
- [ ] Get role information
- [ ] Get permissions for role
- [ ] Check if banned
- [ ] Error on invalid ID

**Testing Strategy:**
```
UNIT TESTS:
├─ User creation
├─ Field validation
├─ Data transformation
└─ Role logic

INTEGRATION TESTS:
├─ Create user → Retrieve user
├─ Update user → Verify changes
├─ Role → Permissions mapping
└─ Invalid inputs

MANUAL TESTING:
├─ Create different user types
├─ Verify all data saved
├─ Update various fields
├─ Check permissions by role
└─ Test with invalid data
```

**Deliverables:**
- User service class
- User repository
- Tests
- API documentation

**Dependencies:** M0.1, M0.2 (Auth)  
**Timeline:** 2-3 days

---

#### **M0.4: Project & Club Entities**

**What to Build:**
```
Core project service:
├─ Create project
├─ Get project by ID
├─ List projects (with filters)
├─ Update project
├─ Set project status
├─ Associate volunteers with project
└─ Get project volunteers

Core club service:
├─ Create club
├─ Get club
├─ Update club info
├─ List club members
└─ Add/remove member
```

**Acceptance Criteria:**
- [ ] Create project with all fields
- [ ] Get project details
- [ ] List projects with pagination
- [ ] Filter projects by status
- [ ] Update project fields
- [ ] Change project status
- [ ] Assign volunteer to project
- [ ] Get all volunteers in project
- [ ] Create club
- [ ] Get club members
- [ ] Add user to club

**Testing Strategy:**
```
UNIT TESTS:
├─ Project creation
├─ Status validation
├─ Field requirements
└─ Relationship logic

INTEGRATION TESTS:
├─ Create project → List project
├─ Update project → Verify changes
├─ Add volunteer → Get volunteers
├─ Filter operations
└─ Pagination

MANUAL TESTING:
├─ Create project with all fields
├─ Verify it appears in list
├─ Update various fields
├─ Add multiple volunteers
├─ Verify filtering
└─ Test pagination limits
```

**Deliverables:**
- Project service & repository
- Club service & repository
- Tests
- API documentation

**Dependencies:** M0.1, M0.2  
**Timeline:** 3-4 days

---

#### **M0.5: Core API Routes (Express Layer)**

**What to Build:**
```
All Phase 1 API endpoints:
├─ POST /api/auth/register
├─ POST /api/auth/login
├─ POST /api/auth/logout
├─ POST /api/auth/refresh
├─ POST /api/auth/password-reset
├─ GET  /api/users/me
├─ PUT  /api/users/me
├─ GET  /api/projects
├─ GET  /api/projects/:id
├─ POST /api/clubs
├─ GET  /api/clubs/:id
└─ Error handling middleware
```

**Acceptance Criteria:**
- [ ] All endpoints working
- [ ] Request validation
- [ ] Response format consistent
- [ ] Error handling proper
- [ ] Status codes correct
- [ ] CORS configured
- [ ] Rate limiting added
- [ ] Logging implemented

**Testing Strategy:**
```
USE POSTMAN:
├─ Test each endpoint manually
├─ Test with valid data
├─ Test with invalid data
├─ Test error cases
├─ Verify response format
├─ Check status codes
└─ Verify rate limiting

INTEGRATION TESTS:
├─ Full auth flow
├─ Full CRUD operations
├─ Error flows
└─ Permission checks

LOAD TESTING:
├─ Rate limit enforcement
├─ Concurrent request handling
└─ Performance under load
```

**Deliverables:**
- Express routes
- Middleware
- Error handlers
- Postman collection
- API documentation
- Tests

**Dependencies:** M0.2, M0.3, M0.4  
**Timeline:** 2-3 days

---

### Phase 1 Validation Gate

Before moving to Phase 2, validate:

```
BACKEND READINESS CHECKLIST:
- [ ] All database tables created & tested
- [ ] All migrations working
- [ ] Authentication working (register → login)
- [ ] User service working
- [ ] Project/Club service working
- [ ] All endpoints tested in Postman
- [ ] All tests passing (>80% coverage)
- [ ] API documentation complete
- [ ] Error handling comprehensive
- [ ] Code review approved
- [ ] Deployed to Azure staging
- [ ] Smoke tests passing in staging
```

**Sign-off Required From:**
- Backend Lead Developer
- Solution Architect
- QA Lead

**If Validation Fails:**
- Do NOT proceed to Phase 2
- Fix issues
- Re-validate
- Root cause analysis
- Document learnings

---

## 4. PHASE 2: MOBILE MVP (Weeks 3-8)

### Objective
Build complete mobile app that allows volunteers to discover, join, and check-in to projects.

### Why Parallel Development?
- Backend foundation is complete and stable
- Mobile team can work independently
- Web team can wait for Phase 1 completion
- Both teams could work in parallel after Phase 1

### Mobile App Modules (Sequential within Phase 2)

#### **M1.1: Mobile Setup & Architecture**

**What to Build:**
```
Flutter project infrastructure:
├─ Flutter project structure
├─ Dependency management (pubspec.yaml)
├─ State management (Riverpod setup)
├─ HTTP client (Dio setup)
├─ Secure storage (flutter_secure_storage)
├─ Environment configuration
├─ Error handling framework
├─ Logging setup
└─ Testing framework
```

**Acceptance Criteria:**
- [ ] Flutter project builds without errors
- [ ] All dependencies resolved
- [ ] Project structure organized
- [ ] State management working
- [ ] HTTP client configured
- [ ] Secure storage working
- [ ] Environment variables working
- [ ] Logging working
- [ ] Tests can run

**Testing Strategy:**
```
1. Build project
2. Verify no dependency errors
3. Test state management (simple provider)
4. Test HTTP client (mock request)
5. Test secure storage
6. Verify logging output
7. Run simple unit test
```

**Deliverables:**
- Flutter project
- pubspec.yaml
- Project structure docs
- Architecture diagram
- Setup guide

**Dependencies:** None  
**Timeline:** 1-2 days

---

#### **M1.2: Auth UI (Login & Register Screens)**

**What to Build:**
```
Mobile authentication UI:
├─ Splash screen
├─ Login screen
│  ├─ Email/phone input
│  ├─ Password input
│  ├─ Login button
│  ├─ Forgot password link
│  └─ Register link
├─ Register screen
│  ├─ Phone number input
│  ├─ SMS verification code input
│  ├─ Name input
│  ├─ Password input
│  ├─ Confirm password
│  ├─ Club selection dropdown
│  └─ Register button
├─ Password reset screen
└─ Navigation between screens
```

**Acceptance Criteria:**
- [ ] Splash shows 2-3 seconds
- [ ] Login form has all fields
- [ ] Validation works (email format, password strength)
- [ ] Register form has all fields
- [ ] SMS code input field works
- [ ] Form submission works (doesn't crash)
- [ ] Navigation between screens works
- [ ] Error messages display
- [ ] Keyboard handling correct
- [ ] Responsive on different screen sizes

**Testing Strategy:**
```
MANUAL TESTING:
├─ Tap on each field
├─ Try invalid inputs
├─ Try valid inputs
├─ Check validation messages
├─ Try navigation
├─ Check button states
├─ Verify keyboard appears
└─ Check on different devices

UNIT TESTS:
├─ Input validation logic
├─ Form state management
├─ Navigation logic
└─ Error handling
```

**Deliverables:**
- Auth screens (UI only, no API calls yet)
- Widgets & components
- Tests
- Screenshots

**Dependencies:** M1.1  
**Timeline:** 2-3 days

---

#### **M1.3: Auth API Integration**

**What to Build:**
```
Connect auth screens to backend API:
├─ API client setup
├─ Register API call
│  ├─ Send phone number
│  ├─ Verify SMS code
│  ├─ Create account
│  └─ Store JWT token securely
├─ Login API call
│  ├─ Send credentials
│  ├─ Receive JWT token
│  └─ Store token
├─ Token management
│  ├─ Store token securely
│  ├─ Auto-refresh token
│  ├─ Check token expiry
│  └─ Clear on logout
├─ Error handling
│  ├─ Invalid credentials
│  ├─ Network errors
│  ├─ Server errors
│  └─ Display error messages
└─ Loading states
```

**Acceptance Criteria:**
- [ ] Register → SMS → Verify → Account created
- [ ] Login → Token received → Stored securely
- [ ] Token sent in all API requests
- [ ] Token refresh works
- [ ] Logout clears token
- [ ] Network errors handled
- [ ] Invalid credentials show error
- [ ] Loading indicators show
- [ ] Error messages user-friendly
- [ ] No crashes

**Testing Strategy:**
```
INTEGRATION TESTING:
├─ Register new user → Login
├─ Login with valid credentials
├─ Login with invalid credentials
├─ Logout → Token cleared
├─ Network error handling
├─ Token expiry → Auto-refresh
└─ Invalid token rejection

UNIT TESTS:
├─ Token storage/retrieval
├─ API request formatting
├─ Error parsing
└─ Token validation
```

**Deliverables:**
- API client class
- Auth service
- Token management
- Tests
- API request examples

**Dependencies:** M0.5 (Backend endpoints), M1.2  
**Timeline:** 3-4 days

---

#### **M1.4: Home Dashboard Screen**

**What to Build:**
```
Mobile home screen showing:
├─ Welcome message (Hi [Name]!)
├─ User stats card
│  ├─ Total points
│  ├─ Current tier badge
│  ├─ Total hours
│  └─ Projects completed
├─ Upcoming projects section
│  ├─ Next 2-3 projects
│  ├─ Project name, date, time
│  └─ Tap to see details
├─ Quick action buttons
│  ├─ Discover projects
│  ├─ View profile
│  ├─ View notifications
│  └─ Settings
└─ Bottom navigation
   ├─ Home
   ├─ Projects
   ├─ Profile
   └─ Settings
```

**Acceptance Criteria:**
- [ ] Screen displays correctly
- [ ] User name shows
- [ ] Stats load from API
- [ ] Upcoming projects show
- [ ] Buttons navigate correctly
- [ ] Bottom nav selects active screen
- [ ] Pull-to-refresh works
- [ ] Loading state shows
- [ ] Error handling shows
- [ ] Responsive layout

**Testing Strategy:**
```
MANUAL TESTING:
├─ Launch app
├─ Verify all elements visible
├─ Tap each button
├─ Verify navigation
├─ Pull to refresh
├─ Check on different screen sizes
└─ Check loading states

INTEGRATION:
├─ Load user stats from API
├─ Load upcoming projects from API
├─ Refresh on navigation back
└─ Handle API errors
```

**Deliverables:**
- Home screen widget
- Stats card component
- Project card component
- Tests
- Screenshots

**Dependencies:** M1.3 (Auth), M0.3 (User service), M0.4 (Projects)  
**Timeline:** 2-3 days

---

#### **M1.5: Project Discovery (Map + List Views)**

**What to Build:**
```
Two ways to discover projects:

MAP VIEW:
├─ Google Maps display
├─ Show projects as pins
├─ Filter by distance (5km, 10km, 20km)
├─ Filter by date
├─ Tap pin → Show project card
├─ Project card → View details button
└─ Current location marker

LIST VIEW:
├─ Projects as scrollable list
├─ Project name, date, time, distance
├─ Filter by date
├─ Filter by distance
├─ Sort by nearest/soonest
├─ Search projects by name
├─ Tap project → View details
└─ Switch to map view button
```

**Acceptance Criteria:**
- [ ] Map loads without error
- [ ] Shows current location
- [ ] Shows project pins
- [ ] Pins are clickable
- [ ] Filters work (distance, date)
- [ ] List shows all projects
- [ ] List sorting works
- [ ] Search works
- [ ] Can switch between map/list
- [ ] Pull-to-refresh works
- [ ] Loading indicators show
- [ ] Error handling

**Testing Strategy:**
```
MANUAL TESTING:
├─ Open map view
├─ Verify location enabled
├─ Tap on pins
├─ Test distance filter
├─ Test date filter
├─ Switch to list
├─ Test search
├─ Test sorting
└─ Test pull-to-refresh

INTEGRATION:
├─ Load projects from API
├─ Filter on client side (or server)
├─ Handle location permission denied
└─ Handle no projects found

UNIT TESTS:
├─ Filter logic
├─ Sort logic
├─ Search logic
└─ Distance calculation
```

**Deliverables:**
- Map view widget
- List view widget
- Filter component
- Search component
- Tests
- Screenshots

**Dependencies:** M1.4, M0.4 (Projects), Google Maps API  
**Timeline:** 4-5 days (hardest module so far)

---

#### **M1.6: Project Detail & Accept/Decline**

**What to Build:**
```
Project detail screen:
├─ Project name, description
├─ Date, time, location
├─ Volunteer count needed
├─ Volunteers signed up
├─ Map showing location
├─ Distance from user
├─ ACCEPT button (green)
├─ DECLINE button (gray)
├─ Back button
└─ Share button

When user taps ACCEPT:
├─ Call API to accept project
├─ Show confirmation
├─ Navigate to home
└─ Update upcoming projects

When user taps DECLINE:
├─ Show reason selector
├─ Send decline + reason to API
├─ Navigate back
└─ Remove from list
```

**Acceptance Criteria:**
- [ ] All project details show
- [ ] Map displays location
- [ ] Distance calculated correctly
- [ ] ACCEPT button works
- [ ] DECLINE button works
- [ ] Reason selector shows
- [ ] API call succeeds
- [ ] Confirmation message shows
- [ ] Navigate back correctly
- [ ] Volunteer count updates

**Testing Strategy:**
```
MANUAL TESTING:
├─ Open project detail
├─ Verify all info visible
├─ Tap ACCEPT
├─ Verify confirmation
├─ Go back
├─ Open another project
├─ Tap DECLINE
├─ Select reason
├─ Verify decline sent
└─ Verify project removed

INTEGRATION:
├─ Accept project → API called
├─ Decline project → API called
├─ Error handling
└─ Volunteer count updated

UNIT TESTS:
├─ Distance calculation
├─ Data formatting
└─ State management
```

**Deliverables:**
- Project detail screen
- Decline reason selector
- Tests
- Screenshots

**Dependencies:** M1.5, M0.4  
**Timeline:** 2-3 days

---

#### **M1.7: Check-In/Out with GPS**

**What to Build:**
```
GPS-based check-in system:

BEFORE PROJECT START:
├─ Show "Not yet time to check-in"
├─ Show countdown timer
└─ Gray out check-in button

WHEN READY TO CHECK-IN:
├─ Show "Ready to check in" message
├─ Show current location accuracy
├─ Show geofence on map (100m radius)
├─ Location must be within 100m
├─ Tap "CHECK-IN" button
├─ Send GPS coordinates + timestamp to API
├─ Show confirmation: "✅ Checked In at 9:05 AM"
└─ Show "Now check out when done"

CHECK-OUT:
├─ Show hours worked (auto-calculated)
├─ Show points earned
├─ Show tier progress
├─ Allow manual hour adjustment
├─ Tap "CHECK-OUT" button
├─ Send checkout + hours to API
├─ Show confirmation: "✅ Checked Out + 60 points earned!"
└─ Show updated stats
```

**Acceptance Criteria:**
- [ ] Requests location permission
- [ ] Gets current location
- [ ] Shows geofence on map
- [ ] Check-in disabled if outside 100m
- [ ] Check-in works when inside 100m
- [ ] Timestamp recorded correctly
- [ ] Check-out form shows hours
- [ ] Hours auto-calculated from check-in time
- [ ] Manual hour adjustment works
- [ ] Check-out sends to API
- [ ] Points updated correctly
- [ ] Offline fallback (store locally)
- [ ] No crashes

**Testing Strategy:**
```
MANUAL TESTING:
├─ Enable location services
├─ Open project detail before time
├─ Verify button disabled
├─ Wait until ready time
├─ Verify button enabled
├─ Go to project location
├─ Tap check-in
├─ Verify confirmation
├─ Wait awhile
├─ Tap check-out
├─ Verify hours calculated
├─ Verify points added
└─ Check profile stats updated

INTEGRATION:
├─ Check-in API call
├─ Check-out API call
├─ Location validation on backend
├─ Error handling

UNIT TESTS:
├─ Location distance calculation
├─ Hours calculation
├─ Geofence logic
├─ Timestamp handling
└─ Offline storage
```

**Deliverables:**
- Check-in screen widget
- Check-out screen widget
- GPS location service
- Geofence implementation
- Tests
- Screenshots

**Dependencies:** M1.4, M0.4, Geolocator package  
**Timeline:** 4-5 days (complex GPS logic)

---

#### **M1.8: Profile & Achievements**

**What to Build:**
```
User profile screen:
├─ Profile picture
├─ Name
├─ Email/phone
├─ Total points
├─ Total hours volunteered
├─ Projects completed
├─ Current tier with badge
├─ Tier progression bar
├─ Edit profile button
│  ├─ Change name
│  ├─ Change profile picture
│  ├─ Change bio/about
│  └─ Save changes
└─ View certificates button

Achievements section:
├─ Tier badges timeline
├─ Milestone achievements
├─ Project certificates
├─ Share achievement button
└─ Social media sharing
```

**Acceptance Criteria:**
- [ ] Profile info displays correctly
- [ ] Edit form appears
- [ ] Profile picture upload works
- [ ] Can update name/bio
- [ ] Changes saved to API
- [ ] Tier badge displays
- [ ] Tier progression shows
- [ ] All achievements show
- [ ] Can share on social
- [ ] Share dialog works

**Testing Strategy:**
```
MANUAL TESTING:
├─ Open profile
├─ Verify all info visible
├─ Tap edit
├─ Change name
├─ Upload profile picture
├─ Save
├─ Verify changes
├─ Scroll to achievements
├─ Tap share
├─ Verify share dialog
├─ Select social media
└─ Verify share

INTEGRATION:
├─ Load user profile from API
├─ Upload profile picture
├─ Update profile API call
├─ Load achievements from API
└─ Handle API errors

UNIT TESTS:
├─ Tier calculation
├─ Points to tier mapping
├─ Image upload formatting
└─ Social share formatting
```

**Deliverables:**
- Profile screen
- Edit profile form
- Achievement card component
- Tests
- Screenshots

**Dependencies:** M1.1, M0.3  
**Timeline:** 3-4 days

---

#### **M1.9: Certificates Display & Download**

**What to Build:**
```
Certificates screen:
├─ List of earned certificates
├─ Certificate image thumbnail
├─ Project name (from certificate)
├─ Date earned
├─ Download button
├─ Share button
├─ Certificate detail view
│  ├─ Full certificate image
│  ├─ QR code for verification
│  ├─ Download button
│  └─ Share options
└─ Pagination (if many certificates)

Share options:
├─ Download as PDF
├─ Share on WhatsApp
├─ Share on LinkedIn
├─ Share on Instagram
└─ Share on Facebook
```

**Acceptance Criteria:**
- [ ] List loads certificates from API
- [ ] Thumbnails display
- [ ] Can tap to see full certificate
- [ ] Download button works
- [ ] Share button works
- [ ] Share dialog appears
- [ ] WhatsApp share works
- [ ] LinkedIn share works
- [ ] PDF download works
- [ ] QR code displays

**Testing Strategy:**
```
MANUAL TESTING:
├─ Navigate to certificates
├─ Verify list loads
├─ Tap on certificate
├─ Verify full image shows
├─ Tap download
├─ Verify PDF saved
├─ Tap share
├─ Verify share dialog
├─ Try share on WhatsApp
└─ Verify message opens

INTEGRATION:
├─ Load certificates from API
├─ Download certificate endpoint
└─ Handle no certificates

UNIT TESTS:
├─ PDF generation
├─ Share URL formatting
└─ QR code generation
```

**Deliverables:**
- Certificates list screen
- Certificate detail screen
- PDF download service
- Share service
- Tests
- Screenshots

**Dependencies:** M1.8, M0.4  
**Timeline:** 2-3 days

---

#### **M1.10: Push Notifications**

**What to Build:**
```
Notification system:

SETUP:
├─ Firebase Cloud Messaging (FCM) setup
├─ Request notification permission
├─ Store FCM token in backend
└─ Handle token refresh

NOTIFICATION TYPES:
├─ Project reminder (24 hours before)
├─ Project reminder (2 hours before)
├─ Tier promotion celebration
├─ New project nearby
├─ Approval notifications
├─ System notifications
└─ Custom admin notifications

NOTIFICATION CENTER:
├─ List of all notifications
├─ Mark as read
├─ Delete notification
├─ Tap notification → Go to project
├─ Badge count on app icon
└─ Notification settings
```

**Acceptance Criteria:**
- [ ] Notification permission requested
- [ ] FCM token obtained
- [ ] Token sent to backend
- [ ] Can receive test notification
- [ ] Notification displays in notification center
- [ ] Tapping notification goes to correct place
- [ ] Mark as read works
- [ ] Delete works
- [ ] Badge count shows
- [ ] Settings control which notifications

**Testing Strategy:**
```
MANUAL TESTING:
├─ Install app
├─ Accept notification permission
├─ Check FCM token sent to backend
├─ Use backend to send test notification
├─ Receive notification on device
├─ Tap notification
├─ Verify it goes to correct screen
├─ Open notification center
├─ Verify notification shows
├─ Mark as read
├─ Delete notification
└─ Check notification settings

INTEGRATION:
├─ Backend sends FCM message
├─ App receives message
├─ Message triggers correct action
└─ Handle expired tokens

UNIT TESTS:
├─ Notification parsing
├─ Badge count logic
├─ Preference storage
└─ FCM token management
```

**Deliverables:**
- FCM setup
- Notification service
- Notification center screen
- Notification settings
- Tests
- Setup documentation

**Dependencies:** M1.8, Firebase setup  
**Timeline:** 3-4 days

---

#### **M1.11: Settings & Preferences**

**What to Build:**
```
Settings screen:
├─ Account settings
│  ├─ Change password
│  ├─ Change email
│  ├─ Change phone
│  ├─ Logout
│  └─ Delete account
├─ Notification preferences
│  ├─ Toggle each notification type
│  ├─ Quiet hours
│  ├─ Preferred channels (push/SMS/email)
│  └─ Save preferences
├─ App settings
│  ├─ Theme (light/dark)
│  ├─ Language
│  ├─ About app
│  └─ Version
└─ Help & Support
   ├─ FAQ
   ├─ Contact support
   ├─ Feedback
   └─ App version
```

**Acceptance Criteria:**
- [ ] All settings visible
- [ ] Can change password
- [ ] Can update email
- [ ] Can toggle notifications
- [ ] Quiet hours selector works
- [ ] Theme toggle works
- [ ] Changes persist
- [ ] Logout works
- [ ] Delete account works
- [ ] Help links work

**Testing Strategy:**
```
MANUAL TESTING:
├─ Open settings
├─ Change password
├─ Verify old password no longer works
├─ Try new password
├─ Toggle notifications
├─ Set quiet hours
├─ Change theme
├─ Verify theme changes
├─ Logout
├─ Verify redirected to login
└─ Login again

INTEGRATION:
├─ Save preferences to API
├─ Load preferences from API
├─ Change password API call
└─ Logout API call

UNIT TESTS:
├─ Password strength validation
├─ Preference storage
├─ Theme application
└─ Time validation for quiet hours
```

**Deliverables:**
- Settings screen
- Settings form components
- Settings service
- Tests
- Screenshots

**Dependencies:** M1.8  
**Timeline:** 2-3 days

---

### Phase 2 Validation Gate

Before marking mobile MVP complete:

```
MOBILE READINESS CHECKLIST:

FUNCTIONALITY:
- [ ] Register → SMS verify → Login flow complete
- [ ] Can discover projects (map & list)
- [ ] Can accept/decline projects
- [ ] Can check-in to project
- [ ] Can check-out from project
- [ ] Points calculated correctly
- [ ] Tier updated correctly
- [ ] Profile shows correct stats
- [ ] Can view & download certificates
- [ ] Notifications received
- [ ] Settings work

QUALITY:
- [ ] No crashes on any screen
- [ ] All tests passing
- [ ] Code review approved
- [ ] >80% test coverage
- [ ] Performance acceptable
- [ ] App size <80 MB
- [ ] Memory usage reasonable

DOCUMENTATION:
- [ ] User guide written
- [ ] API integration documented
- [ ] Architecture documented
- [ ] All modules documented

USER TESTING:
- [ ] Can volunteer use app independently?
- [ ] Is it intuitive?
- [ ] Are there bugs?
- [ ] Is it fast enough?
- [ ] Is it stable?
```

**Sign-off Required From:**
- Mobile Lead Developer
- QA Lead
- Solution Architect
- Product Owner

---

## 5. PHASE 3: WEB APP CORE (Weeks 9-14)

### Objective
Build web app for club presidents to create projects, manage volunteers, and assign marks.

### Why Sequential?
- Backend foundation complete
- Clear API contracts established
- Mobile team finished (no conflicts)
- Can focus entirely on web

### Web App Modules (Sequential within Phase 3)

#### **W1: Web Auth & Dashboard**

**What to Build:**
```
Web authentication:
├─ Login page
├─ Remember me
├─ Reset password
├─ Role-based redirect

Club president dashboard:
├─ Welcome message
├─ Quick stats
│  ├─ My clubs
│  ├─ Projects created
│  ├─ Volunteers managed
│  └─ Pending approvals
├─ Recent projects widget
├─ Pending tasks widget
├─ Sidebar navigation
│  ├─ Dashboard
│  ├─ Projects
│  ├─ Volunteers
│  ├─ Team
│  ├─ Reports
│  ├─ Settings
│  └─ Logout
└─ Responsive layout
```

**Timeline:** 3-4 days

---

#### **W2: Club Management**

**What to Build:**
```
Club management interface:
├─ View my club(s)
├─ Edit club information
├─ Manage team members
│  ├─ Add team member
│  ├─ Assign roles
│  ├─ Remove member
│  ├─ View activity
│  └─ Manage permissions
├─ Club statistics
├─ Club volunteers list
└─ Club settings
```

**Timeline:** 3-4 days

---

#### **W3: Project Creation & Management**

**What to Build:**
```
Project creation form:
├─ Project name
├─ Description
├─ Date & time
├─ Location (map selector)
├─ Volunteers needed
├─ Project type
├─ Budget
├─ OC roles needed
├─ Success criteria
├─ Save as draft
├─ Submit for approval

Project list:
├─ All club projects
├─ Filter by status (draft, submitted, approved, completed)
├─ Search
├─ Edit draft projects
├─ View project details
└─ Complete project

Project detail view:
├─ All project info
├─ Volunteers signed up
├─ Check-in records
├─ Marks assigned
├─ Approval status
└─ Edit/Submit buttons
```

**Timeline:** 4-5 days

---

#### **W4: Volunteer Management**

**What to Build:**
```
Volunteer directory:
├─ List all club volunteers
├─ Search by name/email
├─ Filter by status (active, inactive, banned)
├─ Sort by points, hours, joined date
├─ View volunteer profile
│  ├─ Basic info
│  ├─ Activity history
│  ├─ Points & tier
│  ├─ Projects completed
│  ├─ Current status
│  └─ Ban/unban button (if authority)
└─ Bulk actions
   ├─ Send message to multiple
   └─ Export list

Volunteer detail page:
├─ Profile information
├─ Achievement history
├─ Projects participated
├─ Check-in records
├─ Points breakdown
├─ Activity timeline
└─ Actions (assign role, ban, etc)
```

**Timeline:** 3-4 days

---

#### **W5: Mark Assignment**

**What to Build:**
```
Mark assignment interface:

For a project:
├─ View all check-in records
├─ Verify attendance
├─ Assign OC marks (0-10 per volunteer)
├─ Apply success adjustments
├─ Approve marks
├─ Generate mark report

Mark adjustment:
├─ Override calculated marks
├─ Reason for adjustment
├─ Audit trail
└─ Approval workflow

Mark report:
├─ All marks assigned
├─ Filter by volunteer/project/status
├─ Export as CSV/PDF
└─ Print
```

**Timeline:** 3-4 days

---

#### **W6: Approval Workflow**

**What to Build:**
```
For club presidents:
├─ Submit project for approval
├─ View submission status
├─ See feedback from reviewers
├─ Re-submit after rejection

For district directors:
├─ View pending projects
├─ Review project details
├─ Approve/reject with feedback
├─ Request changes
└─ Track approval chain
```

**Timeline:** 3-4 days

---

#### **W7: Basic Reporting**

**What to Build:**
```
Club reports:
├─ Volunteer performance report
├─ Project performance report
├─ Monthly summary report
├─ Points distribution
├─ Activity by volunteer
└─ Export as PDF/CSV

Report features:
├─ Date range selector
├─ Filter options
├─ Chart/graph display
└─ Print-friendly view
```

**Timeline:** 3-4 days

---

### Phase 3 Validation Gate

Before moving to Phase 4:

```
WEB APP CORE CHECKLIST:
- [ ] Club president can log in
- [ ] Dashboard loads correctly
- [ ] Can create new project
- [ ] Can submit project
- [ ] Can manage team members
- [ ] Can view all volunteers
- [ ] Can assign marks
- [ ] Can view basic reports
- [ ] All tests passing
- [ ] No crashes
- [ ] Code reviewed
- [ ] Deployed to staging
- [ ] Smoke tested
```

---

## 6. PHASE 4: WEB APP ADVANCED (Weeks 15-20)

### Objective
Build advanced features for district directors, analytics, integrations, and admin functions.

### Web App Modules (Sequential)

#### **W8: District Director Views**

**What to Build:**
```
District director dashboard:
├─ All clubs overview
├─ Approval queue
├─ Volunteer statistics (district-wide)
├─ Performance comparison
└─ District reports

Advanced filtering & search
```

**Timeline:** 3-4 days

---

#### **W9: Analytics & Reporting (Advanced)**

**What to Build:**
```
Advanced analytics:
├─ Charts & graphs
├─ Trend analysis
├─ Geographic distribution
├─ Environmental impact metrics
├─ Volunteer leaderboards
├─ Comparison reports
└─ Custom report builder
```

**Timeline:** 4-5 days

---

#### **W10: Complaint Management**

**What to Build:**
```
Complaint system:
├─ File complaint
├─ View complaints (admin)
├─ Investigate
├─ Resolve
├─ Ban volunteer
├─ Appeal process
└─ Audit trail
```

**Timeline:** 3-4 days

---

#### **W11: Inventory Management**

**What to Build:**
```
Inventory system:
├─ Track equipment
├─ Check-out equipment
├─ Check-in equipment
├─ Maintenance tracking
└─ Inventory reports
```

**Timeline:** 2-3 days

---

#### **W12: CRM & Integrations**

**What to Build:**
```
CRM functions:
├─ Partner tracking
├─ Communication history
├─ Email volunteers
├─ SMS volunteers
└─ Engagement tracking

Integrations:
├─ Monday.com board
├─ Make.com automation
├─ Twilio SMS/WhatsApp
└─ Email notifications
```

**Timeline:** 4-5 days

---

#### **W13: Admin Functions**

**What to Build:**
```
Admin panel:
├─ User management
├─ Create/delete users
├─ Assign roles
├─ Reset passwords
├─ System configuration
├─ Audit logs
└─ Backup management
```

**Timeline:** 2-3 days

---

### Phase 4 Validation Gate

```
WEB APP COMPLETE CHECKLIST:
- [ ] All features working
- [ ] All tests passing
- [ ] No critical bugs
- [ ] Performance acceptable
- [ ] Security reviewed
- [ ] Scalability verified
- [ ] Documentation complete
- [ ] User training ready
- [ ] Deployed to production
- [ ] Smoke tested
- [ ] User acceptance testing passed
```

---

## 7. MODULE DEPENDENCIES MATRIX

```
BACKEND DEPENDENCIES:
├─ M0.1 (Database)
│  └─ Required by: All other modules
│
├─ M0.2 (Auth)
│  └─ Required by: M0.3, M0.4, M0.5, All mobile, All web
│
├─ M0.3 (User Service)
│  └─ Required by: M0.5, M1.4, M1.8, Web modules
│
├─ M0.4 (Projects/Clubs)
│  └─ Required by: M0.5, M1.5, M1.6, M1.7, Web modules
│
└─ M0.5 (API Routes)
   └─ Required by: All frontend modules

MOBILE DEPENDENCIES:
├─ M1.1 (Setup)
│  └─ Required by: All mobile modules
│
├─ M1.2 (Auth UI)
│  └─ M1.3 (Auth API)
│     └─ M1.4 (Home)
│        ├─ M1.5 (Discovery)
│        ├─ M1.6 (Detail)
│        └─ M1.7 (Check-in)
│
├─ M1.8 (Profile)
│  ├─ M1.9 (Certificates)
│  └─ M1.11 (Settings)
│
└─ M1.10 (Notifications)
   └─ Can be parallel

WEB DEPENDENCIES:
├─ W1 (Auth)
│  └─ Required by: All web modules
│
├─ W2 (Club Mgmt)
│  └─ W3 (Projects)
│     ├─ W4 (Volunteers)
│     ├─ W5 (Marks)
│     └─ W6 (Approvals)
│
└─ W7 (Reporting)
   └─ Depends on W3, W4, W5
```

---

## 8. VALIDATION & TESTING STRATEGY

### Test Types by Phase

#### **Phase 1 (Backend)**
```
UNIT TESTS (>80% coverage):
├─ All services
├─ All repositories
├─ All business logic
├─ All error handling
└─ Validation rules

INTEGRATION TESTS:
├─ Auth flow (register → login)
├─ CRUD operations
├─ Relationship handling
├─ Error scenarios
└─ API endpoints

MANUAL TESTING (Postman):
├─ All endpoints
├─ Valid inputs
├─ Invalid inputs
├─ Error cases
├─ Rate limiting
└─ Performance

DATABASE TESTING:
├─ Schema integrity
├─ Migrations working
├─ Performance queries
├─ Index effectiveness
└─ Data consistency
```

#### **Phase 2 (Mobile)**
```
UNIT TESTS:
├─ Services
├─ Providers (state management)
├─ Business logic
├─ Validators
└─ Utilities

WIDGET TESTS:
├─ Screen rendering
├─ Input handling
├─ Navigation
├─ Error displays
└─ Loading states

INTEGRATION TESTS:
├─ API calls
├─ State management
├─ Navigation flows
├─ Offline mode
└─ Token management

MANUAL TESTING:
├─ Full user journeys
├─ GPS functionality
├─ Notifications
├─ Different devices
├─ Different OS versions
└─ Network conditions (WiFi, 4G, offline)

PERFORMANCE TESTING:
├─ App launch time
├─ Screen load time
├─ Memory usage
├─ Battery drain
└─ Data usage
```

#### **Phase 3 & 4 (Web)**
```
UNIT TESTS:
├─ Components
├─ Hooks
├─ Utils
├─ Services
└─ State management

COMPONENT TESTS:
├─ Component rendering
├─ User interactions
├─ Form submission
├─ Error handling
└─ Loading states

INTEGRATION TESTS:
├─ Full workflows
├─ API integration
├─ Navigation
├─ Permission checks
└─ State management

MANUAL TESTING:
├─ All features
├─ All browsers (Chrome, Firefox, Safari, Edge)
├─ Responsive design
├─ Accessibility (keyboard, screen reader)
├─ Performance
└─ Edge cases

E2E TESTS (Cypress/Playwright):
├─ Critical user journeys
├─ Cross-feature flows
├─ Error recovery
└─ Permission scenarios
```

### Validation Gates

Each module must pass before moving forward:

```
READINESS CHECKLIST:
1. Code Complete
   - [ ] All functionality implemented
   - [ ] No TODOs or FIXMEs
   - [ ] Code follows style guide
   
2. Testing Complete
   - [ ] Unit tests written
   - [ ] Integration tests written
   - [ ] Manual testing done
   - [ ] All tests passing
   - [ ] Code coverage >80%
   
3. Code Review
   - [ ] Peer review completed
   - [ ] Architect review completed
   - [ ] All feedback addressed
   - [ ] No blocking issues
   
4. Documentation
   - [ ] Code documented
   - [ ] API documented
   - [ ] Architecture documented
   - [ ] Deployment documented
   
5. Performance
   - [ ] Load testing done
   - [ ] Performance acceptable
   - [ ] No memory leaks
   - [ ] Caching configured
   
6. Security
   - [ ] Security review done
   - [ ] Input validation
   - [ ] Authentication/Authorization
   - [ ] HTTPS/SSL configured
   
7. Deployment
   - [ ] Deployed to staging
   - [ ] Smoke tests passing
   - [ ] Ready for production
```

---

## 9. RISK MITIGATION

### High-Risk Areas

#### **Risk 1: Backend Schema Needs Major Changes**

**Impact:** HIGH - affects all downstream modules  
**Probability:** MEDIUM

**Mitigation:**
- [ ] Spend extra time on schema design (add 1 day)
- [ ] Use Prisma migrations (safe rollback)
- [ ] Create comprehensive schema docs
- [ ] Get schema review from architect
- [ ] Never push breaking migrations

**If It Happens:**
1. Stop mobile/web development
2. Create migration script
3. Validate on staging
4. Update all documentation
5. Brief development team
6. Resume development

---

#### **Risk 2: GPS Functionality Fails on Production**

**Impact:** HIGH - core mobile feature  
**Probability:** LOW-MEDIUM

**Mitigation:**
- [ ] Test GPS on real devices (iOS + Android)
- [ ] Test with different location services
- [ ] Test with permissions denied
- [ ] Test in airplane mode
- [ ] Test with different accuracy levels
- [ ] Have fallback without GPS

**If It Happens:**
1. Implement fallback (manual location entry)
2. Use geofence API (less accurate)
3. Allow manual verification
4. Deploy hotfix immediately

---

#### **Risk 3: API Performance Degrades with Load**

**Impact:** MEDIUM - affects user experience  
**Probability:** MEDIUM

**Mitigation:**
- [ ] Load test every phase
- [ ] Add database indexes
- [ ] Implement caching (Redis)
- [ ] Add query optimization
- [ ] Monitor API response times
- [ ] Set performance budgets

**If It Happens:**
1. Identify slow endpoints
2. Profile database queries
3. Add caching
4. Optimize queries
5. Scale infrastructure if needed

---

#### **Risk 4: Authentication Token Issues**

**Impact:** HIGH - breaks everything  
**Probability:** LOW

**Mitigation:**
- [ ] Comprehensive auth testing
- [ ] Test token expiry
- [ ] Test token refresh
- [ ] Test invalid tokens
- [ ] Test with expired tokens
- [ ] Use standard JWT practices

**If It Happens:**
1. Immediately alert all users
2. Force token refresh
3. Clear invalid tokens
4. Investigate root cause
5. Deploy fix immediately

---

#### **Risk 5: Feature Scope Creep**

**Impact:** HIGH - delays all phases  
**Probability:** HIGH

**Mitigation:**
- [ ] Freeze scope during phase
- [ ] Use validation gates
- [ ] No new features mid-phase
- [ ] Track change requests
- [ ] Plan post-MVP features separately

**If It Happens:**
1. Document new request
2. Assess impact
3. Update timeline
4. Get stakeholder approval
5. Integrate into plan

---

### Early Warning Signs

Monitor these indicators:

```
TECHNICAL RED FLAGS:
├─ Tests failing more frequently
├─ Code review cycles increasing
├─ Comments about "it's hacky"
├─ Performance issues mentioned
├─ API response times increasing
└─ Deployment failures

TEAM RED FLAGS:
├─ Developer burnout signals
├─ Frequent context switching
├─ Knowledge silos forming
├─ Documentation falling behind
├─ Communication breaking down
└─ Questions about architecture

PROJECT RED FLAGS:
├─ Module timeline slipping
├─ Validation gates failing repeatedly
├─ Bug count increasing
├─ Rework required
├─ Stakeholder concerns
└─ Scope creeping
```

**Action:** If you see 3+ red flags, stop and reassess.

---

## 10. CODE ORGANIZATION

### Backend Structure

```
zero-plastic-backend/
├── src/
│   ├── controllers/          # Express route handlers
│   │   ├── authController.ts
│   │   ├── userController.ts
│   │   ├── projectController.ts
│   │   ├── checkInController.ts
│   │   ├── marksController.ts
│   │   ├── reportController.ts
│   │   └── ...
│   │
│   ├── services/             # Business logic
│   │   ├── authService.ts
│   │   ├── userService.ts
│   │   ├── projectService.ts
│   │   ├── checkInService.ts
│   │   ├── marksService.ts
│   │   ├── notificationService.ts
│   │   └── ...
│   │
│   ├── repositories/         # Data access layer
│   │   ├── userRepository.ts
│   │   ├── projectRepository.ts
│   │   ├── checkInRepository.ts
│   │   ├── marksRepository.ts
│   │   └── ...
│   │
│   ├── models/              # Data models (Prisma)
│   │   ├── User.ts
│   │   ├── Project.ts
│   │   ├── CheckIn.ts
│   │   ├── Marks.ts
│   │   └── schema.prisma    # Main schema
│   │
│   ├── middleware/          # Express middleware
│   │   ├── authMiddleware.ts
│   │   ├── errorHandler.ts
│   │   ├── requestLogger.ts
│   │   └── validation.ts
│   │
│   ├── utils/               # Utility functions
│   │   ├── validators.ts
│   │   ├── formatters.ts
│   │   ├── errors.ts
│   │   └── logger.ts
│   │
│   ├── routes/              # API routes
│   │   ├── authRoutes.ts
│   │   ├── userRoutes.ts
│   │   ├── projectRoutes.ts
│   │   ├── checkInRoutes.ts
│   │   ├── marksRoutes.ts
│   │   ├── reportRoutes.ts
│   │   └── index.ts
│   │
│   ├── migrations/          # Database migrations
│   │   ├── 001_init.sql
│   │   ├── 002_auth.sql
│   │   └── ...
│   │
│   ├── config/              # Configuration
│   │   ├── database.ts
│   │   ├── auth.ts
│   │   ├── external.ts
│   │   └── index.ts
│   │
│   ├── tests/               # Tests
│   │   ├── unit/
│   │   ├── integration/
│   │   └── fixtures/
│   │
│   └── app.ts               # Express app setup
│
├── package.json
├── tsconfig.json
├── .env.example
├── README.md
└── docker-compose.yml
```

### Mobile Structure

```
zero_plastic_volunteer/
├── lib/
│   ├── main.dart            # App entry point
│   │
│   ├── models/              # Data models
│   │   ├── user.dart
│   │   ├── project.dart
│   │   ├── check_in.dart
│   │   ├── badge.dart
│   │   └── certificate.dart
│   │
│   ├── providers/           # Riverpod state management
│   │   ├── auth_provider.dart
│   │   ├── user_provider.dart
│   │   ├── project_provider.dart
│   │   ├── location_provider.dart
│   │   └── notification_provider.dart
│   │
│   ├── services/            # Business logic
│   │   ├── api_service.dart
│   │   ├── auth_service.dart
│   │   ├── location_service.dart
│   │   ├── notification_service.dart
│   │   └── storage_service.dart
│   │
│   ├── screens/             # UI screens
│   │   ├── auth/
│   │   │   ├── login_screen.dart
│   │   │   ├── register_screen.dart
│   │   │   └── password_reset_screen.dart
│   │   ├── home/
│   │   │   └── home_screen.dart
│   │   ├── projects/
│   │   │   ├── projects_map_screen.dart
│   │   │   ├── projects_list_screen.dart
│   │   │   ├── project_detail_screen.dart
│   │   │   ├── check_in_screen.dart
│   │   │   └── check_out_screen.dart
│   │   ├── profile/
│   │   │   ├── profile_screen.dart
│   │   │   ├── edit_profile_screen.dart
│   │   │   ├── achievements_screen.dart
│   │   │   └── certificates_screen.dart
│   │   ├── notifications/
│   │   │   └── notifications_screen.dart
│   │   ├── settings/
│   │   │   └── settings_screen.dart
│   │   └── widgets/         # Reusable widgets
│   │       ├── project_card.dart
│   │       ├── badge_widget.dart
│   │       ├── loading_indicator.dart
│   │       └── error_dialog.dart
│   │
│   ├── theme/               # App theming
│   │   ├── colors.dart
│   │   ├── text_styles.dart
│   │   └── theme.dart
│   │
│   ├── utils/               # Utilities
│   │   ├── validators.dart
│   │   ├── formatters.dart
│   │   ├── constants.dart
│   │   └── logger.dart
│   │
│   └── config/              # Configuration
│       ├── api_config.dart
│       ├── routes.dart
│       └── firebase_config.dart
│
├── test/                    # Tests
│   ├── unit/
│   ├── widget/
│   └── integration/
│
├── pubspec.yaml
├── .env.example
├── analysis_options.yaml
└── README.md
```

### Web Structure

```
zero-plastic-web/
├── src/
│   ├── main.jsx             # React entry point
│   │
│   ├── pages/               # Page components
│   │   ├── auth/
│   │   │   └── LoginPage.jsx
│   │   ├── dashboard/
│   │   │   ├── DashboardPage.jsx
│   │   │   ├── ClubDashboard.jsx
│   │   │   └── DirectorDashboard.jsx
│   │   ├── projects/
│   │   │   ├── ProjectListPage.jsx
│   │   │   ├── CreateProjectPage.jsx
│   │   │   ├── ProjectDetailPage.jsx
│   │   │   └── ApprovalQueuePage.jsx
│   │   ├── volunteers/
│   │   │   ├── VolunteerListPage.jsx
│   │   │   └── VolunteerDetailPage.jsx
│   │   ├── marks/
│   │   │   └── MarkAssignmentPage.jsx
│   │   ├── reports/
│   │   │   ├── ReportsPage.jsx
│   │   │   └── AnalyticsPage.jsx
│   │   └── settings/
│   │       └── SettingsPage.jsx
│   │
│   ├── components/          # Reusable components
│   │   ├── common/
│   │   │   ├── Sidebar.jsx
│   │   │   ├── Header.jsx
│   │   │   ├── Footer.jsx
│   │   │   └── LoadingSpinner.jsx
│   │   ├── forms/
│   │   │   ├── ProjectForm.jsx
│   │   │   ├── UserForm.jsx
│   │   │   └── FilterForm.jsx
│   │   ├── tables/
│   │   │   ├── VolunteerTable.jsx
│   │   │   ├── ProjectTable.jsx
│   │   │   └── MarksTable.jsx
│   │   ├── charts/
│   │   │   ├── BarChart.jsx
│   │   │   ├── LineChart.jsx
│   │   │   └── PieChart.jsx
│   │   └── modals/
│   │       ├── ConfirmDialog.jsx
│   │       └── DetailModal.jsx
│   │
│   ├── hooks/               # Custom React hooks
│   │   ├── useAuth.js
│   │   ├── useFetch.js
│   │   ├── useForm.js
│   │   └── useLocalStorage.js
│   │
│   ├── services/            # API services
│   │   ├── authService.js
│   │   ├── userService.js
│   │   ├── projectService.js
│   │   ├── marksService.js
│   │   ├── reportService.js
│   │   └── apiClient.js
│   │
│   ├── stores/              # State management (Zustand)
│   │   ├── authStore.js
│   │   ├── projectStore.js
│   │   ├── userStore.js
│   │   └── uiStore.js
│   │
│   ├── utils/               # Utilities
│   │   ├── validators.js
│   │   ├── formatters.js
│   │   ├── constants.js
│   │   └── errorHandler.js
│   │
│   ├── styles/              # Tailwind config
│   │   ├── tailwind.config.js
│   │   └── globals.css
│   │
│   ├── types/               # TypeScript types
│   │   ├── User.ts
│   │   ├── Project.ts
│   │   ├── Marks.ts
│   │   └── Reports.ts
│   │
│   └── App.jsx              # Root component
│
├── public/
├── tests/                   # Tests
│   ├── unit/
│   ├── integration/
│   └── e2e/
├── vite.config.js
├── package.json
├── .env.example
├── tailwind.config.js
└── README.md
```

---

## IMPLEMENTATION TIMELINE SUMMARY

```
TOTAL PROJECT: 20 WEEKS (5 months)

PHASE 1: BACKEND FOUNDATION
Week 1-2: Database, Auth, Services, API
├─ Done: All backend ready
├─ Validation: All tests pass, API documented
└─ Status: ✅ COMPLETE

PHASE 2: MOBILE MVP
Week 3-8: Auth, Discovery, Check-in, Profile, Notifications
├─ Done: Volunteers can use app independently
├─ Validation: All features working, manual testing complete
└─ Status: ✅ COMPLETE

PHASE 3: WEB APP CORE
Week 9-14: Auth, Club Mgmt, Projects, Approvals, Marks, Reports
├─ Done: Club presidents can manage projects
├─ Validation: All workflows tested, no critical bugs
└─ Status: ✅ COMPLETE

PHASE 4: WEB APP ADVANCED
Week 15-20: District views, Analytics, Complaints, Admin, Integrations
├─ Done: Full platform capability
├─ Validation: All features complete, production ready
└─ Status: ✅ COMPLETE

BUFFER: Built-in 10% for unexpected issues
```

---

## QUICK START CHECKLIST

```
BEFORE STARTING PHASE 1:
- [ ] Team assembled
- [ ] Development environment set up
- [ ] GitHub repository created
- [ ] Azure account active
- [ ] Database instance created
- [ ] Development tools installed
- [ ] Postman/API client ready
- [ ] This roadmap understood by all
- [ ] Weekly standup scheduled
- [ ] Escalation path defined

START PHASE 1:
1. Clone backend repo
2. Install dependencies
3. Create database
4. Run migrations
5. Build services
6. Test everything
7. Validate & sign-off
8. Celebrate! 🎉

Then → Move to Phase 2
```

---

## PHILOSOPHY & PRINCIPLES (REITERATED)

```
"SLOW AND STEADY WINS THE RACE"

✅ DO:
├─ Complete one module 100%
├─ Test thoroughly before moving on
├─ Document as you go
├─ Have validation gates
├─ Get sign-off before proceeding
├─ Build stability first
├─ Minimize rework
├─ Communicate progress
└─ Celebrate small wins

❌ DON'T:
├─ Skip testing
├─ Move on before validation
├─ Accumulate technical debt
├─ Work on multiple modules simultaneously
├─ Ignore bugs "for later"
├─ Over-promise timelines
├─ Skip documentation
├─ Have scope creep
└─ Work in isolation
```

---

## MEASURING SUCCESS

At the end of each phase, measure:

```
QUALITY:
├─ Test coverage >80%
├─ No critical bugs
├─ Code review score
├─ Technical debt ratio
└─ Performance metrics

DELIVERY:
├─ On-time completion
├─ Scope adherence
├─ Rework required
├─ Deployment success
└─ User acceptance

TEAM:
├─ Team morale
├─ Knowledge sharing
├─ Documentation quality
├─ Collaboration effectiveness
└─ Learning captured
```

---

## NEXT STEPS

1. **Share this roadmap** with your entire team
2. **Get sign-off** from all stakeholders
3. **Establish validation gates** and sign-off process
4. **Set up weekly standup** to track progress
5. **Start Phase 1 Week 1** with database design
6. **Commit to the process** - no cutting corners

---

**REMEMBER:**

> "Better to take 20 weeks building something rock-solid that doesn't need rework, than 12 weeks building something broken that takes 2 months to fix."

> "Test one module completely before moving to the next. That's how you minimize rework."

---

**Document Status:** ✅ COMPLETE & READY TO IMPLEMENT

**Use this roadmap as your north star. Follow it. Don't deviate.**

---

**Let's build Zero Plastic right. Step by step. Module by module. 🌍💚**


---

## 11. PROPOSED ENGAGEMENT V2 ADDENDUM (2026-11-02)

**Status: founder-review proposal; not yet implemented.** Detailed rules, exceptions and acceptance checks live in [docs/VOLUNTEER-ENGAGEMENT-GEMS-CERTIFICATES-V2.md](docs/VOLUNTEER-ENGAGEMENT-GEMS-CERTIFICATES-V2.md).

### Keep two interfaces with separate responsibilities
- **Volunteer Flutter app:** find and join projects; check in/out; see existing marks/ranks; submit Gem activities and evidence, confirm teams, review awards; self-serve PDF certificates.
- **React web management:** national/district/chapter operations, project setup, attendance verification, Gem catalogue configuration, chapter-level Gem approvals, and certificate template/exception controls.
- **Shared Express/PostgreSQL backend:** access control, claims/ledger, attendance, verified marks, certificate generation and QR verification.

### New proposed vertical slices
1. **Chapter-scoped roles and actual participation data** before awarding Gems or certificates.
2. **Gems** (working name: Kindness Gems): a separate, evidence-backed positive-action recognition track, distinct from formal volunteer marks and rank. University club executive committee makes the **only normal approval** on its web dashboard. Max three confirmed claimants, splitting a fixed Gem pool. Photos required for ordinary claims, with consent-safe exceptions.
3. **500-point self-service volunteer e-certificate:** volunteers request/download automatically from the app once existing verified volunteer marks reach 500. PDF states service period (first to last verified attended project), number of distinct verified attended projects, unique serial and verifiable QR. No printing, routine manual approval or Gems-based threshold substitution.
4. **Engagement UX polish:** optional Gem collections and milestones, suitable for youth without implying low Gem balances mean a person has poor moral character.

### Revised delivery sequencing
Backend roles/chapters + verified attendance and marks → minimal **web operational workflow** (create/approve project, verify attendance) → volunteer mobile MVP → Gem catalogue/web approvals/app claims → automatic certificate pipeline → optional enhanced engagement UI.

This is a sequencing adjustment to enable administrators to publish actual activities before volunteers can participate. Do not assume the 20-week estimates or the old enhanced-schema document represent implemented modules; inspect the real code and current Prisma schema.
