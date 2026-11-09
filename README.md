# Masar Health + Thaniya — Full-stack prototype

Team FlowCare · MED-GWPYB3 · MEDATHON Filter 2

## Versions and access

- Published full-stack demo: https://masar-thaniya-flowcare.ai.studio/
- Full-stack source: `MED-GWPYB3_FlowCare_FullStack.zip` in this repository.
- The backend was published and verified on 10 November 2026 (Asia/Riyadh). The public /api/health endpoint returns {"ok":true,"storage":"sqlite","mode":"simulated-demo","version":"1.0.0"}. A critical assessment was saved through the public UI and its count survived a page reload.

## Implemented backend

Node.js + Express API with SQLite storage, connected to the React frontend on the same origin.

- GET /api/health: service readiness and demo version.
- GET /api/state: isolated browser demo workspace.
- POST /api/triage: server-side screening and routing, assessment summary saved in SQLite.
- POST /api/actions: casualty creation, dispatch, handover, facility/capability simulation, balancing and reset.
- Revision checking rejects stale writes with HTTP 409.
- SQLite transactions update casualty status and bed counts together. Repeated handover does not increment occupancy twice.
- Anonymous HttpOnly SameSite cookies isolate demonstrations. This is not a clinical user authentication system.
- Basic validation, bounded demo records, same-origin writes, no persisted free-text casualty descriptions.
- Audit events store action type and timestamps. Forecasts remain fictional scenarios, not trained model predictions.

## Run

Download and extract the FullStack ZIP, then open its `masar-source` directory. Use Node.js 24 LTS or newer.

```sh
npm ci
npm test
npm run lint
npm run build
npm start
```

Open http://localhost:3000. For development use `npm run dev`. The server serves both API and built frontend. No Gemini API key is required.

Environment: `PORT` (default 3000), `DB_PATH` (default data/masar.sqlite), `COOKIE_SECURE=true` when serving over HTTPS. Set environment variables in the host; .env.example documents them but is not loaded automatically.

## Deployment

A Dockerfile is included. Use a Node-capable host, one application instance, and a persistent writable volume mounted at /app/data. SQLite on an ephemeral Cloud Run filesystem will not survive instance replacement. For scalable deployment migrate persistence to a managed database. Docker deployment has not been executed in this review.

```sh
docker build -t masar-demo .
docker run --rm -p 3000:8080 -e COOKIE_SECURE=false -v masar-data:/app/data masar-demo
```

Public HTTPS deployment should use COOKIE_SECURE=true. Never use this public demonstration for real patient information.

## Files

- server/app.ts: API, validation, workspace isolation, transactions.
- server/index.ts: production static hosting and development integration.
- src/api.ts: frontend API requests and revision management.
- src/App.tsx: server-backed facility and casualty state.
- src/components/PatientTriage.tsx: server triage integration.
- src/components/CommandCenter.tsx: saved demo counts.
- src/utils/routingEngine.ts: shared deterministic simulated routing rules.
- tests/backend.test.ts: HTTP integration tests, restart persistence and safety failures.

## Validation — 10 November 2026

TypeScript and production build passed. Integration tests passed for workspace isolation, database persistence after restart, malformed safety inputs, stale revisions, red-flag escalation, absence of capable emergency destinations, atomic handover, duplicate handover, cross-origin writes and unknown operations. Browser testing on the published public URL confirmed a critical assessment is processed and its saved count survives reload. Google AI Studio also reported both integration test groups, type checking and production build passing on Node 22.23.2.

Remaining limitations: no clinical validation; no real hospital/ambulance integration; no clinical identity or authorization system; no operational security assessment; no multiserver database; forecast and impact metrics are simulated. Build warnings remain for bundle size and future Vite config compatibility. This demonstrates backend implementation, not production readiness or guaranteed competition acceptance.

The conversation attachment is excluded from this source. The earlier frontend-only archive is retained as a historical snapshot; use the FullStack archive for the latest code.
