## The forecaster who is almost always right

Arica sits on the coast of northern Chile, at the edge of the Atacama. It is the driest inhabited place on earth. Some years it does not rain at all. Not a shower, not a drizzle. Nothing.

Give a forecaster in Arica one job. Every morning, say whether it will rain.

She says no. She says no the next day, and the day after that. She says no for a year, and at the end of the year she has been right almost every single time. Her accuracy is extraordinary. You could put it on a poster.

She has told you nothing. A rock would have scored the same. The rock does not need a salary or a satellite feed. And the rock will keep saying no on the one afternoon the clouds finally break. That is the only day anybody needed her.

This is the oldest trap in prediction, and the stock market has its own version of Arica.

## What you are doing when you predict

Step back for a moment, because everything that follows depends on this part.

You want to know something. Your bus is due at 7:42 and you have to be at work by eight. The thing you actually want is a fact: will the bus be late today. That fact exists. It has not happened yet, so you cannot have it.

When you cannot have the fact, you predict. And the moment you predict, you have accepted uncertainty. There is no way around that and no shame in it. A prediction is a statement about something you do not know.

So the useful question stops being "will the bus be late" and becomes a better one: **how sure am I, and what would make me surer?**

Most people already have good instincts here. If you have taken that bus twice, you know nothing. If you have taken it three hundred times, you know a great deal. You know it runs late when it rains. You know the usual spread between the timetable and the truth. You have not removed the uncertainty. You have narrowed it.

That is the whole game. **Historical context does not turn a prediction into a fact. It narrows the range of answers the truth could sit in.** A forecast that says "70% chance of rain" is a better object than one that says "it will rain," because the first tells you how much to trust it and the second does not.

![A prediction interval narrowing as independent observations accumulate. The estimate is held at 52% in all three. At six observations the interval runs +/-40 points, wide enough to contain almost any conclusion you might want to draw. At sixty it is +/-12.7. At six hundred it is +/-4.0 and still crosses the 50% line, which is the honest shape of this problem: six hundred observations do not settle a two-point edge. The point estimate never moves. Only the range does, and the range is what decides whether you have learned anything.](figures/uncertainty-narrows.svg)

Everything in this article is one of two questions:

1. **How much does the history I have actually narrow the range?** Usually far less than the row count suggests.
2. **When the range is still wide, does the system say so?** Usually not.

The rest is detail.

## Equities drift up

Run this test. Take twenty years of daily prices for large US companies. Our backfill holds 2,402,296 daily bars, from January 2006 to September 2026. Step through it one forecast horizon at a time, so that no two bets share a day. That gives 473,405 independent bets. On every one of them, predict up. Never look at anything. Never change your mind.

That constant scores **54.569%** correct, with a 95% interval of [54.427, 54.711] — the band the true rate plausibly falls in.

It scores that because equities drift up. Over long stretches, more days close green than red. The drift is small on any one day and enormous over twenty years, and it is baked into the direction of the market itself. It also gets stronger the further ahead you look: 54.6% over five days, 57.8% over twenty-one, 62.8% over sixty-three. The longer your horizon, the more of your accuracy the rock is handing you for free.

Now hold that number next to a machine learning model that reads earnings, price history, sector rotation and macro data, and reports 52% directional accuracy.

That model has lost. It has lost to a rock. It burned a research quarter and a compute budget to be two and a half points worse than a piece of code that returns `True`.

Nobody publishes it that way, because 52% is above 50%, and 50% feels like the number to beat. Fifty percent is a coin flip, and beating a coin flip sounds like an achievement. Beating a coin flip is free. The market hands it to you before you write a line of code.

## The baseline board

The fix is not complicated. It is just work that most people skip.

Every result has to be reported against a board of baselines. Every baseline has to be measured on exactly the same bets: same days, same companies, same horizon, same costs. Ours has five.

| Baseline | What it does | Why it is on the board |
|---|---|---|
| Random sign | Flips a fair coin each bet | The floor. If you cannot beat this, stop. |
| Always up | Predicts up, every time | Captures the equity drift for free. |
| Buy and hold | Holds the index over the window | The thing a reader could actually do instead. |
| 12-1 momentum | Ranks on the last twelve months, skipping the most recent one | The best-documented anomaly in the literature. |
| Persistence | Predicts that tomorrow repeats today | Catches short-horizon autocorrelation. |

A single coin flip is not a baseline either. The first time we ran the random-sign baseline, one seeded draw scored 666 out of 1,392 — bit-identical to the deployed engine, purely by chance. One draw is an anecdote. The baseline now averages sixty-four of them.

The 12-1 momentum rule comes from Jegadeesh and Titman's 1993 paper, which found that stocks that did well over the previous year kept doing well over the following months. The one-month skip is there because the most recent month tends to reverse. Thirty years later it remains the baseline any new signal has to clear, and it is not a strawman. It is genuinely hard to beat.

A board is only as good as the baselines that can actually run. On our live corpus it scores four of five, because 12-1 momentum needs about 365 days of closes per symbol and that corpus holds eighty. Momentum is implemented and tested against synthetic price series, and it turns on where the twenty-year backfill reaches. Until then nothing can claim to beat every baseline, which is the correct answer rather than an inconvenience. A board of five that scored four is not a board of four.

Same bets is the part that gets fudged. A model evaluated on 2015 to 2025, compared against a baseline someone computed on 1990 to 2020, is not a comparison. It is two unrelated numbers printed next to each other.

What matters is **lift**: how much better than the best baseline, on the identical set of bets. A model at 55.1% against an always-up baseline at 54.6% has produced half a point of lift. That is the whole result. The 55.1% is decoration.

## Five photographs of the same afternoon

Here is where most systems, including honest ones, quietly deceive themselves.

Suppose you forecast five days ahead. You make that forecast every day. Monday's forecast covers Monday through Friday. Tuesday's covers Tuesday through the following Monday.

