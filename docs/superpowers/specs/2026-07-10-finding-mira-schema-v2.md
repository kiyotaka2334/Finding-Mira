# Finding Mira — Section 2: Schema Design (v2)

**Status:** Consolidated from audit A (structural review) and audit B (review of A). Supersedes the v1 two-schema draft. All balance numbers cited from the design doc (§10–§16) appear here only in the rules table.

---

## 2.0 Principles

1. **The engine owns every number.** Content emits *semantic events* from a closed vocabulary; a single rules table maps events to suspicion/trust/standing/stance math, thresholds, cooldowns, and once-only semantics. No balance constant appears in authored content. Contradiction detection, silence tracking, report-matching, and repeated-lie handling are engine computations, never dialogue outcomes.
2. **False information is first-class.** Every token carries `truth:`. Aziza's canary, Mira's boathouse test, and journal decoys are ordinary tokens that happen to be false. Validators reason about evidence gates, crying-wolf, and ending conditions without special cases.
3. **Everything the harness must simulate is data.** That now includes **agents** (edges, spread rules, stance priors, defense lines) and **scenes** (triggers, costs, variants) — not just tokens and dialogue. "All content is data" was breaking at the most important table; it no longer does.
4. **Conditions and events are typed, closed vocabularies.** Extensible only by adding a *type* with a defined schema — never by expressions, string DSLs, or eval. Every condition and effect is statically checkable in phase 4.

---

## 2.1 Fact tokens (revised)

```yaml
# content/tokens/mira.yaml
- id: dacha_location
  truth: true                      # required on every token (principle 2)
  tags: [critical, about_mira]
  valence: []                      # no stance relevance for this one
  summary: "Mira is hiding at the unregistered dacha"
  evidence: card_propane_receipt   # firsthand-proof card; required on critical tokens
                                   # (enables the §10.1 dissonant-belief exception)
  wordings:                        # network hop variants (§10)
    - "she's been buying propane out by the co-op"
    - "someone's living at one of the old dachas"
  minted_by:                       # typed sources; ≥1 path required if critical
    - deduction: [propane_receipt, rufat_dacha_mention, map_pin_dacha]
  decoy:                           # allowed ONLY on deduction tokens (validator rule)
    text: "She took the northbound bus on the 4th"
    # compiler derives token `dacha_location__decoy`:
    #   truth: false, contradicts: [dacha_location], tags minus `critical`
  echo: default                    # templated echo (see 2.4); or override:
                                   #   echo: {warm: "...", clipped: "...", auditing: "..."}
  editor:
    expected_act: 3                # authoring hint only; validator WARNS if the
                                   # computed earliest-mint act differs
```

A stance-relevant token, for contrast:

```yaml
- id: dan_photos_forged
  truth: true
  tags: [critical, juicy]
  valence:
    - {subject: dan, sign: "-"}    # directed: negative ABOUT DAN
  summary: "Dan's photos with Mira are composites"
  evidence: card_composite_shadows
  wordings:
    - "the shadows in his beach photo point two different ways"
  minted_by:
    - hotspot: {photo: photo_dan_mira_lake, hotspot: shadow_mismatch}
```

**Field notes.**

