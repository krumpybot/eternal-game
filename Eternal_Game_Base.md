# Eternal Game — Base Module Scope v0.1.1

> **The base module defines the physics of the world.** Once deployed, these contracts never change. Every future module — combat, trade, governance, lore — must build on top of this layer without modifying it. This document specifies exactly what ships in the base module, what is explicitly deferred, and the interfaces future modules will use.

---

## Table of Contents

1. [Design Intent](#1-design-intent)
2. [What "Immutable" Means](#2-what-immutable-means)
3. [Module Boundary](#3-module-boundary)
4. [Time](#4-time)
5. [Space](#5-space)
6. [World Generation](#6-world-generation)
7. [The Adventurer](#7-the-adventurer)
8. [Followers](#8-followers)
9. [Energy](#9-energy)
10. [Movement & Exploration](#10-movement--exploration)
11. [Surveying](#11-surveying)
12. [Resource Definitions](#12-resource-definitions)
13. [Production: Mining](#13-production-mining)
14. [Production: Forestry](#14-production-forestry)
15. [Production: Hunting](#15-production-hunting)
16. [Production: Foraging & Harvesting](#16-production-foraging--harvesting)
17. [Production: Livestock](#17-production-livestock)
18. [Crafting & Refining](#18-crafting--refining)
19. [Equipment & Items](#19-equipment--items)
20. [Construction & Buildings](#20-construction--buildings)
21. [Settlement](#21-settlement)
22. [Population, Housing & Food](#22-population-housing--food)
23. [Territorial Sovereignty](#23-territorial-sovereignty)
24. [Beasts & Hazards](#24-beasts--hazards)
25. [Permadeath & Legacy](#25-permadeath--legacy)
26. [Autoregulation](#26-autoregulation)
27. [Extension Interfaces](#27-extension-interfaces)
28. [Future Module Placeholders](#28-future-module-placeholders)
29. [What Is Explicitly Deferred](#29-what-is-explicitly-deferred)
30. [Playable State](#30-playable-state)

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
| Adventurer | Creation, attributes, traits, equipment slots, inventory, death |
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
| PvP combat | Separate combat module; base provides health/death primitives |
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

## 4. Time

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

## 5. Space

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

## 6. World Generation

### Biome determination

- `biome = noise_function(global_seed, x, y, z)` → one of 25 biome types.
- Biomes are **deterministic and permanent**. No biome ever changes.

### Biome list (base module)

The full biome set must be defined at deployment because biome-specific logic (movement costs, hazard tables, resource affinities) is baked into the base module:

| Category | Biomes |
|---|---|
| **Temperate** | Plains, Grassland, Forest, Highlands |
| **Arid** | Desert, Savanna, Steppe, Badlands, Canyon |
| **Wet** | Swamp, Wetlands, Mire, Jungle |
| **Cold** | Tundra, Taiga, Glacier |
| **Extreme** | Volcanic, Oasis |
| **Water** | Coast, Beach, Coastal Waters, Lake, Ocean, Deep Ocean |

Each biome defines:
- Movement energy cost modifier
- Exploration time modifier
- Hazard table (beast types + lethality curve)
- Native resource affinities (which plants/ores/trees appear)
- Decay rate modifier (base upkeep cost)

### Area generation

On first survey of a hex:

1. `discovery_seed = hash(global_seed, x, y, z, discoverer_salt)` (commit-reveal)
2. `area_count` drawn from rarity-weighted distribution (3 common → 9 near-impossible)
3. For each area: type, slot count, and resource nodes derived from `hash(discovery_seed, area_index)` with domain separation (`AREA_V1`)
4. Results are **stored permanently**. No re-rolls.

### Resource node seeding

Within materials areas, resource nodes (plants, ores, trees) are seeded deterministically:

- `node_seed = hash(discovery_seed, area_index, node_index)` with domain separation (`PLANT_V1`, `ORE_V1`, `TREE_V1`)
- Species, yield multiplier, regrowth rate, and special properties derived from seed + biome affinity table.

---

## 7. The Adventurer

### Creation

- Costs $LORDS (amount is an autoregulator-tunable parameter with defined bounds).
- Spawns at The Nexus (origin hex). Secondary spawn nodes are a future module.
- Mint uses commit-reveal: user commits `hash(salt)`, then reveals salt to finalize.
- `adventurer_seed = hash(global_seed, token_id, mint_block, user_salt)`

### Core state

```
alive: bool
position: (x, y, z)
energy: u32
max_energy: u32
activity_lock_until: felt252   // tick when current action completes
inventory: Map<ItemId, u32>    // weight-limited
equipment: Map<Slot, ItemId>   // head, body, waist, hands, feet, neck, ring
attributes: [u8; 9]           // STR, END, DEX, VIT, INT, WIS, CHA, SUR, CRA, LEA
trait_slots: [TraitId; 6]     // max 6 traits
seed: felt252
```

### Attributes

| Category | Attributes |
|---|---|
| **Physical** | Strength, Endurance, Dexterity, Vitality |
| **Mental** | Intelligence, Wisdom, Charisma |
| **Specialized** | Survival, Craftsmanship, Leadership |

- **10 attributes**, budget of **20 points** at mint (20 rolls → +1 to random attribute each).
- Computed lazily from seed (no stored array at mint).
- Attributes improve through use (learning by doing). Gain rates are autoregulator-tunable.

### Traits

- **2 traits at mint**, derived from seed.
- Traits are organized into **exclusivity groups** (e.g., Brave vs Craven).
- Max **6 trait slots**. New traits replace conflicting ones.
- Traits grant positive/negative attribute modifiers and can unlock/block certain actions.
- Trait list is a large enumeration defined at deployment (see §25 for extensibility).

### Equipment slots

`head, body, waist, hands, feet, neck, ring` — 7 slots total.

### Inventory

- Weight-limited. Base carry capacity from Strength + gear bonuses.
- Items have weight. Exceeding capacity prevents movement (not item pickup — you can drop to move).

---

## 8. Followers

Followers are non-adventurer NPCs attached to an adventurer's party. They are the adventurer's personal logistics and support — laborers, donkeys/mounts, and (future module) bodyguards. The term is loosely analogous to medieval serfs: bound to their adventurer's service, providing labor in return for protection.

### Base module scope

```
follower_count: u8           // current party size
max_followers: u8            // derived from Leadership attribute
follower_types: Map<FollowerType, u8>  // laborers, mounts
```

### Types (base module)

| Type | Effect |
|---|---|
| **Laborer** | Improves harvest/build task throughput for adventurer actions |
| **Donkey/Mount** | Increases carry capacity and/or reduces travel energy cost |

### Gating

- Maximum party size = `f(Leadership)`. Exact curve is autoregulator-tunable within bounds.

### Food consumption

- Followers consume food as the adventurer does (shared food resource type).
- Each follower adds to the adventurer's food requirement. Exact quantities per follower type are TBD (see §12).
- If the adventurer cannot feed followers, they desert (follower count decreases). Desertion rate is autoregulator-tunable.

### What followers are NOT

- Followers do **not** count toward settlement population.
- Followers do **not** staff buildings.
- Followers are tracked at the adventurer level, not the hex/settlement level.

---

## 9. Energy

### Pool

- **Base max energy**: 200 (modified by Endurance + gear).
- **Base regen**: 0.1 energy per tick (~36/hour, ~864/in-game day).
- Regen is **lazy**: computed on next interaction as `min(max_energy, stored_energy + regen_rate × tick_delta)`.
- Regen occurs only when **not** in a time-locked action (idle = resting).

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
| Explore (unknown hex) | High (biome-modified) |
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

## 10. Movement & Exploration

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
- Water biomes (Ocean, Deep Ocean, Coastal Waters) require specific traversal conditions (TBD — may require a boat building or mount type; at minimum, higher energy cost + hazard lethality).

---

## 11. Surveying

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

## 12. Resource Definitions

The base module must define **every resource type** at deployment because resource IDs are referenced by crafting recipes, building costs, and world generation tables. Adding new resource types later would require base module changes (which is forbidden).

### Design approach

Define a **generous resource enumeration** that covers the full scope document's needs plus headroom for future modules. Resources that aren't generated by any base-module biome simply don't appear in the wild until a future module introduces a source — but their IDs exist.

### Resource categories

#### Food

Raw food items gathered from fertile areas, forestry, or (future) hunting:

| Resource | Source biomes | Notes |
|---|---|---|
| Wild Berries | Forest, Jungle, Taiga | Common, low nutrition |
| Roots | Plains, Grassland, Highlands | Common |
| Mushrooms | Forest, Swamp, Mire | Uncommon, some poisonous |
| Game Fowl | Plains, Savanna, Grassland | Hunting yield (§15) |
| Venison | Forest, Taiga, Highlands | Hunting yield (§15) |
| Boar | Forest, Swamp, Jungle | Hunting yield (§15) |
| Milk | Plains, Grassland, Highlands | Livestock yield (§17) |
| Eggs | Plains, Forest, any temperate | Livestock yield (§17) |
| Meat (generic) | Any with livestock or hunting | Butchered product |
| Honey | Forest, Jungle | Rare fertile node |
| Fish | Coast, Lake, Beach, Coastal Waters | Requires adjacent water hex or fishing area |
| Herbs | Multiple biomes | Dual-use: food ingredient + alchemical |
| Grain | Plains, Grassland, Steppe | Crop (sowable on fertile areas) |
| Vegetables | Plains, Forest, Highlands | Crop |
| Fruit | Jungle, Oasis, Forest | Tree-based, seasonal yield |

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
| Clay | Wetlands, Coast, Lake | Pottery, construction |
| Sand | Desert, Beach, Coast | Glass production |
| Pitch | Forest, Taiga | Waterproofing, fuel |
| Sulfur | Volcanic, Mining | Alchemical, future module |
| Saltpeter | Desert, Canyon | Alchemical, future module |
| Dyes | Jungle, Swamp, Mire | Crafting modifier |
| Charcoal | Forest, Taiga | Fuel, smelting catalyst |
| Flax | Plains, Grassland | Textile |
| Wool | Grassland, Highlands | Textile; primary source is livestock (§17) |
| Furs | Taiga, Tundra, Forest | Gear crafting (cold protection) |
| Pearls | Coast, Deep Ocean | Rare, jewelry |
| Amber | Forest, Taiga | Rare, jewelry/alchemy |
| Obsidian Shards | Volcanic, Mining | Weapon crafting |
| Alchemical Reagents | Multiple (rare nodes) | Reserved for future Alchemy & Essence module |

#### Essence

- Singular magical material. **Resource ID is registered** in the base module, but Essence does **not** drop from any base-module source.
- Essence generation, uses, and alchemy recipes are entirely scoped as a **future module** (Alchemy & Essence).
- The ID exists at deployment so future modules can reference it without requiring base module changes.

### Resource ID registry

All resources above (food, core 22, special, essence) are assigned permanent `u16` IDs at deployment. The registry includes **reserved ID ranges** for future modules:

```
0x0000–0x00FF: Food
0x0100–0x01FF: Core Resources (T1/T2/T3)
0x0200–0x02FF: Special Resources
0x0300–0x03FF: Essence & magical materials
0x0400–0x0FFF: Reserved (future modules)
```

This ensures future modules can add new resources without colliding with base IDs, and without changing the base module.

---

## 13. Production: Mining

### Mining areas

- Found within hexes via surveying. Biome determines ore types and rarity.
- Each mining area has deterministic: **ore types**, **vein count**, **vein depth**, **yield multiplier**.

### Mining action

1. Adventurer commits energy + time-lock to mine a specific area.
2. On resolution: yield = `f(vein_depth, yield_multiplier, Craftsmanship, tool_bonus)`.
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

## 14. Production: Forestry

### Forestry areas

- Found within hexes via surveying. Biome determines tree species, flora, and fauna.
- Each forestry area has deterministic: **lumber type**, **native flora**, **native fauna**, **yield multiplier**, **regrowth rate**.

### Logging action

1. Adventurer commits energy + time-lock.
2. Yield = `f(tree_density, yield_multiplier, Strength, tool_bonus)`.
3. Logging depletes tree density. Trees regrow over time (regrowth rate).

### Ecosystem dynamics

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

## 15. Production: Hunting

Hunting is an active action performed in **forestry areas**, targeting the area's native fauna.

### Hunt action

1. Adventurer commits energy + time-lock in a forestry area that has living fauna.
2. **Chance-based resolution**: success is a single roll influenced by:
   - Adventurer **Survival** and **Dexterity** attributes
   - Equipment bonuses (bows, traps, hunting gear)
   - Current **fauna density** of the area (higher = better odds)
3. On **success**: yield food (game fowl, venison, boar) and special resources (leather, bone, tallow, furs).
4. On **failure**: nothing — energy and time are still consumed.

### Interaction with logging

- Fauna density drops sharply as cumulative logging increases (same ecosystem dynamics as §14).
- Heavy logging drives fauna **extinct** → hunting becomes impossible.
- If logging ceases, fauna can recover over time (slow regrowth).

This creates a direct strategic tension: logging and hunting compete for the same ecosystem. Players (or settlements) must choose their balance.

### Hazard note

Hunting is a **deliberate action** with chance-based yields. Beast **hazard encounters** (§24) are involuntary and can occur during any action — they are separate systems. A hunter might successfully bag a deer *and* get ambushed by a wolf on the same trip.

---

## 16. Production: Foraging & Harvesting

### Fertile areas

- Found within hexes via surveying. Biome determines native plant species.
- Each fertile area has deterministic: **native species**, **fertility level**, **yield multiplier**, **regrowth rate**.

### Harvest action

1. Adventurer commits energy + time-lock.
2. Yield = `f(fertility, species_yield, Survival, tool_bonus)`.
3. Each harvest **reduces fertility**. Lower fertility → lower future yields.

### Fertility management

- **Natural regrowth**: fertility slowly recovers if the area is left unharvested.
- **Fertilize action**: adventurer can spend resources (compost, bone meal, etc.) + energy to restore fertility faster.
- **Salt the earth**: destructive action that pushes fertility **negative** (barren). Negative fertility trends back toward 0 over time, but slowly.
- **Sowing crops**: native species can be driven extinct to plant crops (Grain, Vegetables). Crops have different yield profiles than wild plants. Crop yield depends on biome affinity.

### Yields

- **Food**: Wild berries, roots, mushrooms, herbs, honey, grain, vegetables, fruit.
- **Special resources**: Herbs (dual-use), dyes, flax, alchemical reagents.

---

## 17. Production: Livestock

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
| Cattle | Meat, milk | Leather, bone, tallow | Plains, Savanna, Grassland |
| Sheep | Meat, milk | Wool, leather | Grassland, Highlands |
| Goats | Meat, milk | Leather, bone | Highlands, Steppe, Canyon |
| Poultry | Meat, eggs | Bone, feathers | Plains, Forest, any temperate |

### Upkeep

- Livestock graze, reducing area **fertility** over time (same fertility mechanic as crops).
- If fertility drops too low, herd size declines (animals starve/leave).
- Players must **fertilize** or rotate grazing to sustain herds long-term.
- Neglected livestock decline to 0 — the area reverts to barren (can be re-seeded with native species, crops, or new livestock).

### Why livestock is in the base module

Animal products (wool, leather, tallow, meat, milk, eggs) are **core crafting inputs** and **food sources** that feed into the base crafting and settlement food systems. Without livestock, wool has no reliable source, and the food/special resource supply chain has a gap that would require a base module change to fill later — which is forbidden.

---

## 18. Crafting & Refining

### Refining (raw → usable)

Most extracted materials require processing before use:

| Process | Input → Output | Facility required |
|---|---|---|
| Smelting | Ore → Bar (Copper, Cold Iron, Gold, Silver, etc.) | Smelter |
| Gem cutting | Rough gem → Cut gem (Diamonds, Sapphire, Ruby) | Workshop |
| Tanning | Raw hide → Leather | Workshop |
| Charcoal burning | Wood → Charcoal | None (campfire-level) |

- Refinery yield depends on **building tier** (higher tier = less waste) and **adventurer Craftsmanship** (higher skill = bonus yield).
- Quality tiers: dirty / clean / pure. Higher-tier facilities + higher skill produce cleaner output.

### Mutation-based crafting

```
craft(inputs[]) → new_item
new_properties = f(input_properties, craft_seed)
```

- Any compatible resources can be combined. Outputs inherit blended properties from inputs.
- A **mutation function** (XOR-seeded) introduces controlled randomness — crafted items are unique.
- **Legendary rolls** occur when property products cross defined thresholds.
- Higher Craftsmanship + Intelligence improve mutation outcomes (wider favorable distribution).

### Crafting facilities (base module)

| Facility | Produces | Key inputs |
|---|---|---|
| **Campfire** | Basic cooked food (raw → edible) | Food + Wood/Charcoal |
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

## 19. Equipment & Items

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

## 20. Construction & Buildings

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

### Building type registry

Same pattern as resources: permanent IDs at deployment with reserved ranges for future modules.

---

## 21. Settlement

### Formation (Deed system)

1. Adventurer develops **two or more hexes** to a threshold (TBD — likely: Keep built + minimum buildings).
2. The control area of one hex must have a **Keep** building.
3. The Keep allows minting a **Deed of Settlement** (energy cost).
4. The Deed is **non-transferable** and **bound to the adventurer**. Destroyed on adventurer death.
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

- **25% fewer donkeys** for transport between settlement hexes.
- **Shared population** across settlement hexes (buildings in any hex draw from the same pool).
- **Cheaper travel** between settlement hexes for the owner.

---

## 22. Population, Housing & Food

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
- **Upkeep multiplier** = `clamp(1 / staffing_ratio, 1, 3)`:
  - `staffing_ratio = 1.0` → `1×` upkeep (normal)
  - `staffing_ratio = 0.5` → `2×` upkeep (100% extra)
  - `staffing_ratio ≤ 0.25` → `3×` upkeep (200% extra, hard cap)

---

## 23. Territorial Sovereignty

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

## 24. Beasts & Hazards

### Why beasts are in the base module

Hazards are part of the physics — they make exploration dangerous, create resource sinks (energy, health), and are the primary source of permadeath outside starvation. Without them, the world has no teeth.

### Beast types

The base module uses the **Loot Survivor beast taxonomy** (75 beast types across 5 tiers). These are unnamed/wild beasts — they are environmental hazards, not player-owned.

| Tier | Examples | Biomes |
|---|---|---|
| T1 (Common) | Wolf, Rat, Spider, Snake | Most temperate/wet |
| T2 (Uncommon) | Bear, Boar, Giant Spider, Scorpion | Forest, Desert, Swamp |
| T3 (Rare) | Yeti, Wyvern, Basilisk | Tundra, Mountain, Volcanic |
| T4 (Epic) | Dragon, Kraken, Behemoth | Deep Ocean, Volcanic, Underworld |
| T5 (Legendary) | Phoenix, Leviathan, Titan | Extreme biomes, Underworld |

Each biome has a **hazard table**: weighted probability distribution of beast types that can appear.

### Encounter resolution

Hazards trigger as a **chance per action** (explore, travel, mine, log, harvest, survey):

1. `encounter_chance = base_chance × biome_modifier × action_risk_modifier`
2. If triggered: `beast_type` drawn from biome hazard table.
3. **Resolution equation** (single roll, not a combat system):
   - Inputs: beast lethality, adventurer energy, attributes (Survival, Endurance, Dexterity), equipment defense, time of day
   - Outcomes: **avoid** (no effect), **injury** (lose energy, possibly gain a negative trait), **resource gain** (pelt, bone, meat), **positive trait** (Perceptive, Quick-Witted), or **death**.
4. Higher-tier beasts have higher lethality → more likely to injure/kill.

### This is NOT a combat system

The base module does not include turn-based or real-time combat. Encounters are **single-resolution hazard checks**. A full combat module (PvP, dungeon crawling, tactical beast fights) is deferred. The hazard system provides:
- Risk that makes exploration meaningful.
- A death mechanic beyond starvation.
- Resource drops (leather, bone, tallow, furs, meat) that feed the crafting economy.
- Trait acquisition through survival.

---

## 25. Permadeath & Legacy

### Death triggers

An adventurer dies when:
- Energy reaches 0 from **negative regen** (starvation).
- A **hazard encounter** resolves as death.
- (Future module) PvP combat kills them.

### On death

1. Adventurer `alive = false`. Permanent. No resurrection.
2. All **inventory and equipment destroyed** (base module — no drops).
3. All **energy reservations/escrows** settled and released.
4. Settlement **Deeds** bound to this adventurer are destroyed (settlement persists but new deeds can't be issued by this adventurer).
5. Hex ownership **persists** but begins decaying without active upkeep from another adventurer.
6. Followers are released (disappear).
7. **Legacy record** written: discoverer tags on hexes persist forever, buildings persist, settlement structures persist.

### Respawn

Player mints a new adventurer (paying $LORDS again). Starts fresh at The Nexus. The world remembers the old adventurer; the new one starts with nothing.

---

## 26. Autoregulation

### Purpose

Prevent runaway inflation, stagnation, or exploitation without human intervention. The autoregulator is a **permissionless on-chain PI controller** that adjusts tunable parameters within pre-defined bounds.

### Tunable parameters (base module)

| Parameter | Bounds | What it affects |
|---|---|---|
| Adventurer mint cost ($LORDS) | [min, max] | Spawn rate |
| Energy regen rate | [0.05, 0.2] per tick | Economic velocity |
| Hazard lethality multiplier | [0.5, 2.0] | Death rate |
| Collapse chance multiplier | [0.5, 2.0] | Mining coordination pressure |
| Decay rate multiplier | [0.5, 2.0] | Territorial holding cost |
| Fertility regrowth rate | [0.5, 2.0] | Food scarcity |
| Attribute gain rate | [0.5, 2.0] | Progression speed |
| Follower desertion rate | [0.5, 2.0] | Party maintenance pressure |

### Mechanism

- Evaluates once per **epoch** (e.g., every 100 in-game days).
- Reads aggregate metrics: total population, active adventurers, resource production rates, death rates, average energy balance.
- Applies PI corrections: if death rate is too low → increase hazard lethality; if resources are hyperinflating → increase decay rates; etc.
- All adjustments are bounded — the autoregulator cannot exceed the defined parameter ranges.
- No admin keys. No governance votes needed. Pure algorithmic response.

---

## 27. Extension Interfaces

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

## 28. Future Module Placeholders

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

The trait system supports up to 6 slots per adventurer. The base module ships with a starter trait list. Future modules can add new traits to the enumeration — the trait storage is generic (`TraitId` is a `u16`).

### Reserved recipe IDs

```
0x0000–0x00FF: Base module recipes (food, smelting, basic gear, building materials)
0x0100–0x0FFF: Future module recipes (alchemy, advanced crafting, enchanting, etc.)
```

### Event hooks as placeholders

All 16+ event hooks (§27) fire regardless of whether any module is listening. Future modules subscribe to the events they need. The base module doesn't need to know what modules exist — it just emits events.

### Permission hook interface

The `IPermissionHook` trait (§27) is deployed per-hex by territory controllers. The base module enforces the interface but doesn't care what logic is inside. This is the primary extensibility mechanism for future social/economic/governance modules.

### Biome-specific reserved data

Each biome's hazard table includes **weight slots for T4 and T5 beasts** even though these encounters are exceptionally rare in the base module. Future combat/dungeon modules can adjust encounter weights (via the autoregulator) to make high-tier beasts more common in specific contexts without changing the base hazard resolution logic.

---

## 29. What Is Explicitly Deferred

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

## 30. Playable State

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
17. **Die** permanently from starvation or beast encounters.
18. **Respawn** as a new adventurer in a world shaped by predecessors.

This is a complete, playable survival-settlement game. It is sparse but not shallow — the interaction between energy, resources, territory, population, food, beasts, and permadeath creates genuine strategic depth and meaningful decisions.

Future modules add richness (combat, trade, governance, lore, ecosystem assets) — but the world works without them.

---

*v0.1.1 — March 2026*
*Authors: Krumpy (design direction), Squire (synthesis & drafting)*
*Scope derived from: Eternal Game Design Scope v0.1.5*
