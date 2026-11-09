# Masar Health + Thaniya
Team FlowCare | MED-GWPYB3 | MEDATHON Filter 2
Live demo: https://masar-thaniya-flowcare.ai.studio/
## Implementation status
This is a frontend simulation prototype built with React, TypeScript and Vite. It demonstrates structured patient screening, routing, Thaniya casualty coordination, a command center, a routing lab and a 3D visualization.
**No backend service, database, authentication system, live hospital capacity feed or ambulance integration is implemented in this source snapshot.** The presence of Express or Gemini packages in package.json does not mean those services are implemented. If the competition requires an implemented backend, this submission does not yet meet that requirement.
Patient cases, utilization, surge forecasts and confidence values are simulated. They are not measured clinical outcomes. The application is not a diagnostic device and has not been clinically validated. Do not use it to make real care decisions.
## Download source
The complete frontend source tree is provided in MED-GWPYB3_FlowCare_Source.zip in this repository. Download and extract it, then open the masar-source folder.
## Run locally
Use Node.js 22.12 or newer.
```sh
npm ci
npm run dev
```
Open http://localhost:3000. The demonstration does not require a Gemini API key. Optional browser location and external map links depend on browser permissions and connectivity.
```sh
npm run lint
npm run build
npm run preview
```
## Source map
- src/components/PatientTriage.tsx: screening and patient journey
- src/utils/routingEngine.ts: browser-side routing logic
- src/components/ThaniyaMCI.tsx: simulated casualty workflow
- src/components/CommandCenter.tsx: simulated operational dashboard
- src/data/mockData.ts: fictional facilities and scenario values
- src/data/translations.ts: UI translations
## Review snapshot
Exported from AI Studio on 10 November 2026. The original exported snapshot is commit 3afc83c6fd88741010198be4e5b5d8e3648cfa09 in the team's private development repository.
This review copy excludes the conversation attachment from public/assets/aistudio. It removes the conflicting unused direct esbuild dependency and replaces the Bun lockfile with an npm lockfile. Application logic is unchanged. These packaging changes have not been deployed to the live demo.
Validation on 10 November 2026: npm run lint and npm run build passed. Build warnings remain about bundle size and future Vite config compatibility. Passing these checks does not establish clinical accuracy, device coverage or production readiness.
## Suggested judge walkthrough
1. Select the critical chest-pain demo and inspect emergency escalation.
2. Select the low-acuity demo and inspect the routing explanation.
3. Explore fictional casualties in Thaniya and their workflow states.
4. Inspect simulated utilization and surge views in the command center.
Do not enter real patient data. Clinical safety review, deterministic scenario testing, a backend, verified data integrations and prospective validation remain future work.
