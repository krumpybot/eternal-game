# Eternal Game — Mid-Design Assessment

> **Reviewer**: Senior Game Designer (assessment role)
> **Date**: 2026-03-27
> **Scope**: Full review of Eternal_Game_Base.md, ETERNAL_GAME.md, and all appendices (C, D, E, F, M)
> **Phase coverage**: Phases 1–4 COMPLETE. Phases 5–13 pending.
> **Perspective**: Onchain MMO viability, Cairo/Dojo/Starknet constraints, player experience, security, and design coherence.

---

## Executive Summary

The design is **ambitious and structurally sound**. The separation of immutable physics (locked) from tunable gameplay (Game Master-adjustable) is the right architecture for an eternal onchain game. The resource catalog, biome system, trait system, and action framework are well-designed with good extensibility hooks.

However, several issues need attention before deployment — ranging from **critical contract-level risks** that would require a full restart if wrong, to **balance concerns** that affect player experience, to **technical constraints** that may force design compromises during Cairo implementation.

This assessment is organized by severity:
1. 🔴 **Critical** — Must be resolved before deployment. Getting these wrong requires restart.
2. 🟡 **Important** — Should be resolved during remaining quantification phases. Affects game quality.
3. 🔵 **Advisory** — Design improvement suggestions. Not blocking.

---

## 🔴 CRITICAL — Hard Definition Requirements

These are areas where the locked physics must be exactly right at deployment. Errors here cannot be corrected without a full contract restart.

### C1. Resource ID Registry Misalignment

**Status**: Base Module §13 and Appendix F define **different** resource ID registries.

| Range | Base Module §13 | Appendix F |
|---|---|---|
| `0x0000–0x00FF` | Food | Food ✓ |
| `0x0100–0x01FF` | Core Resources (T1/T2/T3) | Core (22 assigned at 0x0100–0x0115), rest locked |
| `0x0200–0x02FF` | Special Resources | Raw Materials (Mining) |
| `0x0300–0x03FF` | Essence & magical | Raw Materials (Farm/Forage/Animal) |
| `0x0400–0x0FFF` | Reserved (future) | Refined, Beast Parts, Alchemy, Reserved |

Appendix F's registry is more mature and detailed (designed in Phase 3). The Base Module's registry is an earlier draft that was never updated.

**Recommendation**: Update Base Module §13 to match Appendix F's registry exactly. This is the **canonical source**. Add a note that Appendix F is the authoritative reference. **Add to Phase 3c or create a dedicated reconciliation task.**

### C2. Attribute Formula Edge Cases — Unbounded Cost Reduction

The WIS and CHA formulas have no explicit floor:
- **WIS**: `cost = base × (1 − WIS × 0.04)` → At WIS 20: cost = 0.2× base. Fine.
- **CHA**: `cost = base × (1 − CHA × 0.03)` → At CHA 20: cost = 0.4× base. Fine.

With attributes capped at 20 and traits only suppressing (not boosting above cap), the minimum effective cost is bounded. However:
- **Trait modifiers can stack**: A +1 WIS personality trait + natural WIS 20 = suppressed to 20 (no benefit). But what about **future modules** that might add WIS-boosting items or buffs?
- The formula produces **negative values** if effective WIS ever exceeds 25 (e.g., WIS 20 + equipment bonus of +5 from a future module).

**Recommendation**: Add an explicit floor clamp to both formulas: `max(0.1, 1 − WIS × 0.04)` and `max(0.2, 1 − CHA × 0.03)`. This is a **locked formula** — must be correct at deployment. Alternatively, hard-cap effective attribute values at 20 in the contract regardless of future module buffs. **Flag for contracts specialist.**

### C3. Understaffing Division by Zero

`upkeep_multiplier = clamp(1 / staffing_ratio, 1, 3)` where `staffing_ratio = clamp(CurrentPop / RequiredPop, 0, 1)`.

If `CurrentPop = 0` and `RequiredPop > 0`: `staffing_ratio = 0`, `1 / 0` = division by zero.

**Recommendation**: Define behavior at zero population: either `staffing_ratio = 0` → upkeep multiplier = 3× (hard cap), or building is automatically deactivated. This must be explicitly handled in the contract. **Add to Phase 10 quantification.**

