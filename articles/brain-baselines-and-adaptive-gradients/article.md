A model gets six of ten calls right. A rule that calls a gain every time gets seven right on the same ten cases. The new model still has something to prove. A **baseline** gives the comparison a fixed starting point.

## Match the benchmark to the target

A five-session forecast fixes its starting price, issue cutoff, and fifth trading close. A forecast of total return includes the specified dividends. A forecast of sector-relative return measures the gap against a defined benchmark. Each target needs its own baseline, scored on the same cases and dates.

A no-change price forecast gives a point estimate. For a fair comparison of probabilities or intervals, the baseline also needs a distribution. It can use earlier returns at the same horizon and scale their spread by trailing volatility. A market or sector model gives a second reference for testing whether company-specific data adds value.

For a probability baseline, count how often the same event occurred in a prior window. If 30 of 50 comparable cases ended in gains, that window gives a gain rate of 0.60. Freeze the window rule before testing and compare probability scores on the same forecasts. In a rising market, a model can get many directions right while losing to this simple rate.

Keep both coverage and score. If the richer model can run on fewer stocks or dates, score the two on their shared cases. Then test the complete policy, including the fallback, across all cases it is meant to serve. The two tests answer different questions about the model and the service.

## Fit the weights without chasing noise

Linear regression estimates a weight for each input. If two inputs move together, very different weights can fit nearly the same history. **Ridge regression** penalizes large weights, which can make the fit more stable. A stronger penalty also shrinks useful effects, so choose it on earlier validation dates. [Scikit-learn on ridge regression](https://scikit-learn.org/stable/modules/linear_model.html#ridge-regression-and-classification).

In a one-feature example with no intercept, the ridge slope is the sum of feature-return products divided by the sum of squared feature values plus the penalty. With those sums set to 0.08 and 4, a penalty of 1 gives a slope of 0.016. An input of 0.5 then gives a forecast return of 0.8 percentage points. With a penalty of zero, the same fit predicts 1 percentage point.

## Learn from a settled miss

For a binary gain forecast, logistic regression maps a weighted score to a probability. If $p$ is the probability, $y$ is 1 for a gain and 0 otherwise, and $x$ is a feature value, the single-case log-loss gradient for that feature's weight is

$$
g=(p-y)x.
$$

Suppose the model gave a gain probability of 0.60 with an active indicator of 1. The outcome settles as a loss. With a step size of 0.10, plain gradient descent subtracts 0.06 from that indicator's weight in this example. That update responds to one observed miss. It takes repeated, comparable cases to judge whether the change helps.

**AdaGrad** uses the history of squared gradients to set a separate step size for each weight. As that history grows, steps can become too small to keep up with a changing market. **FTRL-Proximal** offers another option when many event indicators should receive zero weight. Both need a fair test against constant-step updates and scheduled refits. [Duchi, Hazan, and Singer on AdaGrad](https://jmlr.org/papers/v12/duchi11a.html) · [McMahan et al. on FTRL-Proximal](https://research.google/pubs/ad-click-prediction-a-view-from-the-trenches/).

A step size sets how much one settled result can change the model. Compare small and large steps under the same replay. Watch whether a cluster of losses makes the model overreact and whether a smaller step takes too long to respond. More than one forecast can depend on the same market move, so several misses on one date deserve care before they trigger several large updates.

The learner saves the features and prediction available at issue time. It waits for the correct target close and applies each eligible update once. The update rule says which version to change when feedback comes late. Score the forecast as issued. If the gradient uses a newer state, record that state with the update.

---

[Building a Brain](/public/articles/building-a-brain) · [Trees and the conditions that change an answer](/public/articles/brain-trees-and-interactions) · [Learning from outcomes](/public/articles/brain-learning-from-outcomes) · [Point-in-time memory](/public/articles/brain-point-in-time-memory)
