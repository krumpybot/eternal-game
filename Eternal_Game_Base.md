# Eternal Game — Base Module Scope v0.1.1

> **The base module defines the physics of the world.** Once deployed, these contracts never change. Every future module — combat, trade, governance, lore — must build on top of this layer without modifying it. This document specifies exactly what ships in the base module, what is explicitly deferred, and the interfaces future modules will use.

---

## Table of Contents

1. [Design Intent](#1-design-intent)
2. [What "Immutable" Means](#2-what-immutable-means)
3. [Module Boundary](#3-module-boundary)
4. [Game Masters Interface](#4-game-masters-interface)
5. [Time](#5-time)
6. [Space](#6-space)
7. [World Generation](#7-world-generation)
8. [The Adventurer](#8-the-adventurer)
9. [Followers](#9-followers)
10. [Energy](#10-energy)
11. [Movement & Exploration](#11-movement--exploration)
12. [Surveying](#12-surveying)
13. [Resource Definitions](#13-resource-definitions)
14. [Production: Mining](#14-production-mining)
15. [Production: Forestry](#15-production-forestry)
16. [Production: Hunting](#16-production-hunting)
17. [Production: Foraging & Harvesting](#17-production-foraging--harvesting)
18. [Production: Livestock](#18-production-livestock)
19. [Production: Fishing](#19-production-fishing)
20. [Crafting & Refining](#20-crafting--refining)
21. [Equipment & Items](#21-equipment--items)
22. [Construction & Buildings](#22-construction--buildings)
23. [Settlement](#23-settlement)
24. [Population, Housing & Food](#24-population-housing--food)
25. [Territorial Sovereignty](#25-territorial-sovereignty)
26. [Beasts & Hazards](#26-beasts--hazards)
27. [Permadeath & Legacy](#27-permadeath--legacy)
28. [Autoregulation](#28-autoregulation)
29. [Extension Interfaces](#29-extension-interfaces)
30. [Future Module Placeholders](#30-future-module-placeholders)
31. [What Is Explicitly Deferred](#31-what-is-explicitly-deferred)
32. [Playable State](#32-playable-state)

---

## Lock Status Convention

Throughout this document and all appendices, the following symbols indicate whether a feature or value is immutable after deployment:

| Symbol | Meaning |
|---|---|
| 🔒 | **Locked** — immutable after deployment. Cannot be changed by Game Masters, the autoregulator, or any future module. Part of the permanent world physics. |
| ⚠️ | **Flagged for review** — section contains implementation-critical logic requiring specialist validation before deployment. |

All features and values **without** a lock symbol are assumed to be **starting values only** — modifiable by Game Masters via consensus (§4 of the Design Scope), the autoregulator within bounded ranges, or future modules.

---

## 1. Design Intent

The base module must satisfy three constraints simultaneously:

1. **Complete physics.** Every primitive that future modules need to reference — coordinates, time, energy, matter, ownership, death — must exist and be final. If a future module discovers it needs a new primitive, the base module has failed.

2. **Fun on its own.** A player with nothing but the base module should be able to: explore, discover terrain, harvest resources, craft items, build structures, form a settlement, manage its population, fight beasts, and die permanently. The core loop must feel like a game, not an SDK demo.

3. **Lean.** Every feature that can be deferred without breaking constraint #1 or #2 must be deferred. Settlement upgrades to cities/kingdoms/empires? Deferred. Guild contracts? Deferred. PvP combat? Deferred. Trade markets? Deferred. The base module is the tightest possible foundation that is still a complete, playable game.

### The Starknet constraint

This game is designed to run forever on Starknet (likely an L3 appchain). Every state read/write is a transaction. Every computation costs gas. The base module must be:

- **Storage-efficient**: lazy evaluation wherever possible; no redundant state.
- **Compute-efficient**: deterministic derivation from seeds over stored arrays.
- **Tick-safe**: robust to chain halts, reorgs, and variable block times.

---

## 2. What "Immutable" Means

Once the base module is deployed:

- **No contract upgrades** that change state layout or behavioral semantics.
- **No balance patches** to world generation outputs (a hex discovered today has the same properties in 10 years).
- **No retroactive changes** to energy rates, regen formulas, biome assignments, or resource definitions.
- **Tunable parameters** (e.g., autoregulator coefficients, hazard lethality curves) are allowed **only** through the on-chain autoregulator — a permissionless PI controller that adjusts within pre-defined bounds. No admin keys.

What *can* change:
- New modules can be deployed that **read** base module state and **write** to their own state.
- New modules can call base module **extension points** (hooks) that the base module exposes for exactly this purpose.
- Client implementations can change freely — the base module is headless.

---

## 3. Module Boundary

### In scope (base module)

| System | Included |
|---|---|
| Time | Tick system, hybrid guard, in-game day cadence |
| Space | Hex grid, cube coordinates, codec, adjacency |
| World generation | Biomes, area generation, resource node seeding |
| Adventurer | Creation, attributes, traits, health, equipment slots, inventory, death |
| Followers | Party system, leadership gating, food consumption |
| Energy | Pool, regen, reservation, spend |
| Movement | Explore (unknown hex), travel (known hex) |
| Surveying | Area reveal within a hex |
| Production | Mining, forestry, hunting, foraging/harvesting, livestock |
| Crafting | Refining (raw → core → quality), mutation-based crafting |
| Items | Equipment, durability, repair, weight |
| Construction | Building placement, material costs, building upkeep |
| Settlement | Deed system, hex clustering, population, food reserves |
| Sovereignty | Ownership, decay, claim/defend |
| Beasts | Biome hazards (Loot Survivor beast types), encounter resolution |
| Permadeath | True death, inventory destruction, legacy records |
| Autoregulation | PI controller for economic parameter tuning |

### Out of scope (future modules)

| System | Why deferred |
|---|---|
| City/Kingdom/Empire upgrades | Requires proven settlement meta first |
| PvP combat | Separate combat module; base provides health/energy/death primitives |
| Trade markets & local economy | Requires settlements + population density |
| Guilds & governance | Social layer; base provides ownership primitives |
| Quests & narrative | Content layer |
| C&C dungeon integration | Requires combat module |
| $SURVIVOR token mechanics | Requires dungeon/bounty systems |
| Realm NFT settlement bonuses | Ecosystem integration module |
| Loot Vault minting | Requires combat + item persistence rules |
| Genesis Adventurer respawn | Requires Loot integration |
| Spawn Nodes (secondary) | Can be added as a module reading base state |
| Agent/autonomous play hooks | x402/AI layer is a module |
| Fog of war | Requires Starknet privacy primitives |

---

## 4. Game Masters Interface

The base module defines the boundary between the **immutable physics** (Phases 1–4: time, attributes, traits, resources, biomes) and the **dynamic game layer** (Phases 5+) managed by autonomous AI agents known as the Game Masters (see §4 of the Design Scope).

### What Game Masters can modify

Everything above the immutable foundation is within Game Master authority, subject to their internal consensus process. In the base module, this includes:

| Domain | Examples | Constraints |
|---|---|---|
| **Crafting recipes** | New recipes, ingredient combinations, yield values | Must use resources from the locked resource catalog (Appendix F). Cannot create new resource IDs. |
| **Cooking recipes** | New meals, buff values, ingredient requirements | Must use food resources from the catalog. Buff values bounded by meal tier system. |
| **Encounter tables** | New beast encounters, social encounters, special events | Must use defined encounter types (beast, combat, social, special). Outcomes bounded by attribute/trait system. |
| **Drop rates & yields** | Resource node yield multipliers, beast drop tables | Within autoregulator bounds. Cannot exceed parameter ranges defined in §27. |
| **Event triggers** | Seasonal events, rare world events, discovery triggers | Must fire through the event hook system (§28). Cannot modify core game loops. |
| **Balance parameters** | Action costs, energy modifiers, time-lock durations | Within autoregulator bounds. Subject to Balancer + Economist consensus. |
| **Lore & flavour** | Item descriptions, encounter narratives, world history entries | Subject to Loremaster approval. |
| **NPC behaviours** | Encounter NPC dialogue, trading behaviours, quest-like interactions | Must operate within the action/encounter framework. |

### What Game Masters cannot modify

The immutable foundation is off-limits:

- Tick length, in-game day duration, or any time constant (§5)
- Attribute formulas, attribute count, or attribute budget (§8)
- Trait definitions in the base trait catalog (Appendix C) — though Game Masters may propose new traits for future modules
- Core resource definitions (the 22 Eternum-heritage resources) or the resource ID registry structure (Appendix F)
- Biome definitions and world generation seed (§7)
- Hex coordinate system and adjacency rules (§6)
- Energy pool mechanics and base regen formula (§10)
- Permadeath rules (§26)
- The autoregulator's algorithm or parameter bounds (§27) — though the Balancer may propose bound changes to the core team

### Base module hooks for Game Masters

The base module exposes these interfaces for Game Master interaction:

```
// Recipe management
register_recipe(recipe_id, inputs[], output, facility, min_skill, metadata)
update_recipe_values(recipe_id, field, new_value)  // within bounds
disable_recipe(recipe_id)  // soft-disable, not delete

// Encounter management
register_encounter(encounter_id, type, trigger_conditions, outcomes[], metadata)
update_encounter(encounter_id, field, new_value)
set_encounter_weight(biome, encounter_id, weight)  // per-biome frequency

// Drop rate management
set_drop_rate(source_type, resource_id, base_rate)  // within autoregulator bounds
set_beast_drop_table(beast_type, drops[])

// Event management
register_event(event_id, trigger, duration, effects[], metadata)
activate_event(event_id)
deactivate_event(event_id)

// Balance adjustment (within autoregulator bounds)
adjust_action_cost(action_id, energy_cost, time_cost)  // bounded
adjust_yield_modifier(resource_id, modifier)  // bounded
```

All Game Master actions are logged on-chain and published in a **player-visible changelog** for full transparency.

### Consensus requirement

All changes require **unanimous approval** from all five Game Masters before deployment. A single objection blocks the change. Every decision must fit within the historical context of the game — Game Masters cannot break continuity or contradict the onchain world state.

### Decentralisation

Each Game Master agent is individually managed by a member of the decentralised core team. They are not co-located or centrally controlled. The unanimous consensus requirement ensures no single operator can push changes unilaterally.

### Anti-collusion constraints

Game Master programming includes hard constraints against self-serving rule changes. They cannot propose, approve, or deploy changes that benefit their own adventurers or their operator's interests.

### Game Master adventurers

Game Masters may operate adventurers. These adventurers:
- Are minted and managed identically to any other adventurer
- Have no special access, information asymmetry, or mechanical advantage
- Live and die by the same physics
- Are **not identified** as Game Master-operated in the UI — they are indistinguishable from any other player
- Cannot benefit from Game Master decisions (enforced by anti-collusion constraints)

---

## 5. Time 🔒

### Tick system

- **Tick length**: `T = 10 seconds`.
- On any state-modifying call:
  - `current_tick = floor(block_timestamp / T)`
  - `wall_delta = current_tick - last_tick`
  - `block_delta = block_number - last_block_number`
  - `tick_delta = min(wall_delta, block_delta)` ← hybrid guard

### In-game day

- **1 in-game day = 4 real-world hours = 1,440 ticks**.
- **6 in-game days per real-world day**.
- Population, food drain, and decay are evaluated on day boundaries.
- The day boundary is computed from tick count, not wall-clock. If the chain halts, in-game days pause.

### Stored time state (per entity)

```
last_tick: felt252
last_block_number: felt252
```

Lazy evaluation: time only advances when an entity is touched by a transaction.

---

## 6. Space 🔒

### Coordinate system

- **Infinite hex grid** using cube coordinates `(x, y, z)` where `x + y + z = 0`.
- Coordinate range: ±2.3 × 10¹² (felt252 encoding).
- **Origin (The Nexus)**: `(0, 0, 0)`.
- Storage: deterministic codec packing `(x, y, z)` into a single `felt252`.

### Adjacency

- Six neighbors per hex (cube-coordinate offsets).
- **Adjacent-only movement** — no teleportation. Distance is physical.

### Hex state

Each hex stores:

```
biome: u8                    // deterministic from noise
discovered: bool
discoverer: ContractAddress   // first to explore
controller: ContractAddress   // first to survey control area (area_index 0)
area_count: u8               // 3–9, finalized on discovery
decay_level: u16             // 0–10000 (basis points)
last_upkeep_tick: felt252
settlement_id: felt252       // 0 if not part of a settlement
```

### Sub-hex: Areas

Each hex contains `area_count` areas (3–9). Each area stores:

```
area_index: u8
area_type: enum { Control, Materials, Bare, Underworld, Special }
surveyed: bool
building_slot_count: u8
// resource node data (type-specific, seeded on survey)
```

Area types, slot counts, and resource profiles are **finalized on first survey** using a discovery seed.

---

## 7. World Generation 🔒

### Biome determination

- `biome = noise_function(global_seed, x, y, z)` → one of 24 biome types.
- Biomes are **deterministic and permanent**. No biome ever changes.
- Noise uses five domain-separated layers: Temperature, Moisture, Elevation, Variation, and Anomaly (see Appendix D §4 for full generation algorithm).
- The Nexus `(0,0,0)` is forced to Grassland; first ring is biased temperate. Extreme biomes cannot generate within 3 hexes of the Nexus.

### Biome list (base module)

The full biome set must be defined at deployment because biome-specific logic (movement costs, hazard tables, resource affinities) is baked into the base module:

| Category | Biomes | Count |
|---|---|---|
| **Temperate** | Grassland, Forest, Woodland, Rainforest, Highlands, Scrubland | 6 |
| **Arid** | Desert, Savanna, Badlands, Canyon | 4 |
| **Wet** | Swamp, Mire, Jungle, Mangrove | 4 |
| **Cold** | Tundra, Taiga, Glacier | 3 |
| **Extreme** | Scorched, Glassfields, Blight | 3 |
| **Water** | Coast, Coastal Waters, Lake, Ocean | 4 |
| | **Total** | **24** |

Each biome defines (quantified in Appendix D):
- Movement energy cost modifier (0.8×–1.8×) 🔒
- Survey cost/time modifiers (0.9×–1.6×) 🔒
- Decay rate modifier (0.8×–2.0×)
- Base hazard chance (2%–20%)
- Encounter type distribution (beast / combat / social / special)
- Fauna tier cap (T2–T5)
- Fertility rating (None / Low / Medium / High) 🔒
- Native resource affinities (quantified in Appendix E)

> **Impassable biomes**: Ocean and Coastal Waters are impassable in the base module. Traversal requires a future Maritime module. Coast and Lake are land-accessible.

### Area generation

On first survey of a hex:

1. `discovery_seed = hash(global_seed, x, y, z, discoverer_salt)` (commit-reveal)

⚠️ **Security requirement**: The reveal transaction MUST incorporate the **reveal-block hash** as additional entropy. The seed formula becomes:
`discovery_seed = hash(global_seed, x, y, z, reveal_block_hash, discoverer_salt)`.
This prevents users from brute-forcing salts to predict area outcomes. **Flag for contracts specialist.**
2. `area_count` drawn from rarity-weighted distribution:

| Area Count | Rarity | Probability | Cumulative |
|---|---|---|---|
| 3 | Common | 35% | 35% |
| 4 | Common | 25% | 60% |
| 5 | Uncommon | 18% | 78% |
| 6 | Rare | 10% | 88% |
| 7 | Epic | 6% | 94% |
| 8 | Legendary | 4% | 98% |
| 9 | Mythic | 2% | 100% |

3. **Area type assignment**: Area index 0 is always the Control area. Remaining areas are drawn from a weighted distribution:

| Area Type | Weight | Notes |
|---|---|---|
| **Materials** | 45% | Fertile, mining, or forestry (sub-type determined by biome) |
| **Bare** | 40% | Buildable, no resources |
| **Underworld** | 10% | Rare, future dungeon hook |
| **Special** | 5% | Rarest, future spawn nodes / unique sites |

> Underworld and Special areas are capped at 1 each per hex. If the roll produces a duplicate, it is re-rolled as Materials or Bare (50/50).

4. **Materials area sub-type** is determined by biome suitability:

| Biome | Fertile Weight | Mining Weight | Forestry Weight |
|---|---|---|---|
| Grassland | 60% | 15% | 25% |
| Forest | 15% | 15% | 70% |
| Woodland | 35% | 15% | 50% |
| Rainforest | 10% | 10% | 80% |
| Highlands | 25% | 50% | 25% |
| Scrubland | 40% | 25% | 35% |
| Desert | 5% | 80% | 15% |
| Savanna | 40% | 15% | 45% |
| Badlands | 0% | 90% | 10% |
| Canyon | 0% | 85% | 15% |
| Swamp | 45% | 15% | 40% |
| Mire | 10% | 30% | 60% |
| Jungle | 30% | 10% | 60% |
| Mangrove | 0% | 0% | 100% |
| Tundra | 5% | 55% | 40% |
| Taiga | 10% | 30% | 60% |
| Glacier | 0% | 70% | 30% |
| Scorched | 0% | 95% | 5% |
| Glassfields | 0% | 100% | 0% |
| Blight | 0% | 20% | 80% |
| Coast | 25% | 20% | 55% |
| Lake | 30% | 20% | 50% |

> For biomes where a sub-type has 0% weight (e.g., Badlands fertile), no fertile areas can generate. The percentages are applied per materials-area roll.

5. **Building slot count** per area type and hex rarity:

| Hex Area Count | Control Slots | Materials Slots | Bare Slots | Underworld Slots | Special Slots |
|---|---|---|---|---|---|
| 3 (Common) | 2 | 2 | 2 | 1 | 1 |
| 4 (Common) | 2 | 2 | 2 | 1 | 1 |
| 5 (Uncommon) | 3 | 2 | 3 | 1 | 1 |
| 6 (Rare) | 3 | 3 | 3 | 1 | 1 |
| 7 (Epic) | 4 | 3 | 4 | 1 | 1 |
| 8 (Legendary) | 5 | 3 | 4 | 1 | 1 |
| 9 (Mythic) | 6 | 3 | 5 | 1 | 1 |

> Slot counts scale with hex rarity primarily for Control and Bare areas — these represent development capacity. Materials areas cap at 3 slots to prevent over-industrialisation of single nodes. Underworld and Special areas always have 1 slot (reserved for future module hooks).

6. Results are **stored permanently**. No re-rolls.

### Resource node seeding

Within materials areas, resource nodes are seeded deterministically on first survey:

- `node_seed = hash(discovery_seed, area_index, node_index)` with domain separation (`PLANT_V1`, `ORE_V1`, `TREE_V1`)

**Node count per materials area:**

| Materials Sub-type | Base Node Count | Modifier |
|---|---|---|
| Fertile | 2–4 | +1 in High/Very High fertility biomes |
| Mining | 1–3 | +1 in primary/signature mining biomes |
| Forestry | 2–4 | +1 in Forest/Rainforest/Taiga/Jungle |

**Node properties** (each seeded individually):

| Property | Range | Derivation |
|---|---|---|
| **Resource type** | From biome affinity table | Weighted roll using Appendix E affinity weights (★=3×, ◆=2×, ✓=1×) |
| **Yield multiplier** | 0.5×–2.0× | Uniform from node seed; higher-rarity resources skew lower |
| **Regrowth rate** | 0.3×–1.5× | How fast the node regenerates after depletion. Fertile areas regrow faster. |
| **Depletion threshold** | 50–200 harvests | Total extractions before temporary exhaustion |
| **Special flag** | 0 or 1 | 5% chance — node has a unique property (e.g., higher rare-drop chance, unusual species mix) |

> **Universal discovery resources** (Rare Metals from mining, Worldroot from logging, Unicorn Hair from foraging) are **not** tied to nodes. They are low-chance rolls on every action of their type, regardless of biome or node. This is handled at the action resolution layer, not during node seeding.

⚠️ **Storage note (contracts specialist)**: Do not store the full biome–resource affinity matrix as a dynamic mapping. Hardcode as constants or pack into bitfields. The matrix is used only for node seeding and can be compiled into contract code.

⚠️ **Implementation note (contracts specialist)**: Simplex noise onchain must use fixed-point arithmetic or a lookup-table approximation. Compute biome once on exploration and store it, do not recompute on every access.

**Regrowth mechanics:**

- Depleted nodes enter a cooldown period: `regrowth_ticks = base_cooldown × (1 / regrowth_rate)`.
- Base cooldown: Fertile = 2,880 ticks (2 in-game days), Mining = 8,640 ticks (6 in-game days), Forestry = 5,760 ticks (4 in-game days).
- Mining nodes may collapse instead of depleting (see §14: Production: Mining).
- Fertility-based nodes degrade gradually rather than binary depletion — each harvest reduces current fertility, which regenerates over time at the regrowth rate.

---

## 8. The Adventurer 🔒

### Creation

- Costs $LORDS (amount is an autoregulator-tunable parameter with defined bounds).
- Spawns at The Nexus (origin hex). Secondary spawn nodes are a future module.
- Mint uses commit-reveal: user commits `hash(salt)`, then reveals salt to finalize.
- `adventurer_seed = hash(global_seed, token_id, mint_block, user_salt)`

⚠️ **Security requirement**: The reveal transaction MUST incorporate the **reveal-block hash** as additional entropy. The seed formula becomes:
`adventurer_seed = hash(global_seed, token_id, reveal_block_hash, user_salt)`.
This prevents users from brute-forcing salts to predict outcomes, since the block hash is unknown at commit time. **Flag for contracts specialist.**

### Core state

```
alive: bool
position: (x, y, z)
energy: u32
max_energy: u32
health: u32
max_health: u32
activity_lock_until: felt252   // tick when current action completes
inventory_resources: Map<ResourceId, u32>  // fungible tokens, unlimited stacking, weight-limited
inventory_items: Map<SlotIndex, ItemId>    // non-fungible crafted items (weapons, armor, etc.), one per slot
equipment: Map<Slot, ItemId>   // head, body, waist, hands, feet, neck, ring
attributes: [u8; 10]          // STR, END, DEX, VIT, INT, WIS, CHA, SUR, CRA, LEA
trait_slots: [TraitId; 10]    // max 10 traits (personality, physical, skill, injury, disability)
seed: felt252
```

> **Inventory note:** Resources are fungible tokens with unlimited stacking; inventory is weight-limited (total weight of all carried resources and items cannot exceed carry capacity). Crafted items (weapons, armor, tools, etc.) are non-fungible due to the mutation-based crafting system and each occupies one inventory slot. Inventory may additionally be slot-limited depending on blockchain storage constraints.

### Attributes

| Category | Attributes |
|---|---|
| **Physical** | Strength, Endurance, Dexterity, Vitality |
| **Mental** | Intelligence, Wisdom, Charisma |
| **Specialized** | Survival, Craftsmanship, Leadership |

- **10 attributes**, budget of **20 points** at mint (20 rolls → +1 to random attribute each).
- Computed lazily from seed (no stored array at mint).
- Attributes serve primarily as **performance gates** for encounters and events (e.g., a beast hunt might have a STR gate where success scales with Strength).

#### Attribute progression (chance-based)

Attributes improve through use — **learning by doing** — but via a **chance-based system** rather than XP accumulation. No experience points or historical data need to be tracked on-chain.

- Every action has a **small base chance** to increase a relevant attribute (e.g., successfully mining has a ~1–2% chance to increase STR or INT).
- The chance decreases **exponentially** with the current **natural** attribute level:
  - `gain_chance = base_chance × (1 − (level / 20))²`
  - Level 0 → 100% of base chance; Level 10 → 25%; Level 15 → 6.25%; Level 19 → 0.25%; Level 20 → **0%** (natural cap).
  - **Natural** attribute level caps at 20, but **effective** attribute can exceed 20 via equipment/traits up to a hard clamp of **30**.
  - Attribute gain chance is calculated using the **natural** attribute value only — equipment and trait bonuses do not affect gain probability.
  - Progressing from 19 → 20 is dramatically harder than 18 → 19.
- **Attributes can never be lost.** They can only be **suppressed** by trait modifiers (e.g., a −1 STR trait reduces effective STR but does not remove the underlying point).
- Multiple attributes may roll for a gain from a single action (e.g., travel may roll for both END and WIS).
- Base chance values per action type are autoregulator-tunable.

#### Attribute effects

| Attribute | Primary Effect | Formula |
|---|---|---|
| **Strength (STR)** | Carry capacity | `carry_cap = 50 + STR × 5 kg` |
| **Endurance (END)** | Max energy, energy regen | `max_energy = 100 + END × 10`; energy regen rate `+2%` per point |
| **Dexterity (DEX)** | Hazard avoidance | `avoid_bonus = DEX × 5%` |
| **Vitality (VIT)** | Max health | `max_health = 100 + VIT × 10` |
| **Intelligence (INT)** | Resource yield | `yield = base × (1 + INT × 0.025)` — applies to harvest, gather, mine, refine |
| **Wisdom (WIS)** | Explore & survey energy efficiency | Energy cost = `max(0.25, base × (1 − WIS × 0.025))` |
| **Charisma (CHA)** | Upkeep energy efficiency | Energy cost = `max(0.4, base × (1 − CHA × 0.02))`; fee discount `CHA × 5%`; increases days until follower desertion |
| **Survival (SUR)** | Hunting, foraging, beast encounters | `hunt_success += SUR × 5%`; `forage_success += SUR × 5%`; `beast_encounter_success += SUR × 2.5%` |
| **Craftsmanship (CRA)** | Craft quality, repair cost, mutation | `craft_quality += CRA × 5%`; repair cost `−4%` per point; widens favorable mutation range |
| **Leadership (LEA)** | Follower count | `max_followers = 1 + floor(LEA / 2)` |

### Health

Adventurers have a **health pool** separate from energy:

- **Base max health**: 100 (modified by Vitality: `max_health = 100 + VIT × 10`).
- **Health regen**: Health does not regenerate passively. Recovery requires the **Rest** action (see Appendix M). Rest costs 0 energy, has a short time-lock (6–12 ticks / 1–2 min), and restores a meaningful amount of health (5–15 HP per rest, scaling with VIT). Rest can be repeated.
- Food and special items may increase health recovery by **enhancing Rest effectiveness**, not by adding passive regen.
- Health is lost through **encounter or activity outcomes** (e.g., a logging accident might cost 25 health, a beast attack might cost 50 health).
- Players must weigh the risks of taking actions while health is low — there is always a **low probability of instant death** from any hazardous action when health is critically low.
- **If health reaches 0: the adventurer dies** (permadeath).
- Health and energy are independent systems: an adventurer can have full energy but low health, or vice versa.

### Traits

Traits are organized into **5 types**:

| Type | Description | Gained | Lost |
|---|---|---|---|
| **Personality** | Core character disposition (Brave, Craven, Studious, etc.) | At mint; rarely gained/lost through major life events | Can be replaced by opposite trait through events |
| **Physical** | Bodily characteristics (Brawny, Nimble, Perceptive, etc.) | At mint; rarely gained/lost | Can be replaced by opposite trait through events |
| **Skill** | Aptitudes earned through experience (Green Thumb, Born Hunter, etc.) | Gained by performing actions — positive from success, negative from failure | **Cannot be lost once gained** |
| **Injury** | Temporary wounds (Lacerated, Fractured, Bruised, etc.) | Gained from encounter/activity damage outcomes | Lost when adventurer returns to >75% of maximum health and completes any action; may convert to a disability trait (probability scales with damage that caused the injury) |
| **Disability** | Permanent scars from severe injuries (One-eyed, Lame, etc.) | Converted from severe injury traits on resolution | **Cannot be lost** |

- **At mint**: 2 personality traits + 1 physical trait, derived from seed.
- **Max 10 trait slots**. Trait gain uses a **chance-based system** (no XP tracking):
  - Every action/event has a small base chance to grant a trait.
  - The **criticality of the event outcome** determines the base chance (routine success ~0.5%, critical event ~8–15%).
  - `gain_chance = base_chance × (1 − trait_count / 10)²` — quadratic decrease; at 10 traits, gain chance is **0%**.
- **Exclusivity groups**: grouped traits are fully exclusive (e.g., Brave vs Craven — impossible to have both). Almost all personality and physical traits have an opposite.
- Traits may grant **attribute modifiers**, **special effects**, or both:
  - Maximum modifier per trait: **+2 or −2** to any single attribute.
  - Possible modifier shapes: `[+1]`, `[−1]`, `[+2]`, `[−2]`, `[+1, +1]`, `[+1, −1]`, `[−1, −1]`. Dual-modifier shapes are rarer.
  - **No doubling**: a trait's attribute modifier must not amplify the same stat as its special effect.
  - Some traits have **only a modifier** or **only a special effect** — not every trait needs both.
  - Trait modifiers **suppress** attributes (reduce effective value) but do not remove underlying points.
- Some trait special effects modify **attribute or trait gain chances** rather than direct mechanics (e.g., "Diligent: +10% chance of attribute gain from all actions" means a 1% base chance becomes 1.1%).
- Skill traits define an adventurer's aptitude from their adventures. Positive skill traits are potential outcomes of **successful** actions; negative skill traits are potential outcomes of **failures and defeats**. Skill traits become harder to acquire as the adventurer accumulates more skills.
- Personality traits can be gained/replaced through **major life events** (very rare — surviving near-death encounters, first settlement, etc.).
- Trait list is a large enumeration defined at deployment (see §29 for extensibility).

### Equipment slots

`head, body, waist, hands, feet, neck, ring` — 7 slots total.

### Inventory

- **Weight-limited**. Base carry capacity = 50 kg + STR × 5 kg + gear bonuses.
- All items and resources have weight in **kg** (consistent with Eternum and Blitz).
- Resources are **fungible tokens** with **unlimited stacking** — inventory tracks total weight, not individual units.
- Crafted items (weapons, armor, tools) are **non-fungible** due to mutation-based crafting; each occupies one inventory slot.
- Inventory may be additionally **slot-limited** depending on blockchain storage constraints.
- Exceeding carry capacity prevents movement (not item pickup — you can drop to move).

---

## 9. Followers

Followers are non-adventurer NPCs attached to an adventurer's party. They are the adventurer's personal logistics and support — laborers, donkeys/mounts, and (future module) bodyguards. The term is loosely analogous to medieval serfs: bound to their adventurer's service, providing labor in return for protection.

### Base module scope

```
follower_count: u8           // current party size
max_followers: u8            // derived from Leadership: 1 + floor(LEA / 2)
follower_types: Map<FollowerType, u8>  // laborers, donkeys, mounts
```

### Types (base module)

| Type | Effect | Notes |
|---|---|---|
| **Laborer** | Improves harvest/build task throughput for adventurer actions | — |
| **Donkey** | +50 kg carry capacity | Pack animal, no travel bonus |
| **Mount** | −20% travel energy cost, −20% travel time, +20 kg carry capacity | Maximum 1 mount per adventurer at any time |

### Gating

- Base followers: **1** (even a lone adventurer can have a mount or donkey).
- Maximum party size: `max_followers = 1 + floor(LEA / 2)`. Autoregulator-tunable within bounds.

### Food consumption

- Followers consume food as the adventurer does (shared food resource type).
- Each follower adds to the adventurer's food requirement. Exact quantities per follower type are TBD (see §13).
- If the adventurer cannot feed followers, they desert (follower count decreases). Desertion rate is autoregulator-tunable.

### What followers are NOT

- Followers do **not** count toward settlement population.
- Followers do **not** staff buildings.
- Followers are tracked at the adventurer level, not the hex/settlement level.

---

## 10. Energy 🔒

### Pool

- **Base max energy**: 100 (modified by Endurance: `max_energy = 100 + END × 10`, plus gear bonuses). ~2.8 hours of storage at base regen — aligned with in-game day length.
- **Base regen**: 0.1 energy per tick (~36/hour, ~864/in-game day). Endurance increases regen rate by **+2% per point**.
- Regen is **lazy**: computed on next interaction as `min(max_energy, stored_energy + effective_regen_rate × tick_delta)`.
- Energy regen is **constant** — it occurs continuously, including during time-locked actions. There is no idle requirement for energy recovery.

⚠️ **Implementation note (contracts specialist)**: Use saturating arithmetic for lazy evaluation (underflow on negative regen, overflow on large tick deltas). Death from starvation (energy reaches 0 from negative regen) is realized lazily on next interaction.

### Reservation model

All actions follow:

1. **Reserve** energy upfront (energy cannot drop below 0).
2. **Lock** adventurer for `duration_ticks`.
3. On lock expiry: **resolve** action (success/failure/hazard), consume reserved energy, produce outputs.

This prevents double-spending and ensures every action has a committed cost before resolution.

### Food and energy (base module)

- Food quality affects regen rate and max energy via buffs/debuffs.
- Raw/basic food: no bonus (default regen).
- Cooked/crafted food: regen bonus + max energy bonus (values per recipe).
- Neglecting food entirely: regen debuff that can turn regen **negative**.
- **If energy reaches 0 from negative regen on any tick: the adventurer dies.**

This is the core starvation mechanic. It is base-module because it ties energy (physics) to food (resource system) to death (permadeath).

### Energy sinks (base module)

| Action | Energy cost |
|---|---|
| Explore (unknown hex) | High (flat — biome unknown) |
| Travel (known hex) | Low-medium (biome-modified, survey-reduced) |
| Survey (reveal area) | Medium |
| Harvest/Mine/Log/Hunt | Per-action (skill-modified) |
| Establish livestock | Per-area (initial setup cost) |
| Craft/Refine | Per-recipe |
| Construct | Per-building (material-dependent) |
| Territorial upkeep | Per-tick (biome + productivity modified) |
| Claim | Escrow (refunded on failure) |
| Deposit food to reserves | Small (intentional maintenance action) |

---

## 11. Movement & Exploration

### Explore (enter unknown hex)

- Higher energy cost + longer time-lock than travel.
- On completion: hex `discovered = true`, `discoverer` recorded.
- Biome is immediately readable (derived from noise — no reveal needed).
- Areas are **not** yet revealed (requires separate survey action).
- Hazard check on resolution (biome-specific beast encounter chance).

### Travel (enter known hex)

- Lower energy cost. Duration scales with biome difficulty.
- Fully surveyed hexes reduce travel cost further (infrastructure bonus).
- Hazard check on resolution (lower chance than explore).

### Movement constraints

- Adjacent hexes only. No skipping.
- Cannot move while activity-locked.
- Cannot move if inventory weight exceeds carry capacity.
- Water biomes (Ocean, Coastal Waters) require specific traversal conditions (TBD — may require a boat building or mount type; at minimum, higher energy cost + hazard lethality).

---

## 12. Surveying

### Survey action

- Reveals one **unsurveyed area** within the adventurer's current hex.
- Medium energy cost + time-lock.
- On completion: area type, building slots, and resource nodes are finalized from the discovery seed and stored permanently.
- The **control area** (area_index 0) is always surveyable first. Surveying it makes the adventurer the hex **controller**.

### Why survey is separate from explore

Exploration discovers the hex (biome, existence). Surveying reveals what's inside. This creates a meaningful two-stage discovery process:
- Explorers push the frontier quickly (low-detail knowledge).
- Surveyors invest more time/energy to unlock the hex's full potential.
- A hex can be explored but unsurveyed — known to exist, but its contents are a mystery.

---

## 13. Resource Definitions 🔒

The base module must define **every resource type** at deployment because resource IDs are referenced by crafting recipes, building costs, and world generation tables. Adding new resource types later would require base module changes (which is forbidden).

### Design approach

Define a **generous resource enumeration** that covers the full scope document's needs plus headroom for future modules. Resources that aren't generated by any base-module biome simply don't appear in the wild until a future module introduces a source — but their IDs exist.

### Resource categories

#### Food

Raw food items gathered from fertile areas, forestry, or (future) hunting:

| Resource | Source biomes | Notes |
|---|---|---|
| Wild Berries | Forest, Jungle, Taiga | Common, low nutrition |
| Roots | Grassland, Highlands | Common |
| Mushrooms | Forest, Swamp, Mire | Uncommon, some poisonous |
| Game Fowl | Savanna, Grassland | Hunting yield (§16) |
| Venison | Forest, Taiga, Highlands | Hunting yield (§16) |
| Boar | Forest, Swamp, Jungle | Hunting yield (§16) |
| Milk | Grassland, Highlands | Livestock yield (§18) |
| Eggs | Forest, any temperate | Livestock yield (§18) |
| Meat (generic) | Any with livestock or hunting | Butchered product |
| Honey | Forest, Jungle | Rare fertile node |
| Fish | Coast, Lake, Mangrove, Swamp | Requires adjacent water hex or fishing area |
| Herbs | Multiple biomes | Dual-use: food ingredient + alchemical |
| Grain | Grassland, Scrubland | Crop (sowable on fertile areas) |
| Vegetables | Forest, Highlands | Crop |
| Fruit | Jungle, Woodland, Forest | Tree-based, seasonal yield |

#### Core Resources (the canonical 22)

These map to the Eternum resource taxonomy. They are **not bridged** from Eternum — they are native to this world with the same names for familiarity:

| Tier | Resources |
|---|---|
| **T1 (Common)** | Wood, Stone, Coal, Copper, Obsidian, Silver |
| **T2 (Uncommon)** | Ironwood, Cold Iron, Gold, Hartwood, Diamonds, Sapphire, Ruby |
| **T3 (Rare)** | Deep Crystal, Ignium, Ethereal Silica, True Ice, Twilight Quartz, Alchemical Silver, Adamantine, Mithral, Dragonhide |

- T1: Mineable/harvestable directly as usable resources (no mandatory refinement).
- T2: Mostly require refinement (ore → bar, rough gem → cut gem). Some exceptions (Ironwood, Hartwood usable raw).
- T3: All require specialized refinement processes and higher-tier facilities.

#### Special Resources

Non-core materials that extend the crafting supply chain:

| Resource | Primary source | Notes |
|---|---|---|
| Leather | Forestry (fauna) | Byproduct of ecosystem |
| Bone | Forestry (fauna) | Byproduct |
| Tallow | Forestry (fauna) | Rendered fat; fuel, crafting |
| Salt | Desert, Coast, Mining | Preservation, trade good |
| Clay | Swamp, Coast, Lake | Pottery, construction |
| Sand | Desert, Coast | Glass production |
| Pitch | Forest, Taiga | Waterproofing, fuel |
| Sulfur | Scorched, Mining | Alchemical, future module |
| Saltpeter | Desert, Canyon | Alchemical, future module |
| Dyes | Jungle, Swamp, Mire | Crafting modifier |
| Silkworms | Forest (caves, deep forest) | Textile (Silk precursor) |
| Flax | Grassland | Textile |
| Wool | Grassland, Highlands | Textile; primary source is livestock (§17) |
| Furs | Taiga, Tundra, Forest | Gear crafting (cold protection) |
| Pearls | Coast, Ocean | Rare, jewelry |
| Amber | Forest, Taiga | Rare, jewelry/alchemy |
| Obsidian Shards | Scorched, Mining | Weapon crafting |
| Alchemical Reagents | Multiple (rare nodes) | Reserved for future Alchemy & Essence module |

#### Essence

- Singular magical material. **Resource ID is registered** in the base module, but Essence does **not** drop from any base-module source.
- Essence generation, uses, and alchemy recipes are entirely scoped as a **future module** (Alchemy & Essence).
- The ID exists at deployment so future modules can reference it without requiring base module changes.

### Resource ID registry

All resources above are assigned permanent `u16` IDs at deployment.

For the complete and authoritative resource ID registry, see **Appendix F: Resource Catalog**. The registry below is a summary; Appendix F takes precedence in case of any discrepancy.

```
BLOCK           RANGE             SLOTS   NOTES
─────────────── ───────────────── ─────── ──────────────────────────────────────
Food            0x0000 – 0x00FF     256   Raw food + processed intermediates
Core Resources  0x0100 – 0x0115      22   Immutable — 22 assigned, fixed
 └─ locked      0x0116 – 0x01FF     234   Will not expand (reserved, unused)
Raw Materials   0x0200 – 0x03FF     512   All raw non-food resources
 ├─ Mining      0x0200 – 0x02FF     256
 ├─ Logging     0x0300 – 0x033F      64
 ├─ Farm/Forage 0x0340 – 0x039F      96
 └─ Animal      0x03A0 – 0x03FF      96   Hunting, Herding, Fishing
Refined         0x0400 – 0x05FF     512   All refined & processed resources
 ├─ Metals      0x0400 – 0x043F      64
 ├─ Textiles    0x0440 – 0x04BF     128
 ├─ Wood/Plant  0x04C0 – 0x04FF      64
 ├─ Building    0x0500 – 0x053F      64
 ├─ Fuels/Chem  0x0540 – 0x059F      96
 └─ Proc. Food  0x05A0 – 0x05FF      96
Beast Parts     0x0600 – 0x07FF     512
Alchemy/Essence 0x0800 – 0x0BFF    1024   Future module
Reserved        0x0C00 – 0x0FFF    1024   Future modules
─────────────── ───────────────── ─────── ──────────────────────────────────────
TOTAL           0x0000 – 0x0FFF    4096
```

---

## 14. Production: Mining

### Mining areas

- Found within hexes via surveying. Biome determines ore types and rarity.
- Each mining area has deterministic: **ore types**, **vein count**, **vein depth**, **yield multiplier**.

### Mining action

1. Adventurer commits energy + time-lock to mine a specific area.
2. On resolution: yield = `f(vein_depth, yield_multiplier, Intelligence, tool_bonus)` — INT governs resource yield.
3. **Vein depletion**: each extraction reduces remaining depth. When a vein is exhausted, it yields nothing (but the area is not destroyed).

### Collapse mechanic

- Each mining action has a **collapse chance** = `f(cumulative_extractions, vein_stability, Survival)`.
- Collapse doesn't destroy the area. It **blocks** it for a lengthy clearing process (high energy + time).
- Shoring Rig building (§18) reduces collapse chance.
- Coordinated mining (multiple adventurers in sequence, with pauses) reduces cumulative stress — incentivizing cooperation.

### Yields

- **Usable raw** (no refinement): Stone, Coal, Obsidian.
- **Ores** (require smelting): Copper ore → Copper, Cold Iron ore → Cold Iron, etc.
- **Rough gems** (require cutting): Raw diamonds → Diamonds, etc.
- **Rare materials**: T3 resources from deep veins only.
- **Special resources**: Bone, Salt, Sulfur, etc. as secondary drops.

---

## 15. Production: Forestry

### Forestry areas

- Found within hexes via surveying. Biome determines tree species, flora, and fauna.
- Each forestry area has deterministic: **lumber type**, **native flora**, **native fauna**, **yield multiplier**, **regrowth rate**.

### Logging action

1. Adventurer commits energy + time-lock.
2. Yield = `f(tree_density, yield_multiplier, Intelligence, tool_bonus)` — INT governs resource yield.
3. Logging depletes tree density. Trees regrow over time (regrowth rate).

### Ecosystem dynamics

**Ecosystem sustainability**: Each production ecosystem has a **sustainability threshold** — a maximum extraction rate below which the ecosystem maintains itself indefinitely. Exceeding this threshold triggers depletion effects. Any logging activity depletes local fauna to some degree (habitat disruption). Overhunting reduces future yield. Heavy harvesting reduces soil fertility (counteracted by fertilization actions). Sustainability thresholds are GM-adjustable parameters, allowing the Game Masters to tune the balance between extraction and conservation. Players must make meaningful choices about resource use intensity.

This is a core base-module mechanic because it creates player-driven scarcity:

- **Logging vs. ecosystem trade-off**: as logging increases, lumber yield rises slightly, but flora/fauna yield drops sharply.
- **Heavy logging** can make native flora/fauna **extinct** (0% yield) while improving lumber yield — and **eliminates hunting** in the area (see §15).
- **Conservation choice**: players may preserve rare flora/fauna instead of maximizing wood (and maintain hunting yields).
- Native species can return over time if logging ceases entirely (slow regrowth).

### Yields

- **Usable raw**: Wood, Ironwood, Hartwood.
- **Flora byproducts**: Herbs, berries, mushrooms, dyes, pitch, charcoal, amber.
- **Fauna byproducts**: Leather, bone, tallow, furs.

---

## 16. Production: Hunting

Hunting is an active action performed in **forestry areas**, targeting the area's native fauna.

### Hunt action

1. Adventurer commits energy + time-lock in a forestry area that has living fauna.
2. **Chance-based resolution**: success is a single roll influenced by:
   - Adventurer **Survival** attribute (`hunt_success += SUR × 5%`)
   - Equipment bonuses (bows, traps, hunting gear)
   - Current **fauna density** of the area (higher = better odds)
3. On **success**: yield food (game fowl, venison, boar) and special resources (leather, bone, tallow, furs).
4. On **failure**: nothing — energy and time are still consumed. Negative skill traits may be acquired from repeated failures.

### Interaction with logging

- Fauna density drops sharply as cumulative logging increases (same ecosystem dynamics as §14).
- Heavy logging drives fauna **extinct** → hunting becomes impossible.
- If logging ceases, fauna can recover over time (slow regrowth).

This creates a direct strategic tension: logging and hunting compete for the same ecosystem. Players (or settlements) must choose their balance.

**Ecosystem sustainability**: Each production ecosystem has a **sustainability threshold** — a maximum extraction rate below which the ecosystem maintains itself indefinitely. Exceeding this threshold triggers depletion effects. Any logging activity depletes local fauna to some degree (habitat disruption). Overhunting reduces future yield. Heavy harvesting reduces soil fertility (counteracted by fertilization actions). Sustainability thresholds are GM-adjustable parameters, allowing the Game Masters to tune the balance between extraction and conservation. Players must make meaningful choices about resource use intensity.

### Hazard note

Hunting is a **deliberate action** with chance-based yields. Beast **hazard encounters** (§24) are involuntary and can occur during any action — they are separate systems. A hunter might successfully bag a deer *and* get ambushed by a wolf on the same trip.

---

## 17. Production: Foraging & Harvesting

### Fertile areas

- Found within hexes via surveying. Biome determines native plant species.
- Each fertile area has deterministic: **native species**, **fertility level**, **yield multiplier**, **regrowth rate**.

### Harvest action

1. Adventurer commits energy + time-lock.
2. Yield = `f(fertility, species_yield, Intelligence, tool_bonus)` — INT governs resource yield (`yield = base × (1 + INT × 0.025)`). Survival affects **forage success chance** (`forage_success += SUR × 5%`).
3. Each harvest **reduces fertility**. Lower fertility → lower future yields.

### Fertility management

**Ecosystem sustainability**: Each production ecosystem has a **sustainability threshold** — a maximum extraction rate below which the ecosystem maintains itself indefinitely. Exceeding this threshold triggers depletion effects. Any logging activity depletes local fauna to some degree (habitat disruption). Overhunting reduces future yield. Heavy harvesting reduces soil fertility (counteracted by fertilization actions). Sustainability thresholds are GM-adjustable parameters, allowing the Game Masters to tune the balance between extraction and conservation. Players must make meaningful choices about resource use intensity.

- **Natural regrowth**: fertility slowly recovers if the area is left unharvested.
- **Fertilize action**: adventurer can spend resources (compost, bone meal, etc.) + energy to restore fertility faster.
- **Salt the earth**: destructive action that pushes fertility **negative** (barren). Negative fertility trends back toward 0 over time, but slowly.
- **Sowing crops**: native species can be driven extinct to plant crops (Grain, Vegetables). Crops have different yield profiles than wild plants. Crop yield depends on biome affinity.

### Yields

- **Food**: Wild berries, roots, mushrooms, herbs, honey, grain, vegetables, fruit.
- **Special resources**: Herbs (dual-use), dyes, flax, alchemical reagents.

---

## 18. Production: Livestock

Livestock raising is an alternative land use for **fertile areas** where native species have been driven extinct (same precondition as sowing crops in §16).

### Establishing livestock

1. Clear a fertile area of native species (through heavy harvesting or salt-the-earth).
2. Choose land use: **sow crops** OR **raise livestock** (mutually exclusive per area).
3. Raise livestock requires:
   - Initial stock (animals acquired from hunting yields, trade, or settlement sources)
   - Energy + time-lock to establish the herd

### Livestock state

```
livestock_type: u8           // species (cattle, sheep, goats, poultry, etc.)
herd_size: u16
area_fertility: i16          // grazing depletes fertility over time
```

### Yields

| Livestock type | Food yields | Special resource yields | Biome affinity |
|---|---|---|---|
| Cattle | Meat, milk | Leather, bone, tallow | Savanna, Grassland |
| Sheep | Meat, milk | Wool, leather | Grassland, Highlands |
| Goats | Meat, milk | Leather, bone | Highlands, Scrubland, Canyon |
| Poultry | Meat, eggs | Bone, feathers | Forest, any temperate |

### Upkeep

- Livestock graze, reducing area **fertility** over time (same fertility mechanic as crops).
- If fertility drops too low, herd size declines (animals starve/leave).
- Players must **fertilize** or rotate grazing to sustain herds long-term.
- Neglected livestock decline to 0 — the area reverts to barren (can be re-seeded with native species, crops, or new livestock).

### Why livestock is in the base module

Animal products (wool, leather, tallow, meat, milk, eggs) are **core crafting inputs** and **food sources** that feed into the base crafting and settlement food systems. Without livestock, wool has no reliable source, and the food/special resource supply chain has a gap that would require a base module change to fill later — which is forbidden.

---

## 19. Production: Fishing

Fishing harvests aquatic resources from water-adjacent and water biomes. It follows a similar pattern to Hunting — chance-based resolution in appropriate areas.

### Where to fish

- **Coast, Lake, Mangrove** biomes support fishing directly.
- Hexes adjacent to water biomes may support limited fishing (river-fed).
- Fishing requires no building — it is a standalone action using natural water access.

### Fishing action

- **Energy cost**: Medium (15–30).
- **Time-lock**: Medium (36–108 ticks / 6–18 min).
- **Resolution**: Chance-based, influenced by SUR, equipment (nets, rods), and aquatic fauna density.
- **Yields**: Fish (primary food resource), plus chance of special catches (pearls, shells, rare aquatic materials — defined in Appendix F).
- **Biome modifiers**: Coast and Mangrove yield saltwater species; Lake yields freshwater species.

### Aquatic ecosystem dynamics

- Aquatic fauna density tracks independently from terrestrial fauna.
- **Overfishing** depletes aquatic fauna density, reducing yield and catch rate.
- Density recovers naturally over time (regrowth rate is a GM-adjustable parameter).
- **Sustainability threshold**: Below a defined extraction rate, aquatic ecosystems maintain themselves indefinitely. Exceeding the threshold triggers depletion. Threshold values are GM-adjustable.

> ⚠️ Exact yield values, species tables, and density curves to be quantified in Phase 6f.

---

## 20. Crafting & Refining

### Refining (raw → usable)

Most extracted materials require processing before use:

| Process | Input → Output | Facility required |
|---|---|---|
| Smelting | Ore → Bar (Copper, Cold Iron, Gold, Silver, etc.) | Smelter |
| Gem cutting | Rough gem → Cut gem (Diamonds, Sapphire, Ruby) | Workshop |
| Tanning | Raw hide → Leather | Workshop |
| Sawmilling | Wood/Ironwood/Hartwood/Moonwood → Planks | Sawmill |

- Refinery yield depends on **building tier** (higher tier = less waste) and **adventurer Intelligence** (higher INT = bonus yield via `yield = base × (1 + INT × 0.025)`).
- Quality tiers: dirty / clean / pure. Higher-tier facilities + higher skill produce cleaner output.

### Mutation-based crafting

```
craft(inputs[]) → new_item
new_properties = f(input_properties, craft_seed)
```

- Any compatible resources can be combined. Outputs inherit blended properties from inputs.
- A **mutation function** (XOR-seeded) introduces controlled randomness — crafted items are unique.
- **Legendary rolls** occur when property products cross defined thresholds.
- Higher **Craftsmanship** improves mutation outcomes (wider favorable distribution via `craft_quality += CRA × 5%` and expanded mutation range).

⚠️ **Security requirement**: `craft_seed` MUST include **reveal-block hash** entropy to prevent players from predicting mutation outcomes. Without block entropy, the crafting system becomes a solved game. **Flag for contracts specialist.**

### Crafting facilities (base module)

| Facility | Produces | Key inputs |
|---|---|---|
| **Campfire** | Basic cooked food (raw → edible) | Food + Wood (fuel) |
| **Cookhouse** | Quality food consumables (energy buffs) | Food + herbs + fuel |
| **Smelter** | Bars, alloys | Ores + fuel |
| **Workshop** | Cut gems, leather, basic tools, repairs | Various raw materials |
| **Forge** | Weapons, armor | Bars, alloys, leather, wood |

The base module includes these five. Additional crafting facilities (Jeweler, Alchemist, etc.) are future modules that use the same crafting interface.

### Recipes

Recipes are defined as:

```
recipe_id: u16
inputs: [(resource_id, quantity)]
facility_required: BuildingType
min_craftsmanship: u8
output_type: ItemType
base_properties: PropertySet
mutation_range: PropertySet   // bounds for random mutation
```

The base module ships with a **starter recipe set** covering:
- Food cooking (raw → cooked, herbs → potions-lite)
- Ore smelting and gem cutting
- Basic weapons (sword, axe, bow, spear)
- Basic armor (leather set, chain set)
- Basic tools (pickaxe, hatchet, sickle, hammer)
- Building materials (planks, bricks, nails, rope)

The recipe registry includes **reserved ID ranges** for future modules (same pattern as resource IDs).

---

## 21. Equipment & Items

### Item model

Every item (crafted, found, or purchased) has:

```
item_id: felt252          // unique
item_type: u16            // recipe/template ID
properties: PropertySet   // mutation-derived stats
durability: u16           // current / max
weight: u16
greatness: u8             // 1–20 rating scale (Loot-compatible)
```

### Durability

- All gear degrades with use. Rate depends on action type and intensity.
- At 0 durability: item is **inert** (no stat bonuses, cannot be used).
- **Repair action**: costs resources + energy at a Workshop or Forge. Restores durability.
- Durability creates ongoing resource demand — the primary item sink.

### Greatness scale

- **1** (crude) to **20** (legendary).
- Crafting produces items in a range determined by inputs + skill + mutation.
- The Greatness scale is the universal item rating — future Loot integration maps directly to it.

### Destruction on death

- In the base module, **all inventory and equipment is destroyed on adventurer death**.
- No item persistence, no drops, no looting corpses. This is the simplest clean model.
- Future modules (PvP, Loot Vaults) can add item recovery/persistence mechanics by interfacing with the death event hook.

---

## 22. Construction & Buildings

### Building system

- Buildings are placed in **building slots** within areas.
- Each area has a fixed number of slots (determined at survey time by area rarity).
- Building placement costs resources + energy + time-lock.

### Building state

```
building_id: felt252
building_type: BuildingType
area_index: u8
slot_index: u8
hex: (x, y, z)
condition: u16            // 0–10000 (basis points), degrades without upkeep
required_population: u8   // staffing requirement
active: bool              // false if condition < threshold or understaffed
```

### Building upkeep

- Each building has an ongoing resource + energy cost per in-game day.
- If upkeep is not paid, `condition` degrades.
- At `condition < 2500` (25%): building is **inactive** (no production, no bonuses).
- Repair restores condition (costs resources + energy, proportional to damage).

### Base module buildings

| Building | Area type | Function | Staffing req |
|---|---|---|---|
| **Keep** | Control | Settlement admin; required for deed minting | TBD |
| **Housing** | Bare / Control | Increases max population | 0 (passive) |
| **Storehouse** | Bare | Increases hex storage capacity | TBD |
| **Smelter** | Bare / Materials | Ore → bar refining | TBD |
| **Workshop** | Bare | Gem cutting, leather tanning, repairs, basic crafting | TBD |
| **Forge** | Bare | Weapons + armor crafting | TBD |
| **Cookhouse** | Bare | Food preparation (cooked food, buffs) | TBD |
| **Shoring Rig** | Materials (mining) | Reduces mine collapse risk in hex | TBD |
| **Greenhouse** | Materials (fertile) | Improves plant regrowth rate | TBD |
| **Watchtower** | Control / Bare | Defense bonus in claim disputes | TBD |
| **Lumber Mill** | Materials (forestry) | Improves logging yield + regrowth | TBD |
| **Campfire** | Any | Basic cooking (no building slot; placed freely) | 0 |

The Campfire is a **zero-slot building** — it can be placed without occupying a building slot but has limited recipes and no condition/upkeep (it's temporary).

⚠️ **Flag (Phase 9b)**: Campfire lifetime is TBD. Define explicit lifetime/decay rules in Phase 9b.

### Building type registry

Same pattern as resources: permanent IDs at deployment with reserved ranges for future modules.

---

## 23. Settlement

### Formation (Deed system)

1. Adventurer develops **two or more hexes** to a threshold (TBD — likely: Keep built + minimum buildings).
2. The control area of one hex must have a **Keep** building.
3. The Keep allows minting a **Deed of Settlement** (energy cost).
4. The Deed is **non-transferable** and **bound to the adventurer**. Destroyed on adventurer death.
   - **Ownership note**: Ownership of settlements, hexes, and buildings is tied to the **wallet address**, not individual adventurers.
   - Deeds of Settlement are items used to link hexes into a settlement, not ownership tickets.
   - Destroying a deed removes the ability to link another hex, but does not end ownership.
   - Ownership persists with the wallet even after adventurer death.
5. Carry the Deed to an adjacent owned hex's Keep → **apply** the Deed:
   - No settlement exists → **create** one.
   - Settlement exists → **add** the hex to it.

### Settlement state

```
settlement_id: felt252
owner: ContractAddress        // the adventurer
hex_ids: [felt252]            // member hexes
population: u32
food_reserves: u16            // 0–10000 (basis points, i.e., 0–100.00%)
last_food_tick: felt252
```

### Constraints

- Max **7 hexes** before exponential maintenance scaling kicks in.
- Hexes must be **adjacent** to at least one other settlement hex (contiguous cluster).
- If a hex is captured by another player, it leaves the settlement automatically.

### Settlement benefits

Settlements provide strategic advantages beyond simple hex clustering:

| Benefit | Description | Default Value | Adjustable |
|---|---|---|---|
| **Shared storage** | Resources deposited in any settlement hex are accessible from any other hex in the settlement | Full access | GM-adjustable (could restrict to adjacent hexes) |
| **Building synergies** | Complementary buildings in the same settlement grant production bonuses (e.g., Smelter + Forge = +10% crafting quality) | +10% per synergy pair | GM-adjustable |
| **Defensive bonus** | Reduced base hazard chance within settlement borders | −25% hazard chance | GM-adjustable |
| **Transport efficiency** | 25% fewer donkeys needed for inter-hex transport within settlement | −25% | GM-adjustable |
| **Travel discount** | Cheaper travel between settlement hexes for the owner | −20% travel cost | GM-adjustable |
| **Trade hub** | Settlements can serve as trade hubs where market listings are more visible (future module hook — interface declared here) | Interface only | Future module |

> Building synergy pairs to be defined in Phase 9. Trade hub interface to be specified in §29 (Extension Interfaces).

---

## 24. Population, Housing & Food

### Population

Each settlement has a **Population** value — the number of non-adventurer inhabitants (workers, maintainers, support staff) who keep buildings functional. Think of them loosely as the settlement's serfs: bound to the land, providing labor and upkeep in return for protection.

Population is constrained by **(1) housing capacity** and **(2) food reserves**.

### Housing capacity

- `base_max_pop = Σ(housing_buildings × capacity_per_housing)` — capacity values are TBD per building type.
- Population cannot exceed `base_max_pop` (Realm metadata bonuses that modify this cap are a future module).

### Food reserves

- **Capped at 100%** (0–10000 basis points internally).
- **Deposit-only**: no automatic routing from storage. An adventurer must manually deposit food from settlement storage into Food Reserves (costs energy).
- Any food type is valid for deposit.

**Drain rate:**
- 2.5% per in-game day (= 250 basis points).
- Full bar → empty in **40 in-game days** (~6.7 real days).

**Scaling:**
- Food required to fill the bar scales **proportionally with `base_max_pop`**.
- Bigger settlements = more food to maintain.

### Population growth/decline

Per in-game day boundary:
- Food Reserves **at zero** → Population −1.
- Food Reserves **greater than zero** → Population +1, up to `base_max_pop`.

### Staffing and building functionality

- Each building has a **Required Population** (staffing expectation).
- `staffing_ratio = clamp(CurrentPop / RequiredPop, 0, 1)`
- Population, food, and upkeep determine whether buildings remain **functional / open for use** (cleaning, preparation, utilities, repair). Adventurers still spend **energy** to perform actual production at buildings.

**Understaffing penalties:**
- **Production efficiency** scales linearly with `staffing_ratio`.
- **Upkeep multiplier** = `if staffing_ratio == 0 then 3.0 else clamp(1 / staffing_ratio, 1, 3)`:
  - `staffing_ratio = 1.0` → `1×` upkeep (normal)
  - `staffing_ratio = 0.5` → `2×` upkeep (100% extra)
  - `staffing_ratio ≤ 0.25` → `3×` upkeep (200% extra, hard cap)

**Note**: All buildings require adventurer interaction to produce — nothing is generated automatically. At zero population, buildings cannot be used and incur maximum (3×) upkeep penalty.

---

## 25. Territorial Sovereignty

### Ownership

- First adventurer to survey a hex's control area (area_index 0) becomes its **controller**.
- Controller sets access permissions via **programmable hooks** (smart contract interface).
- All buildings and areas within a hex are governed by the controller.

### Decay

Every owned hex has a passive upkeep cost (energy + resources per in-game day):

- **Base cost**: biome-dependent (plains cheap, volcanic expensive).
- **Productivity tax**: more active buildings = higher upkeep.
- **Building complexity**: more buildings = higher coordination cost.

If upkeep is unpaid, `decay_level` increases:

| Decay level | Effect |
|---|---|
| 0–20% | Minor efficiency penalties |
| 21–50% | Slower operations, some buildings go inactive |
| 51–80% | Most buildings disabled, infrastructure failing |
| 80–100% | **Territory becomes claimable** |

### Claim/defend

When a hex reaches 80%+ decay:

1. Any adventurer can **initiate a claim** by locking energy into escrow.
2. Original controller has a **grace period** (500 blocks) to defend by investing energy.
3. If undefended → claimant takes control. All area ownership transfers.
4. Claim escrow consumed on success; refunded on failure/expiry.
5. Watchtower building increases the grace period and/or defense bonus.

### Natural limits

This creates organic territorial limits: you can only hold what you can maintain. Empires require sustained economic output, not one-time conquest.

---

## 26. Beasts & Hazards

### Why beasts are in the base module

Hazards are part of the physics — they make exploration dangerous, create resource sinks (energy, health), and are the primary source of permadeath outside starvation. Without them, the world has no teeth.

### Beast types

The base module uses the **Loot Survivor beast taxonomy** (75 beast types across 5 tiers). These are unnamed/wild beasts — they are environmental hazards, not player-owned.

| Tier | Examples | Biomes |
|---|---|---|
| T1 (Common) | Wolf, Rat, Spider, Snake | Most temperate/wet |
| T2 (Uncommon) | Bear, Boar, Giant Spider, Scorpion | Forest, Desert, Swamp |
| T3 (Rare) | Yeti, Wyvern, Basilisk | Tundra, Highlands, Scorched |
| T4 (Epic) | Dragon, Kraken, Behemoth | Ocean, Scorched, Underworld |
| T5 (Legendary) | Phoenix, Leviathan, Titan | Extreme biomes, Underworld |

Each biome has a **hazard table**: weighted probability distribution of beast types that can appear.

### Encounter resolution

Hazards trigger as a **chance per action** (explore, travel, mine, log, harvest, survey):

1. `encounter_chance = base_chance × biome_modifier × action_risk_modifier`
2. If triggered: `beast_type` drawn from biome hazard table.
3. **Resolution equation** (single roll, not a combat system):
   - Inputs: beast lethality, adventurer health, adventurer energy, attributes (Survival `+2.5%` success per point, Dexterity `+5%` avoidance per point), equipment defense, time of day
   - Outcomes: **avoid** (no effect), **injury** (lose health, possibly gain an injury trait), **resource gain** (pelt, bone, meat), **positive skill trait** (Beast Slayer, Survivor), or **death**.
   - Health loss from encounters varies by beast tier and outcome severity.
   - Injury traits (Lacerated, Fractured, Bruised, etc.) are gained from damage outcomes and impose penalties until resolved.
4. Higher-tier beasts have higher lethality → more likely to injure/kill.

### This is NOT a combat system

The base module does not include turn-based or real-time combat. Encounters are **single-resolution hazard checks**. A full combat module (PvP, dungeon crawling, tactical beast fights) is deferred. The hazard system provides:
- Risk that makes exploration meaningful.
- A death mechanic beyond starvation.
- Resource drops (leather, bone, tallow, furs, meat) that feed the crafting economy.
- Trait acquisition through survival.

---

## 27. Permadeath & Legacy

### Death triggers

An adventurer dies when:
- **Health reaches 0** from encounter damage, activity injuries, or accumulated wounds.
- **Energy reaches 0** from **negative regen** (starvation).
- A **hazard encounter** resolves as instant death (low-probability, higher when health is critical).
- (Future module) PvP combat kills them.

### On death

1. Adventurer `alive = false`. Permanent. No resurrection.
2. All **inventory and equipment destroyed** (base module — no drops).
3. All **energy reservations/escrows** settled and released.
4. Settlement **Deeds** bound to this adventurer are destroyed (settlement persists but new deeds can't be issued by this adventurer).
5. Hex ownership **persists** but begins decaying without active upkeep from another adventurer.
   - **Ownership note**: Ownership of settlements, hexes, and buildings is tied to the **wallet address**, not individual adventurers. Ownership persists with the wallet even after adventurer death.
6. Followers are released (disappear).
7. **Legacy record** written: discoverer tags on hexes persist forever, buildings persist, settlement structures persist.

### Respawn

Player mints a new adventurer (paying $LORDS again). Starts fresh at The Nexus. The world remembers the old adventurer; the new one starts with nothing.

---

## 28. Autoregulation

### Purpose

Prevent runaway inflation, stagnation, or exploitation without human intervention. The autoregulator is a **permissionless on-chain PI controller** that adjusts tunable parameters within pre-defined bounds.

### Tunable parameters (base module)

| Parameter | Bounds | What it affects |
|---|---|---|
| Adventurer mint cost ($LORDS) | [min, max] | Spawn rate |
| Energy regen rate | [0.05, 0.2] per tick | Economic velocity |
| Health regen rate | [0.005, 0.02] per tick | Recovery speed |
| Hazard lethality multiplier | [0.5, 2.0] | Death rate / health damage severity |
| Collapse chance multiplier | [0.5, 2.0] | Mining coordination pressure |
| Decay rate multiplier | [0.5, 2.0] | Territorial holding cost |
| Fertility regrowth rate | [0.5, 2.0] | Food scarcity |
| Attribute gain base chance | [0.5, 2.0] multiplier | Progression speed (applied to per-action base chances) |
| Trait gain base chance | [0.5, 2.0] multiplier | Trait acquisition frequency (applied to per-event base chances) |
| Injury→disability conversion rate | [0.5, 2.0] | Permanent scar frequency |
| Follower desertion rate | [0.5, 2.0] | Party maintenance pressure |

⚠️ **Implementation note (contracts specialist)**: Autoregulator metrics must use lazy aggregation with running counters (O(1) reads per epoch), not iteration over all adventurers/hexes.

### Mechanism

- Evaluates once per **epoch** (e.g., every 100 in-game days).
- Reads aggregate metrics: total population, active adventurers, resource production rates, death rates, average energy balance.
- Applies PI corrections: if death rate is too low → increase hazard lethality; if resources are hyperinflating → increase decay rates; etc.
- All adjustments are bounded — the autoregulator cannot exceed the defined parameter ranges.
- No admin keys. No governance votes needed. Pure algorithmic response.

---

## 29. Extension Interfaces

The base module exposes these interfaces for future modules to hook into:

### Event hooks

```
on_adventurer_created(adventurer_id)
on_adventurer_died(adventurer_id, cause, position)
on_hex_discovered(hex, discoverer)
on_area_surveyed(hex, area_index, surveyor)
on_resource_produced(hex, area, resource_id, quantity, producer)
on_item_crafted(item_id, recipe_id, crafter)
on_building_placed(hex, area, building_type, builder)
on_building_destroyed(hex, area, building_type)
on_territory_claimed(hex, old_controller, new_controller)
on_territory_decayed(hex, decay_level)
on_settlement_created(settlement_id, founder)
on_settlement_hex_added(settlement_id, hex)
on_settlement_hex_removed(settlement_id, hex)
on_encounter_resolved(adventurer_id, beast_type, outcome)
```

Future modules subscribe to these events and react accordingly (e.g., a combat module listens to `on_encounter_resolved` to add tactical combat before resolution; a trade module listens to `on_item_crafted` to add marketplace listings).

### Permission hooks

⚠️ **Implementation note (contracts specialist)**: Permission hook calls MUST be gas-bounded. If the hook exceeds the gas limit, the base module should default to **allow** (fail-open) to prevent griefing via intentionally expensive hooks.

Territory controllers can deploy custom smart contracts that implement:

```
trait IPermissionHook {
    fn can_enter(adventurer_id, hex) -> bool;
    fn can_survey(adventurer_id, hex, area) -> bool;
    fn can_harvest(adventurer_id, hex, area) -> bool;
    fn can_build(adventurer_id, hex, area) -> bool;
    fn get_fee(action_type, adventurer_id) -> u256;
}
```

This enables arbitrary access control, tolling, guild gating, profit sharing — all without changing the base module.

### Resource interface

```
trait IResourceRegistry {
    fn get_resource(id: u16) -> ResourceDef;
    fn is_valid(id: u16) -> bool;
    fn get_category(id: u16) -> ResourceCategory;
}
```

Future modules can read resource definitions but cannot modify them.

### Adventurer interface

```
trait IAdventurer {
    fn get_state(id: felt252) -> AdventurerState;
    fn get_attributes(id: felt252) -> [u8; 10];
    fn get_position(id: felt252) -> (felt252, felt252, felt252);
    fn is_alive(id: felt252) -> bool;
    fn get_energy(id: felt252) -> u32;
}
```

Read-only access to adventurer state for any module.

---

## 30. Future Module Placeholders

The base module includes reserved **hooks, IDs, and world-generation slots** that exist solely to support future modules without requiring base module changes. These are "empty sockets" in the physics.

### Reserved resource IDs

```
0x0300–0x03FF: Essence & magical materials (no base-module source; future Alchemy module)
0x0400–0x0FFF: Unallocated (future modules register new resources here)
```

Essence (ID `0x0300`) is registered but has no drop table, recipe, or use in the base module.

### Reserved building type IDs

```
0x0100–0x01FF: Base module buildings
0x0200–0x0FFF: Future module buildings (Jeweler, Alchemist, Barracks, Market Stall, etc.)
```

Future buildings use the same slot/area/construction system — they just don't exist yet.

### Reserved area types

The base module defines 5 area types: `Control, Materials, Bare, Underworld, Special`.

- **Underworld areas**: generated by world gen but have **no base-module actions** beyond surveying. They exist as hooks for the future Dungeon (C&C) module. Adventurers can survey them (revealing that an underworld area exists) but cannot delve or interact further.
- **Special areas**: generated rarely. No base-module actions beyond surveying. Reserved for future modules (Spawn Nodes, shrines, portals, lore sites, etc.).

### Reserved trait slots

The trait system supports up to 10 slots per adventurer. The base module ships with a starter trait list. Future modules can add new traits to the enumeration — the trait storage is generic (`TraitId` is a `u16`).

### Reserved recipe IDs

```
0x0000–0x00FF: Base module recipes (food, smelting, basic gear, building materials)
0x0100–0x0FFF: Future module recipes (alchemy, advanced crafting, enchanting, etc.)
```

### Event hooks as placeholders

All 16+ event hooks (§28) fire regardless of whether any module is listening. Future modules subscribe to the events they need. The base module doesn't need to know what modules exist — it just emits events.

**Seasons & Weather**: The base module includes a **season counter** — 4 seasons per in-game year (1 season = ~90 in-game days). The counter is purely cosmetic in the base module and has no mechanical effect. Future modules may use the season counter to modify production yields, encounter frequencies, movement costs, and other seasonal effects. Weather systems (precipitation, temperature variation, storms) are fully deferred.

### Permission hook interface

The `IPermissionHook` trait (§28) is deployed per-hex by territory controllers. The base module enforces the interface but doesn't care what logic is inside. This is the primary extensibility mechanism for future social/economic/governance modules.

### Biome-specific reserved data

Each biome's hazard table includes **weight slots for T4 and T5 beasts** even though these encounters are exceptionally rare in the base module. Future combat/dungeon modules can adjust encounter weights (via the autoregulator) to make high-tier beasts more common in specific contexts without changing the base hazard resolution logic.

---

## 31. What Is Explicitly Deferred

To maintain clarity on the module boundary, here is what the base module **does not include** and why:

| Feature | Reason for deferral |
|---|---|
| **City / Kingdom / Empire upgrades** | Settlement meta needs to be proven in practice first |
| **Realm NFT bonuses** (city metadata, resource areas) | Ecosystem integration is a separate module |
| **Loot item persistence / Loot Vaults** | Requires item recovery mechanics + Loot contract integration |
| **Genesis Adventurer special rules** | Requires Loot integration |
| **PvP combat** | Separate combat module; base provides death/health primitives |
| **Turn-based / tactical beast combat** | Base uses single-roll hazard resolution |
| **Trade markets** | Social/economic layer; base supports direct adventurer-to-adventurer transfer only |
| **Guilds / governance** | Social layer |
| **Quests / narrative / bounties** | Content layer |
| **C&C dungeon placement** | Requires combat module + C&C contract integration |
| **$SURVIVOR token** | Requires dungeon/bounty loop |
| **Secondary Spawn Nodes** | Can be added as module reading base hex state |
| **Fog of war** | Requires Starknet privacy primitives |
| **Agent/AI autonomous play hooks** | Separate autonomy module |

| **Boats / water traversal buildings** | Water biomes exist; traversal rules are base but specific vehicle buildings are deferred |
| **Essence & Alchemy** | Essence resource ID exists; generation mechanics, alchemy recipes, and magical crafting are a future module |

---

## 32. Playable State

When the base module ships, a player can:

1. **Mint** an adventurer at The Nexus (pay $LORDS).
2. **Explore** outward, discovering new hexes, encountering beasts, risking death.
3. **Survey** hexes to reveal areas and resource nodes.
4. **Harvest** food and plants from fertile areas; manage fertility.
5. **Mine** ores from mining areas; manage collapse risk.
6. **Log** trees from forestry areas; balance lumber vs ecosystem.
7. **Hunt** fauna in forestry areas for food and special resources (leather, bone, furs).
8. **Raise livestock** on cleared fertile areas for wool, meat, milk, eggs.
9. **Refine** raw materials into usable resources (smelt ore, cut gems, tan leather).
10. **Craft** weapons, armor, tools, and food at appropriate facilities.
11. **Build** structures on surveyed area slots (smelters, workshops, forges, housing, etc.).
12. **Settle** — form a settlement across multiple developed hexes using the deed system.
13. **Feed** the settlement population by depositing food into reserves.
14. **Manage** staffing across buildings; deal with understaffing penalties.
15. **Maintain** territory by paying upkeep; watch it decay if neglected.
16. **Claim** decayed territory from inactive players.
17. **Die** permanently from starvation, health depletion, or beast encounters.
18. **Respawn** as a new adventurer in a world shaped by predecessors.

This is a complete, playable survival-settlement game. It is sparse but not shallow — the interaction between energy, resources, territory, population, food, beasts, and permadeath creates genuine strategic depth and meaningful decisions.

Future modules add richness (combat, trade, governance, lore, ecosystem assets) — but the world works without them.

---

*v0.1.1 — March 2026*
*Authors: Krumpy (design direction), Squire (synthesis & drafting)*
*Scope derived from: Eternal Game Design Scope v0.1.5*