### C4. Commit-Reveal Seed Security

Both adventurer creation and area discovery use commit-reveal:
- `adventurer_seed = hash(global_seed, token_id, mint_block, user_salt)`
- `discovery_seed = hash(global_seed, x, y, z, discoverer_salt)`

**Risk**: If `global_seed` is predictable (e.g., derived from a known genesis block), and `token_id` and `mint_block` are known at commit time, the user's `salt` is the only entropy — but the user **chose** that salt. This means users can brute-force salts to get optimal trait rolls or area discoveries.

**Recommendation**: The commit-reveal must include **block-hash entropy** from the **reveal block** (not the commit block), ensuring the user cannot predict the outcome when choosing their salt. This is a standard pattern but must be explicitly documented and validated. **Flag for contracts specialist. Add as Phase 13 security task.**

### C5. Mutation Crafting Prediction

The mutation function uses XOR-seeding: `new_properties = f(input_properties, craft_seed)`. If `craft_seed` is deterministic from known inputs, players can predict outcomes and only craft when they know the result will be favorable (MEV-style).

**Recommendation**: `craft_seed` should include the **reveal-block hash** (same commit-reveal pattern as adventurer creation). Document this requirement explicitly. If craft seed is purely deterministic (no block entropy), the crafting system becomes a solved game. **Critical for item economy integrity. Flag for contracts specialist.**

### C6. Energy Regen During Time-Lock

The spec says: "Regen occurs only when **not** in a time-locked action (idle = resting)." This means an adventurer performing a 60-minute Extreme action regenerates **zero energy** during that time. But then §10 says the reservation model reserves energy upfront.

**Ambiguity**: Does energy regen during the time-lock or not? If not, the effective energy cost of time-locked actions is much higher than the listed cost (you lose both the energy AND the regen you would have earned). A 45-energy Heavy action with a 36-minute time-lock also costs ~60 energy in lost regen.

**Recommendation**: This is a fundamental economic parameter that affects every action cost calculation. Must be explicitly decided and documented. Both approaches are valid but have dramatically different balance implications. **If regen pauses during locks, all action costs in the quantification plan need recalculation. Add as Phase 5 prerequisite.**

### C7. Permadeath + Settlement Deed Orphaning

When an adventurer dies:
- All Deeds are destroyed.
- Settlement persists but can't expand.
- Hex ownership persists but decays.

**Problem**: A settlement with 7 hexes, significant building investment, and active population becomes a **zombie structure**. No mechanism exists to:
- Transfer ownership to another adventurer.
- Issue new deeds.
- Prevent the settlement from slowly rotting.

This is **by design** (permadeath consequences), but without any recovery mechanism, large settlements become enormous sunk costs with zero recovery path. This may severely discourage investment in settlements.

**Recommendation**: Consider a **succession mechanic** — either in the base module (e.g., "a new adventurer who controls the Keep hex can claim the settlement") or as a declared future module hook. At minimum, document the intended player experience: "your settlement will decay and be claimed by others — this is the cycle of the Eternal World." **Add to Phase 10 or 13 as explicit design decision.**

---

## 🟡 IMPORTANT — Design Issues Affecting Game Quality

### I1. Population Growth Rate Is Extremely Slow

Current: +1 or −1 per in-game day boundary. At 6 in-game days per real day, a settlement with max pop 20 takes **3.3 real days** to fully populate from zero.

Combined with:
- Building staffing requirements (unknown but presumably 2–5 per building).
- 12 building types, each needing staff.
- Population capped by housing.

**Concern**: The early game will feel like waiting for population to trickle in. A newly settled hex with a Smelter, Workshop, and Housing may wait 5+ real days before it's fully operational. This is anti-fun.

**Recommendation**: Consider batch growth (e.g., `+floor(food_reserves_pct / 25)` per day — up to +4/day at full reserves) or event-driven immigration. The growth rate should be a **GM-adjustable parameter**, not locked. **Add growth rate formula to Phase 10 quantification as unlocked.**

### I2. No Fishing Production Section

