A burst of trades just before an earnings release raises different questions from a burst just after it. A single volume total hides the difference. A sequence model keeps the order of trades, returns, and dated events so it can test whether the timing improves a forecast.

## Keep a running state

A recurrent neural network reads a series in order and updates a numerical state at each step. A **gated recurrent unit**, or GRU, learns how much of the previous state to keep and how much to replace. It can retain older evidence while responding to a new event. [Cho et al. on gated recurrent units](https://arxiv.org/abs/1406.1078).

Using $z_t$ for the share assigned to the candidate state $\tilde h_t$, one common convention writes the update as

$$
h_{t}=(1-z_{t})h_{t-1}+z_{t}\tilde{h}_{t}.
$$

If one element of the previous state is 0.2 and the candidate is 0.8, giving the candidate one quarter of the weight produces 0.35. The model stores that value. Its output layer uses the state to make a forecast.

## Read the path at several scales

A **temporal convolutional network** applies learned filters across the sequence. A causal filter uses the current step and earlier steps. A dilated filter skips positions so a stack of layers can cover a longer history with fewer weights. [Bai, Kolter, and Koltun on temporal convolutions](https://arxiv.org/abs/1803.01271).

| Example | Ordered daily changes | Sum of changes | Latest change |
|---|---|---:|---:|
| A | −2, −1, +1, +2 | 0 | +2 |
| B | +1, −1, −2, +2 | 0 | +2 |

The toy values use percentage points. The sum and latest move agree. A filter that subtracts the previous day's change sees +1, +2, +1 in A and −2, −1, +4 in B. It can distinguish a gradual recovery from a sharp rebound. A sum of daily returns is a summary here. A realized holding-period return compounds the daily moves.

## Compare distant events

**Attention** lets a model weight earlier positions directly. Time and position inputs help it distinguish an event yesterday from the same event a month ago. A forecast can include a future meeting date when the schedule was known at issue time. The decision announced at that meeting enters only after it becomes available. Temporal Fusion Transformers distinguish such scheduled inputs from observed history. [Lim et al. on Temporal Fusion Transformers](https://arxiv.org/abs/1912.09363).

## Test which signals lead

Suppose a freight-rate index rises several sessions before Maple Freight moves. A **lead-lag relationship** means an earlier reading of one series helps predict a later reading of another. The gap has a direction and a length. It can change with the route, the market, or the time the index reaches the system.

First align the series by what was known at each cutoff. If an index describes Tuesday and is released Thursday, Thursday is the first day it can enter the forecast, subject to the receipt time. Use changes or returns where that fits the question. Two price levels can drift in the same direction for years and show a strong correlation with little value for predicting the next move.

Start with a model that uses Maple's own lagged returns and market or sector moves. Then add earlier readings of the freight index. Pick the candidate lags on training and validation dates. Use the same later dates to score both models. This is the practical idea behind a Granger-style test. Past values of one series must add forecast information given the other history already in the model. A causal claim needs evidence that separates the freight shock from shared forces such as fuel prices and demand. [Granger on predictive relationships](https://doi.org/10.2307/1912791).

Try the comparison in several windows. Check whether the gain survives a change in market conditions and the use of actual release and receipt times. When many lags were tried, judge the winner on a test period that played no part in the search. Then ask whether the sequence model improves on the simpler model with those same lagged inputs.

## Make order prove its value

Two adjacent 60-session windows share 59 input sessions. Their targets can share some of the same returns too. Split by time and remove training labels that settle across the test boundary. Fit the scalers and any feature model on the training dates. Pretrain within that window too.

A shared network can learn across companies while keeping company and sector information available to the model. Compare it with simple lag features and boosted trees on the same cases. An order-shuffling check can show whether the network relies on timing. Held-out forecast scores and compute costs determine whether that reliance is worth paying for.

---

[Building a Brain](/public/articles/building-a-brain) · [Trees and the conditions that change an answer](/public/articles/brain-trees-and-interactions) · [Energy models and relationships](/public/articles/brain-energy-and-relationships) · [Point-in-time memory](/public/articles/brain-point-in-time-memory)
