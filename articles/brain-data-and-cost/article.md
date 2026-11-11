A shipping company files its earnings in May, then corrects them in July. The corrected figure belongs in today's research. A forecast made in June needs the figure that was available then. A data library earns its keep by being able to answer both questions.

## The right source for the question

An exchange measures trades. A company reports its accounts. A statistical agency publishes estimates that may change after the first release. Each source supplies a different part of the evidence, and each has its own clock.

| Evidence | Where it comes from | What needs checking |
|---|---|---|
| Prices and volume | Exchanges and licensed vendors | Instrument, session, currency, and adjustments for splits or dividends |
| Company fundamentals | Filed accounts and financial data vendors | Fiscal period, units, release time, and restatements |
| News and events | Company announcements and licensed news feeds | First publication, receipt time, company named, and duplicates |
| Economic conditions | Statistical agencies and central banks | Initial release, revisions, and when each version became available |
| Sectors and peers | Classification histories and benchmark feeds | Membership at the time and how the benchmark is defined |

The SEC makes filings and structured financial facts available through EDGAR. The St. Louis Fed's archive lets researchers retrieve values known on earlier dates. That vintage date helps establish what the public could know. The time OakQuant received the record still needs its own receipt. [SEC EDGAR APIs](https://www.sec.gov/search-filings/edgar-application-programming-interfaces) · [FRED real-time periods](https://fred.stlouisfed.org/docs/api/fred/realtime_period.html).

## Keep the meaning with the number

Timber is OakQuant's shared data library. An adapter translates each feed into a common form and keeps the source attached. A price record says whether it reflects a split. A filing carries a stable company ID, the fiscal period, the units, and the version we received.

The checks have practical consequences. A price that falls by half could reflect a stock split, a bad record, or a real move. Each calls for a different treatment. A company that reported zero profit has supplied a fact. A missing release or a failed request leaves a gap.

## Give every record its three clocks

| Clock | What it tells us | Example |
|---|---|---|
| Observation period | When the activity took place | The quarter ended March 31 |
| Release time | When this version became public or otherwise eligible for use | The filing went out May 8 at 3:58 p.m. |
| Receipt time | When this version reached the system | The feed delivered it at 4:03 p.m. |

In this example, a 4 p.m. replay of OakQuant's forecast excludes the filing. The filing was already public. The system had yet to receive it. A study of what the public could know uses a different clock and needs to state that choice. If a record still needs approval or validation at receipt, a live replay uses the later time at which it became usable.

A July restatement still describes the March quarter. It gets its own release and receipt times. Keep both versions and the source for each. If we know only the date, use a cutoff rule that allows time for the record to arrive. An intraday replay needs evidence precise enough for its cutoff. [Point-in-time memory](/public/articles/brain-point-in-time-memory).

## Count the event once and keep the trail

Start with a source ID and version, or a hash of the text after removing feed boilerplate. This catches repeat deliveries and exact copies. Compare near copies within a short time window using the company, event type, and phrases they share. An earnings release and a credit downgrade about the same company need separate event IDs.

Suppose three feeds deliver the same announcement, and a fourth adds a new earnings figure. Keep each receipt and its source. Group the repeat copies under one event so the forecast sees one announcement. Store the new figure as a dated update if it changes that event, or as a separate event if it describes a different fact. A replay before the fourth receipt sees only what arrived earlier.

Test the grouping on hand-labeled pairs. A missed match can make one story look like several. A wrong merge can hide a second event. Measure both errors, and check how they change any feature based on the number or timing of events.

## Buy only the evidence the task needs

A request states the fields, dates, freshness, permitted use, and budget it needs. Timber checks its stored records first. Several models can share a verified snapshot when the license allows it. A daily forecast can often use the last completed session. An intraday forecast needs evidence available before its own cutoff.

When the cache falls short, a provider can fill a specific gap. The price per call is one part of the decision. Retries, repairs, time to receive the data, and the rights to train, display, or redistribute it all matter. If no approved source fits, the gap stays visible and an eligible fallback can run.

Suppose a feed charges one cent and returns a usable record on half its requests. A three-cent backup always succeeds in this example. Trying the cheaper feed once and using the backup on failure costs 2.5 cents per usable record on average. That half-cent saving can disappear if the extra call takes too long.

Cambium applies the same discipline to computation. It routes exact calculations to code and empirical questions to statistical routines. When a task needs a language model, it sends a short brief with the facts required for that task.

A new feed earns its place on matched tests. Keep the target and dates fixed, then measure what the feed adds in usable coverage, timeliness, forecast scores, and total cost. Two vendors carrying the same source supply one piece of evidence, even when their numbers agree.

---

[Building a Brain](/public/articles/building-a-brain) · [Baselines and adaptive gradients](/public/articles/brain-baselines-and-adaptive-gradients)
