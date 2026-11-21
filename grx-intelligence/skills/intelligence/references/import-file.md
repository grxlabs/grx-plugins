# Importing a file: checklist

The procedure, its rules and its attempt budgets: https://business.gymrankx.com/docs/intelligence/import/spreadsheets.md (the spreadsheets.md#... anchors below are its sections). A known file (atestados, ponto, Wellhub or Gympass, cadastro): read its cookbook page first (https://business.gymrankx.com/docs/intelligence/cookbook/index.md), then follow this checklist with the cookbook's spec and mode.

1. Get the file in (spreadsheets.md#1-open-the-upload). An upload_id from the chat skips this. Otherwise start_intel_upload: curl to file_url with a terminal, else send upload_page_url to the user and wait. A ZIP gives one upload per spreadsheet, each its own base.
2. inspect_intel_upload (spreadsheets.md#3-inspect). Wrong layout: inspect again with header_row, delimiter, encoding or sheet. Several sheets (spreadsheets.md#4-several-sheets): ask only which to import, then split_intel_upload per extra sheet.
3. find_intel_bases (spreadsheets.md#6-does-the-base-already-exist), always, before creating anything. Ask the user in one line when it recommends "increment", or "new" with candidates; tell them about a duplicate. Never decide for them.
4. Existing base (spreadsheets.md#7-add-to-an-existing-base): drop new_sensitive columns with update_intel_source first; renamed columns take "from": ["New", "Old"], then dry_run_intel_mapping and publish_intel_mapping; a period base gets dry_run_intel_mapping {source_id, upload_id} to check the period before you confirm; a total base needs full_snapshot only for a complete file. Then confirm_intel_upload.
5. New base (spreadsheets.md#8-create-a-new-base): no identifying column, stop and ask. get_intel_identifier_catalog with samples, list_intel_definitions once, write the spec (https://business.gymrankx.com/docs/intelligence/mapping/spec.md), dry_run_intel_mapping, choose mode and key_fields, save_intel_source with drop_columns and the exact spec you tested.
6. Read every warning of save_intel_source or confirm_intel_upload and tell the user the ones that matter.
7. Follow the run (spreadsheets.md#9-follow-the-run).

Report: the source name you chose, rows read, events and what each means, rows with error, rows pending identity. Errors above 0: references/diagnosis.md. Unlabeled fields: offer references/dictionary.md once. A wrong import: spreadsheets.md#change-redo-or-recover.
