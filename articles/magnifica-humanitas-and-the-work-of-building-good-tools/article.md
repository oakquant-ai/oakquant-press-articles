# Magnifica Humanitas and the Work of Building Good Tools

On May 25, 2026, the Vatican released Pope Leo XIV's first encyclical. It is called *Magnifica Humanitas*, and it is about artificial intelligence: five chapters and 245 numbered paragraphs, roughly the length of *Laudato Si'*. Leo signed it on May 15, the 135th anniversary of *Rerum Novarum*, the letter Leo XIII wrote in 1891 when the last industrial revolution was breaking families and rewriting what work meant. The date is a claim in itself. This Pope reads the arrival of machine intelligence as another turn of the same wheel that turned then, and he wants to be measured against that lineage.

The argument at the center of the document is one sentence long, and it is the part worth carrying around. Technology is never neutral. It takes on the character of whoever designs it, funds it, regulates it, and uses it. Everything else in the encyclical, the worry about surveillance and concentrated power, the warning against treating people as data and prediction targets, the appeal for systems whose responsibility stays legible when they grow opaque, follows from that single refusal to believe a tool is innocent of the hands that shape it.

-----

## The serious voices were all saying it

What struck me was not the encyclical on its own. It was how closely it tracked what the most serious enterprise operators had been saying in a completely different vocabulary.

Lori Beer, the global CIO of JPMorgan Chase, was blunt this spring that the bank's agentic systems would not run through an outside vendor where they touch the core of how the firm does business. Her reasoning was not pride of ownership. It was that responsibility, control, and accountability cannot be outsourced at the exact point where the work matters most. Around the same time, Goldman Sachs CIO Marco Argenti was describing the near future in adjacent terms: models turning from chat windows into operating systems that act on your behalf, personal agents arriving for ordinary people, electrical power becoming the binding constraint, and a small set of mega alliances bending the whole landscape toward a winner-takes-most shape, with more than half a trillion dollars of hyperscaler spending behind it in a single year.

Put the Pope, the two CIOs, and the sovereign-AI debate in the same room and they are circling one question from four angles. Leo asks whether our tools will widen the space for human dignity or compress it into the logic of efficiency, control, and profit. Beer asks who holds the controls when an agent touches the flow of the business. Argenti asks who ends up owning the scale. The European autonomy debate, the open-weights movement, and the Global South's resistance to digital dependency all ask the same thing at the level of nations: who gets to build, who gets to wield, and who is left consuming someone else's system. The common thread is simple to state. Intelligence without governance is not infrastructure. It is exposure.

-----

## The missing middle is practice

Most commentary on AI right now lands in one of two registers. There is launch-day enthusiasm, where every capability is a marvel and every concern is friction. And there is generalized ethical alarm, where the technology is a gathering storm and caution is the only respectable posture. Both are easy to write. Neither is much use to a person actually deciding how to build or what to ship.

The register that is missing is the practitioner's. Someone explaining, concretely, what it means to use these systems well: where the gains are real, where the risks are real, and how the design of a thing decides which way it tips. That is the register I want this essay, and this publication, to occupy. Not anti-AI, not boosterish, not safely detached. So instead of arguing the point in the abstract, I will describe what I have actually been doing.

-----

## The experiment I have been running

For about eighteen months I have run a small experiment on myself. Timber, Grove, and Sky began in the second half of 2025 as early attempts at bounded agent orchestration with a clean presentation layer on top. Through 2026, Cadence, Cambium, Acorn, Present, and a rebuilt OakQuant Press followed in quick succession, alongside a full draft of *The Cost of the Machine* trilogy, *The Referendum*, and a run of long-form essays on agentic architecture, sovereignty, and governance.

The dates matter only as a measure of pace. This is roughly a team-decade of multi-format output produced by one person in a year and a half, much of it built and written with AI as a partner. I report that plainly because the pace is the least interesting part of the story, and the part most likely to be misread as a boast.

The interesting part is the division of labor. The machine drafts, refactors, summarizes, tests, and orchestrates. I keep the questions, the framing, the structure, the judgment, and the consequences. Speed is a side effect. The real gain is continuity of thought: I can move between code, essays, slide systems, a publishing stack, and a fiction draft without losing the thread that usually snaps when a person crosses too many domains in a day. Quality holds because the architecture decides where the seams are, and the seams are where judgment lives.

-----

## What the tools are for

Cadence, Cambium, Timber, Grove, Acorn, Sky, Present, and OakQuant Press are not one product wearing eight names. They are a family of experiments around a single question: how should powerful AI be shaped so that it stays useful, legible, and accountable? Some are about orchestration, some about composability, some about presentation and publishing and helping a person think through hard questions with more structure and less noise.

