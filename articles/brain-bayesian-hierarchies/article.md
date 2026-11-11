A newly listed manufacturer has seen only four major jumps in energy prices. A forecast built solely from those four episodes may swing wildly. A forecast that treats every manufacturer alike may miss the firm's unusually long fixed-price energy contracts. A Bayesian hierarchy starts with the group pattern, then lets this company depart from it as its own evidence grows.

The method gives sparse groups a starting point, lets well-observed groups differ, and carries uncertainty about both the group and the individual into a forecast. It does not make the chosen peers correct or turn a small sample into a large one.

![Three estimates for a synthetic company: its noisy four-observation mean at 3 percent, a group mean at 1 percent, and a partially pooled estimate at 2 percent; a narrower interval for the estimated mean is distinct from a wider interval for a future return.](images/partial-pooling.svg)

*Partial pooling moves a thin company estimate toward its group; predicting a new return adds market noise back to uncertainty about the mean.*

**In brief:** Use a hierarchy when related groups have uneven amounts of evidence. Its gain is more stable estimates for thin groups; its risk is shrinking a genuinely unusual company toward the wrong peers.

**Contents:** [Why use it?](#why-use-it) · [How it works](#how-it-works) · [The math](#the-math) · [Where OakQuant uses it](#where-oakquant-uses-it) · [Where it is used elsewhere](#where-it-is-used-elsewhere) · [Alternatives and tradeoffs](#alternatives-and-tradeoffs) · [People, libraries, and further reading](#people-libraries-and-further-reading)

## Why use it?

Suppose an analyst wants the average five-session return following an energy-price shock. Each company has a different number of usable past episodes. A single market estimate ignores real differences; a separate estimate for every company makes the least-observed names look the most dramatic. **Partial pooling** shares information without forcing all companies to have the same effect. The [Stan user's guide](https://mc-stan.org/docs/stan-users-guide/regression.html) describes these two extremes and the hierarchical middle ground.

The method matters most when group sizes are uneven, measurement noise is high, and there is a plausible reason to expect related effects. If the question only needs a market-wide average, a pooled baseline is simpler. If a company has abundant, representative observations and clearly differs from its peers, a separate estimate may be enough. Both comparisons should be tested on later, untouched dates.

## How it works

First, define one outcome and one horizon. Here it is a five-session return measured in percentage points after a specified energy shock. Decide what was knowable when each forecast would have been issued: the shock, company features, group membership, and any revisions to the data all need an **as-of** date. The four episodes in the opening are a synthetic illustration, not observed evidence about a real company.

Next, assign related companies a group-level distribution. That distribution has a center and a spread: the spread says how far company effects may plausibly depart from the group. Each company's observations update its own effect. A short, noisy history stays closer to the group center; a long, precise history can pull away. A deeper model may nest company, industry, sector, and market effects. A shared energy factor can also cross those groups, with different companies carrying different exposures. The hierarchy is an assumption about relationships to test, not a discovery that industry labels are always the right peers.

The output is a **posterior distribution** for the company's mean effect and a **predictive distribution** for its next outcome. The latter is wider because a future return varies even if the true mean were known. For joint forecasts, a market shock must be drawn once and applied across affected firms; drawing a fresh market shock for each firm would erase shared downside. Changing contracts or routes can also make old peers less relevant, so the grouping and time stability deserve checks on future data.

## The math

For a transparent one-level example, assume independent observations and known variances:

$$
\theta\sim\mathcal N(m,\tau^2),\qquad
y_i\mid\theta\sim\mathcal N(\theta,\sigma^2),\qquad
E[\theta\mid y]=w\bar y+(1-w)m,
\quad w=\frac{n\tau^2}{n\tau^2+\sigma^2}.
$$

Here $\theta$ is the company's unknown mean return, $m$ the group mean, $\tau^2$ the variance of company means around that group, $y_i$ one observed return, $\sigma^2$ the observation noise variance, $\bar y$ the company sample mean, and $n$ the number of independent observations. $w$ is the weight on company evidence. This is a conditional calculation with $m$, $\tau^2$, and $\sigma^2$ treated as known; a full Bayesian hierarchy also estimates them and propagates their uncertainty.

Set $m=1$ percentage point, $\tau^2=1$ squared percentage point, $n=4$, $\bar y=3$ percentage points, and $\sigma^2=4$ squared percentage points. Then $w=4/(4+4)=0.5$, so the posterior mean is $0.5(3)+0.5(1)=2$ percentage points. The posterior variance of the mean is $(1/\tau^2+n/\sigma^2)^{-1}=0.5$ squared percentage point. For a **new independent return**, the predictive variance is $\sigma^2+0.5=4.5$ squared percentage points; its standard deviation is about $2.12$ percentage points. The model's point estimate is therefore far more precise than an individual future return.

If the four outcomes overlap in time or share the same market shock, they are not four independent pieces of evidence. Their covariance must enter the likelihood, or the uncertainty will be understated. A fixed shrinkage constant can reproduce the shape of $w$, but it is not automatically this posterior calculation unless its variance assumptions match.

## Where OakQuant uses it

OakQuant has **implemented partial-pooling components**, though the current local code does not establish a fully deployed, market-qualified Bayesian forecasting hierarchy. The `oak-forecast` hierarchy blends a market, group, and symbol estimate with $w=n/(n+K)$; its default $K=50$ is explicitly marked as a placeholder. The `oak-domain-investments` outcome learner folds settled results into market, sector, and symbol statistics. Those estimates track the contributing path, but a count-based blend alone is not a full posterior predictive distribution.

Separately, Timber's `common/learning/hierarchical.py` implements a conjugate Gaussian return model with common slopes and nested sector, industry, and company intercepts. It propagates posterior covariance and observation noise into predictive variance. Its prior scales are fixed fit inputs, and its module explicitly disclaims calibrated tail coverage. These are local software capabilities, not evidence that a customer-facing forecast beats a baseline.

The [current implementation plan](../../../../ML_IMPLEMENTATION_PLAN.md#52-hierarchical-bayesian-forecasting-and-contextual-confidence) **proposes** learning shrinkage from training evidence, time-varying coefficients, cross-cutting themes, and contextual calibration. That is design intent. Market training and household records have separate purposes and permission boundaries; private customer data is not assumed to train a shared market model.

## Where it is used elsewhere

This is useful beyond markets. A [King County, Washington public-health study](https://www.cdc.gov/pcd/issues/2016/15_0536.htm) used a hierarchical Bayesian model to estimate smoking prevalence in small geographic areas from survey data. Small local samples were the problem; pooling information across areas helped stabilize estimates. The study documents that application, not a finance forecasting result. The [CDC's Rate Stabilizing Tools](https://www.cdc.gov/heart-disease-and-stroke-data/rst/) likewise use Bayesian spatial models for county and sub-county health estimates and report uncertainty.

## Alternatives and tradeoffs

| Approach | Useful when | What it gives up or can get wrong |
|---|---|---|
| One pooled model | Groups are similar or data are too thin to distinguish them | Hides real company differences; can systematically miss an outlier. |
| Separate company models | Each company has ample, representative history | Thin names get volatile estimates and intervals; shared evidence is wasted. |
| Hierarchical partial pooling | Related groups have unequal evidence | Wrong group assignments, misspecified noise, or a sudden business change can pull estimates the wrong way. Full posterior fitting can cost more computation. |
| Regularized fixed-effect regression | Many covariates need a fast, stable baseline | Coefficient penalties need tuning; group uncertainty and a joint predictive distribution require additional modeling. |

Whichever method is used, compare forecasts on the same later dates and score probabilities and intervals as well as point errors. A hierarchy should earn its complexity on thin groups in particular, without sacrificing the well-observed ones.

## People, libraries, and further reading

Andrew Gelman, Jennifer Hill, and Masanao Yajima explain how multilevel shrinkage changes comparisons among groups in their [2012 paper](https://stat.columbia.edu/~gelman/research/published/multiple2f.pdf). Their contribution is a clear account of a broad statistical practice, not an invention claim for Bayesian hierarchy. For a first worked example, use [PyMC's multilevel-modeling primer](https://www.pymc.io/projects/examples/en/2022.01.0/case_studies/multilevel_modeling.html), which walks through household radon measurements. The [Stan hierarchical-regression guide](https://mc-stan.org/docs/stan-users-guide/regression.html) is a deeper source for model structure and partial pooling.

[PyMC](https://www.pymc.io/projects/docs/en/stable/) and [Stan](https://mc-stan.org/) are public probabilistic-programming libraries that can fit such models; [CmdStanPy](https://mc-stan.org/cmdstanpy/) is a Python interface to Stan. They are learning and implementation options, not claims about OakQuant dependencies. Start by reproducing the small normal example above, then fit a grouped example and check posterior predictions against held-out groups and dates.
