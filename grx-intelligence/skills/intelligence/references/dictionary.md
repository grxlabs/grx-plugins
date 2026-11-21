# Data dictionary: checklist

The procedure, its attempt budget and how to write a good label: https://business.gymrankx.com/docs/intelligence/data-dictionary.md

1. list_intel_definitions(status "unlabeled"), events and the most used first. Named fields: search each and keep exact matches.
2. list_intel_sources once: a source without a description gets one sentence.
3. When a name is not enough: get_intel_source or list_intel_events(limit 5).
4. apply_intel_labels with origin "ai" (origin "user" only for what the user dictated). Read protected and not_found.

Report how many fields you labeled, how many remain and the ones you could not understand. Never copy example values or personal data into labels.
