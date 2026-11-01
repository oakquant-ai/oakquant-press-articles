Suppose a stock trades at $100. The forecast puts it at $110 by the end of the next trading week. It closes at $94.

The forecast was clear. The market had other plans.

Keep both numbers. Does the original estimate stay where you can inspect it, or does a fresh chart arrive and let everyone forget?

In OakQuant, the financial research and planning platform I built, I want the prediction to remain answerable to the outcome. The line, the range beside it, and the record of past results each have a job.

Bring your own view. Following a company becomes more interesting when you can examine how an estimate was made and return later to see what happened when its date arrived.

## Several methods contribute to the estimate

Open a company in the research workspace and you can inspect its forecast alongside the news and other evidence behind the outlook. The chart shows a price estimate across several time horizons.

Four models contribute. Each reads the price history differently.

Momentum carries recent price changes forward. Mean reversion pulls the estimate toward a recent average. One extends the move. The other expects a return.

Ridge regression looks for a steady link between the price and clues such as recent gains and how jumpy the price has been. It holds back from leaning too hard on any one clue.

Holt's exponential smoothing keeps updating its estimate of the price level and trend as observations arrive, then extends that smoothed trend into the future. It does not look for seasonal cycles.

The forecasting service combines their estimates using weights, which decide how much influence each method gets. Combining several methods this way is called an ensemble.

Think of asking several people to estimate how long a journey will take. You may give more weight to the person who drives that route often, while still listening to someone who knows about roadworks.

The weights matter. Giving more say to someone with useful knowledge can improve the combined answer, while adding another voice with the same blind spot may leave you no better informed.

OakQuant also brings in signals, including price patterns, news sentiment, and economic context. Those signals can adjust the point estimate. You can inspect the model weights and signal breakdown rather than relying on the chart's direction alone.

## The band has its own calculation

A journey estimate of forty minutes leaves an obvious question. How much could it vary?

Price has that question too. The method used to draw the shaded band matters because two methods can produce quite different ranges around the same estimate for the same future date.

In OakQuant's ensemble, each component model produces a lower and upper bound. Some use the size of errors when fitted to past data. Others use measured price variation, with adjustments for the forecast horizon.

The ensemble takes a weighted average of the lower bounds and another of the upper bounds. Those are the edges of the band. The spread between the models' price estimates is not used to set its width.

Signals or the shared prediction engine can change the displayed point estimate. The band stays at the ensemble's original lower and upper price levels, so it does not move with the adjusted line. The estimate can sit off-center or even outside the band.

Read the band on its own terms. To know how often future prices fall inside it, count the later results against the ranges that were saved before those prices arrived.

![The stock starts at $100. At the end of one trading week, the forecast is $110 and the actual price is $94. The synthetic band spans $82 to $138 at that horizon, so the actual price is inside the band despite the wrong directional forecast. The range is fixed at the forecast date. These are teaching values, not OakQuant results.](images/forecast-meets-reality.png)

In this synthetic chart, the band runs from $82 to $138 at the end of the trading week. The $94 close falls inside it. The price fell from $100 even though the $110 forecast pointed up, so the direction was wrong while the range still contained the outcome.

A wide band can contain a large miss. To judge its usefulness, look at how much room it leaves as well as how often prices land inside it.

## Keep the prediction long enough to check it

Return to the example at the start. The forecast was $110 and the later price was $94. To assess that result, you need the estimate as it stood before the outcome arrived.

Check the date and the horizon, because a forecast for next week and one for next year answer different questions even when both show the same price.

OakQuant records the forecast shown during a research run. Its adaptive prediction engine also maintains a record of directional predictions and scores them against later outcomes. Those records serve different checks.

Did the price rise or fall as expected? That is what a directional score checks. Testing the band means counting how often the price landed inside the range, and a correct direction can still leave the size of the move far off.

The premium prediction track record shows scored predictions beside their outcomes, along with strategy results and learned weights. Each strategy has a scored-prediction count. You can see how many results sit behind its score.

Watch for overlap. Forecasts made on successive days can share most of the week they are trying to predict, which means several results may depend on the same price moves.

When the settled records are available, the track record uses their dates to estimate how much independent evidence those overlapping forecasts represent. That estimate is called the effective sample size. The page shows it alongside the count of scored forecasts.

The adjustment widens the uncertainty around the hit rate when forecasts overlap. The hit rate itself stays the same. Making more forecasts over the same price moves adds less evidence than the raw count suggests.

## A changing weight needs a reason

As outcomes arrive, scores can change how much influence a strategy gets in future estimates. That gives the system a way to respond when a method's recent performance changes.

You can inspect the change. Look for the strategy that gained weight and the results that earned it, then check how much evidence the engine had when it made that choice.

A weight update is a decision inside the software. Its value appears in the forecasts that follow, so the record has to continue after the update too. A system that learns needs somewhere to put its bad grades.

For a reader, this changes the task from admiring a chart to examining an argument. The estimate says what the system expects. The band describes a calculated range. The track record gives you evidence about past predictions.

Choose a company you already have an opinion about. Open its forecast and inspect the signals behind it. Where does the outlook agree with you? What gives you a reason to look again?

Note the forecast and its horizon. Return after that period and compare the estimate with the outcome, using the same date and price you recorded when you first looked.

You can begin in the [research workspace at sky.oakquant.ai](https://sky.oakquant.ai/research). The [prediction track record](https://sky.oakquant.ai/predictions) is available with premium access.

This is the fourth article in the OakQuant series. The earlier pieces cover choosing when to use AI, routing a portfolio review, and designing an agent that can discover useful tools.

The prices and chart in this article are illustrative. OakQuant is for research and education. Forecasts can be wrong, so check the evidence before relying on them.
