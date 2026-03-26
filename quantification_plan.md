# Eternal Game Base Module — Quantification Plan

> Each phase builds on the previous. We don't move to the next phase until the current one is locked. Large tables become appendices in the Base Module doc.

---

## PHASE 1 — Units, Constants & Attribute Mechanics ✅ COMPLETE

The atomic layer. Everything else references these.
Dependencies: none — these are leaf primitives.

### 1a. Units & Constants ✅

| Constant | Value | Notes |
|---|---|---|
| Tick length | 10 seconds | Locked (§4 of base doc) |
| Ticks per in-game day | 1,440 | 4 real hours × 360 ticks/hour |
| In-game days per real day | 6 | Locked |
| Base energy regen | 0.1 / tick | ~36/hour, ~864/in-game day |
| Base max energy | 100 | ~2.8 hours of storage at base regen |
| Base health regen | 0.01 / tick | ~3.6/hour, ~86.4/in-game day |
| Base max health | 100 | Much slower recovery than energy |
| Weight unit | kg | Consistent with Eternum and Blitz |
| Base carry capacity | 50 kg | Before STR/gear bonuses |
| Distance unit | 1 hex = 1 movement action | Adjacent-only, locked |
| Stack size (resources) | Unlimited | Fungible tokens, weight-limited |
| Stack size (crafted items) | 1 per slot | Non-fungible due to mutation crafting; inventory may be slot-limited per blockchain constraints |

### 1b. Attribute Formulas ✅

10 attributes, 20-point budget at mint. Attributes serve as **performance gates** for encounters and events. Progression is **chance-based** (no XP tracking).

| Attribute | Primary Effect | Formula |
|---|---|---|
| Strength (STR) | Carry capacity | `carry_cap = 50 + STR × 5 kg` |
| Endurance (END) | Max energy, energy regen | `max_energy = 100 + END × 10`; regen rate `+2%/pt` |
| Dexterity (DEX) | Hazard avoidance | `avoid_bonus = DEX × 5%` |
| Vitality (VIT) | Max health | `max_health = 100 + VIT × 10` |
| Intelligence (INT) | Resource yield | `yield = base × (1 + INT × 0.025)` |
| Wisdom (WIS) | Explore & survey energy efficiency | `cost = base × (1 − WIS × 0.04)` — floor at 0 |
| Charisma (CHA) | Upkeep energy efficiency | `cost = base × (1 − CHA × 0.03)` — floor at 0; fee discount `CHA × 5%`; +days until follower desertion |
| Survival (SUR) | Hunting, foraging, beasts | `hunt += SUR × 5%`; `forage += SUR × 5%`; `beast += SUR × 2.5%` |
| Craftsmanship (CRA) | Craft quality, repair, mutation | `quality += CRA × 5%`; repair `−4%/pt`; widens mutation range |
| Leadership (LEA) | Follower count | `max_followers = 1 + floor(LEA / 2)` |

### 1c. Trait List ✅ (Final review applied 2026-03-23)

5 trait types: **personality**, **physical**, **skill**, **injury**, **disability**. 120 total base traits.

Full catalog: `Appendix_C_Trait_Catalog.md`

> ⚠️ **Pending review**: Trait special effects reference game actions, resources, buildings, and systems not yet fully quantified. Trait percentage values to be revisited for balance in Phase 13d once all systems are locked.

### 1d. Follower Scaling ✅

Followers are **passive buffs** — no commanding action required.

| Type | Effect | Source | Notes |
|---|---|---|---|
| Laborer | Improves production yield/throughput | Recruited from settlements | Human follower |
| Donkey | +50 kg carry capacity | Herded then bound via Recruiting | Animal follower (pack animal) |
| Mount (Horse) | −20% travel energy/time, +20 kg carry | Herded then bound via Recruiting | Animal follower; max 1 mount per adventurer |

Base followers: 1. Max: `1 + floor(LEA / 2)`.
Tamer skill trait grants +1 max **animal** follower specifically.

