Maple Freight has traded through only a few sharp increases in fuel prices. Its own history gives us some evidence about what might happen next. Other carriers offer more cases, though they differ in routes, contracts, and how much fuel they buy at fixed prices.

**Partial pooling** lets a model learn from both sources. An industry estimate gives Maple a starting point. As Maple's own record becomes more informative, its estimate can move farther from the industry average.

## Share evidence where it belongs

A Bayesian hierarchy can place companies within industries, industries within sectors, and sectors within the market. The model learns how much firms vary within a group. A firm with a short or noisy record draws more from its group. A firm with a strong record can stand farther apart. [Gelman on variance in hierarchical models](https://stat.columbia.edu/~gelman/research/published/multi2.pdf).

Some exposures cut across the industry tree. An airline and an oil producer can both respond to crude prices, with effects in opposite directions. A model can include a shared oil factor with distinct company sensitivities. Learned peers can add support when their membership is known at issue time and their forecasts beat the formal industry groups on future dates.

## How the weights work

Take a made-up example for one fixed return horizon. The industry prior has a mean of 1 percentage point and a variance of 1 squared percentage point across company means. Maple has four independent observations averaging 3 percentage points. Each observation has a noise variance of 4 squared percentage points.

In this simple normal model, the posterior mean combines the company sample mean $\bar y$ with the prior mean $m$. If $n$ is the sample count, $\tau^2$ the prior variance, and $\sigma^2$ the observation variance, then

$$
E[\theta\mid y]=w\bar y+(1-w)m,\qquad
w=\frac{n\tau^2}{n\tau^2+\sigma^2}.
$$

Maple gets half the weight because $4/(4+4)=0.5$. Its estimated mean is 2 percentage points. The uncertainty about that mean has a variance of 0.5 squared percentage points. Predicting a new return also requires the 4 squared percentage points of observation noise. That gives a predictive variance of 4.5 squared percentage points.

Real observations overlap and firms share market shocks. Four rows can carry less evidence than four independent observations. A full hierarchy can also carry uncertainty about the industry averages and how much companies differ.

## Carry uncertainty through the forecast

One way to make the forecast is to draw a possible company mean from its posterior and then draw a new return given that mean. Repeating both draws gives the predictive distribution. The first draw carries what we still need to learn about Maple. The second carries the variation that remains even for a known mean.

The same idea applies across firms. If several companies share a market factor, draw that factor once for each simulated market outcome and apply it to all of them. Drawing it separately for each company would hide a source of shared losses. The company estimates can share evidence and still carry different risks.

## Let the estimates change

A company's costs and debt can change faster than a long training window reflects. A state-space model lets a weight change over time. It updates the weight as new results come in. Small steps make it slow to react. Large steps can make it chase noise.

Check the group assumptions when business changes. A carrier that shifts routes or locks in fuel prices can become a worse match for its old peers. Compare the new and old group on dates after the change was known. For the tests, track the number of distinct dates and firms as well as the number of rows.

Test the hierarchy against one market-wide model and separate company models. Compare the three on the same future dates, with particular attention to firms and industries with short histories. When an estimate relies on broader sector evidence, the forecast should name that group and the dates that support it.

---

[Building a Brain](/public/articles/building-a-brain) · [Clustering, peers, and changing regimes](/public/articles/brain-clustering-and-regimes) · [Combining forecasts and calibrating confidence](/public/articles/brain-combining-and-calibrating) · [Point-in-time memory](/public/articles/brain-point-in-time-memory)
