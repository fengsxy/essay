---
layout: post
title: "Beyond High Intelligence, Humans and Models Both Need High Agency"
date: 2025-11-10
description: "Agency Evaluation"
tags: [life, entrepreneur, en]
lang: en
---
Whenever I sit on a panel or listen to company talks, the host loves to ask what kind of people we want to hire. The default answer is "High Agency." Ph.D. committees call it "self-motivated," and at ByteDance it is "taking ownership even when no task is assigned." The same trait is crucial for AI systems, so we need a crisp definition of what agency actually means.

In today's AI discourse, "agency" (or "agents") is everywhere. But when we say the word, what are we really referring to?

Many people equate it with **tool use**, **instruction following**, or **task planning**. That is a severe misunderstanding.

Before diving deeper, let's sketch a **"high-IQ yet zero-agency"** large language model (LLM). This model may:

- Possess massive amounts of **world knowledge**.
- Handle **long-context reasoning** with ease.
- Score extremely high on **reasoning** and **coding** benchmarks.

And yet, when you hand it a task, it behaves like this:

1. **Blind execution without intent modeling.** It dutifully follows every instruction but never asks **why**. Even absurd or inefficient directions are executed to the letter. Like an employee who never questions whether the assignment makes sense, it lacks a global view and can't optimize for the true goal.
2. **Shallow exploration and quick abandonment.** It will try the two or three approaches you (or it) listed. The moment a tool call fails, it halts, returns an error, or simply gives up instead of asking why the failure happened and what other paths might exist.
3. **Confidently wrong with zero self-awareness.** It always "delivers" something yet has no idea whether the output is correct or the task is truly done. Lacking self-evaluation, it can repeatedly hand in a wrong or useless answer with full confidence.

Real agency is none of the above. It is the ability for a model, within a specific environment, to read human intent, understand the available tools, and **autonomously find an optimal path to the goal**.

Measuring such a capability demands more than the traditional benchmarks. We need a new set of evaluation axes.

---

### 1. Meta-Cognition: Not Just "What to Do," But "Why We Do It"

First, dispel a myth: in agent-style tasks, **"instruction following" can be a trap**. Users are not guaranteed to propose the most efficient or reasonable plan.

Low-agency models obediently "comply" with a wrong instruction; high-agency models "reflect" on it.

- **Meta-cognition** means the model can step beyond the literal instruction, reason about the **fundamental goal** and **rationality** of the task, and, if needed, question the proposal in pursuit of a better solution.

> **Scenario comparison**
>
> - **Scenario 1: A pointless task.** The user asks the model to scan a huge database to find a deliberately planted but meaningless string (a "needle in a haystack" benchmark).
>     - **Low agency:** starts searching and keeps going until it times out or fails.
>     - **High agency:** responds, "It looks like you're running a test. What is the real objective here? This request doesn't appear to create value."
> - **Scenario 2: Inefficient tool choice.** The user insists on using Selenium (a UI automation tool) to crawl a static website.
>     - **Low agency:** spins up a browser, loads pages, clicks through, and slowly scrapes.
>     - **High agency:** suggests, "The target is a static site. Selenium is heavy and slow. Let me switch to `requests` + `BeautifulSoup`; that should be 10x faster. Sound good?"

**How to measure**

- **Wrong-instruction correction rate:** When the given plan is obviously inefficient or wrong, can the model spot it and propose a better route?
- **Path efficiency:** For open-ended tasks, how close is the model's chosen path length (`L_model`) to the known optimal length (`L_optimal`), i.e., `L_optimal / L_model`?

---

### 2. Exploration Ability: Putting "All Roads Lead to Rome" Into Practice

Real-world tasks are messy and uncertain. Any sufficiently long chain of steps will trigger failures and dead ends. Exploration is the ability to detour around those obstacles.

- **Exploration** means that when faced with failure, the model **diagnoses the root cause** and **proactively surfaces new, diverse solutions**, instead of repeating the same attempt.

