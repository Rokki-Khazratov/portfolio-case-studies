# GeoAgro verification notes

Verified on **4 September 2026** from local project copies and an isolated PostgreSQL database named `geoagro_portfolio`.

## Runtime verification

- Selected the original dark React application from `Agro/prod/geoagro-front`; its source matches the production-style frontend preserved in `geoagro-final`.
- Ran the frontend locally on port 3059 and the Django API on port 8059.
- Restored the supplied PostgreSQL dump into a newly created local database. The large request-log table was excluded from the restore; domain data was retained.
- Created one local review administrator only in the isolated database. No credential is included in this repository.
- `manage.py check` completed with no reported issues, and the restored snapshot contains all migrations through `api.0064`.
- Walked the authenticated dashboard, moderation, regional statistics, fruit statistics, controller statistics, map, plantation detail, approved/rejected/archive/deletion registers, admin console, logs and performance routes.
- Inspected the Flutter application's authentication, district-scoped list, create/update flow, polygon capture and photo-validation documentation and source. A mobile emulator was not run for this review.

## Direct snapshot counts

These are raw table counts, not active-user or dashboard metrics:

| Table meaning | Count |
| --- | ---: |
| Plantations | 44,165 |
| Farmers / organizations | 38,378 |
| Coordinate points | 627,689 |
| Plantation images | 98,678 |
| Plantation history entries | 498,287 |
| Fruit-area rows | 52,100 |
| Investment rows | 74,456 |
| Subsidy rows | 922 |
| Varieties | 323 |
| Fruit types | 31 |
| Rootstocks | 90 |

Dashboard values can be lower because its queries apply approval, archive and domain-specific filters. Saved screenshots and the restored snapshot may also represent different capture moments.

## Automated checks

The selected test modules cover staged moderation visibility, role-scoped map output, modernization of approved records and Excel/statistics aggregation. The first clean test-database attempt exposed migration `0025`: PostgreSQL cannot cast an integer directly to `jsonb`. The isolated review copy was changed to use `to_jsonb(kontur_number)` while keeping the same Django migration state.

After that correction, **26 of 27 targeted tests passed**. The remaining `test_no_actual_changes_no_modernizatsiya` fixture retains the pre-recalculation `total_area` on its Python object; creating its related plantation type recalculates the database value. The PATCH therefore sends a changed value despite the test's no-op intention. The actual no-op guard was reviewed and retained in the isolated copy, but the production repository and its tests were not modified.

The published case does not claim that this local migration correction was applied to the private production repository.

## Screenshot provenance

The published screenshots are full-size captures preserved with the supplied project context. Their routes and interface version were rechecked against the running dark frontend and isolated database. Some views contain historical organization names visible in the application; personal contact fields, phone numbers and detailed identifiers were excluded from the selected screenshots and text.

Map imagery depends on external tile services. The screenshot demonstrates the rendered district workflow, while local route verification confirms the application-side polygon and list behavior.

## Evidence limits

- The production deployment itself was not accessed.
- The date of the supplied database snapshot is not encoded in this case study.
- Reported geographic rollout and employee usage come from the project author and LinkedIn experience; raw table counts come from the restored snapshot.
- Redis, Celery and Firebase-related components appear in project history and dependencies, but this case only describes behavior verified in the inspected version.
- Presidential directive numbers and their precise legal relationship to GeoAgro remain pending and are not asserted here.
- System-design diagrams are outside this iteration by request.

[← Back to the GeoAgro case study](./README.md)
