---
name: Retrieve a company's curations
description: Authenticate, discover accessible companies, and list a company's curations and the latest exported snapshot.
api: openapi/curated-for-you-openapi-original.yml
operations:
- login_api_v2_users_login_post
- get_companies_api_v2_companies__get
- fetch_curations_api_v2_curations__get
---

# Retrieve a company's curations

Use this skill to pull curated product collections for a retailer from the Curated for You API.

## Auth
1. `login_api_v2_users_login_post` — POST `/api/v2/users/login` with form-encoded `username` and `password`. Read the returned Bearer token; it lasts ~30 minutes. Send `Authorization: Bearer <token>` on every subsequent call.

## Steps
1. `get_companies_api_v2_companies__get` — GET `/api/v2/companies/`. Use repeatable `projection` params (e.g. `projection=code_name`, `projection=display_name`) to trim the response. Capture the `company_id` you need.
2. `fetch_curations_api_v2_curations__get` — GET `/api/v2/curations/?company_id=<id>`. Page with `limit`/`offset` and order with `sort_by=created_at:desc`. Select fields with `projection` (e.g. `curation_display_name`, `curation_group_number`). Keep the `curation_group_number` — it identifies the curation across exported snapshots.

## Rules
- Pagination is limit/offset; responses carry `data`, `total`, `limit`, `offset`.
- A `403` means your organization permissions do not cover that company — contact your customer care rep, do not retry blindly.
- Errors return a FastAPI `detail` envelope; `422` lists field errors in `detail[]`.
- No idempotency key contract — these are read operations, safe to retry.
