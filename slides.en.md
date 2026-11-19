---
marp: true
theme: aifa
paginate: true
lang: en
title: AI-Friendly Architecture
description: Preparing real systems for AI agents
author: Luiz Schons
footer: 'AI-Friendly Architecture · luizschons.com'

---

<!-- _class: cover -->
<!-- _paginate: false -->
<!-- _footer: '' -->

# AI-Friendly Architecture

Preparing real systems for AI agents

<br>

**Luiz Schons**

<!--
Talk based on the 9-article series at luizschons.com/series/ai-friendly-architecture.
Transition: open with the question. The personal intro comes right after the hook.
-->

---

<!-- _class: statement -->

> Is your software ready to be **understood by an AI**?

<!--
Open with the question. Leave the audience a few seconds to think.
-->

---

<!-- _class: statement -->

> AI is the team's **new hire**.

<!--
The analogy that guides the whole talk.
- It doesn't know the domain, the services, past decisions, or where the answers live.
- With well-documented knowledge, it finds answers on its own and gains autonomy.
- When it depends on a specific person, informal context, or someone's memory, it works worse.
Hold on to this line: we'll come back to it at the end.
Transition: "Before we go on, let me introduce myself."
-->

---

<div class="about">
<div>

# Luiz Schons

<p class="role">Senior Software Engineer @ <b>PicPay</b></p>

<div class="chips">
<span>Guarapuava, Paraná, Brazil</span>
<span>DevParaná</span>
<span>UTFPR</span>
<span>PHP</span>
</div>

<p class="site">luizschons.com</p>

</div>
<img class="photo" src="assets/luiz.jpg" alt="Luiz Schons">
</div>

<!--
Quick intro (30 seconds): who I am, where I work, where I'm from.
Transition: "I'll split the conversation into three acts."
-->

---

## The path

<div class="cols3">
<div class="card"><p class="muted small">Act 1</p><h3>The problem</h3></div>
<div class="card"><p class="muted small">Act 2</p><h3>Knowledge and limits</h3></div>
<div class="card"><p class="muted small">Act 3</p><h3>In practice</h3></div>
</div>

<!--
Three acts, built from the 9-article series.
- Act 1: the problem. The context the agent needs is not in the code: it lives in docs, decisions, and observability.
- Act 2: how to organize that knowledge (skills by context) and how to put limits that don't depend on the prompt (guardrails and fitness functions).
- Act 3: all of it together, in a real task, from the epic to production, and how to know whether the agent did a good job.
A single example runs through the talk: the payments domain, the Payment Service, and a partial-refund feature.
-->

---

<!-- _class: divider -->

<div class="kicker">Act 1 · The problem</div>

# The context is not in the code

<!--
First act: why agents get it wrong in real systems, and why the problem is rarely just the model.
-->

---

## The problem isn't always the model

<div class="compare">
<div class="compare-row">
<div class="compare-in"><span>Swap the model</span><span>Rewrite the prompt</span></div>
<span class="compare-arrow">→</span>
<div class="compare-ctx bad">Bad context</div>
<span class="compare-arrow">→</span>
<div class="compare-out bad">Weak decision</div>
</div>
<div class="compare-row">
<div class="compare-in"><span>Same model</span><span>Same prompt</span></div>
<span class="compare-arrow">→</span>
<div class="compare-ctx good">Good context</div>
<span class="compare-arrow">→</span>
<div class="compare-out good">Confident decision</div>
</div>
</div>

<p class="muted">Often, the problem is the <b>system</b>.</p>

<!--
How to read the diagram: in the top row, we swap the model and rewrite the prompt, but the context stays bad, and the decision stays weak. In the bottom row, we keep the same model and the same prompt, we only improve the context, and the decision becomes confident. The bottleneck is the context system, not the model.

How we got here:
1. We learned to scale systems.
2. We learned to modularize code.
3. We separated responsibilities, distributed services, organized teams by context.
4. Now: build software that AI agents can also understand, not only people.

The usual explanation for AI mistakes is "the model isn't good enough". Sometimes that's true.
But systems are designed for people who ALREADY have context. An in-house dev knows:
- where the relevant documentation lives;
- which decisions sit in old ADRs;
- which service owns each responsibility;
- where to look in an incident;
- which metrics point to a real problem.
The agent knows none of that by default. Context that is scattered, hidden, or inconsistent tanks the quality of the answer.
-->

---

## What the code shows

<div class="cols">
<div class="card good">

### It shows
- Implementation
- Structure
- What exists today

</div>
<div class="card bad">

### It doesn't show
- Why
- Trade-offs
- What was discarded
- Production

</div>
</div>

<!--
The trap: "AI reads code, so it understands the system".
The code doesn't show: why the decision was made, which business problem it solves, which trade-offs were accepted, which parts are sensitive, what was tried and discarded, how it behaves in production.
A mature system lives beyond the repo: docs, observability, incidents, tickets, ADRs, dashboards, the team's memory.
-->

---

## AI-friendly is context…

<div class="chips accent">
<span>accessible</span>
<span>structured</span>
<span>reliable</span>
<span>connected</span>
<span>verifiable</span>
</div>

<br>

<p class="muted">…not exposing everything to the AI.</p>

<!--
Definition: organizing systems so agents can DISCOVER, CORRELATE, and APPLY context with as little friction as possible.
Having the information is not enough: it needs to be findable and combinable.
-->

---

## Five traits

<div class="cols5">
<div class="card big">Living docs</div>
<div class="card big">Recorded decisions</div>
<div class="card big">Observability</div>
<div class="card big">Context by domain</div>
<div class="card big">Connected tools</div>
</div>

<!--
1. Living docs: part of the system, kept up to date; not a graveyard of pages.
2. Recorded decisions: ADRs, RFCs, and notes keep the WHY.
3. Observability as a source of truth: logs, traces, and metrics explain the behavior.
4. Context by domain: each context with its own docs, rules, and references.
5. Connected tools: access to observability, the backlog, the repo, incidents, deploys.
-->

---

## Incident: checkout latency

<p class="muted small">Without context</p>
<div class="flow muted-arrows">
<div class="bad">Wrong docs</div><span>→</span>
<div class="bad">Wrong logs</div><span>→</span>
<div class="bad">Generic hypothesis</div>
</div>

<br>

<p class="muted small">With context</p>
<div class="flow">
<div><code>docs/payments</code></div><span>→</span>
<div>Runbook</div><span>→</span>
<div>Queries</div><span>→</span>
<div class="ok">Focused hypothesis</div>
</div>

