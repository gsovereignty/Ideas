# Sparkroom

**One-line pitch:** A mobile app that spins up an instant, time-boxed fan chatroom around any specific cultural moment (a tweet, a photo, a TV-show episode, a book a celebrity is reading), so people who think "I wish there was an app to talk about this with other fans" can join one in a single tap.

**Source niche:** Twitter/X "I wish there was"

## Evidence

- "i wish there was an app where i could talk about this with jeremy renner fans" — @koitotwt, https://x.com/koitotwt/status/1857238951423029383
- "I wish there was an app where we could discuss the book he's reading…" — @RawbertBeef (quote-tweeting a Robert Downey Jr. on-set photo), https://x.com/RawbertBeef/status/1922293716775002539
- "Jeremy Renner is in House? I wish there was an app to discuss this" — @spidey_figs, https://x.com/spidey_figs/status/1969169571346108757
- "i wish there was an app where we could talk about this…" — @spideybegins, https://x.com/spideybegins/status/1918121222870573252

The phrasing is so consistent across unrelated accounts (different fandoms, different topics, different months in 2024 / May 2025 / Sept 2025) that it has crystallized into a recurring complaint and meme on X. The shared sentiment: the moment a fan-worthy thing happens, there is no good place to go talk about *that specific thing* with other fans. Reddit threads are slow, Discords require invites and prior membership, X replies get buried, Tumblr is not a chat. A real product gap is hiding inside a meme.

## Problem

Pop-culture and fandom interest is bursty and granular. A photo, an episode airing, a celebrity sighting, a chapter drop — each spawns a 24-72 hour spike of people who want to react with strangers who care about *exactly that thing*. Today they have only:

- X replies (chronological, hostile, rate-limited, full of strangers and karma trolls)
- Reddit (great archive, terrible for live reactions; subreddit has to already exist; mods gatekeep)
- Discord (high friction: find server → get invite → onboard → find channel)
- Group texts with friends who may not care
- Nothing for niche moments below the threshold of having a dedicated subreddit

The wish "I wish there was an app to discuss this" is repeatedly typed by the very people the meme should have served — proof that none of the above options solved the job.

## Target user

Primary: 18-35 year-old "extremely online" pop-culture fans (Marvel/MCU, K-pop, BookTok readers, Formula 1, anime, sports, niche celebrity watchers). Heavy X and TikTok usage, comfortable with parasocial relationships, identifies strongly with multiple fandoms.

Secondary: Sports live-event watchers, reality-TV viewers, podcast listeners — anyone whose interests are bursty and benefits from real-time community reaction.

## Proposed solution

A mobile-first app where any tweet, news article, photo, episode, or book can become a live "Spark" — an ephemeral, topic-scoped chatroom that auto-decays after 72 hours unless interest sustains it.

Core flow:

1. User pastes an X URL, news link, or types a topic ("Jeremy Renner cameo on House"). Optionally shares from anywhere via the iOS/Android share sheet.
2. App detects whether a Spark already exists. If yes, joins. If no, creates one with the linked media as a pinned header.
3. User picks an anonymous fandom-tagged handle ("AvengersFan2842") and is dropped into a live thread with everyone else who tapped the same Spark.
4. Spark auto-archives after 72 hours of inactivity into a searchable read-only history. Active Sparks bubble up on a discovery feed grouped by fandom.

The product's hard work is auto-merging duplicate Sparks (everyone arriving at the same moment from different links lands in the same room) and recommending the right Spark when a user opens the app right after seeing something elsewhere ("we noticed you just liked this tweet — there's a room for it").

## MVP feature list

- iOS share-sheet extension: share any tweet, link, or photo into the app to create or join a Spark
- Auto-deduplication of Sparks via URL canonicalization, image hashing, and topic embedding similarity
- Live chat (text, reactions, image replies) with low-latency message delivery
- Anonymous fandom-handle system; one persistent identity per fandom but no real-name profile
- Auto-archive after 72 hours; archived Sparks remain searchable and read-only
- Discovery feed: trending Sparks per fandom, with fandom selection on onboarding
- Push notifications when a Spark you joined hits a milestone (100 new messages, a celebrity replies, a major plot twist confirmed) — opt-in
- Lightweight moderation: rate limits, automated slur filter, user-flagging, instant per-Spark mod from the creator
- No DMs at launch; channel-only to avoid moderation hell

## Differentiation

Discord requires planning. Reddit requires a pre-existing community. X replies are public broadcast, not conversation. Group chats are private. Sparkroom is the only product where:

- The unit of community is the *moment*, not the *fandom* — you don't need to commit to a Discord server to react to one episode
- Time-boxed by default — chat dies in 72 hours, removing the social weight of a permanent identity and lowering the bar to participate
- Zero-friction joining via share sheet — same friction as quote-tweeting, except the room has the right people in it
- Anonymous-but-fandom-tagged identity — strangers feel safe but you can tell a Marvel fan from a K-pop fan

Existing fan apps (Stan, Weverse) are owned by labels and locked to one artist. Reddit and Discord are too heavy. X replies are not a chat. The white space is real-time, ephemeral, share-sheet-summoned, fandom-tagged conversation.

## Risks / open questions

- Cold start: a Spark with three strangers feels worse than X replies with thousands of randos. Need to seed Sparks in the top 50 fandoms before public launch (paid micro-influencer farm, or scrape-and-stub strategy).
- Moderation cost is the existential risk. NSFW fandoms, parasocial obsessions, doxxing of celebrities, K-pop wars all live here. Need ML moderation from day one, plus per-Spark mod tools.
- Monetization: free chat, paid persistent identity / paid revival of an archived Spark / paid official-fandom badges sold to fan clubs. Avoid ads — they kill the vibe in this category.
- Legal: hosting fan chat about real public figures is fine; hosting harassment isn't. Need clear policy and fast takedown.
- Defensibility: once it works, X / Reddit / Discord can ship a feature that copies it. Edge is share-sheet UX, dedup quality, and brand within fandoms — speed and taste, not tech moat.
- Open question: do we let fans link their X account to import who they already follow, or keep it pure-anonymous? Likely a toggle, default off.

## Why now

1. The meme itself is the timing signal: in late-2024 through 2025 the phrase "I wish there was an app to discuss this" has become a recognizable template across unrelated fandoms on X. People are typing the spec at us.
2. Subreddit apathy and Discord fatigue are at all-time highs. The 2023 Reddit API changes pushed power users out, and Discord's 200-channel servers have become as overwhelming as the forums they replaced.
3. Cheap real-time infrastructure (Cloudflare Durable Objects, Liveblocks, Convex, Pusher) makes ephemeral live-chat-per-URL economically viable for a small team in a way it wasn't five years ago.
4. Image hashing and topic embeddings (CLIP, OpenAI/voyage embeddings) are now good enough and cheap enough to auto-merge "everyone reacting to the same photo" reliably, which is the core technical feat.
5. iOS share-sheet and App Intents in iOS 18+ make "summon a chatroom from anywhere" a one-tap UX that wasn't possible without OS-level deep-linking until recently.
