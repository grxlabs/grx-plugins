# Code functions: checklist

When to use one, the hooks, the sandbox rules, the flow and its attempt budget: https://business.gymrankx.com/docs/intelligence/functions/index.md. Hooks, ctx, helpers and ready recipes: get_intel_function_sdk (the same as https://business.gymrankx.com/docs/intelligence/functions/sdk.md).

1. Check first that no mapping op does it (references/mapping.md). A function only for HTML or fixed-width responses, request signing, logic between rows, one record into several by a rule, or consolidation at the end of the run.
2. get_intel_function_sdk once. A saved source: get_intel_function(source_id) and list_intel_raws for real records.
3. dry_run_intel_function on the sample (upload_id, probe_id, records, or recipe with secret_values for hooks that call the API). Read samples, errors, on_end and logs. When the budget runs out, stop and report what blocks.
4. Never save code that was not tested or still fails.
5. New source: function {code, secrets} in save_intel_source. Saved source: publish_intel_function with an idempotency_key; reprocess false when the new version only fixes records that failed.
6. Going back to the mapping alone: disable_intel_function.

Report what the function does in one sentence, the test numbers and the run numbers. Never put a secret value in the code.