---

## PHASE 2 — Core Actions & World Physics ✅ COMPLETE (draft values)

**The action layer.** 23 player-initiated actions across 7 categories + 4 encounter types (potential outcomes of actions, not player-initiated).

Dependencies: Phase 1 (attribute formulas, energy/health budgets).

Full catalog: `Appendix_M_Action_Catalog.md`

### 2a. Action Catalog ✅

| Category | Actions | Count |
|---|---|---|
| Movement & Exploration | Travel, Explore, Survey, Delve | 4 |
| Resource Gathering | Mining, Logging, Farming, Foraging, Hunting, Fishing, Herding | 7 |
| Crafting & Refinement | Refining, Crafting, Cooking, Repairing, Salvaging | 5 |
| Construction & Settlement | Building, Demolishing, Upkeep, Stocking | 4 |
| Followers | Recruiting | 1 |
| Social & Economic | Trading | 1 |
| Recovery | Resting | 1 |
| **Total** | | **23** |

Encounter types (potential outcomes): Beast, Combat, Social, Special.

### 2b. Action Properties Template ✅

All 23 actions have draft property cards in `Appendix_M_Action_Catalog.md`. Cost tiers:

| Tier | Energy | Time-lock (real) | Examples |
|---|---|---|---|
| Trivial | 0–5 | 0–2 min | Resting |
| Light | 5–15 | 2–6 min | Cooking, Trading, Salvaging, Stocking |
| Medium | 15–30 | 6–18 min | Survey, Foraging, Fishing, Repairing, Recruiting |
| Heavy | 25–45 | 12–36 min | Travel, Farming, Refining, Mining, Logging, Hunting, Crafting, Herding |
| Extreme | 35–60 | 24–60 min | Explore, Building, Delve, Demolishing |

### 2c. Action–Trait Cross-Reference ✅ (to be revisited in Phase 13d)

Complete matrix in `Appendix_M_Action_Catalog.md`. To be validated during the final trait balance review.

### 2d. Time Budget Analysis ✅ (to be revisited in Phase 13c)

Draft worked example: ~9 actions/in-game day, ~250 energy, ~920 ticks at heavy cost tiers. To be validated when exact action costs are locked.

---

## PHASE 3 — Resources ✅ COMPLETE

Every item, recipe, building cost, and biome affinity references resource IDs and properties. Resources are intrinsic definitions — what exists in the game world — independent of where they appear.

Dependencies: Phase 1 (weight unit, attribute formulas for yield calculations). **No dependency on biomes** — resource properties (weight, rarity, nutrition) are defined before placement.

### 3a. Resource Catalog ✅

153 defined resources + 60 effective beast part variants. 6-tier rarity system (Common → Mythic).

| Layer | Count | Details |
|---|---|---|
| Raw resources | 87 | Mining (30), Logging (6), Farming (13), Foraging (14), Hunting (11), Fishing (1), Herding (12) |
| Core resources | 22 | Eternum heritage, immutable. Mapped to 6 rarity tiers. |
| Refined resources | 44 | Metals (1), Textiles (13), Wood/Plant (5), Building (7), Fuels/Chemicals (10), Processed Food (8) |
| Beast parts | 12 types × 5 quality tiers | 60 effective variants |

Key design decisions locked:
- Cold Iron is iron (Eternum heritage name for the everyday working metal)
- Alloys are emergent crafting properties, not standalone resources (Phase 7)
- 3 universal discovery resources: Rare Metals (mining), Worldroot (logging), Unicorn Hair (foraging)
- Endgame chain: Voidsand (heat) + Starmetal (equipment) → unlocks Mythic metalwork
- Resource ID registry: 4096 addressable IDs, core resources locked at 22

→ Output: **Appendix F: Resource Catalog**

### 3b. Food & Nutrition ⏳

