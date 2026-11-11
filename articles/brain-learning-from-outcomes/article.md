A five-session forecast can be issued today. The answer arrives at the fifth trading close, and the system may have to wait longer for a checked price. Learning starts by keeping those times separate.

## Keep the forecast that went out

Save the target, issue cutoff, input snapshot, model version, and distribution at issue time. When the answer arrives, settle against the named target session. If collection runs late, the collector still needs that session's close. Using a later price would score a different question.

Several specialists can forecast the same company and horizon. Each deserves a score on its own prediction, using the same verified outcome. Ten specialists give us ten forecasts and one market event. Shared shocks and overlapping dates also reduce the amount of independent evidence in a scorecard. [Dawid on evaluating forecasts in sequence](https://rss.onlinelibrary.wiley.com/doi/10.2307/2981683).

## Score the claim the model made

Suppose four forecasts each assign a gain probability of 0.60. Two end in gains. Their average Brier loss is 0.26. A baseline that assigns an even chance to each of those four cases scores 0.25, where lower is better. If a tie counts as a gain call, both get two calls right. The loss tells the two forecasts apart.

A score for a probability judges a different claim from an error in a predicted mean return. The full distribution has its own scores. Compare forecasts on the same cases, and report how much evidence supports the gap. Four outcomes are enough for the arithmetic. A claim of lasting skill requires a much larger and more varied record. [Gneiting and Raftery on proper scoring](https://doi.org/10.1198/016214506000001437).

## Apply each settled outcome once

A retry can resume scoring after a job fails. A stable event key and a saved record of the applied update keep it from changing the model twice for the same event. When a source corrects an outcome, save the new version and reconcile the affected scores and learning state. The original forecast stays available for comparison.

Online updates can change a coefficient, shift weight among specialists, or adjust a calibration map. Each process needs a defined rule for late outcomes and the state it updates. A long horizon creates a queue of open forecasts that can be scored only when checked outcomes arrive. [Joulani, Gyorgy, and Szepesvari on delayed feedback](https://proceedings.mlr.press/v28/joulani13.html).

## Reconcile a correction in order

Suppose two checked outcomes arrive and each changes the weights. The first is later corrected. The second update used the state left by the first, so replacing one score in a table leaves the current weights wrong. Start from the saved state before the changed update. Apply the new result and replay the later updates in the policy's recorded order.

Keep the earlier outcome, its correction, and the time we learned about each. A study of forecasts as issued can still use the original record of what the learner knew then. A revised scorecard can use the corrected outcomes and name its version. Forecasts already sent to readers keep their original probabilities.

## Decide when a larger refit earns its cost

An update responds within an existing model and policy. A refit can change the history window, features, or parameters more broadly. Fit and test that candidate separately. Keep a record of data versions, method changes, and trials, including failures.

For a meaningful comparison, replay a frozen policy in time order, with each update using only evidence available at that point. Test the candidate against the incumbent and baseline on untouched dates. Heavy fits can run away from the forecast-serving path while the approved model continues to issue forecasts. Use the new fit when it scores better, works on more of the required cases, or costs less for the same quality.

---

[Building a Brain](/public/articles/building-a-brain) · [Combining forecasts and calibrating confidence](/public/articles/brain-combining-and-calibrating) · [Why a learning brain needs rules](/public/articles/brain-rules-engine) · [Point-in-time memory](/public/articles/brain-point-in-time-memory)
