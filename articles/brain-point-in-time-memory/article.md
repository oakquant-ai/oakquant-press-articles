Maple Freight releases its first-quarter earnings in May. A July correction changes the figure. To replay a June forecast, use the May version. To study what the revised accounts say today, use the July version. Both numbers belong in the record.

## Rebuild what was known

Keep three times for each version. The observation period says when the activity took place. The release time says when the source made the version available. The receipt time says when our system got it. If a check or license delays use, record when the value cleared that check too.

A **vintage** is a version of the data as it stood at a given time. ALFRED lets researchers ask what an economic series showed on a past date. A vintage gives a view of the source history. A receipt gives a view of what reached the forecast engine. [St. Louis Fed on real-time periods](https://fred.stlouisfed.org/docs/api/fred/realtime_period.html).

An **as-of join** attaches a feature to a forecast row by looking back from that row's issue cutoff. Match the company or series and the record it describes. Take the most recent eligible version under a stated revision rule, within the feature's allowed age. If two records share a time, use a version or source order defined in advance. If no record meets the rule, leave the feature missing. [Feast on point-in-time joins](https://docs.feast.dev/v0.50-branch/getting-started/concepts/point-in-time-joins).

| Record for Maple's first quarter | Received | Use in a June replay |
|---|---|---|
| Original May filing | May 8 | Yes, if the forecast falls within its allowed age |
| July restatement | July 12 | Wait until that version is available |

Apply the same rule to sector members, supply-chain links, and company descriptions. A date on a row tells us what time it describes. The source receipt tells us when we had the right to use it for that forecast.

## Check when the answers were known

A training row has a second clock problem. A five-session forecast made just before a test period can use older inputs, yet its return will depend on prices inside the test period. At a fit cutoff, exclude any training row whose outcome was still open or whose checked result had yet to arrive. Removing rows whose target windows cross a split is a form of **purging**.

In a walk-forward test, fit on earlier dates, predict the next block, then move the cutoff forward. A time gap can add separation where windows overlap or labels arrive late. The gap comes from the horizon, the data delays, and the split design. Check each label's availability as well. A fixed gap alone cannot tell us which results had cleared by the cutoff.

Fit the scaler, peer groups, feature selection, and model inside each training window. Apply those same fitted steps to the next block. Select lags and other settings on earlier validation blocks. Save a final period for the test. Overlapping outcomes and market shocks still affect how much evidence the final scores carry. [Scikit-learn on leakage in preprocessing](https://scikit-learn.org/stable/common_pitfalls.html#data-leakage).

## Make the replay prove itself

Move the May filing's receipt past the cutoff. The feature should vanish from the replay. Add the July restatement. The June row should stay the same. Push a training label's receipt past the fit cutoff and check that the model no longer uses it.

For each forecast, keep the source versions, the cutoff, the fitted model, and the data transforms that went into it. A replay should reproduce the issued inputs and forecast under the saved policy. Later corrections can inform new forecasts and scores while the original record remains available for comparison.

---

[Building a Brain](/public/articles/building-a-brain) · [Giving the brain good senses](/public/articles/brain-data-and-cost) · [Temporal learning and the order of events](/public/articles/brain-temporal-deep-learning) · [Learning from outcomes](/public/articles/brain-learning-from-outcomes)