Nutrition values for each food type. Raw vs cooked multiplier (cooking recipes defined in Phase 7c). How nutrition maps to food-reserve deposit amounts (deposit X food → Y% bar). Energy buff values for cooked food tiers.
→ Output: Food mechanics table

### 3c. Core Resource Tiers ⏳

T1/T2/T3 yield rates relative to each other. Refinement ratios (e.g., 3 copper ore → 1 copper bar). Quality tier split (dirty/clean/pure — what % at each skill level).
→ Output: Refinement ratios table

---

## PHASE 4 — Biomes & World Generation ✅ COMPLETE

The terrain layer. Defines the environment that all actions operate in and determines where resources appear.

Dependencies: Phase 1 (attribute formulas), Phase 2 (action catalog — biome modifiers reference specific actions), **Phase 3 (resource catalog — biome affinity and node seeding need to know what resources exist)**.

### 4a. Biome Properties ✅

27 biomes fully quantified across 6 categories (Temperate 7, Arid 5, Wet 5, Cold 3, Extreme 3, Water 4). Each biome has: unique narrative profile, movement cost/time modifiers, survey cost/time modifiers, decay rate modifiers, base hazard chances (2%–20%), encounter type distributions (beast/combat/social/special), fauna tier caps (T2–T5), fertility ratings (None–Medium–High). Biome clustering algorithm defined (5-layer simplex noise: Temperature, Moisture, Elevation, Variation, Anomaly). Anomaly layer governs Extreme biome placement. Distribution targets and Nexus region override specified.
→ Output: **Appendix D: Biome Table** (`Appendix_D_Biome_Table.md`)

### 4b. Biome–Resource Affinity ✅

Full affinity matrix for all 87 raw resources × 27 biomes. Three-tier system: ✓ (standard, 1×), ◆ (primary, 2×), ★ (signature, 3×). Covers mining (30 resources), logging (6), farming (13), foraging (14), hunting (11), fishing (1), herding (4 livestock types). Key signature assignments: Glassfields (gems, deep crystal), Blight (alchemical silver, spiritbloom, poisonous plants), Scorched (obsidian, sulfur, ignium, demonhide), Rainforest (moonwood, rare herbs), Scrubland (wild herbs, dye plants), Mangrove (seaweed, fishing). Universal discovery resources (Rare Metals, Worldroot, Unicorn Hair) confirmed biome-independent.
→ Output: **Appendix E: Biome–Resource Affinity Matrix** (`Appendix_E_Biome_Resource_Affinity.md`)

### 4c. Area Generation ✅

Area count rarity distribution: 35% (3) → 25% (4) → 18% (5) → 10% (6) → 6% (7) → 4% (8) → 2% (9). Area type weights: 45% Materials, 40% Bare, 10% Underworld, 5% Special (capped at 1 each per hex). Materials sub-type weights defined for all 25 traversable biomes (fertile/mining/forestry). Key sub-type extremes: Glassfields 100% mining, Mangrove 100% forestry, Rainforest 80% forestry. Building slot counts quantified per area type and hex rarity (Control 2–6, Materials 2–3, Bare 2–5, Underworld/Special always 1).
→ Output: Integrated into **Eternal_Game_Base.md §6** (Area generation tables)

### 4d. Resource Node Seeding ✅

Node counts per materials sub-type: Fertile 2–4, Mining 1–3, Forestry 2–4 (with biome bonuses — +1 in Forest/Rainforest/Taiga/Jungle). Node properties: resource type (weighted from affinity matrix), yield multiplier (0.5×–2.0×), regrowth rate (0.3×–1.5×), depletion threshold (50–200 harvests), special flag (5% chance). Regrowth cooldowns: Fertile 2 in-game days, Mining 6, Forestry 4 (modified by regrowth rate). Universal discovery resources confirmed as action-layer rolls, not node-seeded.
→ Output: Integrated into **Eternal_Game_Base.md §6** (Resource node seeding tables)

