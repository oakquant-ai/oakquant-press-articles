One model expects Maple Freight to gain. Another expects it to lose about as much. Take the average and the forecast comes out near zero. To see how far the two models differ, we need the range each one puts on the gain and the loss.

## Combine the distributions

A **weighted mixture** takes a share of each model’s distribution. The baseline can have a share too. Let $F_m(r)$ be the chance that model $m$ gives to a return at or below $r$. The mixture adds those chances. Each share $w_m$ is zero or more, and the shares add to one.

$$
F(r)=\sum_{m} w_{m} F_{m}(r).
$$

Take two equally weighted models centered at −2 and +2 percentage points. Each has a variance of 1 squared percentage point. The mixture mean is zero. Its variance is 5 squared percentage points. One comes from uncertainty within each model, and four come from the difference between their means.

Read the lower and upper bounds from the mix. Averaging the bounds from each model can hide how far apart they are. If a model gives only a few quantiles, it still needs a way to describe the gaps and the tails before it can join the mix.

## Let settled forecasts set the weights

Fit weights on dates the models did not train on. Save later dates to test the mix. A model can earn more weight for an industry or horizon if its scores support it. Where the record is thin, draw on a broader group and name it. Look for models that miss on the same dates.

A **proper scoring rule** rewards an honest probability in expectation. For a gain forecast with probability $p$ and outcome $y$ equal to 1 for a gain and 0 otherwise, Brier loss is $(p-y)^2$. A full distribution can be judged by the continuous ranked probability score, which compares its cumulative probabilities with the realized outcome across return thresholds. [Gneiting and Raftery on proper scoring rules](https://sites.stat.washington.edu/people/raftery/Research/PDF/Gneiting2007jasa.pdf).

One online update, called **Hedge**, multiplies each weight by $\exp(-\eta\ell_m)$ and normalizes the result. Here $\ell_m$ is the loss on eligible settled cases, and $\eta$ controls how fast the weights move. Wait for the right horizon and compare specialists on cases they share. A model that skips the hardest dates can otherwise appear stronger than it is. [Joulani, Gyorgy, and Szepesvari on delayed feedback](https://proceedings.mlr.press/v28/joulani13.html).

Compare the blend with its strongest single model and the baseline on the same dates. Keep the fit dates for weights and calibration apart from the final test. A score gap is easier to trust when it survives several blocks of dates. Resample blocks of market sessions to estimate how much the gap moves when shared shocks stay together.

## Give each number its own meaning

The **chance of gain** concerns the event ahead. The **predictive interval** describes plausible sizes of returns. **Historical skill** reports how earlier forecasts scored against a baseline on comparable cases.

Calibration asks how often the event took place for forecasts with about the same stated chance. Suppose 55 of 100 forecasts near 0.70 ended in gains. Check how much of that gap could come from noise or shared shocks. Fit any correction on earlier settled cases. The final chance of gain and the range should agree with the distribution shown to the reader.

If a calibration map moves the chance of gain from 0.60 to 0.50, the distribution needs to reflect that change. One approach keeps the return patterns within gains and within non-gains, then gives the two groups new shares. Find the return range again from that revised distribution. Test both the event score and the range coverage, since a change that helps one can harm the other.

For each sector and horizon, count how often the return falls inside the range. Check how wide the ranges had to be. **Adaptive conformal inference** can use recent misses to adjust a range and target coverage over time. A rate across the whole market can hide gaps within a sector. [Gibbs and Candès on adaptive conformal inference](https://arxiv.org/abs/2106.00170) · [Barber et al. on conditional coverage](https://arxiv.org/abs/1903.04684).

---

[Building a Brain](/public/articles/building-a-brain) · [Bayesian hierarchies and borrowed evidence](/public/articles/brain-bayesian-hierarchies) · [Learning from outcomes](/public/articles/brain-learning-from-outcomes) · [Point-in-time memory](/public/articles/brain-point-in-time-memory)
