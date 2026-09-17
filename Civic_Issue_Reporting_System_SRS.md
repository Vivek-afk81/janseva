# Software Requirement Specification (SRS)
## Civic Issue Reporting System
**Format:** IEEE 830 Standard
**Version:** 1.0
**Date:** September 2026

---

## 1. Introduction

### 1.1 Purpose
This document specifies the software requirements for the **Civic Issue Reporting System**, a web-based platform that allows citizens to report civic/municipal issues (potholes, garbage, broken streetlights, water leakage, etc.), track their resolution status, and enables municipal authorities (Engineers, Supervisors) to manage, prioritize, and resolve these issues efficiently. This SRS is intended for developers, project evaluators, and municipal stakeholders.

### 1.2 Scope
The system is a single-page web application (SPA) built on React that operates in both online and offline modes. Citizens can submit issues with images and location data even without internet connectivity; submissions are queued locally and synced automatically once connectivity is restored. The platform uses AI (Google Gemini) to automatically assess and assign a priority level to reported issues, reducing manual triage effort for municipal staff. The system includes map-based visualization, role-based dashboards, and real-time status tracking.

**In scope:**
- Citizen issue reporting (with images, geolocation, offline queuing)
- Role-based dashboards for Citizen, Engineer, and Supervisor
- AI-based priority classification
- Map-based issue visualization
- Real-time issue status updates

**Out of scope (current version):**
- Native mobile applications (iOS/Android)
- Payment/billing integration
- Multi-language localization

### 1.3 Definitions, Acronyms, and Abbreviations
| Term | Definition |
|---|---|
| SPA | Single Page Application |
| SRS | Software Requirement Specification |
| RBAC | Role-Based Access Control |
| IndexedDB | Browser-native NoSQL storage used for offline persistence |
| Firestore | Firebase's NoSQL cloud database |
| Gemini | Google's Generative AI model used for priority prediction |
| CRUD | Create, Read, Update, Delete |

### 1.4 References
- IEEE Std 830-1998, Recommended Practice for Software Requirements Specifications
- Firebase Documentation (Authentication, Firestore)
- Supabase Documentation (Storage, PostgreSQL)
- Google Generative AI (Gemini API) Documentation

### 1.5 Overview
Section 2 describes the product's overall context and constraints. Section 3 lists functional requirements. Section 4 lists non-functional requirements. Section 5 describes external interfaces. Section 6 lists other (legal/compliance) requirements. Section 7 contains appendices with the system architecture and data flow.

---

## 2. Overall Description

### 2.1 Product Perspective
The Civic Issue Reporting System is a new, standalone, client-heavy web application. It follows a **client-centric, BaaS (Backend-as-a-Service) architecture** rather than a traditional custom backend server — most backend responsibilities (auth, database, file storage) are delegated to managed cloud services (Firebase, Supabase), with the React client orchestrating calls to these services and to the Gemini AI API.

**High-level architecture layers:**

1. **Presentation Layer** — React 18 SPA (Vite build tooling), React Router for navigation, React Context for global state (auth/session, issue state), React Hook Form for form validation.
2. **Offline/Resilience Layer** — IndexedDB stores issues created while offline; a background Sync Service detects connectivity restoration and pushes queued records to Firestore/Supabase.
3. **Service/Integration Layer** — Axios-based API clients wrapping Firebase SDK, Supabase client, and the Gemini AI SDK.
4. **Backend Services (Managed/Cloud):**
   - **Firebase Authentication** — identity and session management, RBAC claims.
   - **Firebase Firestore** — primary NoSQL datastore for users, issues, status history.
   - **Supabase (PostgreSQL + Storage)** — object storage for issue images (`issue-images` bucket).
   - **Google Gemini API** — receives issue description/category/image metadata and returns a predicted priority score/label.
5. **Presentation Aids** — Leaflet/React-Leaflet for map rendering, Recharts for analytics/dashboards, Lucide React for icons.

### 2.2 Product Functions (Summary)
- Report a new civic issue (with photo, category, description, geolocation)
- Queue and auto-sync issue reports made offline
- View and track "My Issues" with live status
- AI-based automatic priority tagging of new issues
- Assign issues to Engineers (Supervisor role)
- Update issue status and resolution notes (Engineer role)
- Visualize all issues on an interactive map
- View issue details, including image and history
- Role-based dashboards and access control

### 2.3 User Classes and Characteristics
| Role | Description | Key Permissions |
|---|---|---|
| **Citizen** | General public reporting issues | Create issue, view own issues, view map, view issue details |
| **Engineer** | Field staff resolving issues | View assigned issues, update status, add resolution notes |
| **Supervisor** | Municipal authority overseeing operations | Assign issues to engineers, view all issues, view analytics, override priority |

