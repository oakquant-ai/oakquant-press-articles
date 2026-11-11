Higher fuel prices can have a different meaning for a carrier that has locked in its fuel costs. A model that adds a separate effect for each input needs an interaction term to express that difference. A tree can learn it by asking about one input only after it has split on the other.

## What a split can learn

A tree divides past examples at a threshold, such as whether a fuel price change exceeds a given level. It can divide each resulting group again. A terminal group, called a **leaf**, supplies the prediction.

A tree with one split is a stump. A sum of stumps can fit a curved response to each variable on its own. A tree with two levels can make the effect of one variable depend on the other.

| Fuel prices rose | Fuel costs are hedged | Illustrative return in percentage points |
|---|---|---:|
| No | No | −2 |
| No | Yes | +2 |
| Yes | No | +2 |
| Yes | Yes | −2 |

These invented numbers isolate an interaction. Each variable on its own splits the rows into groups with the same mean of zero. A two-level tree can express all four outcomes. A greedy fitting algorithm may stop at the first split because neither variable improves the fit by itself. This is a reason to test explicit interaction terms alongside trees.

## Improve the prediction in stages

**Gradient boosting** starts from a reference prediction and adds small trees. Each tree fits a correction to the combined prediction so far. Under squared error, the correction follows the remaining residual. Other losses define their own direction for improvement. The learning rate scales down each addition. [Friedman on gradient boosting](https://doi.org/10.1214/aos/1013203451).

Depth, minimum leaf size, and tree count affect how closely the ensemble can fit the past. Choose them on earlier validation dates. Beyond the range seen in training, a tree continues to route inputs into existing leaves. Its predictions cannot extend a trend the way a linear model can.

A split also needs enough evidence on each side. Suppose a new tree finds that an unusual fuel cost marks three of the worst cases in the training set. If all three came from one port closure, the leaf has seen one shock. Test minimum leaf size alongside support across dates and markets. That helps tell a repeatable condition from a narrow fit to a single episode.

## Estimate a range of returns

A quantile tree can estimate a cutoff for the upper or lower tail. At the 90th percentile, the **pinball loss** charges nine times as much for missing above the cutoff as for missing the same distance below it. If the cutoff is 2 percentage points, a realized return of 4 gives a loss of 1.8. A realized return of zero gives a loss of 0.2.

Separate fits can produce crossing quantiles. Check their order before issuing intervals. A few quantiles specify particular cutoffs. A full distribution needs an additional model or explicit assumptions about the gaps and tails. **NGBoost** takes another route by predicting parameters of a chosen distribution, such as its location and scale. The distribution family still has to fit the observed tails. [Duan et al. on NGBoost](https://proceedings.mlr.press/v119/duan20a.html).

Check each tail on held-out dates. For a well-calibrated 90th percentile, the return lands above the cutoff about once in ten comparable cases. A leaf can get the average return right and miss that tail often. For a range, also check how wide the bounds had to be to achieve coverage.

Use the same cases to compare trees with ridge regression and a linear model with explicit interactions. Shift a filing past the issue cutoff and check that its value disappears from the inputs. Mask a feed to see whether the trees learned a useful relationship or a quirk of the vendor's missing data.

---

[Building a Brain](/public/articles/building-a-brain) · [Baselines and adaptive gradients](/public/articles/brain-baselines-and-adaptive-gradients) · [Combining forecasts and calibrating confidence](/public/articles/brain-combining-and-calibrating) · [Point-in-time memory](/public/articles/brain-point-in-time-memory)
