# Connecting an external API

1. Ask only what you cannot infer: base URL, auth and credentials, the endpoint that lists the records. With documentation (a Swagger/OpenAPI link, a docs page or pasted text), call read_intel_api_docs(url or text) first: servers give base_url, the security scheme gives auth, the endpoint that lists the records (the one the user wants; otherwise the main list of people or records that change over time) gives request.path, its response fields give records_path and its params show the pagination. Ask for a credential only when the auth needs it, naming exactly which one and where it goes ("a chave da API, enviada no header X-Api-Key").
2. Credentials go only in secrets ({"api_key": "..."}) and are referenced in the recipe as {{secret.api_key}}. Never write a secret value into the recipe, the spec, a function or your answer.
3. Write the recipe (references/api-recipe.md) and call probe_intel_recipe(recipe, secrets). Read error, requests (status, body excerpt), records and warnings. Fix auth, path, records_path, pagination, window or error_when and probe again; at most 6 probes. Stop when records come back and you understand the pagination.
4. list_intel_definitions(kind event) once, then write the mapping spec (references/mapping.md) and test it: dry_run_intel_mapping(spec, records: the probe records). At most 3 dry runs.
5. save_intel_source(name, description, kind "api", mode "incremental" (total only when every read is the full snapshot), recipe, secrets, spec, schedule_minutes when the user wants it scheduled, run_now true).
6. list_intel_runs(source_id) until the first sync ends; report records read, events, errors and pending identification.

Rules:
- One source per independent list endpoint (employees, punches, absences): each gets its own recipe and mapping. Map the system's own person code as system_id with the same namespace in all of them, so their persons merge. A list that needs ids from another list is one source with a recipe in steps (version 2).
- auth takes exactly one type. OAuth2 plus a request signature: oauth2 auth in the recipe and the signature in a beforeRequest function.
- probe_intel_recipe runs no function. For an endpoint that only answers signed or computed requests, write the function first and test the whole thing with dry_run_intel_function(code, kind "api", recipe, secrets: the names the function reads, secret_values: every value the recipe and the function need). Then save_intel_source with function {code, secrets} and secrets holding every value (recipe and function).
- Rate limit (429): each request retries by itself (recipe retry, Retry-After honored). A run that is still rate limited 3 times pauses the source (SOURCE_BREAKER). Slow it down with limits.rate_limit or limits.min_interval_ms, then resume_intel_source.

When the response cannot be expressed by the recipe and the mapping (HTML, fixed-width text, request signing, logic between records), add a code function (references/functions.md).
An existing API source: run_intel_source runs it now, resync_intel_source redoes it from a date or from the start, update_intel_source changes recipe, secrets, mode or schedule.
A webhook source: test the mapping with dry_run_intel_mapping on sample records, then save_intel_source(kind "webhook", mode incremental, spec); create_intel_source takes no mapping. The key the external system sends is a secret and never passes through you: tell the user to generate it on the source page in the console, where it is shown once. Endpoint URL, payload shapes and responses: https://business.gymrankx.com/docs/intelligence/import/webhooks.md
Detail: https://business.gymrankx.com/docs/intelligence/import/api.md