<!--
Introduce the domain that will stay with us until the end: payments. Scenario: checkout latency starts to climb (we'll call it INC-2291).
WITHOUT an AI-friendly architecture, the agent depends on luck: it looks for logs in the wrong service, reads dashboards without knowing which metrics matter, wastes time, and suggests generic hypotheses ("add more resources").
WITH organized context the path is direct: it enters the domain docs folder, finds the latency runbook, uses the ready-made queries, and arrives at a focused hypothesis.
In detail:
1. Identifies the affected service (Payment Service)
2. Consults the payments-domain docs
3. Correlates observability signals (provider latency, timeouts, deploys)
4. Finds related architecture decisions (ADRs)
5. Focuses on the most likely hypotheses
It isn't magic: the system was prepared so AI can work with less friction.
Where to start: incidents, onboarding, and changes in critical areas.
-->

---

## Context Architecture

<div class="cols">
<div class="card bad">

### It isn't
- A giant document
- Everything in a vector DB

</div>
<div class="card good">

### It is
- Paths between sources
- Less effort to find things

</div>
</div>

<br>

<p class="muted">Having information is <b>not</b> having context.</p>

<!--
To understand a system, the new hire needs: code, documentation, architecture decisions, tickets and epics, past incidents, dashboards and metrics, conversations with the team.
In most companies the information EXISTS, but it is DISCONNECTED: code in a repo, decisions in a wiki, tickets in another system, incidents in the ops platform, and the key context in a few people's heads.

Definition: a way to organize and CONNECT the knowledge sources that explain a system.
Questions it should answer:
- What is this service responsible for?
- Who depends on it?
- Why was it built this way?
- Which business rule applies?
- How do we know it works?
- What happened the last time this changed?
It doesn't remove the reasoning: it reduces the effort of finding the right information.
-->

---

## A network, not a place

<svg class="network" viewBox="-10 0 1100 440" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Network of connected context sources">
<defs><marker id="arr" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#3b5bdb"/></marker></defs>
<line x1="500" y1="110" x2="730" y2="40" stroke="#c3c8d4" stroke-width="2"/>
<line x1="500" y1="110" x2="730" y2="220" stroke="#c3c8d4" stroke-width="2"/>
<line x1="500" y1="110" x2="960" y2="110" stroke="#c3c8d4" stroke-width="2"/>
<line x1="500" y1="330" x2="730" y2="220" stroke="#c3c8d4" stroke-width="2"/>
<line x1="500" y1="330" x2="730" y2="400" stroke="#c3c8d4" stroke-width="2"/>
<line x1="500" y1="330" x2="960" y2="330" stroke="#c3c8d4" stroke-width="2"/>
<line x1="730" y1="40" x2="730" y2="220" stroke="#c3c8d4" stroke-width="2"/>
<line x1="730" y1="220" x2="730" y2="400" stroke="#c3c8d4" stroke-width="2"/>
<line x1="730" y1="40" x2="960" y2="110" stroke="#c3c8d4" stroke-width="2"/>
<line x1="730" y1="400" x2="960" y2="330" stroke="#c3c8d4" stroke-width="2"/>
<line x1="960" y1="220" x2="960" y2="110" stroke="#c3c8d4" stroke-width="2"/>
<line x1="960" y1="220" x2="960" y2="330" stroke="#c3c8d4" stroke-width="2"/>
<line x1="730" y1="220" x2="960" y2="220" stroke="#c3c8d4" stroke-width="2"/>
<line x1="157" y1="220" x2="199" y2="220" stroke="#3b5bdb" stroke-width="2.5" marker-end="url(#arr)"/>
<line x1="340" y1="190" x2="432" y2="144" stroke="#3b5bdb" stroke-width="2.5" marker-end="url(#arr)"/>
<line x1="357" y1="220" x2="649" y2="220" stroke="#3b5bdb" stroke-width="2.5" marker-end="url(#arr)"/>
<line x1="340" y1="250" x2="432" y2="296" stroke="#3b5bdb" stroke-width="2.5" marker-end="url(#arr)"/>
<rect x="-2" y="192.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="80" y="228" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Task</text>
<rect x="198" y="192.0" width="164" height="56" rx="14" fill="#3b5bdb" stroke="#3b5bdb" stroke-width="1.5"/>
<text x="280" y="228" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="700" fill="#ffffff">Domain</text>
<rect x="418" y="82.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="500" y="118" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Services</text>
<rect x="418" y="302.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="500" y="338" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Docs</text>
<rect x="648" y="12.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="730" y="48" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Decisions</text>
<rect x="648" y="192.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="730" y="228" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Code</text>
<rect x="648" y="372.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="730" y="408" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Tickets</text>
<rect x="878" y="82.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="960" y="118" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Dashboards</text>
<rect x="878" y="192.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="960" y="228" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Incidents</text>
<rect x="878" y="302.0" width="164" height="56" rx="14" fill="#ffffff" stroke="#e4e1da" stroke-width="1.5"/>
<text x="960" y="338" text-anchor="middle" font-family="Inter, sans-serif" font-size="22" font-weight="600" fill="#1d1f24">Runbooks</text>
</svg>

<!--
A single page called "How the company works" goes stale fast.
Each source stays where it works best. What changes: CLEAR LINKS between them.
In the diagram: the task leads to the domain, and the domain is the entry point to services, code, and docs. From there, everything references everything else: the decision explains the code, the dashboard points to the runbook, the incident ties the code to the dashboard, the ticket cites the decision. Nothing is centralized; everything is reachable.
"The goal is not to centralize knowledge. It is to make it easy to find."
-->

---

## Example: the Payment Service page

<div class="wiki">
<div class="wiki-side">
<div class="wiki-space">Payments</div>
<div class="wiki-node open">Payment Service</div>
<div class="wiki-child">Business context</div>
<div class="wiki-child">Decisions (ADRs)</div>
<div class="wiki-child">Contracts</div>
<div class="wiki-child">Runbooks</div>
<div class="wiki-child">Dashboards</div>
<div class="wiki-child">Incidents</div>
</div>
<div class="wiki-page">
<div class="wiki-title">Payment Service</div>
<div class="wiki-meta">Owner: payments team · updated 2 weeks ago</div>
<div class="wiki-links">
<div><span>Code</span><a>payment-service repository</a></div>
<div><span>Decision</span><a>ADR-012 · Partial refund</a></div>
<div><span>Runbook</span><a>Provider failure</a></div>
<div><span>Signals</span><a>Payments dashboard</a></div>
<div><span>Incident</span><a>INC-2291 · Checkout latency</a></div>
</div>
</div>
</div>

<!--
Example in a wiki (Confluence, Notion, whatever the team uses): the "Payments" space has a page for the Payment Service, a microservice in that context.
- The page explains the service's responsibility and shows who the OWNER is and WHEN it was last updated.
- Child pages organize the context: business, decisions (ADRs), contracts (APIs and events), runbooks, dashboards, incidents.
- The links section POINTS to where each thing lives, without copying: the code in the repository, the ADR, the runbook, the dashboard in the observability tool, the incident.
The tool doesn't matter: it can be a wiki, a repository, or a portal. What matters is having an entry point per domain with reliable links.
The agent doesn't read everything: it starts at the service page and follows only the links relevant to the task.
This is the domain of the talk's example: INC-2291 is the checkout latency we just saw, and ADR-012 (partial refund) will come back in Act 3.
-->

---

## Every context needs an owner

<div class="cols4">
<div class="card big">Who?</div>
<div class="card big">When?</div>
<div class="card big">Source?</div>
<div class="card big">Still valid?</div>
</div>

<br>

<p class="muted">Context without an owner becomes abandoned context.</p>

<!--
Documentation without an owner becomes abandoned documentation. A decision with no date and no author becomes a rule with no reason.
- Who is responsible for this context?
- When was it updated?
- Which system is the original source?
- How do we know it is still valid?
"Context with no signal of validity can be as dangerous as no context at all."
ADRs: the code shows the result; the ADR shows the why (problem, options, criteria, trade-offs, expected outcome). Without that, the agent proposes something elegant that ignores a business or operational constraint.
-->

---

<!-- _class: statement -->

> If the answer is **"go ask so-and-so"**, the problem is architectural.

<!--
The new-hire test: take a real task. Can someone who just arrived find out…
- which part of the system needs to change?
- who owns the domain?
- which decisions constrain the solution?
- how to verify that it worked?
- where to investigate if it fails?
The same test applies to agents. Count the steps of a small task: many systems opened or people consulted = a context-architecture problem, not only a documentation problem.
Context follows the flow: task → domains → decisions → docs next to the code → post-deploy observability → lessons back into the knowledge base. No bureaucracy.
-->

---

<p class="kicker-dark">Observability</p>

## Intent vs. reality

<div class="cols">
<div class="card accent big">Code<br><span class="muted small">what should be happening</span></div>
<div class="card good big">Production<br><span class="muted small">what is happening</span></div>
</div>

<!--
A subsection of Act 1: besides docs and decisions, production is also context.
Production reveals: a changed config, a slower dependency, unexpected volume, a rule that fails only with certain data combinations.
Monitoring asks "is something wrong?". Observability asks "why is the system behaving this way?".
Each signal tells part of the story:
- Logs: events and execution details
- Metrics: trends, volumes, changes
- Traces: the path of a request across services
- Deploy events: when a change reached production
- Business data: impact on users
Connected, they let you form and test a hypothesis.
Where it lives: the repository holds instrumentation, signal names, and LINKS to dashboards and runbooks. The platform holds the history.
-->

---

## An incident = a context investigation

<div class="cols3">
<div class="card big">When?</div>
<div class="card big">Where?</div>
<div class="card big">Which payments?</div>
<div class="card big">What changed?</div>
<div class="card big">Which provider?</div>
<div class="card big">What impact?</div>
</div>

<!--
Back to INC-2291: checkout latency starts to climb. Someone with experience knows where to look. Someone new, or an agent, doesn't.
1. When did the problem start?
2. Which service gave the first signal: checkout or the Payment Service?
3. Did it affect every payment, or one payment method (card, Pix)?
4. Was there a deploy or a config change nearby?
5. Did the payment provider get slower?
6. Did it affect any business metric (approved payments, checkout conversion)?
No single dashboard answers all of that.
-->

---

## Logs that tell a story

```json
{
  "service": "payment-service",
  "event": "payment_processing_failed",
  "order_id": "ord_8412",
  "error_code": "timeout",
  "request_id": "req_a91f",
  "timestamp": "2026-09-14T13:02:11Z"
}
```

<!--
"Error processing payment" only helps someone who already knows the code.
- service says who emitted it (Payment Service)
- request_id ties events across services
- order_id shows the impact on a transaction
- error_code lets you group failures
- timestamp lets you compare with deploys and infra changes
Metrics also need meaning: request_count with no unit, origin, filters, or sense of "normal" leads the agent to find the right metric and read it wrong.
Traces connect the architecture map to real executions: where the latency is, which call repeats, where the first error happened.
-->

---

<!-- _class: statement -->

> `200 OK` does not mean the **business** is working.

<!--
Common mistake: looking only at CPU, memory, error rate, and latency.
- A successful response can hide a business step that is failing.
- A few errors can hit key customers.
- A slow queue can destroy the experience.
Connect business events: approved payments, completed orders, processed documents, transactions that needed manual help.
Not every technical alert is a business incident; not every business problem shows up as a technical error.
After a deploy: which metrics to watch, what behavior is expected, how the old compares to the new. The agent helps write AND verify the change.
-->

---

<div class="dash">
<div class="dash-panel">
<div class="dash-head"><span>Status code · checkout</span><span class="dash-legend"><i class="ok"></i>2xx 99.9% <i class="err"></i>5xx 0</span></div>
<div class="dash-kpis"><div><b class="ok">200 OK</b><span>majority</span></div><div><b class="ok">0</b><span>errors</span></div></div>
<svg viewBox="0 0 720 380" class="dash-chart"><line x1="70" x2="700" y1="20" y2="20" stroke="#eceae4" stroke-width="1.5"/><line x1="70" x2="700" y1="98" y2="98" stroke="#eceae4" stroke-width="1.5"/><line x1="70" x2="700" y1="175" y2="175" stroke="#eceae4" stroke-width="1.5"/><line x1="70" x2="700" y1="252" y2="252" stroke="#eceae4" stroke-width="1.5"/><line x1="70" x2="700" y1="330" y2="330" stroke="#eceae4" stroke-width="1.5"/><rect x="73.9" y="92.6" width="18.4" height="237.4" rx="3" fill="#0f9d6b"/><rect x="73.9" y="90.6" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="100.2" y="84.7" width="18.4" height="245.3" rx="3" fill="#0f9d6b"/><rect x="100.2" y="82.7" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="126.4" y="61.5" width="18.4" height="268.5" rx="3" fill="#0f9d6b"/><rect x="126.4" y="59.5" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="152.7" y="67.5" width="18.4" height="262.5" rx="3" fill="#0f9d6b"/><rect x="152.7" y="65.5" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="178.9" y="51.2" width="18.4" height="278.8" rx="3" fill="#0f9d6b"/><rect x="178.9" y="49.2" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="205.2" y="54.5" width="18.4" height="275.5" rx="3" fill="#0f9d6b"/><rect x="205.2" y="52.5" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="231.4" y="65.3" width="18.4" height="264.7" rx="3" fill="#0f9d6b"/><rect x="231.4" y="63.3" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="257.7" y="61.1" width="18.4" height="268.9" rx="3" fill="#0f9d6b"/><rect x="257.7" y="59.1" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="283.9" y="82.7" width="18.4" height="247.3" rx="3" fill="#0f9d6b"/><rect x="283.9" y="80.7" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="310.2" y="84.6" width="18.4" height="245.4" rx="3" fill="#0f9d6b"/><rect x="310.2" y="82.6" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="336.4" y="106.0" width="18.4" height="224.0" rx="3" fill="#0f9d6b"/><rect x="336.4" y="104.0" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="362.7" y="117.0" width="18.4" height="213.0" rx="3" fill="#0f9d6b"/><rect x="362.7" y="115.0" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="388.9" y="118.2" width="18.4" height="211.8" rx="3" fill="#0f9d6b"/><rect x="388.9" y="116.2" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="415.2" y="114.7" width="18.4" height="215.3" rx="3" fill="#0f9d6b"/><rect x="415.2" y="112.7" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="441.4" y="134.7" width="18.4" height="195.3" rx="3" fill="#0f9d6b"/><rect x="441.4" y="132.7" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="467.7" y="130.7" width="18.4" height="199.3" rx="3" fill="#0f9d6b"/><rect x="467.7" y="128.7" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="493.9" y="115.3" width="18.4" height="214.7" rx="3" fill="#0f9d6b"/><rect x="493.9" y="113.3" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="520.2" y="98.6" width="18.4" height="231.4" rx="3" fill="#0f9d6b"/><rect x="520.2" y="96.6" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="546.4" y="96.7" width="18.4" height="233.3" rx="3" fill="#0f9d6b"/><rect x="546.4" y="94.7" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="572.7" y="88.9" width="18.4" height="241.1" rx="3" fill="#0f9d6b"/><rect x="572.7" y="86.9" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="598.9" y="62.5" width="18.4" height="267.5" rx="3" fill="#0f9d6b"/><rect x="598.9" y="60.5" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="625.2" y="75.0" width="18.4" height="255.0" rx="3" fill="#0f9d6b"/><rect x="625.2" y="73.0" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="651.4" y="47.0" width="18.4" height="283.0" rx="3" fill="#0f9d6b"/><rect x="651.4" y="45.0" width="18.4" height="2" fill="#d6455d" opacity="0"/><rect x="677.7" y="56.9" width="18.4" height="273.1" rx="3" fill="#0f9d6b"/><rect x="677.7" y="54.9" width="18.4" height="2" fill="#d6455d" opacity="0"/><g class="ax"><text x="70" y="364" text-anchor="start">14:00</text><text x="228" y="364" text-anchor="middle">14:15</text><text x="385" y="364" text-anchor="middle">14:30</text><text x="542" y="364" text-anchor="middle">14:45</text><text x="700" y="364" text-anchor="end">15:00</text><text x="56" y="28" text-anchor="end">2k</text><text x="56" y="330" text-anchor="end">0</text></g></svg>
</div>
<div class="dash-panel">
<div class="dash-head"><span>Latency · checkout</span><span class="dash-legend"><i class="p75"></i>p75 <i class="p95"></i>p95 <i class="p99"></i>p99</span></div>
<div class="dash-kpis"><div><b class="warn">1.2 s</b><span>p75</span></div><div><b class="warn">2.2 s</b><span>p95</span></div><div><b class="bad">3.7 s</b><span>p99</span></div></div>
<svg viewBox="0 0 720 380" class="dash-chart"><line x1="70" x2="700" y1="20" y2="20" stroke="#eceae4" stroke-width="1.5"/><line x1="70" x2="700" y1="98" y2="98" stroke="#eceae4" stroke-width="1.5"/><line x1="70" x2="700" y1="175" y2="175" stroke="#eceae4" stroke-width="1.5"/><line x1="70" x2="700" y1="252" y2="252" stroke="#eceae4" stroke-width="1.5"/><line x1="70" x2="700" y1="330" y2="330" stroke="#eceae4" stroke-width="1.5"/>
<line x1="70" x2="700" y1="268" y2="268" stroke="#7a7d86" stroke-width="2" stroke-dasharray="8 8"/>
<text x="78" y="258" class="slo">SLO 800 ms</text>
<polyline points="70.0,316.9 96.2,316.9 122.5,316.5 148.8,315.3 175.0,316.8 201.2,315.9 227.5,315.7 253.8,316.3 280.0,315.9 306.2,317.1 332.5,316.1 358.8,313.6 385.0,309.7 411.2,306.9 437.5,303.1 463.8,298.0 490.0,293.3 516.2,288.2 542.5,281.1 568.8,275.1 595.0,269.5 621.2,261.6 647.5,254.4 673.8,245.8 700.0,238.0" fill="none" stroke="#e8b14a" stroke-width="4" stroke-linejoin="round"/>
<polyline points="70.0,306.0 96.2,303.3 122.5,306.7 148.8,305.5 175.0,304.2 201.2,306.5 227.5,305.2 253.8,307.0 280.0,304.5 306.2,304.2 332.5,303.0 358.8,297.9 385.0,294.7 411.2,286.7 437.5,279.4 463.8,270.9 490.0,261.9 516.2,250.0 542.5,238.4 568.8,228.3 595.0,214.9 621.2,203.9 647.5,187.3 673.8,172.8 700.0,156.0" fill="none" stroke="#c27803" stroke-width="4" stroke-linejoin="round"/>
<polyline points="70.0,287.7 96.2,291.0 122.5,290.4 148.8,288.7 175.0,292.7 201.2,289.9 227.5,291.8 253.8,292.1 280.0,292.4 306.2,285.1 332.5,283.1 358.8,274.2 385.0,263.4 411.2,248.8 437.5,240.7 463.8,223.9 490.0,207.6 516.2,188.5 542.5,170.8 568.8,151.3 595.0,134.6 621.2,112.3 647.5,90.3 673.8,63.6 700.0,38.9" fill="none" stroke="#d6455d" stroke-width="6" stroke-linejoin="round"/>
<g class="ax"><text x="70" y="364" text-anchor="start">14:00</text><text x="228" y="364" text-anchor="middle">14:15</text><text x="385" y="364" text-anchor="middle">14:30</text><text x="542" y="364" text-anchor="middle">14:45</text><text x="700" y="364" text-anchor="end">15:00</text><text x="56" y="28" text-anchor="end">4s</text><text x="56" y="183" text-anchor="end">2s</text><text x="56" y="330" text-anchor="end">0</text></g></svg>
</div>
</div>

<p class="muted small">illustrative example</p>

<!--
Illustrative example (fictional numbers) right after the line "200 OK does not mean the business is working".
Left: the panel everyone looks at. Almost everything is 200, zero 5xx. On the technical dashboard, it's all green.
Right: latency for the same checkout. p75 and p95 climb, p99 spikes and blows well past the 800 ms SLO.
The customer doesn't see an error: they see the checkout spinning, give up, and the payment never happens. The business is broken with a 200 OK.
It's the same for the agent: if it only looks at error rate, it concludes "it's healthy". It needs to know which signals matter (latency by percentile, approved payments) and what is normal.
-->

---

## Start with one flow

<ol class="steps">
<li>One critical flow: <b>checkout</b></li>
<li>Correlation ID</li>
<li>Logs, metrics, and traces</li>
<li>What is <b>normal</b>?</li>
</ol>

<!--
You don't have to instrument everything:
1. Pick a flow that matters to the business (in the example: checkout and payment)
2. Guarantee a correlation ID
3. Structure the main logs
4. Create metrics with clear names and meanings
5. Check that traces go through every service
6. Connect deploys, incidents, and config changes
7. Document what is normal and what is an anomaly
The goal: not pretty dashboards, but a system that tells its own story.
Transition to Act 2: "The context exists, but it is scattered. How do we organize it for the agent, and how do we enforce limits?"
-->

---

<!-- _class: divider -->

<div class="kicker">Act 2 · Knowledge and limits</div>

# Organize the knowledge. Set the limits.

<!--
Second act: if the context is scattered, how do we organize it for the agent (skills by context) and how do we guarantee limits that don't depend on the prompt (guardrails and fitness functions).
-->

---

<p class="kicker-dark">Vocabulary</p>

## Four pieces

<div class="cols4">
<div class="card accent big">Agent<br><span class="small muted">coordinates</span></div>
<div class="card big">Skill<br><span class="small muted">guides</span></div>
<div class="card big">Tool<br><span class="small muted">executes</span></div>
<div class="card big">MCP<br><span class="small muted">connects</span></div>
</div>

<!--
Before talking about skills, align the vocabulary. We'll use these four words until the end.
- Agent: coordinates a goal in a loop of context, decision, tool use, and verification.
- Skill: knowledge and workflow for a task (e.g. .opencode/skills/incident-investigation/SKILL.md). It does not grant permissions.
- Tool: an executable capability with a description, an input schema, and a result (e.g. query_metrics with service, metric, from, to).
- MCP: a protocol that standardizes how clients discover and use tools, resources, and prompts from servers.

Tool vs. skill:
Skill: how to think about the problem, when to check each source, how to interpret, which limits to respect.
Tool: which action can run, which inputs it needs, which result it returns.
An observability skill uses several tools (logs, metrics, traces, deploys) and explains how to combine them. Tools can be shared across skills.
A tool can return a time series without saying whether it is normal: interpretation is the skill's job.
The skill does NOT grant permission. What decides whether the tool is available is the harness.
Hyperf MCP example: a read-only service_health tool + a skill that says "don't declare it healthy from a single query".
-->

---

## The agent loop

<div class="flow">
<div>Goal</div><span>→</span>
<div>Context</div><span>→</span>
<div>Decides</div><span>→</span>
<div>Uses tools</div><span>→</span>
<div>Verifies</div><span>↺</span>
</div>

<br>

<p class="muted">The model decides. The <b>harness</b> provides the environment.</p>

<!--
Agent: it receives a goal, checks the context, decides the steps, and uses external capabilities.
It leaves the loop when: it has reached the goal, information is missing, or a person needs to decide.
Creating an agent is not just picking a model. A good agent answers:
1. What problem does it solve? 2. When should it be used? 3. Which contexts does it know? 4. Which actions can it run? 5. Which ones need approval? 6. How does it present results?
OpenCode example: .opencode/agents/ with mode primary, edit denied, incident-investigation skill allowed; "separate facts, hypotheses, and gaps; never apply changes in production".

Harness: the layer around the model that controls what information the agent receives, which tools it uses, how state is kept, which actions need approval, how results are verified.
When the agent fails, often the problem is not the model or the prompt: a tool is missing, a context source, a validation, a clear permission, or a feedback loop.
Agent levels: read-only (inspects), proposal (suggests), execution (applies, with strong controls and approval).
What should last: tools and protocols change; the separation of responsibilities stays. Swap the connection layer, keep skills, rules, and limits.
-->

---

## The context monolith

<div class="cols middle">
<div class="card big" style="font-family: 'JetBrains Mono', monospace">AGENTS.md</div>
<div class="chips">
<span>architecture</span><span>domains</span><span>patterns</span><span>deploy</span><span>observability</span><span>rules</span><span>runbooks</span><span>tests</span><span>every team</span>
</div>
</div>

<br>

<p class="muted">More context is <b>not</b> better context.</p>

<!--
A common reaction: give EVERYTHING to the agent at once: docs, patterns, rules, runbooks, dashboards.
The agent has to figure out on its own what matters, what is reliable, what is related, and what to ignore. Result: noise.
It's the old mistake of concentrating too much responsibility in one place.

A common first step: a central AGENTS.md with everything.
The benefit: it gets the knowledge out of people's heads.
The problem: it grows fast. It becomes hard to know what applies to this task, who updates each part, which instruction has priority.
The cause is not the central file: it is mixing the knowledge of every domain and tool with no boundaries.
-->

---

## What is a skill?

<div class="flow">
<div>Knowledge</div><span>+</span>
<div>Workflow</div><span>+</span>
<div>Sources</div><span>+</span>
<div>Limits</div><span>=</span>
<div class="hl">Skill</div>
</div>

<!--
A unit of knowledge and flow for ONE context. It can explain: domain concepts, which questions to ask first, which sources to use, how to interpret data, limits and security rules, how to present results.
E.g. an incident-investigation skill: identify the service → rebuild the timeline → check signals → separate facts from assumptions → record evidence.
It knows WHERE to look and HOW to use what it finds, without storing every log.
Examples: Datadog, Argo, Databricks, New Relic, incidents, performance, production changes.
-->

---

## The new microservices?

<div class="cols">
<div class="card">

### The microservices era

<div class="flow">
<div>Monolith</div><span>→</span>
<div class="hl">Services by domain</div>
</div>

</div>
<div class="card accent">

### The agent era

<div class="flow">
<div>Giant prompt</div><span>→</span>
<div class="hl">Skills by context</div>
</div>

</div>
</div>

<br>

<p class="muted">Microservices split <b>software</b>. Context Skills split <b>knowledge</b>.</p>

<!--
The analogy (only once): in the microservices era, we broke the monolith and split services by DOMAIN. Now, in the agent era, we do the same with knowledge: we split skills by CONTEXT. We call those skills Context Skills.
The lesson from microservices: a bounded context = vocabulary, rules, responsibilities, contracts, data, and an owner. "Transaction" means one thing in payments and another in orders.
Same goal: don't give too much responsibility to a single piece, whether it's a microservice or a skill.
Technically, a skill is not a microservice: it is not a production service, it has no business API, it is not a runtime boundary, it doesn't store data. The data stays in the systems that own it. The resemblance is in the decomposition.
Documentation explains a subject; the Context Skill explains how to WORK with it. It's a work contract.
Instead of an agent that knows a little of everything: an agent with a general view that loads specialized context when it needs it.
"If we learned to decompose systems by context, why do we still hand context to AI in giant blocks?"
-->

---

## One agent, several contexts

<div class="flow">
<div>Payments</div><span>+</span>
<div>Observability</div><span>+</span>
<div>Delivery</div><span>→</span>
<div class="hl">Hypothesis</div>
</div>

<br>

<p class="muted">The skill gives <b>depth</b>. The agent does the <b>coordination</b>.</p>

<!--
Specialized is not isolated. Each Context Skill has depth in its context:
- Payments: transaction states, refund policies, provider integration.
- Observability: investigations with logs, metrics, and traces.
- Delivery: the deploy flow, environments, approvals.
Example: refund failures climbing. The agent starts in Payments (what is a refund failure?), moves through Observability (anomalous behavior, since when) and Delivery (a recent change).
The agent's conclusion: "failures rose after yesterday's deploy; the Payment Service started sending a different format to the provider; the failures concentrate on refunds created after the deploy."
It still needs to be verified, and the agent reports what is confirmed and what is uncertain. But it's much better than a list of loose facts.
-->

---

## A well-bounded Context Skill

<div class="cols5">
<div class="card big">Vocabulary</div>
<div class="card big">Contracts</div>
<div class="card big">Sources</div>
<div class="card big">Limits</div>
<div class="card big">Ownership</div>
</div>

<br>

<p class="muted">10 skills for a simple question? <b>Wrong boundary.</b></p>

<!--
What a Context Skill knows, with the payments example:
1. Vocabulary: transaction, authorization, capture, refund. E.g. distinguish an AUTHORIZED transaction from a CAPTURED one before any hypothesis.
2. Contracts: APIs, events, and schemas; what requires versioning.
3. Sources of truth: docs, logs, metrics and traces, recent deploys, read-only queries.
4. Limits: read-only, no write commands, nothing sensitive in the output, what needs approval, when to stop and escalate. The skill guides, but it is not the only protection.
5. Ownership: which service does what and who owns the skill, who reviews it when the context changes (e.g. payments-team). This pulls the next slide.

Boundary: decomposing is not creating hundreds of tiny skills.
A good boundary groups what CHANGES together, is USED together, and has a related OWNER.
Test: if this context changes, who needs to review it? If it involves very different teams, the boundary is too large.
Start large (Observability, Delivery, Data, Payments, Orders, Security) and split only when there is a real need.
-->

---

## If everyone owns it, no one does

```yaml
name: payments-context
owner: payments-team
review_frequency: quarterly
sources:
  - payment-service
  - payment-documentation
  - payment-dashboards
```

<!--
If nobody updates the skill when the API changes, the agent works with stale knowledge.
The file doesn't solve maintenance on its own, but it makes the responsibility visible.
Where it lives: with the team's code, in a platform repo, or in a catalog. Make it clear: the owner, which agents use it, external sources, versioning, review, and permissions.
Observability → platform team, with no copies of production data. Payments → the domain team, pointing to ADRs, contracts, and runbooks.
Transition: "The skill guides. But guiding is not guaranteeing."
-->

---

<!-- _class: statement -->

<p class="kicker-dark">Guardrails</p>

> A prompt **guides**. It does not **protect**.

<br>

<p class="muted">The skill guides. The guardrail enforces.</p>

<!--
"Use the production API for reads only." "Never hit admin endpoints."
Rules in the prompt are not a barrier: the agent receives new docs, combines sources, reads the code; the text may have conflicting instructions.
Checks inside the skill or the MCP have the same weakness.
MCP connects agents to systems, but it is not a security boundary. Authorization lives in the service that protects the resource.
The skill GUIDES. The guardrail ENFORCES.
The skill should say: what is read-only, what needs approval, which data can be exposed, when the evidence is insufficient, when to hand off to a human.
BUT limits cannot exist only as an instruction. Fixed guardrails check permissions, arguments, environment, and impact BEFORE the action.
Never assume the agent will follow a security rule on its own.
How to start: pick a recurring problem and watch the expert. Which questions come first? Which sources? How do they interpret? Which hypotheses do they discard? When do they escalate? What output is useful?
-->

---

## Even a read is a risk

<div class="cols3">
<div class="card big">Reachable resources</div>
<div class="card big">Returned data</div>
<div class="card big">Discoverable connections</div>
</div>

<!--
Example: a token that only allows GET in production. Looks safe.
But by reading code, docs, and contracts, the agent discovers admin routes, internal parameters, relationships between resources, and indirect paths to data outside the investigation.
Instead of "GET yes, POST no", ask: which services? which routes? which fields? which records? how long is the credential valid? how is each access audited?
-->

---

## Context without opening production

<div class="cols3">
<div class="card accent big">Scoped token</div>
<div class="card accent big">Derived data</div>
<div class="card accent big">Minimal role</div>
</div>

<br>

<p class="muted">What blocks is the database, not the agent.</p>

<!--
1. A limited-scope token: principal = investigation agent; resource = payments observability API; specific read operations; excludes PII and card data; expires in 15 minutes. The receiving service validates the scope.
2. Derived data: events, metrics, and sanitized logs go to a replica/lake/warehouse with retention, anonymization, field filters, and delay.
3. Least-privilege database: a dedicated role; SELECT only on payment_health and refund_summary; REVOKE on cards and customers. Views hide sensitive columns.
"Don't trust the agent to avoid dangerous tables. The database blocks."
-->

---

## Preparing ≠ executing

<div class="flow">
<div>Signals</div><span>→</span>
<div>Agent investigates</div><span>→</span>
<div>PR + change request</div><span>→</span>
<div class="wait">A human approves</div><span>→</span>
<div class="ok">The pipeline applies</div>
</div>

<br>

<p class="muted">The agent does not have the execution credential.</p>

<!--
Guardrail: a mechanism OUTSIDE the agent's intentions that validates, limits, or blocks. It lives in identity, network, API, database, filesystem, or approval. It is not an "if" in the prompt.
INC-2291 example: timeouts with the provider at checkout → the agent correlates them with a recent timeout/connection-pool change in the Payment Service and forms a hypothesis → it prepares the fix in the config repo, opens a PR and a change request → the responsible person reviews and approves → the pipeline applies it.
The agent does NOT have the execution credential.
Other limits: paths, network destinations, query size, execution time, number of records, data types.
-->

---

## Fitness functions

<p class="muted">An architecture rule that becomes a <b>pipeline test</b>.</p>

<br>

<div class="flow">
<div>Payments</div><span>→</span>
<div class="hl">Shared</div><span>←</span>
<div>Orders</div>
</div>

<br>

<div class="card bad"><h3>Forbidden</h3>Payments importing Orders internal code</div>

<!--
Fitness functions come from evolutionary architecture: turn an important architecture characteristic into an OBJECTIVE, CONTINUOUS check.
They can be: an architecture test, a pipeline rule, static analysis, a metadata query, a periodic check.
They don't use AI to decide: the same input always produces the same result.
The goal is not to judge whether the agent's answer "looks good", but to verify that the architecture still has the properties the team cares about.
Example in our domain: two contexts that still live in the same code (a modular monolith), Payments and Orders. Rule: Payments cannot import Orders internals; the two talk through a public contract (Shared). In the article the example uses Orders and Billing; the idea is the same.
-->

---

## The rule in code

```yaml
# deptrac.yaml
deptrac:
  paths: [./src]
  layers:
    - name: Payments
      collectors: [{ type: classLike, value: 'App\\Payments\\.*' }]
    - name: Orders
      collectors: [{ type: classLike, value: 'App\\Orders\\.*' }]
    - name: Shared
      collectors: [{ type: classLike, value: 'App\\Shared\\.*' }]
  ruleset:
    Payments: [Shared]
    Orders:   [Shared]
```

<!--
Deptrac: a static analysis tool for PHP that defines layers and the allowed dependencies between them.
- paths: where to analyze (src).
- layers: each context is identified by a class-name pattern.
- ruleset: Payments may only depend on Shared; Orders may only depend on Shared. Any other dependency between contexts is a violation.
In the pipeline: vendor/bin/deptrac analyse --config-file=deptrac.yaml
-->

---

## The agent breaks the rule

<div class="cols wide">
<div>

```php
namespace App\Payments;

use App\Orders\Internal\OrderItemRepository;

final class PartialRefund
{
    // ...
}
```

</div>
<div>

```text
$ vendor/bin/deptrac analyse

Payments → Orders
  App\Payments\PartialRefund
  depends on
  App\Orders\Internal\...

Violations: 1
✕ pipeline failed
```

</div>
</div>

<p class="muted">The PR fails, even if the prompt asked for the opposite.</p>

<!--
Scenario (a preview of Act 3): the agent implements the partial refund in Payments and needs the order items. To "solve it fast", it imports an internal Orders repository instead of using the public contract in Shared.
The prompt asked it to respect the boundary. It doesn't matter: Deptrac runs in the pipeline and the PR fails.
The on-screen output is summarized/illustrative; the point is that the violation is detected deterministically.
Deptrac doesn't read the prompt, doesn't interpret intent, doesn't judge code quality. It only checks dependencies against the rules the team defined.
That's why the fitness function closes the protection layer: prompts and skills guide; it verifies the rule automatically on every PR.
-->

---

## Layers of protection

<div class="flow">
<div>Prompt & skill<br><span class="small muted">guides</span></div><span>→</span>
<div>Permissions<br><span class="small muted">limits access</span></div><span>→</span>
<div>Guardrails<br><span class="small muted">blocks actions</span></div><span>→</span>
<div class="hl">Fitness functions<br><span class="small">checks every PR</span></div>
</div>

<!--
AI-friendly is not letting the agent do more. It is giving context without turning every source into a new attack surface.
Permissions: identity, least privilege, scope, audit, revocation.
Fitness functions: they run in the pipeline and verify on every PR that the architecture rules still hold, even months later and even as the code changes.
Transition to Act 3: "Organized context, limits outside the prompt. Now let's see all of this in a real task."
-->

---

<!-- _class: divider -->

<div class="kicker">Act 3 · In practice</div>

# From the epic to production

<!--
Third act: put it all together in a real payments-domain task, the partial refund, and at the end ask: how do I know it worked?
-->

---

<!-- _class: statement -->

> “I want to return **only some items** from the order.”

<!--
Example epic: partial refund.
It looks small, but it touches: order creation, total calculation, who owns the payment, the item ↔ transaction link, cancellation rules, stock, notifications, provider data.
An agent working only from the epic produces a plausible, but wrong, solution.
-->

---

## The whole delivery

<div class="phases">
<div class="phase">
<div class="phase-title">Understand</div>
<div class="phase-step">Question</div>
<div class="phase-step">Spike</div>
<div class="phase-step">Decision</div>
</div>
<div class="phase">
<div class="phase-title">Plan</div>
<div class="phase-step">Success</div>
<div class="phase-step">Tasks and tests</div>
<div class="phase-step">Signals</div>
</div>
<div class="phase hl">
<div class="phase-title">Build</div>
<div class="phase-step">Code</div>
</div>
<div class="phase">
<div class="phase-title">Operate</div>
<div class="phase-step">Deploy</div>
<div class="phase-step">Monitor</div>
<div class="phase-step">Docs</div>
</div>
</div>

<p class="muted">The code is a step, not the delivery.</p>

<!--
The agent takes part in the whole cycle, not only in writing code.
- Understand: question the epic, do the spike, record the decision.
- Plan: define success, break it into tasks, define tests, decide signals and alerts BEFORE the code.
- Build: the code. Notice: it's one column of four, and the smallest.
- Operate: deploy with human approval, monitor against the baseline, update the documentation with what was learned.
Understanding the problem, decisions, success criteria, environments, and signals can take more reasoning than coding.
-->

---

## Ask → spike → decision

<div class="cols3">
<div class="card">

### Questions
Ambiguity becomes a decision

</div>
<div class="card">

### Spike
Evidence, not a polished answer

</div>
<div class="card accent">

### Decision
Options · Choice · Risks

</div>
</div>

<br>

<p class="muted">The agent's first job is to <b>ask</b>.</p>

<!--
Question the epic, don't create files:
- Who can request a refund? Which items are eligible? Is there a deadline?
- Several refunds per order? What if one is already in progress?
- Where does the amount come from? What if the provider accepts and we fail?
- When is stock released? How is the customer notified?
Questioning doesn't block: it turns ambiguity into explicit decisions, early. Open questions go to the PO and the teams. A planning skill guides this.

A spike guided by Context Skills (orders, payments, observability): order model/contract → cancellation flow → provider integration → refund tests → published events → consumers → production signals. Rebuild the flow, don't just find file names.
Recorded decision (example):
- Context: the current flow only refunds the full amount.
- Options: a new transaction type; several refunds on the existing transaction; a separate service.
- Decision: reuse the transaction and track the refunded total.
- Consequences: the provider supports multiple refunds; events identify items; idempotency; old consumers keep working.
- Risks: concurrency; external vs. internal state diverging; consumers without the new format.
-->

---

## Observability before the code

<div class="cols">
<div class="card">

### Signals
Success · failures · divergence

</div>
<div class="card warn">

### A dashboard with no question
= decoration

</div>
</div>

<br>

<p class="muted">Code existing is <b>not</b> the problem solved.</p>

<!--
Success criteria: eligible items can be selected; the refund never exceeds what was paid; no duplicates; correct data at the provider; stock updated; a clear notification; old consumers ok; error and latency stable; fewer tickets; the operation is investigable afterwards.
Watch out: fewer tickets is only success if the error stays stable. Otherwise the problem just hid from support.
Tasks: endpoint, validation, provider, idempotency, event versioning, consumers, tests, QA, metrics, dashboards, runbook. The team confirms the split.
Tests: "what needs to be set up in QA for us to trust this?" If the answer is "nothing", the plan is weak.

For every feature: logs, metrics, traces, queries, success signals, degradation signals, and rollback triggers.
Partial refund: refunds started and confirmed, provider failures, total amount, time to confirmation, divergence between external and internal state, retries, failures by provider/payment method.
The agent helps write queries and dashboards; choosing what to observe is an engineering decision.
Each panel answers: is it being used? does it work? is it slower? does it affect other contexts? should we stop the rollout?
Deploy: the agent prepares the PR, the change record, the plan, and the risks; humans approve according to policy; monitoring compares against the baseline.
-->

---

## The learning cycle

<ol class="steps">
<li>Epic → <b>questions</b></li>
<li>Spike → <b>decision</b></li>
<li>Operation → <b>evidence</b></li>
<li>Docs → <b>more context</b> next time</li>
</ol>

<!--
1. The epic generates questions. 2. The spike generates a decision. 3. The implementation generates new behavior. 4. Operation generates evidence. 5. The documentation records what was learned. 6. The next demand starts with more context.
Skills by stage: Planning, Architecture, Testing, Observability, Delivery. Tools execute concrete actions; MCP connects to the systems.
Humans remain essential: confirm rules, assess trade-offs, approve contracts, accept risks, approve production, own the decisions.
-->

---

## In practice: the request

<div class="chat">
<div class="msg user"><span class="who">You</span>Take this task for me: <a>PAY-1432</a></div>
<div class="msg agent"><span class="who">Agent</span><div class="skills"><span class="skill">planning</span></div>Opening the task…</div>
</div>

<br>

<p class="muted">Just a link. No explanation of the system.</p>

<!--
A simulation of a real conversation with the agent, to tie together everything we've seen.
Notice the input: just the ticket link. You don't explain the system, don't paste documentation, don't describe the architecture.
The question is: how does the agent reach the right context from that? The answer is in how the ticket and the documentation are structured.
-->

---

## In practice: the ticket

<div class="ticket">
<div class="ticket-head"><span class="ticket-key">PAY-1432</span><span class="ticket-type">Story</span></div>
<div class="ticket-title">Partial refund by item</div>
<div class="ticket-body">
<div>
<div class="ticket-label">Description</div>
<p class="ticket-desc">As a customer, I want to return only some items from the order and receive the proportional amount.</p>
<div class="ticket-label">Acceptance criteria</div>
<ul>
<li>The refund never exceeds the amount paid</li>
<li>No duplicate processing</li>
<li>The customer is notified of the confirmation</li>
</ul>
</div>
<div class="ticket-fields">
<div class="ticket-field"><div class="ticket-label">Epic</div><div class="v">Returns</div></div>
<div class="ticket-field"><div class="ticket-label">Team</div><div class="v">Payments</div></div>
<div class="ticket-field key"><div class="ticket-label">Domain</div><div class="v">Payments / Payment Service</div><span class="hint">the context entry point</span></div>
</div>
</div>
</div>

<!--
The ticket is structured: a business-language description, clear acceptance criteria, epic, and team.
The most important field for the agent is DOMAIN: a link to the Payment Service page in the wiki.
That single field is the entry point. Without it, the agent would have to guess where to look.
Practical tip: make this field required in the team's ticket template.
-->

---

## In practice: the domain page

<div class="wiki compact">
<div class="wiki-side">
<div class="wiki-space">Payments</div>
<div class="wiki-node open">Payment Service</div>
<div class="wiki-child">Business context</div>
<div class="wiki-child">Decisions (ADRs)</div>
<div class="wiki-child">Contracts</div>
<div class="wiki-child">Runbooks</div>
<div class="wiki-child">Dashboards</div>
<div class="wiki-child">Incidents</div>
</div>
<div class="wiki-page">
<div class="wiki-title">Payment Service</div>
<div class="wiki-meta">Owner: payments team · updated 2 weeks ago</div>
<div class="wiki-links">
<div><span>Code</span><a>payment-service repository</a></div>
<div><span>Decision</span><a>ADR-012 · Partial refund</a></div>
<div><span>Contract</span><a>Refund API v2</a></div>
<div><span>Runbook</span><a>Provider failure</a></div>
<div><span>Signals</span><a>Payments dashboard</a></div>
<div><span>Incident</span><a>INC-2291 · Checkout latency</a></div>
</div>
</div>
</div>

<!--
The ticket link leads to the same page we saw in Act 1.
The page doesn't copy anything: it POINTS. Code, decision, contract, runbook, dashboard, and incident, each in the system where it lives.
Owner and last-updated date tell the agent whether it can trust this page.
-->

---

## In practice: one link, all the context

<svg class="network" viewBox="0 0 1640 620" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="From the task to the domain page, and from there to every context source">
<defs><marker id="arrc" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#3b5bdb"/></marker></defs>
<line x1="323" y1="300" x2="525" y2="300" stroke="#3b5bdb" stroke-width="3" marker-end="url(#arrc)"/>
<line x1="868" y1="300" x2="1245" y2="45" stroke="#3b5bdb" stroke-width="3" marker-end="url(#arrc)"/>
<line x1="868" y1="300" x2="1245" y2="147" stroke="#3b5bdb" stroke-width="3" marker-end="url(#arrc)"/>
<line x1="868" y1="300" x2="1245" y2="249" stroke="#3b5bdb" stroke-width="3" marker-end="url(#arrc)"/>
<line x1="868" y1="300" x2="1245" y2="351" stroke="#3b5bdb" stroke-width="3" marker-end="url(#arrc)"/>
<line x1="868" y1="300" x2="1245" y2="453" stroke="#3b5bdb" stroke-width="3" marker-end="url(#arrc)"/>
<line x1="868" y1="300" x2="1245" y2="555" stroke="#3b5bdb" stroke-width="3" marker-end="url(#arrc)"/>
<rect x="60.0" y="260.0" width="260" height="80" rx="16" fill="#ffffff" stroke="#e4e1da" stroke-width="2"/>
<text x="190" y="311" text-anchor="middle" font-family="Inter, sans-serif" font-size="30" font-weight="600" fill="#1d1f24">PAY-1432</text>
<rect x="535.0" y="260.0" width="330" height="80" rx="16" fill="#3b5bdb" stroke="#3b5bdb" stroke-width="2"/>
<text x="700" y="311" text-anchor="middle" font-family="Inter, sans-serif" font-size="30" font-weight="700" fill="#ffffff">Payment Service</text>
<rect x="1255.0" y="10.0" width="270" height="70" rx="16" fill="#ffffff" stroke="#e4e1da" stroke-width="2"/>
<text x="1390" y="56" text-anchor="middle" font-family="Inter, sans-serif" font-size="30" font-weight="600" fill="#1d1f24">Repository</text>
<rect x="1255.0" y="112.0" width="270" height="70" rx="16" fill="#ffffff" stroke="#e4e1da" stroke-width="2"/>
<text x="1390" y="158" text-anchor="middle" font-family="Inter, sans-serif" font-size="30" font-weight="600" fill="#1d1f24">ADR-012</text>
<rect x="1255.0" y="214.0" width="270" height="70" rx="16" fill="#ffffff" stroke="#e4e1da" stroke-width="2"/>
<text x="1390" y="260" text-anchor="middle" font-family="Inter, sans-serif" font-size="30" font-weight="600" fill="#1d1f24">Contract v2</text>
<rect x="1255.0" y="316.0" width="270" height="70" rx="16" fill="#ffffff" stroke="#e4e1da" stroke-width="2"/>
<text x="1390" y="362" text-anchor="middle" font-family="Inter, sans-serif" font-size="30" font-weight="600" fill="#1d1f24">Runbook</text>
<rect x="1255.0" y="418.0" width="270" height="70" rx="16" fill="#ffffff" stroke="#e4e1da" stroke-width="2"/>
<text x="1390" y="464" text-anchor="middle" font-family="Inter, sans-serif" font-size="30" font-weight="600" fill="#1d1f24">Dashboard</text>
<rect x="1255.0" y="520.0" width="270" height="70" rx="16" fill="#ffffff" stroke="#e4e1da" stroke-width="2"/>
<text x="1390" y="566" text-anchor="middle" font-family="Inter, sans-serif" font-size="30" font-weight="600" fill="#1d1f24">INC-2291</text>
<text x="190" y="384" text-anchor="middle" font-family="Inter, sans-serif" font-size="24" font-weight="700" letter-spacing="2" fill="#7a7d86">INPUT</text>
<text x="700" y="384" text-anchor="middle" font-family="Inter, sans-serif" font-size="24" font-weight="700" letter-spacing="2" fill="#7a7d86">ENTRY POINT</text>
</svg>

<p class="muted">The input was a link. The context came from the <b>architecture</b>.</p>

<!--
This is the point of the whole talk, in one slide:
- You sent a link (the ticket).
- The ticket pointed to the domain page.
- The page pointed to everything the agent needs: code, decision, contract, runbook, dashboard, incidents.
The agent didn't need a giant prompt or someone explaining the system. It navigated the Context Architecture.
And it doesn't read everything: it follows only the links relevant to the task (here, ADR-012 and the contract).
-->

---

## In practice: the agent asks

<div class="chat">
<div class="msg agent"><span class="who">Agent</span><div class="skills"><span class="skill">planning</span><span class="skill">payments-context</span></div>I read the ticket and the Payment Service page, and followed the links to <a>ADR-012</a> and the <a>refund contract v2</a>. Before I start:
<ol><li>Can there be more than one partial refund per order?</li><li>Is stock released on the request or on confirmation?</li></ol></div>
<div class="msg user"><span class="who">You</span>Several per order. Stock only on confirmation.</div>
</div>

<!--
The agent loads the planning skill and the payments Context Skill.
It shows where it got the context from (traceability) and does its first job: ASK.
The two questions are not answered in the ticket or the ADR. The answers become explicit decisions, which later go back into the documentation.
-->

---

## In practice: from the plan to the PR

<div class="chat">
<div class="msg agent"><span class="who">Agent</span><div class="skills"><span class="skill">architecture</span><span class="skill">testing</span></div>Plan: reuse the transaction and track the refunded total, as in ADR-012. Tests for concurrency, idempotency, and failure after the provider confirms.</div>
<div class="msg agent"><span class="who">Agent</span><div class="skills"><span class="skill">delivery</span></div>PR opened: <a>#482 · Partial refund by item</a>. Tests passing, fitness functions ok. Change-request draft ready.</div>
<div class="msg user"><span class="who">You</span>I sent the PR to the team for approval and I'm lining up the change request. Once everything is approved, I'll ship it and let you know here.</div>
</div>

<!--
- Architecture and testing skills: the plan rests on the recorded decision (ADR-012), not on a new idea. The tests come from the success criteria.
- Delivery skill: the agent PREPARES the PR and the change record. The guardrails still apply: fitness functions run in the pipeline (the Act 2 rule: Payments does not import Orders internals; the PR passed because the agent used the public contract) and a person is the one who approves.
- Preparing ≠ executing: the team reviews the PR and the change request goes through approval. Only after that does the person ship to production through the pipeline.
-->

---

## In practice: deploy and monitoring

<div class="chat">
<div class="msg user"><span class="who">You</span>Change request approved, I shipped to production. Can you watch it for me?</div>
<div class="msg agent"><span class="who">Agent</span><div class="skills"><span class="skill">observability</span></div>Watching the <a>payments dashboard</a>. 30 min after the deploy:
<ul><li>Partial refunds confirmed, no duplicates</li><li>p95 latency stable</li><li>No divergence between provider and database</li></ul>I'll alert if the failure rate goes above 2%.</div>
<div class="msg agent"><span class="who">Agent</span><div class="skills"><span class="skill done">docs updated</span></div>I updated the Payment Service page with the partial-refund runbook.</div>
</div>

<!--
- Observability skill: the agent knows which dashboard to watch because the link is on the domain page. It compares against the baseline and the success criteria defined before the code.
- The alert threshold (2% failure) was defined by the team; the agent watches and alerts, the decision to continue or roll back is human.
- Closing the loop: the documentation is updated, and the next task starts with more context.
END OF THE SIMULATION and of the main content: from the ticket link to production, with the docs updated for the next task.
Transition: "Now a bonus. The feature worked. But what about the agent — did it do a good job?"
-->

---

<!-- _class: divider -->

<div class="kicker">Bonus</div>

# How do I know my agent performed well?

<!--
Make it clear this is a BONUS: the main content ended at the deploy. This is a taste, without going deep.
The feature was measured. The agent needs to be measured too.
Judging only by the result (PR created, tests passing) doesn't explain the path: how much it cost to find the context, how many tool calls, where approval was needed, whether there was rework.
-->

---

## OpenTelemetry as a common layer

<div class="flow">
<div>AI tool</div><span>→</span>
<div>OTLP</div><span>→</span>
<div class="hl">Collector</div><span>→</span>
<div>Prometheus</div><span>→</span>
<div>Grafana</div>
</div>

<br>

<p class="muted">Measure <b>outcome</b>, not only activity.</p>

<!--
Bonus, quickly. An open standard, open source, no proprietary platform needed to start.
It works for OpenCode, Claude Code, Codex, Cursor, or any tool with an exporter, plugin, or hook. Swap the tool without rebuilding observability.
What to measure: sessions, duration, tool calls, tokens and cost, approvals, rework.
The trap: more tokens or more tool calls is not more progress. It can be better context OR a lost agent.
Context shows up in the metrics: a long search → missing context; a repeated query → a missing skill; approval at every step → a poorly defined permission.
Careful: telemetry is also sensitive data (no prompts in metrics, sanitize arguments, defined retention).
Details in the article, next slide.
-->

---

<div class="qr-slide">
<div>

<p class="kicker-dark">Bonus · to go deeper</p>

# Agent observability

<p class="muted">Article + lab with OpenTelemetry</p>

<p class="site">luizschons.com/posts/agent-observability-with-opentelemetry</p>

</div>
<figure class="qr">
<img src="assets/qr-artigo.svg" alt="QR code for the article">
<figcaption>Scan to read the article</figcaption>
</figure>
</div>

<!--
Leave a few seconds for people to scan.
The article has the full lab: Collector + Prometheus + Grafana with docker compose (github.com/sschonss/agent-observability-with-opentelemetry).
Transition: "Closing the bonus, let's go to the takeaways."
-->

---

<!-- _class: divider -->

<div class="kicker">Closing</div>

# Takeaways

---

## Better agents need

<div class="cols4">
<div class="card big">Context<div class="small muted">docs and ADRs</div></div>
<div class="card big">Limits<div class="small muted">guardrails</div></div>
<div class="card big">Evidence<div class="small muted">real signals</div></div>
<div class="card big accent">Feedback<div class="small muted">docs that evolve</div></div>
</div>

<br>

<p class="muted">Less guessing. More <b>architecture</b>.</p>

<!--
Close in one sentence: better agents depend less on guessing and more on context, limits, evidence, and feedback.
- Context: docs with an owner, recorded decisions (ADRs), a domain page.
- Limits: guardrails and fitness functions, outside the prompt.
- Evidence: observability that tells the story (200 OK is not enough).
- Feedback: each task returns knowledge to the docs, and the next one starts with more context.
All of this helps people first. The agent is just the new hire who shows up every day.
Transition: the article series and questions.
-->

---

<div class="qr-slide">
<div>

<p class="kicker-dark">This talk is based on my article series</p>

# AI-Friendly Architecture

<p class="muted">9 articles</p>

<p class="site">luizschons.com/series/ai-friendly-architecture</p>

</div>
<figure class="qr">
<img src="assets/qr-serie.svg" alt="QR code for the series">
<figcaption>Scan to read the series</figcaption>
</figure>
</div>

<!--
To go deeper: the talk is a summary of the 9-article series (luizschons.com/series/ai-friendly-architecture). Leave a few seconds for anyone who wants to scan.
Then go to questions.
-->

---

<!-- _paginate: false -->
<!-- _footer: '' -->

<div class="qr-slide">
<div>

<p class="kicker-dark">Thank you!</p>

# Questions?

<div class="contacts">
<div><span class="contact-label">Site</span><span class="contact-value">luizschons.com</span></div>
<div><span class="contact-label">LinkedIn</span><span class="contact-value">linkedin.com/in/luiz-schons</span></div>
<div><span class="contact-label">GitHub</span><span class="contact-value">github.com/sschonss</span></div>
</div>

</div>
<figure class="qr">
<img src="assets/qr-feedback-phpconf2026.svg" alt="QR code for the feedback form">
<figcaption>Leave your feedback</figcaption>
</figure>
</div>

<!--
Open for questions. Leave this slide up during Q&A: the QR code goes to this talk's feedback form (luizschons.com/pt-br/feedback/?t=187690&e=1019926).
-->
