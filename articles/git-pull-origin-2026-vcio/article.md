*This is the CIO edition. A [companion edition for general readers](/public/articles/git-pull-origin-2026-vpublic) is also published.*

## Overture: The Window That Rewrote Software

In June 2021, GitHub introduced Copilot as a technical preview and called it "your AI pair programmer," a phrase that turned an unfamiliar machine-learning system into something software teams already understood: a colleague beside you at the keyboard ([GitHub Blog](https://github.blog/news-insights/product-news/introducing-github-copilot-ai-pair-programmer/)). A pair programmer, in software culture, is not a replacement for the person writing code. It is a second mind in the room, watching, suggesting, questioning, and helping the driver move faster without losing judgment.

The framing came from Nat Friedman, a soft-spoken venture investor running GitHub at the time. Friedman did something that day that already seems unimaginable in 2026: he sat in the Hacker News comments and answered engineers personally. He defended the legality of training on public code. He argued that this was the third great wave of programmer productivity — first compilers, then open source, now AI. He treated the panic as legitimate and the conversation as worth having. The framing held. "AI pair programmer" became the seed of an entire industry.

Less than five years later, that industry has produced companies worth tens of billions of dollars, has rewritten the daily work of millions of engineers, and has triggered the most expensive talent war in software history. The first wave of AI coding tools completed lines of code. The second wave chatted about code. The current wave plans work, edits multiple files, runs tests, opens pull requests, and in some cases works in the background while the engineer moves on to something else. The story is no longer just "autocomplete got better." The story is that the unit of software labor is shifting from a typed line to a delegated task.

This guide is written for a broad audience with executive depth. It assumes no formal background in artificial intelligence, data science, or software engineering, but it does not oversimplify the stakes. Every technical term is introduced in plain English. Every major product is treated as both a tool and a strategic signal. The goal is not to memorize vendor names. The goal is to understand who now holds the keyboard, who reviews the work, and how organizations should govern a world where software can increasingly be assigned rather than hand-written.

The structure is deliberate. Part I gives the vocabulary. Part II explains the standards stack emerging underneath the tools. Part III introduces the major players as characters in a still-unfolding story. Part IV identifies the patterns across the field. Part V translates the market into corporate adoption stories that matter in boardroom conversations. Part VI explains skills, reusable work, and operating frameworks. Part VII offers an executive cheat sheet. Part VIII gives analogies that travel well in talks and briefings. Part IX offers a practical starting path for new developers. Part X looks at where the category is going next.

Read cover to cover, this guide should give a CIO, operator, strategist, or curious non-technical reader a clearer view of the AI coding-agent category than most organizations currently hold.

-----

## Part I: The Vocabulary

The category is dense with terms that sound similar and mean different things. Clarity matters because the wrong word can lead to the wrong governance model. A company buying an autocomplete tool needs training and productivity metrics. A company deploying autonomous agents needs review systems, sandboxing, security controls, and new operating norms.

**Large language model (LLM).** A large language model is an AI system trained to predict and generate sequences of text, including computer code. It does not "know" in the human sense. It produces outputs by learning statistical patterns from enormous training data and then generating a response token by token. A token is a small unit of text, often part of a word, a whole word, or a code symbol. In this field, models such as Claude, GPT, and Gemini are the engines; products such as Copilot, Cursor, Claude Code, Codex, and Devin are the vehicles built around those engines.

**IDE, or integrated development environment.** An IDE is the application where software engineers write, run, debug, and organize code. Microsoft Visual Studio Code, JetBrains IntelliJ, and Apple Xcode are familiar examples. For non-technical readers, an IDE is like Microsoft Word for software, except it also knows how to run the document, test it, and reveal where it breaks.

**CLI, or command-line interface.** A CLI is the terminal, the text-based window where engineers type commands directly into the computer. It looks old-fashioned, but it remains central to professional software work because it is fast, scriptable, and precise. Several of the most powerful coding agents live here because the terminal already has access to the machinery of software work: files, tests, package managers, cloud tools, and deployment commands.

**Plugin or extension.** A plugin is a smaller tool that lives inside a larger application. Many AI coding products began as plugins inside an existing IDE. That made adoption easier, but it also limited how deeply the AI could shape the development workflow.

**Fork.** A fork happens when a team takes the source code of an existing open-source project and builds a new project from it. Cursor, for example, is commonly described as a fork of Microsoft's Visual Studio Code, which let it preserve a familiar editor experience while rebuilding the AI layer around it ([TechCrunch](https://techcrunch.com/2023/10/11/anysphere-raises-8m-from-openai-to-build-an-ai-powered-ide/)).

**Autocomplete or code completion.** Autocomplete is the original AI-coding behavior. The engineer starts typing and the tool suggests the next word, line, function, or block. The engineer accepts or rejects the suggestion. The human remains in the driver's seat at every moment.

**Agent.** An agent is a system that can pursue a goal through multiple steps. In coding, that can mean reading the codebase, making a plan, editing files, running tests, interpreting failures, making another edit, and reporting what changed. The difference between autocomplete and an agent is the difference between a calculator and an analyst. One helps at the point of input. The other can carry a task across time.

**Synchronous versus asynchronous.** Synchronous work happens live. The engineer watches, steers, and interrupts. Asynchronous work is delegated. The engineer assigns a task and checks back later. Cursor and Claude Code often feel synchronous. Codex Cloud, Devin, Jules, and GitHub's coding-agent direction push toward asynchronous delegation.

**Sandbox.** A sandbox is an isolated computing environment where software can run without touching critical systems. For AI agents, sandboxes matter because agents need freedom to experiment without freedom to cause damage. OpenAI describes Codex Cloud as working in its own cloud environment and allowing tasks to run in the background, including in parallel ([OpenAI Developers](https://developers.openai.com/codex/cloud)).

**Pull request (PR).** A pull request is the standard review unit in modern software teams. Engineers do not usually push changes directly into production. They create a branch, make changes, and submit a PR so others can review the proposed work. When an AI agent "creates a PR," it has produced a reviewable unit of software contribution.

**SWE-bench.** SWE-bench is a benchmark for evaluating whether AI systems can fix real software issues. Its Verified subset is a human-filtered set of 500 tasks, and the benchmark reports the percentage of instances solved ([SWE-bench](https://www.swebench.com)). Benchmarks are useful, but executives should treat them as directional signals rather than purchasing decisions. A tool can score well and still fit poorly into a company's workflow, codebase, risk posture, or security model.

**MCP, or Model Context Protocol.** MCP is an open standard introduced by Anthropic for connecting AI assistants to the systems where organizational data lives, including content repositories, business tools, and development environments ([Anthropic](https://www.anthropic.com/news/model-context-protocol)). The practical idea is simple: instead of every AI vendor building a custom connector to every tool, a shared protocol lets agents connect through a common pattern. For CIOs, MCP is not a feature. It is integration plumbing.

**ARR, or annual recurring revenue.** ARR is the annualized revenue a subscription business is on track to generate at its current run rate. In software markets, ARR is a shorthand for commercial traction. It is also easy to misuse. Company-reported ARR, rumored ARR, annualized revenue, and signed-contract run rate are not always the same thing.

**Vibe coding.** "Vibe coding" is a term popularized by Andrej Karpathy in February 2025 to describe building software by describing what one wants, accepting AI-generated changes, and steering by feel rather than reading every line of code ([Andrej Karpathy on X](https://x.com/karpathy/status/1886192184808149383)). The term is playful, but the governance issue is serious. Vibe coding can be productive for prototypes. It can be dangerous in production if nobody understands what was generated.

With those terms in hand, the players come into focus.

-----

## Part II: The Standards Stack Beneath the Agents

The tool names change quickly. Standards last longer. A CIO can switch from one coding agent to another, but the deeper strategic question is which protocols define how agents find tools, talk to one another, present interfaces, spend money, authenticate identity, and preserve reusable knowledge. This is the layer where the future operating system of agentic work is being negotiated.

The easiest way to understand the standards stack is to ask five questions:

- **What can the agent use?** That is the MCP question.
- **Which other agents can it coordinate with?** That is the A2A question.
- **How does it show work to a human in a safe interface?** That is the A2UI and AG-UI question.
- **How does it buy, pay, or transact?** That is the agentic-commerce question.
- **How does it remember the team's way of working?** That is the skills question.

### MCP: the USB-C port for tools and context

Model Context Protocol, or MCP, is the standard most likely to matter first inside enterprises. Anthropic introduced MCP in 2024 as an open standard for connecting AI assistants to the systems where data lives, including content repositories, business tools, and development environments ([Anthropic](https://www.anthropic.com/news/model-context-protocol)). In plain English, MCP is a common adapter. Instead of building a custom integration between every agent and every internal tool, an organization can expose a tool through MCP and let compatible agents discover and use it.

For software engineering, MCP can connect an agent to GitHub, Slack, databases, documentation stores, observability systems, ticket queues, deployment tools, and internal APIs. This matters because real engineering work is not only in code. It is in the incident report, the pull request discussion, the customer complaint, the log line, the data contract, and the design decision that was made six months ago.

OpenAI's Apps SDK shows how quickly MCP is becoming more than Anthropic plumbing. OpenAI says its Apps SDK builds on MCP, extends it so developers can design both the logic and interface of apps, and is open source so apps built with it can run anywhere that adopts the standard ([OpenAI](https://openai.com/index/introducing-apps-in-chatgpt/)). Microsoft's NLWeb similarly describes every NLWeb instance as an MCP server, making website content discoverable and accessible to agents that participate in the MCP ecosystem ([Microsoft](https://news.microsoft.com/source/features/company-news/introducing-nlweb-bringing-conversational-interfaces-directly-to-the-web/)).

The executive translation: MCP is not a chatbot feature. It is the enterprise integration layer for agents.

### A2A: when agents need to talk to agents

Agent2Agent, or A2A, answers a different question. MCP helps an agent use tools. A2A helps agents coordinate with each other. Google announced A2A in April 2025 as an open protocol that gives agents a standard way to collaborate regardless of vendor or framework ([Google Developers Blog](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/)). Google describes A2A as complementary to MCP: MCP connects agents to tools and context, while A2A helps agents communicate, delegate, and coordinate across platforms ([Google Developers Blog](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/)).

This sounds abstract until the organization has more than one agent. Imagine a bank with a coding agent, a compliance agent, a security-review agent, a testing agent, and a release-management agent. If each one is trapped inside a separate vendor product, the system becomes brittle. If they can discover one another, describe their capabilities, exchange state, and pass tasks safely, the organization starts to have an agent workforce rather than a pile of disconnected bots.

For readers new to the idea, the analogy is email. Email became powerful because people on different systems could still message one another. A2A is trying to create that kind of interoperability for agents.

### A2UI and AG-UI: when agents need to show, not just tell

A2UI may be one of the most important standards for the next phase because it addresses a problem most executives have not yet named: agents need to generate interfaces, not just text. Google introduced A2UI in December 2025 as an open-source project for agent-driven, cross-platform generative user interfaces ([Google Developers Blog](https://developers.googleblog.com/introducing-a2ui-an-open-project-for-agent-driven-interfaces/)). Its purpose is to let agents generate or populate rich user interfaces that can be rendered by different host applications and UI frameworks ([Google Developers Blog](https://developers.googleblog.com/introducing-a2ui-an-open-project-for-agent-driven-interfaces/)).

The design principle is crucial. A2UI uses a structured, declarative format. Declarative means the agent describes what the interface should contain — for example, a form, table, chart, approval card, or workflow panel — rather than sending arbitrary executable code. The client application then renders that payload using its own trusted components. Google's explanation emphasizes that the client retains control over styling and security, so the agent's output can feel native to the app without letting the agent run unsafe interface code ([Google Developers Blog](https://developers.googleblog.com/introducing-a2ui-an-open-project-for-agent-driven-interfaces/)).

AG-UI, from CopilotKit, sits beside this. AG-UI describes itself as an open protocol for agent-user interaction: a bidirectional connection between a user-facing application and an agentic backend ([CopilotKit](https://www.copilotkit.ai/ag-ui)). CopilotKit's framing is helpful: MCP handles context and tool access, A2A handles agent coordination, and AG-UI defines the interaction layer between the user, the application, and the agent ([CopilotKit](https://www.copilotkit.ai/ag-ui)). It also distinguishes AG-UI from A2UI: A2UI is the generative UI specification, while AG-UI is the runtime connection that keeps the user-facing app and the agent backend in sync ([CopilotKit](https://www.copilotkit.ai/ag-ui)).

The executive translation: the future of agentic software is not a chat box pasted onto every application. It is applications that can reshape their interface around the task, while still preserving enterprise control over security, design, and approvals.

### Agentic commerce: when agents transact

Once agents can act, they eventually need to transact. OpenAI introduced Instant Checkout and the Agentic Commerce Protocol with Stripe in 2025, describing the protocol as a way for AI agents and businesses to complete purchases for users while keeping merchants in control of the customer relationship ([OpenAI](https://openai.com/index/buy-it-in-chatgpt/)). The protocol is designed to work across platforms, payment processors, and business types, and OpenAI says merchants using Stripe can enable agentic payments quickly while others can participate through shared payment-token or delegated-payment specifications ([OpenAI](https://openai.com/index/buy-it-in-chatgpt/)).

This belongs in a coding-agent article because software agents will not stay inside software development. The same patterns will show up in procurement, commerce, customer support, insurance claims, travel booking, and operations. The moment an agent can spend money or trigger a contractual action, governance becomes more than code review. It becomes authority management.

### Skills: the reusable knowledge layer

Skills are the bridge between general intelligence and organizational competence. A model may know Python, React, SQL, or Kubernetes. It does not automatically know how a particular company writes release notes, tests payment flows, reviews Terraform, names database migrations, responds to incidents, or formats board memos. Skills package that local knowledge.

OpenAI describes Codex skills as reusable instructions, resources, and scripts for repeated work, meant to preserve the thread, document, command, or example that made Codex useful the first time ([OpenAI Developers](https://developers.openai.com/codex/use-cases/reusable-codex-skills)). Anthropic's guide defines a Claude skill as a set of instructions packaged as a simple folder that teaches Claude how to handle specific tasks or workflows, with components such as `SKILL.md`, optional scripts, references, and assets ([Anthropic](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf)).

This is where software stops being "from scratch." A junior developer with a blank AI chat window is starting from zero. A junior developer with a company's testing skill, migration skill, security-review skill, design-system skill, incident-summary skill, and deployment skill is starting from the accumulated operating knowledge of the team. That is an enormous uplift.

The strategic lesson: the best organizations will not only buy agents. They will build skill libraries.

-----

## Part III: The Major Players

### 1. GitHub Copilot — The Original On-Ramp

The story begins with GitHub Copilot because it put AI-assisted coding in front of mainstream developers first. GitHub announced Copilot on June 29, 2021 as a technical preview built in collaboration with OpenAI and powered by OpenAI Codex, with the ability to draw context from the code a developer was working on and suggest whole lines or entire functions ([GitHub Blog](https://github.blog/news-insights/product-news/introducing-github-copilot-ai-pair-programmer/)).

The first Copilot experience was intentionally modest. It lived inside the editor. It watched the surrounding code. It suggested what might come next. That modesty mattered because it made the product legible. Engineers could try it without changing their entire workflow, and enterprises could pilot it without pretending they were hiring robot employees.

GitHub later published research claiming that, in a controlled experiment with 95 professional developers writing an HTTP server in JavaScript, developers using Copilot completed the task 55% faster than those who did not use Copilot, with a 95% confidence interval of 21% to 89% for the speed gain ([GitHub Blog](https://github.blog/news-insights/research/research-quantifying-github-copilots-impact-on-developer-productivity-and-happiness/)). That statistic became one of the most quoted numbers in the category, but the executive reading is narrower than the marketing reading. It does not prove every engineer becomes 55% faster on every task. It proves that, under a defined experimental setup, the productivity effect was large enough to take seriously.

Copilot's most important strategic move came in 2024, when GitHub announced multi-model choice inside Copilot. At GitHub Universe 2024, the company said developers using Copilot in Visual Studio Code and on github.com would be able to choose among models from Anthropic, Google, and OpenAI, and GitHub CEO Thomas Dohmke framed the change around "the right model for the right use case" ([GitHub Newsroom](https://github.com/newsroom/press-releases/github-universe-2024)). That move repositioned Copilot from a single-model assistant into a distribution platform for AI coding.

**Why it stands out.** Copilot's advantage is reach. It sits inside the GitHub and Microsoft ecosystem, which makes it the path of least resistance for many organizations. If an enterprise already uses GitHub, Visual Studio Code, Microsoft identity, and Microsoft procurement, Copilot is the lowest-friction starting point.

**The honest critique.** Copilot is not always the most loved tool among power users. Developers who want deeper autonomy, richer agent loops, or a more AI-native editor often move toward Cursor, Claude Code, or Codex. But Copilot remains the category's best on-ramp: familiar, institutionally acceptable, and easy to trial.

### 2. Cursor — The AI-Native Editor and the MIT Dropouts

Cursor is the clearest example of a startup turning developer workflow into a high-growth software category — and the story behind it is one of the most extraordinary in modern software. Anysphere, the company behind Cursor, was founded in 2022 by four MIT students: Michael Truell, Sualeh Asif, Arvid Lunnemark, and Aman Sanger.

Their backgrounds were not ordinary. Truell had built one of the world's most popular programming games at age fourteen. Asif had grown up in Karachi, Pakistan, and represented his country at the International Mathematical Olympiad. Lunnemark, originally Swedish, had won medals at the International Olympiad in Informatics — the elite high-school world championship of computer programming. All four turned down standard MIT-graduate paths at Google and hedge funds to build something together.

The company's first year, in Truell's own description, was "wandering in the desert." The team initially tried to build an AI tool for *mechanical* engineers, picking the space partly because it was, as Truell told a Y Combinator audience, "sleepy and uncompetitive." It was also sleepy because it was small, and the four of them lacked deep domain expertise. They flailed.

The pivot, when it came, was decisive. *"We were obsessed with AI's potential to change software development,"* Truell later recalled. *"But existing tools like GitHub Copilot weren't pushing the limits. We realized AI should not just assist coding — it should be the foundation of how developers work."* The crucial word was *foundation*. They were not going to build a plugin. They were going to fork Microsoft's Visual Studio Code, preserve the editor experience millions already knew, and rebuild the AI layer around the codebase rather than bolting it on.

Anysphere raised an $8 million seed round in October 2023 led by OpenAI's Startup Fund ([TechCrunch](https://techcrunch.com/2023/10/11/anysphere-raises-8m-from-openai-to-build-an-ai-powered-ide/)). What followed is one of the fastest growth stories in software history. By 2026, TechCrunch reported that Cursor was in talks to raise at least $2 billion at a $50 billion pre-money valuation, after a prior reported valuation of $29.3 billion, and that the company had reached $2 billion in annualized revenue in February, according to people familiar with the matter ([TechCrunch](https://techcrunch.com/2026/04/17/sources-cursor-in-talks-to-raise-2b-at-50b-valuation-as-enterprise-growth-surges/)). Those numbers should be presented carefully because they are reported by press sources, not audited public-company filings. Even with that caveat, the direction is unmistakable: AI coding is no longer a productivity add-on. It is a venture-scale platform market.

**The human experience.** Cursor feels like a familiar editor with a more capable colleague inside it. A developer can highlight code and ask for a change, ask the agent to edit multiple files, or use natural language to move through unfamiliar parts of a project. The experience is synchronous enough to feel controllable and agentic enough to feel meaningfully different from autocomplete. Truell has been almost obsessive about *speed* — the team built advanced caching systems specifically so the AI feels instant rather than laggy.

**Why it stands out.** Cursor sits at the sweet spot between accessibility and power. It is visual, fast, model-flexible, and close to the daily habits of professional engineers. For organizations that want engineers to feel the productivity gain immediately, Cursor is often the product that creates the "now I understand" moment.

**The honest critique.** Cursor's strength is also its strategic vulnerability. It is a tool company in a world where the model providers are building tools of their own. If the underlying frontier models become more interchangeable, Cursor must keep winning on workflow, speed, enterprise trust, and developer love.

### 3. Claude Code — The Side Project That Became the Standard

Of all the stories in this category, Claude Code is the one that most resembles a startup myth told inside a big company. Anthropic's documentation describes Claude Code as an AI-powered coding assistant that understands the codebase, works across multiple files and tools, runs in the terminal, and can stage changes, write commit messages, create branches, and open pull requests ([Anthropic Docs](https://docs.anthropic.com/en/docs/claude-code/overview)).

The protagonist behind it is Boris Cherny, a software engineer with a winding path. He was born in Odessa, Ukraine. His grandfather was one of the first programmers in the Soviet Union, working with punch cards — the physical paper media that early programs were stored on. When Boris's mother was a child, she would draw on those punch cards because to her, they were just paper; her father, meanwhile, was using them to write some of the earliest software in his country. The family moved to the United States in 1995. Cherny started his first startup at eighteen, has no computer science degree (he studied economics before dropping out), and spent a decade at Meta, working at Facebook and Instagram and rising to principal engineer.

He joined Anthropic in September 2024 and started, in his own words, "hacking around using Claude in the terminal." His first prototype could read what music he was listening to and change the song based on his input. It could not read files. It could not write code. It was mostly a toy. But after a conversation with Cat Wu, a product manager researching how AI agents could use computers, Cherny gave the terminal more capabilities: file access, the ability to run commands. He showed it to colleagues. Within five days of an internal release in November 2024, half of Anthropic's engineering team was using it.

The advice Cherny's manager Ben Mann gave him would define the project: *"Don't build for today's model, build for the model six months from now."* That meant the early version felt mediocre — but when Anthropic's Claude 4 model series launched in 2025, the same harness produced strikingly competent work.

The terminal matters because it is where much of serious software work already happens. Tests run there. Dependencies install there. Servers start there. Logs appear there. A terminal-native agent can act in the same environment an engineer uses to diagnose and repair systems. Claude Code also benefits from Anthropic's broader context strategy: MCP, introduced in late 2024, is designed to connect AI tools to external data sources through a standard protocol rather than one-off integrations ([Anthropic](https://www.anthropic.com/news/model-context-protocol)). For coding agents, this is especially important because the code is rarely enough — real software work lives across GitHub issues, Linear tickets, Slack threads, design documents, databases, logs, and internal runbooks.

**The human experience.** Claude Code feels less like a shiny app and more like a senior assistant sitting inside the machinery room. The user describes a task. Claude reads files, proposes a plan, edits, runs commands, and explains the changes. It is less visual than Cursor, but for many experienced engineers it feels more direct. Cherny has noted that 80–90% of Claude Code itself is now written by Claude Code — the product writes its own product.

**Why it stands out.** Claude Code stands out for depth, configurability, and the close relationship between model and tool. Anthropic controls both the Claude models and the coding agent, which lets the company tune the experience across the full stack.

**The honest critique.** The terminal-first posture narrows the audience. It is powerful for engineers who already live in command-line workflows, but less welcoming for teams that want a visual, IDE-centered experience. For broad enterprise rollout, that means enablement and training matter.

### 4. OpenAI Codex — The Asynchronous Comeback

Codex is a name with history. The original Codex was the code-specialized model that powered early GitHub Copilot. The current Codex is a coding-agent product line that launched in 2025 and represents OpenAI's bet that software work will become a task queue. OpenAI's developer documentation describes Codex as a coding agent that can read, edit, and run code, while Codex Cloud can work on tasks in the background, including in parallel, using its own cloud environment ([OpenAI Developers](https://developers.openai.com/codex/cloud)).

That background-work model is the key. Codex is not only a conversation partner. It is a delegation surface. A developer can kick off a cloud task, monitor progress, and apply resulting diffs locally, or tag Codex from GitHub issues and pull requests so it can propose changes directly from the collaboration system where software teams already work ([OpenAI Developers](https://developers.openai.com/codex/cloud)).

OpenAI has expanded Codex across surfaces. Its changelog describes Codex availability across the app, CLI, IDE extension, and web, with frontier coding models appearing in the Codex model picker, including the Codex-Spark research-preview line introduced for real-time coding at speeds reported in excess of 1,000 tokens per second ([OpenAI Developers](https://developers.openai.com/codex/changelog)). (Specific minor-version model names move quickly; readers should check the live changelog before quoting any particular version.)

**Why it stands out.** Codex is the cleanest expression of OpenAI's belief that software work will become a task queue. It is strongest when the work can be described, isolated, run in a cloud environment, and returned as a reviewable change. The open-source Codex CLI, made available under the Apache 2.0 license, is unique among frontier-lab flagships — anyone can read, audit, and modify the code that drives the agent.

**The honest critique.** Asynchronous delegation is powerful but not always comfortable. Engineers often want to steer while the work is happening, especially in ambiguous codebases. Codex works best when the task is scoped clearly and the review process is strong.

### 5. Devin — The Autonomy Bet, and the Most Extraordinary Founder Story

Devin, built by Cognition, is the most explicit attempt to make an AI system feel like a software engineer rather than a tool. It is also fronted by the most extraordinary individual story in the category.

Scott Wu was born in 1997 in Louisiana to a Chinese immigrant family. He attended Baton Rouge Magnet High School. By the time he was a teenager, he was one of the strongest competitive programmers in the world. He won three gold medals at the International Olympiad in Informatics — the high-school world championship of computer programming — and placed first overall in 2014. He came third in Google's 2021 Code Jam. He achieved Legendary Grandmaster status on Codeforces, the platform where the world's best competitive programmers train.

He went to Harvard, dropped out after two years, and co-founded a startup called Lunchclub, which used AI to match professionals for one-on-one networking conversations. In 2023, he started Cognition AI with Steven Hao and Walden Yan. Wu's brother Neil Wu, also a competitive programming champion, joined the team.

Cognition's pitch to investors was simple and audacious: we are not building a coding assistant. We are building an actual AI software engineer — one that can be assigned a ticket the way you would assign one to a human, and that will return a finished, reviewed pull request hours later. They named the product Devin, after Devin Smith, an early engineer at Cognition. The name was loaded — a person's name, not a feature description — and that was deliberate. They wanted users to think of it as a colleague.

When they unveiled Devin in March 2024, the demo went viral. Stripe co-founder Patrick Collison called it "very impressive in practice." Other engineers were nervous; the social-media discourse swung between awe and predictions of mass unemployment.

A telling cultural detail: at Cognition's San Francisco headquarters, visitors take off their shoes at the door. The team keeps Allbirds-branded slippers in a basket. The ritual was a holdover from the company's earlier home in Atherton, a wealthy California suburb where Cognition operated out of a $10.5 million neoclassical mansion, sleeping in the bedrooms and turning the basement into a pit of monitors. *"These next three years are what we're going to tell our grandkids about,"* one early investor said. She had quit her venture-capital job to join Cognition full-time.

Cognition's own 2025 performance review reports that, 18 months after launch, Devin was working inside engineering teams at thousands of companies, including Goldman Sachs, Santander, and Nubank, and had merged hundreds of thousands of pull requests ([Cognition](https://cognition.ai/blog/devin-annual-performance-review-2025)). Goldman Sachs turned Devin into a boardroom reference point. CNBC reported in July 2025 that Goldman was evaluating Devin for use alongside its roughly 12,000 developers, with technology chief Marco Argenti saying the firm planned to start with hundreds of Devin instances and potentially expand into the thousands ([CNBC](https://www.cnbc.com/2025/07/11/goldman-sachs-autonomous-coder-pilot-marks-major-ai-milestone.html)). CNBC also reported Argenti's framing that engineers would need to articulate problems clearly, convert them into prompts, and supervise agent activity.

**Why it stands out.** Devin is the purest autonomy story. It gives executives a concrete picture of an AI "worker" inside the engineering organization, with humans shifting toward specification, supervision, and review.

**The honest critique.** Devin's framing can outrun reality if leaders treat it as a full replacement for human software judgment. The stronger interpretation is that Devin changes the *shape* of engineering management. It can absorb defined work, but the organization still needs humans to define the work, inspect the output, and own the consequences.

### 6. Windsurf — The Acquisition Saga and the Workflow Lesson

Windsurf matters for two reasons: product and market structure. As a product, it became known for an AI-first development environment centered around an agent named Cascade. As a market story, it became one of the clearest signals that AI coding had moved from tool category to strategic battleground — and the corporate drama that surrounded it in 2025 may be the most astonishing M&A saga in software history.

The founders are Varun Mohan and Douglas Chen, MIT classmates who graduated in 2017. Mohan was born to Indian immigrant parents in Sunnyvale, California, raised in the heart of Silicon Valley, and excelled at math and computing competitions during his high school years at the Harker School in San Jose. The two founded a company in 2021 called Exafunction. Their original product had nothing to do with coding — it was infrastructure for virtualizing GPUs, the chips most AI workloads run on. They had a profitable business.

In 2022, watching the explosion of generative AI, Mohan and Chen made a bet-the-company pivot. They abandoned the GPU virtualization business and rebranded as Codeium. They built an AI code-completion tool that they gave away for free across dozens of IDEs, attracting hundreds of thousands of developers. By August 2024, after a $150 million Series C, they were a unicorn at a $1.25 billion valuation. In November 2024, they launched a full IDE called Windsurf Editor, built around the Cascade agent. In April 2025, they renamed the entire company Windsurf.

Then the soap opera began. In May 2025, Bloomberg reported that OpenAI had agreed to acquire Windsurf for approximately $3 billion. The deal collapsed — Microsoft, which had invested over $13 billion in OpenAI, held contractual rights to OpenAI's intellectual property, including IP gained through acquisitions. That meant Windsurf's technology would have effectively become accessible to Microsoft, whose own GitHub Copilot was Windsurf's direct competitor. Mohan reportedly objected. Microsoft and OpenAI could not resolve the dispute. The exclusivity period on the deal expired in July 2025.

Three days after that exclusivity expired, Google moved. Reuters reported on July 11, 2025 that Google hired Windsurf CEO Varun Mohan, co-founder Douglas Chen, and some members of Windsurf's R&D team in a $2.4 billion deal that gave Google a non-exclusive license to certain Windsurf technology, without Google acquiring equity or control of the company ([Reuters](https://www.reuters.com/business/google-hires-windsurf-ceo-researchers-advance-ai-ambitions-2025-07-11/)). The legal name for this maneuver is a "reverse acqui-hire."

Three days *after that*, Cognition AI — Devin's parent company — announced a deal to acquire what remained of Windsurf, including its intellectual property, product offerings, trademark, brand, and personnel ([CNBC](https://www.cnbc.com/2025/07/14/cognition-to-buy-ai-startup-windsurf-days-after-google-poached-ceo.html)). Cognition got a pre-built customer base, a product, and a massive installed user base, while Google got the founders.

The transaction left a sour aftertaste. Of Windsurf's roughly 250 employees, the founders and a small inner circle did very well. Many of the remaining employees, who had been counting on a payout from the OpenAI deal, found their equity rendered nearly worthless by the structure of the Google licensing arrangement. The story became a small parable in venture capital about founder-employee alignment.

**Why it stands out.** Windsurf's story reveals how valuable the coding-agent layer has become. The product mattered, but the people and workflow knowledge may have mattered even more. A $3 billion deal collapsing because of competitive overlap with a parent company is unprecedented; Google paid $2.4 billion essentially to hire forty people.

**The honest critique.** Acquisition sagas can distract from product evaluation. For executives, the lesson is not that every AI coding startup will be bought. The lesson is that vendor stability, talent retention, roadmap continuity, and data portability should be part of procurement diligence.

### 7. Replit Agent — The Democratization Play, From Amman to Nine Billion Dollars

Replit is the rare story in this category that begins outside Silicon Valley.

Amjad Masad was born in 1988 in Amman, Jordan, to a Palestinian father from a refugee background and an Algerian mother. As a child, Masad did not own a computer. He learned to program on borrowed machines and at internet cafés in Amman. *"Growing up in Amman, Jordan, I didn't have a computer. I learned to program on borrowed computers, or at internet cafés,"* he later recalled. That experience defined his entire career. Setting up a development environment had been the slowest, most discouraging part of learning to code. His idea, which he had as early as 2009, was that this should be unnecessary. If Google Docs let you write a document in a browser without installing Microsoft Word, why couldn't programming work the same way?

Masad became a founding engineer at Codecademy in 2011 and joined Facebook in 2013, where he led JavaScript infrastructure. In 2016, he co-founded Replit with his wife Haya Odeh and his brother Faris Masad. The name comes from REPL, an acronym used in programming since the 1960s for *read-evaluate-print loop* — the cycle of typing in some code, having the computer run it, and seeing the result. Y Combinator rejected them four times.

By the end of 2016, despite the rejections, Replit had attracted 750,000 users. The team focused, between 2017 and 2020, on the unglamorous infrastructure problems of running other people's code safely in a browser at scale. Then they pivoted aggressively to AI. They launched Ghostwriter, an AI pair programmer. In September 2024, they shipped Replit Agent, an AI that could build entire applications from a natural-language description.

Replit announced in March 2026 that it had raised $400 million at a $9 billion valuation and launched Agent 4, describing the new agent as 10 times faster than Agent 3 and designed to move ideas from prompt to production inside a unified project environment ([Replit Blog](https://blog.replit.com/replit-raises-400-million-dollars)). Replit had previously introduced Agent 3 as its most autonomous agent, with browser testing, automatic fixing, and the ability to run independently for extended tasks ([Replit Blog](https://blog.replit.com/introducing-agent-3-our-most-autonomous-agent-yet)).

The Replit story matters because it changes who counts as a software creator. In the old model, the bottleneck was environment setup, syntax, and deployment. In the new model, the bottleneck becomes idea quality, product judgment, and whether the builder can recognize when the generated system is good enough, safe enough, and maintainable enough.

**Why it stands out.** Replit is the most accessible end-to-end path from idea to deployed application. That makes it strategically important far beyond the professional developer market — for a teenager building a first web app, a doctor building a triage tool, a teacher building a grading assistant, Replit is unmatched.

**The honest critique.** Replit is strongest for new applications and smaller projects. The more a project depends on a large legacy codebase, subtle institutional rules, compliance constraints, or deep production history, the more human engineering judgment remains central. A team rebuilding a thirty-year-old mainframe banking system is using the wrong tool.

### 8. Gemini CLI and Jules — Google's Open and Async Flank

Google's coding-agent strategy is best understood as a two-track play: Gemini CLI for developers who want an open-source terminal agent, and Jules for asynchronous delegated coding work. Google introduced Gemini CLI in June 2025 as a free, open-source AI agent under the Apache 2.0 license, bringing Gemini directly into the terminal ([Google Blog](https://blog.google/technology/developers/introducing-gemini-cli-open-source-ai-agent/)).

The Gemini CLI offer is unusually generous for individual developers. Google's announcement says a personal Google account gives access to Gemini Pro, a one-million-token context window, 60 model requests per minute, and 1,000 requests per day at no charge ([Google Blog](https://blog.google/technology/developers/introducing-gemini-cli-open-source-ai-agent/)). It also supports grounding prompts with Google Search, which lets the agent bring live web context into coding and research workflows.

Jules is the async counterpart. Google describes Jules as an autonomous coding agent that fetches a GitHub repository, clones it to a cloud virtual machine, develops a plan, provides diffs, and creates a pull request for approval ([Jules](https://jules.google)). The Jules page also describes fully asynchronous, multi-agent development with plan-based limits for daily tasks and concurrent tasks.

**Why it stands out.** Google's differentiator is openness plus scale. Gemini CLI gives developers a transparent, permissively licensed tool with a large free tier. Jules gives Google an async workflow surface that competes more directly with Codex and Devin.

**The honest critique.** Google has world-class models and infrastructure, but developer-tool adoption depends on trust, taste, and workflow fit. The products must become not only capable, but loved.

### Open-Source Counterweights: Aider, Cline, Continue, and OpenHands

The open-source tier matters because it keeps the market honest. **Aider**, created by veteran engineer Paul Gauthier — a former founding CTO of search-engine pioneer Inktomi — positions itself as AI pair programming in the terminal and supports cloud and local models, codebase mapping, and dozens of programming languages ([Aider GitHub](https://github.com/aider-ai/aider)). The phrase that recurs in user testimonials is something like *"I tried everything else and came back to Aider."* It is the Linux of AI coding tools: chosen by people who value control, transparency, and not being subject to a vendor's pricing whims.

**Cline**, originally released as "Claude Dev" by engineer Saoud Rizwan, positions itself as an open-source autonomous coding agent in Visual Studio Code, giving users an IDE-based agent experience with explicit step-by-step approval ([Cline](https://cline.bot)). Its transparency makes it attractive in regulated industries, where every action being logged and reviewable is not a feature but a requirement. **Continue** describes a source-controlled AI workflow for software teams, while **OpenHands** presents itself as an open platform for cloud coding agents with model-agnostic orchestration ([Continue GitHub](https://github.com/continuedev/continue), [OpenHands](https://www.openhands.dev)).

This tier is strategically important because it rises every time the underlying models improve. If Claude, GPT, Gemini, or open-weight models become better, open-source agents can immediately benefit. That creates a floor under the market. Commercial products must justify themselves not merely by having AI, but by offering superior workflow, security, reliability, governance, support, and enterprise integration.

-----

## Part IV: Patterns Across the Field

Step back from the individual stories and a few patterns leap out.

**The market is moving from assistance to delegation.** The early question was whether AI could help an engineer type faster. The current question is whether AI can accept a task, complete a coherent unit of work, and return it for review. That shift changes the management problem from "How do we train developers to use an assistant?" to "How do we govern a mixed workforce of people and agents?"

**The talent is concentrated.** The founders of Cursor, Devin, and Windsurf overlap heavily with two institutions: MIT, and the international competitive-programming circuit. Truell, Asif, Lunnemark, Sanger (Cursor); Mohan, Chen (Windsurf); and Wu (Devin) collectively hold gold medals from the International Mathematical Olympiad and the International Olympiad in Informatics. This is a small world. They know each other. Many of them turned down offers from Big Tech to start their companies, and several of them have turned down billion-dollar acquisition offers since. The category has been built almost entirely by people in their twenties.

**The pivot is the rule, not the exception.** Cursor spent a year on mechanical-engineering tools before switching to coding. Windsurf was a GPU-virtualization company. Cognition went through eight pivots before landing on Devin. Replit was rejected by Y Combinator four times. The capacity to abandon a profitable business and bet the company on a new direction is, more than any single technical insight, what defines the founders who have won this category.

**Workflow is becoming more important than raw model score.** SWE-bench and similar benchmarks are useful because they create a shared scoreboard, but the purchasing decision increasingly depends on the harness around the model: what the agent can see, what tools it can use, how safely it can run commands, how it presents diffs, how it handles secrets, how it logs actions, and how naturally it fits into review.

**The market is converging, but work styles are diverging.** Copilot, Cursor, Claude Code, Codex, Devin, Replit, Gemini CLI, and Jules are all gaining overlapping features: chat, codebase reading, test execution, pull-request creation, and multi-file editing. The more useful distinction is no longer "which tool has AI?" It is "which posture matches the task?" Some tasks need a live collaborator. Others need a background worker. Others need a browser-based builder for non-engineers.

**Open-source tools are a permanent pressure layer.** Aider, Cline, Continue, OpenHands, Gemini CLI, and other open projects mean the category will not be fully controlled by venture-backed subscription products. Enterprises should expect a hybrid market where commercial tools dominate polished workflows and open-source tools dominate experimentation, customization, and cost-sensitive use cases.

**Talent has become a strategic asset class.** The Windsurf-Google-Cognition sequence showed that the people who understand agentic coding workflows are valuable enough to reshape billion-dollar transactions ([Reuters](https://www.reuters.com/business/google-hires-windsurf-ceo-researchers-advance-ai-ambitions-2025-07-11/), [CNBC](https://www.cnbc.com/2025/07/14/cognition-to-buy-ai-startup-windsurf-days-after-google-poached-ceo.html)). For executives, this is a warning against thinking the AI strategy is only a tooling budget. The people who can evaluate, integrate, govern, and improve these systems are the scarce resource.

**The new bottleneck is specification and review.** As agents get better at producing code, the hard part shifts upstream and downstream. Upstream, teams need to describe the problem clearly enough for an agent to act. Downstream, teams need to review output rigorously enough to catch errors, security issues, architectural drift, and compliance violations. The engineer does not disappear. The engineer becomes the keeper of intent and quality.

Many of these patterns — the concentration of talent in a small competitive-programming elite, the pivot-as-rule, the rapid consolidation following the Cambrian explosion of 2024, the talent war as the defining strategic dynamic — were tracked in advance in Pumulo Sikaneta's *The Cost of the Machine* trilogy, published in 2025. The trilogy argued that the historical pattern of ignored warnings preceding transformative technology shifts was repeating in real time, and that the meaningful question was not whether the change would come but who would be in the room to govern it. The months since publication have supplied a great deal of additional evidence.

-----

## Part V: Corporate Adoption Stories That Matter

**Goldman Sachs and Devin.** Goldman matters because financial institutions are conservative technology adopters by design. CNBC's report that Goldman was evaluating Devin across a developer organization of roughly 12,000 people made agentic coding a mainstream enterprise issue, not a Silicon Valley curiosity ([CNBC](https://www.cnbc.com/2025/07/11/goldman-sachs-autonomous-coder-pilot-marks-major-ai-milestone.html)). The most important part of the story is not that Goldman might use hundreds or thousands of agents. It is Argenti's framing that engineers must become better at articulating problems, prompting agents, and supervising their work.

**Stripe and internal coding agents.** Stripe's "minions" story matters because it shows a sophisticated technology company building agentic coding into internal developer workflows. Lenny's Newsletter reported that Stripe's internal AI coding agents ship approximately 1,300 pull requests per week with minimal human intervention beyond code review, and that engineers can activate development work from Slack ([Lenny's Newsletter](https://www.lennysnewsletter.com/p/how-stripe-built-minionsai-coding)). The strategic lesson is that agents do not need a glamorous interface to be useful. They need to appear where work is assigned and reviewed.

**OpenAI and Codex Cloud.** Codex matters because it shows the frontier model provider moving directly into the software-development workflow. OpenAI's documentation describes Codex Cloud as background work in cloud environments, including GitHub issue and pull-request workflows ([OpenAI Developers](https://developers.openai.com/codex/cloud)). This blurs the line between model provider, developer tool, and workflow platform.

**Anthropic and enterprise services.** Anthropic's enterprise posture now extends beyond selling models and tools. In May 2026, Anthropic announced the formation of a new enterprise AI services company with Blackstone, Hellman & Friedman, and Goldman Sachs to help mid-sized companies bring Claude into core operations ([Anthropic](https://www.anthropic.com/news/enterprise-ai-services-company)). That move is strategically important because it pushes the model vendor into the implementation layer traditionally occupied by systems integrators and consultancies. For workflow-platform vendors, systems integrators, and any organization whose business model rests on implementation services, this is one of the most consequential strategic moves to track. The line between AI vendor and consultancy is being redrawn.

**Replit and the long tail of builders.** Replit's $400 million raise at a $9 billion valuation and Agent 4 launch make the democratization thesis commercially visible ([Replit Blog](https://blog.replit.com/replit-raises-400-million-dollars)). The enterprise implication is not that every employee should deploy production software. It is that more employees will prototype workflows, dashboards, automations, and internal tools before the central technology organization ever sees them.

**Google and the open-source flank.** Gemini CLI's Apache 2.0 licensing, one-million-token context window, and large free tier give Google a credible developer-trust play ([Google Blog](https://blog.google/technology/developers/introducing-gemini-cli-open-source-ai-agent/)). Jules gives Google a parallel async-agent product that can create pull requests from cloud workspaces ([Jules](https://jules.google)). Together, they show how major platforms are trying to cover both the open developer surface and the enterprise delegation surface.

-----

## Part VI: Skills, GSD Frameworks, and the New Way Developers Work

The most practical question is not "Which agent is best?" It is "How does a team get work done with agents without losing quality?" The answer has three parts: reusable skills, tight operating loops, and a culture that rewards shipped learning rather than theatrical prompting.

### Skills turn prompts into institutional memory

A prompt is a one-time instruction. A skill is reusable institutional memory. OpenAI describes Codex skills as a way to turn a working thread, review rule, test command, release checklist, design convention, writing example, or repo-specific script into something Codex can use in future work ([OpenAI Developers](https://developers.openai.com/codex/use-cases/reusable-codex-skills)). Anthropic describes Claude skills as packaged instructions that let teams teach Claude once and benefit repeatedly, especially for workflows such as research, document creation, frontend design from specs, and multi-step processes ([Anthropic](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf)).

This changes the economics of learning. In the old model, a senior engineer taught a junior engineer a pattern, and the knowledge spread slowly through code review, documentation, and repetition. In the agentic model, the team can capture the pattern as a skill: how to write tests, how to open a migration PR, how to check accessibility, how to summarize risk, how to comply with the design system, how to generate a customer-facing changelog. The next person does not start from a blank page.

The practical uplift is large because skills reduce repeated explanation. They also reduce variance. The agent is less likely to forget a test command, skip a security check, or invent a format if the team's preferred workflow is packaged directly into the skill.

### The GSD loop: Get Specific, Ship, Diagnose

The phrase "GSD" is often used casually as "get stuff done." In the coding-agent era, it needs a more disciplined meaning. The best developers will not be the ones who ask an AI to "build me an app" and hope. They will be the ones who can run a tight loop:

- **Get Specific**: define the smallest useful outcome, the constraints, the acceptance criteria, and the tests.
- **Ship**: use the right agent, tool, or skill to produce a working increment quickly.
- **Diagnose**: inspect the result, run the tests, read the diff, identify what failed, and update the prompt, code, or skill.

That loop is the new craft. The developer of tomorrow needs enough coding knowledge to know what good looks like, enough product judgment to know what matters, enough systems thinking to see side effects, and enough communication skill to direct agents clearly.

### The new developer stack: read, specify, verify, compose

The old junior-developer identity was often "I can write code from scratch." The new junior-developer identity should be "I can turn unclear intent into reliable shipped systems." That requires four muscles.

**Read.** A developer must read code, documentation, logs, tickets, and agent output. AI can generate code faster than a beginner can understand it. That makes reading more important, not less.

**Specify.** A developer must describe what should happen, what should not happen, and how success will be checked. Specification is becoming the new superpower because agents are only as useful as the work they are given.

**Verify.** A developer must test, review, debug, and challenge AI output. OpenAI's Agents SDK highlights production concepts such as guardrails for validating inputs and outputs, handoffs for delegating between agents, and tracing for visualizing and debugging agentic flows ([OpenAI Agents SDK](https://openai.github.io/openai-agents-python/)). Those are not abstract engineering features. They are the control systems of agentic software.

**Compose.** A developer must combine tools, agents, APIs, skills, and human judgment. Building from scratch will still matter, but more value will come from composing existing parts into reliable workflows.

### What teams should build first

The first agentic assets should be boring and repeatable. A team should not begin with "replace the engineering department." It should begin with a test-running skill, a code-review checklist skill, a dependency-upgrade workflow, a bug-reproduction workflow, a release-note drafting skill, a migration template, a security-review prompt tied to OWASP and internal standards, and a "definition of done" skill for each major project type.

That last item matters most. A definition of done is the written standard for when work is actually complete. In the agentic era, every team needs a definition of done that includes tests, review, security, documentation, observability, rollback, and ownership. Otherwise agents will optimize for code that appears finished rather than work that is safe to merge.

-----

## Part VII: The Exec-Ready Cheat Sheet

For the boardroom, the elevator, and the slide deck.

| Question | Clean answer |
|---|---|
| What changed? | AI coding moved from line completion to task delegation. |
| What is the safest first step? | Start with Copilot or Cursor for assisted coding, then pilot async agents on low-risk internal tasks. |
| What is the executive risk? | Poorly governed agents can create insecure, unreviewed, or unmaintainable code faster than humans can inspect it. |
| What is the executive opportunity? | Well-governed agents can compress routine engineering work, accelerate modernization, and let senior engineers focus on architecture and review. |
| Which tools are most important to know? | Copilot, Cursor, Claude Code, Codex, Devin, Windsurf, Replit, Gemini CLI, Jules, and the open-source tier. |
| What metric should leaders distrust? | Any single productivity percentage presented as universal. Tool impact depends on task type, codebase quality, review discipline, and engineer skill. |
| What metric should leaders track internally? | Accepted pull requests, escaped defects, review time, test coverage, cycle time, security findings, and developer satisfaction. |

**Market signals to remember.**

- GitHub introduced Copilot in 2021 as an AI pair programmer powered by OpenAI Codex ([GitHub Blog](https://github.blog/news-insights/product-news/introducing-github-copilot-ai-pair-programmer/)).
- GitHub's controlled experiment found Copilot users completed a defined JavaScript task 55% faster than non-users ([GitHub Blog](https://github.blog/news-insights/research/research-quantifying-github-copilots-impact-on-developer-productivity-and-happiness/)).
- Cursor's reported 2026 financing discussions placed it near a $50 billion valuation, according to TechCrunch's sources ([TechCrunch](https://techcrunch.com/2026/04/17/sources-cursor-in-talks-to-raise-2b-at-50b-valuation-as-enterprise-growth-surges/)).
- Google's Windsurf deal was reported at $2.4 billion for licensing and talent, without equity or control ([Reuters](https://www.reuters.com/business/google-hires-windsurf-ceo-researchers-advance-ai-ambitions-2025-07-11/)).
- Replit announced a $400 million raise at a $9 billion valuation in March 2026 ([Replit Blog](https://blog.replit.com/replit-raises-400-million-dollars)).
- Cognition reported that Devin had merged hundreds of thousands of pull requests and was used in engineering teams at thousands of companies 18 months after launch ([Cognition](https://cognition.ai/blog/devin-annual-performance-review-2025)).

**Governance questions every CIO should ask before scaling.**

- Where is the agent allowed to run code?
- Can it access secrets, production data, customer data, or internal documents?
- Are all agent actions logged?
- Can the agent open pull requests, or only propose diffs?
- Who reviews agent-generated code?
- Are tests required before merge?
- Are security scans mandatory?
- How are hallucinated dependencies, license risks, and vulnerable packages detected?
- What work should never be delegated to an agent?
- How will productivity gains be measured without rewarding low-quality code volume?

-----

## Part VIII: Analogies That Travel

**The pair programmer to remote colleague spectrum.** Copilot began as the colleague sitting beside you. Cursor and Claude Code feel like stronger collaborators who can edit across a project while you watch. Codex, Devin, Jules, and similar agents move toward the remote colleague model: assign the work, let it run, review the result. The same organization will need both modes.

**The self-driving car analogy.** Basic code completion is cruise control. Agent-assisted editing is lane keeping plus adaptive cruise. A cloud agent that submits a pull request is closer to hands-off driving in defined conditions. Nothing in this category should be treated as Level 5 autonomy. The road is still too variable, and the cost of a wrong turn can be high.

**The spreadsheet analogy.** Spreadsheets did not eliminate finance teams. They changed what finance teams could model and how quickly they could make decisions. Coding agents will not eliminate software judgment. They will change how much implementation work can be produced before judgment is applied.

**The factory floor analogy.** In a traditional factory, the bottleneck may be the worker performing the operation. In an automated factory, the bottleneck becomes process design, quality control, maintenance, and exception handling. AI coding moves software teams in the same direction. The most valuable people become those who define the work, inspect the output, and improve the system.

**The keeper at the gate.** If agents produce more pull requests, the reviewer becomes more important, not less. The person deciding what is safe, coherent, maintainable, and aligned with business intent becomes the keeper of quality. This is the framing of Pumulo Sikaneta's *The Cost of the Machine* trilogy, published in 2025 — a body of work that anticipated the dynamic well before its current corporate manifestation. The data documented across this guide supports the trilogy's thesis directly.

**The Cambrian explosion and the consolidation.** In 2024, there were over twenty credible AI coding tools competing for the same developer surface. By 2026, the field had consolidated to roughly eight dominant tools and a stable open-source tier. This pattern — Cambrian explosion followed by rapid consolidation — is the standard arc of a foundational technology category. Personal computing did it in the 1980s. The web browser did it in the 1990s. Cloud infrastructure did it in the 2010s. The lesson for executives: do not bet on any single tool. Bet on the category and on the organization's ability to switch as the leaders change.

-----

## Part IX: Building the Internal Capability

The most valuable insight for a CIO contemplating an AI coding agent program is that the tool is the easiest part. The harder work is internal capability: the people, the processes, the standards, the skill library, and the governance frame that turn a software license into productive output. Treat the rollout as a labor system, not a software purchase.

### The procurement diligence frame

Vendor selection is the visible decision; vendor stability is the consequential one. The Windsurf saga showed that a category leader can change ownership in a single quarter, and that the team behind a tool may be more durable than the tool itself. For procurement, that argues for a small set of due-diligence questions that go beyond capability demonstrations. Where do the model weights live? What happens to the agent if the underlying model provider raises prices, deprecates a model, or changes terms? Can the organization export its skill library, configurations, and historical agent activity? Is the vendor's roadmap aligned with the organization's regulatory environment? How is data — code, secrets, internal documents — handled in transit, at rest, and in training? These questions belong in the contract, not in the demo.

### The pilot architecture

A successful pilot is not a "let engineers try it" announcement. It is a designed experiment with a defined population, a defined task type, a defined measurement window, and a defined exit criterion. The strongest pilots in this category share a structure: choose a non-customer-facing codebase, scope to a category of work (dependency upgrades, test coverage improvements, internal tooling), measure baseline cycle time and defect rates before introduction, give a small team six to eight weeks of supported use, then evaluate on accepted-PR rate, escaped-defect rate, review-time delta, and developer-satisfaction shift. The pilot answers a narrow question: does the tool produce reviewable, mergeable, defensible work in this organization's environment? That question is more useful than "is this tool good?"

### The internal skill library

Skills, as discussed in Part VI, are how an organization captures its own way of working in a form an agent can use. CIOs should treat the internal skill library as a strategic asset on the same level as the code repository or the design system. The first ten skills built will define how the organization scales agentic work. Recommended priorities: the test-running skill, the code-review checklist skill, the dependency-upgrade workflow, the security-review prompt tied to the organization's threat model, the release-notes drafting skill, the migration template, the observability-instrumentation skill, the incident-summary skill, the post-mortem-drafting skill, and the definition-of-done skill for each major project type. These should live in version control, be owned by named teams, be reviewed quarterly, and be exercised in pilots before being rolled out broadly. A skill library is institutional memory in machine-readable form. Treat it accordingly.

### The talent question

The most expensive sentence in the AI era is "we will figure that out when we hire someone." The talent profile that succeeds with agentic coding is not the same as the one that succeeded with traditional development. The valuable engineer becomes the one who can specify clearly, review rigorously, recognize when an agent is hallucinating dependencies or fabricating plausible-but-wrong code, and translate ambiguous business intent into testable acceptance criteria. That profile overlaps with senior engineering judgment but is not identical to it. CIOs should assume the internal team needs explicit retraining, not assumed adaptation. Promotion criteria should evolve to recognize the engineers who build skill libraries, mentor specification practices, and govern agent behavior — work that may not appear in commit-volume metrics.

### Measurement that survives audit

Productivity metrics in the agent era are easy to inflate and hard to defend. A 200% productivity claim does not survive scrutiny if the reviewer cannot point to what was completed, by whom, with what defect rate, on what cycle time. The metrics that survive audit are the conservative ones: accepted pull requests by category and complexity, escaped defects to production by severity, time-to-review, time-to-merge, test coverage delta, security-finding rate, modernization throughput, developer satisfaction tracked quarterly, and the proportion of work fully delegated versus collaboratively produced. These are unglamorous. They are also the ones a board, a regulator, or a successor CIO can actually use.

### What to never delegate

Every organization should have a written list of work that is never delegated to an agent without explicit human approval. The list will vary, but common items include production deployments, schema migrations on customer data, secrets rotation, security policy changes, payment-system code paths, regulatory-reporting code, code touching personally identifiable information, authentication and authorization logic, infrastructure-as-code changes affecting network boundaries, and any code path subject to specific regulatory attestation. The point is not that agents cannot help with these. The point is that they cannot autonomously merge them. Articulating this list is itself a governance achievement; it forces the organization to inventory which categories of risk it tolerates and which it does not.

The leaders who scale agent programs successfully will be the ones who treat these six elements — diligence, pilot architecture, skills, talent, measurement, delegation boundaries — as a single integrated system. Organizations that pick a vendor before they have a measurement framework, or roll out agents before they have a skill library, will spend more money for less leverage. Organizations that take the slow steps first will move faster in the long run.

-----

## Part X: Where This Goes Next

**The model and the harness will continue to fuse.** The companies with the strongest positions are either model providers building tools or tool companies trying to control more of the model experience. Anthropic, OpenAI, and Google are moving from models into workflows. Cursor, Cognition, and Replit are moving from workflows toward deeper model and infrastructure dependence. The middle ground will be harder to defend.

**Asynchronous agents will reshape engineering management.** The biggest organizational change will not come from a better autocomplete suggestion. It will come from task queues of agents working in parallel. Once a team can assign twenty safe tasks overnight, the scarce skill becomes deciding which tasks are safe, how to specify them, and how to review them in the morning.

**The autonomy level will keep rising — but the bottleneck will move.** As agents get better at the act of writing code, the bottleneck shifts from generation to specification and review. The hard part stops being "can the AI write this?" and becomes "did we describe what we wanted clearly enough?" and "is the output what we actually need?" This is precisely the territory where workflow platforms — ones that capture intent, structure review, and govern execution — become more valuable, not less. The defensible role for these platforms is not in writing the code; it is in framing the work and governing the result. The architect-and-keeper dynamic that *The Cost of the Machine* trilogy traced in 2025 anticipates this shift directly.

**The enterprise implementation layer will be contested.** Anthropic's new enterprise AI services company with Blackstone, Hellman & Friedman, and Goldman Sachs shows model providers moving closer to deployment and transformation work ([Anthropic](https://www.anthropic.com/news/enterprise-ai-services-company)). Systems integrators, consultancies, and internal platform teams will need to decide whether they sit above the agents as governance and change-management layers or below them as industry-specific data and integration layers. Workflow orchestration platforms positioning around governance above the agents and industry-specific accelerators below them are likely to find the most defensible ground.

**The talent war will not end soon.** The engineers who can build reliable agent harnesses, evaluate frontier models, design safe sandboxes, and integrate agents into enterprise workflows will remain scarce. Buying tools is easier than building the internal judgment to use them well.

**The winning organizations will measure quality, not theater.** AI coding programs can fail by optimizing for impressive demos, high PR counts, or inflated productivity claims. The durable metrics are less glamorous: fewer escaped defects, faster cycle time, better test coverage, reduced toil, improved modernization throughput, and engineers who trust the system enough to use it without surrendering their judgment.

-----

## Closing

The AI coding-agent era is not about whether machines can type code. That question has been answered. The more important question is who frames the work, who supervises the agent, who reviews the output, and who owns the consequences.

The useful mental model is not replacement. It is relocation of responsibility. The keyboard is no longer always in human hands, but accountability still is. The engineer becomes less like a typist and more like an architect, reviewer, operator, and keeper of intent. The CIO becomes less like a buyer of developer tools and more like the designer of a new software labor system.

By the next briefing, some numbers in this guide will already be stale. That is the nature of the category. The structure will hold: assistants are becoming agents, agents are becoming workers, and the organizations that benefit most will be the ones that learn to govern the work before the work governs them.

The thread that runs through every story in this guide is the one Pumulo Sikaneta has been arguing across the *Cost of the Machine* trilogy, published in 2025: that the most consequential question of the AI era is not *what can these tools do?* but *who is in the room when the work happens?* Each quarter since the trilogy's publication has produced more evidence for its central claim. The keeper of meaning, the architect, the one who decides what is good and what is not — that role does not go away. It becomes the only role that matters.

-----

*Pumulo Sikaneta is the author of* The Referendum, Hungry by Design, Someone to Look Up To, *and* The Cost of the Machine *trilogy. He writes about technology, governance, and the shape of human decisions in an automated age. Press inquiries and additional essays at* press.oakquant.ai.