> **Phase 4 summary**: 27 biomes × 87 raw resources fully mapped. 2 appendices (D, E) rewritten at v0.2.0. Base module §6 updated with 27-biome materials sub-type weights and node seeding parameters. Biome list expanded from 25 to 27: added Woodland, Rainforest, Scrubland, Mangrove, Glassfields, Blight; renamed Wetlands→Marsh, Volcanic→Scorched; removed Beach, Oasis, Deep Ocean. All values are base-module defaults subject to autoregulator bounds and Game Master adjustment (§27).

---

## PHASE 5 — Movement, Exploration & Surveying

Locks the action catalog entries for movement actions with exact per-biome values.

Dependencies: Phase 1 (energy budget), Phase 2 (action definitions + draft costs), Phase 4 (biome modifiers).

### 5a. Travel Action

Base energy cost, base time-lock, per-biome cost multiplier, survey-completeness discount, infrastructure discount, mount discount.
→ Output: Per-biome travel cost table

### 5b. Explore Action

Base energy cost, base time-lock, per-biome cost multiplier, hazard check trigger, area discovery quality formula.
→ Output: Per-biome explore cost table

### 5c. Survey Action

Energy cost, time-lock. Does it vary by area density or type?
→ Output: Survey costs

### 5d. Delve Action

Energy cost, time-lock, hazard multiplier, reward scaling. Distinct from explore — focused on sub-hex dungeon/ruin/cave areas.
→ Output: Delve mechanics

### 5e. Water Traversal

Which water biomes are passable (Coast, Lake)? Which are impassable without future module (Ocean, Coastal Waters)? Energy multiplier for passable water.
→ Output: Water rules

---

## PHASE 6 — Production

Locks all resource-generating action formulas with exact values.

Dependencies: Phase 1 (attributes), Phase 2 (action definitions + draft costs), Phase 3 (resource catalog), Phase 4 (biomes — modifiers, node placement).

### 6a. Mining

Full cycle: prospecting → extraction → hauling → collapse management. Energy cost, time-lock, yield formula `f(vein_depth, multiplier, INT, tool_bonus)`, vein depth ranges per ore type, depletion per extraction, collapse probability curve, clearing cost/time, shaft reinforcement.
→ Output: Mining mechanics table

### 6b. Logging

Full cycle: tree selection → felling → bucking → hauling. Energy cost, time-lock, yield formula, tree density depletion rate, regrowth rate, ecosystem trade-off curve (logging % → fauna/flora yield %).
→ Output: Logging mechanics table

### 6c. Farming

Full cycle: soil preparation → sowing → tending → harvesting. Energy cost per sub-phase, growth time (ticks from sow to harvest-ready), yield formula, fertility depletion per harvest cycle, natural fertility regrowth rate, fertilise action cost + effect, biome affinity yield modifiers, crop type variety.
→ Output: Farming mechanics table

### 6d. Foraging

Full cycle: scouting → identification → gathering. Energy cost, time-lock, yield formula, wild growth depletion and regrowth, poisoning probability curve, foraged food types vs farming crops. Distinct from Farming — exploits natural growth rather than cultivating it.
→ Output: Foraging mechanics table

### 6e. Hunting

Full cycle: tracking → stalking → kill → field dressing. Energy cost, time-lock, success probability formula `f(SUR, gear, fauna_density)`, yield table on success (meat, pelts, bone, tallow), fauna density → success chance curve, fauna depletion from over-hunting and regrowth.
→ Output: Hunting mechanics table

### 6f. Fishing

Full cycle: locating waters → casting/setting → landing. Energy cost, time-lock, yield formula. Fish is a core Food resource (carried from Eternum). Water-adjacent hex requirement, biome affinity (coast, lake, river), aquatic fauna density modifiers.
→ Output: Fishing mechanics table

### 6g. Herding

Full cycle: feeding → breeding → health management → product collection → working animal raising. Establishment cost (energy + initial stock), yield rates (food + materials per in-game day), grazing fertility depletion rate, herd decline thresholds, sickness probability, biome yield modifiers. Donkey and horse breeding rates, maturation time, and binding cost (Recruiting action) for animal follower acquisition.
→ Output: Herding mechanics table

