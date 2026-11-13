# OfferFly verification notes

Verified on **14 September 2026** from local OfferFly repositories and an isolated PostgreSQL database populated with synthetic data.

## Repositories inspected

- `casify-mvp`: candidate experience, B2C flow, central API, administration and shared interview domain.
- `enterprise-dashboard`: recruiter dashboard, roles, candidates, reports, statistics and import.
- `agent-worker`: Python real-time voice agent and LiveKit provider orchestration.
- `offerfly-landing`: current public product site.
- `offerfly-landing` `b2b-relaunch` branch: B2B landing iteration.
- Local Telegram operations bot.

The public organization is available at [github.com/offerfly](https://github.com/offerfly). The case study describes behavior confirmed across the inspected local copies; it does not imply that every implementation repository is public.

## Runtime and visual verification

- Created a separate local PostgreSQL database and applied the Prisma schema without connecting to production.
- Loaded synthetic data for one fictional organization: 8 roles, 46 candidates, 34 completed interviews and 12 invited candidates.
- Built and started the core Next.js application on port 3001.
- Built and started the enterprise dashboard on port 3002.
- Started the landing application on port 3010.
- Exercised recruiter authentication, dashboard, statistics, role pipeline, candidate report and data-import routes.
- Exercised a synthetic candidate's magic-link invitation and consent page.
- Captured all published screenshots from those local runtimes.

Development mode initially exceeded the macOS file-descriptor limit and failed to register some watched route files. Production builds and `next start` were used for the review; both applications built successfully and exposed the expected routes. No product source change was needed for that environment-specific issue.

## Direct code measurements

| Measurement | Result |
| --- | ---: |
| Prisma models | 30 |
| Core API `route.ts` handlers | 94 |
| Core application pages | 21 |
| Enterprise dashboard pages | 15 |
| Total Next.js pages | 36 |

Counts reflect the inspected checkout on the verification date and may change as the product develops.

## Automated checks

| Component | Result |
| --- | ---: |
| Core app Vitest suite | 176 passed |
| Enterprise dashboard Vitest suite | 13 passed |
| Python voice-worker unittest suite | 10 passed |
| **Total** | **199 passed** |

The core application and enterprise dashboard also completed production Next.js builds. Architecture files were validated as Excalidraw JSON after generation.

## Architecture evidence

The system diagrams combine source inspection with the production topology documented in the project's technical architecture notes:

- Next.js 15 / React 19 core application and separate enterprise dashboard.
- PostgreSQL 16 through Prisma 5.
- Python 3.11 LiveKit worker with no direct database access.
- LiveKit Cloud, Deepgram, Gemini through Vertex AI and ElevenLabs in the B2B voice path.
- Resend, Stripe, Google Forms intake and Telegram integrations.
- Hetzner VPS, Docker Compose, Nginx, Let's Encrypt and persistent database/upload volumes.

The current production design does not claim Kubernetes, Redis, a message queue, object storage, a separate staging environment or complete centralized observability.

## Evidence limits

- The **2 B2B pilot clients** figure is the founder's current reported commercial status. Client identities and confidential pilot details are intentionally omitted.
- Production servers, databases and candidate records were not accessed for this case study.
- Screenshots contain only generated names and synthetic recruiting data. Their counts are not presented as production usage.
- A live paid call through all external voice and AI providers was not initiated. Provider adapters, runtime configuration, state logic and automated tests were inspected instead.
- The recruiter report is decision support. The final hiring decision remains human.
- Public diagrams simplify implementation detail to make boundaries and workflows readable. Cards represent logical responsibilities unless the text explicitly describes a separate deployment.

[← Back to the OfferFly case study](./README.md)

## November 2026 refresh

The B2B landing ran locally at revision `f0f891b` and was recaptured. The recruiter and candidate screenshots above are from the September synthetic workspace. The core app at `e19f3f5` passed **199 tests across 33 files** against a new PostgreSQL schema created with Prisma schema push. The checked-in migration chain did not bootstrap that empty database: a migration referenced `InterviewSession` before it existed. The test result does not establish that a clean migration deploy works. Live interview providers were not exercised in this refresh.
