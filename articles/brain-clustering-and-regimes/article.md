Two shipping companies can look alike on a list of industry codes and respond quite differently to a credit squeeze. A carrier and a retailer may share a port dependency even though their stocks rarely move together. The right grouping depends on the question.

## Five uses for groups

| Task | What the grouping contributes |
|---|---|
| Company peers | Firms with similar past behavior or business exposures can share evidence in a hierarchy. |
| News and events | Separate duplicate wire copies, reports of the same event, and broader topics. |
| Market regimes | Group dates by shifts in prices, credit spreads, or market breadth. |
| Historical comparisons | Retrieve earlier episodes with similar inputs and outcomes already known at issue time. |
| Model errors | Find specialists that miss on the same settled cases, so shared errors remain visible. |

The inputs tell the model what to compare. For peers defined by trading behavior, first estimate and remove the broad market move on earlier dates. For peers defined by what companies do, compare routes, customers, and costs. A news model can use **text embeddings**, lists of numbers that place related passages close to each other.

## Find peers for the question at hand

Suppose two carriers rise with the market. After we subtract their estimated market exposure, one still moves with Maple Freight when fuel prices change. The other trades more like the firms in its domestic market. That first carrier gives Maple a candidate peer for fuel exposure. A test can compare peers found this way with industry groups and a wider sector pool.

Start with trailing returns from dates the two companies share. Fit each company's market exposure on the training dates and compare the residuals, the moves left after removing that exposure. Add business facts if the question calls for them. Set the scale and weight of each input on earlier dates. For a firm with little shared history, a formal group can give the hierarchy a sounder starting point.

## Find models with the same blind spot

Two methods can disagree on normal days and both miss when port delays spike. Line up the forecasts they issued for the same company, cutoff, and horizon. For a return estimate, subtract the realized return from each prediction. For a gain probability, subtract the outcome flag from the stated chance. Compare each kind of error within its own task.

A group of models with similar errors deserves a check as a group. Ask how much the second model improves the blend after the first is in it. Repeat the test on later dates. A score for each model on its own leaves that question open. Where the shared record is thin, keep track of how many dates support the apparent link.

## Choose the number of groups

Try a small range of group counts on training windows. A **silhouette score** compares how close a point is to its own group and how close it is to the next best group. Use it to spot groups that run together. Also check the size of each group. A group with three weeks from the same selloff gives us little basis for a separate forecast rule. [Scikit-learn on choosing a cluster count](https://scikit-learn.org/stable/auto_examples/cluster/plot_kmeans_silhouette_analysis.html).

Refit on nearby windows and with different starting seeds. Assign the same held-out records under each fit. Then check how often pairs stay together, or use the **adjusted Rand index** to compare the two sets of labels after accounting for chance. For market dates, resample blocks of sessions so the test keeps some of the dependence within a selloff.

A stable group can still be of little use. Compare the models with and without each grouping on later dates. Choose the simplest group count that gives dependable gains for the task. [Von Luxburg, Williamson, and Guyon on clustering for a purpose](https://proceedings.mlr.press/v27/luxburg12a.html).

## Let an uncertain state stay uncertain

**K-means** assigns each record to its nearest center and fits those centers to reduce squared distance within groups. Feature scale affects distance, so fit any scaling on the training window. A Gaussian mixture can give a record a probability of belonging to each group. [Scikit-learn on clustering](https://scikit-learn.org/stable/modules/clustering.html) · [Gaussian mixtures](https://scikit-learn.org/stable/modules/mixture.html).

Suppose a model assigns probabilities of 0.70 and 0.30 to calm and stressed markets. Its five-session chance of a gain is 0.55 in calm conditions and 0.40 in stress. Combining those estimates yields

$$
p=0.70(0.55)+0.30(0.40)=0.505.
$$

A hard assignment to calm would give 0.55. Keeping both states gives a forecast that reflects uncertainty about which one applies.

## Respect the order of the evidence

A **hidden Markov model** learns how often a state tends to last or change. Filtering estimates each state’s chance from the facts so far. Smoothing also looks at what happened next. To replay a forecast, fit the model on earlier data and use filtering. A later crash cannot become evidence that the market was already in stress at issue time. [Rabiner on hidden Markov models](https://www.cs.ubc.ca/~murphyk/Bayes/rabiner.pdf).

A new fit can swap the numbers used to name the groups. Match them by who is in them or where their centers lie. Save both fits. Allow an unknown state when the current inputs are far from the fitted groups. Several nearby dates in a single selloff can be useful comparisons, though they share much of the same evidence.

For a regime model, compare the forecast with one that sees the original market inputs without the group labels. The group earns its place if it helps on later cases or gives comparable forecasts at lower cost.

---

[Building a Brain](/public/articles/building-a-brain) · [Bayesian hierarchies and borrowed evidence](/public/articles/brain-bayesian-hierarchies) · [Combining forecasts and calibrating confidence](/public/articles/brain-combining-and-calibrating) · [Point-in-time memory](/public/articles/brain-point-in-time-memory)