---

## PHASE 7 — Crafting & Recipes

Transforms resources into items.

Dependencies: Phase 3 (resource catalog — recipe inputs/outputs). Phase 6 (production yields) is informational for balance but not a hard dependency — recipes can be defined independently of exact yield rates.

### 7a. Refining Recipes

Full recipe list: input resources + quantities → output + quantities. Facility required. Min craftsmanship. Quality tier probability at each skill bracket.
→ Output: Appendix G: Refining Recipe Table

### 7b. Crafting Recipes

Full recipe list: inputs → output item type. Facility, min craftsmanship, base properties, mutation range, greatness range.
→ Output: Appendix H: Crafting Recipe Table

### 7c. Food & Cooking Recipes

Cooking is part of the crafting system. Campfire and Cookhouse recipes: input food + fuel → cooked food. Energy buff values, durations, meal tiers (Basic → Standard → Fine → Feast). Chef trait interaction. Recipe discovery mechanics.
→ Output: Food & Cooking recipe table (within Appendix H)

### 7d. Salvaging

Material recovery rates from breaking down crafted items. Recovery % as function of item condition and CRA. Which materials are recoverable vs lost. Comparison: Salvaging (items) vs Demolishing (buildings) — different recovery formulas.
→ Output: Salvaging mechanics

### 7e. Building Material Recipes

Planks, bricks, nails, rope, etc. — inputs + quantities.
→ Output: Building material recipes

### 7f. Mutation Function

XOR-seed parameters. Property blend weights. Legendary threshold values. CRA influence on distribution width.
→ Output: Mutation parameters

---

## PHASE 8 — Items & Equipment

What crafted items actually do when equipped.

Dependencies: Phase 7 (recipes define what items exist), Phase 1 (attributes for requirements).

### 8a. Base Item Stats

Per item type (sword, axe, bow, spear, leather armor, chain armor, etc.): base damage/defense, attribute requirements, weight (kg), durability max.
→ Output: Appendix I: Item Catalog

### 8b. Greatness Scale

Greatness 1–20 → stat multiplier curve. Linear, logarithmic, exponential?
→ Output: Greatness curve

### 8c. Durability & Degradation

Durability loss per action type (encounter, mining, logging, travel). Repair cost formula (resources + energy as function of damage). CRA reduces repair cost by −4%/pt.
→ Output: Durability table

### 8d. Tool Bonuses

Pickaxe → mining yield. Hatchet → logging. Sickle → harvest. Bow/traps → hunting success. Hammer → build speed.
→ Output: Tool bonus table

---

## PHASE 9 — Buildings

The big construction table.

Dependencies: Phase 3 (resources for costs), Phase 7e (building material recipes), Phase 1b (staffing formulas).

### 9a. Building Catalog

For each of 12 base buildings: construction cost (resources + building materials + energy + time), staffing requirement, upkeep cost (resources + energy per in-game day), condition degradation rate, effect/bonus values, area type restrictions.
→ Output: Appendix J: Building Catalog

### 9b. Campfire

Special case: no slot, no upkeep, limited recipes, temporary. Placement cost, lifetime.
→ Output: Campfire rules

### 9c. Building Condition

Degradation rate per missed upkeep day. Repair cost formula (building repair is part of Upkeep, distinct from equipment Repairing in Crafting). Inactive threshold (25% — confirm).
→ Output: Condition mechanics

### 9d. Demolition

Energy cost, time-lock, material recovery rate (% of original construction cost returned). Slot freed on completion. Distinct from Salvaging — Demolishing recovers materials from buildings, Salvaging recovers materials from crafted items.
→ Output: Demolition mechanics

---

## PHASE 10 — Settlement, Population & Food

Dependencies: Phase 9 (building catalog for staffing + housing capacity).

### 10a. Housing Capacity

