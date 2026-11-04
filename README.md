# AlphaCI order desk: fail frontend

[![Strict preflight](https://github.com/St-Peter-Sandbox/alphaci-demo-fail-fe/actions/workflows/ci.yml/badge.svg)](https://github.com/St-Peter-Sandbox/alphaci-demo-fail-fe/actions/workflows/ci.yml)

React frontend at the repository root, matching the separate repositories AlphaCI creates. The companion repository is [alphaci-demo-fail-be](https://github.com/St-Peter-Sandbox/alphaci-demo-fail-be).

Deliberately incorrect implementation: the ten-item discount tests must fail. The tests and CI gates are identical to the passing version.

This is a GitHub-only copy source. It is not an AlphaCI project. Checks run on temporary runners; they do not publish images, deploy, register releases, or call AlphaCI.

## Order rule

Orders of ten or more get a 10% discount and free delivery; smaller orders have a ₱50 delivery fee. Calculations use integer cents. Ten items at ₱250 must total ₱2,250. The intentional bug uses quantity > 10 for the discount, returning ₱2,500 for ten items.

## Checks

React/Jest, 95% coverage, ESLint with zero warnings, TypeScript, build, dependency audit, licenses, Gitleaks, blocking SonarCloud analysis, Playwright, Docker build and container smoke test.

Reusable workflows remain pinned to b107b06612c698fba7fd17463b2ffb3b575a87ee. Only the bootstrap src/main.tsx and declaration src/vite-env.d.ts are excluded from unit coverage.

## Copy into the live AlphaCI repository

1. Create the real project in AlphaCI with separate React frontend and .NET backend repositories (Node 24 and .NET 10). Clone the generated frontend repository and create a working branch.
2. From this sample clone, preview:

```powershell
.\Copy-Demo.ps1 -Repository C:\Demo\live-order-fe -ReplaceStarter -WhatIf
```

3. Run without -WhatIf, review git diff, commit and push. The helper requires a clean target clone; it replaces starter src/tests and conflicting build configuration while preserving the generated .github workflows, README, existing Sonar/editor configuration and environment files. It does not copy this sample's README, LICENSE or helper.
4. For recovery, use the matching passing frontend repository's helper against the same clone, leaving tests and gates unchanged.

## Run locally

Run from the repository root with Node 24:

```text
npm ci --ignore-scripts
npm run lint
npm run typecheck
npm test -- --coverage
npm run build
npx playwright install chromium
npm run test:e2e
```

Start the UI with npm run dev -- --port 4173. It previews quotes locally by default. To call the backend, set the public VITE_API_BASE_URL before building and allow the frontend origin in backend CORS_ORIGINS.

The container listens on port 8080; use distinct host ports when running the frontend and backend together.

## Standalone CI configuration

The maintainer configures SONAR_TOKEN, SONAR_ORGANIZATION and SONAR_PROJECT_KEY as GitHub Actions secrets. Automatic Analysis is disabled because CI performs the scan. These sample settings do not belong to the real project created during the demo.
