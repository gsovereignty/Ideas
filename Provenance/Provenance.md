# Provenance

**One-line pitch:** A decision-graph layer for product teams that answers "why did we decide that?" by stitching Slack threads, meeting notes, PRD versions, and ticket comments into one queryable, time-travel-able trail.
**Source niche:** r/ProductManagement

## Evidence

Three convergent pain points surfaced in 2025-2026 PM/UX-research community discussion threads and tooling reviews citing them:

1. **Decision archaeology is broken across the modern PM stack.** A widely-shared 2025-2026 review of meeting-notes tooling (frequently linked back into r/ProductManagement Notion-vs-Granola debates) crystallised the gap with this exact complaint: *"a week later, you're searching for 'Why did we choose Postgres over MongoDB?' Granola doesn't know. It has your meeting notes, but not the Slack thread, not the GitHub discussion, not the Notion spec."* (Graphlit/Zine vs. Granola comparison, 2026 — https://www.graphlit.com/vs/granola and https://www.zine.ai/vs/granola). The decision is real; it just isn't reachable.

2. **PRDs decay into a non-source-of-truth almost immediately.** From a widely-circulated 2025 piece on "Product-as-Code" pulled into r/ProductManagement discussions: *"decisions drift as the PRD loses its value as a reliable source of truth. Requirements shift and priorities evolve faster than a static artifact can absorb, and new customer signals pile up in separate tools where they rarely influence the next iteration of the PRD."* And from the canonical PM-tooling debate: *"Every PM has written a PRD that was either so vague that engineering built the wrong thing, or so detailed that nobody read it."* (UserVoice, "Is the Product Requirements Document Dead? A Debate" — https://uservoice.com/blog/is-the-product-requirements-document-dead).

3. **PMs spend a working day per week on inbox/feedback triage before any synthesis happens.** From product-feedback tooling research aggregating r/ProductManagement complaints in 2025: *"Product managers spend between 3-5 hours per week on inbox management — reading new feedback, deciding where it belongs, deduplicating items, and updating statuses, which is close to a full working day every week before any actual prioritization happens."* (Userorbit/Hermes feedback-triage analysis, 2025 — https://userorbit.com/blog/hermes-automated-feedback-triage).

A fourth, ambient signal: 2025-2026 r/ProductManagement burnout threads (summarised by LaunchNotes, "The Silent Epidemic: Product Manager Burnout") repeatedly attribute exhaustion not to volume of work but to *re-explaining the same decisions to different audiences* — engineering on Monday, sales on Tuesday, the exec review on Wednesday — because no shared memory exists.

## Problem

The "why" of every product decision is splintered across at least five tools (Slack, meeting-notes app, Notion/Confluence, Linear/Jira, Figma comments), each with private search and no notion of which message *resolved* a question vs. which one *raised* it. When a stakeholder asks six weeks later "why did we deprioritise the export feature?", the PM either:

- Searches four tools, reconstructs the answer from memory, and writes it up again (the "I am the institutional memory" tax), or
- Says "I'll get back to you" and never quite does, and the decision quietly gets re-litigated.

PRDs were meant to be this memory. They aren't, because they're written *before* the decision and almost never updated *after* the conversation that actually changed it. AI PRD writers (ChatPRD, Miro, Chisel, etc.) make the front-end of this problem cheaper but make the back-end worse — more documents, same fragmentation.

## Target user

Senior / staff product managers and group PMs at 50-500-person product orgs, owning 2-4 squads. Specifically the ones who:

- Own multiple cross-functional initiatives where the same decision affects eng, design, GTM, and support,
- Are the *de facto* memory of the team (a recognised burnout signature in 2025 r/ProductManagement threads),
- Already pay for Linear or Jira, Notion or Confluence, Slack, and one of Granola/Fellow/Fireflies/Otter.

Secondary: UX research leads who hit the same gap with interview decisions ("did we already decide not to test that flow? when?").

## Proposed solution

A **decision graph** that sits on top of the tools the team already uses, not another doc store. Three layers:

1. **Capture** — passive integrations with Slack, Linear/Jira, Notion/Confluence, GitHub PRs, Figma comments, and a meeting-notes provider (Granola/Fellow/Fireflies). No new place to write.
2. **Extraction** — an LLM pipeline that, in near-real-time, identifies *decision events* (a question raised → discussion → resolution) and links them to the artefacts they affect (PRD section, ticket, Figma frame, customer interview clip). Each decision gets a stable URL, an author, a "rationale-in-one-paragraph", a "what we considered and rejected", and the linked source threads.
3. **Recall** — a query interface (Slack bot, Raycast, browser extension, MCP server for Claude/Cursor) that answers "why did we…?", "what did we decide about X last quarter?", and "what changed since last week's review?" with citations back to the original thread. Plus a *decision diff* on every PRD: hover any line, see the conversation that put it there.

The product is *not* an AI PRD writer. The PRD becomes a view onto the decision graph rather than the source of truth.

## MVP feature list

- Slack + Linear + Notion ingestion (read-only, OAuth).
- Decision-extraction pipeline: classify messages/comments into `question | proposal | decision | revisit`, group into decision threads, write a one-paragraph rationale.
- Decision page: title, status (`open | decided | revisited`), rationale, alternatives considered, sources (linked threads, with timestamps), affected artefacts.
- Slack `/why <question>` slash command — returns the top 3 decisions with citations.
- "What changed this week" digest — auto-posted to a chosen Slack channel: new decisions, revisited decisions, decisions whose linked PRDs were edited.
- PRD overlay: a Notion / Confluence sidebar that shows the decision(s) behind the section your cursor is in.
- MCP server so Claude/Cursor agents can query the decision graph during coding ("what was decided about retry logic on the export job?").

## Differentiation

- **Not an AI PRD writer.** ChatPRD, Miro AI, Chisel, etc. all generate the *front* of the document. Provenance owns the *afterlife* of the document.
- **Not a meeting-notes tool.** Granola, Fellow, Fireflies own the transcript. Provenance ingests their output and connects it to the rest of the trail. We will partner, not compete.
- **Not a knowledge base.** Glean/Notion AI search returns *documents*. Provenance returns *decisions*, with structure (rationale / alternatives / status) and provenance.
- **Not Productboard.** Productboard organises feature *requests* into a roadmap. Provenance organises *decisions* (including the decisions to *not* build things).
- The defensible asset is the decision graph itself: structure that compounds with team usage, hard for incumbents to retrofit because their core data model is documents or tickets, not decisions.

## Risks / open questions

- **Extraction quality.** False-positive "decisions" are worse than missing them — they create phantom precedent. Need a human-in-the-loop confirm step in v1, with an explicit goal of dropping it as accuracy crosses ~90% on a held-out eval set.
- **Privacy / Slack DMs.** A lot of real decisions happen in DMs. Ingesting them is invasive; not ingesting them leaves a hole. Likely answer: opt-in per user, with a "promote this DM thread to the decision graph" Slack action.
- **Distribution / cold start.** A decision graph is worth nothing on day one. Need a "seed by importing the last 90 days" flow that produces a usable graph before the user has to change behaviour.
- **Buyer.** The PM feels the pain, but the budget often sits with the eng or ops VP. Pricing probably needs to land as per-seat infra ($15-25/user/mo) rather than a tool the PM expenses.
- **Incumbent threat.** Notion or Linear could ship a "decisions" primitive. Mitigation: be the cross-tool layer they can't be without cannibalising their walled gardens, and ship the MCP/agent surface fast — that is where Notion and Linear are slowest.
- **AI agents are a wildcard.** If a meaningful share of "decisions" in 2027 are made by coding agents acting on PRDs, the decision graph needs to ingest agent traces too. Possibly an asset rather than a risk.

## Why now

Three things are simultaneously true in 2026 in a way they were not in 2023:

1. **The PM stack finally stabilised around APIs that allow this.** Slack, Linear, Notion, GitHub, and the major meeting-notes providers all expose the events needed. Three years ago this was a six-integration slog with brittle scrapers.
2. **LLMs cleared the extraction bar.** Reliable classification of "is this message a decision, a proposal, or a question?" with citations was a research problem in 2023 and a paid-API call in 2026. Cost per decision-page is now in the cents, not dollars.
3. **The PM role itself is being rewritten around decisions, not documents.** The dominant 2025-2026 r/ProductManagement narrative — that AI ate the "draft the PRD" part of the job and the surviving high-leverage work is *judgement and decision-making* — points exactly here. As one widely-shared 2025 piece put it: PMs are "trying to be their best selves in fundamentally broken systems." The broken system is decision memory. The artefact-first generation of PM tools (PRD writers, roadmap apps, feedback boards) cannot fix it because their data model is the artefact, not the decision behind it.