### 2.4 Operating Environment
- **Client:** Any modern web browser (Chrome, Edge, Firefox) supporting IndexedDB and Service Workers, on desktop or mobile viewport.
- **Server/Backend:** Fully cloud-hosted — Firebase (Google Cloud) and Supabase (managed PostgreSQL); no self-hosted application server required beyond the Vite-built static frontend.
- **Development Environment:** Node.js 18+ LTS, npm, Vite dev server (`localhost:5173`).

### 2.5 Design and Implementation Constraints
- Must function without persistent internet connectivity for issue creation (offline-first requirement).
- Must use Firebase for authentication (project-level constraint).
- Image storage is on Supabase rather than Firebase Storage, requiring dual-service credential management.
- AI priority prediction depends on third-party Gemini API availability and quota limits.
- Environment secrets (Firebase keys, Supabase keys, Gemini API key) must be managed via `.env` and never committed to source control.

### 2.6 Assumptions and Dependencies
- Users have a modern browser with JavaScript and IndexedDB enabled.
- Firebase and Supabase projects are provisioned and correctly configured before deployment.
- The Gemini API key has sufficient quota for expected issue volume.
- Devices used for offline reporting will regain connectivity within a reasonable time window to sync data.

---

## 3. Functional Requirements

### FR-1: User Authentication
- FR-1.1: The system shall allow users to register and log in using email/password via Firebase Authentication.
- FR-1.2: The system shall assign a role (Citizen, Engineer, Supervisor) to each authenticated user.
- FR-1.3: The system shall restrict access to dashboards/actions based on the authenticated user's role (RBAC).

### FR-2: Issue Reporting
- FR-2.1: The system shall allow a Citizen to create a new issue report with title, description, category, image, and geolocation.
- FR-2.2: The system shall allow issue creation while offline, storing the record in IndexedDB.
- FR-2.3: The system shall automatically sync offline-queued issues to Firestore/Supabase once connectivity is restored.
- FR-2.4: The system shall upload issue images to the Supabase `issue-images` storage bucket.

### FR-3: AI Priority Assessment
- FR-3.1: On issue submission, the system shall send relevant issue data to the Gemini AI API to predict a priority level.
- FR-3.2: The system shall store the AI-predicted priority alongside the issue record.
- FR-3.3: A Supervisor shall be able to manually override the AI-assigned priority.

### FR-4: Issue Tracking and Status
- FR-4.1: The system shall allow a Citizen to view the real-time status of their submitted issues ("My Issues").
- FR-4.2: The system shall allow an Engineer to view issues assigned to them ("Assigned Issues").
- FR-4.3: The system shall allow an Engineer to update issue status (e.g., Open → In Progress → Resolved) and add notes.
- FR-4.4: The system shall reflect status changes in real time across all connected clients viewing that issue.

### FR-5: Issue Assignment
- FR-5.1: The system shall allow a Supervisor to assign an open issue to a specific Engineer.
- FR-5.2: The system shall notify (in-app) the assigned Engineer of a new assignment.

### FR-6: Map Visualization
- FR-6.1: The system shall display all reported issues as markers on an interactive map (Leaflet).
- FR-6.2: The system shall allow filtering map markers by status, category, or priority.

### FR-7: Issue Details
- FR-7.1: The system shall provide a detailed view of a single issue, including image, description, location, status history, and priority.

---

## 4. Non-Functional Requirements

### NFR-1: Performance
- The application shall load its initial view in under 3 seconds on a standard broadband connection.
- Map rendering with up to 500 markers shall not visibly degrade UI responsiveness.

### NFR-2: Reliability & Offline Availability
- Issue creation shall succeed and persist locally even with zero network connectivity.
- Queued offline issues shall not be lost if the browser is closed before sync completes (IndexedDB persistence).

### NFR-3: Security
- All Firebase/Supabase credentials shall be stored as environment variables, not hardcoded.
- Role-based access control shall be enforced both on the client (UI gating) and via Firestore/Supabase security rules (server-side enforcement).
- User passwords shall never be handled directly by the application (delegated to Firebase Auth).

### NFR-4: Scalability
- The system shall support a growing number of concurrent users via Firebase Firestore's managed horizontal scaling, without application-level changes.

### NFR-5: Usability
- The UI shall be responsive across desktop and mobile viewport sizes.
- Forms shall provide inline validation feedback (via React Hook Form) before submission.

### NFR-6: Maintainability
- The codebase shall separate concerns into distinct service modules (`firebase/config.js`, `supabase/config.js`) to isolate third-party integration logic.

### NFR-7: Availability
- The system's uptime is bound by the SLAs of Firebase, Supabase, and the Gemini API (managed cloud dependencies).

---

## 5. External Interface Requirements

### 5.1 User Interfaces
- Landing Page, Login Page, Citizen Dashboard, Report Issue form, My Issues list, Engineer's Assigned Issues view, Map View, Issue Details view (as captured in project screenshots).

