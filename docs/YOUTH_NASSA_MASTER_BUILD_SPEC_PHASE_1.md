# YOUTH NASSA MASTER BUILD SPECIFICATION — PHASE 1

> Audit date: 2026-10-03
> Repository audited: Tomeking/Youth-nassa
> Default branch inspected: main
> Audit status: Initial repository audit complete; external deployments and external platform copies still require verification.

## 1. Executive Summary

The repository is a public GitHub repository named `Tomeking/Youth-nassa`. The current repository is an Angular/Firebase scaffold rather than a completed Youth NASSA application.

The repository contains:
- Angular frontend under `frontend/`
- Firebase Hosting configuration
- Firestore configuration and rules
- Firebase Cloud Functions under `functions/`
- Static hosting content under `public/`

The current frontend uses Angular 21.2.x and TypeScript 5.9.x. Its route table is currently empty, and the root Angular component is still named `frontend`.

The current Firebase configuration targets Firestore in `nam5`, Firebase Hosting, and Cloud Functions using Node 24.

A critical security finding is present in `firestore.rules`: the only Firestore rule is an expired time-based development rule ending 2025-09-20. It therefore must not be treated as a production authorization model.

## 2. Verified Repository Facts

### Repository

- Repository: `Tomeking/Youth-nassa`
- Visibility: public
- Default branch: `main`
- Repository language reported by GitHub: HTML
- Repository is not archived or disabled.
- Repository contains one open issue according to the repository metadata.
- Latest reported push on GitHub metadata: 2026-06-16.

### Top-level structure

```
.
├── .firebaserc
├── .github/
├── .gitignore
├── SECURITY.md
├── firebase.json
├── firestore.indexes.json
├── firestore.rules
├── frontend/
├── functions/
├── package.json
└── public/
```

## 3. Frontend Audit

### Verified stack

- Angular: 21.2.x
- Angular CLI/build: 21.2.2
- TypeScript: 5.9.x
- RxJS: 7.8.x
- Vitest: 4.0.8
- npm package manager declaration: npm 11.11.1

### Current frontend structure

The Angular source currently contains:

```
frontend/src/
├── app/
│   ├── app.config.ts
│   ├── app.css
│   ├── app.html
│   ├── app.routes.ts
│   ├── app.spec.ts
│   └── app.ts
├── index.html
├── main.ts
└── styles.css
```

### Important finding

`app.routes.ts` currently exports an empty route array.

Therefore the repository does not currently contain a verified route architecture for:
- About
- Programs
- Projects
- Opportunities
- Applications
- AI Ambassador
- Command Center
- Administration
- WebXR/Sphere

Those routes must be designed rather than assumed to exist in this repository.

## 4. Backend Audit

### Firebase Cloud Functions

The `functions/package.json` confirms:

- Node runtime: 24
- firebase-admin: 13.6.0 range
- firebase-functions: 7.0.0 range
- ESLint-based linting
- Firebase Functions deployment scripts

The current `functions/index.js` only establishes global function options and imports the HTTP function infrastructure/logger. The previously scaffolded Hello World function is commented out.

### Conclusion

There is currently no verified production API/function layer in this repository.

## 5. Firebase / Firestore Audit

`firebase.json` configures:

- Firestore database: default
- Firestore location: `nam5`
- Firestore rules: `firestore.rules`
- Firestore indexes: `firestore.indexes.json`
- Cloud Functions source: `functions`
- Firebase Hosting public directory: `public`
- SPA rewrite: all requests to `/index.html`
- Firebase emulator single-project mode

### Critical security finding

Current `firestore.rules` contains only a development-style rule allowing read/write until:

`2025-09-20`

That date has passed.

This means the existing rule file is not an acceptable production authorization design and must be replaced before real user/application data is introduced.

Do not expose or recreate unrestricted read/write access.

## 6. Database Status

### Verified

A Firestore configuration exists.

### Not yet verified

No application-specific Firestore collections/schema were identified during this initial repository inspection.

The target logical model should therefore be designed deliberately rather than reverse-engineered from an existing application schema.

Candidate entities:

```
users
profiles
programs
projects
opportunities
applications
organizations
partners
events
media
documents
notifications
ai_conversations
activity_logs
analytics
```

These are proposed target entities, not verified existing collections.

## 7. 3D/WebXR Status

No verified Three.js, Babylon.js, WebXR, or other 3D engine dependency was found in the inspected frontend package manifest.

Therefore the 3D/WebXR Command Sphere should be treated as a planned subsystem, not an existing subsystem.

Recommended architectural boundary:

```
Angular application
        |
        +-- Standard 2D experience
        |
        +-- Command Sphere feature
                |
                +-- WebGL renderer
                +-- 3D scene
                +-- interaction layer
                +-- media surfaces
                +-- navigation bridge
                +-- accessibility fallback
```

The 2D application must remain functional when WebXR is unavailable.

## 8. AI Digital Ambassador Status

No verified AI-agent implementation was found in the inspected repository files.

The AI Digital Ambassador should therefore be designed as a future service boundary rather than embedded directly into the 3D renderer.

Target flow:

```
User
 ↓
AI interface
 ↓
Intent classification
 ↓
Approved knowledge retrieval
 ↓
Live application data retrieval where authorized
 ↓
Response generation
 ↓
Validation / guardrails
 ↓
User
```

The AI must distinguish verified Youth NASSA information from generated explanation and must not invent programme, funding, partnership, application, or statistical information.

## 9. Target Application Architecture