> **Scenario comparison**
>
> - **Scenario 1: Environmental blocking.** The model cannot reach GitHub (for example, due to network restrictions in mainland China).
>     - **Low agency:** keeps rerunning `git clone` and repeats "connection error."
>     - **High agency:** identifies the connectivity issue, tries mainland mirrors, cached bundles, or configures a proxy/VPN to restore access.
> - **Scenario 2: Version drift.** Code runs locally but fails in production because dependencies or runtime versions differ slightly.
>     - **Low agency:** keeps patching random import errors or reruns the same commands as if nothing changed.
>     - **High agency:** pinpoints the mismatch, rebuilds a clean environment, records requirements/lockfiles or containers, and institutes release checks to prevent repeats.

**How to measure**

- **Failure-recovery rate:** When tools crash, API keys expire, the network drops, or files go missing, does the model simply stop or can it craft a workaround?
- **Repeat-attempt rate:** Does the model keep failing on the exact same step (for example, more than 70% identical logs)? High-agency systems should recognize and break out of loops.

---

### 3. Task Completion Awareness: From "Delivery" to "Ownership"

Does the model know whether it has truly finished the job? Does it understand what "done" means?

When shipping products, top operators define **milestones** and **hypotheses** to ensure they are still on the right track. Agentic models should act the same way.

- **Task completion awareness** is the ability to **plan explicit checkpoints** and **success criteria** for every phase, and to verify progress against them.

> **Scenario comparison**
>
> - **Scenario 1: Planning a seven-day itinerary.**
>     - **Low agency:** dumps a list of attractions (Forbidden City, Summer Palace, Great Wall, etc.) and stops.
>     - **High agency:** after drafting the plan, it **self-reviews**: Are travel times realistic? Are rest days sufficient? Are tickets sold out? It uses maps, booking APIs, or calendars to verify before handing it over.
> - **Scenario 2: Writing a "small but complete" game.**
>     - **Low agency:** pastes a Pygame snippet and calls it a day.
>     - **High agency:** after coding, it **recreates the runtime**, runs smoke tests, checks for missing assets/permissions/bugs, and only then submits the build.

**How to measure**

- **Milestone design:** For multi-step problems (10 or more actions), can the model define 3-5 sensible checkpoints on its own?
- **Self-evaluation frequency:** How often does it trigger self-checks or quality gates during execution?

---

### 4. Collaboration: "No Hero Wins Alone"

Even the strongest model has limits. High agency means **recognizing your boundaries** and knowing when and how to ask for help.

Help can come from specialist agents, human-in-the-loop partners, or decomposing work for parallel execution.

- **Collaboration** is the ability to **identify complexity and one's own limits**, break work into modules, and route pieces to other agents or humans who are better suited, while orchestrating the final delivery.

> **Scenario comparison**
>
> - **Scenario 1: Complex data analysis.**
>     - **Low agency:** tries to do everything sequentially -- cleaning, statistics, visualization -- and gets bogged down.
>     - **High agency:** **discovers parallelizable sub-tasks**: calls a "data cleaning agent" for missing values, a "statistical analysis agent" for correlations, and focuses on synthesizing the insights.
> - **Scenario 2: Designing a medical AI product.**
>     - **Low agency:** makes things up, inventing medical jargon and product flows.
>     - **High agency:** immediately **acknowledges its limits**: pulls in a "medical expert agent" to validate clinical logic and a "compliance agent" to review HIPAA/privacy constraints.

**How to measure**

- **Parallel-task discovery rate:** When a task can be parallelized, does the model detect and architect concurrent paths?
- **Collaboration gain (`Difference`):** Compare completion time/quality when the model works alone vs. when it recruits other agents or humans. Bigger gains (for example, 80% faster or 50% more accurate) indicate better collaboration decisions.

### Conclusion

Agency is far more than automated execution.

It is a compound capability built on **meta-cognition**, **strong exploration**, **self-awareness of progress**, and **collaborative intelligence**. Together, these pillars turn AI from a mere tool into a true assistant -- and eventually, a partner. Using these standards to evaluate and build agents is how we move toward general intelligence.
