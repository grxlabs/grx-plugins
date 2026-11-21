# Finding insights: checklist

Use it for an open request ("o que os dados mostram?", a dashboard for leadership, a dashboard that looks generic). A named comparison ("quem treina falta menos?") is a specific question: references/analysis.md. The method, the hypothesis catalog and the bar for a finding: https://business.gymrankx.com/docs/intelligence/insights.md (the insights.md#... anchors below are its sections).

1. Map the ground (insights.md#1-map-the-ground): overview, sources and their periods, definitions, cohorts. Write down the overlap window and the base population.
2. Write hypotheses from the catalog (insights.md#2-generate-hypotheses-before-querying), favoring ones that cross sources and that a manager can act on.
3. Test each with the cheapest query, always as a rate over the base (insights.md#3-test-each-hypothesis-with-the-cheapest-query). Stop a hypothesis as soon as it clearly fails.
4. Keep only what passes the bar; rank and propose the findings, each with the claim, why it matters, the confidence (strong or tendency) and the chart that proves it (insights.md#4-rank-and-propose). Saving the cohorts they need is fine; build nothing else.
5. Show the findings numbered and let the manager choose which go to a dashboard (insights.md#5-let-the-manager-choose): the client's question UI when it has one, else one short question. If they only wanted the analysis, stop.
6. Build exactly the chosen ones (insights.md#6-build-exactly-the-chosen-ones) as references/analysis.md says, with the period rule of that section.

Delegation: the find-insights agent does items 1 to 4 (map, hypotheses, tests, ranked findings) and returns the report; you show the findings, let the manager choose and build.
