# School CRM — verification notes

Reviewed on **4 September 2026**. These notes document the case-study capture session, rather than a production certification.

## Evidence sources

- Developer statement: sole implementation, June–September 2026; in production at one private school; 150 students, two managers, four users and 600+ recorded accounting transactions.
- Local application source at commit `8b105618753341f548a20e4e18bcbe5cc46f57b7`, plus the two small UI fixes described below.
- Fresh browser observations against a separate local PostgreSQL instance and synthetic data.
- Existing unit/component and PostgreSQL integration suites run during the session.

## Demo isolation and dataset

A separate source snapshot and PostgreSQL cluster were used. Production environment files and database contents were not copied. The demo contains 40 synthetic pupils, three tariffs, 240 monthly charges and an initial 200 tuition records. Two additional entries were created through the browser while checking the payment form, bringing the final demo total to 202. Screenshots capture different points in this sequence.

Synthetic history covers January–May and September 2026 to exercise the school billing calendar and reporting filters. These dates are demonstration data, not a claim that the product was operating before its development period. Phone fields use a deliberately non-operational placeholder. Demo role names are labelled “local”.

Three correction requests demonstrate pending, approved and rejected states. Payment creation and correction setup use application services, so related ledger and audit records follow the implemented rules.

No Telegram credentials were configured and no delivery worker was started. The local capture verifies queued notification intent and the monitoring interface, not live Telegram delivery.

## Browser observations

| Route | Observed behaviour |
| --- | --- |
| `/uz/login` | Demo owner signed in through the regular authentication form |
| `/uz/dashboard` | Daily records, active students, expected charges, debt and recent operations rendered |
| `/uz/students` | Student search/filter controls, tariffs, contact placeholders and balances rendered |
| `/uz/students/STU-000002` | Monthly ledger, partial settlement, tariff history and recent records rendered |
| `/uz/payments` | 200 initial records, allocations, methods and filtered-total controls rendered |
| `/uz/payments/new` | Student selected; 2,000,000 UZS recorded with 1,500,000 allocated and 500,000 advance; a subsequent 100,000 UZS record verified the refresh fix |
| `/uz/statistics?from=2026-01-01` | Historical totals, payment count, distinct pupils, chart and method breakdown rendered |
| `/uz/debtors` | Outstanding and overdue amounts with oldest due date rendered |
| `/uz/corrections` | Original/proposed values, reason and owner decision controls rendered |
| `/uz/notifications` | Queued events and zero delivery attempts rendered; no messages were sent |
| `/uz/audit` | Payment, correction and sign-in events attributed to demo users rendered |

## Automated checks

| Check | Result |
| --- | --- |
| Existing Vitest unit/component suite | **279 passed**, 29 test files |
| Existing PostgreSQL integration suite | **34 passed**, 2 test files |
| TypeScript `tsc --noEmit` | Passed, including after the small form fix |
| Targeted ESLint check on the changed payment form | Passed |
| Both Excalidraw scene validators | Passed |

The existing suites were run before the final payment-form refresh adjustment. That adjustment was subsequently checked in the browser, with TypeScript and targeted lint. No production deployment, production load test or backup restore drill was performed for this case study.

## Small fixes made while running the project

1. **Profile control clipping:** the identity text inherited the header's large line height. Giving the profile button a normal line height keeps the name and role within the header.
2. **Stale allocation balances after saving:** a successful payment reset the amount but did not refetch the selected student's outstanding charges. The form now refreshes charges when its post-submit idempotency key changes, clearing the old charge list while loading. A second browser submission showed the remaining March debt decrease from 600,000 to 500,000 UZS without a page reload.

The two fixes were also applied to the original local source checkout. Only presentation files and synthetic-data screenshots are published in this repository. The development indicator was hidden in the isolated demo configuration for screenshots.

## Technical claim boundaries

- A payment record represents staff-entered accounting. Payment-method names do not imply gateway integration.
- The payment transaction includes its allocations and outbox event. Service-level audit writes are a separate boundary.
- The audit trigger blocks normal updates and deletes; it is not cryptographic tamper evidence or protection from a privileged database administrator.
- Retry and deduplication mechanisms do not establish exactly-once delivery to Telegram.
- Production adoption numbers are developer-reported. No measured time savings, revenue lift or corruption-prevention outcome is claimed.
- The architecture diagrams represent inspected code structure; production infrastructure details remain unspecified.