### 5.2 Hardware Interfaces
- None beyond standard client device requirements (camera/gallery access for image upload, GPS/location services for geolocation).

### 5.3 Software Interfaces
| Interface | Purpose |
|---|---|
| Firebase Authentication API | User sign-up/login, session/token management |
| Firebase Firestore API | CRUD operations on users, issues, status records |
| Supabase Storage API | Upload/retrieve issue images |
| Google Generative AI (Gemini) API | Issue priority prediction |
| Leaflet / OpenStreetMap tiles | Map rendering |

### 5.4 Communication Interfaces
- HTTPS REST calls (via Axios and provider SDKs) to Firebase, Supabase, and Gemini endpoints.
- `VITE_API_BASE_URL` reserved for any custom backend API endpoints (e.g., `http://localhost:5000/api` in development).

---

## 6. Other Requirements

- **Legal/Compliance:** User-submitted images and location data should be handled per applicable data-privacy expectations; access to personal data restricted by role.
- **Licensing:** Third-party libraries (React, Leaflet, Recharts, etc.) are used under their respective open-source licenses.
- **Environment Configuration:** A `.env` file (excluded from version control) must define all Firebase, Supabase, and Gemini credentials before the application can run.

---

## 7. Appendices

### 7.1 System Architecture Diagram (Textual)

```
                ┌─────────────────────────────┐
                │        React 18 SPA          │
                │   (Vite, Router, Context)    │
                └───────────────┬───────────────┘
                                │
        ┌───────────────────────┼───────────────────────┐
        │                       │                       │
┌───────▼────────┐   ┌──────────▼──────────┐   ┌─────────▼─────────┐
│   IndexedDB     │   │   Axios Service      │   │  Leaflet / Recharts│
│ (Offline Queue) │   │      Layer            │   │  (Map / Analytics) │
└───────┬─────────┘   └──────────┬───────────┘   └────────────────────┘
        │  Sync Service          │
        │ (on reconnect)         │
        └───────────┬────────────┘
                     │
     ┌───────────────┼────────────────────┐
     │               │                    │
┌────▼─────┐  ┌───────▼────────┐  ┌────────▼─────────┐
│ Firebase │  │ Firebase        │  │ Supabase          │
│   Auth   │  │ Firestore (DB)  │  │ (PostgreSQL +     │
│          │  │                 │  │ Image Storage)     │
└──────────┘  └────────┬────────┘  └────────────────────┘
                        │
                ┌───────▼────────┐
                │ Google Gemini   │
                │  AI (Priority   │
                │   Prediction)   │
                └─────────────────┘
```

### 7.2 Data Flow — Issue Reporting (Online)
1. Citizen fills the Report Issue form (React Hook Form validates input).
2. Image uploaded to Supabase `issue-images` bucket → returns image URL.
3. Issue data + image URL sent to Gemini API for priority prediction.
4. Final issue record (with AI priority) written to Firestore.
5. Real-time Firestore listener updates "My Issues" view instantly.

### 7.3 Data Flow — Issue Reporting (Offline)
1. Citizen submits the form while offline.
2. Record (including image as blob/base64) is stored in IndexedDB.
3. Sync Service listens for a `navigator.onLine` / connectivity event.
4. On reconnect, queued records are uploaded in order: image → Supabase, metadata + AI priority → Firestore.
5. Local IndexedDB entry is cleared upon successful sync.

### 7.4 Role–Permission Matrix
| Action | Citizen | Engineer | Supervisor |
|---|:---:|:---:|:---:|
| Report issue | ✅ | ❌ | ❌ |
| View own issues | ✅ | — | — |
| View assigned issues | ❌ | ✅ | ✅ |
| Assign issue to engineer | ❌ | ❌ | ✅ |
| Update issue status | ❌ | ✅ | ✅ |
| Override AI priority | ❌ | ❌ | ✅ |
| View map / analytics | ✅ | ✅ | ✅ |

### 7.5 Technology Stack Summary
| Layer | Technology |
|---|---|
| Frontend Framework | React 18 + Vite |
| Routing | React Router |
| State Management | React Context |
| Forms | React Hook Form |
| HTTP Client | Axios |
| Authentication | Firebase Auth |
| Primary Database | Firebase Firestore |
| Image/File Storage | Supabase (PostgreSQL) |
| Offline Storage | IndexedDB |
| Maps | Leaflet / React-Leaflet |
| AI Priority Engine | Google Generative AI (Gemini) |
| Charts | Recharts |
| Icons | Lucide React |

### 7.6 Glossary
- **Offline-first:** A design approach where core functionality works without network access, syncing data once connectivity resumes.
- **Priority Prediction:** AI-assisted classification of issue urgency (e.g., Low/Medium/High/Critical) based on description and category.
