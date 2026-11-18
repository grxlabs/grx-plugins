# Code functions (JavaScript)

Use a function only when the mapping cannot express the rule: HTML or fixed-width responses, request signing, logic between rows (dedupe, header plus detail, pivot), one record split into several by a rule, business rules (ignore vacation, add overtime), consolidation at the end of the run. Clean, rename, convert or ignore rows with mapping ops first. The function and the mapping live together: transform receives ctx.mapped.

1. get_intel_function_sdk once: hooks (beforeRequest, afterResponse, transform, transformBatch, onStart, onStepEnd, onEnd, onError), ctx fields, helpers, libraries and ready recipes.
2. For a saved source, get_intel_function(source_id) shows the active code and the secrets it can read; list_intel_raws shows real records.
3. Write the code and test it: dry_run_intel_function(code, mapping or source_id, plus upload_id for a file, records for API records, or recipe with secrets (names) and secret_values when beforeRequest, afterResponse or onStepEnd must make the real call). Read samples (input to events), errors, on_end and logs (console.log comes back in logs). Fix and test again until there are no errors, at most 5 tests. Never save code that was not tested or that still fails.
4. Save it: for a new source, pass function {code, secrets} to save_intel_source (the server tests it again on the sample and refuses it if any record fails); for a saved source, publish_intel_function(source_id, code, secrets), which reprocesses without duplicating events. reprocess defaults to true (every record is redone); send reprocess false when the new version only fixes records that failed, so only the records with error are redone. disable_intel_function goes back to the mapping alone.
   Secrets: a function reads only api source secrets, by name. The source secrets hold every value, the recipe's and the function's; function.secrets lists the names the function reads.

Rules: hooks are plain top-level function declarations, synchronous (no async/await, fetch, timers, require or import). Each batch runs in a fresh sandbox, so globals do not survive between batches: keep state across rows and batches in ctx.db (SQLite of the run, shared by every batch and by onEnd; always with ? parameters). Every event returned by onEnd needs a unique key (re-running replaces by key). A function that throws turns only that record into an error; if most records fail the source is paused.

Detail: https://business.gymrankx.com/docs/intelligence/functions/index.md
