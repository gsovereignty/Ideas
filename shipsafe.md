# ShipSafe
**One-line pitch:** A pre-launch security and edge-case audit built specifically for vibe-coded apps (Lovable / Cursor / Claude Code / Bolt / v0 / Replit) — point it at a deployed URL or repo and it returns a prioritized "things that will break in production" report a non-engineer can actually fix.
**Source niche:** r/SideProject / r/Entrepreneur

## Evidence

**Quote 1 — solo founder burning months on auth/boilerplate instead of the differentiating product (r/SideProject, July 2025):**
> "Spent 6 months building login screens instead of my actual app. Don't be me."
> Author's takeaway: "the app's value lay in the recovery workflows, not the login screen" — they pivoted to pre-built auth/compliance SaaS only after realising they were rebuilding standard components by hand.
Source: r/SideProject post [Spent 6 months building login screens instead of my actual app. Don't be me.](https://www.reddit.com/r/SideProject/comments/1m3fd5o/spent_6_months_building_login_screens_instead_of/), surfaced and analysed at [vynixal.com — r/SideProject 2025-07-25 analysis](https://vynixal.com/analysis/SideProject/2025-07-25/spent-6-months-building-login-screens-instead-of-my-app).

**Quote 2 — non-technical and semi-technical founders complaining their AI-built app is broken in ways they can't even diagnose (echoed across r/SideProject "love to build but despise marketing/sales"-style threads and r/Entrepreneur vibe-coding retrospectives, mid-late 2025):**
> "Anyone else love to build but despise marketing/sales?" — r/SideProject canonical thread on the maker / commercialisation gap.
Source: r/SideProject post [Anyone else love to build but despise marketing/sales?](https://www.reddit.com/r/SideProject/comments/1ky8qvy/anyone_else_love_to_build_but_despise/), summarised in [vynixal.com — r/SideProject 2025-05-30 analysis](https://vynixal.com/analysis/SideProject/2025-05-30).

**Quote 3 — the production-failure pattern these founders keep running into (industry-wide signal, recirculated heavily across r/SideProject and r/programming in 2025-2026):**
> "170 [out of 1,645 Lovable apps] had vulnerabilities that would expose user data to anyone who looked."
> "The error handling was literally: `} catch (error) { console.log("Something went wrong"); }`" — in a live payment system with no actual logging or recovery.
> "16 of 18 CTOs reported production disasters directly caused by AI-generated code, ranging from performance collapses to data corruption to bypassed subscription systems." (Final Round AI, August 2025)
Source: [The Vibe Coding Hangover Is Real — DEV Community](https://dev.to/paulthedev/the-vibe-coding-hangover-is-real-what-nobody-tells-you-about-ai-generated-code-in-production-399h) and [Vibe Coding Failures: 7 Real Apps That Broke in Production](https://getautonoma.com/blog/vibe-coding-failures).

**Supporting signal:** Vynixal's recurring r/SideProject pain-point analyses ([2025-08-07](https://vynixal.com/analysis/SideProject/2025-08-07), [2025-10-21](https://vynixal.com/analysis/SideProject/2025-10-21)) consistently flag "concerns about accuracy and reliance on AI", "platform / domain suspension risk", and "unhappy-path failure" as the top recurring complaints from solo founders shipping AI-assisted apps.

## Problem

The Lovable / Cursor / Claude Code / Bolt cohort has dramatically lowered the cost of *getting an app deployed* — but the apps shipped this way fail predictably in a small set of categories: exposed Supabase row-level-security policies, leaked API keys in client bundles, missing rate limiting, broken auth recovery flows, runaway LLM cost loops, and "happy-path-only" error handling. The solo founders shipping these apps are often non-engineers (or engineers working far outside their stack) and don't know what they don't know. Generic SAST tools (Snyk, Semgrep) are built for engineering teams, output thousands of unprioritised findings, and require config-as-code workflows the user doesn't have. Nothing in market today says "here are the 7 things about *your specific Lovable/Cursor app* that will hurt you in the next 30 days, in priority order, with copy-pasteable fixes."

## Target user

Solo founders and small teams shipping consumer or B2B-lite SaaS built primarily with AI coding tools (Lovable, Bolt, v0, Replit Agent, Cursor + Claude Code, Windsurf). They have a deployed URL and either Supabase / Firebase / Neon as a backend. They have between 0 and ~2,000 users, are between $0 and $5k MRR, post weekly to r/SideProject, and feel a sharp anxiety the day they take their first paying customer or get featured anywhere with real traffic. Not for engineering teams with a security budget — for the founder who built the app on a Saturday and is terrified to sleep on launch night.

## Proposed solution

The user pastes a deployed URL (and optionally connects a GitHub repo or Supabase project read-only). ShipSafe runs a curated battery of checks tuned to the vibe-coding failure catalogue: client-bundle secret scan, Supabase RLS / Firebase rules permissiveness probe, common auth-flow weaknesses (no rate limit on `/login`, exposed signup endpoints, password-reset token leak), AI-cost runaway patterns (unbounded retry loops, missing token caps), unhappy-path probes (50k-character inputs, expired sessions, double-submit, race conditions), and a Lovable/Bolt-specific config audit. Output is a single report ranked by blast radius × likelihood, with a one-paragraph plain-English explanation per finding and a copy-pasteable prompt-fix tailored to the user's coding tool ("Paste this into Cursor: ..."). Free tier scans the first 5 findings; paid unlocks the full report, weekly re-scans, and a pre-launch "ship-safe badge".

## MVP feature list

- URL-only scan (no auth, no repo) that catches: leaked client-side secrets, open Supabase/Firebase rules, missing rate limits on auth endpoints, mixed-content / cookie misconfig
- Supabase OAuth read-only connector for RLS policy review (highest ROI single integration)
- Findings ranked by *founder-relevant* severity (exposes user data > breaks paywall > breaks UX), not CVSS
- Per-finding "fix prompt" generator that produces a paste-ready prompt for the user's stated coding tool (Lovable / Cursor / Claude Code / Bolt)
- Report format optimised for posting back to r/SideProject as social proof ("ShipSafe says my app is clean" badge)
- Email re-scan reminders 7 / 30 days post first scan
- Stripe paywall at $19 one-time scan / $9 mo unlimited (deliberately impulse-priced)

## Differentiation

Snyk / Semgrep / GitGuardian target engineering orgs, gate behind CLI / CI integration, and produce thousand-line reports — the solo Lovable founder bounces in 30 seconds. Vibe App Scanner exists but is single-vendor (Lovable) and shallow. The Lovable / Bolt platforms themselves have an incentive *not* to highlight how often their output ships with RLS holes. ShipSafe wins by (a) being tool-agnostic and meeting users at their deployed URL, not their repo; (b) ranking findings by founder-blast-radius, not CVE severity; (c) outputting fix-prompts in the exact dialect the user's AI coding tool understands; (d) distributing inside r/SideProject itself, where the customers literally post their broken apps every day.

## Risks / open questions

- Liability if a paid scan misses a vulnerability that later leaks user data — needs explicit "advisory, not warranty" framing and possibly a capped-liability ToS.
- Lovable / Bolt may ship competing native scanners — but they have a conflict of interest, and the tool-agnostic angle plus r/SideProject distribution is defensible.
- How deep can a URL-only scan get without becoming a vulnerability scanner that triggers WAF / Cloudflare blocks? Need to stay on the "passive recon + opt-in active probe" side of the line.
- Conversion from r/SideProject visibility to paid: the sub is famously dopamine-rich but conversion-poor. Mitigation: badge-and-share loop turns each free scan into inbound for the next.
- Recurring revenue depends on re-scan loop being habit-forming; one-time-scan dynamic may dominate.

## Why now

Vibe coding moved from a Karpathy tweet (Feb 2025) to ~36% of new startups being solo-founded by mid-2025, with Lovable, Bolt, Cursor, and Replit Agent producing thousands of newly-deployed apps weekly. The first wave of public security incidents (Lovable's 48-day exposure window, Replit's Jason-Lemkin database wipe, the 10% RLS-vuln rate) hit the news cycle in late 2025. The market has *just* learned the word for this problem ("vibe coding hangover") and is actively searching for a fix — but the only response so far is generic SAST or expensive consultancies. A tool-agnostic, founder-priced, prompt-fix-output scanner shipped in the next 90 days catches that demand at peak willingness-to-pay, before Lovable / Bolt / Cursor build native versions.
