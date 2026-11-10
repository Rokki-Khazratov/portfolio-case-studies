# Sofmetal ERP verification

**Reviewed 10 November 2026.** This is a review of a local historical source snapshot, not the deployed environment.

| Component | Source |
| --- | --- |
| Backend | Private `ERP-backend`, baseline `9c7dd33` (20 July 2025), with local working-copy API changes and migration `0024` |
| Frontend | Private `ERP-frontend`, `a8ead4c` (21 July 2025) |
| Demo runtime | Python 3.11, Django 5.0.2, Django REST Framework 3.14, React 18 and a fresh SQLite database |

## Runtime and data isolation

- Copied source to a separate workspace. Used a new database and media directory; did not read the old database or use its media for screenshots.
- Installed the historical dependencies with two local adjustments: changed the unavailable `pytest-django==4.7.1` pin to `4.7.0`, and installed `requests`, which the backend imports but did not declare in its requirements.
- Applied migrations through the local `0024`. Django system check passed. The React development build compiled with lint warnings.
- Created nine fictional role accounts, four fictional clients, four catalogue items, a procurement request, demo inventory batches, and nine staged sample deals. The smoke test created a tenth deal through the API.
- All screenshots came from Chrome against the real local frontend and API. Browser sessions authenticated through the app as administrator, finance or logistics. The UI remains in its original Russian. All visible identities, prices and documents are demonstration data; no live customer data was used.
- No Redis or Celery worker, Telegram delivery, live email or production integrations were run. The smoke test stubbed the external exchange-rate lookup to a deterministic value.

## Checks and limits

| Check | Result |
| --- | --- |
| App boot and database migrations | Passed on the isolated SQLite database |
| Browser login and populated screens | Verified the deal register/detail, catalogue, procurement, customer directory, logistics action area and finance queue |
| API creation and workflow | Empty line list rejected; mixed physical and supplier sourced deal created. Viewer denied; salesperson blocked from approving their own deal. Sales lead, accounting and finance moved it to fulfilment. Warehouse and procurement left it waiting; logistics released the gate; finance closed it. |
| Legacy deal validation suite | Six tests failed during fixture setup: `Supplier.type` is required, but the fixtures omit it. Those tests did not reach their assertions. |
| Standalone logistics detail route | Placeholder in this snapshot. The screenshot shows the populated logistics section inside the deal instead. |
| Warehouse reconciliation, printed documents, external notifications | Not verified end to end. Typed upload fields and worker code are visible in source, which does not prove those integrations operate in the deployed system. |

The owner reported production use separately. No adoption count, measured savings, live deployment topology or independent uptime figures are claimed here.

[← Sofmetal ERP](./)