The thesis underneath all of them is the same one that runs through the essays. Cognition can be distributed, accelerated, and amplified. Judgment cannot be shrugged off onto a black box. In practice that means designing bounded agents instead of romanticizing autonomy, separating concerns instead of hiding everything behind one interface, and privileging traceability, reversibility, and human override over anything that markets itself as magic. The point of the architecture is to let one person supervise more intelligently, not to let the person disappear from the loop. That is, almost word for word, what the encyclical means when it asks that responsibility stay legible as systems grow complex.

-----

## A personal laboratory, not a competing platform

This needs saying plainly. None of this is a product line. It is a personal laboratory.

My day work sits inside exactly the landscape Beer and Argenti are describing. At Pegasystems, in partnership with Accenture, the problems I help large regulated enterprises work through are governance problems before they are model problems: how an autonomous agent is given an identity, what it is permitted to touch, how its actions are logged and reversed, and where a human has to sign off. The distinction I keep returning to is easy to say and hard to build. Cognition is what agents are for. Coordination is what workflows are for. The intelligence can be bought. The accountability has to be designed. The version of this I argue for in enterprise rooms has a deliberately unglamorous name, the accountable agentic enterprise, and it means only this: you can always answer who is responsible when an agent acts.

The OakQuant tools and the writing are deliberately smaller in scope, independent in branding, and pointed at a different job. They exist to clarify ideas, test patterns, and make arguments concrete. None of the positions in this essay are my employer's or its partners', and none of these tools are Pega or Accenture products. The relationship is adjacency, not imitation. I am not trying to rebuild enterprise platforms in miniature or compete with systems that operate at a different scale and under far heavier obligations. I am building exemplars and intellectual scaffolding that help me think, prototype, publish, and communicate more clearly about the same shift those platforms are navigating at production scale. The conviction underneath both is identical, and it is the encyclical's: a tool carries the character of whoever shapes it, so the shaping is the moral work.

-----

## The dual-use problem is real

The optimistic story is not the whole story, and pretending otherwise would forfeit the credibility the essay is trying to earn.

Every capability here cuts both ways. A system that helps one person research, synthesize, and publish faster can also be aimed at flooding the world with persuasion, surveillance, and synthetic certainty. A bounded-agent framework can structure trustworthy work, and the same framework can be pointed at coercive ends by an operator who is careless or malicious. So the useful question is never whether a tool is good in the abstract. It is whether the design puts real constraints on misuse and preserves real accountability when something goes wrong. In my own work that means clean seams, explicit boundaries, logs, reversible steps, human sign-off where it counts, and a standing refusal to confuse fluency with authority. The goal is not to deny dual use. It is to build with dual use assumed.

-----

## The books and essays are the same project by other means

The software and the writing are not separate efforts. They are two forms of one inquiry.

*The Cost of the Machine* is fiction about what happens when frontier AI becomes infrastructure, leverage, theft target, state asset, and moral hazard all at once. *The Referendum* and the long essays on this site ask the same questions without the cover of story: who decides, under what constraints, with what architecture, and on whose sovereign assumptions. That is why *Magnifica Humanitas* matters to me, and it has nothing to do with sharing every premise or every word of its vocabulary. It matters because the document is wrestling seriously with the one question I keep circling, whether our tools expand the room for human dignity and responsible judgment or quietly collapse both into the logic of efficiency and control. My code, my essays, my slide systems, my publishing stack, and my fiction are all attempts to answer in the affirmative, with enough realism to admit how easily the answer goes the other way.

-----

## Why publish this now

There is an immediate reason to write this in May 2026 rather than later. The encyclical, the bank CIOs, and the sovereign-AI debate have, within a few weeks of each other, put the same question on the table in moral, enterprise, and geopolitical language at once. The thing missing from the coverage is the builder's account that sits between the boosters and the alarmists.

That is the seat I want OakQuant Press to hold. A place where current events, enterprise architecture, sovereign questions, books, applications, and lived practice can sit in one frame, because they are all part of the same historical turn. A site that stays current should do more than report what Popes, CIOs, and model labs are saying. It should also show what one careful practitioner has actually been building in response.

-----

## What it amounts to

The most useful thing I can offer about AI right now is not a prediction. It is a description. Over the last eighteen months, with these systems as collaborators rather than substitutes, I have been able to build and write at a pace, breadth, and level of integration that would once have demanded a much larger apparatus. That is a genuine gain, and it is also a debt.

*Magnifica Humanitas* closes on the Magnificat, on a vision of history seen from below, through the eyes of those most exposed to whatever we build. Read that way, the encyclical's question is not whether AI is good or bad. It is whether AI stays answerable to the human person. The only respectable response to that question is not to nod along. It is to build accordingly: tools with seams, workflows with accountability, writing with ownership, and systems that extend the reach of judgment without dissolving it. Technology is never neutral. It takes on the character of whoever shapes it. The quality of our tools will be measured not only by what they can do, but by what they make it easier for human beings to remain.
