# Paper Trail
**One-line pitch:** An AI scope-creep referee that watches your client Slack/email/Loom threads, auto-flags out-of-scope asks against your signed SOW, and drafts the "this is a change order" reply before you forget you ever agreed to it.
**Source niche:** r/freelance

## Evidence

Three converging signals from 2025-2026 freelancer subreddit discussions and adjacent reporting:

1. **Scope creep is a quantified, unsolved problem.** A 2026 MicroGaps analysis of r/freelance and r/smallbusiness threads concluded that "57% of agencies lose $1,000-$5,000 per month to unbilled scope creep, and 30% lose more than $5,000 per month, with only 1% successfully billing for all out-of-scope work" and that solo freelancers are losing "$7,800-$15,600 per year each to scope creep." The r/freelance thread "Feeling overwhelmed by a web dev project..." is repeatedly cited as the canonical example of scope spiraling on a fixed-price contract. ([MicroGaps, Feb 2026](https://www.microgaps.com/gaps/2026-02-18-ai-scope-creep-detector-freelancers))

2. **The pain is feedback chaos, not contracts.** Coverage of r/graphic_design and r/Talesfromdesigners discussions notes that "when clients choose how to give feedback through spontaneous Slack messages or late-night emails, it leads to scattered and contradictory notes that are hard to keep track of" and that "design by committee" via multiple emailers is the structural cause of revision blowouts. A widely-shared r/freelance thread, "Client wants 47 revisions and refuses to pay," surfaced the now-classic comment: *"the moment you hand over the final work, your leverage disappears completely. You're no longer negotiating — you're begging."* ([CloserConnect, 2025](https://www.closerconnect.com/insights/graphic-design-client-approval-software/), [DEV Community, 2025](https://dev.to/jaysomani/i-asked-reddit-one-question-3200-freelancers-responded-34ii))

3. **AI is reshaping who pays for what, fast.** A r/freelanceWriters post that broke into mainstream press read: *"I literally lost my biggest and best client to ChatGPT today. This client is my main source of income, he's a marketer who outsources the majority of his copy and content writing to me."* The writer added that the client admitted the human was better, but *"he can't ignore the profit margin."* Upwork data shows writing projects fell 32% YoY in 2025, while AI-related freelance work crossed $300M annualized and AI-aware freelancers earn 44% more per hour. ([Real Wealth Concepts, 2025](https://realwealthconcepts.substack.com/p/chatgpt-is-hurting-freelance-writers), [Mediabistro, 2026](https://www.mediabistro.com/go-freelance/freelance-writing-jobs-in-the-age-of-ai-what-the-data-says-and-how-to-position-yourself/))

The unmet need: freelancers don't lose money because they lack a contract template. They lose money because nobody is reading the contract back at them in real time when a Slack message says "oh and can you also..."

## Problem

Solo freelancers and 2-3 person studios sign a clear SOW on day one and then watch it get quietly demolished across 40 Slack messages, 12 Loom comments, three email threads, and one verbal call over the next six weeks. By week four the freelancer can't actually remember whether the second landing page was in scope, the client genuinely believes it was, and the freelancer eats the work because re-litigating it feels worse than just doing it. Every existing tool (Bonsai, Moxie, HelloSign, Notion contract templates) helps you write the SOW. None of them watch what happens after.

## Target user

The 1-3 person creative or technical service business: freelance designers, copywriters, web devs, brand strategists, video editors, fractional marketers. Annual revenue $40k-$250k. Already uses Slack Connect or shared email with at least 2 clients. Has been burned by scope creep at least twice. Charges fixed-fee or retainer (not hourly), because hourly people don't have this problem the same way.

## Proposed solution

A read-only assistant that ingests the SOW once, then connects to the freelancer's client comms surfaces (Gmail, Slack Connect, Loom transcripts, Google Docs comments, optional Zoom transcripts) and does three jobs:

1. **Scope diff.** Every new client message is silently classified as in-scope, ambiguous, or out-of-scope against the SOW deliverables. Out-of-scope asks get a desktop notification within 60 seconds.
2. **Change-order drafting.** When something is flagged out-of-scope, the assistant drafts a polite reply naming the specific SOW clause, quoting the client's exact words, and proposing a change-order amount based on the freelancer's stored rate card. The freelancer hits "send," edits, or dismisses.
3. **Receipts ledger.** Every scope decision (accepted, billed, absorbed) is logged into a per-project ledger that exports to PDF at project close. This is the "paper trail" that wins disputes, justifies rate increases, and feeds the next SOW.

## MVP feature list

- Gmail + Slack Connect read-only OAuth ingestion
- SOW upload (PDF, Google Doc, or paste); LLM extracts deliverables, revision rounds, and exclusions into a structured scope object
- Per-message scope classifier with confidence score and the specific SOW line it matched against
- Desktop + email notification when an out-of-scope ask is detected, with snooze
- One-click change-order draft (email and Slack flavors), inserting client name, exact quote, SOW reference, and dollar figure
- Project-level ledger view: every scope event with timestamp, source message link, and disposition
- End-of-project PDF export ("Scope summary for [Client]") suitable for attaching to the final invoice
- Rate card: stored hourly and per-asset rates used to auto-suggest change-order pricing

Explicitly out of MVP: invoicing, time tracking, contract signing, payments. Integrate with Bonsai/Stripe/HelloSign instead of rebuilding.

## Differentiation

Every adjacent product is in the wrong half of the project lifecycle. Bonsai, Moxie, HelloBonsai, and Harlow help you write the contract. Revisio and CloserConnect consolidate revision feedback inside a design tool. None of them sit on the comms layer where scope creep actually happens, and none of them produce the artifact that ends the argument: a timestamped, source-linked ledger of every ask the freelancer agreed to or pushed back on. Closest analog is something like Loop Email or Superhuman's AI summaries, but those summarize threads; they don't enforce a contract against them.

## Risks / open questions

- **Slack Connect access.** Many clients use their own Slack workspace and don't want to install a third-party app. May need a forwarded-email fallback or a desktop overlay that reads the freelancer's local Slack window.
- **Classifier false positives.** A wrongly-flagged "out of scope" notification that the freelancer fires off as a change order will torch the client relationship. Confidence threshold and human-in-the-loop draft (never auto-send) are non-negotiable.
- **Privacy posture.** Reading client comms is sensitive. Need clear "freelancer's data, not client's data" framing, on-device redaction of PII before LLM calls, and a kill switch per project.
- **Will freelancers actually send the change order?** Many won't, because confrontation is the real product. The ledger value prop has to stand alone even if 70% of flagged items get absorbed; the export at project end is what justifies the next rate hike.
- **AI-era pricing pressure.** If the median freelance writer is being undercut by ChatGPT, do they have $29/mo for a tool? Counter: the survivors charging premium rates are exactly the buyers, because their margin per project is high enough that one prevented scope-creep incident pays for two years of subscription.

## Why now

Three things were not simultaneously true before 2025. (1) LLMs are finally cheap and accurate enough to classify "is this Slack message in scope" at a price point that works for a $20-30/mo SaaS. (2) Slack Connect, Loom, and shared Notion docs have collapsed client comms into a small number of API-accessible surfaces, making ingestion tractable for a solo founder. (3) The AI shock to freelance markets has made survivors hyper-conscious of margin per project; the 44% rate premium for AI-aware freelancers means the buyers exist and they are actively shopping for tools that protect their unit economics. Reddit threads from 2025-2026 show the community has named the problem ("scope creep" is now a tagged flair on r/freelance) but is still solving it with PDF templates from 2018. The wedge is open for about 18 months before a Bonsai or a HelloSign bolts a worse version onto their existing product.