Capacity value per Housing building.
→ Output: Single value or tiered

### 10b. Food Reserves Scaling

Exact formula: `food_to_fill = base_max_pop × k`. What is k? How many food units = 1% bar? Stocking action parameters: energy cost, time-lock, deposit rate.
→ Output: Food scaling formula + Stocking mechanics

### 10c. Settlement Threshold

"Sufficiently developed hex" — minimum buildings to qualify for deed.
→ Output: Threshold definition

### 10d. Deed Costs

Mint deed: energy + time-lock. Apply deed: energy + time-lock.
→ Output: Deed costs

### 10e. Exponential Maintenance

Settlement maintenance curve beyond 7 hexes. Exact formula.
→ Output: Scaling formula

### 10f. Total Staffing Check

Worked examples: typical settlement's total staffing vs population growth rate. Does it balance?
→ Output: Sanity check

---

## PHASE 11 — Territorial Sovereignty

Upkeep, decay, and claim mechanics.

Dependencies: Phase 4 (biomes for per-biome upkeep), Phase 9 (buildings), Phase 10 (settlements).

### 11a. Hex Upkeep Formula

`upkeep = base_biome_cost × (1 + productivity_tax) × (1 + building_complexity)`
Define each component. Energy + resource split.
→ Output: Upkeep formula + per-biome base costs

### 11b. Decay Rate

How fast `decay_level` increases per missed upkeep day, per biome.
→ Output: Decay rate table

### 11c. Claim Mechanics

Escrow amount (energy). Grace period (500 blocks → convert to ticks/hours). Defense cost. Watchtower bonus.
→ Output: Claim parameters

---

## PHASE 12 — Encounters & Hazards

The danger and interaction layer. Four encounter types, each with distinct resolution mechanics.

Dependencies: Phase 1 (attributes for resolution), Phase 2 (actions trigger encounters), Phase 3 (resources for drops), Phase 4 (biomes for hazard tables), Phase 8 (items for defense calculations).

### 12a. Beast Catalog

All 75 Loot Survivor beast types: name, tier (T1–T5), lethality value, biome affinity, resource drops on success.
→ Output: Appendix K: Beast Catalog

### 12b. Biome Hazard Tables

Per biome: probability weight for each beast type, encounter trigger chance per action type, encounter type distribution (beast/combat/social/special).
→ Output: Appendix L: Biome Hazard Tables

### 12c. Beast Encounter Resolution

Single-resolution hazard check (not a combat system). Exact formula: inputs (lethality, health, energy, SUR, DEX, equipment defense, time-of-day) → outcome probabilities (avoid, injury, resource, trait, death). Health damage scales encounter severity.
→ Output: Beast encounter resolution formula

### 12d. Combat Encounter Resolution

Hostile entity encounters triggered during explore/delve. Distinct from beast encounters — may involve armed opponents or dungeon guardians. Resolution formula, attribute gates, risk/reward profile.
→ Output: Combat encounter resolution formula

### 12e. Social Encounter Resolution

Friendly or neutral entity encounters during explore/travel/recruiting. Outcomes: trade opportunities, follower recruitment, information, reputation effects. CHA and LEA as primary resolution attributes.
→ Output: Social encounter resolution formula

### 12f. Special Encounter Resolution

Rare encounters tied to surveyed special areas (spawn nodes, ruins, shrines, placeholder structures). Variable resolution depending on area type. May grant unique rewards, unlock content, or trigger story beats.
→ Output: Special encounter resolution formula

### 12g. Encounter Detail Tables (Consolidated)

Each encounter/event needs: name, description, trigger conditions, outcome calculation, attribute gates, potential outcomes (success/failure/injury/death/trait gain). Covers all four encounter types plus production hazards (mining collapse, logging accidents).
→ Output: Event catalog

### 12h. Injury & Damage Scaling

**Injury severity scales with damage dealt**, not fixed per injury type. Severity determines disability conversion probability on healing.

