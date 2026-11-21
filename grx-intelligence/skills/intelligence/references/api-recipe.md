# Writing an API recipe: checklist

Every recipe field: https://business.gymrankx.com/docs/intelligence/api-recipes/schema.md. Read the page for the part you are writing:
- One list request, with an optional detail call per record: https://business.gymrankx.com/docs/intelligence/api-recipes/v1.md
- Chained calls (units, then employees, then punches), id batches, asynchronous exports: https://business.gymrankx.com/docs/intelligence/api-recipes/steps-v2.md
- Auth (api_key, bearer, basic, oauth2, login, hmac, cookie_session): https://business.gymrankx.com/docs/intelligence/api-recipes/auth.md
- Pagination (page, offset, cursor, next_url, link_header, GraphQL): https://business.gymrankx.com/docs/intelligence/api-recipes/pagination.md
- XML, SOAP, CSV, files, ZIP, base64: https://business.gymrankx.com/docs/intelligence/api-recipes/formats.md

1. Start from the docs: servers give base_url, the security scheme gives auth (exactly one type), the list endpoint gives request.path, its response gives records_path, its params give pagination.
2. Credentials as {{secret.name}}; the values go in secrets.
3. probe_intel_recipe and read, in order: error, requests (status, body_preview), records and keys, page_details, warnings. Fix the first thing that is wrong.
4. A version 2 probe follows 2 records per step: fix the first step with errors.
5. Report the recipe that worked (with {{secret.name}}, never a value), how pagination works and how many probes you made.
