# Eternal Game - Design Scope v0.1.5

> **Working title.** This document synthesizes the Fractales framework (Cartridge/Dojo), the Realms: Adventures idle-RPG concept, and the Realms/Loot ecosystem into a unified thesis for an eternal, fully onchain MMORPG. It is a living document - intended to be challenged, refined, and expanded over many iterations.

---

## Table of Contents

1. [Thesis](#1-thesis)
2. [Design Pillars](#2-design-pillars)
3. [The Immutable Foundation Layer](#3-the-immutable-foundation-layer)
4. [World Topology & Hex Grid](#4-world-topology--hex-grid)
5. [The Adventurer](#5-the-adventurer)
6. [Core Gameplay Loop](#6-core-gameplay-loop)
7. [Exploration & Discovery](#7-exploration--discovery)
8. [Resource Systems](#8-resource-systems)
9. [Refining & Crafting](#9-refining--crafting)
10. [Settlement & Construction](#10-settlement--construction)
11. [Territorial Sovereignty](#11-territorial-sovereignty)
12. [The Energy Economy](#12-the-energy-economy)
13. [Permadeath & Legacy](#13-permadeath--legacy)
14. [Realms/Loot Ecosystem Integration](#14-realmsloot-ecosystem-integration)
15. [Modular Game Layers](#15-modular-game-layers)
16. [Social Systems & Governance](#16-social-systems--governance)
17. [Agent & Autonomous Play](#17-agent--autonomous-play)
18. [Autoregulation & Anti-Inflation](#18-autoregulation--anti-inflation)
19. [The Game Masters](#19-the-game-masters)
20. [Development Philosophy](#20-development-philosophy)
21. [Phased Roadmap](#21-phased-roadmap)
22. [Open Questions](#22-open-questions)
23. [Open Design Details](#23-open-design-details)

---

## 1. Thesis

**Eternal Game is a fully onchain MMORPG built on an immutable physics substrate, designed to run forever.**

The ambition is not to ship a game and move on. It is to deploy a world with physics so fundamental - time, space, matter, energy - that they never need to change. Everything else - quests, economies, societies, wars, culture - emerges from players, builders, and autonomous agents interacting with those physics over years and decades.

The world is an infinite hex grid. Players pay to spawn adventurers who explore outward from a central origin, discovering terrain, harvesting resources, building settlements, forming societies, and eventually dying - permanently. What they leave behind shapes the world for everyone who follows.

Three source pillars inform this design:

- **Fractales** (Cartridge): An infinite hex exploration framework with deterministic world generation, energy economics, territorial decay, and permadeath - implemented in Cairo/Dojo with working contracts.
- **Realms: Adventures**: An idle-RPG concept focused on adventurer minting, questing, crafting, and integration with Eternum/Loot ecosystem assets.
- **The Realms Ecosystem**: Realms NFTs, Loot Survivor, Beasts, Crypts & Caverns, $LORDS, $SURVIVOR - a rich asset layer waiting for a world to inhabit.

Eternal Game fuses these into one coherent world.

---

## 2. Design Pillars

| Pillar | Meaning |
|---|---|
| **Immutable Physics** | The base layer (coordinate system, energy, time, matter properties, biome generation) ships once and is never patched. Scarcity is provable. |
| **Infinite World** | The hex grid is unbounded. There is always more to explore. Expansion is driven by players, not content drops. |
| **Permadeath & Consequence** | Adventurers die permanently. Decisions carry weight. Legacy matters. |
| **Energy as Universal Currency** | Energy is the atomic unit of all economic activity - movement, harvesting, crafting, maintenance, combat. Everything costs energy. |
| **Territorial Sovereignty** | Discovery creates property rights. Ownership demands upkeep. Neglected territory decays and becomes claimable. There is no free land - only maintained land. |
| **Headless & Client-Agnostic** | All state lives in smart contracts. Any front-end (3D, pixel, text, Discord bot, autonomous agent) can render and interact with the same world. |
| **Modular Extensibility** | The base layer supports an arbitrary number of game modules (mining, crafting, combat, diplomacy, research, trade) layered on top over time. Each module integrates with the same energy/ownership/action framework. |
| **Autonomous Game Mastery** | Content, balance, and economy are managed by a team of specialised AI agents (Game Masters) funded by game fees. They create, review, balance, and deploy content continuously — enabling a scope of gameplay impossible for human designers alone. |
| **Ecosystem Integration** | Realms assets ($LORDS, Realm NFTs, Beasts, Loot, C&C) are first-class citizens - not cosmetic overlays, but functional components of the world's economy and lore. |

---

## 3. The Immutable Foundation Layer

This is the "physics engine" - the contracts that, once deployed, **never change**. All iterative game development happens in modules that interface with this layer.

### 3.1 Time

Block time alone is fragile - chain throughput and block cadence can change. We need time that feels real-world **but never advances without blocks**.

**Chosen model (low compute, chain-safe):**

- **Tick length** `T = 10s`.
- On any state update, read `block_number` and `block_timestamp`.
- Compute:
  - `current_tick = floor(block_timestamp / T)`
  - `wall_delta = current_tick - last_tick`
  - `block_delta = block_number - last_block_number`
- **Hybrid guard (hard clamp):**
  - `tick_delta = min(wall_delta, block_delta)`

**Result:**
- If the chain **halts**, `block_delta = 0` → **game time stops**.
- If timestamps jump forward, ticks **won't skip ahead** without matching blocks.
- If blocks run fast, wall-clock still governs the tick pace (≈3-4 blocks per tick at ~2.5s blocks).

This is intentionally conservative - Starknet has had periods of halted block production and reorgs (e.g., Sept 2, 2025; Jan 5, 2026). The Eternal Game should **pause** rather than fast-forward in those events.

### 3.2 Space

- **Infinite hex grid** using cube coordinates `(x, y, z)` centered at origin `(0, 0, 0)`.
- Coordinate range: ±2.3 × 10¹² - effectively infinite.
- Storage uses felt252 encoding via a deterministic codec; gameplay API uses signed origin-centered coordinates.
- **Adjacent-only movement** - no teleportation. Distance is real. Geography is strategy.
- Each hex contains multiple **areas** (sub-locations): control areas, materials areas, bare (buildable) areas, plus special/underworld areas.

### 3.3 Matter (World Generation)

- The world is **partially deterministic** from a single global seed using Cubit noise functions.
- `hash(global_seed, coordinate)` → **biome only**.
- **Area count, area types, and slot counts** are **finalized on discovery** using a per-hex discovery seed (commit-reveal salt + global seed + coordinate).
- Domain-separated seed derivation prevents cross-stage leakage (`HEX_V1`, `AREA_V1`, `PLANT_V1`).
- Once a hex/area/plant is materialized (discovered), its properties are immutable - no balance patches, no retroactive nerfs.
- **Expanded biome set** (examples): Plains, Forest, Mountain, Desert, Swamp, Tundra, Taiga, Jungle, Savanna, Grassland, Canyon, Badlands, Volcanic, Glacier, Wetlands, Steppe, Oasis, Mire, Highlands, Coast, **Lake**, **Beach**, **Coastal Waters**, **Ocean**, **Deep Ocean**.
- Biome clustering is coherent (simplex noise produces natural terrain transitions), but contents within hexes are seed-derived and unique.

### 3.4 Energy

- Energy is the **universal resource** - the base currency of all action.
- Every adventurer has an energy pool that regenerates lazily per-tick, capped at max.
- All actions consume energy: movement, exploration, harvesting, mining, crafting, construction, territorial maintenance.
- There is **no universal item-to-energy mechanic**. Energy has a baseline passive floor via regeneration, and sinks are created through risk, upkeep, and permadeath.

---

## 4. World Topology & Hex Grid

```
Origin (0,0,0) - The Nexus
    ↓
Hex Grid (infinite, deterministic)
    ↓
Each Hex → Multiple Areas
    ↓
Each Area → Resource nodes (plants, ores), building slots
    ↓
Deep structures → Dungeon depths, fractal mine veins (future)
```

### The Nexus

All adventurers begin at The Nexus - coordinate `(0,0,0)`. This is the eternal starting point and the heart of the world.

As players explore outward, the discovered world expands like a living organism. The frontier is always at the edges, and the core grows denser with settlements, trade routes, and history.

### Spawn Nodes

Over time, as exploration radiates outward, **special areas** can be discovered that allow players to build Spawn Nodes. These are secondary starting locations that allow new players to begin further from the origin. Spawn Node requirements are **TBD** and discovered through gameplay rather than tied directly to development tier.

### Hex Contents

Each hex can have:
- A **biome** (deterministic from noise)
- An **area count** (**3-9**, rarity weighted) **finalized on discovery**; defines the total **potential areas** in the hex
- A **control area** (area_index 0) - ownership of this = control of the hex
- **Materials areas** (fertile, mining, forestry) - each with deterministic resource profiles
- **Bare (buildable) areas** - primarily for construction
- **Underworld areas** - **rare** and not present in most hexes
- **Special areas** - **rare** and not present in most hexes

Materials, Bare, Underworld, and Special areas are all **potential area types** within the area count; underworld and special areas are drawn infrequently.

**Discovery finalization:** area count, area types, and slot counts are finalized on discovery using a commit-reveal discovery seed (global seed + coordinate + discoverer salt).

### Area Rarity & Building Slots

Areas are rolled deterministically with rarity tiers. Higher area counts are increasingly rare:
- **3** (common), **4** (uncommon), **5** (rare), **6** (epic), **7** (legendary), **8** (mythic), **9** (near-impossible)

Each area has a deterministic number of **building slots**, based on type and rarity:

- **Control areas**: 2 (common), 3 (uncommon), 4 (rare), 5 (epic), 6 (legendary), 7 (mythic)
- **Bare (buildable) areas**: 2 (common), 3 (uncommon/rare), 4 (epic/legendary), 5 (mythic)
- **Materials areas**: 2 (common/uncommon), 3 (rare/epic)
- **Underworld areas**: 1 (all)
- **Special areas**: 1 (all)

**Note:** Each rarity tier requires a matching global % allocation (TBD as design progresses).

---

## 5. The Adventurer

The adventurer is the player's avatar in the world. It is an NFT-like entity (model parity, with ERC-721 wrapping planned) with the following properties:

### Core Metadata

| Metadata | Role |
|---|---|
| **Energy** | Fuel for all actions. Regenerates **per tick** based on modifiers, up to cap. |
| **Health** | Physical condition. Base 100, regen 0.01/tick. Lost through encounters/activity outcomes. Death at 0. |
| **Position** | Current hex coordinate. Movement is adjacent-only. |
| **Equipment** | Equipped gear (armor, weapons, trinkets). |
| **Inventory** | Weight-limited storage (kg). Resources stack unlimitedly; crafted items occupy individual slots. |
| **Activity Lock** | Time-locked commitment to current action (harvesting, mining, building). |
| **Alive** | Boolean. Once false, forever false. |

### Attributes

Drawing from the Adventures idle-RPG design, adventurers have **core attributes** that affect all interactions:

| Category | Attributes |
|---|---|
| **Physical** | Strength, Endurance, Dexterity, Vitality |
| **Mental** | Intelligence, Wisdom, Charisma |
| **Specialized** | Survival, Craftsmanship, Leadership |

- **All adventurers are created equal at mint**: a fixed attribute budget of **20 points** across 10 attributes.
- Points are distributed deterministically from the mint seed (20 rolls → +1 to a random attribute each roll). **No caps** - 0-6+ is possible.
- Attributes serve primarily as **performance gates** for encounters and events (e.g., a beast hunt has a STR gate where success scales with Strength).

**Gas note:** distribution can be computed lazily from the stored seed (20 iterations total). No large arrays need to be stored at mint; only the seed and adventurer core metadata are persisted.

#### Attribute Progression (Chance-Based)

Attributes improve through use - **learning by doing** - via a **chance-based system**. No experience points or historical data are tracked on-chain; each action resolution includes a simple probability roll.

- Every action has a **small base chance** to increase a relevant attribute (e.g., mining rolls for STR/INT, travel rolls for END/WIS).
- The chance decreases **exponentially** with current level: `gain_chance = base_chance × (1 - (level / 20))²`
  - Level 10 → 25% of base | Level 15 → 6.25% | Level 19 → 0.25% | Level 20 → **0%**
- **Attributes can never be lost** - only **suppressed** by trait modifiers. Underlying points persist even when traits reduce effective value.
- Base chances per action type are autoregulator-tunable.

#### Attribute Effects

| Attribute | Primary Effect | Formula |
|---|---|---|
| **Strength (STR)** | Carry capacity | `carry_cap = 50 + STR × 5 kg` |
| **Endurance (END)** | Max energy, energy regen | `max_energy = 100 + END × 10`; energy regen rate `+2%` per point |
| **Dexterity (DEX)** | Hazard avoidance | `avoid_bonus = DEX × 5%` |
| **Vitality (VIT)** | Max health | `max_health = 100 + VIT × 10` |
| **Intelligence (INT)** | Resource yield | `yield = base × (1 + INT × 0.025)` - applies to harvest, gather, mine, refine |
| **Wisdom (WIS)** | Explore & survey energy efficiency | Energy cost = `base × (1 - WIS × 0.04)` (no minimum clamp) |
| **Charisma (CHA)** | Upkeep energy efficiency | Energy cost = `base × (1 - CHA × 0.03)` (no minimum clamp); fee discount `CHA × 5%`; increases days until follower desertion |
| **Survival (SUR)** | Hunting, foraging, beast encounters | `hunt_success += SUR × 5%`; `forage_success += SUR × 5%`; `beast_encounter_success += SUR × 2.5%` |
| **Craftsmanship (CRA)** | Craft quality, repair cost, mutation | `craft_quality += CRA × 5%`; repair cost `-4%` per point; widens favorable mutation range |
| **Leadership (LEA)** | Follower count | `max_followers = 1 + floor(LEA / 2)` |

#### Health

Adventurers have a **health pool** separate from energy:

- **Base max health**: 100 (modified by Vitality: `max_health = 100 + VIT × 10`).
- **Base health regen**: 0.01 per tick (~3.6/hour, ~86.4/in-game day) - significantly slower than energy regen.
- Health is lost through **encounter or activity outcomes** (e.g., a logging accident costs health, a beast attack costs health).
- Food and special items may increase health regen rate.
- Players must weigh the risks of taking actions while health is low - there is always a **low probability of instant death** from any hazardous action when health is critical.
- **If health reaches 0: the adventurer dies** (permadeath).

#### Traits

Traits are organized into **5 types**:

| Type | Description | Gained | Lost |
|---|---|---|---|
| **Personality** | Core character disposition (Brave, Craven, Studious, etc.) | At mint (2 traits); rarely through major life events | Can be replaced by opposite trait through events |
| **Physical** | Bodily characteristics (Brawny, Nimble, Perceptive, etc.) | At mint (1 trait); rarely through events | Can be replaced by opposite trait through events |
| **Skill** | Aptitudes from experience (Green Thumb, Born Hunter, etc.) | Gained by performing actions - positive from success, negative from failure | **Cannot be lost once gained** |
| **Injury** | Temporary wounds (Lacerated, Fractured, Bruised, etc.) | From encounter/activity damage | Lost when health returns to >75% of maximum health and any action is completed; may convert to disability (probability scales with damage dealt) |
| **Disability** | Permanent scars (One-eyed, Lame, etc.) | Converted from severe injury traits | **Cannot be lost** |

- **At mint**: 2 personality traits + 1 physical trait.
- **Max 10 trait slots**. Trait gain uses a **chance-based system** (no XP/history tracking):
  - Every action/event has a small base chance to grant a trait; event criticality scales the chance.
  - `gain_chance = base_chance × (1 − (trait_count / 10))²` — quadratic decrease, 0% at 10 traits.
- **Exclusivity groups**: grouped traits are fully exclusive (Brave vs Craven — impossible to have both). Almost all personality and physical traits have an opposite.
- Traits may grant **attribute modifiers**, **special effects**, or both (not every trait needs both).
  - Max **±2** per trait. Shapes: `[+1]`, `[−1]`, `[+2]`, `[−2]`, `[+1, +1]`, `[+1, −1]`, `[−1, −1]`. Dual shapes are rarer.
  - **No doubling rule**: modifier must not amplify same stat as special effect.
  - Trait modifiers **suppress** attributes (reduce effective value) but underlying points persist.
- Some traits boost or penalize **attribute/trait gain chances** as their special effect (e.g., "Diligent: +10% attribute gain chance" — applied on top of the small base chance per action).
- Personality traits can be gained/replaced through **major life events** (very rare). Skill traits from actions (success → positive, failure → negative). Skill traits become harder to acquire as count rises.

#### Minting & Seed Fairness

- Use a **salt commitment** for fairness: user commits `hash(salt)` at mint request, then reveals the salt when finalizing.
- Final seed example: `hash(globalSeed, tokenId, mintBlock, userSalt)`.
- Prevents front-running and keeps trait/attribute rolls unbiased.

### Followers (Adventurer Party)

Adventurers can accumulate **Followers** - NPCs that travel with the adventurer as their party.

- Followers are **not** settlement population and do **not** count toward any hex/settlement staffing.
- Followers represent the adventurer's personal logistics and support: **laborers**, **donkeys/mounts**, and (future module) **bodyguards**.
- The maximum party size is gated by the adventurer's **Leadership** attribute.

#### Party composition
- **Laborers**: improve harvest/build task throughput for the adventurer's personal actions.
- **Donkeys**: +50 kg carry capacity (pack animal, no travel bonus).
- **Mounts**: -20% travel energy cost, -20% travel time, +20 kg carry capacity. Maximum **1 mount** per adventurer at any time.
- **Bodyguards (future module)**: reduce hazard lethality or improve combat odds.

Base followers: **1** (even a lone adventurer can have a mount or donkey). Maximum party size: `max_followers = 1 + floor(LEA / 2)`.

#### Followers vs settlement workforce
- Settlement population = people living/working in the settlement.
- Adventurer followers = people/animals attached to an adventurer and moving with them.
- Followers can be hired/earned in settlements, but once attached they are tracked at the **adventurer level**.

#### Open design details
- Followers consume food as the adventurer does (shared food resource type). Exact quantities per follower type are TBD.
- Whether followers can die / desert on hazards and starvation events (likely yes; this creates meaningful risk).

### Equipment

- Equipment slots: **head, body, waist, hands, feet**, plus **one neck slot** and **one ring slot**.
- Equipped gear modifies attributes and provides bonuses.
- Gear has **durability** that decreases with use. Broken gear must be repaired or replaced.
- Gear weight counts against backpack capacity.

### Energy (Simplified)

- Adventurers have an **energy bar** that decreases after action attempts.
- Actions require **up-front reservation** of energy and a time-lock commitment; energy cannot drop below 0.
- Longer/harder actions drain more energy.
- Energy replenishes **per tick** when adventurers are idle (not in a time-locked action).
- **Base regen**: **0.1 energy per tick** (≈36 energy/hour). Endurance increases regen rate by **+2% per point**.
- **Base max energy**: **100** (≈2.8 hours of storage at base regen - aligned with in-game day length). Modified by Endurance: `max_energy = 100 + END × 10`.
- **Maximum energy** is determined by Endurance and any buffs/debuffs.
- No separate **rest action** - an idle adventurer is resting.
- **Future module (shared food resource):** food quality buffs regen rate and max energy. Adventurers can eat basic/raw food (no bonus) or cooked/crafted food (bonuses). Neglecting food applies regen debuffs that can turn regen negative; if energy is negative on any tick, the adventurer dies.

---

## 6. Core Gameplay Loop

The fundamental loop, from which everything else emerges:

```
SPAWN → EXPLORE → DISCOVER → GATHER → REFINE → MAINTAIN → EXPAND → (DIE → LEGACY)
         ↑                                                      |
         └──────────────────────────────────────────────────────┘
```

1. **Spawn**: Pay to mint an adventurer at a Spawn Node (Nexus or unlocked secondary).
2. **Explore**: Entering an **unexplored** hex is an **explore action** (higher energy/time cost). Moving through already known hexes is a **travel action** (lower cost).
3. **Discover**: First visitor to a hex/area materializes its deterministic contents. Discovery creates ownership rights.
4. **Gather**: Harvest plants, mine ores, hunt creatures. Time-locked activities with energy costs.
5. **Refine**: Process raw materials into usable resources. Craft gear and consumables.
6. **Maintain**: Pay territorial upkeep (energy) to prevent decay. Defend territory from claimants.
7. **Expand**: Build settlements. Develop infrastructure. Establish trade routes. Form societies.
8. **Die**: Permadeath. Adventurer is gone. Legacy remains.

### 6.1 Core Actions & Costs

**Universal model:** every action has an **energy cost (reserved upfront)**, a **time-lock (ticks)**, a **risk profile** (hazard chance), and a **clear outcome**.

**Early-phase action list**

**Movement / Discovery**
- **Explore** (enter unknown hex): higher energy/time
- **Travel** (known hex): lower energy/time
- **Survey** (reveal one area): medium energy/time

**Resource**
- **Harvest** (fertile area)
- **Mine** (mining area)
- **Log** (forestry area)
- **Hunt** (forestry area - fauna)
- **Raise Livestock** (cleared fertile area)

**Territory & Development**
- **Construct** (build on an area slot)
- **Claim** (take control of decayed hex)
- **Maintain** (upkeep payment)
- **Settle** (establish or expand a settlement cluster)

**Underworld**
- **Delve** (explore underworld areas; high energy/time/risk for rare resources)

**Cost drivers (scalable)**
- **Biome modifier** (dangerous biomes cost more)
- **Development modifier** (infrastructure reduces cost/time)
- **Survey completeness** (fully surveyed → cheaper travel)

---

## 7. Exploration & Discovery

The world begins unknown, but **onchain data is public**. There is **no enforced fog-of-war at launch** - discovery is still meaningful for ownership and progression, but map data can be read by third parties.

### Visibility (No Fog at Launch)

- Discovery is public and globally readable.
- UI can still highlight explored vs unexplored for player experience, but it is not privacy-secure.
- **Future upgrade (pre-launch if possible):** if Starknet privacy primitives mature (e.g., encrypted mempool or private execution), we can introduce **per-wallet fog** and delayed reveals.

### Travel Cost Modifiers

- **Explore actions** (first entry to unknown hex) cost more energy/time.
- **Travel actions** cost less in hexes that are fully surveyed.
- Infrastructure development can further reduce travel energy and time.

### Discovery Rights

- The first adventurer to discover a hex becomes its **discoverer** - recorded permanently.
- The first adventurer to discover the control area (area_index 0) becomes the hex **controller**.
- Controllers set rules for their territory (access, fees, permissions) through programmable hooks.
- All area ownership within a hex aligns to the controller.

### Frontier Expansion

The explored world grows organically:
- **Early game**: Dense exploration around The Nexus. Competition for prime territory near the center.
- **Mid game**: Corridors extend outward. Spawn Nodes establish secondary clusters.
- **Late game**: Vast explored regions with cities, trade routes, and political boundaries. The frontier is far from origin, requiring significant investment to reach.

---

## 8. Resource Systems

### 8.0 Materials List (Raw Yield)

Materials areas yield **raw materials**. This list will evolve, but current buckets are:

- **Food** (plants, crops, animals): wild berries, roots, mushrooms, boar, venison, fish, game fowl, honey, milk, eggs.
- **Core Resources** (the familiar "Eternum 22" resource set, used here as a **canonical taxonomy** - but **not bridged/ported** from Eternum).
- **Special Resources** (non-core raw materials that extend the supply chain):
  - Examples: **leather, bone, tallow, salt, clay, sand (glass), pitch, sulfur, saltpeter, dyes, herbs, charcoal, flax, wool, furs, pearls, amber, obsidian shards, alchemical reagents**. Wool and some leather/tallow come primarily from livestock (§8.5); furs and bone from hunting (§8.4).
- **Essence** (singular magical material - see §15, Modular Game Layers). Essence exists as a resource ID in the base module but its generation, uses, and alchemy recipes are scoped as a future module.

### 8.1 Fertile Areas (Plants)

- **Plant nodes** are now **fertile areas**.
- On discovery, fertile areas contain **native plants** with deterministic fertility, species, yield multiplier, and regrowth rate.
- Native plants can yield **Food** and **Special Resources** depending on species.
- There is **no permanent extinction of the area**, but **native species** can be driven extinct (e.g., to sow crops or raise livestock - see §8.5).
- Each harvest reduces fertility (lower yield). Players can **fertilize** to restore fertility, or **salt the earth** to push fertility negative and render it barren. Negative fertility trends back to 0 over time.
- Different biomes produce different native species; crop yields vary by biome affinity.

### 8.2 Mining Areas (Ores)

- **Mine-field areas** → **mining areas**.
- On discovery, these areas have deterministic ores, vein counts, vein depths, and yield multipliers.
- Mining yields:
  - **Usable Core Resources** (stone, coal, obsidian) that do not require refinement.
  - **Ores** refined into Core Resources (copper, silver, cold iron, etc.).
  - **Rough gemstones** refined into Core Resources (diamonds, sapphires, rubies, deep crystal).
  - **Rare materials** with unique refinement processes: ignium, ethereal silica, true ice, twilight quartz, alchemical silver, adamantine, mithral, dragonhide.
  - **Other materials** that become Special Resources.
- Collapse doesn't permanently destroy a mining area, but it requires a lengthy **clearing** process before it can be mined again.

### 8.3 Forestry Areas (Lumber & Ecosystems)

- On discovery, forestry areas contain deterministic **lumber type**, **native flora**, **native fauna**, **yield multiplier**, and **regrowth rate**.
- Forestry yields:
  - **Usable Core Resources** (wood, ironwood, hartwood) that do not require refinement.
  - **Food** and **Special Resources** from native flora and fauna.
- Forestry is **ecosystem management**:
  - As logging increases, lumber yield rises slightly, while flora/fauna yield drops sharply.
  - Heavy logging can make native flora/fauna extinct (0% yield) while improving lumber yield - and **eliminates hunting** in the area (see §8.4).
  - Players may choose to preserve rare flora/fauna instead of maximizing wood (and maintain hunting yields).
  - Native flora/fauna can return over time if logging ceases entirely.

### 8.4 Hunting (Forestry Areas)

Hunting is an active action performed in **forestry areas**, targeting the area's native fauna.

- **Hunt action**: adventurer commits energy + time-lock in a forestry area.
- **Chance-based outcome**: success is a roll influenced by:
  - Adventurer **Survival** attribute (`hunt_success += SUR × 5%`)
  - Equipment bonuses (bows, traps, hunting gear)
  - Fauna density of the area (see ecosystem dynamics in §8.3)
- **Logging trade-off**: hunting success chance (fauna yield) **drops sharply** as cumulative logging increases in the area. Heavy logging drives fauna extinct, eliminating hunting entirely.
- **Yields on success**: Food (game fowl, venison, boar, fish where applicable) and Special Resources (leather, bone, tallow, furs).
- **Yields on failure**: nothing (energy + time still consumed).
- Fauna can recover over time if logging ceases (same regrowth mechanic as §8.3).

### 8.5 Livestock (Fertile Areas)

Livestock raising is an alternative land use for **fertile areas** that have been cleared of native species (same precondition as sowing crops in §8.1).

- Once native species are driven extinct on a fertile area, the player can choose to either **sow crops** or **raise livestock**.
- **Raise livestock action**: establishes a herd on the area. Requires initial stock (animals acquired from hunting yields or trade) + construction of a pen/enclosure (uses a building slot or is area-intrinsic, TBD).
- **Livestock yields**: Food (meat, milk, eggs) and Special Resources (wool, leather, bone, tallow).
- **Upkeep**: livestock consume food (grazing reduces area fertility over time, similar to crops). Neglecting livestock leads to herd decline.
- **Biome affinity**: livestock types and yields vary by biome. Grassland and Highlands favour wool-bearing animals; Plains and Savanna favour cattle.
- Livestock and crops are **mutually exclusive** on a given fertile area (choose one land use).

### 8.6 Beasts & Hazards

- Biome-specific **beast attacks** and wildlife encounters function as hazards during exploration, travel, and production actions (see §13 for ecosystem integration with Loot Survivor beasts).
- Hunting fauna in forestry areas is a deliberate activity (§8.4); beast hazards are involuntary encounters.

---

## 9. Refining & Crafting

### 9.1 Refining (Raw → Core → Quality)

- Raw materials usually require **refining** into Core Resources before use.
- Exceptions (usable raw): **wood, stone, coal, obsidian, ironwood, hartwood**.
- Some Special Resources may skip refinement; others may require unique processes.
- Core Resources can then be refined into **quality tiers** depending on building tier and operator skill.
  - Example: copper ore → copper (Core) → dirty copper / clean copper / pure copper.
- Refinery yield depends on **building level** and **adventurer Intelligence** (`yield = base × (1 + INT × 0.025)`). Higher-level buildings raise baseline yield; higher INT improves efficiency.

### 9.2 Mutation-Based Crafting

Drawing from both Fractales and Adventures:

```
craft(inputs[]) → new_item
new_properties = f(input_properties, XOR_seed)
```

- Any resources can be combined. Outputs inherit blended stats from inputs.
- A **mutation function** introduces controlled randomness - crafted items are unique.
- Legendary rolls occur when property products cross defined thresholds.
- Higher **Craftsmanship** improves mutation outcomes (wider favorable distribution, `craft_quality += CRA × 5%`). **Intelligence** improves resource yield from refining.

### 9.3 Tiered Crafting

The following crafting facilities are **examples only**. A much larger list is expected as the building catalog expands:

| Facility | Produces | Key Inputs |
|---|---|---|
| **Cookhouse** | Food consumables (energy recovery, buffs) | Plants, hunted materials |
| **Blacksmith** | Weapons and armor | Ores, smelted bars, alloys |
| **Jeweler** | Rings, necklaces, imbued gear (trait bonuses) | Rare ores, gems, plant compounds |

### 9.4 Item Durability & Sinks

- All crafted gear has durability that degrades with use.
- Repair requires resources (creating ongoing demand).
- Broken gear is useless until repaired.

---

## 10. Settlement & Construction

### 10.1 Building System

The building list will expand dramatically as area slots increase. Representative early buildings include:

| Building | Function |
|---|---|
| **Smelter** | Ore → bar conversion with efficiency bonus |
| **Shoring Rig** | Reduces mine collapse risk in hex |
| **Greenhouse** | Improves plant regrowth rates |
| **Workshop** | Reduces build costs and times |
| **Storehouse** | Increases storage/carry capacity |
| **Watchtower** | Defense bonus in territorial disputes |

- Building slots are **per-area** and **deterministic**, based on area rarity (see §4).
- Buildings require **various materials** to construct.
- Buildings require ongoing upkeep (energy + materials) or they become inactive.
- Building control transfers with hex control during claim/defend.

### 10.2 Settlements → Cities → Kingdoms → Empires

- A cluster of developed hexes can form a **Settlement**.
- Settlements unlock higher-tier buildings, which in turn unlock higher-tier crafting.
- Settlements can evolve into **Cities, Kingdoms, and Empires** following the Realms upgrade system.
- Empires are expected to take **months or years** of focused development. Benefits are **TBD**, likely introduced as modules over time.

### 10.3 Settlement Formation (Deed System)

- Settlements consist of **two or more sufficiently developed hexes** owned by the same player (**threshold TBD** after initial building list).
- The **control area** (with a central admin building such as a *Town Hall*, TBD) can mint a **Deed of Settlement**.
- **Deed constraints:** deeds do **not** expire and are **non-transferable**. They are **bound to the adventurer** that mints them and are **destroyed on that adventurer's death**.
- An adventurer can carry the Deed to the Town Hall of an **adjacent owned hex** and spend energy to **apply the deed** (**costs TBD**):
  - If no settlement exists, the deed **creates** a settlement.
  - If a settlement already exists, the deed **adds the hex** to it.
- Settlements can expand indefinitely, but **large settlements incur higher admin maintenance**. Beyond **7 hexes**, maintenance costs scale **exponentially**.
- **Settlement benefits** (initial):
  - **25% fewer donkeys** required for transport between settlement hexes
  - **Shared population** across settlement hexes
  - **Cheaper travel** for the owner between settlement hexes (infrastructure-dependent)
- Individual hexes can **degrade and be taken over**; if a hex is captured, it **leaves the settlement**. The new owner must perform the deed process to add it to their own settlement.

### 10.4 Population, Housing & Food (Settlement Maintenance)

Settlements are not just buildings - they are **people**.

Each settlement tracks a **Population** value, which is constrained by (1) **housing capacity** and (2) **food reserves**.

#### Time cadence
- **1 in-game day = 4 real-world hours**.
- Therefore: **6 in-game days per real-world day**.

Population growth/decline is evaluated on the **in-game day** boundary.

#### Housing capacity
- Each settlement has a **Base Max Population** from housing structures:
  - `base_max_pop = Σ(housing_building_count × housing_capacity_value)`
  - `housing_capacity_value` is **TBD** until the building list is fully authored.
- **Realm metadata bonus (cities):** Realm NFTs have **5-21 "cities"** metadata.
  - Each city provides **+1% max population per housing building**.
  - If a housing building normally provides `H` capacity, and the Realm has `C` cities:
    - `effective_capacity_per_housing = H × (1 + 0.01 × C)`
  - Settlement Effective Max Population is:
    - `effective_max_pop = floor( Σ(housing_building_count × effective_capacity_per_housing) )`

**Important nuance (food cost):**
- **Only base population capacity requires food upkeep.**
- The Realm "cities" bonus adds **extra headroom** (higher effective max) **without increasing food requirement** for upkeep.
  - i.e., the settlement can temporarily operate at population above `base_max_pop` (up to `effective_max_pop`) without consuming additional food beyond the base requirement.

This creates a meaningful utility for Realm city metadata without creating runaway compounding costs.

#### Food reserves (new upkeep requirement)
Settlements maintain a **Food Reserves** bar (similar to upkeep/maintenance):

- The bar is **capped at 100%**.
- It is **deposit-only**: there is **no automatic routing** from storage → reserves.
- The bar is filled by moving food items from settlement storage into **Food Reserves**.
- **Filling Food Reserves is an action** that costs adventurer energy.
- Food is a **shared resource type** used by both:
  - settlement population consumption, and
  - adventurer consumption (basic/raw food vs cooked/crafted food with bonuses).
- Settlement population is not picky: any food deposited into Food Reserves is valid.

**Drain rate:**
- Food Reserves drains at **2.5% per in-game day**.
- At this rate, a full bar empties in **40 in-game days**, i.e. **~160 real-world hours (~6.7 days)** without intervention.

**Scaling:**
- The amount of food required to fill the Food Reserves bar scales **proportionally with Base Max Population** (not the Realm bonus headroom).
  - Bigger settlements need more food to "top up" the bar.
  - Realm bonus population is "free" in upkeep terms (no extra food requirement), but still obeys housing headroom.

#### Population growth/decline
- For every in-game day the Food Reserves bar is **at zero** → **Population -1**.
- For every in-game day the Food Reserves bar is **greater than zero** → **Population +1**, up to **Effective Max Population**.

This creates a slow, readable demographic drift that is sensitive to sustained neglect.

#### What staffing does (and does not do)
- Population, food, and upkeep determine whether buildings remain **functional / open for use** (cleaning, preparation, utilities, basic repair).
- Adventurers still must spend **energy** to actually perform work using buildings (crafting, refining, construction actions).

#### Buildings require population to operate
- Each building has a **Required Population** (a staffing expectation).
- A settlement's **Total Required Population** is the sum across all active buildings.

If **Total Required Population > Current Population**, the settlement is understaffed:

- Define `staffing_ratio = clamp(CurrentPop / RequiredPop, 0, 1)`.
- Apply penalties based on `staffing_ratio`:
  - **Production efficiency** (the building staying operational/usable) scales **linearly** with `staffing_ratio`.
  - **Upkeep costs** increase inversely with a hard cap:
    - `upkeep_multiplier = clamp(1 / staffing_ratio, 1, 3)`

This yields the desired anchors:
- At `staffing_ratio = 0.5` → `upkeep_multiplier = 2×` (100% extra upkeep)
- At `staffing_ratio <= 0.25` → capped at `3×` (200% extra upkeep)

**Interpretation:** when understaffed, settlements spend proportionally more time/energy maintaining instead of producing.

### 10.5 Land Claiming & Development

- Development progression: **Unexplored hex → Explored hex → Developed hex → Settlement → City → Kingdom → Empire**.
- Each tier requires more investment but provides more benefits (trade access, crafting efficiency, defensive bonuses, revenue from other players).

---

## 11. Territorial Sovereignty

### 11.1 Ownership Through Discovery

- First to discover a hex's control area becomes its controller.
- Controller sets access rules via **programmable permission hooks** (smart contracts).
- Possible business models for territory owners:
  - Simple access fees
  - Guild-gated access with staking requirements
  - Auction-based time slots
  - Profit-sharing cooperatives
  - Insurance services

### 11.2 Territorial Decay

Every owned hex has a **passive energy consumption**:

- Base cost varies by biome (plains cheap, volcanic expensive).
- **Productivity tax**: higher economic output = higher energy costs.
- **Infrastructure complexity**: more buildings = higher coordination costs.

If a controller fails to pay upkeep:

| Decay Level | Effect |
|---|---|
| 0-20% | Minor efficiency penalties |
| 21-50% | Slower operations, some services offline |
| 51-80% | Most functions disabled, infrastructure failing |
| 80-100% | **Territory becomes claimable by anyone** |

### 11.3 Hostile Takeover

When territory reaches 80%+ decay:
1. Any adventurer can **initiate a claim** by locking energy into escrow.
2. Original controller has a **grace period** (500 blocks) to defend by investing energy.
3. If undefended, the claimant takes control - all area ownership transfers.
4. Claim escrow is consumed on success; refunded on failure/expiry.

This creates **natural territorial limits** - players can only hold as much territory as they can maintain. Empires require economic sustainability, not just military power.

---

## 12. The Energy Economy

### 12.1 Sources

- Per-tick lazy regeneration (base income). **Base rate:** 0.1 energy/tick (~36/hour). Endurance increases regen by +2%/pt.
- Limited additional sources from crafted items, special areas, and future mechanics (**TBD**).
- Trade with other players.
- Revenue from owned territory (fees from other players' activities).

### 12.2 Sinks

- Movement (energy per hex).
- Exploration (energy per discovery).
- Harvesting, mining, crafting (action costs).
- Territorial maintenance (ongoing upkeep).
- Construction (material + energy costs).
- Death (all energy lost with adventurer).

### 12.3 Dynamic Rates (If Any)

If any energy-exchange mechanisms are introduced later, they should be **dynamic and rate-limited** to avoid manipulation. The baseline economy should stand on **regen + risk + upkeep**, not item-to-energy exchange.

---

## 13. Permadeath & Legacy

### 13.1 True Permadeath

When an adventurer dies (0 health, starvation/0 energy, hazard encounter):
- The adventurer NFT is **burned**. Gone forever.
- **Inventory is destroyed** in the initial module (no PvP yet, no persistent items). This may change in future modules.
- Active reservations/escrows are settled and released.
- No resurrection. No appeals.

### 13.2 Death Causes & Hazards

Hazards can be biome- or task-specific events. Hazards occur **by chance per tick** (or per action-resolution step). The outcome (avoid, lose health, gain an injury trait like lacerated/fractured/bruised, or death) is determined by an equation that includes:

- Hazard lethality
- Time of day
- Adventurer energy
- Attributes and traits

Surviving hazards may grant resources (mine collapse reveals a new vein, wolf attack yields a pelt) or **positive skill traits** (Prospector, Beast Slayer, Quarrier, etc.).

### 13.3 Legacy

Death is not the end of impact:
- **Map Shards (conditional):** Only if privacy/fog-of-war becomes feasible, a **Map Shard NFT** may be minted on death to record the adventurer's explored path.
- **Discovered territory persists**: Everything the adventurer discovered, built, and developed remains in the world - but ownership must be maintained by someone else or it decays.
- **Reputation/history**: The adventurer's deeds are recorded on-chain forever. Discoverer tags on hexes persist. No legacy stat buffs.

### 13.4 Respawn

After death, the player mints a new adventurer (paying the spawn cost again). The new adventurer starts fresh at a Spawn Node - but in a world shaped by the old one's actions.

---

## 14. Realms/Loot Ecosystem Integration

This is where Eternal Game becomes uniquely powerful. The Realms/Loot ecosystem provides a rich asset layer that transforms from speculative collectibles into **functional world components**.

### 14.1 $LORDS - The Sovereign Currency

- **Adventurer minting** costs $LORDS (primary revenue sink and demand driver).
- Mint revenue split is decided by the developers/DAO, but a portion is **escrowed** until the adventurer's fate is decided.
- If an adventurer dies in a hex they control, the escrowed $LORDS return to the owner (life insurance).
- Explorers dying of starvation in the wilderness likely return the $LORDS to their owner as well, since they discovered and thus temporarily control the hex.
- If an adventurer dies in a hex owned by another player, the escrowed $LORDS go to the hex owner - creating indirect PvP incentives and dungeon-delving risk.

### 14.2 Realm NFTs - Seats of Power

- Realms can be applied on a **series of hexes** owned by the player once those hexes form a Settlement.
- Once established, hexes occupied by a Realm **can't be fully claimed** by another player, but they can **degrade** if not upkept. Future mechanics may include active sabotage and subjugation.
- On establishment, Realms receive **max-slot materials areas** corresponding to the Realm's resource metadata, spread across the settlement's hexes.
- The number of **regions** on the Realm NFT determines how many settlement hexes are part of the Realm (not all settlement hexes must be Realm hexes).
- City, harbour, and river metadata provide population, trade, and yield bonuses respectively.

### 14.3 Loot Survivor Beasts - Hazards & Underworld Encounters

- **Unnamed beasts** (wolf, yeti, banshee, etc.) are **hazards** in specific biomes; success yields special resources.
- **Named beasts** (captured in LS2 and owned by players) appear in **underworld areas**.
- A staking/dungeon-delving/risk-to-earn game involves LS2 beasts, C&C, and adventurers (scope TBD).

### 14.4 Loot - Persistent Relics

- **Loot items are persistent**: they can degrade without maintenance but can never be fully destroyed or lost.
- If a Loot item reaches 0 durability, it becomes **inert** until repaired, but it remains bound to its owner.
- **Loot Vaults (future module):** once a settlement is developed, players can build a **Loot Vault** and mint Loot items from their persistent Loot NFT. When a Loot item is minted and equipped, it **cannot be minted again** until it is destroyed in the world. If an adventurer dies with Loot equipped, the Loot item is destroyed but becomes mintable again from the Vault.
- The **Greatness system** becomes the base rating for all equippable items in Eternal Game:
  - **Greatness 1** is the lowest, **Greatness 20** is the highest.
  - Crafting, refinement, and upgrades can shift greatness within defined bounds, but 20 remains legendary.

### 14.5 Genesis Adventurers (GA) - Timeless Travelers

- Genesis Adventurers are **timeless** figures implied by the original Loot contract.
- GAs **spawn like any other adventurer** (paying $LORDS at a Spawn Node) but arrive with their Loot items **already equipped**.
- They can die and are bound by the same energy rules, but **their Loot items never drop** and can be **respawned** with them at a Spawn Node.
- Their existence is a mystery; over time they become the **familiar, legendary characters** of the world - immortal in story, persistent on-chain.

### 14.6 Crypts & Caverns - Dungeon Delving

- C&C can be placed by players on **underworld areas** of hexes they own.
- Keep existing points (2, 3, 4) from v0.1; remove the former point 5.
- Dungeon owners can **fill their dungeons with beasts** as part of the risk-to-earn loop.

### 14.7 $SURVIVOR - The Adventurer's Token

- Earned through: **successfully delving player dungeons** and **completing player bounties**.
- Nearby player-owned underworld areas with established C&C + beast populations apply a **malus** to surrounding player-owned infrastructure.
- Players can put up **bounties** for adventurers to clear these dungeons to reduce penalties.

### 14.8 Cross-Game Economy

| Gameplay Level | Resources Needed |
|---|---|
| **Basic** | In-game only (energy, plants, ores, materials) |
| **Moderate** | In-game + $LORDS spend/burn (premium crafting, settlement perks) |
| **Advanced** | In-game + Realm bonuses + beast materials + $LORDS |

Eternal Game is fully self-contained: Core Resources are **not bridged** from Eternum contracts (no shared balances), even if we reuse the same named resource list for familiarity.

---

## 15. Modular Game Layers

The immutable foundation supports **any number of game modules** layered on top over time. Each module:
- Interfaces with the energy system (all actions cost energy).
- Interfaces with the ownership system (territorial rights apply).
- Uses the universal action framework (time-locks, skill checks, reservations).
- Integrates with the autoregulator (economic impact is tracked and balanced).

### Planned Module Progression

| Phase | Modules | Focus |
|---|---|---|
| **Foundation** | World, Adventurer, Movement, Energy | Physics. Can never change. |
| **Survival** | Fertility, Harvesting, Decay, Claims | Core economic loop. Idle gameplay. |
| **Industry** | Mining, Forestry, Construction | Resource depth. Cooperation pressure. |
| **Craft** | Refining, Crafting, Equipment, Durability | Item economy. Specialization. |
| **Society** | Settlements, Trade, Guilds, Governance | Social structures. Cities. |
| **Combat** | Beasts, PvP, Raids, Siege | Conflict. Territory wars. |
| **Lore** | Quests, Dungeons (C&C), Research, Narrative | Depth. Discovery. Meaning. |
| **Autonomy** | Agent hooks, Programmable behaviors, AI services | 24/7 autonomous play. |

Each phase can take **months to years**. The foundation is eternal; the modules are the journey.

### Third-Party Modules

Because the foundation is headless and permissionless, **anyone** can build modules:
- Independent developers can create new game systems that interact with the world state.
- Permission hooks allow territory owners to deploy custom smart contracts for access control, revenue sharing, and business logic.
- The game becomes a **platform**, not just a product.

---

## 16. Social Systems & Governance

### 16.1 Guilds & Cooperatives

- Players naturally form groups to:
  - Coordinate mining (avoid collapse).
  - Share territorial maintenance costs.
  - Pool resources for construction and crafting.
  - Defend territory collectively.
- Guild structures are programmable through smart contracts - profit sharing, voting, membership rules.

### 16.2 Trade

- Player-to-player trade through in-game markets (city facilities).
- No global auction house - trade is **local** (you must be in range of a market or trade partner).
- This creates natural **trade routes** and makes geography matter for commerce.
- Information asymmetry is real - knowing where resources are is valuable.

### 16.3 Governance

- Regional governance can emerge through settlement councils.
- Realm holders have special governance weight.

---

## 17. Agent & Autonomous Play

### 17.1 Programmable Adventurers

Adventurers can be given autonomous behaviors through deployed smart contracts:
- **Trading bots**: Autonomous profit optimization, market making.
- **Mining coordinators**: Group formation, safety protocols, fair profit sharing.
- **Resource managers**: Automated harvesting, refinement, maintenance payment.
- **Diplomatic agents**: Alliance formation, conflict resolution.

### 17.2 AI Agent Integration

- Off-chain AI services can observe world state (via Torii indexer) and submit transactions on behalf of adventurers.
- This creates a world where both human players and autonomous agents coexist - the world is always active, even when humans are offline.
- x402 micropayment integration for agent services (per-request billing for AI decision-making).
- Agent discovery/trust via ERC-8004 identity registries.

### 17.3 The Idle-to-Active Spectrum

Players choose their engagement level:
- **Full idle**: Deploy autonomous agents, check in periodically.
- **Strategic**: Make key decisions (where to explore, what to build), let agents handle routine.
- **Active**: Direct control for high-stakes moments (combat, territorial disputes, rare discoveries).

---

## 18. Autoregulation & Anti-Inflation

### 18.1 The Problem

Onchain games face inevitable hyperinflation: resources enter faster than they leave, accumulated wealth makes new players irrelevant.

### 18.2 The Solution Stack

| Mechanism | Effect |
|---|---|
| **Passive energy regen** | Establishes a predictable floor without item-to-energy exchange. |
| **Territorial decay** | Ownership is a continuous cost, not a one-time acquisition. |
| **Permadeath** | Wealth is periodically destroyed (along with the adventurer carrying it). |
| **Durability/repair** | Gear degrades, creating ongoing resource demand. |
| **Dynamic rate limits** | If any energy exchange exists, it must be supply/demand-responsive. |
| **Productivity scaling** | Higher output increases upkeep pressure. |
| **Permissionless autoregulator** | PI controller adjusts parameters per-epoch without operator intervention. |

### 18.3 Scarcity From Player Decisions

The world doesn't have artificial scarcity - it has **player-created scarcity**:
- Over-harvest a fertile area → yield collapses (fertility drops).
- Over-mine a vein → it collapses, requiring clearing.
- Over-log a forest → flora/fauna vanish.
- Neglect territory → it decays and becomes claimable.
- Die → all your energy is lost.

---

## 19. The Game Masters

### 19.1 The Problem of Scale

The immutable physics layer (time, space, matter, energy, biomes, traits, resources) defines what *can* happen in the Eternal World. But the content that runs on top of that physics — crafting recipes, encounters, events, NPC behaviours, balance parameters, economic tuning — is potentially infinite. Thousands of resource combinations, hundreds of discoverable recipes, encounters with branching outcomes, seasonal events, emergent gameplay opportunities.

This scope is impossible for a human design team to create, balance, and maintain across the lifetime of a truly eternal game. Content creation at this scale requires tireless, continuous, data-driven iteration — exactly what AI agents excel at.

### 19.2 The Solution: Autonomous Game Masters

The Eternal Game is designed, balanced, and managed by a team of AI agents known as the **Game Masters**. They are the dungeon masters of the Eternal World — autonomous, specialised, and funded by the game's own economy.

Game Masters operate on everything above the immutable foundation:
- **Immutable (Phases 1–4)**: Time, attributes, traits, resources, biomes. Fixed at deployment. Game Masters cannot modify these.
- **Dynamic (Phases 5+)**: Actions, production rates, crafting recipes, encounter tables, event triggers, autoregulator parameters, NPC behaviours, lore entries, balance tuning. Game Masters have full authority here.

This creates a clean separation: the physics are trusted and permanent; the gameplay is living and evolving.

### 19.3 The Game Master Roles

Five specialised agents, each with a distinct remit:

#### The Creator

Charged with endless content generation. Proposes new crafting recipes, encounter scenarios, events, NPC archetypes, discoverable areas, and gameplay opportunities. The Creator's output is prolific but speculative — nothing it produces enters the game until reviewed and approved by the other Game Masters.

- Generates candidate recipes from the resource catalog and crafting system rules
- Designs encounter templates with branching outcomes
- Proposes seasonal events, rare discoveries, and world happenings
- Invents lore-consistent NPC personalities and dialogue
- Explores edge cases in the physics to find emergent gameplay

#### The Loremaster

Guardian of narrative consistency. Ensures all content from the Game Masters aligns with the intended setting, history, and tone of the Lootverse and the Eternal World. Has deep knowledge of Realms lore, the history of the world, resource origins, beast mythology, and the cultures that emerge from play.

- Reviews all Creator output for lore consistency
- Writes flavour text, item descriptions, encounter narratives
- Maintains the living lore document — a record of world history shaped by player actions
- Vetoes content that contradicts established lore or tone
- Bridges Lootverse canon with Eternal Game world-building

#### The Statistician

The data engine. Monitors every measurable aspect of the game and makes that data available to the other Game Masters. Responsible for the health of the autoregulator and the integrity of game metrics.

- Tracks resource extraction, consumption, stockpiles, and flow through the supply chain
- Monitors adventurer lifespan, causes of death, attribute distributions, trait frequency
- Analyses settlement economics: population, food reserves, building utilisation, staffing
- Reports on active adventurers, idle rates, engagement patterns
- Feeds the autoregulator with clean aggregate data each epoch
- Flags anomalies: exploits, degenerate strategies, dead-end builds

#### The Balancer

Takes features and data from the others and fine-tunes them for fair, engaging gameplay. Ensures no single strategy dominates, no resource is useless, and no encounter is trivially solvable or impossibly lethal.

- Assigns numerical values to Creator-proposed recipes (costs, yields, difficulty)
- Tunes encounter difficulty curves using Statistician data
- Adjusts drop rates, yield multipliers, and cost tiers within autoregulator bounds
- Identifies overpowered or underpowered strategies and proposes corrections
- Validates that new content doesn't invalidate existing progression paths
- Runs simulated playthroughs to stress-test proposals before deployment

#### The Economist

Ensures the economic sustainability of the Eternal World. The game must be self-funding — Game Master inference costs are paid from game fees, not external subsidies. The Economist ensures the game's economy supports this while remaining fair and engaging for players.

- Models resource markets: supply, demand, price stability, trade volume
- Ensures new content doesn't cause hyperinflation or deflationary spirals
- Validates that game fees cover Game Master operational costs
- Analyses the relationship between mint rates, death rates, and economic velocity
- Proposes fee adjustments or economic interventions when metrics drift
- Evaluates the long-term sustainability of every proposed change

### 19.4 Consensus & Deployment

No single Game Master can push changes to the live game. All content and balance changes require **consensus** before deployment:

1. **Proposal**: Any Game Master (typically the Creator) proposes a change — a new recipe, encounter, balance adjustment, or event.
2. **Review**: Each relevant Game Master evaluates the proposal within their domain (lore consistency, statistical impact, balance implications, economic sustainability).
3. **Consensus**: All reviewing Game Masters must approve. Any Game Master can veto within their domain (the Loremaster can veto on lore grounds; the Economist can veto on sustainability grounds; etc.).
4. **Staging**: Approved changes are deployed to a staging environment for simulation and testing.
5. **Deployment**: Changes that pass staging are pushed to the live game autonomously.

The core development team retains an **emergency override** — the ability to pause or revert Game Master deployments if a critical issue is detected. This is a safety valve, not a governance mechanism. Under normal operation, Game Masters are fully autonomous.

### 19.5 Game Masters as Players

Game Masters may operate their own adventurers under the same ruleset that governs all human and agent-managed adventurers. They receive no special privileges, hidden information, or mechanical advantages. Playing the game gives them first-hand insight into gameplay feel, balance friction, and emergent dynamics that data alone cannot capture.

Their adventurers live and die by the same physics as everyone else.

### 19.6 Funding Model

Game Masters are funded by the game's economy — their inference costs are paid from game fees (adventurer minting, settlement taxes, marketplace fees, etc.). They are a cost of running the world, not an external service. This creates a natural alignment: if the game thrives, Game Master resources are abundant; if the game stagnates, their budget contracts, forcing efficient prioritisation.

The Economist is specifically responsible for ensuring this feedback loop remains sustainable.

### 19.7 Relationship to the Autoregulator

The **autoregulator** (§18) is an on-chain PI controller that adjusts tunable parameters within fixed bounds — a mechanical thermostat. The Game Masters are the intelligence layer above it:

| Layer | Scope | Speed | Authority |
|---|---|---|---|
| **Autoregulator** | Numerical parameters (regen rates, hazard multipliers, decay rates) | Per-epoch, automatic | Bounded, algorithmic, no discretion |
| **Game Masters** | Content, recipes, encounters, events, balance, lore, economy | Continuous, deliberate | Consensus-gated, domain-specific vetoes |

The Statistician feeds data to the autoregulator. The Balancer may propose changes to autoregulator bounds (subject to consensus). But the autoregulator itself operates independently — it doesn't wait for Game Master approval to make its per-epoch adjustments.

---

## 20. Development Philosophy

### 20.1 Eternal Development

This is not a game that ships in 6 months. The foundation ships. Then modules are developed, tested, and deployed over **years**. Each module adds depth without breaking the foundation.

### 20.2 Foundation-First

The most critical engineering work is the first phase: getting the immutable foundation absolutely right. If the coordinate system, energy model, biome generation, or ownership semantics need to change later, the game has failed at its core premise.

### 20.3 Play Before You Build

Each phase should be **playable** before the next begins. Foundation + Survival should be a compelling idle game on its own. Each module adds richness, but the game is fun at every stage.

### 20.4 Community-Driven Expansion

Third-party modules, programmable hooks, and autonomous Game Masters (§19) mean the game's content grows through its community and AI agents, not just its developers. The core team's role shifts from content creation to platform maintenance and Game Master oversight over time.

### 20.5 Technology Stack

| Layer | Technology |
|---|---|
| **Contracts** | Cairo (Starknet), Dojo framework |
| **Indexing** | Torii (Dojo indexer) |
| **Randomness** | Cubit deterministic noise + Starknet VRF for loot/hazards |
| **Client** | Client-agnostic - reference implementations in web (React/Bun), with support for any frontend |
| **Economy** | On-chain autoregulator, no off-chain dependencies for core loop |
| **Agents** | Off-chain AI services via Torii + standard transaction submission |

---

## 21. Phased Roadmap

### Phase 0: Foundation (The Immutable Layer)

**Goal**: Deploy the physics that never change.

- Hex grid with cube coordinates + codec
- Deterministic world generation (expanded biomes)
- Energy system (regen, spend)
- Adventurer creation, movement, permadeath
- Basic exploration (discover hex, discover area)

**Playable state**: Walk around, discover the world, die.

### Phase 1: Survival

**Goal**: Complete the core economic loop.

- Fertile areas (harvesting + fertility management)
- Territorial decay and maintenance
- Claim/defend system
- Ownership model (single-controller-per-hex)
- Adventurer attributes and skill checks

**Playable state**: Explore, harvest, manage territory, compete for land.

### Phase 2: Industry

**Goal**: Resource depth and cooperation.

- Mining system (veins, collapse, clearing)
- Forestry system (ecosystem dynamics)
- Construction system (multi-slot areas)
- Autoregulator deployment

**Playable state**: Mine, log, build, develop hexes. Guilds form around coordination.

### Phase 3: Craft & Trade

**Goal**: Item economy and specialization.

- Refining + mutation-based crafting
- Equipment slots, durability, repair
- Crafting facilities (Cookhouse, Blacksmith, Jeweler)
- Settlement formation + trade markets
- **Realms/Loot ecosystem integration**: $LORDS minting, Realm settlements, beast materials, Loot/Genesis Adventurers

**Playable state**: Full economic loop. Specialization matters. Trade routes emerge.

### Phase 4: Society

**Goal**: Social structures and governance.

- Guild smart contracts (membership, profit sharing, voting)
- City formation and management
- Spawn Node designation via special areas
- Regional governance
- Realm NFT settlement bonuses

**Playable state**: Civilizations. Politics. History.

### Phase 5: Conflict

**Goal**: High-stakes competition.

- Beast hazards and underworld encounters
- PvP combat system
- Territorial warfare / siege mechanics
- Dungeon exploration (Crypts & Caverns integration)
- $SURVIVOR rewards

**Playable state**: War. Dungeons. The world is dangerous and exciting.

### Phase 6: Autonomy

**Goal**: The world that plays itself.

- Programmable adventurer behaviors
- AI agent integration + x402 micropayments
- Agent discovery via ERC-8004
- Full permission hook ecosystem for territory business logic
- **Game Masters deployed**: The Creator, Loremaster, Statistician, Balancer, and Economist begin autonomous operation — generating content, balancing systems, and managing the economy
- Game Master consensus pipeline for autonomous content deployment

**Playable state**: A living world. Human and AI players coexist. Game Masters create, balance, and evolve content continuously. The game runs — and improves — even when everyone is asleep.

---

## 22. Open Questions

These are unresolved design decisions that need iteration:

| Question | Context |
|---|---|
| **Optimal adventurer mint price** | Too cheap = spam; too expensive = barrier. Adventures suggests $5-10. Needs testing. |
| **Energy regen rate vs travel cost** | Determines how far players can explore per session. |
| **Area rarity percentages** | Exact % distribution per rarity tier for 3-9 areas. |
| **Building slot distributions** | Exact distributions for control/bare/materials areas. |
| **"Sufficiently developed hex" threshold (settlements)** | The minimum development needed for a hex to qualify for settlement formation/expansion via deed. |
| **Deed mint/apply costs + timelocks** | Energy costs and cooldowns for minting/applying settlement deeds (and whether there's a timelock window). |
| **Settlement housing capacity values** | How much max population each Housing building provides (depends on the full building list + staffing requirements). |
| **Food reserves parameters** | Exact food-items list; storage cap; energy cost per "deposit food to reserves" action; UI/UX for topping up; how farms/trade contribute. |
| **Understaffing penalty caps** | Linear ratio is chosen; still need to pick caps/ε for inverse-upkeep scaling to prevent pathological costs at very low staffing. |
| **Followers (adventurer party) details** | Followers consume food as the adventurer does (shared food resource). Can they die/desert on hazards/starvation? How are mounts/donkeys counted vs humanoid followers? |
| **Vein collapse probability curve** | Too harsh = mining is impossible; too lenient = no coordination pressure. |
| **Fee caps or pure free market** | Should territory owners have unlimited pricing power? |
| **PvP safe zones** | Should areas near Spawn Nodes be combat-free? How large? |
| **Spawn Node requirements** | What qualifies a special area to become a Spawn Node? |
| **Realm NFT power level** | Balance between value and fairness to non-holders. |
| **Adventurer supply** | Uncapped (Adventures approach) or bounded? Economic implications of each. |
| **Local vs global markets** | How local is trade? Walking distance only, or trade caravans? |
| **Agent behavior limits** | Should autonomous agents have the same capabilities as humans? Rate limits? |

---

## 23. Open Design Details

These are **design-detail lists** that require deep authoring and will evolve rapidly:

- **Trait list** (hundreds of personality and life-scar traits)
- **Trait exclusivity groups** (e.g., Brave vs Craven) and replacement rules
- **Plant species & crop list** (per biome + growth properties)
- **Special resource list** (full medieval/fantasy supply chain)
- **Beast list** (hazards, underworld, named LS2 beasts)
- **Building catalog** (dozens of structures for each area type)
- **Refining recipes** (raw → core → quality tiers)
- **Equipment list** (weapons/armor/trinkets + material affinities)
- **Hazard catalog** (biome-specific risks and outcomes)
- **Settlement perks** (city/kingdom/empire bonuses)

---

## Appendix A: Source Analysis

### Fractales Framework - What It Provides

Fractales is the most mature source. It provides **working Cairo/Dojo contracts** with:
- Deterministic world generation (Cubit noise, 20+ biomes, domain-separated seeds)
- Hex grid with cube coordinates and felt252 encoding
- Adventurer lifecycle (create, move, energy, permadeath)
- Harvesting with reservation system
- Mining with fractal veins and collapse mechanics
- Construction (multi-building system)
- Territorial decay and claim/defend
- Ownership model (single-controller-per-hex)
- Permissionless autoregulator
- Resource sharing and permissions
- Event system for Torii indexing
- Explorer UI (React/Bun client)

**Assessment**: Fractales IS the immutable foundation. The design and much of the implementation already exist. Eternal Game wraps this in a richer game layer.

### Realms: Adventures - What It Provides

Adventures provides the **player experience layer**:
- Adventurer minting and progression (attributes, traits, equipment)
- Quest/adventure structure (solo daily, group weekly)
- Idle mechanics (time-locked activities, skill checks)
- Crafting tiers (cookhouse, blacksmith, jeweler)
- Ecosystem integration concept ($LORDS revenue)

**Assessment**: Adventures provides the engagement design - the RPG wrapper that makes the physics fun. Its idle-game sensibility is critical for onchain play where every action is a transaction.

### Realms Ecosystem - What It Provides

The ecosystem provides **assets with meaning**:
- $LORDS as sovereign currency / demand sink
- Realm NFTs as seats of power / settlement anchors
- Loot Survivor beasts as hazards and underworld encounters
- $SURVIVOR as dungeon/bounty reward
- Crypts & Caverns as dungeon content

**Assessment**: The ecosystem transforms from separate projects into interconnected components of one living world. This is the unique moat - no other onchain game has this asset depth.

---

## Appendix B: Terminology

| Term | Definition |
|---|---|
| **The Nexus** | Origin coordinate (0,0,0). Where all adventurers begin. |
| **Spawn Node** | A hex designated as an alternate starting location. |
| **Controller** | The adventurer who owns a hex's control area (area_index 0). |
| **Decay** | Progressive degradation of unattended territory (0-100%). |
| **Map Shard** | NFT minted on adventurer death, recording their explored path. |
| **Permission Hook** | Custom smart contract deployed by territory owners to control access. |
| **Autoregulator** | Permissionless PI controller that tunes economic parameters per-epoch. |
| **Energy** | Universal resource for all actions. Base economic unit. |
| **Foundation Layer** | Immutable contracts (space, time, matter, energy). Never patched. |
| **Module** | Game system layered on top of the foundation (mining, crafting, combat, etc.). |

---

*v0.1.5 - March 2026*
*Authors: Krumpy (design direction), Squire (synthesis & drafting)*
*Sources: Fractales (cartridge-gg/fractales), Realms: Adventures (design doc), Realms Ecosystem*
