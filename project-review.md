# Project review

Reviewed 13 November 2026. The assessment is about the code and workflows available for review. A working local demo establishes less than a production deployment, and a deployed public page establishes less than an operational check of its admin and notification flows.

| Project | What holds up | Where the evidence stops |
| --- | --- | --- |
| [OfferFly](./offerfly/) | Recruiters can trace a score back to the transcript. The voice worker and the server have clear jobs: conversation on one side, evidence and scoring on the other. | The core suite passed 199 tests after setting up a fresh schema with Prisma. Migration deploy into an empty database did not work in the inspected revision. The landing was recaptured locally; the voice providers were not rerun. |
| [GeoAgro](./geoagro/) | The permission model follows the geography of the work. Field staff collect boundaries; reviewers can trace changes to approved records. | The map sequence comes from the historical web demo and project captures. A new mobile or web run was not completed. Scale figures are snapshot counts and reported rollout, not live usage. |
| [Sofmetal ERP](./sofmetal-erp/) | A mixed order can wait on warehouse, procurement and logistics at the same time, with an explicit finance close. | The local mixed-deal path passed. The legacy tests still fail in fixture setup; the check did not cover receiving, printed documents or external delivery. |
| [UZMM](./uzmm/) | The catalogue gives buyers dimensions and prices before they send a request. The backend copies the catalogue price to the order line. | Production browsing and cart were checked. Local Spring Boot and PostgreSQL handled an order and an inquiry; staff access and production notification delivery were not exercised. |
| [Tender ERP](./tender-erp/) | An auction deadline changes what staff need to do, even when nobody edits the deal. The API and scheduled tasks both account for time. | The React and Django demo ran on SQLite. Twenty-five targeted tests passed. Telegram delivery and production scheduling were not checked in this run. |
| [School CRM](./school-crm/) | A balance can be explained from its charges and allocations. A correction shows the owner what the manager wants to change. | The new PostgreSQL demo and 295 unit/component tests passed. The accounting screenshots are fictional. The reported production numbers date to September 2026. |
| [ExamTwin](./examtwin/) | A mock is tied to the exam's blueprint and rules. The analytics screen can explain its recommendation using previous attempts. | The browser-only demo ran and 27 frontend tests passed. The FastAPI backend and live AI providers were not exercised in this refresh. |
| [MenuMargin AI](./menumargin-ai/) | A restaurant operator can follow an ingredient forecast through the recipe to a dish margin, then try a price constraint. | The local UI and API served the sample restaurant and simulated a scenario from cached forecasts. The demo does not establish out-of-sample forecast accuracy. |

For an ERP portfolio, start with Sofmetal, GeoAgro and School CRM. They show three different business problems: departments waiting on a deal, territorial approval of field records, and the accounting behind a student's balance. UZMM is the easiest project for someone to try without credentials.

[All case studies](./)