```
                    ┌──────────────────────┐
                    │       USER           │
                    └──────────┬───────────┘
                               │
                 ┌─────────────▼─────────────┐
                 │ Youth NASSA Web Application│
                 │        Angular             │
                 └─────────────┬─────────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
              ▼                ▼                ▼
        Standard UI      Command Sphere     AI Ambassador
                              │                │
                              └───────┬────────┘
                                      ▼
                              Application API
                                      │
                         ┌────────────┴────────────┐
                         ▼                         ▼
                    Data layer                Storage
                         │
                         ▼
                Firestore / approved
                backend architecture
                         │
                         ▼
                 Admin / Operations
```

This is a target architecture. The final backend choice between retaining Firebase and migrating to Supabase requires a separate migration assessment.

## 10. Content Architecture

Create canonical content domains:

- Mission
- Vision
- About
- Programmes
- Projects
- Opportunities
- Education
- Skills
- Entrepreneurship
- Investment
- Agriculture
- Events
- News
- Media
- Applications
- Partners
- Governance
- Policies
- Contact

Each content object should have:
- stable identifier
- title
- summary
- body/content
- status
- publication state
- owner
- created/updated timestamps
- optional media references
- audit metadata

## 11. Command Sphere Zones

The future WebXR/3D interface should expose:

1. Command Center
2. Projects
3. Education
4. Skills
5. Opportunities
6. Investment
7. AI Ambassador
8. Media
9. Events
10. Take Action

Each zone must map to a real application route or data service.

The 3D scene must not become the source of truth for business data.

## 12. Security Architecture

Before production data is introduced:

- Replace the expired Firestore rules.
- Define authenticated vs public data.
- Define role-based administrative access.
- Add server-side validation for privileged operations.
- Keep secrets outside source control.
- Audit Firebase configuration.
- Add structured activity/audit logging.
- Define backup and recovery procedures.
- Define least-privilege access.
- Test authorization rules before deployment.

## 13. Gap Analysis

### Existing and usable

- GitHub repository
- Angular application scaffold
- Firebase Hosting configuration
- Firestore configuration
- Firebase Functions project
- Firebase emulator configuration
- Basic frontend testing configuration
- Security policy file

### Existing but needs work

- Frontend architecture
- Route architecture
- Application identity/branding
- Firebase authorization rules
- Cloud Functions/API layer
- Database model
- Testing coverage
- Deployment pipeline
- Documentation
- Production security controls

### Missing / not verified

- Complete Youth NASSA route system
- User authentication implementation
- Application workflow
- Programme/project data layer
- Admin dashboard
- AI Digital Ambassador
- WebXR Command Sphere
- 3D engine integration
- Media management architecture
- Notification system
- Analytics architecture
- Production-grade authorization model

## 14. Priority Matrix

### P0 — Critical

1. Replace expired Firestore rules before introducing real data.
2. Establish authoritative backend/data ownership.
3. Establish secure authentication/authorization architecture.
4. Establish environment/secret management.
5. Establish a reproducible build/test/deployment path.

### P1 — Core

1. Define canonical data model.
2. Define application routes.
3. Build core Youth NASSA content/data services.
4. Implement authentication.
5. Implement applications/opportunities/projects.
6. Establish admin/operations foundation.

### P2 — Enhancement

1. AI Digital Ambassador.
2. Advanced analytics.
3. Notifications.
4. Rich media/content management.
5. Advanced user profiles.

### P3 — Experimental / immersive

1. Command Sphere.
2. WebGL scene.
3. WebXR support.
4. 3D media surfaces.
5. Spatial interaction.

## 15. Backend Decision Gate

Do not migrate from Firebase to Supabase merely because Supabase is part of the proposed target architecture.

First compare:

- current Firebase assets
- authentication requirements
- Firestore data needs
- Cloud Functions
- hosting
- storage
- RLS requirements
- operational complexity
- migration effort
- cost
- AI integration requirements
- long-term maintainability

Then produce a Firebase-vs-Supabase decision record.

## 16. Immediate Next Implementation Task

The single next task is:

**Build the Phase 1 application foundation and security/data specification before building the 3D sphere.**

That task consists of:

1. Define canonical route map.
2. Define target database schema.
3. Define authentication and roles.
4. Define public/private data boundaries.
5. Replace expired development Firestore authorization rules with a designed security model.
6. Define backend service boundaries.
7. Define environment configuration.
8. Define automated build/test checks.
9. Produce a Firebase-vs-Supabase migration decision record.

Only after those decisions are documented should the Command Sphere implementation begin.

## 17. External Verification Still Required

This repository audit does not by itself verify:

- currently deployed websites
- Base44 applications
- Lovable applications
- external Supabase projects
- external Firebase console configuration
- production database contents
- live authentication configuration
- existing external AI agents
- external media repositories

Those systems must be audited separately before declaring the repository the complete Youth NASSA source of truth.

## 18. Decision Log

### Verified

The GitHub repository currently contains an Angular 21.2.x frontend and Firebase infrastructure.

### Verified

The frontend route list is empty.

### Verified

The Cloud Functions source is still essentially a scaffold.

### Verified

The Firestore rules are an expired development rule and require replacement.

### Not verified

Whether another Youth NASSA deployment or repository currently contains a more complete application.

### Not verified

Whether Supabase is currently authoritative anywhere outside this repository.

## 19. Phase 1 Completion Gate

Phase 1 is complete when:

- authoritative source of truth is identified
- backend choice is documented
- schema is approved
- route map is approved
- authentication model is approved
- security rules are designed and tested
- deployment environments are defined
- external Youth NASSA implementations are reconciled
- Command Sphere specification is approved

Only then proceed to:

**PHASE 2 — CORE PLATFORM IMPLEMENTATION**

followed by:

**PHASE 3 — COMMAND SPHERE / WEBXR**

---

## Audit Principle

**Do not rebuild blindly.  
Do not migrate blindly.  
Do not add 3D before the application foundation is stable.  
Preserve useful existing work.  
Make every architectural decision traceable.**
