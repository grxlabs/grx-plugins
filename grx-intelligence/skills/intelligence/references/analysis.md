# Questions, charts, dashboards and alerts: checklist

Use it for a specific question or a named chart, comparisons included ("quem treina falta menos?"): build directly. An open question or a leadership dashboard goes to references/insights.md.

## A question about the data
The required page: https://business.gymrankx.com/docs/intelligence/queries/index.md. Open when the question needs it: groups of people compared, https://business.gymrankx.com/docs/intelligence/queries/comparing-groups.md; saved groups, https://business.gymrankx.com/docs/intelligence/queries/cohorts.md; a value spread per person, https://business.gymrankx.com/docs/intelligence/queries/distribution.md; running totals and smoothing, https://business.gymrankx.com/docs/intelligence/queries/time-transforms.md
1. list_intel_definitions for the real names, labels and units; a business word can be several events.
2. run_intel_query with an explicit date_range. Crossing sources or comparing groups: the overlap window of the sources, cohorts (preview_intel_cohort, create_intel_cohort) and a rate over math "persons", as comparing-groups.md says.
3. Answer in one or two sentences with the number, the comparison and the base, using friendly labels; say when a sample is too small.

## A chart or a dashboard
The required page: https://business.gymrankx.com/docs/intelligence/dashboards/choosing-charts.md. Open when you need them: tools and fields, https://business.gymrankx.com/docs/intelligence/dashboards/index.md; a kind and its example, https://business.gymrankx.com/docs/intelligence/dashboards/catalog.md; overlays, https://business.gymrankx.com/docs/intelligence/dashboards/overlays.md; worked HR questions, https://business.gymrankx.com/docs/intelligence/dashboards/examples.md; what the server refuses, https://business.gymrankx.com/docs/intelligence/capabilities.md
1. list_intel_dashboards and get_intel_dashboard to see what exists.
2. Pick the widget from the shape of the answer (choosing-charts.md#1-name-the-answer-then-pick-the-widget).
3. Test every query with run_intel_query; send its query (in the console chat, query_ref).
4. create_intel_dashboard or add_intel_widget with an idempotency_key: takeaway title, options.subtitle, highlight and reference line when they carry the point.
5. Fix every warning (update_intel_widget, set_intel_dashboard_layout), then run_intel_widget each widget and fix what reads wrong.
6. Tell the user in one or two lines what it shows.
"Who are they" on a point: drill_intel_widget, then run_intel_query.

## Alerts
The required page (what is measured, where the widget goes, conditions, recipients): https://business.gymrankx.com/docs/intelligence/alerts.md
1. The widget the alert watches (alerts.md#the-widget-of-an-alert): on an existing dashboard of the topic, else on a dashboard named "Alertas"; a number widget whose trends query measures exactly the period of the alert (this_month for "por mês", -7d for "por semana"), with one series that lists every event of the business word. Dashboard filters never reach it.
2. create_intel_alert with an idempotency_key; "me avise" is the user e-mail from whoami, passed as received.
3. test_intel_alert, and tell the user the value it measures now and on which dashboard the widget is. list_intel_notifications shows what fired.