Health damage ranges per beast tier. Injury trait acquisition thresholds. Severity → disability conversion probability curve.
→ Output: Injury scaling table

---

## PHASE 13 — Autoregulator, Minting & Game Masters

The balancing layer. Needs everything else to set sensible initial values. Also defines the interface between the immutable physics (Phases 1–4) and the dynamic game layer managed by autonomous Game Masters.

Dependencies: all previous phases (integration/balance pass).

### 13a. Adventurer Mint Cost

Initial $LORDS cost. Autoregulator bounds [min, max].
→ Output: Mint pricing

### 13b. Autoregulator Parameters

Initial values for all tunable params (11 total including health regen, trait gain, injury→disability conversion). Epoch length (in-game days). PI coefficients (Kp, Ki). Metric definitions + target ranges.
→ Output: Autoregulator config

### 13c. Economic Sanity Check

Worked example: typical adventurer's first 24 real-hours. Energy budget, health risk profile, exploration range, resources gathered, items crafted, buildings built. Does the economy feel right? Revisits the Phase 2d time budget analysis with locked values.
→ Output: Narrative walkthrough

### 13d. Trait Balance Review

Revisit all 120 trait special effect percentages against the now-complete action costs, production yields, and encounter formulas. Validate that no single trait creates degenerate strategies. Revisit the Phase 2c action–trait cross-reference. Adjust percentages as needed.
→ Output: Trait balance audit

### 13e. Game Master Authority Map

Define the precise boundary between immutable physics and Game Master-modifiable parameters. For every system quantified in Phases 1–12, classify each parameter as:
- **Immutable**: Cannot be changed after deployment (time constants, attribute formulas, resource definitions, biome seeds)
- **Autoregulator-tuned**: Adjusted algorithmically within fixed bounds per epoch
- **Game Master-modifiable**: Can be proposed, reviewed, and deployed by the Game Master consensus process

Define the Game Master API surface: which hooks, tables, and parameters they can read/write. Define consensus rules (domain-specific vetoes, approval thresholds). Define the staging/deployment pipeline.
→ Output: Game Master authority matrix + API specification

---

## Phase Dependency Graph

```
Phase 1 (Units/Attributes)
  └→ Phase 2 (Core Actions)
       └→ Phase 3 (Resources) ← no upstream dependency on biomes
            └→ Phase 4 (Biomes) ← needs resource catalog for affinity/seeding
                 ├→ Phase 5 (Movement) ← needs biome modifiers
                 └→ Phase 6 (Production) ← needs biomes + resources
                      └→ Phase 7 (Crafting) ← needs resources (hard), production (soft)
                           └→ Phase 8 (Items) ← needs crafting recipes
                                ├→ Phase 9 (Buildings) ← needs resources + building materials
                                │    └→ Phase 10 (Settlement) ← needs buildings
                                │         └→ Phase 11 (Territorial) ← needs biomes + buildings + settlements
                                └→ Phase 12 (Encounters) ← needs biomes + resources + items
                                     └→ Phase 13 (Autoregulator) ← needs everything
```

---

## Appendices

| Appendix | Phase | Estimated rows |
|---|---|---|
| C: Trait Catalog | 1c | ~120 rows |
| D: Biome Table | 4a | 25 rows × 6 cols |
| E: Biome–Resource Affinity | 4b | 25 × ~50 (sparse) |
| F: Resource Catalog | 3a | ~60 rows |
| G: Refining Recipes | 7a | ~20–30 rows |
| H: Crafting Recipes | 7b | ~30–50 rows |
| I: Item Catalog | 8a | ~25–40 rows |
| J: Building Catalog | 9a | 12 rows (dense) |
| K: Beast Catalog | 12a | 75 rows |
| L: Biome Hazard Tables | 12b | 25 biomes × variable |
| M: Action Catalog | 2 | 23 actions + 4 encounter types |

---

*Quantification plan v0.5 — March 2026*
*Phases 1–2 complete. Phase 3 (Resources) next.*
