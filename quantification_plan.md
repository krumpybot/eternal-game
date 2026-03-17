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

10 attributes, 20-point budget at mint. Attributes serve as **performance gates** for encounters and events.

| Attribute | Primary Effect | Formula |
|---|---|---|
| Strength (STR) | Carry capacity | `carry_cap = 50 + STR × 5 kg` |
| Endurance (END) | Max energy, energy regen | `max_energy = 100 + END × 10`; regen rate `+2%/pt` |
| Dexterity (DEX) | Hazard avoidance | `avoid_bonus = DEX × 5%` |
| Vitality (VIT) | Max health | `max_health = 100 + VIT × 10` |
| Intelligence (INT) | Resource yield | `yield = base × (1 + INT × 0.025)` |
| Wisdom (WIS) | Explore & survey energy efficiency | `cost = base × (1 − WIS × 0.04)` — floor at 0 (100% reduction theoretically possible but near-impossible by design) |
| Charisma (CHA) | Upkeep energy efficiency | `cost = base × (1 − CHA × 0.03)` — floor at 0; fee discount `CHA × 5%`; +days until follower desertion |
| Survival (SUR) | Hunting, foraging, beasts | `hunt += SUR × 5%`; `forage += SUR × 5%`; `beast += SUR × 2.5%` |
| Craftsmanship (CRA) | Craft quality, repair, mutation | `quality += CRA × 5%`; repair `−4%/pt`; widens mutation range |
| Leadership (LEA) | Follower count | `max_followers = 1 + floor(LEA / 2)` |

**No minimum clamp** on WIS/CHA reduction — floor is 100% reduction (0 energy cost). This is near-impossible to achieve due to the 20-point attribute budget and diminishing returns from system design.

### 1c. Trait List ✅

5 trait types: **personality**, **physical**, **skill**, **injury**, **disability**.

- Spawn: 2 personality + 1 physical
- Max 10 traits, increasingly difficult to gain
- Modifier shapes: `[+1]`, `[−1]`, `[+2]`, `[−2]`, `[+1, +1]`, `[+1, −1]`, `[−1, −1]`
- No-doubling rule (modifier ≠ same stat as special effect)
- Exclusive pairs enforced for personality/physical
- **Personality and physical traits**: fully scoped in base module (see `trait_table_draft.md`)
- **Skill, injury, disability traits**: base set defined; new traits can be introduced by future modules

Full catalog: `trait_table_draft.md`

### 1d. Follower Scaling ✅

| Type | Effect | Notes |
|---|---|---|
| Laborer | Improves harvest/build throughput | — |
| Donkey | +50 kg carry capacity | Pack animal |
| Mount | −20% travel energy/time, +20 kg carry | Max 1 mount per adventurer |

Base followers: 1. Max: `1 + floor(LEA / 2)`.

### Follow-up from Phase 1

**Encounters & events detail table** — needed as part of quantification. Each encounter/event requires: event name, description, outcome calculation, attribute gates, potential outcomes (success/failure/injury/death/trait gain). This cuts across multiple phases (Beasts §11, Production §5, Movement §4) and will be consolidated as encounters are quantified in those phases.

---

## PHASE 2 — Biomes & World Generation

The terrain layer. Defines the environment that all actions operate in.
Dependencies: Phase 1 (attribute formulas inform what "modifier" means).

### 2a. Biome Properties

For each of 25 biomes: movement energy cost modifier, explore time modifier, travel time modifier, decay rate modifier (territorial upkeep), base hazard chance.
→ Output: Appendix D: Biome Table

### 2b. Biome Resource Affinity

For each biome: which resource nodes can appear (food, ores, trees, special). Probability weights for each. World-gen lookup table.
→ Output: Appendix E: Biome–Resource Affinity Matrix

### 2c. Area Generation

Rarity distribution for area counts (3–9): exact percentages. Per-area-type slot count distributions. Per-rarity building slot counts.
→ Output: Area rarity table

### 2d. Resource Node Seeding

Per materials area: how many nodes, node type distribution (weighted by biome affinity), yield multiplier range, regrowth rate range.
→ Output: Node generation parameters

---

## PHASE 3 — Resources

Every item, recipe, and building cost references resource IDs and properties.
Dependencies: Phase 2 (biome affinity tells us which resources appear where).

### 3a. Resource Properties

For every resource (food, core 22, special): weight per unit (kg), rarity tier, base nutrition value (food only), base trade value (relative index).
→ Output: Appendix F: Resource Catalog

### 3b. Food & Nutrition

Nutrition values for each food type. Raw vs cooked multiplier. How nutrition maps to food-reserve deposit amounts (deposit X food → Y% bar). Energy buff values for cooked food tiers.
→ Output: Food mechanics table

### 3c. Core Resource Tiers

T1/T2/T3 yield rates relative to each other. Refinement ratios (e.g., 3 copper ore → 1 copper bar). Quality tier split (dirty/clean/pure — what % at each skill level).
→ Output: Refinement ratios table

