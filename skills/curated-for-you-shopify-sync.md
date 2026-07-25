---
name: Sync and analyze a Shopify store's collections
description: Ensure the Curated for You Shopify app is installed, check store status, and trigger analysis/resync of collections.
api: openapi/curated-for-you-openapi-original.yml
operations:
- ensure_installed_api_v2_shopify_ensure_installed_post
- get_store_status_api_v2_shopify_status_get
- request_setup_api_v2_shopify_request_setup_post
- list_collections_api_v2_shopify_collections_get
- request_collection_analysis_api_v2_shopify_collections__handle__request_analysis_post
- resync_collection_api_v2_shopify_collections__handle__resync_post
---

# Sync and analyze a Shopify store's collections

Use this skill to onboard a Shopify store into Curated for You and keep its collections analyzed.

## Auth
Obtain a Bearer token via `login_api_v2_users_login_post` (POST `/api/v2/users/login`) and send `Authorization: Bearer <token>`.

## Steps
1. `ensure_installed_api_v2_shopify_ensure_installed_post` — POST `/api/v2/shopify/ensure-installed` to confirm/complete the app install for the store.
2. `get_store_status_api_v2_shopify_status_get` — GET `/api/v2/shopify/status` to read the store's current integration state.
3. `request_setup_api_v2_shopify_request_setup_post` — POST `/api/v2/shopify/request-setup` if the store still needs initial setup.
4. `list_collections_api_v2_shopify_collections_get` — GET `/api/v2/shopify/collections` to enumerate collections by `handle`.
5. `request_collection_analysis_api_v2_shopify_collections__handle__request_analysis_post` — POST `/api/v2/shopify/collections/{handle}/request-analysis` to kick off lifestyle analysis for a collection.
6. `resync_collection_api_v2_shopify_collections__handle__resync_post` — POST `/api/v2/shopify/collections/{handle}/resync` to refresh a collection after catalog changes.

## Rules
- `{handle}` is the Shopify collection handle from step 4 — never invent one.
- These POSTs are not documented as idempotent; treat analysis/resync as async jobs and poll status rather than firing repeatedly.
- Validation failures return `422` with `detail[]`.
- Curated for You also receives inbound Shopify webhooks at POST `/api/v2/shopify/webhooks` (store-to-platform events); it is not a consumer-facing event stream.
