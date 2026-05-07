![A keyboard at twilight on a wooden desk, lit by a brass lamp, with a coffee cup and an open notebook nearby](images/IMG_0014.jpeg)

## Overture: The Window That Rewrote Software

In June 2021, the CEO of GitHub — a soft-spoken venture investor named Nat Friedman — wrote a short blog post announcing something called Copilot. He described it as an "AI pair programmer." A pair programmer, in software culture, is a colleague who sits next to you while you code; one of you drives the keyboard, the other watches and thinks. Friedman was suggesting, with measured language, that the colleague could now be a machine.

That phrase — "AI pair programmer" — became the seed of an entire industry. Less than five years later, that industry has produced companies worth tens of billions of dollars, has rewritten the daily work of millions of engineers, and has triggered the most expensive talent war in software history. As of mid-2026, four products dominate the conversation, several others sit just outside the front rank, and a small constellation of open-source tools quietly powers a meaningful slice of the work.

This guide is built to be read once and consulted whenever a fact, a story, or a clean framing is needed. It assumes nothing about prior knowledge. Every acronym is unwrapped on first use. Every founder is given a face and a backstory. Every claim that an executive might want to challenge has a number behind it.

The structure is deliberate. Part I provides the vocabulary. Part II tells the story of each major product as if it were a character in a novel — origin, motive, voice, where it stands today. Part III steps back and looks at the patterns. Part IV walks through the corporate adoption stories that matter most for boardroom conversations. Part V is a cheat sheet of numbers. Part VI offers analogies that translate directly into talks and briefings. Part VII looks at where this is going.

Read cover to cover, this guide offers a clearer view of the category than most executives currently hold.

