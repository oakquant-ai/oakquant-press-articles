A language model in my software was given 600 tokens to reason and write its reply, and it spent 572 of them thinking. The visible answer was 24 tokens long. Half a sentence.

The software saved the fragment and showed it to users because a request had gone out and some text had come back. That counted as success.

That happened in OakQuant, the financial research and planning platform I built. The failure is recorded in the code because I want the next person changing it to understand what went wrong.

A token is a small chunk of text. Models use tokens for reading and writing as well as reasoning, so work that produces no visible words can still count toward the bill and the request limit. In this case, reasoning consumed most of the allowance before the answer reached the reader.

The fix started with a fact the software had thrown away: why the model stopped. The shared response now exposes a flag when the output limit cuts an answer short. The caller can then retry, use a fixed fallback, or show the result as incomplete.

The model had done work. The user still needed an answer.

That distinction runs through how I build OakQuant. Before choosing a model, I want to know what job needs doing and what would count as finishing it.

## Some questions already have an answer

You do not hire a PhD to add two numbers. You use a calculator.

The calculator follows a rule, and you can check its result. A question about what might happen next month asks for something else. A question about how to explain that uncertainty asks for something else again.

Putting all three through a language model makes the software easy to describe. It also means paying a model to do jobs that a lookup or a formula could finish directly.

OakQuant has a small library called Cambium that supports a cheaper order of work, and its `resolve` module tries three tiers until a result clears the acceptance threshold.

Take a request to find a saved workflow, which is a set of steps for completing a job. Someone might ask to review a portfolio or start an assessment.

First, try an exact lookup. If the request identifies a known workflow, retrieve it. There is no reason to write another copy.

Next, try a search by meaning. A person might say "check my investment mix" when the saved process is called "portfolio review." The wording differs, but the request may point to the same job.

That search uses an embedding: a list of numbers that represents the meaning of the text. Comparing those lists helps find likely matches. A close match is a candidate worth checking.

Finally, if the existing options cannot satisfy the request, a model can be asked to draft something new. That is the job that calls for generation.

An exact lookup follows a fixed rule, which engineers call deterministic behavior. A statistical method estimates a match from data. A generative model writes new content.

Cambium supplies that order of work while leaving each application to define which answers are acceptable for the task its user is trying to finish.

## A confident answer still has to earn trust

Suppose a search returns "close account" when the user meant "close the account window."

The words are close. The consequences are far apart.

A similarity score tells you how closely two things match under a particular method. To find out how often the match is right, test it against requests with known answers.

Cambium's default acceptance threshold is 0.8. The application can set a different bar for a job where the consequences demand it.

The library can also accept trust weights learned from past results and discount the confidence of a method that has repeatedly given poor answers. A good record never raises its score above what it originally reported.

An application could record a failed workflow check against the method that proposed it, then supply an updated trust weight on later requests.

After several mistakes by a coworker who answers quickly and sounds sure, you start checking more of their work before passing the results to someone else. Speed remains useful. Certainty stops persuading you.

A spending limit adds another boundary. When the budget is exhausted, the resolver can return its best candidate even if it fell below the threshold. The application should show that result as a fallback, with its uncertainty intact.

## Two people give the same answer

OakQuant's Financial DNA assessment puts a question in front of you: your $100,000 portfolio has fallen to $70,000. What do you do?

Sell everything. Sell some. Hold steady. Buy more.

Suppose two people choose "Hold steady." One selects it at once. The other first chooses "Sell everything," changes their mind, and submits the same answer after a long pause.

A conventional questionnaire could record identical answers. There is more to observe.

The screen records when a person arrives, when they first interact, and when they submit. It also counts changes to the answer. Those details can put the final choice in context.

The second person has given you something the final answer alone would hide: a change of mind. That suggests a follow-up. What made selling everything look right at first, and what made holding steady look better a moment later?

Timing and answer changes give the conversation somewhere to start. Someone may be weighing the loss. Someone may be answering the door. A clock is useful, but it has never met either person.

The assessment uses explicit scoring rules. Given the same inputs and version of those rules, you can reproduce the profile and follow how it was reached.

The interesting design choice is what to record. Asking a model to write more words would add little if the software had already thrown away the hesitation.

## Send the model only what the job needs

When a model is useful, the next saving is often in the question.

Suppose you want a short summary of a news item. The data bundle may also contain repeated fields, processing details, and records that have no bearing on the summary. Sending the whole bundle makes the model read material the reader will never need.

Cambium's `distill` module prepares that input. It can remove unnecessary material, shorten a payload, or recognize that a fixed output can be assembled without a model call.

In one news-sentiment test, a bundle went from about 14,000 tokens to about 1,300. That is one test, with the input reduced to roughly a tenth of its original size. The next check is whether the answer still contains what the reader needs.

Keep the date and the source. Keep the uncertainty. Removing any of those could change the meaning of the answer and leave the reader with a cheaper mistake.

## Count the answer the reader received

The half-sentence at the start counts as unfinished work, however busy the model was.

A cost report needs that same care because a lower bill tells you little until you inspect the quality of the work that reached the user. Failed attempts, retries, and time spent correcting errors belong in the account too.

The useful discoveries here came from looking closely at the job: a stop reason the caller needed, a hesitation the questionnaire could preserve, and a news bundle carrying far too much. Each changed what the software should do before model size entered the discussion.

Choose one task in your next AI feature. Write down the answer the user needs and the cheapest method that might produce it. Then name the check that would tell you it worked.

This is the first of four articles about building OakQuant. The next follows a portfolio review through code, a model, and a human decision.

Start with the Financial Digest at [sky.oakquant.ai](https://sky.oakquant.ai/), then open a company in the research workspace and follow the evidence behind its outlook.
