# LinkedIn content pack: Lumen Ridge Lakehouse

## Publishing guardrails

- Say that Lumen Ridge is fictional and the rehearsal used synthetic data.
- Describe the numbers as measured rehearsal evidence, with their scope.
- Keep Azure charges and Databricks list-price usage separate.
- Link to the ebook and PDF; do not publish internal repository links, tenant IDs, workspace URLs, real credentials or source data.

## Lead article

### We rehearsed a lakehouse before real data arrived. Here is what changed.

Most data-platform failures are discovered too late: after a report has gone live, after a duplicate has changed a financial total, or after a “small” refresh has left a warehouse running for twenty minutes.

We tried a different approach. Before connecting a fictional operator’s real sources, we built the full lakehouse on synthetic data designed to misbehave. The rehearsal covered online casino, sportsbook, wallet and payments data, along with retail bets, cash, floats and shifts.

The point was not a pretty demo. The generator created late arrivals, whole-file re-sends, conflicting versions with the same timestamp, missing settlements and changing reference data. Then we had to prove that the platform could catch and explain them.

Three lessons stood out.

**1. Integrity has to stop the run.**

Bronze keeps the delivered evidence. Silver has one row per declared business key. Gold has one fact per business process. A manifest check, grain test, uniqueness check, reconciliation ladder and “summary equals fact” test all fail closed. If one fails, the run stops and the next layer does not change.

In the rehearsal, 25 tested retail Silver and Gold grains had zero duplicate keys, including data with planted re-sends. That is not a claim that every future source will be clean. It is evidence that a declared grain and an enforced gate are more useful than a dashboard warning after the fact.

**2. The most effective performance work happened upstream.**

The first report refreshed against row-level facts and ran for more than twenty minutes. We moved the common calculations into seven daily Gold summaries and made the report use those first. The summaries built in 53 seconds for about USD 0.04 and matched their underlying facts in both rows and money.

That changed Power BI from a repeated scan of tens of millions of rows into a small, reconcilable read. Detail data remains available for drill-through, but it is no longer the default route for every visual.

**3. Dev and production must be separated by a real boundary.**

The most important line in the architecture is the one with no arrowhead. Development uses synthetic or masked samples. Production ingests its own source data into its own Bronze layer. Only reviewed code, data contracts, tests, job configuration and approved infrastructure definitions cross the promotion boundary.

This keeps test data out of production and forces production results to be reproducible from production evidence.

The rehearsal landed 221.6 million synthetic rows: 106.7 million online and 114.9 million retail. It also taught us to distinguish Databricks list-price workload from the Azure invoice, to use serverless by default, and to treat fixed network costs as a design choice rather than an afterthought.

The full public write-up, evidence limits and diagrams are here: https://anthonyapollis.github.io/lumen-ridge-lakehouse/

Downloadable PDF: https://anthonyapollis.github.io/lumen-ridge-lakehouse/assets/Lumen-Ridge-Lakehouse-ebook.pdf

#DataEngineering #Databricks #dbt #DataQuality #PowerBI

## Short post 1: integrity

**A duplicate check at the end of a pipeline is too late.**

In our fictional lakehouse rehearsal, we treated integrity as a gate:

- Bronze preserved every delivery as evidence.
- Silver enforced one row per declared business key.
- Gold enforced one fact per business process.
- A failure stopped the run before reports changed.

We tested 25 retail Silver and Gold grains against deliberately re-sent data: zero duplicate keys.

The result matters because of the control, not because “zero” is a magical number. Future sources still need their own contracts, keys, replay tests and reconciliation.

The practical lesson: define table grain in plain language, encode it as a test, and make the test fail closed.

Full rehearsal: https://anthonyapollis.github.io/lumen-ridge-lakehouse/ebook.html#ch4

#DataQuality #DataEngineering #Databricks #dbt

## Short post 2: cost and reporting

**Our first report refresh ran for more than 20 minutes. The fix took 53 seconds.**

The report was reading row-level facts by default. We moved the repeated calculations upstream into seven daily Gold summary tables.

Measured rehearsal result:

- summaries built in 53 seconds;
- about USD 0.04 to build;
- row and money totals matched the detail facts;
- Power BI could use the small summary first and reserve detail for drill-through.

This is why aggregation belongs in the data product when the calculation is shared. It reduces warehouse time, makes metrics consistent and gives reconciliation a clear target.

The source data was synthetic and Lumen Ridge is fictional, but the design lesson applies widely.

Full explanation: https://anthonyapollis.github.io/lumen-ridge-lakehouse/ebook.html#ch6

#PowerBI #DataEngineering #Databricks #AnalyticsEngineering

## Short post 3: dev and production

**The most important line in a dev-to-production diagram has no arrowhead.**

Raw dev data does not flow into prod.

Development runs on synthetic or masked data. Production ingests its own sources into its own Bronze layer. The only things promoted are reviewed code, schemas and data contracts, tests, job configuration and approved infrastructure definitions.

That boundary protects production data, removes a dangerous shortcut, and makes every production result traceable to its own source evidence.

The rest of the design follows from it: separate catalogs, storage, identities and secrets; production gates that stop and alert; and a protected backup with restore tests.

See the diagram and the rationale: https://anthonyapollis.github.io/lumen-ridge-lakehouse/ebook.html#ch9

#DataGovernance #DevOps #Databricks #DataEngineering

## Visual plan

### Lead article cover

Use the generated colour illustration supplied with this pack. It shows the four layers, a red integrity gate and reporting output. Leave the upper third clear for an optional title overlay:

> Rehearsing a lakehouse before real data arrives
>
> What 221.6M synthetic rows taught us about integrity, cost and promotion

**Alt text:** Four colour-coded data layers receive synthetic inputs, pass through a red integrity gate and feed business dashboards, representing a controlled lakehouse rehearsal.

### Carousel sequence

1. **Cover:** “We rehearsed the data platform before real data arrived.”
2. **Integrity:** “25 tested grains. 0 duplicate keys.” Add the scope: “Synthetic rehearsal; declared-grain checks.”
3. **Cost:** “20+ minute refresh -> 53-second Gold summary build.”
4. **Promotion:** “No raw dev data flows to production.” Use the colour-coded promotion-barrier figure from the ebook.
5. **Close:** “Build controls as gates. Reconcile every hop. Aggregate upstream.” Link to the ebook.

Use the ebook’s amber for Bronze, teal for Silver, green for Gold, violet for controls and red only for a failure boundary. Keep each slide to one idea and one evidence statement.

## Suggested sequence

1. Publish the lead article with the cover visual and ebook link.
2. Two or three days later, publish the integrity post with the fail-closed-gate figure.
3. The following week, publish the cost and Gold-summary post with the before-and-after row chart.
4. Close with the dev-to-production post and Figure 4 from the ebook.