A note on positioning. My professional work is concentrated in the Pega ecosystem, which is one of the workflow platforms whose role this guide touches on briefly in Part VII. The field-guide treatment of the AI coding tools themselves is presented as the analyst community describes them, with no Pega-specific advocacy inside the vendor walkthroughs. The deeper argument for why the workflow and governance layer becomes more valuable rather than less as agentic coding matures is published separately as [*The Analyst Consensus on Agentic AI Architecture*](https://press.oakquant.ai/public/articles/the-analyst-consensus), also at press.oakquant.ai.

---

## Part I: The Vocabulary

The category is dense with terms that sound similar and mean different things. The clearer the terms, the more confident the audience.

**Large language model (LLM).** A type of AI system trained on enormous amounts of text, including computer code. When prompted, the model produces a response one piece at a time, choosing each next word based on patterns it learned during training. GPT-5, Claude Opus, and Gemini are all LLMs. The model is the engine. Everything else — the chat window, the IDE, the agent — is the car built around it.

**IDE (integrated development environment).** The application a software engineer writes code in. Microsoft Visual Studio Code, JetBrains IntelliJ, and Apple Xcode are the dominant ones. Think of it as the equivalent of Microsoft Word for writers, but with extra tools for running and debugging code.

**CLI (command-line interface).** The black-text-on-blank-screen window where engineers type commands directly to their computer. The terminal. It is older than graphical interfaces and remains the preferred environment for many serious engineers because it is fast, scriptable, and stays out of the way.

**Plugin or extension.** A small add-on that lives inside a host application — a Word add-in, a Chrome extension, a VS Code extension. Many AI coding tools started as plugins inside an existing IDE.

**Fork.** When a team takes the source code of an existing open project and starts a new project from that base, modifying it for their own purposes. Cursor is a fork of Microsoft's Visual Studio Code editor.

**Autocomplete or code completion.** The original AI-coding capability. As the user types, the tool suggests the next word, line, or block of code. Hit Tab to accept. The user stays in the driver's seat the whole time.

**Agent.** This is the word that changed everything. An agent does not just suggest. It plans, takes multiple actions in sequence, runs tests, fixes its own mistakes, and reports back. Give it a goal — "find the bug that's breaking the login screen and fix it" — and walk away. It might take five minutes or thirty. The difference between an autocomplete and an agent is the difference between a calculator and an accountant.

**Synchronous vs. asynchronous.** Synchronous means in-the-moment — the user watches the AI work, step by step, and steers. Asynchronous means fire-and-forget — assign a task and check back later for the result, the way one would assign a task to a remote colleague over Slack.

**Sandbox.** An isolated, disposable computing environment where an AI can experiment without touching real systems. If it breaks something inside the sandbox, the sandbox just gets thrown away.

**Pull request (PR).** In software, edits do not usually go directly to the main version of a project. Changes are written on a side branch and a pull request is submitted asking the team to review and merge them. PRs are the unit of contribution. When people say an AI "produces a pull request," they mean it has done a complete, reviewable piece of work.

**SWE-bench.** Pronounced "S-W-E bench." Short for *software engineering benchmark*. It is the de facto exam used to compare these tools. Researchers gathered hundreds of real bug reports from popular open-source projects and ask each tool to fix them. The score is the percentage solved. SWE-bench Verified is the cleaned-up, human-checked version of the test. As a frame of reference, in 2023 the best models scored under 5%. By late 2025, several were over 80%.

**MCP (Model Context Protocol).** A standard, introduced by Anthropic in late 2024, that lets AI tools plug into external systems — databases, ticketing platforms, document stores — through a common interface. Before MCP, every integration was custom. After MCP, an AI agent can ask "what's in my Linear ticket queue?" the same way regardless of who built it. It is one of the most consequential pieces of plumbing in the field.

**ARR (annual recurring revenue).** The total revenue a software business is on track to generate over the next twelve months at its current run rate. ARR is the metric venture investors care about more than any other. A company at $1B ARR is at a scale comparable to a mature, public software business.

**Vibe coding.** A term coined in early 2025 by Andrej Karpathy, an OpenAI co-founder. It describes the new style of building software by talking to AI rather than typing code — describing what is wanted, watching it appear, redirecting when it goes wrong. Karpathy, with characteristic dryness, said the developer "fully gives in to the vibes, embraces exponentials, and forgets that the code even exists." The phrase stuck.

With those terms in hand, the players come into focus.

---

## Part II: The Eight Players

### 1. GitHub Copilot — The Original

The story of AI coding for a general audience starts here. Copilot was the first tool to put this kind of capability in front of millions of working engineers, and it is still — by usage, if not by buzz — the most widely deployed.

The origin story has a particular character to it because of who was running GitHub at the time. Nat Friedman is one of the more thoughtful figures in the open-source world. He had spent years investing in developer tools before Microsoft acquired GitHub and put him in charge. When OpenAI showed him an early version of a model trained specifically on source code — a model called Codex, built on top of the GPT-3 family — he immediately saw the implications.

Friedman launched Copilot as a technical preview on June 29, 2021. The original was simple: it lived as an extension inside Visual Studio Code, watched what the user typed, and offered to autocomplete the next few lines. He framed it explicitly as a colleague: an "AI pair programmer." That framing was deliberate. Friedman did not want to argue that AI would replace engineers. He wanted to argue that this was the third great wave of programmer productivity — first compilers, then open source, now AI.

The early reception was a mixture of awe and panic. Engineers on Hacker News and Reddit asked the obvious question: *can this replace our jobs?* Friedman responded directly in the comments, calmly, several times — a CEO behavior that today feels almost unimaginable. He also defended the legality of training the model on public code by stating that "training ML systems on public data is fair use," a sentence that would later be tested by a class-action lawsuit filed in November 2022 and that remains an active question in court.

By 2023, GitHub was claiming Copilot was already writing 46% of code in the files where it was active and helping developers code 55% faster. Whether one believed those exact numbers or not, the trajectory was clear.

Copilot's defining strategic move came later. For its first three years, it was tied to a single OpenAI model. Then in 2024, GitHub did something unusual for a Microsoft-owned property: it opened up. Today, Copilot users can choose their underlying model from a list that includes OpenAI's GPT family, Anthropic's Claude, Google's Gemini, and xAI's Grok. In February 2026, GitHub extended access to Claude and Codex models across all paid tiers. The bet is that being a *neutral platform* is more valuable than being locked to one model.

The product itself has expanded far beyond autocomplete. Copilot now has a chat interface, a coding agent that can be assigned issues directly from a GitHub repository, a command-line tool, and a code-review function. The new CEO of GitHub, Thomas Dohmke, has been pushing the platform toward what he calls "agentic workflows" — meaning Copilot doesn't just complete code, it can take a ticket, write the fix, run the tests, and submit the pull request.

**Why it stands out.** Reach. Copilot is integrated into Microsoft's distribution machine. It runs in VS Code, Visual Studio, JetBrains products, Neovim, and now even on github.com itself. For a company that has standardized on the Microsoft ecosystem, it is the path of least resistance. The free tier and the $10-a-month entry price make it the easiest first step into AI coding for any team.

**The honest critique.** Copilot is rarely the most exciting tool in any given comparison. Power users routinely move to Cursor or Claude Code once they want something more autonomous. But Copilot's role is to be the on-ramp — to take a developer who has never used AI and give them a usable, low-risk experience. In that role, nothing else competes.

---

### 2. Cursor (Anysphere) — The Dropouts Who Built a Thirty-Billion-Dollar Company

If Copilot is the establishment, Cursor is the upstart that almost ate it.

The founders are four MIT students — Michael Truell, Sualeh Asif, Arvid Lunnemark, and Aman Sanger — who met through MIT's Computer Science and Artificial Intelligence Laboratory (CSAIL). All four were intensely accomplished. Truell had built one of the most popular online programming games at age fourteen. Asif had grown up in Karachi, Pakistan, and represented his country at the International Mathematical Olympiad. Lunnemark won medals at the International Olympiad in Informatics, the same elite competition that produced Devin's CEO. Sanger had worked on medical AI and large-scale machine learning before MIT.

In 2022, all four turned down the kind of jobs that MIT computer science graduates routinely receive — Google, hedge funds, FAANG offers — and incorporated a company they called Anysphere. The name was a mathematician's joke; it referred to a hypothetical mathematical set containing "any sphere." The product they would later build, Cursor, took its name from the blinking cursor in a code editor — the small flashing line that marks where the next character will appear. The conceit was that an AI should be able to predict and generate what should come next.

The first year was, in Truell's own description, "wandering in the desert." The team initially tried to build an AI tool for *mechanical* engineers — software for designing physical objects. They picked the space, Truell said in a Y Combinator interview, partly because it was "sleepy and uncompetitive." It also turned out to be sleepy because it was small, and the four of them lacked deep domain expertise. They flailed.

The pivot, when it came, was decisive. Truell later told it this way: *"We were obsessed with AI's potential to change software development. But existing tools like GitHub Copilot weren't pushing the limits. We realized AI should not just assist coding — it should be the foundation of how developers work."* The crucial word in that sentence is *foundation*. They were not going to build a plugin. They were going to build their own editor, with AI baked into the core.

So they did. Cursor is technically a fork of Microsoft's Visual Studio Code — meaning they took Microsoft's open-source editor as their starting point and rebuilt the AI parts from scratch. This gave them the familiar look and shortcuts that millions of developers already knew, while letting them control the AI experience completely.

What followed is one of the most extraordinary growth stories in the history of software. Cursor's seed round in October 2023 was $8 million, led by OpenAI's own startup fund. Their Series A, in August 2024, was $60 million at a $400 million valuation. By November 2025, they had raised $2.3 billion in a Series D round at a $29.3 billion valuation. By early 2026, they had passed $2 billion in annual recurring revenue. One analysis described Cursor as the fastest B2B software company ever to reach $1 billion in ARR.

To put that in perspective: they doubled revenue every two months for a stretch of 2025. They went from $100 million in ARR to $500 million in ten months. As of the most recent disclosures, more than fifty thousand businesses are paying Cursor — including Nvidia, Adobe, Uber, and Shopify — and the platform has more than a million daily active users.

In April 2026 came the news that captured how seriously Silicon Valley was taking the company. SpaceX announced an option to acquire Cursor for $60 billion later in the year, with a $10 billion fee if the deal didn't close. OpenAI had, earlier, reportedly tried to acquire Cursor outright at a multi-billion-dollar price and been turned down.

**The human experience.** Using Cursor feels like Visual Studio Code with a much smarter colleague reading over the shoulder. Highlight a piece of code and ask, in plain English, "make this faster" or "add error handling here." Tell it to build a whole feature, and it will edit several files at once while the developer watches. The interface preserves the comfort of a standard editor while folding the AI in seamlessly. Truell has been almost obsessive about *speed* — the team built advanced caching systems specifically to make sure the AI feels instant rather than laggy. This is one reason Cursor feels different from many competitors: it does not feel slow.

**Why it stands out.** Cursor sits at the sweet spot of accessibility and power. It looks like an editor a developer already knows. It has agentic capabilities. It has model choice — pick GPT, Claude, or other models, and switch them depending on the task. It has 360,000 paying individual users, an enormous developer community, and an aggressive enterprise motion.

**The honest critique.** Cursor's pricing model uses a credit system that has surprised heavy users with bigger bills than they expected. Independent benchmarks have shown that for the same task, Cursor's architecture can use roughly 5.5 times more tokens (units of text, which determine cost) than Anthropic's Claude Code. The company has been working on this, but the credit-billing controversy of mid-2025 caused real trust damage that has not fully repaired.

---

### 3. Claude Code (Anthropic) — The Side Project That Became the Standard

Of all the stories in this category, Claude Code is the one that most resembles a startup myth told inside a big company.

The protagonist is Boris Cherny, a software engineer with a fairly winding path. He was born in Odessa, Ukraine. His grandfather was one of the first programmers in the Soviet Union, working with punch cards (the old physical media that programs used to be stored on). When Boris's mother was a child, she would draw on those punch cards because to her, they were just paper; her father, meanwhile, was using them to write some of the earliest software in his country. The family moved to the United States in 1995. Cherny started his first startup at eighteen, has no computer science degree (he studied economics before dropping out), and spent a decade at Meta — working at Facebook and Instagram and rising to principal engineer.

He joined Anthropic in September 2024. Anthropic, for those unfamiliar, is the AI safety company founded in 2021 by Dario and Daniela Amodei and several other researchers who had left OpenAI. Anthropic builds the Claude family of models. Cherny joined as an engineer wanting to get familiar with Claude through Anthropic's public API.

He started, in his own words, "hacking around using Claude in the terminal." His first prototype could read what music he was listening to on his Mac and change the song based on his input. It could not read files. It could not write code. It was mostly a toy.

But then he had a conversation with Cat Wu, a product manager researching how AI agents could use computers. The conversation reshaped the prototype. Cherny gave the terminal more capabilities — file access, the ability to run commands. He showed it to colleagues. They started using it. Within five days of an internal release in November 2024, half of Anthropic's engineering team was using it.

Boris's manager, Ben Mann, had given him a piece of advice that would define the project: *"Don't build for today's model, build for the model six months from now."* That meant the early version felt mediocre — but when Anthropic's Claude 4 model series launched in 2025, the tool's capabilities took a quantum leap. Suddenly the same harness was producing strikingly competent work.

Claude Code was launched publicly as a research preview on February 24, 2025, alongside Claude 3.7 Sonnet. It went generally available on May 22, 2025. By six months after general availability, it was at $1 billion in annualized run-rate revenue — faster than ChatGPT's revenue ramp. By March 2026, it had passed $2 billion. As of early 2026, more than 80% of Anthropic's own engineers used it daily, and Cherny would later note that on any given day, 80–90% of Claude Code itself is now written by Claude Code. The product writes its own product.

The unofficial moment when Claude Code crossed from "interesting" to "watershed" was in November 2025, when Anthropic released Claude Opus 4.5 — the first model to break the 80% barrier on the SWE-bench Verified benchmark, with a score of 80.9%. One observer, Sergey Karayev, captured the mood: he called it "the Gutenberg press. The sewing machine. The photo camera." That kind of hyperbole has been more common in this field than is strictly responsible, but in this case, the data behind it was real.

The other story that became a defining anecdote: a team of four people at Anthropic, using Claude Code, built an entirely new product called Claude Cowork in a one-and-a-half-week sprint. When asked how much of it Claude Code wrote, Cherny answered: *"All of it… It was not zero intervention! We had to plan, design, and go back and forth with Claude. Claude wrote all the code."*

**The human experience.** Claude Code lives in the terminal. It does not have a graphical interface. The user opens a folder containing a project, types `claude` at the command line, and a conversation begins. The user describes what is wanted. Claude Code reads the files, plans an approach, makes changes, runs tests if they exist, and shows what it did. The user either accepts or pushes back. Engineers who switch to it from IDE-based tools often report a kind of weight-loss feeling — the visual clutter is gone, and engineer and AI are talking in plain English about the codebase. It is more demanding of focus than Cursor and gives less visual feedback. In return, it tends to use fewer tokens, produce more precise edits, and feel like it is genuinely thinking.

**Why it stands out.** Claude Code consistently leads the SWE-bench Verified benchmark — the most-cited measure of real-world coding skill. It has the deepest support for MCP, the protocol that lets AI agents talk to other tools. It has the largest configuration depth — features like sub-agents (small specialized AIs that handle subtasks), custom hooks, and slash commands give power users an enormous amount of control. And the loop is end-to-end owned: the same company trains the model and builds the tool, optimizing both together.

**The honest critique.** Heavy users routinely report bills of $100–200 per month, and that's on plans where they are not paying per call. The terminal-only experience is unfamiliar territory for engineers used to graphical IDEs. Anthropic has been catching up here with IDE plugins and a web app, but the philosophical home of Claude Code remains the terminal, and that is a deliberate choice that excludes some users.

---

### 4. OpenAI Codex — The Comeback

There are two things called Codex in OpenAI's history, and confusing them is one of the most common errors a non-specialist makes.

The *original* Codex, launched in 2021, was the model that powered GitHub Copilot. It was an early code-specialized version of GPT-3. OpenAI shut it down in 2023.

The *current* Codex, launched as a CLI tool on April 16, 2025 and as a cloud-based agent platform on May 16, 2025, is something completely different. It is a full coding agent platform powered by OpenAI's frontier models — currently the GPT-5 family, including specialized variants like GPT-5.3-Codex. As of April 2026, OpenAI's CEO, Sam Altman, confirmed that Codex had roughly four million weekly active users. By an enterprise update later in the year, the number climbed to roughly five million.

Codex has a multi-surface design. It is, all at once: a command-line tool (the open-source Codex CLI, written in Rust and made freely available under the Apache 2.0 license — meaning anyone can read, modify, or build on the source code); a web application accessible from inside ChatGPT; a Mac desktop app; a Slack integration where users can mention `@Codex` in a thread; and a software development kit (SDK) that lets developers build their own tools on top of it. The Rust-based CLI alone has more than sixty thousand stars on GitHub, the platform's measure of community attention, and hundreds of contributors.

The execution model is distinctively cloud-first. When a user gives Codex a task, it can spin up an isolated cloud sandbox preloaded with the code, do the work in there, and submit a pull request. The user doesn't watch it work in real time. They assign a task — fix a bug, write a feature, refactor a module — and check back in five to thirty minutes for the result. This is the asynchronous archetype.

The strategic advantage Codex has, beyond the model quality, is that the same company building the tool is training the underlying model. OpenAI can tune the GPT models specifically for the kinds of long-running, multi-step coding tasks Codex needs. They can also offer something that no competitor can: in late 2025, OpenAI deployed a specialized variant called GPT-5.3-Codex-Spark on Cerebras WSE-3 hardware — a different chip family from the Nvidia GPUs that everyone else uses — and it can produce more than a thousand tokens per second. That is roughly three to five times faster than typical AI output.

Codex is also where OpenAI's broader product strategy lands. In March 2026, OpenAI launched an in-Codex application security agent that analyzes a codebase, builds a threat model, and looks for vulnerabilities. That kind of vertical extension — taking the agent and pointing it at a specific corporate problem like security review — is the playbook for the next year.

**Why it stands out.** Codex offers the strongest fire-and-forget asynchronous workflow on the market. The pricing is aggressive: a $20 ChatGPT Plus subscription gets more sessions than the equivalent Claude plan. The tool ships with five reasoning levels that can be dialed in — minimal, low, medium, high, very high — letting users trade speed against thoughtfulness depending on the task. And the open-source CLI is unique among frontier-lab flagships: anyone can read, audit, and modify the code that drives the agent.

**The honest critique.** Codex routinely sits second on SWE-bench Verified, behind Claude Code. Configuration depth lags Claude's hooks-and-sub-agents system. The async-first design creates friction for engineers who want to steer in the moment. And, like Claude Code, Codex does not let users bring their own model — they are inside OpenAI's walls.

---

### 5. Devin (Cognition) — The Autonomy Bet

Devin is the most controversial product in this category, and its founder is the most extraordinary individual story.

Scott Wu was born in 1997 in Louisiana to a Chinese immigrant family. He attended Baton Rouge Magnet High School. By the time he was a teenager, he was one of the strongest competitive programmers in the world. He won three gold medals at the International Olympiad in Informatics — the high-school world championship of computer programming — and placed first overall in 2014. He came third in Google's 2021 Code Jam, a global elite competition. He achieved Legendary Grandmaster status on Codeforces, the platform where the world's best competitive programmers train.

He went to Harvard, dropped out after two years, and co-founded a startup called Lunchclub, which used AI to match professionals for one-on-one networking conversations. In 2023, he started Cognition AI with Steven Hao and Walden Yan. Wu's brother Neil Wu, also a competitive programming champion, joined the team.

Cognition's pitch to investors was simple and audacious: we are not building a coding assistant. We are building an actual AI software engineer — one that can be assigned a ticket the way you would assign one to a human, and that will return a finished, reviewed pull request hours later. The implication was direct: the goal is to put a serviceable junior engineer into a Slack channel.

They named the product Devin, after Devin Smith, an early engineer at Cognition. The name was loaded — a person's name, not a feature description — and that was deliberate. They wanted users to think of it as a colleague. Cognition raised $21 million in a Series A led by Peter Thiel's Founders Fund.

When they unveiled Devin in March 2024, the demo went viral. Stripe co-founder Patrick Collison called it "very impressive in practice." Other engineers were nervous; the social-media discourse swung between awe and predictions of mass unemployment.

Devin is the most autonomous product in the category. A user assigns it a task through Slack, Linear (a popular project management tool), or GitHub. It works in a fully sandboxed cloud environment with its own browser, terminal, and code editor. It takes minutes to hours. It produces a pull request. It is async, end-to-end. The product was originally priced at $500 per month — a price that signaled, deliberately, that this was an enterprise tool. In 2025, Cognition cut the price to $20 a month plus consumption-based billing, opening it up to individual developers.

The subsequent corporate adoption story is what made Cognition matter. In July 2025, Goldman Sachs's CIO, Marco Argenti, announced that the bank — which employs roughly 12,000 human developers — would begin deploying Devin alongside them. He framed it as a "hybrid workforce" targeting 20% efficiency gains, the equivalent of adding 2,400 developers without hiring anyone. By the time Cognition closed its September 2025 funding round, Devin and its sister product Windsurf were in production at Citi, Dell, Cisco, Ramp, Palantir, Nubank, Mercado Libre, and NASA. Cognition's annual recurring revenue grew approximately seventy-three times in nine months. Enterprise usage grew roughly eighty-fold over the past year.

By April 2026, Cognition was valued at $10.2 billion and reportedly worth more after subsequent rounds. Wu, asked by the venture-capital newsletter Lenny's about how his own engineering team works, said that each of Cognition's roughly fifteen engineers operates with about five Devins each. They are, in Wu's framing, no longer bricklayers but architects — they design and review, the Devins do the bricklaying.

A telling cultural detail: at Cognition's San Francisco headquarters, visitors take off their shoes at the door. The team keeps Allbirds-branded slippers in a basket. The ritual was a holdover from the company's earlier home in Atherton, a wealthy California suburb where Cognition operated out of a $10.5 million neoclassical mansion, sleeping in the bedrooms and turning the basement into a pit of monitors. *"These next three years are what we're going to tell our grandkids about,"* one early investor, Emily Cohen, said. She had quit her venture-capital job to join Cognition full-time.

**The human experience.** Devin is the least like talking to a tool and the most like talking to a colleague. Users do not see it work. They message it. It messages back when it has something to share. The design choice is calculated: if Devin acts like an engineer, users learn to manage it like one — give it context, give it a clear task, expect a thoughtful question or two before it starts.

**Why it stands out.** Pure autonomy. Devin is the cleanest expression of the bet that AI software engineers will operate as actual workforce members, not tools. The enterprise traction at Goldman, Citi, and others has given it a financial-services anchor that is unusual for an AI-native product.

**The honest critique.** Independent researchers in 2024 found that the early version of Devin struggled with complex coding tasks — failing more often than its demo videos suggested. The 2.0 and later versions have closed much of that gap, but Devin is still better at well-scoped tasks (upgrade this dependency, fix this bug, migrate this module) than at open-ended ones (design this system from scratch). And on SWE-bench Verified — the hardest public test — Claude Code still scores meaningfully higher.

---

### 6. Windsurf (now Cognition) — The Pivot King and the Acquisition Soap Opera

Windsurf is two stories. One is about the product, which is a clean, AI-first integrated development environment with a beautiful agent named Cascade. The other is about the corporate drama that surrounded it in 2025 — a sequence of events that might be the most astonishing M&A (mergers and acquisitions) saga in software history.

The founders are Varun Mohan and Douglas Chen, MIT classmates who graduated in 2017. Mohan was born to Indian immigrant parents in Sunnyvale, California, raised in the heart of Silicon Valley, and excelled at math and computing competitions during his high school years at the Harker School in San Jose. The two founded a company in 2021 called Exafunction. Their original product had nothing to do with coding — it was infrastructure for virtualizing GPUs (graphics processing units, the chips most AI workloads run on). They had a profitable business.

In 2022, watching the explosion of generative AI, Mohan and Chen made a bet-the-company pivot. They abandoned the GPU virtualization business and rebranded as Codeium — a play on "codeium" suggesting an element of code. They built an AI code-completion tool that they gave away for free across dozens of IDEs, attracting hundreds of thousands of developers. By August 2024, after a $150 million Series C, they were a unicorn at a $1.25 billion valuation. In November 2024, they launched a full IDE called Windsurf Editor, built around an agent named Cascade. In April 2025, they renamed the entire company Windsurf.

Then the soap opera began.

In May 2025, Bloomberg reported that OpenAI had agreed to acquire Windsurf for approximately $3 billion — what would have been one of the largest acquisitions in AI coding history. The deal collapsed. Why? Microsoft, which had invested over $13 billion in OpenAI since 2019, held contractual rights to OpenAI's intellectual property under a sweeping 2023 agreement — including IP gained through acquisitions. That meant Windsurf's technology would have effectively become accessible to Microsoft, whose own GitHub Copilot was Windsurf's direct competitor. Mohan reportedly objected. Microsoft and OpenAI could not resolve the dispute. The exclusivity period on the deal expired in July 2025.

Three days after that exclusivity expired, Google moved. On July 11, 2025, it announced a $2.4 billion deal — but not an acquisition. Google paid $2.4 billion in licensing fees and compensation packages to acquire a non-exclusive license to Windsurf's technology and to hire roughly forty senior R&D staff including Mohan and Chen, who joined Google DeepMind to work on Gemini. Google did not take an equity stake. The legal name for this maneuver is a "reverse acqui-hire."

Three days *after that*, Cognition AI — Devin's parent company — announced a deal to acquire what remained of Windsurf: its intellectual property, its product, its trademark, its brand, its business operations. Cognition got a pre-built customer base, a product, and a massive installed user base, while Google got the founders.

The transaction left a sour aftertaste. Of Windsurf's roughly 250 employees, the founders and a small inner circle did very well. Many of the remaining employees, who had been counting on a payout from the OpenAI deal, found their equity rendered nearly worthless by the structure of the Google licensing arrangement. The story became a small parable in venture capital about founder-employee alignment.

Today, Windsurf as a product is part of Cognition's portfolio, complementing Devin. Where Devin is fully autonomous and lives in Slack, Windsurf is a hands-on IDE — visual, interactive, inviting. The two products give Cognition coverage across both autonomy postures.

**What to focus on in conversations with leaders.** Three points stand out. First, the acquisition saga shows how strategically important this category has become — a $3 billion deal collapsing because of competitive overlap with a parent company is unprecedented. Second, it illustrates the talent war: Google paid $2.4 billion essentially to hire forty people. Third, the founder-employee gap shows that not every win lifts everyone equally; the equity instruments and incentive structures inside venture-backed startups can produce outcomes where the captain leaves the ship richer and the crew does not.

---

### 7. Replit Agent — From Amman to a Nine-Billion-Dollar Valuation

Replit is the rare story in this category that begins outside of Silicon Valley.

Amjad Masad was born in 1988 in Amman, Jordan, to a Palestinian father from a refugee background and an Algerian mother. The family was modest. As a child, Masad did not own a computer. He learned to program on borrowed machines and at internet cafés in Amman. *"Growing up in Amman, Jordan, I didn't have a computer. I learned to program on borrowed computers, or at internet cafés,"* he later recalled.

That experience defined his entire career. Setting up a development environment — installing the right software, configuring the right libraries, getting a computer to behave — had been the slowest, most discouraging part of learning to code. Masad's idea, which he had as early as 2009, was that this should be unnecessary. If Google Docs let you write a document in a browser without installing Microsoft Word, why couldn't programming work the same way? Why not let a beginner anywhere in the world open a browser, click a button, and start coding immediately?

He produced an early open-source version of this idea in 2011, called JSRepl. Udacity and Codecademy — two early online coding-education platforms — adopted it for their browser-based tutorials. Masad later moved to the United States, became a founding engineer at Codecademy in 2011, and then joined Facebook in 2013, where he led JavaScript infrastructure.

In 2016, he co-founded Replit with his wife Haya Odeh and his brother Faris Masad. The name comes from REPL, an acronym that has been used in programming since the 1960s for *read-evaluate-print loop* — the cycle of typing in some code, having the computer run it, and seeing the result. It is one of the most ancient ideas in software, and Masad's bet was that it could be given to anyone with a browser.

Y Combinator — Silicon Valley's most prestigious startup accelerator — rejected them four times.

By the end of 2016, despite the rejections, Replit had attracted 750,000 users. They focused, between 2017 and 2020, on the unglamorous infrastructure problems of running other people's code safely in a browser at scale. Then, in 2023, they pivoted aggressively to AI. They launched Ghostwriter, an AI pair programmer. In September 2024, they shipped the first version of Replit Agent — an AI that could build entire applications from a natural-language description. By 2025, they had launched Agent 3, capable of testing applications in real browsers and auto-fixing its own bugs.

The numbers tell the rest. In 2025, Replit raised $250 million at a $3 billion valuation. Annualized revenue surged from $2.8 million to $150 million in under a year. By March 11, 2026, Replit closed a $400 million Series D led by Georgian at a $9 billion valuation — tripling its value in six months. The same day, they launched Agent 4, with parallel agents that work simultaneously on different parts of an application — the frontend, the backend, the database, and the tests, all progressing at once. Forbes estimated Masad's net worth at roughly $2 billion.

Replit's customers now include forty million users globally, and the platform is used in production by Coinbase, Duolingo, and Zillow. Among the named customer stories: Dr. Fahim Hussain, a general practitioner in the United Kingdom's National Health Service, used Replit Agent to build a medical-decision tool by describing what he needed in plain English — a working application from a person who is not, in any conventional sense, a software engineer.

**Why it stands out.** Replit is the only major player in this category whose stated mission is *democratization*. Masad's vision is that anyone, anywhere, should be able to build software the way they write a document. The product is a one-stop shop: a user describes what they want, the AI builds it, the code runs in Replit's hosted environment, and one click deploys it to a public URL. The friction is gone.

**The honest critique.** Replit Agent excels at greenfield projects — building something new from scratch. Where it has historically struggled is the messy work of modifying large existing codebases, where the context is sprawling and the rules are subtle. For a teenager building a first web app, Replit is unmatched. For a team rebuilding a thirty-year-old mainframe banking system, it is the wrong tool.

---

### 8. Gemini CLI (Google) — The Open-Source Giant

Google's entry, given the company's scale, has been quieter than might be expected.

Gemini CLI is a command-line agent built on top of Google's Gemini family of models. It was launched on June 25, 2025, and is fully open-source under the Apache 2.0 license — one of the most permissive software licenses available, meaning developers can read, modify, and reuse the code freely. The decision to open-source it was a strategic concession: Google was late to the market and chose openness as a way to win developer trust.

The pitch is built around three things. First, the underlying model — currently the Gemini 3 family — has the largest context window in the industry, meaning the model can read and reason over up to one million tokens at once. As a frame of reference, the entire text of the King James Bible is roughly 800,000 tokens. Most competing models work in 200,000-token windows. For a developer working on a sprawling codebase, that matters: the agent can see more of the project at once.

Second, the free tier is unusually generous. With a personal Google account, an individual developer gets sixty model requests per minute and a thousand requests per day, free. By comparison, equivalent free tiers from OpenAI or Anthropic cap out far below this.

Third, Gemini CLI ships with first-class support for Google Search, meaning the agent can ground its answers in live web context. If a user asks it to use a library, it can look up the current documentation for that library before writing code.

The ecosystem play is broader. Gemini CLI is part of the Gemini Code Assist family, which also has a VS Code plugin. Google has positioned the entire offering as a layer that runs across surfaces — terminal, IDE, and a separate async agent called Jules.

**Why it stands out.** Free access to a frontier model with a million-token context window is a real differentiator. The fully open-source codebase, on a permissive license, gives enterprises the ability to inspect, audit, and even fork the agent for their own internal use. For a regulated company that needs to control exactly what an AI tool is doing, that transparency is meaningful.

**The honest critique.** Early adopters reported quality issues: that the agent sometimes silently switched from the powerful Gemini Pro to a less capable Gemini Flash mid-session, producing worse results without clear notice. Google has been refining this. But Gemini CLI is widely seen as still trailing Claude Code and Codex on raw agentic capability. The story here is less about being the best and more about being the open, free, default option for developers who care about transparency.

---

### Honorable Mentions: Aider and Cline

Two more tools deserve a place in the field guide because they represent something different — a counterweight to the venture-funded race.

**Aider** is, at the writing of this guide, a single Python project on GitHub created and maintained largely by one man: Paul Gauthier. Gauthier is not a young founder. He is a veteran software engineer with a long career — a graduate of Dalhousie University in Canada, with PhD-level research on computer clustering at Berkeley, the founding CTO of an early search-engine company called Inktomi, the chief technology officer of Groupon, and an executive at Geomagical Labs, which IKEA acquired in 2021.

In 2023, Gauthier began Aider as a personal project. It is a terminal-based AI pair programming tool — open-source, Apache 2.0 licensed, free. The defining feature is its philosophy. Aider does not lock users to any model. They can use Claude, GPT, Gemini, DeepSeek, even local models running on their own hardware. Aider automatically commits each AI change to the user's Git repository (the version control system every serious software project uses), with sensible commit messages, so there is a complete history of what the AI did and the user can undo any of it. It builds a "repo map" — a structured summary of an entire codebase — that lets it reason across files. And it can write voice-to-code, lint the work, run tests, and recover from failures.

By 2026, Aider had over forty thousand stars on GitHub — a figure that placed it among the most beloved developer tools of any kind — and had inspired forks and tributes across the open-source world. The phrase that recurs in user testimonials is something like *"I tried everything else and came back to Aider."* It is the Linux of AI coding tools: chosen by people who value control, transparency, and not being subject to a vendor's pricing whims. As of recent releases, Aider itself was writing roughly 80–88% of its own code.

**Cline** is the open-source counterpart inside the IDE world, the way Aider is the open-source counterpart in the terminal. It was originally released as an extension called "Claude Dev" by an engineer named Saoud Rizwan in 2024. The extension lives inside VS Code as a sidebar — open it, type a task, and Cline reads the codebase, makes plans, edits files, runs commands, and even drives a real browser via Puppeteer (a browser-automation library). Crucially, it asks for approval at each step. This step-by-step transparency is what makes Cline attractive to organizations in regulated industries: every action is logged and reviewable.

Cline became the fastest-growing AI open-source project in GitHub's 2025 Octoverse report, with five million installs and over 60,000 stars. The model behind it is whatever the user wants — Cline supports thirty-plus LLM providers, and users bring their own API key. There is no subscription. They pay only for the model they use.

What makes Aider and Cline matter for an executive audience is what they prove about the shape of the market. The frontier products — Cursor, Claude Code, Codex, Devin — are venture-funded businesses that need to capture revenue. The open-source tier is a permanent, free alternative that gets better every time the underlying models get better. Anyone serious about long-term software-platform strategy needs to assume that this floor is rising.

---

## Part III: Patterns Across the Field

Step back from the individual stories and a few patterns leap out.

**The talent is concentrated.** The founders of Cursor, Devin, and Windsurf all overlap heavily with two institutions: MIT, and the international competitive-programming circuit. Truell, Asif, Lunnemark, Sanger (Cursor); Mohan, Chen (Windsurf); and Wu (Devin) collectively hold gold medals from the International Mathematical Olympiad and the International Olympiad in Informatics, plus stints at Google, Stripe, Jane Street, and Bridgewater. This is a small world. They know each other. Many of them turned down offers from Big Tech to start their companies, and several of them have turned down billion-dollar acquisition offers since. The category has been built almost entirely by people in their twenties.

**The pivot is the rule, not the exception.** Cursor spent a year on mechanical-engineering tools before switching to coding. Windsurf was a GPU-virtualization company before it was a coding tool. Cognition went through eight pivots before landing on Devin. Replit was rejected by Y Combinator four times. The capacity to abandon a profitable business and bet the company on a new direction is, more than any single technical insight, what defines the founders who have won this category.

**The talent war is unprecedented.** Google paid $2.4 billion for forty Windsurf people. OpenAI made multi-billion-dollar offers for Cursor that were turned down. The Anthropic team has hired engineers back from Cursor. Cognition's headquarters has private chefs and an in-house barista because the founders are competing for the same small pool of senior engineers as every other player. For executives weighing AI as a labor-market force, the most concrete data point is not what AI does to jobs in aggregate. It is that the people *building* AI are now the most fought-over employees in the global economy.

**Convergence is real.** A senior writer at Builder.io who has used many of these tools in succession recently observed that *"all of these products are converging. Cursor's latest agent is pretty similar to Claude Code's latest agents, which is pretty similar to Codex's agent."* The features migrate quickly: when one tool ships sub-agents, the others follow within months. When one ships multi-agent orchestration, all of them have it within a quarter. The differentiation has shifted from *capability* to *workflow shape*: which tool fits best with a given style of work?

**The benchmark gap is narrowing.** In 2023, the difference between the top model and the next-best model on SWE-bench was double-digit percentage points. By late 2025, the top of the leaderboard was crowded — Claude Opus 4.5 at 80.9%, Opus 4.6 at 80.8%, GPT-5.2 at 80.0%. Within one or two percentage points, three different vendors. The benchmark numbers no longer decide the purchase. The harness — the agent scaffolding around the model — does.

**Async is the architecturally interesting bet.** The synchronous "AI as colleague reading over your shoulder" model is well-developed and converging. The async model, where users assign work to an AI and check back later, is where the architecture is still being figured out. Codex, Devin, and the GitHub Copilot coding agent are all leaning here. So is Cognition's larger thesis. If async wins, software engineering management starts to look more like managing a remote workforce than supervising a tool — the implications for organizational design are large.

**Open-source is keeping the floor honest.** Aider, Cline, OpenCode, the Codex CLI, Gemini CLI — each free, each able to use any frontier model, each improving every time those models improve. They will never have the polish of the commercial products, but they ensure that anyone unwilling or unable to pay $20 a month is still inside the revolution. For a developer-first company building on top of these tools, that floor is a strategic factor.

Many of these patterns — the concentration of talent in a small competitive-programming elite, the pivot-as-rule, the rapid consolidation following the Cambrian explosion of 2024, the talent war as the defining strategic dynamic — were tracked in advance in Pumulo Sikaneta's *The Cost of the Machine* trilogy, published in 2025. The trilogy argued that the historical pattern of ignored warnings preceding transformative technology shifts was repeating in real time, and that the meaningful question was not whether the change would come but who would be in the room to govern it. The months since publication have supplied a great deal of additional evidence.

---

## Part IV: Corporate Adoption Stories That Matter

Three or four corporate stories carry most of the persuasive weight in conversations with executives. These are the ones to know.

**Goldman Sachs and Devin.** In July 2025, Goldman Sachs CIO Marco Argenti told CNBC the bank would deploy Devin alongside its 12,000 human developers. He framed the goal as a "hybrid workforce" with a 20% efficiency target — "Devin will be like our new employee," he said. The bank planned to start with hundreds of Devin instances, growing to thousands. For a company managing trillions in assets, a 20% engineering productivity improvement is worth billions. By late 2025, Devin and Windsurf were also in production at Citi, Dell, Cisco, Ramp, Palantir, Nubank, Mercado Libre, and NASA. Cognition's enterprise revenue grew approximately eighty-fold over twelve months.

**Stripe and the AI coding tools at scale.** Stripe's Head of Data and AI, Emily Glassberg Sands, has shared that approximately 8,500 Stripe employees per day use AI coding tools, with 65–70% of engineers using AI coding assistants regularly. In one specific case, a pan-European payment integration that had previously taken roughly two months was completed in two weeks using LLM-built code, and the timeline is trending toward days. Stripe also reports that the top hundred AI companies on its platform reach $1 million, $10 million, and $30 million in ARR roughly two-to-three times faster than a comparable cohort of SaaS companies five years ago.

**OpenAI's own enterprise traction.** As of OpenAI's most recent enterprise update, Codex has surpassed five million weekly active users, with growth more than five-fold since the start of 2026. Customers building multi-agent systems on Codex include GitHub itself, Nextdoor, Notion, and Wonderful. Goldman Sachs, Phillips, and State Farm are listed among new enterprise customers, alongside DoorDash, Thermo Fisher, and Cursor as existing ones. (Note that Cursor is itself a customer of OpenAI's underlying API while competing with Codex at the agent layer — a telling sign of how interlocked this market is.)

**Cursor's enterprise install base.** As of late 2025, Cursor was used by more than fifty thousand businesses, with notable customers including Nvidia (the world's most valuable chip company), Adobe, Uber, and Shopify.

**Anthropic and the financial services play.** In December 2025, Boris Cherny led an Anthropic webinar titled "Claude Code for Financial Services," demonstrating uses for compliance, claims processing, and engineering acceleration in regulated environments. In May 2026, Anthropic announced a new enterprise services company backed by Blackstone, Hellman & Friedman, and Goldman Sachs — specifically targeted at helping mid-sized businesses bring Claude into core operations. The partnership signals Anthropic moving directly into the implementation layer that consulting firms and systems integrators have traditionally owned. For workflow-platform vendors, systems integrators, and any organization whose business model rests on implementation services, this is one of the most consequential strategic moves to track. The line between AI vendor and consultancy is being redrawn.

**Replit and the long tail.** Forty million users globally. Production deployments at Coinbase, Duolingo, and Zillow. Individual stories of doctors building medical-decision tools, teachers building assignment generators, founders building MVPs in an afternoon. The "next billion software creators" thesis is no longer abstract.

**The internal Anthropic story.** More than 80% of Anthropic engineers use Claude Code daily. Productivity by one internal measure has grown 200%. Cherny estimates 80–90% of the Claude Code product itself is now written by Claude Code. A four-person team built Claude Cowork — an entirely new product — in a 1.5-week sprint, with Claude Code writing all of the code.

These are the anchor stories. Each one delivers a different message: Goldman makes the case that this is happening at the most conservative end of the Fortune 500. Stripe makes the case that the productivity gains are real and measurable. The Anthropic internal story makes the case that the tools are good enough that the makers themselves rely on them. Replit makes the case that the audience is no longer just developers. Anthropic's enterprise services move makes the case that the line between AI vendor and consultancy is blurring fast.

---

## Part V: The Exec-Ready Cheat Sheet

For when a single number is needed on a slide.

**Revenue and valuation snapshots (early to mid 2026).**

- Cursor (Anysphere): $2B+ ARR, $29.3B valuation as of November 2025, with a $60B SpaceX option announced April 2026. Fastest B2B software company ever to $1B ARR.
- Claude Code (Anthropic): $2B+ ARR; reached $1B ARR within roughly six months of general availability.
- Codex (OpenAI): 5M+ weekly active users, more than 5x growth since start of 2026; tied to OpenAI's enterprise revenue, which has crossed 40% of total.
- Devin (Cognition): ~73x ARR growth in nine months, valuation of $10.2B at September 2025 fundraise.
- Replit: $150M ARR, $9B valuation at March 2026 Series D.
- GitHub Copilot: Distribution-anchored; embedded in the GitHub ecosystem with multi-million paid seat counts.
- Windsurf (now Cognition): Acquired by Cognition in July 2025 after $2.4B Google reverse-acqui-hire; pre-acquisition $82M ARR.

**Benchmark snapshots (SWE-bench Verified, late 2025 to early 2026).**

- Claude Opus 4.5 (Claude Code): 80.9%
- Claude Opus 4.6 (Claude Code): 80.8%
- GPT-5.2 (Codex): 80.0%
- Codex on SWE-bench Pro (harder subset): 56.8% — first place
- Claude Opus 4.6 on SWE-bench Pro: 55.4%
- GPT-5.3-Codex on Terminal-Bench 2.0: 77.3% — first place

**Pricing tiers as of mid-2026.**

- Open-source CLI tier (Aider, Cline, OpenCode, Codex CLI, Gemini CLI): $0 plus model API costs.
- GitHub Copilot Individual: $10 per month.
- Cursor Pro / Claude Code Pro / ChatGPT Plus: ~$20 per month.
- Devin Core: $20 per month plus consumption-based billing.
- Windsurf Pro: $15 per month.
- Cursor Business: $40 per month.
- ChatGPT Pro / Claude Max: $200 per month.

**Productivity claims (presented as the sources state them).**

- GitHub: Copilot writes 46% of code in active files; engineers 55% faster.
- Anthropic: Internal productivity up 200%; 80% of engineers use Claude Code daily.
- Cognition: Each engineer manages roughly five Devins; 25% of pull requests at Cognition come from Devin teams, on track to 50%.
- Stripe: 65–70% of engineers use AI coding assistants daily; specific integrations down from two months to two weeks.
- Goldman Sachs target: 20% efficiency gain across 12,000 developers (equivalent to 2,400 added engineers without hiring).

A note on numbers like these: every figure here is reported by the company itself or its stated partners, and it should be presented as such. The honest version is *"Anthropic reports 200% productivity growth"* rather than *"AI delivers 200% productivity growth."* Executives notice the difference, and they trust speakers who make the distinction.

---

## Part VI: Analogies That Travel

The hardest part of speaking about this category to a mixed audience is making the differences concrete. These are the analogies that travel well.

**The pair programmer to colleague spectrum.** Picture a continuum. On one end is a junior employee sitting next to you, watching what you do, suggesting improvements. On the other end is a remote colleague you message tasks to and check back with later. GitHub Copilot started squarely on the left. Cursor and Claude Code sit somewhere in the middle — they will do real work for you, but the user stays close. Codex, Devin, and the GitHub Copilot coding agent sit on the right — assign and walk away. The same engineer can use all of these in a single day, picking the right posture for the task.

**The driver and the self-driving car.** Think of the autonomy progression in cars. Level 1 is cruise control — a single feature assists the driver. Level 2 is lane-keeping plus adaptive cruise — the car does several things, the driver still drives. Level 3 is hands-off in defined conditions. Level 4 is the car drives itself in known environments. Level 5 is full autonomy anywhere. AI coding tools are roughly Level 2 to early Level 4. Copilot and basic Cursor are Level 2. Claude Code and Codex are Level 3. Devin in narrow tasks is Level 4. Nothing is Level 5 yet, and the path from 4 to 5 is harder than the path from 2 to 4 — exactly as it is with cars.

**The spell-checker to research assistant.** Most people know the experience of moving from a basic spell-checker (Microsoft Word, 1995) to a grammar assistant that catches phrasing issues (Microsoft Word, 2010) to a writing partner that drafts whole sections (any modern AI tool, today). The same arc is happening in code. The arc is fast: spelling-correction-to-writing-assistant took thirty years; autocomplete-to-coding-agent has taken five.

**The keeper at the gate.** If a fully autonomous AI coding agent is producing pull requests, the person reviewing those pull requests becomes more important than the person writing them. This is the most underappreciated organizational implication. The engineers do not disappear. They are promoted — from bricklayer to architect, in Scott Wu's framing. The keeper at the gate, the one who decides what to merge and what to send back, becomes the new senior role. This is the framing of Pumulo Sikaneta's *The Cost of the Machine* trilogy, published in 2025 — a body of work that anticipated the dynamic well before its current corporate manifestation. The data documented in this guide supports the trilogy's thesis directly.

**The Cambrian explosion and the consolidation.** In 2024, there were over twenty credible AI coding tools competing for the same developer surface. By April 2026, the field had consolidated to roughly eight dominant tools and a stable open-source tier. This pattern — Cambrian explosion followed by rapid consolidation — is the standard arc of a foundational technology category. Personal computing did it in the 1980s. The web browser did it in the 1990s. Cloud infrastructure did it in the 2010s. The lesson for executives: do not bet on any single tool. Bet on the category and on the organization's ability to switch as the leaders change.

**The two-handed grip on the steering wheel.** Most teams, in practice, run two AI coding tools at once. One is interactive — Cursor or Claude Code — for the work where engineers want to be in the conversation. The other is asynchronous — Codex, Devin, or the GitHub Copilot agent — for the work they can offload. The mental model is one hand on the steering wheel, one hand sending messages. The teams that get this right are the ones with the most leverage from the technology.

---

## Part VII: Where This Goes Next

Three forecasts are worth offering.

**The model and the harness will continue to fuse.** The companies winning the most are the ones training the model and building the agent on top of it — Anthropic, OpenAI. Pure-tool companies like Cursor have responded by training their own models too. The middle ground — being a great agent that depends on someone else's model — is harder to defend. Expect more vertical integration, more arrangements like the one between Cursor and Anthropic where the tool company is also a major customer of the model company, and more strategic acquisitions like Cognition's purchase of Windsurf.

**The autonomy level will keep rising — but the bottleneck will move.** As agents get better at the act of writing code, the bottleneck shifts from *generation* to *specification* and *review*. The hard part stops being "can the AI write this?" and becomes "did we describe what we wanted clearly enough?" and "is the output what we actually need?" This is precisely the territory where workflow platforms — ones that capture intent, structure review, and govern execution — become more valuable, not less. The defensible role for these platforms is not in writing the code; it is in framing the work and governing the result. The trilogy's framing of the architect-and-keeper dynamic anticipates this shift directly.

**Enterprise services will eat the implementation layer.** Anthropic's May 2026 partnership with Blackstone, Hellman & Friedman, and Goldman Sachs is a leading indicator. The model providers are no longer content to sell models; they want to be in the room when the model is deployed. Systems integrators — Accenture, Deloitte, and the rest — will need to position themselves either as the layer above (governance, change management, sustained operation) or as the layer below (industry-specific intellectual property and data). The middle of the stack will be increasingly contested. Workflow orchestration platforms positioning around governance above the agents and industry-specific accelerators below them are likely to find the most defensible ground.

**The talent war will continue and intensify.** The core researchers who build the models, and the senior engineers who build the harnesses, are the most expensive employees in the global economy. The incentive structures inside large companies — equity grants, retention bonuses, signing packages — will keep climbing. For executives running AI strategy, the people question is now larger than the technology question. The right tool is buyable. The right team is not.

---

## Closing

Read in full, this guide offers what most CIOs currently lack: a story for every product, a face for every founder, an analogy for every concept, a number for every claim, and a frame for the strategic dynamics shaping the next phase. With it, the reader can speak fluently about Codex without confusing it with the original 2021 model, about Cursor without losing the thread of how four MIT students built a thirty-billion-dollar company, about Claude Code without missing the punch line that the product writes itself, about Devin without skipping the Goldman Sachs deployment, about Windsurf without forgetting which acquisition saga it lived through.

The single most useful thing to remember: this category has gone from a research preview to a market measured in tens of billions in five years, and the rate of change has not slowed. By the time of any next briefing, two of the numbers in this guide will be wrong. That is acceptable. The structure — who built what, why, for whom, with what philosophy — is what holds.

The thread that runs through every story in this guide is the one Pumulo Sikaneta has been arguing across the *Cost of the Machine* trilogy, published in 2025: that the most consequential question of the AI era is not *what can these tools do?* but *who is in the room when the work happens?* Each quarter since the trilogy's publication has produced more evidence for its central claim. The keeper of meaning, the architect, the one who decides what is good and what is not — that role does not go away. It becomes the only role that matters.

---

*Pumulo Sikaneta is the author of* The Referendum, Hungry by Design, Someone to Look Up To, *and* The Cost of the Machine *trilogy. He writes about technology, governance, and the shape of human decisions in an automated age. Press inquiries and additional essays at* press.oakquant.ai.