---

## PHASE 4 — Movement, Exploration & Surveying

Costs for the most basic actions.
Dependencies: Phase 1 (energy budget), Phase 2 (biome modifiers).

### 4a. Explore Action

Base energy cost, base time-lock (ticks), biome modifier application (multiply or add?), hazard check trigger.
→ Output: Per-biome explore cost table

### 4b. Travel Action

Base energy cost, base time-lock, survey-completeness discount, infrastructure discount.
→ Output: Per-biome travel cost table

### 4c. Survey Action

Energy cost, time-lock. Does it vary by area type?
→ Output: Survey costs

### 4d. Water Traversal

Which water biomes are passable (Coast, Beach, Lake, Coastal Waters)? Which are impassable without future module (Ocean, Deep Ocean)? Energy multiplier for passable water.
→ Output: Water rules

---

## PHASE 5 — Production

All resource-generating actions. The big one — lots of formulas and yield tables.
Dependencies: Phase 1 (attributes), Phase 2 (biomes), Phase 3 (resources).

### 5a. Mining

Energy cost, time-lock, yield formula `f(vein_depth, multiplier, INT, tool_bonus)`, vein depth ranges per ore type, depletion per extraction, collapse probability curve, clearing cost/time.
→ Output: Mining mechanics table

### 5b. Forestry (Logging)

Energy cost, time-lock, yield formula, tree density depletion rate, regrowth rate, ecosystem trade-off curve (logging % → fauna/flora yield %).
→ Output: Forestry mechanics table

### 5c. Hunting

Energy cost, time-lock, success probability formula `f(SUR, gear, fauna_density)`, yield table on success, fauna density → success chance curve.
→ Output: Hunting mechanics table

### 5d. Foraging & Harvesting

Energy cost, time-lock, yield formula, fertility depletion per harvest, natural regrowth rate, fertilize action cost + effect, salt-the-earth cost + recovery time.
→ Output: Foraging mechanics table

### 5e. Livestock

Establishment cost (energy + initial stock), yield rates (food + special per in-game day), grazing fertility depletion rate, herd decline thresholds, biome yield modifiers.
→ Output: Livestock mechanics table

### 5f. Sowing Crops

Energy cost, growth time, yield per harvest, biome affinity yield modifiers, fertility impact vs wild foraging.
→ Output: Crop table

---

## PHASE 6 — Crafting & Recipes

Transforms resources into items. References Phase 3 + 5.
Dependencies: Phase 3 (resource catalog), Phase 5 (production yields).

### 6a. Refining Recipes

Full recipe list: input resources + quantities → output + quantities. Facility required. Min craftsmanship. Quality tier probability at each skill bracket.
→ Output: Appendix G: Refining Recipe Table

### 6b. Crafting Recipes

Full recipe list: inputs → output item type. Facility, min craftsmanship, base properties, mutation range, greatness range.
→ Output: Appendix H: Crafting Recipe Table

### 6c. Food Recipes

Campfire and Cookhouse recipes. Input food + fuel → cooked food. Energy buff values + duration.
→ Output: Food recipe table

### 6d. Building Material Recipes

Planks, bricks, nails, rope, etc. — inputs + quantities.
→ Output: Building material recipes

### 6e. Mutation Function

XOR-seed parameters. Property blend weights. Legendary threshold values. CRA influence on distribution width.
→ Output: Mutation parameters

---

## PHASE 7 — Items & Equipment

What crafted items actually do when equipped.
Dependencies: Phase 6 (recipes define what items exist), Phase 1 (attributes).

### 7a. Base Item Stats

Per item type (sword, axe, bow, spear, leather armor, chain armor, etc.): base damage/defense, attribute requirements, weight (kg), durability max.
→ Output: Appendix I: Item Catalog

### 7b. Greatness Scale

Greatness 1–20 → stat multiplier curve. Linear, logarithmic, exponential?
→ Output: Greatness curve

### 7c. Durability & Degradation

Durability loss per action type (encounter, mining, logging, travel). Repair cost formula (resources + energy as function of damage). CRA reduces repair cost by −4%/pt.
→ Output: Durability table

### 7d. Tool Bonuses

Pickaxe → mining yield. Hatchet → logging. Sickle → harvest. Bow/traps → hunting success. Hammer → build speed.
→ Output: Tool bonus table

---

## PHASE 8 — Buildings

The big construction table.
Dependencies: Phase 3 (resources), Phase 6d (building materials), Phase 1b (staffing).

### 8a. Building Catalog

For each of 12 base buildings: construction cost (resources + building materials + energy + time), staffing requirement, upkeep cost (resources + energy per in-game day), condition degradation rate, effect/bonus values, area type restrictions.
→ Output: Appendix J: Building Catalog

### 8b. Campfire

Special case: no slot, no upkeep, limited recipes, temporary. Placement cost, lifetime.
→ Output: Campfire rules

### 8c. Building Condition

