# Finding Mira — Design Document

**Date:** 2026-07-10
**Status:** Draft for review
**Working title:** *Finding Mira* (case #1 of a possible anthology)
**Format:** Narrative OSINT detective game, 2–4 hours, single case, freeform (no fail state)
**Target platform (indicative, decided at planning):** web, data-driven content, code-light / writing-heavy

---

## 1. Pitch

You've just moved back to your small hometown of **Ozerny** — the kind of place where a question asked at the bakery is town news by dinner. A stranger named **Dan** DMs you: his best friend Mira, the town pharmacist, has been missing for four days, and the police won't act. You have no badge and no training. You have what everyone has: her social media, a search bar, a gallery full of other people's photos, and the fact that in Ozerny, you know everyone.

The game is played from a hybrid interface — a desktop for research, a slide-out phone for DMs — and the core loop is how a real civilian would investigate: scroll feeds, zoom into photo backgrounds, cross-reference timestamps, and, above all, *talk to people*. But Ozerny's gossip network is a live system: everything you tell anyone travels. Choosing who to trust is the game.

**The twist (full spoilers throughout this document):** Mira isn't lost. She fled a stalker three years ago and rebuilt her life under a new name. Dan *is* the stalker — he found her town but not her, and a small town notices strangers, so he recruited the one thing he could never be: a trusted local. You. Every discovery you make and share is guiding him to her.

## 2. Design pillars

1. **The player's own verbs are the trap.** Stalking, charming, cross-referencing — the things the game teaches are the things the antagonist is using the player for. The rug-pull is mechanical, not just narrative.
2. **Information is the only material.** No stats, no inventory, no combat. Every system — trust, suspicion, gossip, endings — is a question of *who knows what*.
3. **Consequences, never fail states.** The player cannot be stopped, only wrong. Every path reaches an ending with a full epilogue; some endings are heavy.
4. **The town is a system the player can learn.** Gossip propagation is deterministic — and so is belief: people absorb what fits what they already think. Leaks are always attributable in hindsight. Mastery feels like understanding people, not beating dice.
5. **Telemetry through prose.** No visible meters. Suspicion, trust, and standing are read through tone, behavior, and rumor — the way real people read them.

## 3. Player-facing description (spoiler-free)

A missing-person mystery you investigate from your screen. Research on a desktop: two fictional social networks, a news archive with a search bar, a map, a photo viewer with zoom, and your case journal. Talk on a phone panel: DMs with the townspeople and the man who hired you, plus the town group chats. Days pass as you act; each morning brings new posts, new rumors, and the consequences of yesterday's conversations. What you tell people travels — choose confidants carefully. There is no game-over; the story always ends, shaped by what you uncovered, what you let slip, and who believed you when it mattered.

---

# PART A — STORY (SPOILERS)

## 4. The truth

Three years ago, **Katya Zimina**, a pharmacist from the city of Vetrogorsk, was stalked by **Artyom Krayev** — a former coworker. A restraining order was granted, expired, and was never renewed; the harassment never crossed a provable line again. Katya stopped trusting the process, moved away, changed her name to **Mira Sokolova**, and built a deliberately boring life in Ozerny: a pharmacy job, a rented room, a curated feed of pastries and sunsets designed to look like a person with nothing to hide.

Recently Artyom finally narrowed her location — not from her accounts, which are clean, but from the background of *someone else's* photo: a bakery's promo shot that caught her reflection in the window. He identified the town but not her address, routine, or current name-usage in local circles. Ozerny notices outsiders; he cannot canvass it in person without becoming the town's topic of the week. So he built "Dan": a warm, grieving best friend with an eight-month-old account, backdated posts, and composite photos "with Mira" assembled from her old public pictures. Then he found a local with reach and no reason to doubt him — the player, freshly returned, connected to everyone.

Mira, however, has good instincts. A week before the game starts, she noticed signals (a probing message to an old contact, a view on a dormant account) and vanished *preemptively* — she is hiding at an unregistered dacha outside town, watching the town chats through a lurker account to learn whether the threat is real. She is safe only as long as she is unfindable. That is the problem: the player is very good at finding people.

## 5. Why the twist is earned

Every lie Dan tells is discoverable from Act 1 for a sufficiently paranoid player:

- His account age (8 months) vs. claimed years of friendship.
- His "shared memories" match Mira's old public posts verbatim — he was scraping, not remembering.
- His composite photos have inconsistent lighting/shadows discoverable in the photo viewer.
- He knows facts the player never told him (the invisible second informant, §14) — his "echo lines" are systematic tells.
- The reflection technique that opens Act 1 (the player geolocates Mira from a background detail) is *exactly* how Artyom found the town — taught early so the reveal lands as "I did the same thing he did."

First-time players will mostly help him. Replayers see seams everywhere. Both experiences are intended.

## 6. Act structure

Acts advance on **key fact tokens**, not timers. Day counts are expected pacing, not hard limits.

**Act 1 — The Favor** (~days 1–3, ~45 min). Public-layer OSINT: Mira's curated feed, the town group chat, geolocating photos, mapping her routine. The game teaches its rule: anything said in the town chat is townwide knowledge by morning. Ends when the player has established Mira's routine and last-seen picture (token: `routine_mapped`).

**Act 2 — The Circle** (~days 4–8, ~60–90 min). The people who actually knew her: her boss, her landlady, her one real friend, the teenager she tutored. Trust-gated access to her work records, her abandoned real account, and — through the friend, or a slower archive path — her old name. Cross-checking Dan's claims turns up rot. Ends when the player learns the old name (token: `mira_real_name`).

**Act 3 — The Turn** (~days 9–11 + endgame, ~45–60 min). Searching "Katya Zimina" unlocks the archive: news items, a forum thread, the expired restraining order, Artyom's face. The verbs invert: the player is now *containing* information — feeding Dan false leads, deciding whether to weaponize the gossip network against him, and trying to reach Mira through the one channel she still checks. The Act 3 midpoint is the **diary confrontation** (§8). How much leaked in Acts 1–2 determines the remaining room to maneuver.

## 7. Cast

| Character | Role | System function |
|---|---|---|
| **Dan** (Artyom Krayev) | The client; the antagonist | Suspicion meter; layered dialogue; echo-line tells; comes to town in Act 3 |
| **Mira Sokolova** (Katya Zimina) | The missing pharmacist | Present through two feeds (curated + real/abandoned); lurker account; dead-drop channel and loyalty test in Act 3 |
| **Rufat** | Pharmacy owner, her boss | Discretion 3 (except pharmacy matters); gates work-schedule records; hates drama |
| **Lyudmila Petrovna** | Her landlady | Discretion 0; the gossip superconductor; fast intel at maximum exposure cost; possible second informant |
| **Aziza** | Mira's one real friend | Discretion 3; the gatekeeper; runs the canary test; only trusted path to the old name and the dead-drop |
| **Timur**, 16 | The kid Mira tutored | OSINT sidekick if won over; leak risk (screenshots everything); lookout in the endgame |
| **Mom** | Player's mother | Free background intel; comic relief; the leak vector the player forgets counts as one; ambient reporter of the player's own routine (§13.1); Dan's beachhead into the town's heart in Act 3; lets Dan into the house; carries the honesty arc and the endgame verdict beat (8.14) |
| **Oleg** *(minor)* | Unemployed townie, chronically in the town chat | Alternate second informant, motivated by Dan's "reward" |

## 8. Set-piece scenarios

All set-pieces are consequences of the systems (Part B), not scripted interruptions. **Core** = always occurs (possibly in variants). **Reactive** = occurs only if system conditions are met.

**Act 1**

1. **Who's asking?** *(core, reactive timing)* — The player's first town-chat questions get them investigated back; screenshots circulate; by evening Mom asks why the market thinks they're police. Mints `player_asking_questions` and demonstrates propagation on the player's own skin.
2. **Only Galina** *(core, scripted)* — The private lesson to pair with the public one: the player tells Mom one mildly embarrassing finding "just between us." She agrees warmly. By morning it is townwide; confronted, she is genuinely baffled — "I only told Galina." Galina told three people. Low-stakes by design (the leaked token is about the player, not Mira): it teaches that trust and discretion are different stats, previews the "between us" spend (§13), and makes Mom's spread rule personal before it becomes dangerous.
3. **The envelope** *(core, scripted)* — Dan sends money "for expenses." Accepting: suspicion −10 now, mints `dan_paid_player` (leverage he can reveal in the endgame Standing fight, unless the player pre-discloses it publicly). Refusing: no leverage, but slower trust with Dan and one info channel delayed. The first trap that doesn't look like a choice.
4. **The reflection** *(core, scripted)* — The player pins Mira's routine not from her posts but from the background of the bakery's promo photo. Teaches the geolocation verb and plants the twist's method.

**Act 2**

5. **Aziza's canary** *(core, reactive)* — Aziza gives the player one deliberately false, juicy detail unique to them. If its token ever appears in the town chat or in Dan's knowledge, she attributes it (only the player knew), trust drops to 0, DM access severed; the old name is then reachable only via Timur's slow archive path (+2 days). The game never announces the test; trustworthy players may never know it happened.
6. **The search party** *(core, scripted)* — The town organizes volunteer searches, coordinated in an open chat Dan reads. Well-meaning neighbors become his search engine ("places already checked" maps). The player's job quietly inverts: making the search *worse* helps Mira. Public sabotage risks Standing; subtle misdirection is the lesson in tradecraft.
7. **The second informant** *(core, reactive selection)* — The player discovers Dan has another local source: **Lyudmila** if her trust ≤1 at Act 2 start, otherwise **Oleg**. Misinformation to Dan now works only if it doesn't contradict the informant's channel; the player must feed the parallel channel, co-opt it, or discredit it.
8. **The second account** *(core, scripted)* — Mira's real, abandoned profile: same dates as the curated feed, opposite emotional truth. The re-read-everything moment; source of the dacha's paper trail (a propane receipt photo).
9. **Timur's screenshot** *(optional, reactive)* — Carelessly shared findings surface in Timur's class chat as local true-crime content. Costs Rufat's trust; handled kindly, converts Timur into the game's most loyal ally (his repair beat is the player taking public blame — Standing −1, Timur trust +2).

**Act 3**

10. **The diary confrontation** *(core midpoint; two variants)* — Trigger: Dan's suspicion ≥70 at any point after Act 2 (*suspicious variant*), or Act 3 day 2 reached with suspicion <70 (*impatient variant*). The player returns from a lead to find Dan in their room — Mom let him in ("such a nice young man; I put the kettle on") — reading the case journal. What Dan learns = the journal's literal diegetic contents at that moment (truth entries and decoy entries as written; omitted knowledge stays safe). Confrontation branches, no fail state: **Play dumb** (thin-ice double agent), **Mask off** (call him Artyom; open war for the town's belief), **Counter-bluff** (claim the journal is bait — only credible if ≥2 decoy entries exist), **De-escalate** (become his asset again and race him). After this scene Dan stays in Ozerny. The aftermath with Mom branches: blow up at her (Mom trust −1; her intel channel dims for 2 days) or explain calmly (she becomes an early-warning asset and recalls that "he asked about your notebook before I let him in" — minting `dan_searched_room`, a usable evidence card).
11. **Dan comes to town** *(core, branches from 10)* — He charms Ozerny in person: helps with groceries, donates to the search fund, cries at the vigil. He works the gossip network better than the player — and he picks a beachhead: **Mom**. Fixes her fence, brings pastries, listens ("such a lonely boy"). Her stance toward him (§10.1) climbs daily and is authored to be the hardest in town to flip. The endgame becomes a war for belief fought in the game's own medium: reputation (Standing, §16) filtered through stances (§10.1).
12. **Mira's test** *(core, reactive mirror)* — Mira, lurking the chats, opens a dead-drop channel to the player (via Aziza at tier 3, or a riskier direct route if Aziza is burned). Her first message is a false location ("the old boathouse"). If that token reaches the town chat or Dan within 2 ticks, the channel goes dark and the *she-runs-again* outcomes lock in. The Aziza scene, faced from the other side.
13. **The dacha problem** *(core, capstone)* — The player can deduce `dacha_location` (propane receipt + Rufat's offhand mention + a map pin). Physically going there costs a day slot, and with Dan in town it adds `dacha_location` to his knowledge *unless* the player runs the lookout beat (requires Timur trust ≥2). The final puzzle is resisting the genre urge to walk to the X on the map: the best outcomes never make the trip and use the dead-drop instead.
14. **Mom's verdict** *(core, endgame beat)* — Before any public move resolves, the player faces the one juror who matters most and is hardest to move: Mom, whose stance Dan has been feeding for days. She argues back with authored defensive lines ("he fixed the fence, and you show me shadows in a photograph?"). Winning requires evidence that passes her stance gate (§10.1) — or the honesty arc (§13.1), which lets truth outweigh proof. The outcome swings Standing by ±1 on its own (the town watches whose side the player's own mother takes) and, if won with the honesty arc complete, unlocks the one-time **teaching-Mom flip** (§13.1).
15. **Turning the town** *(ending path, reactive)* — With a strong truth-tagged evidence file (§15), Standing ≥4, and the town's stance on Dan flipped (§10.1), the player can expose Artyom in the same chats he exploited and watch a small town close ranks. The gossip network was never the villain; endings differ by who is holding it.

---

# PART B — SYSTEMS

## 9. Fact tokens (the universal currency)

Every discrete piece of information is a **fact token**: `mira_had_second_phone`, `dan_account_is_8mo_old`, `dacha_location`, `player_suspects_dan`. Tokens carry **tags**: `juicy`, `mundane`, `pharmacy`, `about_player`, `about_mira`, `critical`. Systems only ever ask: *which agents hold which tokens?* Agents = the town cast (Rufat, Lyudmila, Aziza, Timur, Mom, Oleg, plus Mom's chat-circle: Galina and one other) + `town_chat` + `search_party_chat` + `class_chat` + Dan + Mira + the player.

**Conclusion minting:** when the player holds all prerequisites of a defined combination (e.g., `dan_shared_memories_verbatim` + `mira_old_posts_public` → `dan_is_scraping`), the journal offers a connect prompt. Conclusions are themselves tokens. ~150 tokens total, of which 5–6 are `critical` (old name, dacha, dead-drop, forged-photos proof, informant identity, restraining-order record).

## 10. The gossip network (deterministic)

Each NPC has: **edges** (who they talk to), **spread rules** (deterministic, personality-shaped), and per-fact awareness. Each in-game morning the network **ticks**: every agent applies their rules to every token they learned since their last tick.

| Agent | Spread rule |
|---|---|
| Lyudmila | Spreads any `juicy` token to Mom, the town chat, and (if informant) Dan, next tick. Cannot be suppressed for more than 3 ticks (§13). |
| Mom | Spreads `about_player` and `juicy` tokens to Lyudmila and two chat-circle friends. Never to strangers — but Dan-in-person doesn't read as a stranger to her. |
| Rufat | Spreads nothing except `pharmacy` tokens, which go to Lyudmila (they feud; he corrects her rumors and thereby feeds them). |
| Aziza | Spreads nothing. Ever. |
| Timur | Spreads `juicy` tokens to the class chat; class chat leaks to the town chat after one further tick. |
| Oleg | If informant: relays everything in the town chat to Dan each tick. |
| Town chat | Universal: any token posted there is held by all town agents (and Dan, via his informant or lurking of public channels) next tick. |
| Search-party chat | Public; Dan reads it directly, same tick. |

**Determinism is a pillar:** no dice. The player experiences rules as personality, learns them like a puzzle, and every leak is attributable in hindsight — which is what makes the canary scenes (8.5, 8.12) fair.

**Distortion is cosmetic only.** Wording mutates as tokens hop (authored variants per token, 1–2 each); the token identity never changes. Full-mutation systems are a writing tarpit and are out of scope.

### 10.1 Stances and confirmation bias (deterministic)

Belief obeys the same discipline as gossip: no dice, learnable, attributable in hindsight.

- **Stance** is an integer −2…+2 per (agent, subject). Subjects: **Dan**, **the player**, **Mira's character**. A **town stance** (held by the `town_chat` agent) exists for Dan and the player.
- **Valence:** tokens about a subject carry `+` or `−` valence (~40 tokens tagged).
- **Absorption rule:** a token whose valence matches the agent's stance sign (or arrives at stance 0) is believed — stance steps 1 toward that sign (capped at ±2) and normal spread rules apply. A **dissonant** token is believed only if (a) it is `critical` proof presented first-hand (in person, or with its evidence card attached), or (b) it is the (|stance|+1)-th dissonant token about that subject from *independent sources*. Otherwise the agent holds it as `disbelieved`: it does not spread, does not gate their behavior, and instead emits an authored **defensive line** ("he carried my groceries, and you show me shadows in a photograph?").
- **First impressions:** the first valenced token an agent receives about a subject sets the initial stance sign. Authored priors override: Aziza starts −1 toward anyone asking about Mira (including the player); Lyudmila +1 toward charming men; Mom +1 toward anyone polite to her.
- **Prebunking beats debunking:** because first impressions are a rule, seeding the town's read on "the stranger from the city" *before* Dan arrives is far cheaper than flipping the town after he has charmed it. Timing becomes a weapon the player can learn.
- **Crying wolf:** a public accusation of Dan that fails its evidence gate sets the town stance on Dan +1 and permanently raises his evidence threshold by +1. Premature exposure inoculates the town against the truth.
- **System ripples:** the second informant (8.7) can be neutralized by discredit — a stance attack — instead of feeding; Dan's daily charm (8.11) is stance reinforcement, giving the endgame clock a second dial; defensive lines are the sibling of echo lines (§14) — one shows what the network told Dan, the other shows what the network refuses to hear. Readout stays prose, per pillar 5.

## 11. Time and the action economy

Each day has **three slots** (morning / afternoon / evening). In-person visits and *flagged long conversations* (any DM scene marked `deep`) cost one slot. Desktop research — reading feeds, search, gallery, journal, short DMs — is free; the archive available on any given day is finite, so free reading cannot exhaust the game. Ending the evening (or spending the third slot) ends the day; each morning runs one gossip tick, delivers new posts, and serves the rumor feed (§17). Expected total: 10–12 in-game days.

## 12. Dan's suspicion meter (hidden, 0–100, sticky)

**Rises:** a token he learns via the network contradicts what the player told him (+15 per contradiction); the player goes silent ≥2 consecutive days after day 3 (+5/day); refusing his small asks (+5).
**Falls (only ways):** delivering a report that matches his other channels (−10); accepting the envelope (−10, once). No passive decay — paranoia doesn't fade, and turtling doesn't work.
**Thresholds:** ≥40 — he starts testing (asking questions he knows the answers to); ≥70 — diary scene, suspicious variant; ≥90 — he cuts contact and works the town solo (the player loses the misinformation channel but keeps all other verbs).
**Telemetry through prose:** the number is never shown. His texting style is the readout — tone tiers (§14): warm <40, clipped 40–69, auditing ≥70.

## 13. Trust (per-NPC, 0–5) and the "between us" spend

Trust moves only through information choices: kept promises (+1), being the attributed source of a leak about them (−2 to −5), sharing something real and personal (+1), public defense of them (+1). **Gates:** tier 1 (≥1) they answer DMs; tier 2 (≥3) they volunteer information; tier 3 (≥4) they hand over artifacts (Rufat: schedule records; Aziza: the old name, later the dead-drop; Timur: archive digging and the lookout beat).

**"Keep this between us":** when telling an NPC a token, the player may spend 1 trust with them to suppress their spread rule for that token, permanently. **Exception:** Lyudmila costs 2, and suppression lasts only 3 ticks before she bursts — some people cannot hold secrets, and the game says so out loud.

**Repair beats (one per NPC, authored, costly):** Mom — an honest conversation (cheap; she's Mom). Timur — take public blame for his screenshot (Standing −1, Timur +2). Rufat — cover a pharmacy shift and correct a rumor about him in the chat. Lyudmila — feed her a harmless exclusive. Aziza — nearly unreachable: requires publicly admitting your own leak (Standing −2) *and* Mira vouching through the dead-drop in the endgame.

### 13.1 Mom: the closest edge

Mom is the game's thesis at kitchen-table scale — maximum love, zero discretion — and gets systems of her own:

- **Routine reporting:** each morning Mom mints one mundane `about_player` token describing yesterday's slot pattern ("barely home these days," "on that laptop till two") and tells Lyudmila. This is how Dan "thoughtfully" knows the player had a long day: his care questions are echo-line camouflage (§14). The player is being watched by proxy from Act 1 — and can feel it before understanding it.
- **The honesty arc:** her repair beat (the honest conversation) plus one kept promise unlock — after winning Mom's verdict (8.14) — the one-time **teaching-Mom flip**: her spread rule stops applying to `critical` tokens (only; never generalized, never retroactive). The comedy of "Only Galina" (8.2) earns its payoff: the player character's arc, made mechanical, is teaching their mother discretion.
- **Diary aftermath:** per 8.10, she can end that scene as a casualty or as the early-warning asset who noticed him asking about the notebook.
- Her epilogue postcard tone keys off the honesty arc and the verdict, per §21.

## 14. Dan's dialogue architecture

Authored as **layers**, assembled at runtime, to prevent combinatorial explosion: a **tone skin** per suspicion tier (warm / clipped / auditing) × **content blocks** keyed on his known-token set × the current story beat. **Echo lines** are systematic: whenever Dan holds a token the player never gave him, his next `deep` conversation includes a templated line quoting it ("Someone mentioned she'd been asking about propane refills — you hadn't said that"). Echo lines are the player's only window into the invisible informant and the network's reach; they are generated by rule, not hand-placed.

The **warm tier is a deliberate likability budget**: reciprocal interest in the player ("how's your mom taking you being back?"), running jokes, and scraped-memory anecdotes authored to charm on first read and curdle on re-read — each traces verbatim to one of Mira's old public posts, so the tell and the re-read payoff are the same line. Half his warmth is the network reporting on the player through Mom's routine tokens (§13.1). And the mask never becomes a cartoon: confronted, Dan gets *calmer* — lines shorten, punctuation tightens, and his reveal line is relief, not rage ("…Finally.").

## 15. The journal (deduction aid, trap object, evidence file)

- **Evidence cards** (posts, photos, screenshots) auto-collect.
- **Conclusions** are written only at connect prompts (§9), each offering: **Record the truth** (token tagged `in-journal-true`), **Record a decoy** (authored decoy text; token tagged `in-journal-decoy`, shown to the player with a small player-only marker — externalizing that the character knows which pages they faked), or **Leave it out** (`known-unwritten`; safe from the diary scene, absent from the evidence file).
- **As trap:** the diary scene transfers to Dan exactly the union of `in-journal-true` truths and `in-journal-decoy` texts at that moment.
- **As evidence:** the *turning the town* path requires ≥3 `in-journal-true` critical tokens, including the forged-photos proof.
- The journal never grades or hints toward the twist; it organizes. Freeform pillar preserved.

## 16. Standing (the town's belief, hidden integer)

Fed by: leaks of `about_player` juicy tokens (−1 each), publicly kept promises (+1), search-party conduct (+/−1), the Timur fallout handling, pre-disclosing the envelope (+0 but neutralizes Dan's leverage), and endgame evidence quality. Dan-in-person accrues his own Standing daily from Act 3 (+1/day, capped at +4), and each day also reinforces the town's stance toward him (§10.1) — the endgame belief war has a clock on two dials. Mom's verdict (8.14) swings Standing ±1 by itself: the town watches whose side the player's own mother takes. Standing gates the *Closed Ranks* finale and colors every epilogue.

## 17. The rumor feed (the network working *for* the player)

Each morning, 1–3 rumors arrive through Mom, Timur, or the chats, each with a visible source chain ("Mom ← Lyudmila ← the bakery girl"). Rumors are either **distorted truths** (pointing at a real token the player hasn't found — e.g., "a stranger was asking at the bus station" → Artyom's scouting) or **noise** (authored red herrings). ~30 authored rumors: fixed per-day drops plus reactive ones triggered by network state (if `player_asking_questions` is townwide: "people say you're working for the city police"). The feed makes the town feel alive, drip-feeds leads on the game's clock, and teaches distortion by letting the player *receive* it. Two authored garnishes ride it: **defensive rumors** — stance-generated pushback when disbelieved evidence circulates ("people will say anything about a boy who cries at a vigil") — and 2–3 **memory-slip beats**, where a firsthand detail returns from the network contradicted ("she always walked east… no, west, Galina says west"), teaching that witnesses are not recordings. Both are authored instances, not simulation.

## 18. Search engine and keyword unlocks

The in-game browser has a search bar over a fictional news archive + social index. Queries normalize to a keyword table (~25 entries). Any proper noun the player learns is a potential key; the signature beat is typing **"Katya Zimina"** and watching Act 3 unlock. Other keys: Vetrogorsk, the pharmacy's old scandal, Artyom Krayev (post-reveal), the dacha cooperative's name. Unknown queries return authored filler results so the bar never feels dead. Knowledge is the inventory system.

## 19. Geolocation: gallery zoom + map pinning

Zooming past a threshold on an evidence photo highlights authored **detail hotspots** (a sign, a reflection, a receipt on a table). The player pins the photo to the **town map app**; a correct pin mints the corresponding fact token. Wrong pins carry no penalty; after 3 wrong attempts on one photo, a soft nudge appears in the rumor feed or from Timur. The map doubles as the spatial mind-map of Mira's routine and the dacha deduction. No pixel-hunting: hotspots enumerate on sufficient zoom.

## 20. Interface

Desktop canvas: browser (two fictional social networks, news archive, map), gallery/file viewer with zoom, the journal. A **phone dock** on the right edge slides out for DMs and the three chats (town / search party / class, the last read-only via Timur's screenshots). "Real time" reads as: stories that expire at day's end, typing indicators, and *last seen* statuses that are genuine clues — Mira's abandoned account briefly showing **online 2 min ago** is how the endgame opens. Time advances by actions (§11), never by wall-clock.

## 21. Endings

**Three hidden axes.** **Exposure** — which `critical` tokens reached Dan. **Clarity** — when the player minted `dan_is_artyom` (before the diary scene / at it / never); Clarity does not enter the decision table directly: it determines *which endgame choices exist* (you cannot expose or knowingly deceive a man you haven't seen through). **Standing** — who the town believes when it comes to a fight.

**Finale decision table** (evaluated in priority order at the endgame beat; exhaustive by construction):

| # | Condition | Finale |
|---|---|---|
| 1 | Dan holds `dacha_location` AND the dead-drop warning never reached Mira in time | **Found** — the gut-punch ending: restrained, off-screen, no violence depicted; the horror is a door left open and a feed gone silent. The epilogue carries the town's reckoning. |
| 2 | Dan holds `mira_real_name` AND (the dead-drop leaked OR Mira's test failed) | **Vanishing Twice** — she runs again before he closes in; melancholy aftermath; the player's leaks did this. |
| 3 | Player exposed Artyom publicly AND Standing ≥4 AND town stance on Dan flipped (§10.1) AND journal holds ≥3 `in-journal-true` critical tokens incl. the forged-photos proof | **Closed Ranks** — the town turns the gossip network on him; Mira comes home. |
| 4 | Player, at the final beat, chooses to keep Dan's channel open (knowing double agent) AND Artyom not exposed | **The Long Game** — ambiguous, unnerving; the case never closes, it becomes a job. |
| 5 | Otherwise | **The Quiet Save** — she is safe and gone forever; the town never learns; only the player knows what almost happened. |

**Epilogue mosaic:** after every finale, one postcard scene per NPC (Mom, Timur, Aziza, Rufat, Lyudmila), each in one of 3 tones selected by the player's trust/leak history with that NPC — so identical finales still feel personal. 5 finales + 5×3 epilogue variants.

**No fail state, restated:** dark endings are outcomes with full epilogues. The promise is "you can't be stopped," not "you can't be wrong."

## 22. Content, tone, and safety notes

Stalking is the subject; the game's stance is cautionary, never instructional glamor. Techniques shown are common-knowledge fictionalizations (reverse-image search on fictional networks, background details in photos) — no real-world doxxing tradecraft, tools, or services are named or taught. No graphic violence anywhere; the *Found* finale is handled off-screen and restrained. Dan is never sympathetic-by-design, but he is competent — the horror is his patience, not his menace. Recommended store-page style content note: stalking, harassment, implied threat.

Two presentation rules: the canary tests (8.5, 8.12) are never surfaced — no UI hint, achievement, or post-game recap acknowledges them; and the reveal never turns Dan theatrical — patience, not menace, is the horror (§14).

## 23. Content scope (authoring budget)

| Asset | Estimate |
|---|---|
| Fact tokens (+1–2 wording variants each) | ~150 |
| Social posts across both networks + archives (signal) | ~200 |
| Ambient noise pool (menus, bake-sale posts, school scores, dog photos) | ~120 micro-items |
| Stance table + valence tags | 6 agents × 3 subjects; ~40 tagged tokens |
| Defensive/argue-back lines | ~50 across key NPCs |
| DM trees | 7 full (Dan layered per §14) + 1 minor (Oleg) |
| Rumor table | ~30 |
| Search keyword table | ~25 |
| Evidence photos with hotspot annotations | ~35 |
| Finales + epilogues | 5 + 15 postcard variants |
| News-archive articles | ~12 |

Design target remains 2–4 hours for a focused run; completionists and replayers should expect 5–7. Writing — not code — is the dominant production risk: every fake post must read as a different human. Writing-heavy, systems-light — the deterministic rules replace tuning. All content is data (JSON/YAML-style tables); no generated dialogue, no simulation beyond the daily tick.

## 24. Testing: the sim harness

Because the network, suspicion, trust, and endings are deterministic functions of player actions, the design mandates a **headless simulation harness** from day one: scripted player policies (the Blabbermouth, the Paranoiac, the Dan Loyalist, the Speedrunner, the Perfect Friend) run the full token/tick model without UI. Assertions: every finale is reachable by at least one policy; every canary attributes to the correct source; no `critical` token is unmintable; no fact token is orphaned (unreachable or never referenced); the diary scene fires in both variants; Act gates cannot deadlock (Aziza-burned path still reaches `mira_real_name` via Timur); a Premature Accuser policy triggers crying-wolf and still reaches a finale; a Prebunker policy flips the town stance before Dan arrives; Mom's routine token mints daily; the teaching-Mom flip fires at most once. The harness is a spec requirement, not a nice-to-have.

## 25. Out of scope (YAGNI)

- Full rumor-mutation/paraphrase simulation (cosmetic variants only).
- NPC schedules, locations, or movement simulation — visits are menu choices, not a walking sim.
- Visible meters, karma bars, or UI numbers for suspicion/trust/standing.
- Voice acting, cutscenes, 3D, real-network integration of any kind.
- Case #2 content (concepts B "One of Two" and C "She's Reading This" are noted as anthology candidates sharing this engine; nothing here is built for them specifically).
- AI-generated or procedural dialogue.
- Systemic memory mutation or belief drift beyond the deterministic stance rules (memory-slips are authored beats, §17).
- Journal handwriting/stress rendering (the decoy marker carries that load).

## 26. Decisions log (from brainstorming)

Design/story first, no code yet → missing-person case → stranger-to-inner-circle access arc → player-driven branching DMs → freeform, no fail state → twisty tone, assumptions weaponized → 2–4 hours → Concept A ("you are the weapon") → hybrid desktop+phone interface → small fictional town where secrecy requires choosing whom to trust → surprise-me endings (resolved as §21) → player is a local with history → diary-confrontation midpoint locked in → 13 set-pieces approved → systems review applied: deterministic gossip, action economy, marked decoy journal + conclusion minting, search-keyword unlocks, rumor feed, layered Dan dialogue with echo lines, trust repair + "between us" spend, sticky suspicion, third ending axis (Standing), map-pinning verb, sim harness → external review folded in: confirmation-bias stance layer (§10.1) with first-impression, prebunking, and crying-wolf rules; Mom expanded (Only Galina, routine reporting, Dan's beachhead, Mom's verdict, honesty arc / teaching-Mom flip); Dan's Act-1 warmth specified as likability budget and camouflage; ambient noise pool for signal-to-noise; authored memory-slip beats; canaries never surfaced; calm reveal; resisted: systemic memory mutation, handwriting simulation.
