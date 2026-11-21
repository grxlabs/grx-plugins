# Mapping spec: writing or fixing one

Every field and rule of a spec, the dry-run loop and its attempt budget: https://business.gymrankx.com/docs/intelligence/mapping/spec.md. Open when you need them: ops with examples, https://business.gymrankx.com/docs/intelligence/mapping/operations.md; conditions, https://business.gymrankx.com/docs/intelligence/mapping/conditions.md; dates, https://business.gymrankx.com/docs/intelligence/mapping/dates.md; the exact schema, https://business.gymrankx.com/docs/intelligence/mapping/schema.md

Writing a spec (new source):
1. list_intel_definitions(kind event) once, to reuse event and property names.
2. Identifiers as spec.md#person-identifiers says: every id column, matricula and system_id with trim only, the namespace the identifier catalog gave.
3. Event names from what happened (a kind column maps to names), properties for the occurrence, person properties in set or set_once. Placeholders with null_if first. A fixed name is {"value": ...}.
4. dry_run_intel_mapping; read summary, errors, warnings and samples. Errors in most rows mean the spec is wrong.

Changing the mapping of a saved source:
1. get_intel_source for the active spec.
2. Change only what is wrong. A renamed column takes the list form "from": ["New", "Old"].
3. dry_run_intel_mapping with source_id and the whole new spec; for a fix, failed_only to prove it on the records that failed (https://business.gymrankx.com/docs/intelligence/observability/runs-and-logs.md, repair flow).
4. publish_intel_mapping with an idempotency_key; it reprocesses the records with error and the ones pending identity (reprocess true only when records that worked must change).
5. Follow the run and report how many records changed and how many still fail.

When the mapping cannot express the rule: references/functions.md