Those two forecasts share four of their five days. They overlap by 80%. Wednesday's shares three days with Monday's. By the time you have made 1,400 of these forecasts, you have something that looks like 1,400 independent tests and is nothing of the sort.

![Five five-day forecast windows placed on consecutive days, stacked so the overlap is visible. Monday's window covers Monday to Friday. Tuesday's covers Tuesday to the following Monday. Each window shares four of its five days with the one above it, and the shared span is shaded. Counting these as five independent tests is the error: taken together they carry closer to one bet's worth of independent evidence than five.](figures/overlapping-windows.svg)

Think of it as photographs. You stand in a field and take a photograph every ten seconds for an hour. You have 360 photographs. You do not have 360 afternoons. You have one afternoon, photographed 360 times. If it was raining that afternoon, every photograph agrees with every other one. None of that agreement tells you anything about the weather in general.

Statisticians call the honest count the **effective sample size**, written $n_{\text{eff}}$. For overlapping windows there is a standard correction. You compute a variance inflation factor from the overlap:

$$\text{VIF} = 1 + 2\sum_{k=1}^{h-1}\left(1 - \frac{k}{h}\right)\rho_k$$

where $h$ is the horizon in days and $\rho_k$ is the correlation between bets $k$ days apart. Then $n_{\text{eff}} = n / \text{VIF}$.

For a five-day horizon this factor lands somewhere between three and four. In our harness, roughly 1,400 overlapping five-day bets carry about 372 bets' worth of independent evidence. Roughly one in four.

Watch what that does to a result.

Our own engine scored 666 correct out of 1,392 bets with a defined direction. That is 47.84%, which sits 2.16 points away from a coin flip. Against a naive count of 1,392, it reads as 1.6 standard deviations from chance. That is close enough to significance that a person gets excited and starts writing the summary paragraph.

Recompute it with $n_{\text{eff}} = 372$ and the same 47.84% is 0.83 standard deviations from chance. It is noise. Nothing happened.

The number did not change. The denominator did, and the denominator was the whole story.

This is the bus again. Three hundred rides narrow the range. Three hundred photographs of one ride do not.

The denominator hides a second lesson. We originally scored 1,396 bets. Four of them had a return of exactly zero, which means no direction is defined — the stock finished where it started. The old denominator counted those four as misses. Correcting it moved the rate from 47.71% to 47.84%. That changes nothing about the conclusion and tells you everything about how carefully a denominator has to be built.

There was a worse one hiding in the same place. Our five-day labels were tagged `5d`, and that tag never said whether it meant five trading days or five calendar days. The overlap kernel read it as calendar days. The labels were trading days. A tag claiming a longer settle than the labels actually have inflates $n_{\text{eff}}$ by about 40%, which is to say it hands you 40% more evidence than exists, and nothing downstream would ever have flagged it. **A unit that lives only in a string is a unit nobody is checking.** That one is now fixed at source.

> **Sidebar: what a rate must carry**
>
> A percentage on its own is not a finding. Every rate we report carries three things or it does not get reported:
>
> 1. **The denominator**, and the effective denominator if the bets overlap.
> 2. **An interval**, so the reader can see the range the truth plausibly sits in.
> 3. **The baseline it is being compared against**, measured on the same bets.
>
> "We beat the market" is a claim about an interval. Until the interval excludes the baseline, the honest sentence is "we cannot tell yet."

This is the same instinct you already have about other people's numbers. A friend tells you a restaurant is excellent. You ask how many times he has been. He says once. You do not conclude the restaurant is bad. You conclude that he does not know yet, and neither do you.

A baseball player goes two for six in his first week. He is batting .333, which would make him one of the best hitters alive. Nobody in baseball believes it, because everyone in baseball has internalized what six at-bats is worth. Finance has not built the same reflex, and finance deals in numbers that make people rich or broke.

## Why you cannot simply wait for more evidence

The obvious response is patience. Run the system longer. Collect more bets. Let the evidence accumulate.

Work out how long that takes.

Suppose the real edge is 52% against a 50% baseline. To detect a gap that small you need roughly 4,900 **independent** bets. Two standard conditions come attached to that figure. **80% power** means an 80% chance of spotting the edge if it is genuinely there. A **5% significance level** means a one-in-twenty risk of being fooled by luck when it is not.

One stock gives you about fifty independent five-day bets in a year, because a year holds about 252 trading days and non-overlapping five-day windows fit into it about fifty times. So 4,900 independent bets is about 97 stock-years.

You might think this is easy. Trade 500 stocks and you are done in ten weeks.

You are not, because the 500 names move together. When the market falls, almost everything falls. Correlated bets are the overlapping-window problem again, wearing a different hat, and they collapse in the same way. The independent evidence in a day of 500 correlated names is much closer to one bet than to 500.

So accumulating evidence forward, in real time, takes decades. A research program that waits for it will learn the answer long after everyone who asked the question has moved on.

There is a sharper version of this, and we walked straight into it.

Our system reads the news each day and produces one board — the themes firing right now. Every symbol forecast that day is scored against that same board. With 36 symbols a single day's board generates dozens of attributions. Widen the universe and it generates hundreds.

Those are not hundreds of observations about a theme. They are **one observation wearing three hundred hats.**

![One news board, attributed across every symbol forecast that day. The board fans out to hundreds of symbols and each produces its own attribution row, but all of them came from the same eight themes on the same morning. Counted as hundreds, n climbs, the shrinkage weight climbs with it, and the theme sounds certain on the strength of a single day. The fix is to count distinct days: one observation per theme, per sector, per day. It is the overlapping-windows problem again, one level up — across names instead of across time.](figures/one-board-many-hats.svg)

Counting them as independent does something worse than waste them. It makes the model confident. Feed 300 near-duplicate rows into a shrinkage weight and $n$ climbs, $w$ climbs with it, and the theme concludes it has earned the right to speak for itself. The fix is to count distinct days rather than rows: one observation per theme, per sector, per day, before anything is folded in. Without that, every interval the theme layer reports is wrong in the confident direction.

This inverted an assumption I had held without examining it. I expected the news layer to be starved beside the price layer, and to need protecting from being drowned out. The rates say otherwise. About 108 outcomes a day across 36 symbols is roughly 3 per symbol, while those same outcomes reach 8 themes at 12 to 37 each. Themes firm up faster than symbols do. The urgent guard is a **ceiling**, not a floor.

## The witness who has read tomorrow's newspaper

If evidence cannot be gathered forward fast enough, it has to be reconstructed backward. Replay the system across decades of history and see what it would have said.

This is where almost every backtest goes wrong, and the failure has a specific shape.

Put a witness on the stand. Ask what she saw on the night of the fire. She gives you a clear, confident account: which door was open, who left first, which way the wind was blowing. Then you discover she read three weeks of newspaper coverage before testifying.

Her account may still be true. It is no longer evidence, because you can no longer separate what she saw from what she later learned. The contamination is not in her conclusion. It is in her access.

A feature builder replaying history has exactly this problem. If the code that computes a company's earnings surprise for March 2009 reaches into a database that holds the restated figures published in 2011, the model is testifying with the newspaper in its lap. It will look brilliant. It has learned nothing.

The rule we enforce is stronger than a convention, and I want to be precise about why. A convention is something a careful engineer remembers. This has to be something a careless one physically cannot violate.

Every feature builder runs against a data interface constructed with an **as-of timestamp**. The interface will not return a row stamped after that moment. Not filtered out downstream. Not dropped in a later step. Never handed over. A builder asking for data it should not have gets an empty result, and an empty result is loud.

That property has a name in the trade: **point-in-time integrity**. Think of it as a signature on the output. The signature says: this number was produced by code that was structurally incapable of seeing the future. A backtest without that signature is a story about the past, told by someone who already knows the ending.

There is a second copy of the rule, because there are two code paths. The guarded builder is correct and slow. A fast path exists because replaying millions of bars through the slow one is not practical. So the fast path is pinned to the guarded one by two tests. The first requires the two to agree to twelve decimal places. The second corrupts every bar dated after the as-of moment and requires the output not to move at all. Speed is the most common reason a leak gets into a backtest. It is not going to be the reason here.

The formal version is purged and embargoed walk-forward cross-validation, set out in Marcos Lopez de Prado's work on financial machine learning. Purging removes training samples whose outcome windows overlap the test period. The embargo adds a gap after the test period, because a five-day label leaks five days forward. Ordinary k-fold cross-validation, applied to overlapping financial labels, will hand you a beautiful score and no information whatsoever.

## The bias that runs backward

Everyone in finance has heard of survivorship bias. Most people have learned it as a rule of thumb, and the rule of thumb is wrong half the time.

The familiar version goes like this. Build a universe from the companies in the index today. You have just excluded every company that went bankrupt, got delisted, or was absorbed on bad terms. Your history contains only survivors. Your backtest looks better than reality. Survivorship flatters.

That is true for a long-only strategy. It is exactly backward for a long-short one. Here is why.

A long-short strategy buys some companies and sells others short. Short selling profits when a stock falls. A company that goes to zero is the single best outcome available to whoever is short it. It is a 100% return on that leg.

Put it somewhere you can see it. You bet against horses, so you make money when a horse fails to finish. Now someone hands you the record of every race run last season, with all the horses that did not finish quietly removed. Your losing bets are all still there. Your winners have been deleted from history.

Now delete every company that went to zero.

You have not removed losses from the short book. You have removed its **wins**. The most profitable trades the short leg would ever have made are the ones your dataset has quietly erased. A survivors-only universe does not flatter a long-short result. It guts it.

Here is the worked example, and it is the one that made this concrete for me.

Take an S&P universe built only from companies that survived to today. Sort them into five buckets by momentum. Look at the worst-momentum bucket, the beaten-down names, in **March 2009** — the month the market bottomed after the financial crisis.

That bucket returned **+42.95%** in a single month. A thousand dollars spread across those names at the start of March 2009 was worth about $1,429 by the end of it. One month, one bucket, one number — a denominator of exactly 1.

At first glance this looks like a stunning finding about buying losers. It is not a finding at all. It is an artifact of the universe.

In March 2009 the beaten-down bucket was full of companies the market had priced for death. Some of them died. Those companies are not in a survivors-only universe. The only beaten-down names left in the sample are, by construction, the ones that recovered — and in March 2009 they recovered violently.

The dataset answered a question nobody asked: *how did the losers who were going to survive perform?* You cannot know that in advance. That is the whole difficulty of investing, removed from the data by a quiet act of selection.

The rule I take from this: **a bias has a direction, and the direction is a property of the estimator, not the dataset.** The same missing companies flatter a long-only backtest and destroy a long-short one. Before trusting any correction for bias, derive its sign for the specific thing you are measuring. Inheriting the sign from a rule of thumb is how a corpus artifact gets written up as a discovery.

I have made this mistake. It is not hypothetical.

### The same rule, applied to old news

I had to derive a sign a second time, for a different bias, and that time the answer made a whole class of data usable instead of unusable.

We would like to learn from old news. Old news is curated. Stories get retracted, de-indexed, put behind paywalls, quietly edited. Links rot. Re-query a window from 2009 today and what comes back is a version of the past that has been tidied up by what happened afterward.

The reflex is to throw it out. Derive the sign instead.

Curation inflates **magnitude** far more than it flips **sign**. A retracted story rarely reverses what a theme implied at the time. It removes the noise that stood around it. So the direction estimate survives the bias and the effect size does not.

That asymmetry is worth having, because direction is most of what we want from a theme in the first place.

### Letting weak evidence be quiet instead of silent

So biased history is admissible, provided it arrives with a discount attached rather than a vote.

Let every observation carry a **credibility** $c$ between 0 and 1, and fold it in as $c$ observations rather than as one. The node statistics are already sums, so this changes nothing structural. An observation with $c = 0.35$ moves the estimate the right way, a third as fast, and can never outvote a live one.

$$c = c_{\text{source}} \times c_{\text{age}} \times c_{\text{competition}}$$

| Term | Form | What it is discounting |
|---|---|---|
| $c_{\text{source}}$ | live 1.0, replayed about 0.35 | roughly three replayed observations count as one live one |
| $c_{\text{age}}$ | $\text{floor} + (1-\text{floor}) \cdot 0.5^{\,\text{age}/H}$, floor 0.25, $H \approx 180$ days | curation accumulates with age — de-indexing, paywalls, link rot, editorial cleanup |
| $c_{\text{competition}}$ | $1/(1 + \lambda(k-1))$, $\lambda \approx 0.1$, $k$ themes firing | on a crowded board, which theme owned the move is less certain |

![How credibility decays and where it stops. The curve is the age term, 0.25 plus 0.75 times one-half raised to the age in 180-day units. It falls from 1.0 for a fresh observation toward a floor of 0.25, which it never goes below. Beneath it are the other two terms. The source term is 1.0 for a live observation and about 0.35 for a replayed one, so roughly three replayed observations count as one live one. The competition term falls from 1.0 when a single theme is firing to about 0.59 when eight are. The three multiply together, and because every one of them floors above zero, no observation is ever worth nothing.](figures/credibility-weights.svg)

**The floors matter as much as the decay.** Every term bottoms out above zero, so an old observation on a crowded board comes out quiet and never silent. Nothing is drowned out. It is simply outweighed by better evidence, in proportion to how much better that evidence is.

One trap is written into the plan in capital letters, because it is the kind of thing a tidy mind does by accident. $c_{\text{age}}$ is a **data-quality** discount: old records have been curated more. Regime-recency weighting is a **different** idea: old market conditions are less like today's. Both point the same direction, which is exactly why they will get merged into one number — and then neither can be tuned, because a single knob cannot answer two questions. Two parameters, two names, two hypotheses.

## One observation can own a mean

The same survivors-only test produced a second lesson, and this one is arithmetic that any reader can check.

First, what the test measures. A **winners-minus-losers premium** works like this. Buy the best-momentum stocks. Sell the worst ones short. Hold for a month, then record what the pair returned together. Do that every month for twenty years and you have roughly 240 monthly numbers.

The average of those 240 months was **-0.075%**. On every thousand dollars committed, that is a loss of about seventy-five cents a month. Slightly negative. Momentum does not work, apparently.

Three other numbers from the same 240 months:

- The **median** month was **+0.419%** — a gain of about four dollars twenty on the same thousand, pointing the opposite way.
- **55%** of the months were positive.
- The **trimmed mean**, with the extreme months removed, sat on the positive side with the median.

A mean of -0.075% and a median of +0.419% cannot both describe a typical month. The gap between them is about half a percentage point per month. Multiply half a point by 240 months and you get roughly 118 percentage points of return that has to be accounted for somewhere. That is not a rounding difference — it is more than the whole strategy. And that much return does not arrive quietly, spread evenly across two decades. It arrives in a few enormous months.

Those months are momentum crashes. Kent Daniel and Tobias Moskowitz have documented them. Momentum works for long stretches, then reverses with terrible violence. It usually happens in a panic recovery, when the beaten-down names snap back all at once. March 2009 is the textbook case, which is why the same month shows up in both of these lessons.

So the reported mean is not a summary of what momentum did over twenty years. It is a summary of what momentum did in a handful of months, with 235 other months along for the ride.

The practical rule is small and easy to apply. **When a mean disagrees with its median, the mean is being carried by something. Go and find it.** Report the median, the trimmed mean and the fraction of periods that were positive alongside every average, and the reader can see the disagreement for themselves. A single average, alone on a slide, hides exactly the thing they most need to know.

## The part of the past that cannot be rebuilt

Reconstructing history backward works beautifully for prices. Prices are recorded, adjusted, and available for decades. Ours go back to January 2006.

It does not work at all for what people were paying attention to.

Our system reads the news and sorts it into themes. Not keywords and not clusters — a stable, versioned taxonomy, where `central_bank_policy` belongs to the family `macro` and means the same thing this week that it meant last week. A recent board carried eight themes drawn from 342 articles across a three-day window.

Now look at what history we actually hold for each dimension:

![The two dimensions the system predicts from, drawn to the same time scale. The price record runs from January 2006 to September 2026 — 2,402,296 daily bars, twenty years wide. The macro theme record is sixteen snapshots covering four days, a mark so narrow it is barely visible against the other. Prices can be replayed backward through history. Themes cannot, because nobody was recording them.](figures/evidence-asymmetry.svg)

| Dimension | Rows | Span |
|---|---|---|
| Daily price bars | 2,402,296 | Jan 2006 – Sep 2026, twenty years |
| News clusters | 57 | Jul 2026 – Sep 2026, two months |
| Macro theme snapshots | 16 | four days |
| Settled theme outcomes | 0 | none yet |

Twenty years of prices. Four days of themes. Zero settled observations connecting a theme to what happened next.

You cannot fix this by being clever. There is no honest way to reconstruct what the world was paying attention to in March 2009 from data collected in 2026. And the tempting shortcut — infer the themes from the price moves they are supposed to predict — is a lookahead wearing a feature's clothes. It would produce a model that appears to read the news and is in fact reading the answer.

There is a second trap in the same neighborhood, and we walked into it. An earlier version of the system generated features named `news_clust_<hash>`, one per detected cluster. Across the corpus, 104 distinct names appeared. Only 26 of them ever appeared on a second day. Seventy-eight fired once and never again.

Those are row identifiers wearing feature names. A model cannot associate a feature with an outcome if the feature never occurs twice. It was not a weak signal. It was a signal that was structurally incapable of being learned, sitting in the feature set looking like ninety-nine dimensions of information.

The themes do not have that problem, because the taxonomy is stable by design. That is why the memory is built on themes and the clusters were left alone.

## What a stock inherits

Everything so far has been about measurement. Here is the architecture, and it exists because of the measurement problem rather than in spite of it.

You want to estimate how a particular stock behaves. Some stocks have long, rich histories in your system. Others have six observations. Some themes have fired two thousand times. Most have fired never.

There are two obvious things to do and both are wrong.

**Trust each node's own data completely.** A stock with six observations gets an estimate built from six observations. This is the rookie batting .333 after six at-bats. You will chase noise forever, and every new name will arrive with a wild estimate attached.

**Ignore the individual entirely and use the group average.** Every stock in a sector gets the sector's number. Now a company with 600 observations, which genuinely does behave differently from its peers, is told it is average. You have thrown away the information you actually paid for.

The correct answer sits between them and slides smoothly along that line depending on how much you know. It is called **partial pooling**, or empirical Bayes shrinkage, and the classic demonstration is Efron and Morris in the 1970s, predicting baseball batting averages. Shrinking each player's early-season average toward the league mean beat using their own average, for almost every player.

The estimate is built as a sum:

![The estimator as it is being rebuilt. A price spine runs market drift to sector to symbol and produces the base estimate. Alongside it a theme hierarchy runs family to theme to theme-and-sector and produces a theme adjustment, with each theme credited only its apportioned share of the residual. The two are added rather than chosen between: final equals the base estimate plus a macro weight times the theme adjustment. Every arrow is the same shrinkage operation, w = n/(n+K). Along the bottom, the macro term's share of the answer is bounded — a hard floor at 10% so it can never be argued to zero, a ceiling at 35% so one loud news day cannot run away with a forecast, and a 15 to 25% target band which is a prior borrowed from published variance decompositions rather than anything we have measured.](figures/hierarchy.svg)

```
estimate(symbol) = market drift                     <- the floor
                 + sector deviation                 shrunk toward market
                 + symbol deviation                 shrunk toward sector
                 + Σ themes firing now              shrunk theme×sector
                                                    -> theme -> family
```

Two hierarchies, sharing one kernel. The **price spine** runs market to sector to symbol. The **theme hierarchy** runs a theme's effect in a specific sector, toward that theme's average effect everywhere, toward its family. A theme that has only ever fired in energy still has something to say about industrials, weighted by how much the wider family has been seen.

Every arrow in that picture is the same operation, governed by one number:

$$w = \frac{n}{n + K}$$

where $n$ is how many observations that node has and $K$ says how much evidence is needed before a node earns the right to speak for itself. Each estimate is then

$$\hat{\mu}_{\text{node}} = w \cdot \bar{x}_{\text{node}} + (1 - w) \cdot \hat{\mu}_{\text{parent}}$$

Here $n$ counts the settled bets that node has actually been graded on. Look at what $w$ does with $K = 50$:

| Observations $n$ | $w = n/(n+K)$ | Effect |
|---|---|---|
| 6 | 0.11 | The node is 89% its parent. It inherits. |
| 50 | 0.50 | Half its own, half its parent's. |
| 200 | 0.80 | Mostly its own. |
| 600 | 0.92 | It speaks for itself. |

![The shrinkage weight w = n/(n+K) plotted against the observation count n, for several values of K. Each curve rises from zero, where a node is entirely its parent, toward one, where it speaks entirely for itself. There is no step and no threshold anywhere on it. A larger K slides the whole curve to the right, demanding more evidence before a node is trusted. The two failure modes are the two ends of the family: K at zero collapses the curve into a vertical line that trusts a single observation completely, and K at infinity flattens it onto the floor, where nothing is ever allowed to differ from its parent.](figures/shrinkage-curve.svg)

There is no threshold, no rule that says a stock becomes trustworthy at its hundredth observation. Confidence arrives continuously, in proportion to evidence. A new listing starts out as its sector and earns its way to its own identity.

The two failure modes are the two limits of the same formula. As $K \to 0$, $w \to 1$ and every node trusts its own noise. As $K \to \infty$, $w \to 0$ and every node is identical to its parent. Both look like corners to avoid, and taste is a bad way to choose between them. So $K$ is tuned against outcomes on a purged walk-forward split rather than picked. Hold on to that sentence — the measurement had something to say about it that I did not expect.

One implementation detail earns its place here, because it is what makes the dial cheap to turn. A node stores only its sufficient statistics: the count, the sum, the number of up moves. It never stores a shrunken value. Shrinkage happens when an estimate is requested, not when data is folded in. So one pass over 2.4 million bars can be graded at any value of $K$ without refitting. Folding in a new observation is a constant-time update rather than a rebuild. And $K$ can be retuned without a database migration. **A parameter you cannot afford to sweep is a parameter you will end up choosing by taste.**

### The root is the baseline

Make the root of the hierarchy the measured market drift — the always-up baseline itself.

That one choice changes what the baseline is. Earlier in this piece, always-up at 54.569% was the model's competitor, the thing it had to beat. Put that same drift at the root and it becomes the model's **floor**.

A stock the system has never seen has $n = 0$, so $w = 0$, so its estimate is its sector's, which is shrunk toward the market's. With no information at all, the model predicts the market drift. It starts where the rock starts. Every observation it gathers can only move it away from that floor in a direction the data supports.

A system that cannot underperform the baseline through ignorance is a different kind of object from one that competes with the baseline and might lose. The baseline stopped being the opponent and became the ground.

### And then the wiring defeated it

That is the design. Here is what the code was doing.

```
per_tier = predict_cascade(models, features)
if not per_tier:
    return hierarchy_prior(...)    # the only place the theme layer is consulted
return cascade_answer              # macro never enters
```

The prior was consulted **only when nothing else fitted**. The moment any tier of the cascade produced an answer, the hierarchy — and the whole theme layer with it — stopped being consulted at all. Not phased down to something small. Switched off, at zero, abruptly.

I had built the prior as an *alternative* to the model, when the entire argument for it is that it should be a *component* of the model. The floor described above was real in the arithmetic and unreachable in the wiring. A reader who accepted my account of it would have believed something the running system did not do.

The correction is to make macro additive rather than conditional:

$$\text{final} = \text{base estimate} + w_{\text{macro}} \times \text{theme adjustment}$$

and then to bound what that term is allowed to be. Its share of the answer is measurable directly, as $|macro| / (|base| + |macro|)$, so it can be constrained rather than hoped for.

| Bound | Value | Why |
|---|---|---|
| Floor | 10% | macro is always a factor and may never be argued to zero |
| Target band | 15–25% | conservative against the reference below |
| Ceiling | 35% | stops one loud news day running away with a forecast |

**The 15–25% band is a prior, not a finding, and it has to be labeled that way every time it is quoted.** We have no data that justifies it. The defensible anchor is separate and weaker than a measurement: for large-cap equities, market and sector factors account for roughly 30 to 50% of a single stock's short-horizon return variance. Holding macro to 15–25% of predictive contribution is therefore conservative rather than generous. That is a reference point borrowed from other people's work. It is not our result, and we do not have one yet.

$w_{\text{macro}}$ is tuned, on a pre-registered grid, with the floor as a hard constraint the optimizer is not allowed to argue away. The trials ledger then deflates the winner for multiple testing, because the best of eight weights always looks better than it is.

### The dial had no best setting

Here is the result I did not expect, and it is the most useful thing the price history has told us.

We graded the price spine on those same 473,405 non-overlapping bets. Because the windows never touch, $n_{\text{eff}}$ equals $n$, and the intervals come out at about ±0.14 percentage points of hit rate. It is the most evidence this program has ever had about any candidate, and it was enough to give a decisive answer rather than a shrug.

Then we swept $K$ across four orders of magnitude and measured lift on identical bets.

Before the numbers, what they are. **Lift is the model's hit rate minus the baseline's hit rate**, counted in percentage points of direction called correctly. Always-up scored 54.569% on these bets. A lift of zero means the model tied the rock. A positive lift would be the first real result this program has produced. A negative lift means the model called direction correctly *less often* than a constant did.

| $K$ | Model's hit rate | Lift over always-up | Extra wrong calls per 1,000 |
|---|---|---|---|
| 0 | 54.094% | −0.475 points | 4.8 |
| 50 | 54.199% | −0.370 | 3.7 |
| 1,000 | 54.343% | −0.226 | 2.3 |
| 50,000 | 54.411% | −0.158 | 1.6 |

Every value is negative. Read that plainly: **at every setting we tried, you would have done better by ignoring the model.** Across all 473,405 bets, the worst setting got about 2,250 more calls wrong than the rock did. The best setting still got about 750 more wrong.

Now watch the direction of travel, because that is the part that matters. The number climbs: −0.475, then −0.370, then −0.226, then −0.158. It is heading toward zero. But zero *is* always-up. Every step of that improvement is the model doing less of its own thinking and copying its parent more. The destination is the model contributing nothing at all.

Put it in terms of somebody's money. Imagine paying a manager to call which way each stock moves next week. Compare them against a rule you could follow for free: assume every stock goes up. The dial controls how much the manager may back their own opinion instead of copying the free rule. We turned it from "all opinion" to "almost none." They lost at every setting. They improved only in the sense that they were doing less, and the best version of this manager is the one who stops making calls.

There is no peak anywhere in the middle. More pooling is monotonically better, and the limit of "better" is *exactly* always-up.

One honesty note, because direction is not money. A hit rate counts how often a call was right. It says nothing about how much was riding on each one. You can be right on many small moves, wrong on a few enormous ones, and still lose badly. That is precisely what the momentum-crash months did earlier in this article. So none of these figures is a claim about returns. They are a claim about whether the model knows which way a stock is heading, and the answer is that it knows slightly less than nothing.

The reason runs straight back to the survivorship section. On a universe that drifts up, the sign of a stock's historical mean is almost always positive. An estimator built out of level means therefore re-derives the very baseline it is shrinking toward. Its only lever for adding anything of its own is calling "down" on the few names whose drift is negative. On a survivor universe, those are precisely the names that fell hard and then came back. The one move available to it is the move the dataset has been rigged against.

So the intercept hierarchy is a **prior, not a predictor**. That is not a failure of the design. It is the design being measured properly and found to be the right shape for one job and the wrong shape for another. As the cold start it is exactly right, and that is how it ships — the thing an unseen symbol inherits before it has earned an opinion. As a source of edge on price history alone, it has nothing, and the sweep says so with intervals of ±0.14 points instead of a shrug.

A dial with no interior optimum is a real answer. It is also the kind of answer you only get if you sweep the dial instead of picking the value you like the look of.

### What a theme is credited with

The theme layer has one more property, and it is the piece I would most like a critical reader to attack.

When a bet settles, the outcome is attributed back to the themes that were firing **at the moment the bet was placed**, not the themes firing now. And the theme is credited only with the residual:

$$r = \text{actual} - \text{spine expected}$$

The price spine has already made its prediction. If a stock rose and the spine expected it to rise, the themes get nothing for that. They are credited only with the part the price history did not already explain.

A tutor is hired for a student who has been scoring 80 all year. The student scores 82. The tutor gets credit for two points, not eighty-two. Crediting the whole score would make every tutor in the country look extraordinary, and would tell you nothing about which ones help.

Without this, the theme layer would slowly re-take credit for the equity drift. Every theme in the taxonomy would drift toward looking mildly bullish, because most things do. Attributing the residual keeps the layers from double-counting the same effect.

There was a second double-count underneath that one, and it survived until two halves of the system were made to state their answer in the same units.

Learning credited **every** firing theme with the **whole** residual. Prediction **summed** weight times effect across all of them. With three themes firing, learning taught each one the entire move, and prediction then added three entire moves together.

The error scales with how busy the news is. On a quiet day it is nearly invisible. On the day it matters most — a crowded board, everything firing at once — it is at its worst.

The fix is apportionment. Credit theme $i$ with

$$r_i = r \cdot \frac{w_i}{\sum_j w_j}$$

so the credits reconstruct exactly one residual between them. Learning and prediction then agree by construction rather than by luck.

Nobody found this by reading the code. It surfaced from a question — what happens when two narratives compete for the same move? — taken literally instead of answered in the abstract.

### Why this survives having almost no data

Return to the table above: four days of themes, zero settled outcomes.

Under this structure, that is safe. With zero observations a theme has $n = 0$, so $w = 0$, so its contribution is exactly `0.0` and the estimate is the price spine unchanged. Not through a special case or a feature flag. Through arithmetic.

It is how you already treat a new colleague. On their first day you have no information about them, so you assume they are roughly like the team they joined. You do not invent a rating for them and you do not refuse to work with them. As they do things, your picture of them separates from the team's, in proportion to what you have actually seen.

Say a theme has fired three times, and across those three the sector moved 10 percentage points more than the price spine expected. The theme does not get to claim 10 points. It contributes under 3 of them, and the other 7 stay with its parent until more evidence arrives. A theme seen two thousand times earns close to the whole effect. Everything in between is a smooth ramp.

That is the strongest argument for partial pooling here, and it is worth stating plainly: **you do not need twenty years of news to start. You need a structure that makes four days safe and four hundred days valuable, with nothing rewritten in between.**

## Knowing when to say nothing

There is a third property, and it is the one most prediction systems are built without.

Ask a good doctor to read a scan. Sometimes the answer is a diagnosis. Sometimes the answer is that the scan is inconclusive and you need a different test. The second answer is not a failure of the doctor. A doctor who produced a confident diagnosis from every scan, including the unreadable ones, would be more dangerous than one who says nothing.

Most predictors cannot say nothing. They are built to emit a number for every symbol on every day, because that is what a dashboard expects and what an API contract demands. So on the days when the model has no idea, it produces a number that looks exactly like the number it produces on the days it does.

A model that can decline is a strictly better object, and it is better in a way that shows up in measurement. If the model abstains on the bets it is least sure about, the hit rate on the bets it *does* take should rise. If it does not rise, the confidence score is decorative and you have learned that too.

The same idea has a smaller version one layer down, and it is open work. A theme that has fired fewer times than $K$ currently contributes a heavily shrunken guess. It could instead contribute nothing at all, and say so. That difference matters most in exactly the situation we are in today, when almost every theme sits below that line.

This is the same idea as the rest of the article, moved from the instrument into the model. The instrument has to be able to report "we cannot tell." So does the thing being measured.

## The discipline

The techniques matter less than the habits, and the habits are three.

**Pre-register the verdict before running the test.** Write down, in advance, what result would count as success and what result would count as failure. Commit it to the repository with a timestamp. The gate for the phase we are in reads, in full:

> *With the news dimension carrying real settled evidence, the full estimate beats always-up on identical bets, with an overlap-aware interval that excludes it.*

That was written down while the theme memory held zero observations, which is the only time it could have been written honestly.

This sounds bureaucratic until you have watched yourself — I mean yourself, not a colleague — look at an ambiguous result and find a reason it was encouraging. Once a number exists, the mind starts negotiating with it. Deciding the threshold beforehand is the only defense, and it costs ten minutes.

**A null result is a deliverable.** Our own evaluation gate has returned "inconclusive" and that outcome was recorded, versioned and kept, in the same form a positive result would have taken. Inconclusive is information. It says the instrument works and the evidence is not there yet. A research program that can only produce good news is not a research program. It is a marketing function with a compiler.

**No naked percentages, ever.** Denominator, effective denominator, interval, baseline. A number that arrives without them does not enter the record, and that rule applies to numbers that flatter us most of all.

These three are what separate a system that compounds knowledge from one that generates years of activity and learns nothing. The second kind is common. It ships dashboards, it holds reviews, it produces charts that trend upward, and after four years nobody can state a single thing it established.

## Where this actually stands

Here is the honest position, stated plainly.

We have not demonstrated an edge in predicting stock direction. What we have built is an instrument that can tell us when we do not know, and an estimator whose structure is safe while it does not know. Right now the instrument is telling us we do not know.

Everything described above is built and deployed, and one piece of it is not yet doing what I said it does.

The hourly job that folds settled outcomes into the theme memory is live, and the memory has started filling. The hierarchy serves the cold start. But because the prior is consulted only when no cascade tier fits, **macro's influence on a live forecast today is approximately zero** — structurally, not as a tuning choice. Making it an additive term with a floor and a ceiling is the next change. It is the one that turns the theme layer from an idea into a term in the answer.

Five feature families now attach where four did before, and why that took so long is worth telling. One family had never once computed. The builder unpacked a four-value return into two names, the exception that raised was swallowed, and zero of 1,425 scored rows ever carried those features. The suite was green the entire time, because the test double declared the same wrong signature the buggy code assumed. **A double built from the code's own assumptions cannot contradict them.** It took a run against the real interface to see it.

What all of this buys is a clock, not an answer. The system places about 108 predictions a day. Each settles after five trading days and attributes to between one and three firing themes, so a day of running yields roughly 100 to 300 theme observations. Against a $K$ in the region of 40 to 80, a theme needs several hundred before its effect moves meaningfully away from its parent. The macro boards cannot be backfilled, so this is the only rate at which that evidence can ever arrive. Gate H is weeks from being answerable, and it was written down before any of it started, which is the only reason its answer will mean anything when it comes.

The order of work is settled, and the first item is the cheapest thing in the whole program. Widening the universe from 36 symbols to about 200 multiplies theme observations by roughly five and a half times a day. No backfill. No history to buy. And no curation bias at all, because every observation it produces is forward-looking. It buys the same evidence as a six-month replay with none of the trap. After that: macro additive with its bounds, then apportionment and de-duplication, then the credibility weights.

The gate for that work was written down before any of it started:

> **Gate M.** With macro additive and apportioned, the full estimate's macro contribution sits inside 10% to 35% on live bets, and the evaluation reports the macro term's out-of-sample lift with an interval — whichever way it points.

A null result there would say that macro adds nothing measurable at any weight in the band. That is a deliverable, and it would be worth more than another decade of asserting that macro must matter.

I want to be careful about one thing. The price-spine result above is decisive, and it is negative, and it is a verdict on seven price features. It is not a verdict on the architecture, because the twenty-year corpus carries no news whatsoever. The theme layer has barely any settled observations yet and has therefore been graded on nothing. Presenting a price-only result as a verdict on the whole design would be exactly the error this article is about, so we are not doing it.

A system reporting 52% with no denominator, no interval and no baseline board would look better than what I have just described. It would also be worth less. It could not tell the difference between a real signal and the drift that every stock in the market shares for free.

The measurement came first on purpose. Building a predictor before you can measure one produces a number nobody can interpret and a team that argues about it. Building the measurement first produces a verdict — sometimes a disappointing verdict, delivered quickly and cheaply, which is the best thing a research program can buy.

None of this is investment advice, and nothing here should be used to make a trade.

## What is open

The knobs below are genuinely open, and I would rather be argued with than agreed with on any of them.

- **The shrinkage constant $K$, on live evidence.** The sweep above ran on price history. The values running in production are placeholder round numbers, and tuning them against settled theme outcomes cannot begin until there are settled theme outcomes.
- **Theme effects are lifetime means.** A theme's implication drifts with the regime, so old evidence should decay rather than count forever. The trust ledger elsewhere in this system already has half-life machinery, and this should reuse it rather than reinvent it.
- **The hierarchy's shape.** The price spine has three levels and the theme hierarchy has three. Industry, country, and factor-exposure levels are all plausible parents. More levels means more parameters and less evidence per node, and where that trade turns negative is an empirical question we have not answered.
- **The macro weight $w_{\text{macro}}$ is untuned.** It runs on a pre-registered grid with the 10% floor as a hard constraint, and the winner is deflated for multiple testing. No value of it has earned anything yet.
- **The 15–25% band is borrowed, not measured.** It rests on other people's variance decompositions, not on our bets. Replacing it with a measurement is what Gate M is for.
- **The credibility terms are guesses with the right shape.** A replayed observation worth 0.35 of a live one. A 180-day half-life on curation. A 0.1 crowding penalty. Every one of those is a starting point, and the structure matters more than the values.
- **The historical replay cannot validate anything, and must never be presented as if it could.** Six months across 36 correlated mega-caps is about 4,500 overlapping bets, worth perhaps 300 to 900 independent ones, against the 4,900 a 52% edge needs. It is a bootstrap and a product surface. Nothing more.
- **Residual attribution assumes the spine is right.** Crediting a theme with `actual − spine expected` is only sound if the spine's expectation is unbiased. If the spine is systematically wrong for a sector, that error lands in the themes and looks like a discovery.
- **The effective-sample-size correction** uses the standard overlapping-window inflation factor. It does not yet account for cross-sectional correlation between names, which means our $n_{\text{eff}}$ is optimistic. The honest number is smaller than the one we report.
- **Multiple testing is counted, and the counter is only as honest as the log.** The evaluation command tracks trials to date and deflates significance accordingly, along the lines Lopez de Prado and others have proposed for backtest evaluation. It can only count the trials that passed through it. An idea discarded at a whiteboard never enters the count, and it was still a test.

If you work on this class of problem and something above is wrong, I would like to hear it. The measurement apparatus is the part of this system I am confident about, and the fastest way to improve an instrument is to hand it to someone who wants to break it.

---

## Sources and notes

The measured figures in this article come from our own evaluation harness, and each carries its denominator where it appears. The ideas it leans on are other people's:

- Narasimhan Jegadeesh and Sheridan Titman, "Returns to Buying Winners and Selling Losers: Implications for Stock Market Efficiency," *Journal of Finance*, 1993. The source of the 12-1 momentum baseline.
- Kent Daniel and Tobias Moskowitz, "Momentum Crashes," *Journal of Financial Economics*, 2016. Why a momentum mean and a momentum median disagree.
- Marcos Lopez de Prado, *Advances in Financial Machine Learning*, Wiley, 2018. Purged and embargoed walk-forward cross-validation.
- David H. Bailey and Marcos Lopez de Prado, "The Deflated Sharpe Ratio: Correcting for Selection Bias, Backtest Overfitting, and Non-Normality," *Journal of Portfolio Management*, 2014. The multiple-testing correction referred to in the closing section.
- Bradley Efron and Carl Morris, "Stein's Paradox in Statistics," *Scientific American*, 1977. The batting-average demonstration of shrinkage that the hierarchy is built on.

Nothing in this article is investment advice.
