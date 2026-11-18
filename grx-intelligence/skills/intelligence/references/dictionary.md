# Data dictionary (labels)

Every event, event property, person property and source description can have a friendly label, a short description and a unit or format. Charts, tables and the console use them. What a person wrote (origin "user") is never overwritten.

1. list_intel_definitions(status "unlabeled", limit 40; offset 40 and 80 for more, at most 3 calls). Each item has kind, event, name, value_type, count and examples; "unlabeled" says how many are left in the whole catalog. Start with the most used and with events; stop at 60 items per round. When the user named fields, search each name and keep only exact matches.
2. list_intel_sources once: a source with an empty description (or one written by the AI when redoing) gets a description of what it brings, in one sentence.
3. When the name is not enough to understand a field, look at get_intel_source (which column and operations produce it) or list_intel_events (limit 5) for sample values; at most 4 such calls.
4. Save with apply_intel_labels(items: [{kind, event, name, label, description, unit, origin: "ai"}]), up to 60 items per call. A label the user dictates ("chame X de Y") goes with origin "user".
   - label: Brazilian Portuguese, up to 5 words, what an HR analyst would say, sentence case, no technical names ("Check-ins na academia" for checkin_academia). Two fields of the same kind never share a label.
   - description: one sentence, up to 200 characters, what the value means and where it comes from; null when the label says everything.
   - unit: horas, dias, R$, %, texto, número, data dd/mm/aaaa, CPF só dígitos...; null for events and values with no unit.
   Never copy example values, names, documents, health data or any personal data into labels: describe the pattern.
5. Tell the user how many fields you labeled and how many remain for a next round.
