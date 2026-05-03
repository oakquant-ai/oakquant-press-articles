*A companion to* [Insight Belongs to the Machine. Decisions Belong to the Human.](https://press.oakquant.ai/public/articles/insight-belongs-to-the-machine)

---

## Why this companion exists

In February 2024, Klarna told the world it had replaced seven hundred customer service jobs with an AI assistant built on OpenAI. The press release said the bot was handling 2.3 million conversations a month, roughly seventy-six thousand a day, doing the work of 700 full-time agents. The number circulated everywhere. Investor decks referenced it. The story was clean: AI works, savings are real, the future is here.

In May 2025, Klarna's CEO Sebastian Siemiatkowski walked it back. Customer satisfaction had dropped on complex interactions. The bot could process volume but couldn't handle the conversations that actually mattered to keeping a customer. *Cost, unfortunately, seems to have been a too predominant evaluation factor when organising this. What you end up having is lower quality.* The company began rehiring human agents. By early 2026, the reversal was effectively complete. The Klarna story became the canonical 2026 cautionary tale, the one executives now have to explicitly explain how their plan avoids.

Across the Atlantic, in the same period, a different kind of story was running in parallel. JPMorgan's Contract Intelligence platform, known internally as COiN, had been quietly reviewing roughly twelve thousand commercial credit agreements a year since 2017. The work that previously took the bank's lawyers approximately 360,000 hours annually now took seconds, with an error rate lower than human reviewers achieved. To anchor the productivity claim: 360,000 hours is roughly 175 lawyers working full-time for a year, the equivalent of eliminating a mid-sized in-house legal department from the workflow without firing anyone. By 2026, COiN had become invisible in the way good infrastructure becomes invisible. No press release. No reversal. No 700-agent claim. Just a system that worked because it had been built on the right architecture for what it had to do.

These two stories are representative bookends to the success of agentic AI strategies in 2026. One company tried to substitute AI for human judgment in interactions that required human judgment, measured the wrong things on the wrong timeline, and was forced to quietly unwind the experiment. Another company assigned a probabilistic system to a bounded, repeatable, structurally legible task, governed it carefully, and let it scale. The architectural difference between the two is what the analyst community has spent the last eighteen months examining and trying to understand what can make the successes repeatable and the failures avoidable. They have devised different vocabularies. Gartner's *agentic governance* and *decision intelligence*, Forrester's *blended AI and CSR experiences*, McKinsey's *agentic mesh*, BCG's *combining predictive, generative, and agentic AI*. They share a very similar underlying pattern. Cognition belongs to the agent. Coordination belongs to the workflow. Statistical prediction sits between them. Each layer needs its own governance.

This companion piece is for the reader who wants to understand where the main article sits in the broader analyst landscape, and where the analyst community thinks the next twenty-four months are heading. I have organized it around six questions. Each one starts with a story, then brings in the analyst data, because the data only makes sense once you have seen what it describes.

---

## 1. How big is the gap between agentic AI ambition and agentic AI reality?

Klarna is the loud version on the extreme end of the spectrum. The quiet version is told in the countless stories that sit somewhere in the middle, the stories that fill the spectrum between Klarna and COiN.

A March 2026 survey of 650 enterprise technology leaders across financial services, manufacturing, healthcare, retail, and professional services painted the picture in numbers. Seventy-eight percent had at least one AI agent pilot running.

Of those that tried, eighteen percent had successfully scaled an agent to organization-wide operational use. The remaining eighty-two percent were stuck somewhere in between. A working demo, a controlled pilot, a planned rollout that kept getting deferred. Another way to look at these numbers is that only fourteen percent of the 650 enterprises had successfully implemented an agent.

The senior partner of one of the major consultancies described the pattern privately as *the second half problem* (only fourteen percent passed this test). The first half is easy (seventy-eight percent passed this test). You connect a model to a few systems. You run it in a sandbox. The demo works. Leadership applauds. Then you try to give it real data, real volume, real edge cases, real regulatory scrutiny, and the system breaks in ways the demo did not.

The five most common breaking points in the survey were, in order:

1. integration complexity with legacy systems
2. inconsistent output quality at volume
3. absence of monitoring tooling
4. unclear organizational ownership
5. insufficient domain training data

These are not technology problems. They are operational and architectural problems. They are what Klarna ran into eighteen months earlier, in the customer service version. They are what every enterprise that has tried to scale a pilot in 2025 and 2026 has run into in some form.

The analyst community has been writing about this gap for two years and the applicability of these five observations has stayed remarkably consistent.

### Gartner: the steepest enterprise adoption curve being measured anywhere

Gartner's 2026 CIO and Technology Executive Survey found that only seventeen percent of organizations have deployed AI agents to date, while more than sixty percent expect to do so within the next two years. *CIO* refers to the senior executive responsible for the technology systems an enterprise runs on, the role that owns the budget and the accountability for whether agentic AI deployment actually works.

Roughly one in six enterprises has deployed today. Roughly six in ten plan to deploy in the next twenty-four months. The gap between those two figures is the steepest enterprise adoption curve being measured anywhere in the technology stack, more aggressive than the comparable curves for cloud, mobile, or SaaS adoption at their respective inflection points.

Gartner places agentic AI at the Peak of Inflated Expectations on its 2026 Hype Cycle. The *Hype Cycle* is a Gartner framework that traces an emerging technology across five stages from Innovation Trigger through Peak of Inflated Expectations, Trough of Disillusionment, Slope of Enlightenment, and Plateau of Productivity. The Peak position indicates extraordinary market attention running ahead of operational maturity.

Translation: the bot demos are at their most enthusiastic moment in the cycle. The bills haven't come due yet.

### McKinsey: six in ten stuck, one in ten at scale

McKinsey's State of AI survey from late 2025 gives the picture from a different angle. Sixty-two percent of organizations reported experimenting with or piloting AI agents. In any given business function, no more than ten percent of respondents said their organizations had reached scale.

Six in ten organizations are stuck in pilot. One in ten has reached scale. The remaining gap is the entire competitive opportunity for the next twenty-four months.

### BCG: workers see the future but cannot use it

BCG's December 2025 research adds the human dimension. Three in four employees believe AI agents will matter for future success. Only thirteen percent say their companies have broadly integrated them into workflows. Only one-third say they understand how the agents work.

Most workers are operating in environments where the agentic future is real to them as an idea but invisible to them as a working tool.

### Deloitte: only one in five has the governance to operate

Deloitte's 2026 State of AI in the Enterprise report, surveying more than 3,200 business and IT leaders, contributes the most quotable governance number in the landscape: only twenty-one percent of companies have a mature governance model for autonomous agents.

### What the four numbers say together

That is a lot of statistics in four subsections, and the numbers tell a story that is not always intuitive to see when they sit next to each other. They align more closely than they appear.

Roughly two-thirds to three-quarters of enterprises have a pilot running. Somewhere between ten and twenty-one percent have actually deployed at scale or built the governance to operate the deployment safely, depending on which dimension the survey is measuring.

The honest math, taken across the four major analysts, is that *between one in ten and one in five enterprises have crossed the line from pilot to scaled production with the discipline to keep the deployment running*.

Stated in the language of a board meeting: *who would sign up for a project with a one-in-ten-to-one-in-five chance of reaching scale?* Two-thirds to three-quarters of large enterprises, that's who, because they have judged the risk of not trying to be larger than the risk of trying and stalling. The competitive imperative is real. The execution discipline that turns the pilot into the production deployment is not yet broadly built.

What the data also says, more quietly, is that the sixty-or-seventy percent in the middle are not Klarnas. They are not loud failures. They are silent stalls. Their pilots worked in the sandbox. They never got to find out whether the theory would survive contact with production data, real volume, real edge cases, real audit pressure.

Everyone in that middle wants to be the next COiN, the system that runs invisibly and saves 360,000 hours a year. Almost everyone in that middle is terrified of becoming the next Klarna, the system that ran loudly and had to be quietly rebuilt.

That fear is rational. The discipline that distinguishes the COiN outcome from the Klarna outcome is the architectural and governance work the analyst data is describing in aggregate and the main article is describing in detail.

---

## 2. Has the three-probabilism thesis become analyst consensus?

Air Canada had a chatbot. In November 2022, a passenger named Jake Moffatt asked the airline's customer service bot about bereavement fares for a last-minute flight to attend his grandmother's funeral. The bot told him he could book the flight and apply for a bereavement discount retroactively within ninety days. Moffatt did exactly that. Air Canada refused the refund, on the grounds that its actual published policy required bereavement fares to be applied for *before* travel, not after.

Moffatt sued. Air Canada's defense was novel: the airline argued that the chatbot was *a separate legal entity that is responsible for its own actions*. The British Columbia Civil Resolution Tribunal disagreed. In February 2024, it ruled that Air Canada was responsible for everything on its website, including what the chatbot said. The airline paid the difference plus tribunal fees. The case is now cited in nearly every enterprise legal training on AI deployment.

The architectural lesson is simple but worth being explicit about. The chatbot was a generative system answering customer questions in natural language. The bereavement fare policy was a deterministic rule maintained by the airline. The two were never properly composed. The bot had a probabilistic plausible answer. The airline had a non-negotiable policy. There was no governance layer between them deciding which answer was authoritative. When the two diverged, the customer trusted the bot, the bot was wrong, and the tribunal made the airline pay for the divergence.

That divergence between what generative cognition produces and what the deterministic policy actually says is one face of the three-probabilism question. Generative cognition is one mode of probabilism. Statistical prediction (a credit risk model, a fraud score, a churn classifier) is another. Workflow coordination is the third mode that is not probabilistic at all but governs whether the other two combine into something an enterprise can actually run. The argument the main article develops at length is that all three need to be present, distinct, and properly governed for an agentic system to be production-credible.

In 2024, this was contrarian. In 2026, it is recognizable across multiple analyst frames.

### BCG: combine predictive, generative, and agentic AI

The cleanest articulation comes from BCG's *How Agents Are Accelerating the Next Wave of AI Value Creation*, published December 2025. BCG's prescriptive guidance to CEOs explicitly names the composition: *combine predictive, generative, and agentic AI for impact.* *CEO* refers to the senior executive accountable to the board and the shareholders for the entire performance of the enterprise, the role at which strategic technology decisions stop being delegated and start being owned.

The framing assumes the three capabilities are distinct and complementary, which is precisely the architecture the main article develops in its seventh part.

### Gartner: composition is the core architectural move

Gartner published the inaugural Magic Quadrant for Decision Intelligence Platforms in January 2026, defining the category as *software to create decision-centric solutions that support, augment and automate decision making of humans or machines, powered by the composition of data, analytics, knowledge and AI.*

Two structural observations matter. First, the category is named *decision intelligence*, not *AI*. Gartner is signaling that the sophistication of the system is in how decisions are composed, not in how impressive any single model is. Second, the definition explicitly invokes *composition*, which is the core architectural move.

Air Canada had components. It did not have composition.

### Forrester: tightly blended AI and CSR experiences

Forrester, in its 2026 Customer Service Solutions Wave, is more pointed. Principal Analyst Kate Leggett's recommendation to enterprises: *Look for vendors that offer tightly blended AI and CSR experiences and measurement and optimization frameworks for AI.* *CSR* refers to the *customer service representative*, the human agent who handles inquiries the automated system cannot resolve, with the architectural question being how the AI agent and the human agent share work.

The phrase *tightly blended* is doing a lot of work in that sentence. Forrester is naming the same brain-and-rails composition the main article describes, in vocabulary calibrated for customer service buyers.

Klarna is what *not tightly blended* looked like. The model handled volume; the company assumed it had handled service. Volume and service are different things, governed by different rules, and that is what *blended* means in operational practice.

### The argument has not changed. The reception has.

The convergence matters because it changes what a credible architectural conversation sounds like. In 2024, an architect proposing a three-layer composition with distinct governance regimes for each layer was making a contrarian claim. In 2026, the same architect is making a claim that aligns with the explicit guidance of all three major industry analysts. The argument has not changed. The reception has.

---

## 3. Has the analyst community recognized agentic governance as a distinct category?

In 2023, McDonald's was running an AI-powered drive-thru pilot in partnership with IBM at more than a hundred US restaurants. The system was supposed to take orders by voice and pass them to the kitchen with fewer errors than a human cashier. By June 2024, McDonald's discontinued the program. Customer videos had gone viral showing the AI adding hundreds of chicken nuggets to a single order, misinterpreting requests, and assigning items the customer had explicitly declined. The technology worked in the demo. It did not survive contact with how people actually order food in cars at midnight. Beyond the operational failure, there was a governance failure: nobody had defined what the system was allowed to do unsupervised, what triggered escalation, or how the brand experience was protected when it failed. The agent was not properly governed.

Eighteen months later, the analyst community has formally recognized this as a distinct category problem.

### Gartner: governance designed in alongside the agents, not retrofitted afterward

Gartner's 2026 Hype Cycle for Agentic AI is the most consequential signal. The Hype Cycle places *agentic AI governance*, *agentic AI security*, and *FinOps for agentic AI* as distinct profiles alongside core agentic AI technologies.

*FinOps* is the cloud cost management discipline that emerged when enterprises started running production workloads on metered cloud infrastructure and discovered that engineering decisions were directly driving operating expense, now extending to the metered consumption of AI inference.

Gartner's framing: these supporting profiles indicate *rising enterprise concern about accountability, control and economic sustainability as agentic systems become more autonomous and interconnected*. Their placement on the curve *highlights that the need for oversight and discipline is becoming evident early in the adoption cycle, not only after large-scale deployment*.

That last sentence is worth re-reading. The 2024 narrative around AI governance was that governance follows deployment, first the pilots, then the audit. The 2026 narrative, per Gartner, is that governance must be designed in alongside the agents, not retrofitted afterward.

McDonald's is what retrofitting looks like. By the time the chicken nugget videos were on social media, the governance question was downstream of a brand crisis. The 2026 architectural argument is that the governance question has to be upstream of the deployment, not downstream of the embarrassment.

### Forrester: AI governance is now its own vendor category

Forrester's AI Governance Wave Q3 2025 named **Credo AI** a Leader. The naming formally established the AI governance platform as a recognized vendor category alongside the workflow platforms and the agent frameworks.

Credo AI's positioning, which covers registry, policy enforcement, evidence aggregation, and audit-ready documentation, is structurally adjacent to but distinct from the workflow layer.

The architectural implication is that a sophisticated 2026 enterprise AI program now has at least three distinct technology layers: the agent framework, the workflow platform, and the AI governance platform. Each governs different things. Each is a separate procurement decision. Each is a separate competency to build.

### COiN: what designed-in alongside actually looks like

JPMorgan's COiN is, in this light, an instructive case in what early-baked governance looks like. The platform was scoped narrowly from the start: extracting structured meaning from a specific class of legal documents, classifying clauses into roughly 150 attributes, with human lawyers reviewing the outputs at launch and gradually being moved to higher-judgment work as the error rate proved durably low.

The governance was not a layer added after the model worked. It was the constraint that defined what the model was allowed to do in the first place. That is what *designed in alongside* means in operational practice.

### Gartner: five trends in agentic governance enforcement

Gartner's 2026 Magic Quadrant for Data and Analytics Governance Platforms identifies five trends that compound the picture:

1. **Agentic governance enforcement.** Governance shifting from AI-assisted recommendations to fully automated, agent-driven policy enforcement.
2. **Trust models over control models.**
3. **AI governance convergence.** Data and analytics governance platforms becoming the primary foundation for operationalizing AI governance.
4. **Horizontal market consolidation.**
5. **Ecosystem-led innovation.**

The fourth, horizontal market consolidation, is what enterprise architects increasingly mean when they talk about *titan convergence*. The market is consolidating siloed governance tools into unified platforms, and the platforms that win that consolidation will define how enterprise AI operates for the rest of the decade.

### Where the main article touches this layer, and where it does not

The main article touches the AI governance platform layer in Part VI but does not develop it as a distinct category. The reason for the lighter touch is that the architecture stands on its own without it; the reason it deserves a fuller treatment in future work is that the analyst data, and the McDonald's-and-Klarna-and-Air-Canada landscape it summarizes, now positions it as a first-class concern.

A useful way to read the analyst signal in plain terms: in 2024, fewer than one in three enterprises had a deliberate governance model for AI agents in place at the design phase. In 2026, the analyst consensus is that running an agentic system without one is reckless, and the Deloitte twenty-one percent number is the count of enterprises that have actually built the discipline.

The remaining seventy-nine percent are operating with governance retrofitted onto deployments that already exist or that are already being scoped without governance in the room. The McDonald's drive-thru pilot, the Air Canada chatbot, the Klarna customer service rollout. These are not exotic edge cases. They are the modal failure pattern when governance arrives downstream of deployment.

COiN is the modal success pattern when governance arrives upstream. Most enterprises in 2026 know they want the second pattern. Most are still operating in the first.

---

## 4. What is happening at the platform layer, and what does *titan convergence* actually mean?

A regulated bank in early 2026 commissions an architectural review of its agentic AI roadmap from one of the major consultancies. The consultancy partner walks in, opens a deck, and proposes a reference architecture organized around OpenAI's Frontier platform with Gemini Enterprise as the secondary stack. The reference architecture is technically reasonable. The partner is professional and persuasive. The bank's chief architect goes home that night and looks up something he had vaguely registered in the trade press a few weeks earlier. The consultancy he just briefed is one of four firms that publicly entered OpenAI's Frontier Alliance in February 2026, with dedicated practice groups, certified teams, and OpenAI's own forward-deployed engineers embedded in client engagements. The same consultancy is also a partner in Google's Gemini Enterprise Acceleration Program, with a $750 million partner ecosystem fund and dedicated practice expansions on the Google side as well.

The chief architect is not naïve. He has worked with consultancies for twenty years. But the structural question he is now sitting with is new. *Would the architecture I just heard pitched be different if my advisor were not commercially aligned with the platforms inside it?* That question was not on the table in 2024. In 2026, it is the most consequential question an enterprise architect can ask before committing to an agentic AI roadmap.

The clearest signal of titan convergence is structural rather than technological. In the first half of 2026, every major foundation model lab has formed direct partnerships with the major management consultancies, and the partnerships are commercial, not advisory.

OpenAI announced Frontier Alliances in February 2026 with Boston Consulting Group, McKinsey & Company, Accenture, and Capgemini. The terms include dedicated practice groups inside each consultancy, certified teams trained on OpenAI technology, and OpenAI's own forward-deployed engineers working alongside the consultancy teams in client engagements. OpenAI describes its Frontier platform as *a semantic layer for the enterprise, a unified platform that lets AI agents navigate business software, execute workflows, and make decisions across an organization's entire technology stack*. The framing is significant: OpenAI is no longer positioning as a model provider. It is positioning as a workflow-and-orchestration platform, with the consultancies as its distribution channel.

Google announced a $750 million partner ecosystem fund at Cloud Next 2026, with Accenture, BCG, Deloitte, and McKinsey receiving early access to Gemini models, dedicated practice expansions, and embedded Google forward-deployed engineers. Deloitte specifically is forming a dedicated Google Cloud Agentic Transformation practice and rolling out Gemini Enterprise to more than 100,000 of its own teams. McKinsey is launching the McKinsey Google Transformation Group. The structural pattern is identical to the OpenAI move: foundation model vendor, distributed through consultancy, embedded with forward-deployed engineering.

Anthropic has formal partnerships with Accenture, Deloitte, PwC, and other major system integrators on similar lines.

The implication for enterprise architects is uncomfortable but worth naming directly. The major consultancies are no longer neutral advisors on agentic AI architecture in 2026. They have commercial commitments to specific platform stacks. McKinsey is simultaneously inside OpenAI's Frontier Alliance and Google's Gemini Enterprise Acceleration Program, which means architectural advice arriving from McKinsey now carries platform-specific incentives the client may not see at the start of the engagement.

This does not make the consultancies untrustworthy. It makes their counsel platform-conditioned, in ways that the architectural conversation has not yet fully absorbed. A regulated enterprise commissioning agentic AI architecture work in 2026 should ask, first, *which platforms is this advisor commercially aligned with*, and second, *would this advice change if those alignments were different.* Both questions are reasonable. Neither was on the table in 2024.

The convergence is also visible at the equity markets. Investors in early 2026 have been reported as punishing the share prices of traditional SaaS vendors over concerns that customers will choose foundation-model-vendor agentic platforms instead, or that AI coding tools will eliminate the need for the underlying SaaS products entirely. *SaaS*, or *software as a service*, is the business model under which enterprise software is delivered as a hosted subscription rather than as installed product, the model that has dominated enterprise software since roughly 2010. The thesis the markets are testing is that the orchestration layer, not the application layer, is where enterprise AI value will accrue. Whether the thesis is right is a separate question; the fact that it is being tested at all is the structural change.

The structural change has a downstream consequence the bank architect at the start of this section is already living. Two-thirds to three-quarters of large enterprises are commissioning agentic AI roadmaps right now, and almost all of them are doing so in conversation with one of the four or five major consultancies that have commercial alignments with the foundation model vendors whose platforms the roadmaps recommend. The architectural advice is good. The architectural advice is also platform-conditioned in ways that were not on the table eighteen months ago. The architects who walked into 2024 trusting their consultancy were not wrong then. The architects who walk into 2026 with the same trust are operating with one fewer instrument than the conversation requires. *Which platforms is this advisor commercially aligned with* is the question that separates an architect who will look back on the 2026 commitment with confidence in 2028 from one who will be quietly unwinding it the way Klarna unwound the 700-agent press release.

---

## 5. What is the strategic move available to incumbent platform vendors, and which ones are making it?

The equity-market punishment of incumbent SaaS vendors in late 2025 and early 2026 is a story being told in dollar terms. Salesforce stock down roughly thirty-two percent year-to-date through April 2026. ServiceNow off twenty-three percent on Claude Cowork's launch day alone. Adobe down twenty-seven percent. The iShares Expanded Tech-Software Sector ETF off more than twenty percent year-to-date by February. To anchor what the percentages mean: Salesforce's market capitalization shed roughly $90 billion across those four months, an amount larger than the entire combined market cap of the next-tier workflow vendors below it in the SaaS leaderboard. The market is not punishing one vendor. It is repricing an entire stack. The numbers are big enough that they deserve attention on their own. The more interesting question is what the analyst community thinks the incumbent vendors should be doing about it, and which of them are doing it credibly versus which are signaling defensiveness without yet executing on the strategy.

The analyst guidance, taken across the major frames the previous sections have referenced, points to a single durable position. Gartner's Data and Analytics Governance Magic Quadrant identifies *agentic governance enforcement* as a defining 2026 trend, with governance shifting from AI-assisted recommendations to fully automated, agent-driven policy enforcement, and the workflow and governance layer becoming the primary foundation for operationalizing the agentic deployment. Forrester's Customer Service Solutions Wave recommends *tightly blended AI and CSR experiences and measurement and optimization frameworks for AI*. The *blended* requirement explicitly places the orchestration responsibility above the model layer. McKinsey's *agentic mesh* framing names the same architectural composition from a different angle. BCG prescribes *combining predictive, generative, and agentic AI* with the explicit assumption that the three are distinct and that the composition is itself a discipline. Across vocabularies, the analyst consensus is consistent: the defensible position for an incumbent platform vendor with a regulated-enterprise customer base is the workflow, decisioning, and governance layer that the foundation model layer is structurally not built to be.

The strategic implication for an incumbent vendor watching its share price get punished is that the response is not to compete with OpenAI or Anthropic on the agent layer. That is a fight the foundation labs win on capability, on capital, and on go-to-market scale. The response is to make the workflow, governance, and orchestration layer so visibly necessary in regulated production deployment that no foundation lab engagement in a regulated industry can land safely without it. The case studies the analyst community is citing, including Klarna's reversal, Air Canada's tribunal ruling, McDonald's drive-thru retreat, and JPMorgan's COiN succeeding for nearly a decade because it was governed at the design phase, are all variations of the same underlying observation. Probabilistic systems need deterministic governance. Agentic AI in regulated environments needs a workflow layer that can answer to a regulator. The incumbent vendors that own the workflow layer can still own the answer to that question. The vendors that try to also be the agent layer will not.

The strategic moves visible in the public 2026 record vary in how cleanly they execute the analyst-consensus positioning. Salesforce's Agentforce ramp to roughly $800 million in annual recurring revenue by April 2026, paired with Marc Benioff's contrarian one-thousand-graduate hire and the Headless 360 framing, is one variant. A workflow-and-data-layer reassertion that does not yet seem to be fully landing with the equity market despite its operational traction. ServiceNow's AI Control Tower positioning emphasizes the orchestration-and-governance layer explicitly. Microsoft's Copilot Studio and Foundry positioning is structurally different because Microsoft owns parts of multiple layers simultaneously through its Azure-OpenAI relationship. Oracle's positioning is anchored to its database-and-application-suite footprint with agentic features layered above. SAP under Christian Klein has emphasized regulated-enterprise governance and EU AI Act compliance in ways that are coherent with the analyst-consensus positioning but slower in execution cadence than the US peers. Workday has staked a vertical position in HCM and finance. Pega's Agentic Process Fabric positioning explicitly names the workflow-and-decisioning-and-governance layer composition the analyst frames are converging on. The list is illustrative, not exhaustive, and the proportional treatment matters. None of these vendors has yet demonstrated unambiguously that the analyst-consensus positioning will translate into equity-value recognition on the timeline the equity markets are operating against.

The structural observation that holds across the named examples is that the incumbents pursuing the workflow-and-governance positioning credibly are doing four things in some combination. They are publishing reference architectures that explicitly name how their platform composes with multiple foundation models rather than competing with any single one. They are emphasizing regulatory durability across banking, insurance, healthcare, government, and telecom, as the dimension on which their position strengthens as agentic AI moves from pilot to production. They are building out governance, audit, identity, and policy enforcement capabilities at the orchestration layer, often in partnership with dedicated AI governance vendors like Credo AI rather than attempting to build everything in-house. And they are framing the conversation with consultancies and clients around *the workflow and governance layer your foundation lab engagement needs to compose with* rather than around *the workflow platform that competes with your foundation lab engagement*.

The incumbent vendors that do not pursue this positioning have a more difficult two-year window. The analyst consensus is not predicting that any particular incumbent will fail. The analyst consensus is observing that the incumbents that do not credibly own a layer the foundation labs structurally cannot own will face accelerating margin compression as their application-layer functionality is increasingly substitutable by agentic systems built on top of the foundation models their customers are deploying anyway. *Punished* is the right word for the equity-market reaction. *Repositioned* is the word for what the analyst community is observing among the vendors moving fastest. The two outcomes are not yet decided for any of the named incumbents, but the strategic move that separates them is visible.

The bank architect from the previous section is the practical reader of this observation. Her question, *which platforms is this advisor commercially aligned with*, is one part of the diligence. The companion question is *which incumbent vendors in my existing stack are credibly executing the workflow-and-governance positioning, and which are signaling it without execution*. The vendors that are credibly executing are the ones whose 2026 commitments will look defensible in 2028. The vendors that are signaling without execution are the ones whose share prices are leading indicators of an architectural problem that the equity market is pricing faster than the vendors are repositioning.

---

## 6. What does the analyst community think the next twenty-four months will look like?

Imagine two enterprises sitting at the same starting point in early 2026. Both are in regulated industries. Both have similar revenue, similar headcount, similar technology debt. Both have done the obligatory pilots: a customer service bot, an internal knowledge agent, a code generation assistant. Both want to scale. Both have boards asking when the AI line item is going to start producing the productivity numbers the analysts have been promising.

Enterprise A treats 2026 as a year of experimentation. It runs more pilots. It rotates through three foundation model vendors, looking for the best demo. It commissions a strategic review from the consultancy with the strongest existing relationship and accepts the recommended platform stack without asking which commercial alignments the consultancy is operating under. It has not built a governance platform layer because it is *too early to commit*. It has not built TCO instrumentation because the inference costs *are still trending down*. It has not assigned dedicated AI operations ownership because the function *will get clearer once the right platform is chosen*.

Enterprise B treats 2026 as a year of decision. It picks a primary stack with explicit awareness of the commercial alignments inside the recommendation. It builds the governance platform layer in parallel with the agent framework. It instruments TCO from the first pilot, on the assumption that inference costs will compound faster than budgets will grow. It assigns a senior leader, not a committee, operational ownership of agentic deployments, with clear gates between sandbox, pilot, departmental rollout, and enterprise scale.

By the end of 2027, the productivity gap between these two enterprises will be visible in their earnings calls. The analyst community is not predicting which enterprise will win. It is predicting that the gap between them will be larger than it has ever been between enterprises operating with comparable resources.

Three forecasts are worth grounding in the analyst data.

**Adoption will accelerate, but scaling will lag.** Gartner predicts that forty percent of enterprise applications will be integrated with task-specific AI agents by the end of 2026, up from less than five percent in 2025. That is an eight-fold expansion in twelve months, with most of the integration happening in enterprises that have not yet built the operational discipline to run the integrations safely. By 2027, one-third of agentic AI implementations will combine agents with different skills to manage complex tasks within application and data environments. By 2028, Gartner projects that one-third of user experiences will shift from native applications to agentic front ends. The aggregate picture is one of rapid surface-level adoption alongside slow operational maturity, which is the same gap McKinsey, BCG, and Deloitte describe in their respective surveys. The expansion is real. The scaling discipline is not yet built. Enterprise A and Enterprise B will both have the agents. Only one of them will have the discipline to operate them at scale.

**Spending will rebalance, not just grow.** BCG's analysis of agentic AI's cost implications suggests that enterprise technology spending will shift from labor toward technology in proportions that change the operational economics. BCG's example: in retail banking, technology may shift from twenty-to-thirty percent of operating cost today to thirty-five-to-forty-five percent within the next several years, not as a budget increase but as a rebalanced cost mix. McKinsey's parallel finding is that AI workloads will drive infrastructure costs to two-to-three times their 2025 levels by 2030 while budgets remain flat, creating direct pressure on architects to reduce per-transaction inference cost without sacrificing audit quality. The TCO discipline the main article advocates is not a vendor-selection nicety in this environment. *TCO* means *total cost of ownership*, the full cost of running a system across its life including infrastructure, inference, integration, governance, and the human labor required to keep it operating safely. It is a budget-survival discipline. Klarna's reversal cost real money that was not in the original business case. The cost of unwinding a deployment is part of the TCO of building one.

**The window for unconscious lock-in is closing.** Multiple analyst frames now warn that enterprises without an explicit agentic AI architecture strategy are making a default lock-in decision, usually toward whichever vendor has the strongest commercial relationship with their existing systems integrator. Gartner's recommendation in the Data and Analytics Governance Magic Quadrant is to *use the Magic Quadrant as a starting point, not a shortlist*. The *Magic Quadrant* is Gartner's framework for ranking vendors in a given market across the axes of *ability to execute* and *completeness of vision*. The deeper recommendation, implicit across the analyst commentary, is that architectural commitments made in 2026 will define operating shape for several years. Architects who treat 2026 as a year of decision will be better positioned in 2028 than architects who treat it as a year of experimentation.

The leadership thinning the analyst community is signaling, without using the phrase, is specific. The thinning is not at the level of vendor count. There are more agentic AI vendors than ever. The thinning is at the level of *enterprises that have built the architectural and governance discipline to scale*. That cohort, per the cross-analyst data, sits between ten and twenty-one percent depending on the survey. The remaining seventy-nine to ninety percent are the contested ground for the next twenty-four months. Most of them will choose by default. A smaller group will choose by architecture.

---

## What this means for the architectural argument

The main article makes a specific and committed architectural claim: cognition belongs to the agent, coordination belongs to the workflow, and statistical prediction sits between them, each with its own governance regime, each in its proper place in the composition.

The analyst data summarized here suggests that this claim is now, in 2026, the consensus architecture for credible enterprise AI deployment. Different analysts describe it in different vocabularies. Gartner names *agentic governance* and *decision intelligence*. Forrester names *blended AI and CSR experiences*. McKinsey names *the agentic mesh*. BCG prescribes *combining predictive, generative, and agentic AI*. The vocabulary varies. The architecture does not.

The case studies tell the same story in less abstract terms. Klarna replaced human judgment with a probabilistic system on tasks that required judgment, and the reversal is now public record. Air Canada deployed a generative system without a governance layer between it and the policies it was answering for, and a tribunal made the airline pay. McDonald's pushed an agent into production without defining what it was allowed to do unsupervised, and the brand paid the price in viral video. JPMorgan scoped a probabilistic system narrowly, governed it carefully from the start, and let it scale to the point that 360,000 hours of human work a year disappeared without a press release.

What remains genuinely contested in 2026 is not *whether the three-layer composition is right*. It is *how to operationalize it under regulatory load, at the scale a regulated enterprise actually runs, with the governance discipline an examiner will eventually inspect.* That operational question is the subject of the main article. This companion is, I hope, the analyst-grounded context that lets a careful reader take the main article seriously without mistaking its argument for an idiosyncratic view.

The architecture is right. The market knows it is right. The hard part, the part that separates the leadership ten-to-twenty percent from the experimenting majority, is the discipline to build it.

---

*Pumulo Sikaneta*

*This companion piece supplements* [Insight Belongs to the Machine. Decisions Belong to the Human.](https://press.oakquant.ai/public/articles/insight-belongs-to-the-machine)

*Analyst data referenced from: Gartner 2026 Hype Cycle for Agentic AI; Gartner 2026 Magic Quadrant for Decision Intelligence Platforms; Gartner 2026 Magic Quadrant for Data & Analytics Governance Platforms; Gartner 2026 Magic Quadrant for Customer Service Solutions; Gartner 2026 Magic Quadrant for Integration Platform as a Service; Forrester Wave™: Customer Service Solutions Q1 2026; Forrester Wave™: AI Governance Q3 2025; Forrester Wave™: AI Infrastructure Solutions Q4 2025; McKinsey* Reimagining tech infrastructure for agentic AI *(2026); McKinsey* The State of AI in 2025: Agents, innovation, and transformation; *BCG* How Agents Are Accelerating the Next Wave of AI Value Creation *(December 2025); Deloitte 2026 State of AI in the Enterprise. Vendor and partnership announcements referenced from public press releases by Google, OpenAI, and Anthropic during Q1–Q2 2026. Enterprise case studies grounded in published reporting on Klarna's AI rollout and reversal (2024-2026), the Moffatt v. Air Canada decision of the British Columbia Civil Resolution Tribunal (February 2024), the McDonald's-IBM AI drive-thru pilot and its 2024 discontinuation, and JPMorgan Chase's COiN platform deployment (2017-present). The 650-leader pilot-to-production survey is drawn from publicly reported March 2026 enterprise technology research.*
