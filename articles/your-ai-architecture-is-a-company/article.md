I spent last week at PegaWorld, and the word I heard more than any other was agents. Not AI. Agents. Usually plural. Often very plural. Somewhere between the keynotes and the hallway conversations, a pattern became hard to ignore. Everyone agrees they are the future of enterprise software. Almost nobody agrees on how many you should have, what each one should know, or how they should talk to each other. Those turn out to be the only questions that matter.

Here is the mental model I keep offering, because it works on everyone from engineers to CFOs, and because it converts an abstract architecture debate into a decision most leaders have already made once in their careers.

Your AI architecture is a company. Staff it like one.

## The employee, the hands, the training

An agent is an employee. The model behind it is raw intelligence, the talent you hired. A frontier model is a brilliant generalist on day one, capable of reasoning about almost anything and knowledgeable about almost nothing that is specifically yours.

Tools are its hands. They define what the agent is allowed to touch. In practice this means the systems it can reach, the APIs it can call, the records it can read and write. The Model Context Protocol, MCP, has become the common way to grant this access, and the badge-access metaphor is more literal than it sounds. A tool grant is a security decision, an audit surface, and a statement about trust, exactly like deciding which floors an employee’s badge opens.

Skills are its training. They are the playbook, the institutional knowledge, the procedures and standards that turn a smart generalist into someone who actually knows how your claims, disputes, and exceptions run. Intelligence without skills is a gifted new hire who has never seen your business. Skills without intelligence is a binder nobody can apply to a situation the binder did not anticipate. You need both, attached to the same agent.

Once you see agents this way, the central architectural question becomes a staffing question, and staffing questions have a long and well-documented history.

## The question every founder answers

Would you rather build with five exceptional people who carry deep expertise and broad access, or fifty narrow specialists who each know one thing and must hand everything off?

Most leaders answer instantly. Nobody dreams of founding a bureaucracy. And yet, walk the floor of any enterprise architecture review this year and you will find diagrams with dozens of single-purpose agents arranged in elaborate chains: one agent that classifies, which calls one that extracts, which calls one that validates, which calls one that decides, which calls one that writes the letter. The fifty-person org chart, rebuilt in software, by people who would never staff a team that way.

The math punishes this twice, and the second punishment is the one the diagrams hide.

## Brooks: the cost of talking

The first punishment is coordination, and Fred Brooks worked it out in 1975. In The Mythical Man-Month he observed that communication paths grow with the square of headcount. Five people have 10 possible paths between them. Fifty people have 1,225. His conclusion, that adding people to a late software project makes it later, follows directly: every new person adds more communication burden than working capacity.

Agent architectures inherit this geometry exactly. Every path between agents is an interface to design, a contract to maintain, a place where intent gets diluted. Anyone who has watched a decision travel through six layers of an organization knows that what arrives at the far end is not what left. The difference is that in a human organization the dilution is gradual and partially self-correcting, because people share context, history, and the ability to walk down the hall and ask. Agents have none of that, which brings us to the second punishment.

## Amnesia: the cost of forgetting

Agents are employees with amnesia. A human handoff carries shared history. Two colleagues who have worked together for a year exchange a two-line message and understand each other completely, because the other 10,000 lines are already in both heads. An agent handoff carries only what fits in the briefing document. Every hop in a chain means re-explaining the context from zero, and the briefing is paid for in tokens, every time.

Consider a simple model, with the assumptions stated plainly. Suppose a piece of work requires a body of context, call it C tokens, to be done correctly: the customer history, the policy terms, the relevant regulations, the case so far. In a chain of n agents, that context must be transmitted at every hop. If each agent forwards the full context, the chain consumes on the order of n times C in context tokens alone, before any actual work happens. If, to save cost, each agent forwards a summary instead, the spend drops but something worse happens: detail is shed at every hop, and the agent making the final decision works from a summary of a summary of a summary. You can pay in tokens or pay in truth. A deep chain makes you pay in both.

This is why a ten-agent chain does not split the work ten ways. It pays for the same onboarding ten times and loses a little fidelity at each transfer. It is the meeting that should have been an email, except you are billed per word.

## Coase: why firms exist at all

There is a deeper reason this matters, and it was identified long before software existed. In 1937, the economist Ronald Coase asked a question so simple it took a Nobel Prize to answer: if markets coordinate economic activity so well, why do firms exist at all? Why is anything organized inside a company rather than purchased, transaction by transaction, on the open market?

His answer was transaction costs. Every market exchange carries the cost of finding the right counterparty, negotiating the terms, and enforcing the agreement. When those costs exceed the cost of simply bringing the work inside and directing it, a firm forms. The boundary of the firm sits exactly where internal coordination becomes cheaper than external transaction.

Now read that paragraph again with agents in mind. Every handoff Brooks would count as a communication path is also, in Coase’s terms, a market transaction. There is a discovery cost, deciding which agent should handle the next step. A negotiation cost, serializing the request into a form the next agent can act on. An enforcement cost, validating that what came back is what was asked for. These costs are real, they are paid in tokens and latency and error rates, and a sprawling mesh of independent agents pays them at every single exchange.

Coase tells you what to do about it. When transaction costs dominate, you internalize. You form a firm. In agent architecture, the firm is the governed workflow: the structure that holds the state, the history, and the rules of the work in one place, so that coordination stops being a series of expensive bilateral transactions and becomes direction.

## Three shapes, three companies

In practice, multi-agent architectures take three broad shapes, and each has an organizational twin.

The first is the peer-to-peer chain, agents calling agents calling agents. Its twin is the company with no management at all, where work moves by whoever-knows-someone, and the failure mode is exactly what you would expect: nobody can say where a piece of work is, why a decision was made, or which link in the chain dropped the critical detail. The telephone game, productionized.

The second is the supervisor tree, an orchestrating agent that decomposes work and delegates to subordinate agents, which may delegate further. Its twin is the layered hierarchy, and it inherits the hierarchy’s classic ailment. Every layer of management is a layer of summarization. The supervisor knows what its reports told it, which is not the same as what happened. Supervisor trees are better than chains because someone is at least nominally accountable, but the context still lives in transit, repeated and compressed at every level, and the token meter runs at every one.

The third is hub-and-spoke around a system of record. Here the context does not travel at all. It lives in one governed place, a case, a workflow, a spine, and agents come to the work rather than passing the work among themselves. Each agent reads the slice of context it needs, does its job, and writes its result back to the same record, with the workflow enforcing sequence, authority, and audit. Its organizational twin is the well-run firm that Coase described: a small senior team with a good operating system, where the case file, not the corridor conversation, is the memory of the institution.

The handoff in this third shape stops being a summary and becomes a shared file. That single change collapses both punishments at once. Brooks’s quadratic paths flatten, because agents talk to the work instead of each other. The amnesia tax disappears, because nothing has to be re-explained to a record that never forgot.

## Small and structured

So the winning shape is small and structured. A few genuinely capable agents, each carrying real skills and real tools, reporting to a spine that remembers. Not because small is fashionable, but because the mathematics of coordination and the economics of the firm both point at it, from arguments separated by half a century, written long before anyone had a token budget.

I have made the longer version of this argument before: that governed structure is not the enemy of AI capability but the thing that makes capability deployable, and that the systems worth trusting are the ones whose worst day is survivable, not the ones whose best demo is dazzling. The staffing argument is the same thesis wearing different clothes. A company is not great because it employs the most people. An architecture is not capable because it runs the most agents.

The design principle follows directly. Build the smallest number of agents that can cover the work, and anchor them on a governed system of record.

Headcount was never the goal in a company. It is not the goal in an architecture either.