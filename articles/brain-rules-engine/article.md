A forecast needs a feed the caller has no right to use. The system checks the request against the access rules and stops. It can make that choice without a record of who got access in the past.

A **rules engine** evaluates those explicit conditions. Given the same inputs and rule version, it makes the same choice. The forecasting models have a different job. They estimate market outcomes from evidence.

## Make the order explicit

The order of checks matters when more than one condition applies. A request can lack permission and also be missing data. Under a first-match policy, the first applicable row determines what happens next.

| Condition | Action |
|---|---|
| The caller lacks permission | Reject the request |
| The target is invalid | Ask for a valid target |
| A required source is unavailable | Use an approved fallback if it can run, or report the gap |
| No approved model can run | Report which evidence is missing |
| The forecast contract is satisfied | Run the approved forecast policy |

Some tables collect all the matching results. Others allow just one row to match. The **hit policy** tells the engine which rule to use when it reads the table. [Decision Model and Notation specification](https://www.omg.org/spec/DMN/1.4/).

## Enforce the forecasting contract

A five-session forecast needs an issue cutoff, a starting price, and a named target close. The system checks those fields before it issues an answer. A model can use a fallback only if that fallback supports the same target and has the required inputs.

The same rules apply after issue. A label becomes eligible for scoring when the checked outcome is available. A retry can finish work that was interrupted, and the event key prevents it from counting the same outcome twice. If a source revises a price, the system records a new outcome version and reconciles the affected learning updates.

Arithmetic follows explicit rules too. The value of a holding comes from the number of shares and the price. The published explanation takes its probabilities and return ranges from the saved forecast. Save the inputs and the rule version so the same checks can be run again.

## Keep the rule and the decision together

A decision record needs the rule version, the facts it checked, the first row that matched, and the result. If a rule refers to the current time or to a source's status, save the value it saw. Otherwise a replay can run the same table and take a different path.

Give missing values their own cases. A feed that has yet to arrive calls for a different response from one that arrived and failed a check. An unknown permission leaves the request blocked. These distinctions let us test the reason for the result, as well as the result itself.

A rule can select an action more than once if a job is retried. The step that carries it out needs its own guard. For example, a forecast record can have one stable ID so a second attempt to save it returns the existing record.

## Test the policy as well as the code

A test can prove that a rule takes effect at the stated threshold. A separate evaluation asks whether the threshold makes the system better. A rule that routes away from a stale feed can be tested at the freshness boundary, then judged on the quality and coverage of the forecasts it produces.

The same distinction applies to trading heuristics. A rule that issues a bullish call after a large price rise makes a market claim. It needs the same comparison against a baseline on future dates as a learned model.

---

[Building a Brain](/public/articles/building-a-brain) · [Learning from outcomes](/public/articles/brain-learning-from-outcomes) · [Giving the brain imagination and words](/public/articles/brain-generative-capability)