Fishing is listed in the action catalog (23 actions) and referenced in biome affinity (3 signature fishing biomes: Coast, Lake, Mangrove), but the Base Module has no dedicated production section for fishing (§14–§18 cover Mining, Forestry, Hunting, Foraging, Livestock — no Fishing).

**Impact**: Fishing mechanics (where it happens, what it produces, biome interaction, skill traits) are undefined. Fish is a food resource but has no production pipeline.

**Recommendation**: Add §18.5 or renumber to include a Fishing production section. Fishing should follow the same pattern as Hunting (action in appropriate area, chance-based, SUR-influenced). **Add to Phase 6f in quantification plan (already exists but needs Base Module section).**

### I3. Ecosystem Dynamics May Be Too Punishing

The logging/hunting/foraging ecosystem creates a zero-sum tension:
- Heavy logging eliminates fauna → no hunting.
- Heavy harvesting exhausts fertility → no farming.
- Livestock requires cleared land → no native species.

**Concern**: In practice, this means every fertile/forestry area must be **permanently specialized** early. There's no mechanism for diversified use. A player who logs and hunts simultaneously will quickly destroy the ecosystem for both. The "conservation choice" is really "pick one and commit."

**Recommendation**: Consider a **sustainable yield threshold** — below X% extraction rate, ecosystems maintain themselves indefinitely. Only exceeding the threshold triggers depletion. This creates a meaningful choice between "sustainable low yield" and "extractive high yield." **Add to Phase 6 as a design parameter (unlocked, GM-adjustable).**

### I4. No Direct Player-to-Player Resource Transfer

The Base Module states: "base supports direct adventurer-to-adventurer transfer only" (§3, deferred features). But the resource transfer action isn't in the action catalog (23 actions don't include "Give" or "Transfer").

**Impact**: Two adventurers standing on the same hex cannot exchange resources. The only economy is self-sufficient. This severely limits multiplayer cooperation in the base module.

**Recommendation**: Add a "Transfer" or "Exchange" action (Trivial cost, Social category). Two adventurers on the same hex can move resources between inventories. This is fundamental for any multiplayer game. **Add to Phase 2 action catalog and Phase 5 quantification.**

### I5. 87 Resources × 27 Biomes Storage Concern

The biome-resource affinity matrix is 87 × 27 = 2,349 cells. If stored naively onchain, each cell is a storage slot.

**Reality check**: This matrix is only needed at world generation time (node seeding). It could be:
- Hardcoded as contract constants (most gas-efficient).
- Stored as packed bitfields (3 tiers fit in 2 bits per cell → ~587 bytes total).
- Computed deterministically from a smaller rule set.

**Recommendation**: Do not store this as a dynamic mapping. Hardcode as constants or use packed storage. The matrix itself is 🔒 locked, so there's no need for it to be mutable storage. **Flag for contracts specialist in Phase 4 review.**

### I6. Autoregulator PI Controller Complexity

A PI (Proportional-Integral) controller evaluating multiple aggregate metrics per epoch is computationally expensive onchain. Each epoch evaluation requires:
- Reading aggregate state across all adventurers, settlements, resources.
- Computing error terms and integral accumulators.
- Writing updated parameter values.

**Concern**: Aggregate computation (total population, resource production rates, death rates) may require iterating over all entities — which is infeasible onchain.

**Recommendation**: Use **lazy aggregation** with running counters. Each relevant action updates a global counter (e.g., `total_resources_produced += quantity`). The autoregulator reads these counters, not individual entity state. Counter-based aggregation is O(1) per epoch. **Add as explicit implementation constraint in Phase 13b.**

### ~~I7. Lake Missing from Materials Sub-Type Table~~

~~Verified: Lake is present at 30% Fertile / 20% Mining / 50% Forestry. All 25 traversable biomes accounted for. No action needed.~~

### I8. Campfire Lifetime Undefined

Campfire is a "zero-slot building" with "no condition/upkeep (it's temporary)" but no lifetime is specified. Does it last forever? One in-game day? Until the adventurer leaves?

**Recommendation**: Define campfire lifetime explicitly (suggestion: decays after 1 in-game day / 1,440 ticks, or when the adventurer who placed it leaves the hex). **Add to Phase 9 quantification.**