Degradation rate per missed upkeep day. Repair cost formula. Inactive threshold (25% — confirm).
→ Output: Condition mechanics

---

## PHASE 9 — Settlement, Population & Food

Dependencies: Phase 8 (building catalog for staffing + housing capacity).

### 9a. Housing Capacity

Capacity value per Housing building.
→ Output: Single value or tiered

### 9b. Food Reserves Scaling

Exact formula: `food_to_fill = base_max_pop × k`. What is k? How many food units = 1% bar?
→ Output: Food scaling formula

### 9c. Settlement Threshold

"Sufficiently developed hex" — minimum buildings to qualify for deed.
→ Output: Threshold definition

### 9d. Deed Costs

Mint deed: energy + time-lock. Apply deed: energy + time-lock.
→ Output: Deed costs

### 9e. Exponential Maintenance

Settlement maintenance curve beyond 7 hexes. Exact formula.
→ Output: Scaling formula

### 9f. Total Staffing Check

Worked examples: typical settlement's total staffing vs population growth rate. Does it balance?
→ Output: Sanity check

---

## PHASE 10 — Territorial Sovereignty

Upkeep, decay, and claim mechanics.
Dependencies: Phase 2 (biomes), Phase 8 (buildings), Phase 9 (settlements).

### 10a. Hex Upkeep Formula

`upkeep = base_biome_cost × (1 + productivity_tax) × (1 + building_complexity)`
Define each component. Energy + resource split.
→ Output: Upkeep formula + per-biome base costs

### 10b. Decay Rate

How fast `decay_level` increases per missed upkeep day, per biome.
→ Output: Decay rate table

### 10c. Claim Mechanics

Escrow amount (energy). Grace period (500 blocks → convert to ticks/hours). Defense cost. Watchtower bonus.
→ Output: Claim parameters

---

## PHASE 11 — Beasts & Hazards

The danger layer.
Dependencies: Phase 1 (attributes), Phase 2 (biomes), Phase 3 (resources).

### 11a. Beast Catalog

All 75 Loot Survivor beast types: name, tier (T1–T5), lethality value, biome affinity, resource drops on success.
→ Output: Appendix K: Beast Catalog

### 11b. Biome Hazard Tables

Per biome: probability weight for each beast type. Total encounter chance per action type.
→ Output: Appendix L: Biome Hazard Tables

### 11c. Encounter Resolution

Exact formula: inputs (lethality, health, energy, SUR, DEX, equipment defense, time-of-day) → outcome probabilities (avoid, injury, resource, trait, death). Health damage scales encounter severity.

**Encounter & event detail tables** (consolidated from Phase 1 follow-up): each encounter/event needs: event name, description, outcome calculation, attribute gates, potential outcomes (success/failure/injury/death/trait gain). Covers beast encounters, production hazards (mining collapse, logging accidents), and exploration events.
→ Output: Resolution formula + event catalog

### 11d. Injury & Damage Scaling

**Injury severity scales with damage dealt**, not fixed per injury type. A 50-damage beast attack produces a high-severity laceration; a 15-damage scratch produces a low-severity bruise. Severity determines disability conversion probability on healing.

Health damage ranges per beast tier. Injury trait acquisition thresholds. Severity → disability conversion probability curve.
→ Output: Injury scaling table

---

## PHASE 12 — Autoregulator & Minting

The balancing layer. Needs everything else to set sensible initial values.
Dependencies: all previous phases (integration/balance pass).

### 12a. Adventurer Mint Cost

Initial $LORDS cost. Autoregulator bounds [min, max].
→ Output: Mint pricing

### 12b. Autoregulator Parameters

Initial values for all tunable params (11 total including health regen, trait gain, injury→disability conversion). Epoch length (in-game days). PI coefficients (Kp, Ki). Metric definitions + target ranges.
→ Output: Autoregulator config

### 12c. Economic Sanity Check

Worked example: typical adventurer's first 24 real-hours. Energy budget, health risk profile, exploration range, resources gathered, items crafted, buildings built. Does the economy feel right?
→ Output: Narrative walkthrough

---

## Appendices (added to Eternal_Game_Base.md)

| Appendix | Phase | Estimated rows |
|---|---|---|
| C: Trait Catalog | 1c | ~100 rows |
| D: Biome Table | 2a | 25 rows × 6 cols |
| E: Biome–Resource Affinity | 2b | 25 × ~50 (sparse) |
| F: Resource Catalog | 3a | ~60 rows |
| G: Refining Recipes | 6a | ~20–30 rows |
| H: Crafting Recipes | 6b | ~30–50 rows |
| I: Item Catalog | 7a | ~25–40 rows |
| J: Building Catalog | 8a | 12 rows (dense) |
| K: Beast Catalog | 11a | 75 rows |
| L: Biome Hazard Tables | 11b | 25 biomes × variable |

---

*Quantification plan v0.2 — March 2026*
*Phase 1 completed. Phase 2 next.*
