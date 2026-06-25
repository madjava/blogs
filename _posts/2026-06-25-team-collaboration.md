---
layout: post
author: Felix Eyetan
title: Team Collaboration
level: Intermediate
is_blog: true
---

A while back, I gave a talk at our team's Away Day on the topic of team collaboration in engineering. It was part of a broader discussion we were having as a platform operations team about how we work together, and more importantly, how we *should* work together.

This post is an attempt to capture and expand on those ideas, drawing on two books I'd recommend to any engineer: **Accelerate** by Forsgren, Humble, and Kim — and **Leaders Eat Last** by Simon Sinek. Both of them shaped how I think about what high-performing teams actually look like.

## Why Collaboration Matters Now

There's a quote I opened my talk with:

> "High-performing engineering organisations aren't defined by their tools — they're defined by how well their teams collaborate."

I think most engineers would nod at that, but we often don't act like we believe it. We invest heavily in tooling, automation, and architecture — which are all important — but collaboration tends to get treated as a soft skill, something you either have or you don't.

Accelerate makes a compelling case that collaboration between development, operations, security, and platform teams is one of the *strongest predictors* of delivery performance and organisational health. Not tools. Not talent density. How well teams work together.

## What Collaboration Actually Means

Here's the thing — collaboration isn't meetings. I've been in plenty of organisations that have tons of meetings and very little actual collaboration. What it really comes down to is three things: shared purpose, shared context, and shared accountability.

### Shared Purpose

Shared purpose means everyone understands the customer impact of their work. Platform teams see themselves as enablers, not gatekeepers. Engineers understand *why* golden paths, standards, and guardrails exist — not just that they exist.

Without shared purpose, collaboration collapses into transactional handoffs. You end up with teams optimising for their local metrics, not the mission.

### Shared Context

Shared context means the whole team understands the landscape well enough to make good decisions without waiting for a meeting. It's not about more communication — it's about *better* information.

A lot of cognitive load in platform engineering comes from siloed knowledge. When understanding of how systems work lives in only a few senior engineers' heads, everyone else slows down and decisions get bottlenecked. Good collaboration closes that gap.

### Shared Accountability

Shared accountability means teams own outcomes together. When incidents happen, people swarm to fix them instead of assigning blame. Platform teams treat developers as customers. Developers treat the platform as a contract they help shape and improve. Security, QA, and Ops are engaged early — not at the end when it's too late to make meaningful changes.

In short: collaboration is not a calendar entry. It's a culture. It's the operating system of high-performing engineering organisations.

## Challenges We Actually Face

I want to be honest about what makes collaboration hard in engineering teams, because it's not just people being difficult. There are structural things that get in the way:

- Complex, multi-cloud environments and inconsistent tooling create friction at every join point between teams.
- Tribal knowledge — where understanding lives in a few senior engineers — makes it hard for anyone else to contribute confidently.
- Cognitive load around tooling, pipelines, infrastructure-as-code, and security controls means developers are often overwhelmed before they even start their actual work.
- Reactive culture — where teams wait for escalations rather than co-owning outcomes — kills flow.

Accelerate puts it well: high cognitive load and fragmented ownership kill flow and delivery performance. That's not just a management problem. It's an engineering design problem.

## What Good Collaboration Looks Like

### Clear Contracts and Interfaces

High-performing teams create clear "contracts" between the platform and its consumers — APIs, SLAs, support models, and paved paths. This reduces ambiguity and accelerates delivery. When developers know what they can rely on and where the guardrails are, they can move faster with confidence.

### Platform Teams with a Product Mindset

Platform engineering teams that treat developers as customers — understanding their needs, iterating on the developer experience, and measuring adoption — see significantly higher uptake and better business results. The principle is simple: make the right thing the easy thing.

### Psychological Safety and Blameless Culture

Simon Sinek's "Circle of Safety" is essentially this: people perform their best when they feel protected, not judged. Teams with trust, transparency, and blameless communication deliver better outcomes and innovate faster. Blameless post-incident reviews are a concrete way to build this — they shift the question from "who caused this?" to "what can we learn from this?"

### The Accelerate Principles in Practice

A few principles from Accelerate that I think are worth calling out explicitly:

- **Fast feedback loops** — quick detection and resolution of issues prevents escalation and keeps systems stable. If feedback is slow, problems compound.
- **Small batch work** — limiting work-in-progress enhances flow and simplifies deployment complexity. Big batches hide problems.
- **Reducing handoff friction** — minimising handoffs shortens cycle times and empowers teams to deliver independently. Every handoff is a delay and a potential loss of context.
- **Learning culture** — encouraging blameless post-incident reviews fosters experimentation and organisational resilience.

## What Leadership Enables

This isn't just about individual engineers doing better. Leadership behaviours matter enormously here.

When leaders create a Circle of Safety — protecting their teams rather than exposing them to unnecessary risk or blame — teams thrive. When psychological safety is present, people surface risks early instead of hiding them until they become incidents. When engineers are given real autonomy and their experimentation is supported, you get innovation.

Celebrating learning — not just success — reinforces the right behaviours. An engineer who tried something, failed safely, and learned something valuable is worth more than an engineer who never risks anything. Recognition should reflect that.

## Outcomes Worth Working Towards

When collaboration is genuinely working, you tend to see:

- **Faster delivery and safer changes** — streamlined workflows and reduced coordination overhead mean teams can ship faster without cutting corners on safety.
- **Reduced cognitive load** — golden paths, self-service tools, and consistent documentation mean developers spend less time fighting the platform and more time solving real problems.
- **Better reliability and faster recovery** — shared ownership and rapid feedback loops increase platform reliability and reduce mean time to recovery.
- **Better morale and retention** — strong collaboration builds trust. And trust is what makes people want to stay.

## Where to Start

If I had to leave you with one practical takeaway: pick one new collaboration practice and measure its impact.

That could be introducing blameless post-incident reviews. It could be setting up a clearer SLA between your platform team and your developers. It could be cutting a paved path for a workflow that currently requires tribal knowledge. It could be as simple as changing how your team runs a retrospective.

Collaboration transforms capability into impact. It's how we deliver secure, reliable, user-centred services at scale — not just individually, but as a team.