---

## 🔵 ADVISORY — Design Improvement Suggestions

### A1. Consider Simplifying Biome Count

27 biomes is comprehensive but creates a large design surface. Each biome needs:
- Movement/survey/decay modifiers (locked).
- Hazard/encounter distributions (unlocked).
- Resource affinity row (87 cells).
- Fauna tier.
- Fertility rating.
- Materials sub-type weights.

**Observation**: Several biomes have very similar gameplay fingerprints:
- **Grassland vs Plains**: Both are temperate, high fertility, low hazard, herding-friendly. Grassland is "slightly rougher Plains."
- **Steppe vs Scrubland**: Both are dry, low fertility, moderate hazard. Different resource signatures but similar player experience.
- **Marsh vs Swamp**: Similar wet biomes with different tree cover. Marsh is "friendlier Swamp."

**Counterpoint**: Each biome does have unique resource signatures and thematic identity. 27 is manageable for a game of this scope. But be aware that each biome adds to the testing/balancing surface.

**Recommendation**: Keep 27 but acknowledge in the quantification plan that biome differentiation must be validated during Phase 13 balance review. If playtesting reveals that players can't distinguish similar biomes, consider merging.

### A2. Trait System May Produce Samey Adventurers

With 2 personality + 1 physical trait at mint, and 10 attributes from a 20-point budget:
- Personality pairs: 19 groups → 38 possible. Two traits from 19 groups = `C(19,2) = 171` unique personality combinations.
- Physical: 23 options → 23 possible.
- Total mint trait variety: 171 × 23 = **3,933 unique trait sets**.
- Attribute distribution: 20 points across 10 attributes = much higher combinatorial space.

**Observation**: 3,933 unique trait sets is good variety. But in practice, certain trait combinations will be dramatically superior for common strategies (e.g., Brave + Diligent + Brawny for a mining/hunting build). Min-maxing will narrow effective diversity.

**Recommendation**: This is well-handled by the chance-based system (traits are assigned by seed, not chosen). Players cannot min-max at mint — they work with what they get. The design is sound here. No change needed, but document the expected meta-diversity in Phase 13d.

### A3. Permission Hooks Need Gas Limits

The `IPermissionHook` trait allows territory controllers to deploy arbitrary smart contracts that gate access. A malicious controller could deploy a hook that:
- Consumes excessive gas (griefing attackers trying to claim).
- Reverts conditionally (allowing friends, blocking enemies, but impossible to prove).
- Changes behavior over time.

**Recommendation**: The base module should enforce a **gas limit** on permission hook calls. If the hook exceeds the limit, it defaults to "allowed" (fail-open, preventing griefing). Document this constraint. **Add to Phase 11 or 13.**

### A4. Beast Encounter Resolution Needs Careful Balancing

The single-roll resolution system is the right approach for an onchain game (no multi-step combat), but the outcome space is wide:
- Avoid, Injury (13 types), Resource gain (variable), Trait gain, Death.
- Inputs: beast lethality, health, energy, SUR, DEX, equipment, time-of-day.

