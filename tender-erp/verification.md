# Tender ERP — verification notes

Verified on 4 September 2026 against private frontend revision `1d547ab` and backend revision `208e96f`, with the small local audit-view correction described below.

## Local execution

- Actual React/TypeScript frontend compiled through its existing React Scripts setup and connected to the Django REST API.
- Django runtime: 5.0.2. Separate demo environment resolved the original environment's missing `pytz` import; no framework upgrade or frontend redesign was required.
- Database: newly migrated SQLite, populated with deterministic synthetic ORM/Faker fixtures. Original production data, environment files and media were not copied.
- Authentication and role checks enabled in the demo. Local memory cache/broker configuration; no live Telegram credentials, worker or beat process.
- Six screenshots: dashboard, register, deal overview, companies, activity and statistics.

## Automated checks

The following targeted test modules passed: **25 tests**.

```text
tests.test_auth
tests.test_deal_status_transition
tests.test_deal_cancel
tests.test_rbac
```

An additional authenticated API assertion requested audit events for one deal and verified that every returned record matched both its entity type and ID. The browser activity tab then showed only that deal's events.

## Small correction discovered during the run

The audit view declared `AuditLogFilter` but had no `DjangoFilterBackend`; global REST settings did not enable it either. Consequently the per-deal activity request returned unrelated records. The correction explicitly enables the filtering backend and ordering backend on this view. It was applied to the isolated demo and saved to the private local backend checkout. No private application source was published here.

## Interpretation limits

- Production use and business context are author-reported. No production scale, revenue, time-saving percentage or testimonial has been inferred from demonstration records.
- The demo has 32 deals, 64 line items, 6 companies, 12 clients, 4 brokers and 5 user accounts. Amounts and identities are fictional.
- PostgreSQL deployment behavior, full test suite, load, concurrency, live notification delivery and scheduled deployment health were not verified in this run.
- Audit is a recorded change history; database-level immutability is not claimed.
- Documents were inspected in source; client media was not exposed or used for screenshots.
- Architecture diagrams distinguish configured background infrastructure from locally exercised components. The lifecycle diagram is illustrative rather than an exhaustive transition graph.
- Reflection sections discuss the implementation and this review; they are not client endorsements.

## November 2026 refresh

The same backend and frontend revisions ran again against a clean SQLite database. The four targeted modules passed **25 tests**. The two new screenshots show an active auction and its queue in a separate fixture with three fictional deals; the earlier company, audit and reporting screens still show the larger September fixture. Telegram delivery was disabled.
