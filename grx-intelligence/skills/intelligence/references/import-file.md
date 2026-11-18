# Importing a spreadsheet

## 1. Get the file in
- The conversation already has an upload_id (a file attached in the console chat): skip to step 2.
- Otherwise call start_intel_upload(file_name) with the real file name. It returns file_url and upload_page_url, valid for 1 hour and for one file.
  - With a terminal (Claude Code, Cowork on the user's computer): `curl -sS -T "<path>" -H "x-file-name: <file name, URL-encoded>" "<file_url>"`. The JSON answer already carries the inspection (or, for a ZIP, archive.members with one upload per spreadsheet).
  - Without a terminal (claude.ai, ChatGPT): send upload_page_url to the user, ask them to drop the file there and tell you when it is done, then call inspect_intel_upload(upload_id).
- A ZIP becomes one upload per spreadsheet (archive.members): treat each one as its own file and its own base, never one merged base. With more than 4 spreadsheets, ask which ones to import.

## 2. Read it
inspect_intel_upload shows format, encoding, delimiter, sheets, header row, columns (type guess, examples, date formats, frequent values) and a preview. If the layout looks wrong (a title above the header, one giant column, broken accents), call it again with header_row, delimiter, encoding or sheet.
- Workbook with several sheets: each imported sheet is its own base. A report with a "Resumo" sheet and a detail sheet is one base fed by the detail sheet. Suggest skipping sheets that are only notes or summaries; ask the user only which sheets to import, one line per sheet with what it holds. For every sheet after the first, split_intel_upload(upload_id, sheet) opens a separate upload already read on that sheet.
- Columns flagged sensitive (passwords, tokens, hashes, signed links that open private files) are never stored: put them in drop_columns. Use keep_sensitive only when the user explicitly asked to keep that column.

## 3. Does this base already exist?
Always call find_intel_bases(upload_id) before creating anything.
- duplicate: this exact file was imported before. Say so; import again only if the user wants.
- recommendation "increment": a base matches (score, reasons, column diff). Ask the user in one line: add this file to base X, or create a new base. Never decide for them.
  - Before confirming: new_sensitive not empty means the file brings sensitive columns the base does not drop yet. Call update_intel_source(source_id, drop_columns: the base drop_columns plus those) first; confirm_intel_upload has no drop_columns of its own.
  - Add to the base, mapping still fits (mapping_impact empty): confirm_intel_upload(upload_id, source_id).
  - Add to the base, mapping columns missing or renamed: get_intel_source for the active spec, turn each renamed "from" into the list form ["New name","Old name"] (first not empty, so old and new files both work), dry_run_intel_mapping(source_id, upload_id, spec), publish_intel_mapping, then confirm_intel_upload. A new column the mapping does not use is fine.
  - Period base: the period comes from period_from/period_to, else the file name (name_period), else the min and max of the date column. When name_period disagrees with the dates you saw in the preview or the dry run (a name that says October, rows from November), pass period_from and period_to.
- recommendation "new" but candidates exist: name the closest candidate (name, score) and ask in one line whether the file belongs to it.
- No candidate (or the user chose a new base): step 4.
- Always read warnings in the answer of save_intel_source and confirm_intel_upload (dates outside the period, mapping columns missing in the file, a similar base, a kept sensitive column) and tell the user the ones that matter.

## 4. Create the base
1. If no column identifies a person (cpf, matricula, email, a system id), stop and ask which column does and what kind of id it is. A name alone does not identify anyone; a bare code (Ref, Código) does not say what it identifies.
   Then call get_intel_identifier_catalog(samples: up to 40 values of each candidate id column, never CPF, which comes masked) and use the id_type of its matches (system_id:pontotel is id_type system_id with namespace pontotel). A column that matches nothing gets the id_type the user confirms.
2. list_intel_definitions(kind event) once, to reuse existing event and property names. Map the person name (set_once nome) only when no roster brings it: when list_intel_definitions(kind "person_property", search "nome") returns nome, names already come from another source.
3. Write the mapping spec (references/mapping.md) and test it: dry_run_intel_mapping(upload_id, spec, drop_columns, limit 200). Read summary (sample_rows, total_rows, skipped, events, persons new/existing/pending), errors, warnings and the samples. Fix and test again, at most 3 dry runs. Errors in a few rows of a dirty file are fine; errors in most rows mean the spec is wrong.
4. Choose the mode:
   - period: the file is a report of a date range that will be sent again for other ranges or corrected (names like 20261001-20261115_relatorio.xlsx or r19-01-07-2026-a-31-07-2026.xlsx). Also the choice for a dated report (daily punches, atestados of a month) with no unique key: in incremental, a corrected row without a key becomes a second record. Set period_column (the date column that places each row, exactly as in the header) and period_format (a format the server accepted for that column in the inspection).
   - total: each file is the complete current snapshot of the whole base (a roster).
   - incremental: a log that only grows; the default when unsure.
   If the user already said how the files come, follow it.
5. key_fields: only columns whose values are unique per row (a protocol, a record id). Never the cpf or matricula of a person with many rows; empty when no column is unique (the server refuses keys that repeat).
6. save_intel_source(name, description, kind "file", mode, key_fields, drop_columns, period_column, period_format, upload_id, spec). Name: short, says what each row is, never the file name. Description: one sentence with what each row is, the period or snapshot and the person identifier.

## 5. Report
list_intel_runs(source_id) until the run is done. Tell the user rows read, events written, rows with error and rows waiting for identification, and what each event means. Errors: list_intel_error_groups explains them (references/diagnosis.md). Then offer the data dictionary when list_intel_definitions shows unlabeled fields (references/dictionary.md).