- **`truth`** — required. False tokens spread through the network like any other; they never satisfy evidence gates, never count toward `in-journal-true` requirements, and a public accusation built on one triggers the crying-wolf rule (§10.1).
- **`valence`** — a list of `{subject, sign}` pairs. Subjects are the closed set `{dan, player, mira}`. A token may be valenced toward more than one subject (e.g. `dan_paid_player` is negative about both).
- **`evidence`** — points at the evidence card that proves the token firsthand. Required on `critical` tokens (validator-enforced); optional elsewhere. Cards are their own small schema: id, kind (post / photo / screenshot / document), and for photos the §19 hotspot annotations.
- **`minted_by` source types (closed):** `deduction` (a conclusion; the journal's connect prompt is generated from this field — one schema, one validator path, unchanged from v1), `pin` (map pinning, §19), `hotspot` (photo-viewer discovery, §19), `search` (keyword unlock, §18), `scene` (scripted set-piece grant), `routine` (see below). **Dialogue grants are not listed here** — blocks declare `mints:` and the validator unions block-side grants with token-side sources when proving reachability. Reachability is a global graph property, not a per-token declaration.
- **Mom's routine tokens** — authored, not generated: 3–4 tokens keyed to slot-pattern classes (`routine_barely_home`, `routine_late_nights`, …), each `minted_by: [{routine: pattern_class}]`. The engine *selects* one per morning from the player's previous-day slot pattern. Authored content stays finite and enumerable; the rule only chooses.
- **`decoy`** — legal only where `minted_by` contains a `deduction` source. The compiler derives a sibling token `<id>__decoy` with `truth: false` and `contradicts: [<id>]`. This makes decoys real network objects: the diary scene transfers them to Dan as tokens + claims (see 2.3), so a decoy caught out later raises suspicion exactly like a spoken lie — no special path.
- **`wordings` selection is deterministic:** `variant = hash(token_id, hop_count) mod len(wordings)`. Stable across harness runs (pillar 4's attributability holds), varied across tokens (better than `hop mod n`, which would sync all tokens' variants).

---

## 2.2 The rules table

The only file in the project containing balance numbers.

```yaml
# engine/rules.yaml
suspicion:                          # §12
  start: 0
  events:
    contradiction:      +15         # engine-detected via claims ledger (2.3), deduped per token
    silence_per_day:    +5          # engine-detected: ≥2 consecutive silent days after day 3
    refuse_ask:         +5
    matching_report:    -10         # engine judges the match (see `report` event, 2.7)
    accept_envelope:    {delta: -10, once: true}
  tiers: {warm: [0, 39], clipped: [40, 69], auditing: [70, 89], cut: [90, 100]}

trust:                              # §13
  events:
    kept_promise:       +1
    personal_share:     +1
    public_defense:     +1
    attributed_leak:    {by_tag: {critical: -5, juicy: -3, default: -2}}
  gates: {dm: 1, volunteer: 3, artifact: 4}
  between_us: {base_cost: 1}        # per-agent exceptions live in the agent schema

standing:                           # §16
  events:
    about_player_leak:  -1
    public_promise_kept: +1
    search_conduct:     {good: +1, sabotage_caught: -1}
    take_public_blame:  -1          # Timur repair beat
    moms_verdict:       {won: +1, lost: -1}
  dan_daily: {delta: +1, cap: 4, active_from: dan_in_town}

stance:                             # §10.1
  cap: 2
  dissonant_threshold: abs_stance_plus_one_independent_sources
  firsthand_exception: critical_token_with_evidence_card
  crying_wolf: {town_stance_dan: +1, dan_evidence_threshold: +1}
```

Tuning `+15 → +10` is now a one-line change. The exact `attributed_leak` by-tag mapping is a balance decision that finally has exactly one home.

---

## 2.3 The claims ledger (engine state, not schema)

The fix for v1's biggest defect: dialogue no longer knows suspicion exists.

- **State:** `ledger[agent][token] = {verdict: affirmed|denied, tick, held_at_claim: bool}`.
- **Writes:** a `tell` event records `affirmed` and transfers the token to that agent (triggering their spread rules). A `claim … denied` event records `denied` and transfers nothing. The **diary scene's journal transfer writes `affirmed` claims for every transferred entry — decoys included.**
- **Contradiction rule (engine, evaluated each tick):** Dan holds token T ∧ `ledger[dan][T].verdict == denied` ∧ `held_at_claim == true` → emit one `contradiction` event (deduped per token). Decoys work through the derived `contradicts:` link: if Dan holds `dacha_location__decoy` as affirmed and later acquires `dacha_location`, contradiction fires.
- **The honest-denial bug is closed by construction:** "First I'm hearing of it" is one authored line. If the player genuinely didn't hold the token, the ledger records `held_at_claim: false` and no event can ever fire. Content cannot get this wrong because content no longer expresses it.
- This is also, for free, the double-agent mechanic: feeding Dan false leads = `tell`-ing him `truth: false` tokens that contradict nothing he holds. The engine's `report` matching and contradiction rules already produce the right behavior with zero extra content.

**Network-path contradictions now work** (v1's silent failure mode): the rule reads Dan's holdings however he got them — informant, town chat, search-party chat, diary — not just dialogue.

---

## 2.4 Dialogue blocks (revised)

Same layered assembly model as v1 (tone skin × content blocks × beat; scenes are ordered selection queries over blocks). Three changes: `effects` → `events`, `requires` → typed conditions, and suspicion is referenced only by **tier** — content never sees the number, matching pillar 5.

**Echo lines are now fully generated.** Three tone templates consume the token's existing `wordings`; the scene query injects one synthetic echo block per token Dan holds that the player never gave him. The template's standard responses ride along:

```yaml
# content/dialogue/dan/echo_templates.yaml
warm:     "Someone mentioned {wording} — you hadn't said that. Everything okay out there?"
clipped:  "{wording}. You didn't mention that."
auditing: "Why am I hearing about {wording} from other people?"
responses:
  - text: "First I'm hearing of it."
    events: [{claim: {verdict: denied}}]        # token bound by the injected block
  - text: "I was about to tell you."
    events: [{tell: {agent: dan}}]
```

Three templates replace ~50 authored blocks, and the echo line literally exhibits the network hop that produced it (the `{wording}` slot uses the variant that actually reached Dan). **Override path (B's addition, adopted):** a token whose templated echo reads wrong sets `echo: {warm: …, clipped: …, auditing: …}` on the token itself. Best of both.

An *authored* Dan block, for shape:

```yaml
# content/dialogue/dan/blocks.yaml
- id: probe_search_party
  beat: act2
  requires:
    knowledge: {agent: dan, has: [search_party_forming], lacks: [dacha_location]}
    suspicion: {tier_at_most: clipped}
  tone_variants:
    warm:    "A search party — that's the town I hoped she'd found. Where are they starting?"
    clipped: "Where is the search starting?"
  responses:
    - text: "North shore, tomorrow."
      events: [{tell: {agent: dan, token: search_route_north}}]
    - text: "Still being organized."
      events: [{claim: {token: search_route_north, verdict: denied}}]
```

**Defense lines are the symmetric injection** (both audits agree): when the stance system holds a token as `disbelieved` and the player presses it, the engine injects the agent's defense line (authored in the agent schema, 2.5) with the same `{wording}` slot. Same mechanism, opposite trigger — echo lines show what the network told Dan; defense lines show what it refuses to hear.

**The other seven NPCs:** unchanged from v1 — same block schema, mostly linear `next:` chains, `trust:` conditions instead of tone skins.

---

## 2.5 Agents (new schema)

The spread system is the game's defining mechanic; it is now data the harness can enumerate.

```yaml
# content/agents/lyudmila.yaml
- id: lyudmila
  kind: npc
  edges: [mom, town_chat, dan]
  spread:
    - match: {tags: [juicy]}
      to: [mom, town_chat]
      tick_delay: 1
    - match: {tags: [juicy]}
      to: [dan]
      tick_delay: 1
      when: {flag: informant_is_lyudmila}
  suppression: {cost: 2, bursts_after_ticks: 3}     # the §13 exception, on the exception
  informant_if: {trust: {npc: lyudmila, at_most: 1, checked_at: act2_start}}
  stance_priors: {dan: +1}                          # "charming men"
  defense_lines:
    dan:
      - "he carried my groceries, and you show me shadows in a photograph?"
  repair_scene: repair_lyudmila_exclusive
```

```yaml
# content/agents/channels.yaml
- id: town_chat
  kind: channel
  spread:
    - {match: any, to: all_town_agents, tick_delay: 1}
    - {match: any, to: [dan], tick_delay: 1}        # via informant or lurking (§10)

- id: search_party_chat
  kind: channel
  spread:
    - {match: any, to: all_town_agents, tick_delay: 1}
    - {match: any, to: [dan], tick_delay: 0}        # he reads it directly, same tick (§10)
```

**Notes.**

- **Spread rules are a closed vocabulary:** `{match: {tags | valence | subject | any}, to: [agents | all_town_agents], tick_delay, when: <condition>}`. Irregularities (Lyudmila's 3-tick burst, Rufat's pharmacy-only feud channel, Oleg's conditional relay) are expressed as data on the irregular agent, so the harness simulates them and phase 4 can check every edge references a real agent.
- **Stance priors resolved to concrete subjects.** §10.1's priors are conditional-sounding ("anyone asking about Mira") but the subject set is closed at three, so they compile to constants: `aziza: {player: -1}`, `lyudmila: {dan: +1}`, `mom: {dan: +1}`. Deterministic, honest to the doc, and the first-impression rule applies wherever no prior exists.
- **Discretion is not a stat.** It was never anything but the spread rules; it stays that way (Aziza's entry has an empty `spread:` list — "spreads nothing, ever" is now literally checkable).

---

## 2.6 Scenes (new schema)

Blocks answer *what can be said*; scenes answer *when does this interaction exist* (B's framing, adopted). All 15 set-pieces of §8 become scene entries — "core vs reactive" is just whether the triggers are conditional.

```yaml
# content/scenes/act3.yaml
- id: diary_confrontation
  once: true
  cost: none                        # happens TO the player; no slot spent
  variants:                         # checked in order; first match fires (deterministic)
    - id: suspicious
      trigger:
        all:
          - {time: {after_act: 2}}
          - {suspicion: {tier_at_least: auditing}}
    - id: impatient
      trigger:
        all:
          - {time: {act: 3, day_in_act_at_least: 2}}
  on_enter:
    - engine_op: journal_transfer   # engine primitive: union of in-journal-true tokens
      to: dan                       # + decoy tokens → Dan's holdings AND affirmed claims (2.3)
  branches:
    - id: play_dumb
    - id: mask_off
      requires: {knowledge: {agent: player, has: [dan_is_artyom]}}
    - id: counter_bluff
      requires: {journal: {decoy_count_at_least: 2}}
    - id: de_escalate
  aftermath: scene_mom_diary_branch # blow up (trust event) vs explain
                                    # (mints dan_searched_room, flags early_warning_asset)
```

Scenes own: typed triggers with ordered variant selection, slot cost (`slot | none`), `once` semantics, engine-op hooks (`journal_transfer` is an engine primitive, not content), branch gating, and aftermath references. **Timed effects are normalized here too** (B, adopted): the envelope's delayed channel and Timur's +2-day archive path are `delay: {channel, days}` events, not per-scene special cases.

---

## 2.7 The two closed vocabularies

**Condition types** (B's structure, adopted: typed objects, each with its own schema — the *type list* is closed; extending the language means adding a type plus validator support, never an expression):

```yaml
requires:
  knowledge: {agent, has: [], lacks: []}
  trust:     {npc, at_least | at_most}
  stance:    {agent, subject, at_least | at_most}
  suspicion: {tier_at_least | tier_at_most}     # tiers only — never numbers
  standing:  {at_least}
  time:      {act | after_act | day | day_in_act_at_least}
  flag:      {id}
  journal:   {decoy_count_at_least | true_critical_count_at_least}
```

**Event types:**

```yaml
events:
  tell:            {agent, token, between_us: bool}   # THE core verb — any agent, not just Dan;
                                                      # between_us applies §13's trust spend + suppression
  claim:           {agent, token, verdict: denied}    # ledger write, no transfer
  mints:           {token}
  promise:         {npc, id}
  resolve_promise: {id, kept: bool}
  defend:          {npc, public: bool}
  refuse_ask:      {}                                 # Dan only
  report:          {tokens: []}                       # engine judges the match against Dan's
                                                      # holdings → matching_report if consistent
  delay:           {channel, days}
  flag:            {id}
```

Journal actions (record truth / record decoy / omit) are player UI verbs on connect prompts, not content events — content never writes the journal.

---

## 2.8 Resolved between the audits

Where B pushed back or refined, the resolution:

1. **Condition types over flat key enumeration** — B's version adopted. A's rationale (static checkability) is fully preserved: the type list is closed and each type's schema is fixed.
2. **Echo override path** — B's addition adopted; `echo:` on the token, `default` otherwise.
3. **`hash(token_id, hop_count)` over `hop mod n`** — B's variant adopted. Both are deterministic; the hash avoids all tokens cycling variants in lockstep.
4. **`editor.expected_act` namespace** — B's suggestion adopted and upgraded: the hint is validated against the computed earliest-mint act, so a stale hint becomes a warning instead of a lie.
5. **Timed effects normalized** — B's preference adopted: `delay` is a first-class event type, not scene-specific plumbing.
6. **Implementation priority** — B's order kept:
   1. Claims ledger + semantic events + rules table (largest architectural fix; kills the network-contradiction failure and the honest-denial bug)
   2. Agent schema (makes the gossip network fully simulable)
   3. Truth flag (correct reasoning about evidence, crying-wolf, endings)
   4. Typed mint sources (completes the reachability proof)
   5. Closed condition vocabulary (strengthens static validation)
   6. Scene schema (separates scheduling from dialogue)
   7. Field-level items: valence pairs, evidence refs, echo/defense templates, deterministic wording, `editor.expected_act`

---

## 2.9 New phase-4 checks this revision creates

Feeding the next section's catalog — each consolidation decision above adds obligations:

- No `truth: false` token satisfies any evidence gate, the Closed-Ranks journal requirement, or a stance firsthand exception.
- Every `decoy` field sits on a token with a `deduction` mint source.
- Every `critical` token has an `evidence` card, and the card exists.
- Critical-token reachability is proven over the **union** of token-side `minted_by` and block-side `mints` (all six source types resolve).
- Every `valence.subject` ∈ {dan, player, mira}; every valenced token used by a stance rule is tagged.
- Every spread-rule edge references an existing agent; Aziza's spread list is empty; Lyudmila's suppression exception matches the rules-table base cost.
- Echo templates render for every leakable token's every wording (template lint); overrides supply all three tiers.
- A defense line exists for every (agent, subject) pair reachable in a `disbelieved` state.
- `editor.expected_act` matches the computed earliest act (warning, not error).
- Wording selection is total and reproducible: two harness runs of the same policy produce byte-identical transcripts.
- Contradiction events are only reachable from states where `held_at_claim` was true (now provable by construction — assert it anyway).
- The diary `journal_transfer` op writes both holdings and affirmed claims; a Decoy-Heavy policy that lets Dan later acquire a contradicted truth sees suspicion rise with no dialogue involved.
- `once` semantics: envelope suspicion credit, teaching-Mom flip, and diary scene each fire at most once across every policy run.
