A port closes. The first report names the terminal and the expected length of the interruption. The harder questions concern the companies that depend on it. Can Maple Freight move cargo through another port? Which customers will feel the delay? What changes if the closure lasts a month?

A language model can propose those questions and point researchers toward records worth checking. A forecast needs dated evidence about the routes, capacity, and companies involved. A scenario can be useful even when there is too little evidence to assign it a probability.

## What the language model predicts

An autoregressive language model estimates which **token**, or unit of text, comes next. It adds that token and repeats the process. Attention lets it use relationships among the words and passages in its context. The task for a return model is to estimate a defined market outcome, such as the spread of possible returns over five trading sessions. It learns from settled examples of that outcome. [Vaswani et al. on attention](https://proceedings.neurips.cc/paper/2017/hash/3f5ee243547dee91fbd053c1c4a845aa-Abstract.html).

That difference shapes how OakQuant uses generation. It can help phrase a question, propose a test, or explain a forecast. If it proposes a new signal, that signal enters research. The researchers need a source they can use and a test on dates the model has never seen.

## Give it a brief that can be checked

A forecast explanation starts with the recorded target, horizon, probability, and return interval. It also needs the relevant source facts and any gaps in the evidence. If the probability uses data from the broader sector, the brief names the group that supplied it. Cambium can reduce the brief to the fields the task needs and the data the selected provider is permitted to receive.

Code supplies the arithmetic and checks that the numbers in the draft match the saved forecast. The explanation needs a source for each reported fact. Where verification fails, a fixed template can show the values and their limits directly. Fluent text can contain invented facts and citations, a risk NIST calls **confabulation**. [NIST Generative AI Profile](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf).

## Put extracted facts through their own test

A language model can also turn a filing or transcript into a structured candidate feature. It can identify a plant closure, a supplier, or a disclosed fuel exposure. Evaluate the extraction against labeled records. Then measure whether the feature improves the forecast enough to justify its cost and delay.

Historical text needs special care. A modern extractor may have learned later events during pretraining. An old filing's date alone cannot establish that the extractor was free of hindsight. Freeze the extractor and collect a forward test with a dated record of every document and result.

## Measure what the text step adds

Ask the extractor for a named event, the company involved, the date, and the source passage that supports it. A passage about a port closure can name a carrier that diverted ships and another that kept sailing. Save the roles so they do not turn into the same event flag.

Suppose 15 passages in a hand-labeled set describe closures. The extractor flags 12 passages and gets 9 right. It got 9 of its 12 flags right, a measure called **precision**. It found 9 of the 15 real closures, its **recall**. Look at both counts and read the misses. A feature that confuses plans with events that took place can shift a forecast even when its overall score looks good.

Then replay the same forecasts with and without the extracted event. Count the extra compute time and the delay before the feature is ready. A clear summary can help a reader even if the signal adds little to return forecasts.

A reader should be able to trace the difference between a fact in a filing, a factor that changed the model's forecast, and an explanation of what might have caused a market move. Each is a different claim and carries its own evidence.

---

[Building a Brain](/public/articles/building-a-brain) · [Giving the brain good senses](/public/articles/brain-data-and-cost) · [Why a learning brain needs rules](/public/articles/brain-rules-engine) · [Point-in-time memory](/public/articles/brain-point-in-time-memory)
