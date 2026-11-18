# Questions, charts, dashboards, cohorts and alerts

## Answering a question about the data
1. list_intel_definitions (kind event, or search) to find the real event and property names, their friendly label, description and unit (horas, dias, R$, %).
2. run_intel_query with the right kind: trends (series over time, breakdown, compare), events (the records), persons (one row per person with metrics and having), funnel, retention, matrix, scatter, paths. Every query needs date_range.from ("-30d", "this_month", "2026-10-01"); weeks start on Monday in the company time zone. source restricts a series to one source (id or exact name). The schema of the tool describes every field.
3. Answer with the numbers in one or two sentences: the biggest deviation or the top item ("Quem fez 4+ check-ins teve 15,8% com atestado, contra 23,8% de quem não usa"). Use the friendly label, never the technical name. Say when a sample is too small to conclude.

Crossing sources: persons with events in each source. A cohort with match "all" and one behavior per source (or per event with operator and count) selects them; preview_intel_cohort counts without saving, create_intel_cohort saves it, and cohort_id restricts any query to it. Example: "pessoas do ponto com 4+ check-ins Wellhub" versus "com 0 check-ins" (operator "eq", count 0).

## Dashboards
- list_intel_dashboards and get_intel_dashboard show what exists. create_intel_dashboard(name, widgets) creates one with up to 20 widgets; add_intel_widget(dashboard_id, title, kind, query, layout, options) adds one; update_intel_widget, delete_intel_widget and set_intel_dashboard_layout (12-column grid) change them.
- Test every query with run_intel_query before turning it into a widget. In the console chat, pass the artifact_id of that result as query_ref instead of the query.
- One chart per question; titles in plain Portuguese with the friendly labels; display options (format, decimals) from the unit.
- run_intel_widget runs a saved widget; drill_intel_widget(widget_id, point) returns the events or persons query behind a clicked point, to answer "who are they".

## Alerts
create_intel_alert(widget_id, name, condition, schedule_minutes, recipients) watches a trend widget (number, line or bar): above, below, change_up_pct or change_down_pct. test_intel_alert measures now without sending. list_intel_notifications shows what fired.
