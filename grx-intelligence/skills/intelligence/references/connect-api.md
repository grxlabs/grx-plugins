# Connecting an API or a webhook: checklist

## API
The procedure, its rules and its attempt budgets: https://business.gymrankx.com/docs/intelligence/import/api.md (the api.md#... anchors below are its sections). A worked HRIS example (OAuth2, cursor pagination, roster mapping): https://business.gymrankx.com/docs/intelligence/cookbook/hris-api.md

1. Ask only what you cannot infer (base URL, auth and the exact credential, the list endpoint). With docs, read_intel_api_docs first.
2. Write the recipe (references/api-recipe.md). Credentials only in secrets, as {{secret.name}}; never a secret value anywhere else.
3. probe_intel_recipe until records come back and the pagination is understood. Signed or computed requests: dry_run_intel_function with recipe and secret_values instead (api.md#signed-or-computed-requests).
4. list_intel_definitions(kind event) once, write the spec (references/mapping.md), dry_run_intel_mapping with the probe_id of the last probe, never the masked records.
5. One source per independent list endpoint, the same system_id namespace in all of them.
6. save_intel_source(kind "api", mode, recipe, secrets, spec, schedule_minutes when asked, run_now true) with an idempotency_key.
7. Follow the first sync (api.md#5-follow-the-first-sync).
8. Unlabeled fields: offer references/dictionary.md once (api.md#6-label-the-data).

Stop and ask the user when a credential fails or the endpoint is unknown; never guess. Maintenance (run now, resync, pause, new secret): api.md#maintain-an-api-source.

## Webhook
The procedure and its rules: https://business.gymrankx.com/docs/intelligence/import/webhooks.md

1. Get a sample payload, dry_run_intel_mapping on sample records (one item, not the whole body).
2. save_intel_source(kind "webhook", spec), never create_intel_source. Give the user endpoint_url.
3. The key is a secret that never passes through you: the user generates it on the source page of the console.
4. When the first deliveries arrive, offer references/dictionary.md once for unlabeled fields.

Report: source name, events and what each means, schedule, the run numbers (records read, events, errors, pending identity).
