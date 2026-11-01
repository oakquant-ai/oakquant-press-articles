When I open a new AI coding session, I want it to inherit the lessons from the last one. Otherwise I am paying to explain the same mistake twice.

The code is already there. The reason for an awkward rule, the test that caught a convincing wrong answer, and the evidence behind a design choice may be harder to find.

I want those decisions to survive a new conversation, a different model, and my own imperfect memory. That capability is becoming part of what I am building at OakQuant.

I believe a growing share of software's intellectual property (IP) will live in this ability to build, judge, and improve the work. I am trying to give that belief a working form.

OakQuant is a financial research and planning platform. It brings together research, assessments, forecasts, and workflows that can include a human decision. Explaining it in one sentence is much quicker than making those parts agree with one another.

I built it with AI coding tools. The first saved snapshot of its agent's code dates to October 2, 2025. About thirteen months later, I have both a product and a growing body of instructions and checks for developing it.

## A fix needs somewhere to leave its lesson

A person finishes an assessment, sees their profile, and returns later to an empty screen. The scoring worked. The handoff failed. Somewhere between saving the result and retrieving it, the software lost track of what that person had finished.

A coding tool can help fix the handoff. I still have to decide where that fix belongs and check that the result reaches the person who needed it. Then I want the next feature to benefit from what we found.

## I need the next session to remember

The next feature may cross the same services. Its coding session may start without the conversation that explained the first fix, leaving the agent to rediscover a rule I already paid to learn.

I could explain it again. I would rather leave instructions beside the code, with a check the next session can run before it makes the same mistake.

That is how I use skills. Each one names the task that should trigger it, explains the relevant rules, and points to a command or checklist that tests the work.

As of November 1, 2026, my workspace index lists 83 skills across 10 repositories. Each repository holds a project and its change history.

Of those skills, 79 are committed in Git, the tool that saves snapshots of that history so I can compare versions or return to an earlier one. Four are local additions awaiting their first commit.

They cover where code belongs, access controls, data and forecasting, workflow definitions, screen layouts, release checks, and research methods. Some are broad. Others deal with a narrow failure that was expensive enough to deserve its own reminder.

A screen change needs different guidance from a forecast change, so an index lets the coding agent find the skills relevant to the job before it starts making changes. Loading all 83 into every conversation would make a fairly thorough reading assignment.

The count tells you how much guidance I have captured. The useful part is whether the right instruction reaches the work while there is still time to change it.

## I write down what a good answer must do

A stock digest can read well and attach a story to the wrong company. An agent can sound helpful while trying to open another person's records. The prose may be good. Both answers need to fail review.

Those are the kinds of cases I keep in evaluations, or evals. Each case describes a task and the outcome I expect, including what the system must refuse or leave alone.

My working library contains 98 cases across eight areas, including newer local suites. They cover agent boundaries, market digests, investment workflows, mobile tasks, two news-attribution sets, research drafts, and voice interactions.

The task stays separate from the model or provider that will attempt it, so I can compare candidates against the same expectations without rewriting the exam for each one. The model can change.

I treat the first run like a practice exam taken before studying. Its score is the baseline. A separate set of questions stays out of practice and becomes the final exam, called the held-out set.

That keeps me from mistaking familiarity with the practice questions for better performance on fresh ones. A critical failure stays visible. A good average must not hide it.

The scorer needs checking too. Before I let a score decide which approach to keep, the rule is to try it on a known good result and a deliberately bad one. A scorer that likes both needs fixing.

GitHub gives the work a place for review. A pull request proposes a change so it can be examined before being merged into the shared version of the project. The comparison shows which lines changed.

The instructions and eval cases can travel through review alongside the code so a change that makes the passing score easier to achieve is visible in the same place.

The shared quality workflow defines automatic checks for pull requests and updates to the main branch, the project's shared version. Those include tests of the eval validator and validation of the case files.

That checks the files. The behavior still needs to be run and scored. One of my skills specifically asks whether the tests executed and whether their result could actually stop a change.

## There is a skill for improving the skills

After a difficult fix, I want the next session to inherit something more useful than a long account of what went wrong.

The meta-skill, called the skill radar, asks whether the lesson is likely to recur and whether it can become a check. It also looks for an existing skill that should own it.

The agent can then propose an addition or an update. I decide. A one-off workaround should not quietly become a rule that every later feature has to obey.

This is how I want development to accumulate value. A fix repairs today's result, and a well-chosen check gives the next session a chance to catch the same class of mistake before I see it again.

![Each useful fix can improve the product and the instructions and checks that guide the next coding session. The flow moves from finding a problem to capturing a lesson, then to my review and approval before updating skills and evals. The next session inherits the relevant guidance, and new work can reveal the next lesson.](images/founder-learning-loop.png)

## What I want OakQuant to own

When I try a different coding model, the skills and evals give it the task rules and checks I have already developed. I can judge its work against expectations that existed before it arrived.

Another builder can write a skill file. The format is simple. The hard work is deciding which lesson belongs there, finding a case that exposes the failure, and making sure the check runs when that failure could return.

That is the capability I mean by future IP. It holds the judgment about what to build, the evidence for accepting it, and a process for revising the rules when experience proves them wrong.

The count will change. A smaller set of skills that reliably improves the work would be worth more to me than a large collection the agent never reads.

As code becomes easier to produce, I expect this accumulated judgment to matter more because each useful fix can improve both the product and the process that builds its next feature.

I still choose what belongs in the product and approve changes to the rules. The skill radar proposes. It does not get to rewrite my standards because a new answer would score better under easier ones.

For the next feature, I want a person with a task and a result they can use. When I open that coding session, the relevant instructions and checks should already be there.

You can explore what I have built at [sky.oakquant.ai](https://sky.oakquant.ai/) and read the accompanying essays on [OakQuant Press](https://press.oakquant.ai/public/articles).
