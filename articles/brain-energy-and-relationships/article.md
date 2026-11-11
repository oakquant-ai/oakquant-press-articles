Two sectors can each have an even chance of rising over five sessions. They could still rise and fall together much of the time. A forecast for each sector on its own leaves that joint behavior open, and a household that owns both has reason to care.

## Estimate which signals move together

An **energy model** assigns a score to each joint state. Lower energy gives a state more probability after the scores are normalized. A small Ising model can do this for two sector return signs $s_1$ and $s_2$, each either −1 or +1. With no separate directional bias and a coupling $J$, its energy is

$$
E(s_1,s_2)=-Js_1s_2,\qquad
P(s_1,s_2)=\frac{\exp[-E(s_1,s_2)]}{\sum_{u,v}\exp[-E(u,v)]}.
$$

With $J=0.5$, each of the two matching states gets about 0.3655 of the probability. Each of the two opposing states gets about 0.1345. Each sector still has a gain probability of one half. The chance that they move in the same direction rises from one half under independence to about 0.7311. These are model values for an invented example. [Wainwright and Jordan on graphical models](https://doi.org/10.1561/2200000001).

With eight signs, there are 256 joint states. The model can sum them all. The count doubles with each new sign. Test the joint forecast against models that treat the signs as independent or explain them with one market factor. Fit return sizes separately.

A joint score tests the pattern directly. If both sectors rise in the toy example, the joint model assigned that state about 0.3655. The independent model assigned it 0.25. If one rose and one fell, the ranking reverses. **Joint log loss** takes the negative log of the probability of the state that occurred. Score many later states before judging either model. A good fit to the separate up rates can still give a poor score on the pairs.

## Learn patterns with hidden units

A **restricted Boltzmann machine** connects observed inputs to hidden units. It has no links within either layer. A version with Gaussian visible units can take continuous inputs such as scaled returns and volatility. Its hidden activations can supply features to a supervised return model. [Hinton's guide to restricted Boltzmann machines](https://www.cs.toronto.edu/~hinton/absps/guideTR.pdf).

Fit those features on earlier data and compare them with principal components, simple feature products, and other ways to summarize the same inputs. Keep the forecast target and test dates fixed. Samples drawn from a fitted model reflect its assumptions and belong in simulation. An energy score needs its own validation before it can serve as a warning about unfamiliar inputs. Studies of other density models show why a high likelihood alone can mislead. [Nalisnick et al. on out-of-distribution inputs](https://arxiv.org/abs/1810.09136).

## Use the links a company actually has

Maple Freight depends on ports, fuel suppliers, and customers. A **graph model** represents those entities as nodes and their dated relationships as edges. A graph neural network can use information from neighboring nodes to update the representation for Maple. That gives it a way to use news about a port in a forecast for a carrier that depends on it. [Kipf and Welling on graph convolution](https://arxiv.org/abs/1609.02907).

An edge has a type and a date. A fuel supplier, a shared port, and a customer carry different information. The model can pass their features along separate paths. It can also weight links by exposure when the source supports that measure. A link to one port in a large network means something different from a link to the only port on a route.

The graph can use only links known at the forecast cutoff. Compare it with a simple average of neighbor features, with industry groups, and with shuffled links. Test whether the graph adds information after accounting for common market and sector moves. A link shows who is exposed. We still need data on what it costs.

---

[Building a Brain](/public/articles/building-a-brain) · [Temporal learning and the order of events](/public/articles/brain-temporal-deep-learning) · [Combining forecasts and calibrating confidence](/public/articles/brain-combining-and-calibrating) · [Point-in-time memory](/public/articles/brain-point-in-time-memory)
