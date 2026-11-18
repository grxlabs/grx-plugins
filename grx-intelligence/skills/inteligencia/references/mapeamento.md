# Mapping spec

Turns each raw record (a row or an API record) into person identifiers, person properties and events.

```
{"version":1,
 "skip_when":[cond],
 "explode":{"path":"items","as":"item"},
 "person":[{"id_type":"cpf|email|matricula|phone|system_id","from":"Col","ops":[...],"namespace":"only for system_id, e.g. wellhub"}],
 "set":{"prop": value}, "set_once":{"prop": value},
 "events":[{"name": value, "when":[cond], "timestamp":{"from":"Col","format":"dd/MM/yy","ops":[...]},
            "ends_at":{...}, "uuid": value, "properties":{"prop": value}}]}
```

- value: "Col" (shorthand for {"from":"Col"}), {"from":"Col" or ["ColA","ColB"] (first not empty), "ops":[...]} or {"value": fixed}. A fixed event name MUST be {"value":"atestado"}; a bare string is read as a column name.
- Paths: a.b, list[0], ["Col. with dots"]. Meta fields: $received_at $row_number $source_id $index.
- ops: trim lower upper digits remove_accents cpf email cep number_br ("1.234,56") number phone{country} pad{length,char,side} date{format} map{values,default} regex{pattern,group|replace} split{separator,index} default{value} concat{from[],separator} unit{from,to} duration{to:"h"|"min"} ("03:15", "Duração 03:15", "1h30", "90 min" become a number) null_if{values:[...]} (listed values mean empty).
- Date formats: tokens dd MM yyyy yy HH mm ss, or iso, excel_serial, unix_s, unix_ms. Pick from the date_formats the inspection lists for the column. Text after the date that has no digits ("16/09/26 Qua") is ignored, so the plain format works.
- cond: {"from":"Col","ops":[],"is":"eq|neq|in|not_in|empty|not_empty|matches|gt|gte|lt|lte","value":...}.
- Events that last (atestado, afastamento, férias) get "ends_at": {"duration_from":"Dias","unit":"days|hours"} (counted from the timestamp) or {"from":"Data fim","format":"dd/MM/yyyy"} (an end date without time includes that whole day).
- In the employee roster (HRIS, ERP or cadastro with admission and termination dates), give matricula and email their validity: "valid_from":{"from":"Admissão","format":"dd/MM/yyyy"}, "valid_to":{"from":"Demissão","format":"dd/MM/yyyy"}. Reused matrículas and e-mails then go to whoever held them on the event date.

## How to map well
- Identifiers: every column that identifies the person goes in person: cpf with op cpf, email with op email, matricula digits only, and a code of a system as system_id with that system's namespace (pontotel, wellhub). More ids mean more merges across sources.
- Event names come from what happened. When one column names the KIND of occurrence (MOTIVO, TIPO, a category with a handful of values: Atestado Médico, Declaração de Horas, Licença Casamento), the event name comes from that column, mapped to snake_case Portuguese ({"from":"MOTIVO","ops":[{"op":"map","values":{"Atestado Médico":"atestado_medico"},"default":"ausencia"}]}). Do not create one generic event with the kind as a property. Reuse existing names from list_intel_definitions.
- One row can yield several events with when conditions (a time-clock row: "dia_trabalhado" when Entrada is not empty, "falta" when Horas Falta is not empty).
- Properties: every other column with analytic meaning is an event property (durations with duration, dates with date, categories as text). What describes the PERSON and repeats across rows (name, workplace, department, role) is a person property: set_once from secondary sources, set when it changes over time or comes from the master roster. Never in the event.
- Codes and registrations (CRM, CNPJ, matrícula of another system, CEP, protocol) are text: use trim so no leading zero is lost; never number or digits.
- Placeholders that mean nothing ("000.0 Sem CID", "N/A", "-", "Não informado") show up in the frequent values of the inspection: end every property fed by such a column with null_if.
- A code glued to its description (CID "A09 Diarreia") becomes two properties: regex{pattern:"^([A-Z]\\d{2}(?:\\.\\d)?)",group:1} for the code and regex{pattern:"^[A-Z]\\d{2}(?:\\.\\d)?\\s*",replace:""} for the text.
- The launch date of a record is a property, never the event timestamp when another column dates the occurrence.
- Rows that only continue the previous one (an extra punch on the same day with no date and no id) carry no new fact: skip_when the date column is empty.
- Rows about a dependent or family member that carry the employee's ids (Wellhub "Subscriber Type": Family Member) must not count as the employee's own activity: give them another event name ("checkin_wellhub_dependente"). Drop the family member's name and e-mail columns.
- Links that open private files (signed URLs with an expiry) are flagged sensitive and dropped; they expire anyway.
- Event and property names in snake_case Portuguese (atestado_medico, dias, cid, unidade).

Example (atestados): person [{"id_type":"cpf","from":"CPF","ops":[{"op":"cpf"}]},{"id_type":"system_id","namespace":"pontotel","from":"CODIGO","ops":[{"op":"digits"}]}], set_once {"nome":"FUNCIONARIO","local_trabalho":"LOCAL DE TRABALHO"}, events [{"name":{"from":"MOTIVO","ops":[{"op":"map","values":{"Atestado Médico":"atestado_medico","Declaração de Horas":"declaracao_de_horas"},"default":"dispensa"}]},"timestamp":{"from":"DIA","format":"dd/MM/yyyy"},"properties":{"horas":{"from":"QUANTIDADE HORAS","ops":[{"op":"duration","to":"h"}]},"especialidade":"ESPECIALIDADE","cid_codigo":{"from":"CID","ops":[{"op":"null_if","values":["000.0 Sem CID"]},{"op":"regex","pattern":"^([A-Z]\\d{2}(?:\\.\\d)?)","group":1}]}}}].
