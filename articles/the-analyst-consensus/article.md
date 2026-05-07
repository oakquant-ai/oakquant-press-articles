![An analyst's working desk in late afternoon light, with stacked printed reports, a coffee cup, reading glasses, and an open laptop in soft focus.](images/IMG_0015.jpeg)

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

Gartner's 2026 CIO and Technology Executive Survey found that only seventeen percent of organizations have deployed AI agents to date, while more than sixty percent expect to do so within the next two years. The survey was conducted with the role that owns the budget and the accountability for whether agentic AI deployment actually works. This is what gives the observation its credibility. This group is uniquely positioned to plan and set the direction we are assuming in the article.

Roughly one in six enterprises has deployed today. Roughly six in ten plan to deploy in the next twenty-four months. The gap between those two figures is the steepest enterprise adoption curve being measured anywhere in the technology stack, more aggressive than the comparable curves for cloud, mobile, or SaaS adoption at their respective inflection points.

Gartner places agentic AI at the Peak of Inflated Expectations on its 2026 Hype Cycle. The *Hype Cycle* is a Gartner framework that traces an emerging technology across five stages:

1. Innovation Trigger
2. **Peak of Inflated Expectations *(we are here in 2026)***
3. Trough of Disillusionment
4. Slope of Enlightenment
5. Plateau of Productivity

The Peak position indicates extraordinary market attention running ahead of operational maturity.

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

![A diagram showing two probabilistic components, generative cognition and statistical prediction, sitting above a deterministic workflow coordination substrate that contains four named capability regions: governance and policy, case state and durability, audit and compliance, and composition and orchestration.](images/fig-three-probabilism-composition.svg)

In 2024, this was contrarian. In 2026, it is recognizable across multiple analyst frames.

### BCG: combine predictive, generative, and agentic AI

The cleanest articulation comes from BCG's *How Agents Are Accelerating the Next Wave of AI Value Creation*, published December 2025. BCG's prescriptive guidance to CEOs explicitly names the composition: *combine predictive, generative, and agentic AI for impact.* This advice to the role at which strategic technology decisions stop being delegated and start being owned is evidence of how strongly BCG feels about this vision.

The framing assumes the three capabilities are distinct and complementary, which is precisely the architecture the main article develops in its seventh part.

### Gartner: composition is the core architectural move

Gartner published the inaugural Magic Quadrant for Decision Intelligence Platforms in January 2026, defining the category as *software to create decision-centric solutions that support, augment and automate decision making of humans or machines, powered by the composition of data, analytics, knowledge and AI.*

Two structural observations matter. First, the category is named *decision intelligence*, not *AI*. Gartner is signaling that the sophistication of the system is in how decisions are composed, not in how impressive any single model is. Second, the definition explicitly invokes *composition*, which is the core architectural move.

Air Canada had components. It did not have composition.

### Forrester: tightly blended AI and CSR experiences

Forrester, in its 2026 Customer Service Solutions Wave, is more pointed. Principal Analyst Kate Leggett's recommendation to enterprises: *Look for vendors that offer tightly blended AI and CSR experiences and measurement and optimization frameworks for AI.* *CSR* refers to the *customer service representative*, the human agent who handles inquiries the automated system cannot resolve, with the architectural question being how the AI agent and the human agent share work.

The phrase *tightly blended* is the key thing to focus on. Forrester is highlighting the same brain-and-rails composition the main article describes, in vocabulary calibrated for customer service buyers.

Klarna is what happens when an agentic solution is not tightly blended. An application pivoted fully to AI, with the CSR layer eliminated. The model handled volume; the company assumed it had handled service. Volume and service are different things, governed by different rules, and that is what *blended* means in operational practice.

### The argument has not changed. The reception has.

The convergence matters because it changes what a credible architectural conversation sounds like. In 2024, an architect proposing a three-layer composition with distinct governance regimes for each layer was making a contrarian claim. In 2026, the same architect is making a claim that aligns with the explicit guidance of all three major industry analysts. The argument has not changed. The reception has.

---

## 3. Has the analyst community recognized agentic governance as a distinct category?

In 2023, McDonald's was running an AI-powered drive-thru pilot in partnership with IBM at more than a hundred US restaurants. The system was supposed to take orders by voice and pass them to the kitchen with fewer errors than a human cashier. By June 2024, McDonald's discontinued the program. Customer videos had gone viral showing the AI adding hundreds of chicken nuggets to a single order, misinterpreting requests, and assigning items the customer had explicitly declined. The technology worked in the demo. It fell apart when it had to deal with how people actually order food in cars at midnight. Beyond the operational failure, there was a governance failure: nobody had defined what the system was allowed to do unsupervised, what triggered escalation, or how the brand experience was protected when it failed. The agent was not properly governed.

Eighteen months later, the analyst community has formally recognized this as a distinct category problem.

### Gartner: governance designed in alongside the agents, not retrofitted afterward

Gartner's 2026 Hype Cycle for Agentic AI is the most consequential signal. The Hype Cycle places *agentic AI governance*, *agentic AI security*, and *FinOps for agentic AI* as distinct profiles alongside core agentic AI technologies.

*FinOps* is the discipline that emerged a decade ago when companies moved their software systems into the cloud and started paying for computing power by the hour, the gigabyte, and the transaction rather than buying servers up front. Once the bill arrived monthly instead of annually, finance teams discovered something engineers already knew: a single careless design choice could quietly multiply costs by ten or a hundred. FinOps is the practice of managing those choices deliberately. The same discipline is now extending to AI, because every question asked of a model carries a real cost, and those costs add up fast at enterprise scale.

Gartner's framing: these supporting profiles indicate *rising enterprise concern about accountability, control and economic sustainability as agentic systems become more autonomous and interconnected*. Their placement on the curve *highlights that the need for oversight and discipline is becoming evident early in the adoption cycle, not only after large-scale deployment*.

That last sentence is worth re-reading. The 2024 narrative around AI governance was that governance follows deployment, first the pilots, then the audit. The 2026 narrative, per Gartner, is that governance must be designed in alongside the agents, not retrofitted afterward.

McDonald's is what retrofitting looks like. By the time the chicken nugget videos were on social media, the governance question was downstream of a brand crisis. The 2026 architectural argument is that the governance question has to be upstream of the deployment, not downstream of the embarrassment.

### Forrester: AI governance is now its own vendor category

Forrester's AI Governance Wave Q3 2025 formally established AI governance as a recognized vendor category alongside the workflow platforms and the agent frameworks. **Credo AI** was named the Leader of that inaugural Wave. The capabilities Forrester evaluated against define what an AI governance platform is supposed to be:

- *Registry*, a catalog of every AI system the enterprise operates and what it is allowed to do.
- *Policy enforcement*, the mechanism that prevents an AI system from taking actions outside its approved scope.
- *Evidence aggregation*, the collection and storage of records that demonstrate the system did what it was supposed to do.
- *Audit-ready documentation*, the artifacts a regulator or internal auditor can ask for at any moment without the team scrambling to assemble them.

These capabilities sit structurally adjacent to but distinct from the workflow layer, which is why Forrester treats them as their own vendor category.

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

The remaining seventy-nine percent are operating with governance retrofitted onto deployments that already exist or that are already being scoped without governance being adequately addressed. The McDonald's drive-thru pilot, the Air Canada chatbot, the Klarna customer service rollout. These are not exotic edge cases. They are the modal failure pattern when governance arrives downstream of deployment.

COiN is the modal success pattern when governance arrives upstream. Most enterprises in 2026 know they want the second pattern. Most are still operating in the first.

---

## 4. What is happening at the platform layer, and what does *titan convergence* actually mean?

A regulated bank in early 2026 commissions an architectural review of its agentic AI roadmap from one of the major consultancies. The consultancy partner walks in, opens a deck, and proposes a reference architecture organized around OpenAI's Frontier platform with Gemini Enterprise as the secondary stack. The reference architecture is technically reasonable. The partner is professional and persuasive. The bank's chief architect goes home that night and looks up something he had vaguely registered in the trade press a few weeks earlier. The consultancy he just briefed is one of four firms that publicly entered OpenAI's Frontier Alliance in February 2026, with dedicated practice groups, certified teams, and OpenAI's own forward-deployed engineers embedded in client engagements. The same consultancy is also a partner in Google's Gemini Enterprise Acceleration Program, with a $750 million partner ecosystem fund and dedicated practice expansions on the Google side as well.

The chief architect is not naïve. He has worked with consultancies for twenty years. But the structural question he is now forced to address is new. *Would the architecture I just heard pitched be different if my advisor were not commercially aligned with the platforms inside it?* That question was not on the table in 2024. In 2026, it is the most consequential question an enterprise architect can ask before committing to an agentic AI roadmap.

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

The strategic moves visible in the public 2026 record vary in how cleanly they execute the analyst-consensus positioning. Salesforce's Agentforce reached roughly $800 million in annual recurring revenue by April 2026, paired with Marc Benioff's contrarian one-thousand-graduate hire and a workflow-and-data-layer reassertion the equity market is not yet rewarding despite its operational traction. ServiceNow's AI Control Tower positioning emphasizes the orchestration-and-governance layer explicitly. Microsoft's Copilot Studio and Foundry positioning is structurally different from the rest because Microsoft owns parts of multiple layers simultaneously through its Azure-OpenAI relationship. Oracle anchors to its database-and-application-suite footprint with agentic features layered above. SAP under Christian Klein has emphasized regulated-enterprise governance and EU AI Act compliance, which is the right positioning but slower to execute than the US peers. Workday has staked a vertical position in human capital and finance.

The vendor I have the most direct working knowledge of, and the vendor whose strategy I can describe with specificity rather than from public observation alone, is Pega. As the top-of-piece disclosure notes, my professional work is concentrated in the Pega ecosystem, and the description that follows reflects that. Pega's strategy combines three architectural assets that together describe the workflow-and-governance positioning the corpus has been arguing for. Predictable AI is the runtime decisioning capability — predictive scoring, adaptive learning, and decision strategies that have been making operational decisions deterministic and accountable inside Pega applications for years. Blueprint, the AI-powered design environment that has been used at scale since 2024, generates the application structure from natural language. The Claude skill announced for Pega Infinity '26.1, described later in this section, is the larger umbrella that extends design, authoring, configuration, integration, testing, and deployment of Pega applications into Claude itself, so a developer never has to leave the foundation lab UI to build a regulated production application against the Pega runtime.

None of these vendors has yet demonstrated unambiguously that the workflow-and-governance positioning translates into equity-value recognition on the timeline the equity markets are operating against. The market is pricing the dispersion across them faster than any of them is executing.

The structural observation that holds across the named examples is that the incumbents pursuing the workflow-and-governance positioning credibly are clearing three specific barriers that pure agentic frameworks cannot, and they are doing it together rather than separately.

The first is the *invocation barrier*. Until late 2025, the only place a developer could author a workflow application — a Pega case, a Salesforce flow, a ServiceNow process — was inside the workflow vendor's own development environment. The AI assistant could read documentation about the workflow platform, but it could not actually build inside it. The Agent Skills specification that Anthropic published in December 2025, and that OpenAI adopted shortly after for Codex CLI and ChatGPT, changed this. The workflow vendor publishes a skill once, against an open format, and any compliant AI assistant can invoke the workflow platform's authoring capability without the developer leaving Claude or ChatGPT or Cursor. Salesforce's `forcedotcom/afv-library` is the company's officially curated agent skills library, optimized for Salesforce's Agentforce Vibes IDE — announced at Salesforce TDX in April 2026 with Claude Sonnet 4.5 as its default coding model. SAP's developer community has a parallel sap-skills library covering BTP, CAP, Fiori, ABAP, HANA, and Analytics Cloud, community-driven rather than first-party but architecturally on the same substrate. Pega's Infinity '26.1 release, scheduled for detailed disclosure at PegaWorld 2026 in June, is reported to extend this pattern with a Claude skill that lets a Claude user author Pega applications from inside the foundation-lab UI while the runtime, governance, and compliance posture remain in the Pega platform.

The second is the *interaction barrier*. Solving invocation alone is not enough for regulated workflows, because regulated workflows almost always require a human to remain in the loop at the decision moment — an underwriter approving a loan, a claims adjuster authorizing a payout, a customer service representative resolving a dispute. Until late 2025, the only way an AI assistant could solicit that decision was through text dialogue: please confirm the loan amount, please specify the rejection reason, please choose between options A, B, and C, all rendered as conversation. This is the chat wall. It is the UI barrier endemic to human-in-the-loop workflows in regulated industries, and it is exactly the place where Klarna's reversal lived. The customer service representative's value was not just in making the decision; it was in being able to *see* what the decision was about in a structured form the chat interface could not deliver. Google's open-source A2UI protocol, released in December 2025 and at v0.9 by April 2026, lets the AI assistant render an actual approval form, the actual customer record, the actual structured decision surface, at the exact moment the human needs to decide. The agent describes the UI declaratively in JSON; the workflow platform's components render it natively. The chat wall comes down. The workflow vendor's twenty years of accumulated discipline on what an approval screen actually has to show — what fields, what context, what audit trail visible to whom — becomes the asset that makes A2UI useful in regulated industries. A2UI is not the only emerging standard in this space; Anthropic's MCP Apps and OpenAI's Apps SDK are doing related work from different starting points, and the standards picture is currently fragmented. The architectural direction is consistent across all of them.

The third is the *state and governance barrier*, and it is the barrier where the workflow vendors keep their durable advantage no matter how good the foundation labs' UIs and skill formats get. Pure agentic frameworks have been keeping process state at whichever agent is closest to the customer at the moment the state matters. This works for simple agents and falls apart for anything regulated. A loan application is not a single conversation; it is a multi-week process with a credit pull on day one, a verification step on day three, an underwriter review on day seven, an approval committee on day fourteen, and a disbursement on day twenty-one. State has to live somewhere durable across all of that. Audit has to be able to reconstruct who decided what, when, on what evidence. Governance has to be able to enforce that the underwriter actually saw the credit report before approving. None of that lives at the agent closest to the customer. It lives in the workflow platform. Pega's case management, ServiceNow's process modeling, Salesforce's flow engine, SAP's BPM — these are mature, regulator-tested, audit-traceable systems for holding state across long-running processes with multiple decision points and multiple humans involved. The agentic frameworks have been re-implementing this badly, because their frame of reference is *the conversation* rather than *the case*. A bank cannot run a loan workflow on conversational state, and a regulator will not let it.

Together these three barriers describe the architectural territory the workflow vendors hold and the foundation labs structurally cannot. Skills clear the invocation barrier. A2UI clears the interaction barrier. The workflow runtime clears the state and governance barrier. The first two are where the foundation labs and the workflow vendors meet. The third is where the workflow vendors keep durable advantage. All three together are what makes it possible to keep the human in the room — present, informed, supported by the right interface — at the moment the agentic system needs a decision the regulator and the customer ultimately need a person to make. The bet is unproven. The fact that multiple credible incumbents are making it simultaneously, with publicly inspectable artifacts, is itself the analytically meaningful observation.

The credible incumbents are also doing two other things alongside clearing the three barriers. They are emphasizing regulatory durability across banking, insurance, healthcare, government, and telecom, as the dimension on which their position strengthens as agentic AI moves from pilot to production. And they are building out governance, audit, identity, and policy enforcement capabilities at the orchestration layer, often in partnership with dedicated AI governance vendors like Credo AI rather than attempting to build everything in-house.

The strongest evidence that the entry-point migration is not speculative comes from the developer community itself. *GSD*, short for *Get Shit Done*, is a community-built framework on top of Claude Code, written by an independent developer named Lex Christopherson. It is one of three major frameworks — alongside *Superpowers*, which Anthropic accepted into its official marketplace in January 2026, and a third called *gstack* — that together account for roughly ninety-four thousand active developers as of April 2026. What these frameworks have in common is that they let developers do their full software development lifecycle from inside the AI assistant: planning, building, reviewing, shipping, all without leaving the foundation lab UI. The developer community has not waited for the official frameworks to mature. Developers have built their own tooling to do the work where they want to do it, which is inside Claude or Cursor or Codex rather than inside the traditional IDE. The migration of the developer authoring surface is not a forecast. It is already in motion, driven by demand from below rather than push from above. The workflow vendors that meet the developer where the developer already is will own the architectural relationship the developer brings into the regulated enterprise. The workflow vendors that wait for the developer to come back to the traditional IDE will not.

The incumbent vendors that do not pursue this positioning have a more difficult two-year window. The analyst consensus is not predicting that any particular incumbent will fail. The analyst consensus is observing that the incumbents that do not credibly own a layer the foundation labs structurally cannot own will face accelerating margin compression as their application-layer functionality is increasingly substitutable by agentic systems built on top of the foundation models their customers are deploying anyway. *Punished* is the right word for the equity-market reaction. *Repositioned* is the word for what the analyst community is observing among the vendors moving fastest. The two outcomes are not yet decided for any of the named incumbents, but the strategic move that separates them is visible.

The bank architect from the previous section is the practical reader of this observation. Her question, *which platforms is this advisor commercially aligned with*, is one part of the diligence. The companion question is *which incumbent vendors in my existing stack are credibly executing the workflow-and-governance positioning, and which are signaling it without execution*. The vendors that are credibly executing are the ones whose 2026 commitments will look defensible in 2028. The vendors that are signaling without execution are the ones whose share prices are leading indicators of an architectural problem that the equity market is pricing faster than the vendors are repositioning.

---

## 6. What does the analyst community think the next twenty-four months will look like?

Imagine two enterprises sitting at the same starting point in early 2026. Both are in regulated industries — a bank, an insurer, a healthcare system, a telecom, a utility, the specifics do not matter. Both have similar revenue, similar headcount, similar technology debt. Both have done the obligatory pilots: a customer service bot, an internal knowledge agent, a code generation assistant. Both want to scale. Both have boards asking when the AI line item is going to start producing the productivity numbers the analysts have been promising.

Enterprise A treats 2026 as a year of experimentation. It runs more pilots through the year. It rotates through three foundation model vendors looking for the best demo. It commissions a strategic review from the consultancy with the strongest existing relationship and accepts the recommended platform stack without asking which commercial alignments the consultancy is operating under. It has not built a governance platform layer because it is *too early to commit*. It has not built TCO instrumentation because the inference costs *are still trending down*. It has not assigned dedicated AI operations ownership because the function *will get clearer once the right platform is chosen*. It has noticed that other enterprises are publishing skills against the open Agent Skills standard but has decided to wait, because *the standard might shift*.

By the end of 2026, Enterprise A has more pilots than the previous year and not a single one in production at enterprise scale. The board is patient, but the patience is fraying. By the middle of 2027, the consultancy has been replaced with a new one, the platform stack has been re-evaluated and partially unwound, the governance question has surfaced as an audit finding from the regulator, and the inference budget is forty percent over plan. The TCO instrumentation that was deferred has been retrofitted onto deployments whose costs were never measured at design time. The CFO is asking who decided to commit to this platform stack. Nobody has a clean answer because the decision was made by the consultancy nobody is currently working with. The architects who recommended it are arguing that the original decision was sound and the failure is in execution. The CIO is reading the McKinsey six-in-ten-stuck statistic and wondering whether the company is in the six or in the one.

Enterprise B treats 2026 as a year of decision. It picks a primary stack with explicit awareness of the commercial alignments inside the recommendation, naming the alignments as part of the procurement record. It builds the governance platform layer in parallel with the agent framework, in partnership with a dedicated AI governance vendor rather than building it in-house. It instruments TCO from the first pilot, on the assumption that inference costs will compound faster than budgets will grow. It assigns a senior leader, not a committee, operational ownership of agentic deployments, with clear gates between sandbox, pilot, departmental rollout, and enterprise scale. It ships a skill against the Agent Skills standard so that its developers can author internal applications from inside the foundation lab UI without leaving the company's governance perimeter.

By the end of 2026, Enterprise B has fewer pilots in flight than Enterprise A but two of them are running in production at departmental scale. Its inference budget is on plan because the TCO instrumentation has been telling the team where costs are concentrating. The regulatory dialogue is quieter, because the governance posture was designed in alongside the deployment rather than retrofitted onto an audit finding. By the middle of 2027, two more deployments are at enterprise scale, the productivity gains are visible in the operational metrics the CFO uses, and the conversation with the board has shifted from *when will the AI investment produce returns* to *how aggressively can we expand the cohort of regulated workflows the platform now governs*.

By the end of 2027, the productivity gap between these two enterprises will be visible in their earnings calls. Enterprise B listened to what the analysts have been advising for two years. Enterprise A did not. The statistics that opened this companion were the warning. The two enterprises are what ignoring or heeding the warning produces in practice. Enterprise B is COiN at scale. Enterprise A is the next Klarna, prevented from full reversal only by the smaller scale at which it has so far deployed.

BCG analyzed retail banking and concluded that technology's share of operating cost will roughly double over the next several years, going from a quarter today to closer to forty percent. The shift is not a budget increase. It is a rebalanced cost mix. McKinsey forecasts that AI workloads alone will drive infrastructure costs to two-to-three times their 2025 levels by 2030, while overall budgets stay flat. The math is unforgiving. *TCO* — total cost of ownership, the full cost of running a system across its life including infrastructure, inference, integration, governance, and the human labor to operate it safely — therefore decides whether 2027's deployments scale or get unwound. Architects who have not measured TCO from the first pilot are running deployments they will be asked to defend without the numbers needed to defend them. Enterprise A is in this position by 2027. Enterprise B is not.

The directive to the technology buyer is plain. Choose the vendor whose product fits your company's objectives, judged on its own merits, and do not let the commercial alignments of the consultancy advising you decide the architecture for you. Gartner says the same thing in its 2026 guidance, more carefully: *use the Magic Quadrant as a starting point, not a shortlist*. The *Magic Quadrant* is Gartner's framework for ranking vendors across the axes of *ability to execute* and *completeness of vision*.

Three developments over the next twenty-four months are worth watching.

- **Shared standard for connecting workflow tools to AI assistants becomes standardized.** Today, a Pega skill or a Salesforce skill works inside Claude, inside ChatGPT, and inside the developer tools the same labs publish, because the AI labs agreed on a shared format for how these skills are written. Anthropic published the format in December 2025 and OpenAI adopted it shortly after. If Anthropic, OpenAI, and Google decide they are better off with their own incompatible formats so customers cannot move between assistants, Enterprise B's investment in publishing a skill becomes useful only inside whichever assistant still supports the format. *This would be a bad outcome.* The shared standard is what makes the workflow vendors' bet on the new entry-point posture viable. Watch the AI labs' 2026 product announcements for any softening of the commitment.
- **Inference costs fall faster than workloads grow.** *Inference cost* is the per-question charge an enterprise pays each time an agent calls a model. McKinsey forecasts that infrastructure costs from AI workloads will outpace any per-question cost decline by two-to-three times by 2030. If the per-question cost falls faster than that — driven by cheaper chips, cheaper energy, more efficient models, or all three — Enterprise A's failure to instrument TCO from the start becomes a smaller mistake, because the deployments they cannot defend on cost terms today become defensible by accident. *This would be neither good nor bad on its own merits.* It would let undisciplined architectures off the hook, which is bad for the discipline the corpus advocates, but it would also expand the addressable population of regulated workflows that can be safely automated, which is good for the broader productivity argument. The current trajectory — GPU constraint, energy constraint, agentic workload growth — does not support this scenario.
- **The consultancies advising on agentic AI architecture disclose their commercial alignments.** None of the major consultancies operating inside the OpenAI Frontier Alliance, the Google Gemini Enterprise program, or the Anthropic partnership tier currently disclose, in real time and at the granularity that would let a chief architect adjust a recommendation against the alignment, which platforms their practice is commercially incentivized to recommend. The disclosure norms in agentic AI advisory in 2026 are weaker than the disclosure norms in pharmaceutical clinical trials, in equity research, and in legal practice. *Stronger disclosure would be a good outcome.* It would let the technology buyer answer the *which platforms is this advisor commercially aligned with* question in 2026, before the architectural commitment is made, rather than discovering the answer in 2027 when the consequences land. Watch for any of the Big Four, the Magic Circle of strategy consultancies, or the system integrators publishing alignment-disclosure policies of their own accord. The corpus's expectation is that none will, and that the disclosure question will end up being settled by regulators rather than by the consultancies themselves.

The two-enterprise comparison does not look the same in every market.

In Frankfurt and Paris, the EU AI Act sets a regulatory minimum that pushes every regulated enterprise toward Enterprise B's discipline by 2027 or exposes them to fines. The question in Europe is not whether to operate like Enterprise B. It is how much further past the regulatory minimum a given enterprise can go.

In Singapore, Tokyo, and Seoul, financial regulators have been treating workflow-runtime durability as a baseline supervisory expectation since 2024. More enterprises in those markets are already where Enterprise B is, because their regulators got there earlier than the EU did.

In China, the question of whether to use an AI lab from outside the country was settled by policy years ago. Enterprise A and Enterprise B both run on DeepSeek-grade open-weight models inside state-aligned workflow platforms. What separates them is how fast they ship, not what they ship on.

In Riyadh, Abu Dhabi, and Doha, the sovereign wealth funds underwriting agentic AI deployments make cost discipline less of a constraint than it is elsewhere. But the same funds also make the architectural advisors more often state-aligned, which changes which enterprises end up where on the A-to-B spectrum, and rarely in the direction of Enterprise B's independence.

In Mumbai, São Paulo, Lagos, and Nairobi, the population of enterprises drifting toward Enterprise A is larger. Architectural discipline is harder to source locally, and the consultancy commercial alignments arrive imported from US and European parent firms with the alignments still attached.

Globally, the gap between the disciplined and the drifting will be larger than the gap inside any single market, because the disciplined cohort and the drifting cohort do not distribute evenly across regions.

The pattern the analysts are describing without using the phrase is a thinning. Not in the number of vendors — there are more agentic AI vendors than ever. A thinning in the number of enterprises that have built the discipline to operate agents at scale and keep operating them. That cohort sits between ten and twenty-one percent depending on which discipline is being measured. The remaining seventy-nine to ninety percent are the contested ground for the next twenty-four months. Most of them will choose by default. A smaller group will choose by architecture.

The architectural decision the next twenty-four months will force on every regulated enterprise is whether the human stays present when the agent makes the call. Enterprise A's drift is not a question of insufficient ambition. It is a question of insufficient architecture for keeping the human present as the agents proliferate. Enterprise B's discipline is not a question of better technology. It is a question of having designed the workflow so the human is still the one making the decisions the regulator and the customer ultimately need a person to make. *Removing the human from the room is the central danger of the age* is not a philosophical position. It is the architectural diagnostic that separates the disciplined enterprises of 2027 from the drifting ones. The analyst data points to it. The cross-vendor moves point to it. The next twenty-four months will measure it.

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
