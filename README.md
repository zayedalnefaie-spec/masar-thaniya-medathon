# Masar Health + Thaniya — FlowCare

MEDATHON Filter 2 · Team MED-GWPYB3

- Live demo: https://masar-thaniya-flowcare.ai.studio/
- Submission repository: https://github.com/zayedalnefaie-spec/masar-thaniya-medathon

A software prototype for structured symptom screening, simulated facility routing, mass-casualty dispatch/handover and a command center. All cases, capacities, forecasts and impact figures are synthetic. It is not a diagnostic tool, clinically validated triage system or operational ambulance/hospital service.

## Submission files

Download and extract `MED-GWPYB3_FlowCare_FullStack.zip` for the current frontend and backend. The earlier `MED-GWPYB3_FlowCare_Source.zip` is a historical frontend-only snapshot. Use the Filter2 PDF for the completed presentation; the PPTX is editable.

Verification on 10 November 2026: five integration tests, TypeScript checks and production build passed locally. The public health endpoint returned Firestore connected, Anonymous Auth enabled and version 1.2.0. Technical verification is not clinical validation.

## Source layout

`src/` contains the React/TypeScript frontend, demo data and shared routing rules. `server/` contains the Express API and storage adapters. `tests/backend.test.ts` contains five HTTP integration tests. `firestore.rules` specifies owner isolation. `firebase-applet-config.json` is public Firebase client configuration, not an administrative credential.

## Run

Use Node.js 22.13+ (including `node:sqlite`) and npm.

```sh
npm install
npm run dev
```

The Express server starts on port 3000 and serves Vite in development. For production:

```sh
npm run build
npm start
```

Production defaults to Firestore. For your own deployment, configure your own Firebase project, enable Anonymous Authentication, create the named Firestore database, deploy `firestore.rules`, replace the public client configuration, and set `FIREBASE_PROJECT_ID`, `FIRESTORE_DATABASE_ID`, `FIREBASE_API_KEY`. No service-account JSON or broad shared-runtime IAM grant is required. Do not use the demonstration project's data for production.

Explicit local test storage:

```sh
STORAGE_BACKEND=sqlite npm run dev
```

SQLite is not an automatic production fallback. The bundled frontend still initializes Firebase Auth; local backend tests use isolated SQLite databases directly.

## Architecture and retention

The browser signs in anonymously with Firebase's persistent browser identity. Express verifies Firebase ID tokens cryptographically, then calls Firestore REST using the user's token. Rules restrict a workspace at `/demoWorkspaces/{uid}` to that authenticated owner. State updates use Firestore `updateTime` preconditions and reject stale revisions.

There is no automatic 24-hour record deletion. Clearing browser/site data, changing browser or using private browsing can lose access to an anonymous workspace. Permanent storage and uninterrupted hosting are not guaranteed. The historical 400-day cookie remains in the implementation, but Firebase browser identity determines production workspace access.

## Technical checks

```sh
npm test
npm run lint
npm run build
```

Tests cover isolated workspaces, high/low-acuity demo paths, stale writes, duplicate handover, cross-origin writes, retention beyond 24 hours, cookie renewal and storage failure. These tests use SQLite/test doubles; a successful test suite does not establish clinical accuracy or independently prove deployed Firestore security. Live verification must be performed separately.

## Limits relevant to reviewers

- Owners can write their own synthetic workspace directly within the Firestore rule limits; this is demo isolation, not clinical authorization or a tamper-proof audit trail.
- Rules bound the outer document and the serialized state size; they do not validate every nested patient/facility field. The Express API validates its own accepted inputs.
- The health probe creates an anonymous test identity to check connectivity. Do not use it for high-frequency monitoring; rate limiting and a side-effect-free health design are future hardening work.
- Workspace creation is not designed for simultaneous first requests from many tabs. Normal updates use optimistic concurrency checks.
- No real patient data, live facility capacity, EMS integration, clinical validation or measured benefit is included.
- Arabic and English are the main demonstration languages. Language controls and other visual modules should not be interpreted as clinically validated translations or prediction models.

Built for demonstration and technical review, not patient care.