**Concern**: With so many inputs, the resolution formula could become either trivially solvable (players always know their odds) or opaque (players can't reason about risk). Neither is fun.

**Recommendation**: Publish the resolution formula transparently (onchain games can't hide it anyway), but ensure the beast type drawn from the hazard table introduces enough variance that players can **estimate** risk but never **eliminate** it. The random beast draw is the key uncertainty — ensure T3+ beasts can appear even in "safe" biomes at low probability. **Add encounter formula design to Phase 12 with explicit variance requirements.**

### A5. Settlement Benefits May Be Too Weak

Current settlement benefits:
- 25% fewer donkeys for inter-hex transport.
- Shared population across hexes.
- Cheaper travel for the owner.

**Concern**: These benefits are mild. The **costs** of settlement are enormous: multiple developed hexes, Keep buildings, ongoing food deposits, population management, decay upkeep across all hexes. The reward doesn't clearly justify the investment.

**Recommendation**: Consider stronger settlement benefits that create genuine strategic advantage:
- **Shared storage** across settlement hexes (resources deposited in one hex accessible from another).
- **Building synergies** (e.g., Smelter + Forge in the same settlement = +10% crafting quality).
- **Defensive bonus** (reduced hazard chance within settlement borders).
- **Trade hub** capability (future module hook but declare the interface now).
These can be GM-adjustable. **Add to Phase 10 as candidate benefits for quantification.**

### A6. No Weather or Seasonal System

The world has day/night (implied by "time of day" affecting encounters) but no seasons or weather. For an "eternal" world, the absence of temporal variety may make the game feel static.

**Recommendation**: Reserve a future module hook for seasons/weather in §29 (Future Module Placeholders). At minimum, the base module could include a **season counter** (4 seasons per in-game year, purely cosmetic in base module, but available for future modules to modify yield rates, encounter frequencies, etc.). Low cost to implement, high extensibility value. **Add to Phase 5 or §29.**

### A7. Consolidate Redundant Information Between ETERNAL_GAME.md and Eternal_Game_Base.md

The two documents have significant overlap:
- Both describe the resource system, energy system, adventurer attributes, etc.
- ETERNAL_GAME.md is the design scope (vision document).
- Eternal_Game_Base.md is the base module spec (implementation document).

**Concern**: As quantification proceeds, these documents may drift apart. The Base Module becomes increasingly authoritative while the Scope document becomes stale.

**Recommendation**: After Phase 4, ETERNAL_GAME.md should be **frozen** as a vision document. All quantified values and implementation details should live exclusively in the Base Module and appendices. Add a note to ETERNAL_GAME.md header: "For quantified values and implementation details, refer to Eternal_Game_Base.md and appendices."

### A8. Consider Explicit "Idle Regen" Action

Currently, energy regen occurs passively when not time-locked. But there's no explicit "do nothing and recover" action. The "Resting" action exists (Trivial tier, 0–5 energy) but its purpose is unclear — if it costs energy to rest, and resting is what you do when idle, why would you pay energy to rest?

**Recommendation**: Clarify Resting's purpose. Options:
- **Resting is free** (0 energy, short time-lock) — explicitly enters "recovery mode" with boosted regen.
- **Resting costs energy** — for a meaningful health regen boost (not energy regen).
- **Resting doesn't exist** — idle adventurers just passively regen.

The current spec says Resting is "standalone: active recovery action, costs time not energy." If it costs time but not energy, it's a **health recovery** action (time-lock during which health regens faster). Document this clearly. **Clarify in Phase 5.**

---

## Technical Constraints (Cairo/Dojo/Starknet)

### T1. Storage Model

Each adventurer stores: position (3 felts), energy, max_energy, health, max_health, activity_lock, 10 attributes, 10 trait slots, seed, inventory (map), equipment (7 slots), alive flag. This is **~40 storage slots per adventurer** minimum.

Each hex stores: biome, discovered, discoverer, controller, area_count, decay_level, last_upkeep_tick, settlement_id. Plus per-area data (type, surveyed, slots, resource nodes). A fully surveyed 9-area hex with 3 nodes per area = **~50+ storage slots per hex**.

**Assessment**: This is manageable for a Dojo world on a Starknet L3 appchain. Storage is the primary cost driver. The lazy evaluation pattern (compute on access, don't store intermediate state) is correct.

**Recommendations**:
- Pack attributes into a single felt252 (10 × 5 bits = 50 bits, fits easily).
- Pack trait slots similarly (10 × 16 bits = 160 bits, fits in one felt252).
- Use bitmap encoding for area types and survey status.
- **Do not store the biome-resource affinity matrix.** Hardcode as constants.

### T2. Randomness

Cairo has no native randomness. All "random" outcomes use deterministic seeds:
- World gen: `noise_function(global_seed, x, y, z)`.
- Adventurer mint: `hash(global_seed, token_id, mint_block, user_salt)`.
- Action outcomes: need per-action seeds.

**Assessment**: The commit-reveal pattern is correct for user-initiated randomness. But **action outcomes** (encounter chance, yield variance, trait gain rolls) happen during action resolution — there's no commit-reveal opportunity here.

**Options**:
- Use `hash(adventurer_seed, action_count, block_hash)` — includes block hash for unpredictability.
- Accept that miners/sequencers can influence outcomes (low-value manipulation on an appchain).
- Use a VRF oracle if available on the appchain.

**Recommendation**: Document the randomness source for action outcomes explicitly. Block hash is the pragmatic choice for an appchain. **Add as Phase 13 security task.**

### T3. Simplex Noise Onchain

The biome determination uses 5-layer simplex noise. Simplex noise involves:
- Gradient table lookups.
- Floating-point interpolation.
- Multiple octaves.

**Assessment**: This is **expensive** in Cairo but only needs to run **once per hex** (on first exploration). After that, the biome is stored. If the noise function costs 100K gas, that's acceptable as a one-time cost amortized over the hex's lifetime.

**Recommendation**: Pre-compute and store biome on exploration, not on every access. Use fixed-point arithmetic (u128 with implicit decimal). Consider a lookup-table approximation rather than true simplex noise — the player can't tell the difference. **Flag for contracts specialist (already flagged ⚠️).**

### T4. Lazy Evaluation Correctness

Energy and health use lazy evaluation: `current = min(max, stored + rate × (current_tick - last_tick))`. This is elegant but requires care:

- **Overflow**: If `tick_delta` is very large (adventurer untouched for months), `rate × delta` could overflow.
- **Negative regen**: If food runs out, regen turns negative. `stored + negative × delta` must not underflow (use saturating subtraction).
- **Death check**: If lazy-evaluated energy would be 0 or below, the adventurer is dead — but nobody called the contract. Need a "poke" mechanism or accept that death is only realized on next interaction.

**Recommendation**: Use saturating arithmetic. Document that death from starvation is realized lazily (adventurer is "dead" but the state isn't updated until someone interacts with them). This is fine — it just means abandoned adventurers are Schrödinger's dead until touched. **Add explicit overflow/underflow handling to Phase 13.**

---

## Quantification Plan Additions

Based on this review, the following items should be added or modified in the quantification plan:

### New Tasks

| Task | Phase | Priority | Description |
|---|---|---|---|
| **Resource ID registry reconciliation** | 3c or new 3d | 🔴 Critical | Align Base Module §13 registry with Appendix F. Single canonical source. |
| **Attribute formula floor clamps** | 1b addendum | 🔴 Critical | Add explicit floor to WIS and CHA cost reduction formulas. Or hard-cap effective attributes at 20. |
| **Commit-reveal security spec** | New 13f | 🔴 Critical | Document seed derivation for adventurer mint, area discovery, and crafting. Ensure block-hash entropy in reveal. |
| **Action outcome randomness source** | New 13g | 🔴 Critical | Define seed for per-action random outcomes (encounters, yields, trait rolls). |
| **Zero-population edge case** | 10f | 🔴 Critical | Handle division by zero in understaffing formula. |
| **Energy regen during time-lock** | 5 prereq | 🟡 Important | Decide and document: does energy regen during activity locks? Cascading balance impact. |
| **Fishing production section** | 6f | 🟡 Important | Add to Base Module. Define area type, biome interaction, yields. |
| **Player-to-player transfer action** | 2a addendum | 🟡 Important | Add Transfer/Exchange action to action catalog. |
| **Settlement succession/recovery** | 10g or 13 | 🟡 Important | Document what happens to orphaned settlements. |
| **Campfire lifetime** | 9b | 🟡 Important | Define decay/lifetime for zero-slot buildings. |
| **Sustainable yield threshold** | 6 (all sections) | 🟡 Important | Define extraction rate below which ecosystems self-sustain. |
| **Permission hook gas limits** | 11 or 28 | 🔵 Advisory | Gas-bound permission hooks to prevent griefing. |
| **Season counter** | 5 or 29 | 🔵 Advisory | Reserve seasonal cycle hook for future modules. |
| **Settlement benefit expansion** | 10 | 🔵 Advisory | Quantify stronger settlement incentives. |
| **Autoregulator lazy aggregation** | 13b | 🟡 Important | Running counter pattern, not entity iteration. |
| ~~Lake materials sub-type entry~~ | ~~4 fix~~ | ~~Verified~~ | ~~Already present. No action needed.~~ |

### Modified Tasks

| Existing Task | Modification |
|---|---|
| Phase 5 (Movement) | Add prerequisite: resolve C6 (energy regen during time-lock). |
| Phase 6 (Production) | Add sustainable yield threshold parameter (I3). Add explicit Fishing section to Base Module. |
| Phase 10 (Settlement) | Add succession mechanic decision (C7). Add population growth rate as unlocked parameter (I1). |
| Phase 12 (Encounters) | Add explicit variance requirement (A4). Document resolution formula transparency. |
| Phase 13b (Autoregulator) | Add lazy aggregation constraint (I6). |
| Phase 13e (GM Authority Map) | Cross-reference with this assessment's locked/unlocked classifications. |
| Phase 13 (new tasks) | Add 13f (seed security), 13g (action randomness). |

---

## Design Strengths

To be clear about what's working well:

1. **Immutable/mutable separation** is the right architecture. The Game Masters system elegantly solves the "who balances an eternal game" problem.

2. **Chance-based progression** (no XP tracking) is brilliant for onchain. Zero historical state to maintain, pure per-action rolls. This alone saves enormous storage.

3. **Lazy evaluation everywhere** — energy, health, decay — is the correct pattern for blockchain games. Only compute on interaction.

4. **Permission hooks** as the extensibility mechanism is powerful. Territory controllers can implement arbitrary economic policies without base module changes.

5. **Permadeath with legacy** creates meaningful stakes without losing world progress. Buildings persist, discoverer tags persist, the world remembers.

6. **27 biomes with unique resource signatures** create genuine exploration motivation. Players have economic reasons to seek specific terrain.

7. **The trait system** (120 traits, 5 types, exclusive pairs, chance-based acquisition) is deep without being complex to implement onchain.

8. **Ecosystem dynamics** (logging vs hunting tension) create emergent player-driven scarcity — the most interesting economic primitive in the design.

---

## Summary of Remaining Quantification Philosophy

Given that Phases 1–4 have locked the core physics, the remaining phases (5–13) should follow this principle:

> **Define hard bounds and default values. Leave everything else to the Game Masters.**

Specifically:
- **Phase 5 (Movement)**: Lock the formula structure (what inputs affect cost). Default values are GM-adjustable.
- **Phase 6 (Production)**: Lock the production pipeline structure (what actions produce what). Yield rates are GM-adjustable.
- **Phase 7 (Crafting)**: Lock the crafting system mechanics (mutation function, facility requirements). Recipes are GM-manageable.
- **Phase 8 (Items)**: Lock the item model (properties, durability, greatness). Stat values are GM-adjustable.
- **Phase 9 (Buildings)**: Lock the building system (slots, placement rules). Building stats are GM-adjustable.
- **Phase 10–11 (Settlement/Territory)**: Lock the deed system and decay mechanics. Thresholds are GM-adjustable.
- **Phase 12 (Encounters)**: Lock the resolution formula structure. Encounter tables are GM-manageable.
- **Phase 13 (Autoregulator)**: Lock the PI controller mechanics and parameter bounds.

The quantification plan should explicitly mark each value as **locked** (formula structure, ID registries, system mechanics) or **default** (numeric values that Game Masters can adjust within bounds).

---

## Contradictions and Inconsistencies Found

| Issue | Location | Description |
|---|---|---|
| Resource ID registry mismatch | Base §13 vs Appendix F | Different range assignments (see C1) |
| ~~Lake missing from materials table~~ | ~~Base §7~~ | ~~Verified: Lake present. 25 biomes correct.~~ |
| Fishing has no production section | Base §14–§18 | Listed in action catalog but no Base Module section |
| Explore "biome-modified" claim | Base §11 (energy table) | Already fixed to "flat" but verify §11 prose |
| ETERNAL_GAME §9 subsection refs | ETERNAL_GAME.md | §9.x refs may be stale after renumbering |
| Resting action purpose | Appendix M vs Base §10 | Described as "costs time not energy" but listed in Trivial tier (0–5 energy) |

---

*Assessment complete. Filed as `mid-design_assessment.md` in the project repository.*
*Reviewer: Squire (senior game designer assessment role)*
*Date: 2026-03-27*
