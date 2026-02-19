# Eternal Game — Design Scope v0.1

> **Working title.** This document synthesizes the Fractales framework (Cartridge/Dojo), the Realms: Adventures idle-RPG concept, and the Realms ecosystem into a unified thesis for an eternal, fully onchain MMORPG. It is a living document — intended to be challenged, refined, and expanded over many iterations.

---

## Table of Contents

1. [Thesis](#1-thesis)
2. [Design Pillars](#2-design-pillars)
3. [The Immutable Foundation Layer](#3-the-immutable-foundation-layer)
4. [World Topology & Hex Grid](#4-world-topology--hex-grid)
5. [The Adventurer](#5-the-adventurer)
6. [Core Gameplay Loop](#6-core-gameplay-loop)
7. [Exploration & The Fog](#7-exploration--the-fog)
8. [Resource Systems](#8-resource-systems)
9. [Crafting & Item Economy](#9-crafting--item-economy)
10. [Settlement & Construction](#10-settlement--construction)
11. [Territorial Sovereignty](#11-territorial-sovereignty)
12. [The Energy Economy](#12-the-energy-economy)
13. [Permadeath & Legacy](#13-permadeath--legacy)
14. [Realms Ecosystem Integration](#14-realms-ecosystem-integration)
15. [Modular Game Layers](#15-modular-game-layers)
16. [Social Systems & Governance](#16-social-systems--governance)
17. [Agent & Autonomous Play](#17-agent--autonomous-play)
18. [Autoregulation & Anti-Inflation](#18-autoregulation--anti-inflation)
19. [Development Philosophy](#19-development-philosophy)
20. [Phased Roadmap](#20-phased-roadmap)
21. [Open Questions](#21-open-questions)

---

## 1. Thesis

**Eternal Game is a fully onchain MMORPG built on an immutable physics substrate, designed to run forever.**

The ambition is not to ship a game and move on. It is to deploy a world with physics so fundamental — time, space, matter, energy — that they never need to change. Everything else — quests, economies, societies, wars, culture — emerges from players, builders, and autonomous agents interacting with those physics over years and decades.

The world is an infinite hex grid. Players pay to spawn adventurers who explore outward from a central origin, discovering terrain, harvesting resources, building settlements, forming societies, and eventually dying — permanently. What they leave behind shapes the world for everyone who follows.

Three source pillars inform this design:

- **Fractales** (Cartridge): An infinite hex exploration framework with deterministic world generation, energy economics, territorial decay, and permadeath — implemented in Cairo/Dojo with working contracts.
- **Realms: Adventures**: An idle-RPG concept focused on adventurer minting, questing, crafting, and integration with Eternum/Loot ecosystem assets.
- **The Realms Ecosystem**: Realms NFTs, Loot Survivor, Beasts, Crypts & Caverns, $LORDS, $SURVIVOR — a rich asset layer waiting for a world to inhabit.

Eternal Game fuses these into one coherent world.

---

## 2. Design Pillars

| Pillar | Meaning |
|---|---|
| **Immutable Physics** | The base layer (coordinate system, energy, time, matter properties, biome generation) ships once and is never patched. Scarcity is provable. |
| **Infinite World** | The hex grid is unbounded. There is always more to explore. Expansion is driven by players, not content drops. |
| **Permadeath & Consequence** | Adventurers die permanently. Decisions carry weight. Legacy matters. |
| **Energy as Universal Currency** | Energy is the atomic unit of all economic activity — movement, harvesting, crafting, maintenance, combat. Everything costs energy; everything can be converted back to energy. |
| **Territorial Sovereignty** | Discovery creates property rights. Ownership demands upkeep. Neglected territory decays and becomes claimable. There is no free land — only maintained land. |
| **Headless & Client-Agnostic** | All state lives in smart contracts. Any front-end (3D, pixel, text, Discord bot, autonomous agent) can render and interact with the same world. |
| **Modular Extensibility** | The base layer supports an arbitrary number of game modules (mining, crafting, combat, diplomacy, research, trade) layered on top over time. Each module integrates with the same energy/ownership/action framework. |
| **Ecosystem Integration** | Realms assets ($LORDS, Realm NFTs, Beasts, Loot, C&C) are first-class citizens — not cosmetic overlays, but functional components of the world's economy and lore. |

---

## 3. The Immutable Foundation Layer

This is the "physics engine" — the contracts that, once deployed, **never change**. All iterative game development happens in modules that interface with this layer.

### 3.1 Time

- Time is measured in **blocks**. All game mechanics (movement, harvesting, decay, regeneration) operate on block deltas.
- No real-world clock dependency. The world ticks with the chain.
- Lazy evaluation: state changes are computed on-demand from the last checkpoint block, making the system gas-efficient and deterministically replayable.

### 3.2 Space

- **Infinite hex grid** using cube coordinates `(x, y, z)` centered at origin `(0, 0, 0)`.
- Coordinate range: ±2.3 × 10¹² — effectively infinite.
- Storage uses felt252 encoding via a deterministic codec; gameplay API uses signed origin-centered coordinates.
- **Adjacent-only movement** — no teleportation. Distance is real. Geography is strategy.
- Each hex contains multiple **areas** (sub-locations): control areas, plant fields, mine fields, wilderness.

### 3.3 Matter (World Generation)

- The entire world is **deterministically generated** from a single global seed using Cubit noise functions.
- `hash(global_seed, coordinate)` → biome, area count, area types, resource profiles, plant species, ore veins.
- Domain-separated seed derivation prevents cross-stage leakage (`HEX_V1`, `AREA_V1`, `PLANT_V1`, `GENE_V1`).
- Once a hex/area/plant is materialized (discovered), its properties are immutable — no balance patches, no retroactive nerfs.
- **20 canonical biomes**: Plains, Forest, Mountain, Desert, Swamp, Tundra, Taiga, Jungle, Savanna, Grassland, Canyon, Badlands, Volcanic, Glacier, Wetlands, Steppe, Oasis, Mire, Highlands, Coast.
- Biome clustering is coherent (simplex noise produces natural terrain transitions), but contents within hexes are seed-derived and unique.

### 3.4 Energy

- Energy is the **universal resource** — the base currency of all action.
- Every adventurer has an energy pool that regenerates lazily per-block, capped at max.
- All actions consume energy: movement, exploration, harvesting, mining, crafting, construction, territorial maintenance.
- All items can be converted back to energy (universal floor value).
- Energy is the substrate upon which all economic activity rests.

---

## 4. World Topology & Hex Grid

```
Origin (0,0,0) — The Nexus
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

All adventurers begin at The Nexus — coordinate `(0,0,0)`. This is the eternal starting point and the heart of the world.

As players explore outward, the discovered world expands like a living organism. The frontier is always at the edges, and the core grows denser with settlements, trade routes, and history.

### Spawn Nodes

Over time, as exploration radiates outward, specific hexes can be designated as **Spawn Nodes** — secondary starting locations that allow new players to begin further from the origin. Spawn Nodes emerge through gameplay achievement (e.g., building a settlement of sufficient size/tier, or community challenge completion). This creates a **node-based expansion pattern**: clusters of civilization connected by explored corridors through wilderness.

### Hex Contents

Each hex has:
- A **biome** (deterministic from noise)
- An **area count** (deterministic from seed, typically 3-8 areas)
- A **control area** (area_index 0) — ownership of this = control of the hex
- **Resource areas** (plant fields, mine fields) — each with deterministic resource profiles
- **Building slots** — one per discovered area

---

## 5. The Adventurer

The adventurer is the player's avatar in the world. It is an NFT-like entity (model parity, with ERC-721 wrapping planned) with the following properties:

### Core Attributes

| Attribute | Role |
|---|---|
| **Energy** | Fuel for all actions. Regenerates per-block up to cap. |
| **Position** | Current hex coordinate. Movement is adjacent-only. |
| **Backpack** | Weight-limited inventory. Carry capacity varies by stats/gear. |
| **Activity Lock** | Time-locked commitment to current action (harvesting, mining, building). |
| **Alive** | Boolean. Once false, forever false. |

### Traits (from Adventures concept, expanded)

Drawing from the Adventures idle-RPG design, adventurers have **10 core traits** that affect all interactions:

| Category | Traits |
|---|---|
| **Physical** | Strength, Endurance, Agility, Vitality |
| **Mental** | Intelligence, Wisdom, Charisma |
| **Specialized** | Survival, Craftsmanship, Leadership |

- Traits are randomized at mint (within bounds).
- Traits improve through use — **learning by doing**. Mining improves relevant physical traits; crafting improves craftsmanship; leading groups improves leadership.
- Trait values gate certain activities: heavy armor requires minimum strength; advanced crafting requires minimum craftsmanship + intelligence.
- Synergies exist: high craftsmanship + intelligence = innovation bonuses on crafting outcomes.

### Equipment

- Adventurers have equipment slots (head, body, hands, feet, weapon, accessory).
- Equipped gear modifies traits and provides bonuses.
- Gear has **durability** that decreases with use. Broken gear must be repaired or replaced.
- Gear weight counts against backpack capacity.

### Stamina (from Adventures)

- Adventurers have a stamina bar that decreases after action attempts.
- Longer/harder actions drain more stamina.
- Stamina replenishes through rest (time-locked idle action) or food consumption.
- Low stamina reduces effectiveness of all actions (skill check penalties, reduced yields).

---

## 6. Core Gameplay Loop

The fundamental loop, from which everything else emerges:

```
SPAWN → EXPLORE → DISCOVER → GATHER → CONVERT → MAINTAIN → EXPAND → (DIE → LEGACY)
         ↑                                                      |
         └──────────────────────────────────────────────────────┘
```

1. **Spawn**: Pay to mint an adventurer at a Spawn Node (Nexus or unlocked secondary).
2. **Explore**: Move through adjacent hexes, revealing biomes and discovering areas. Each step costs energy.
3. **Discover**: First visitor to a hex/area materializes its deterministic contents. Discovery creates ownership rights.
4. **Gather**: Harvest plants, mine ores, hunt creatures. Time-locked activities with energy costs.
5. **Convert**: Process raw materials into usable resources. Convert items to energy. Craft gear and consumables.
6. **Maintain**: Pay territorial upkeep (energy) to prevent decay. Defend territory from claimants.
7. **Expand**: Build settlements. Develop infrastructure. Establish trade routes. Form societies.
8. **Die**: Permadeath. Adventurer is gone. Legacy remains.

### Idle Mechanics (from Adventures)

Not every action requires real-time attention. Drawing from the Adventures concept:

- **Solo adventures** refresh daily — shorter, lower stakes, individual play.
- **Group adventures** refresh weekly — require multiple players, higher difficulty and reward.
- **Activity locks** mean players commit adventurers to tasks and return later to collect results.
- **Skill checks** determine success/failure during adventures, based on traits + gear + stamina.

This creates a natural **idle game** rhythm: commit your adventurer(s) to actions, check back later for results, make strategic decisions about what to do next.

---

## 7. Exploration & The Fog

The world starts as an infinite fog. Only The Nexus is known. Everything else must be discovered by adventurers walking into the unknown.

### Fog of War

- Undiscovered hexes show nothing — not even biome type (in the full version; MVP reveals biome from noise).
- Future enhancement: **commit-reveal** scheme where players commit to exploring a hex, then reveal it — preventing map-scraping by third parties.

### Discovery Rights

- The first adventurer to discover a hex becomes its **discoverer** — recorded permanently.
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

### 8.1 Harvesting (Plants)

- Each area contains **plant nodes** with deterministic species, yield, regrowth rate, and genetics.
- Harvesting is time-locked and reservation-based (prevents over-commitment by concurrent harvesters).
- Plants regrow per-block unless harvested to 0 yield → **permanent extinction** of that node.
- Stress accumulates from aggressive harvesting, reducing future yields.
- Different biomes produce different plant species with varying properties.

### 8.2 Mining (Ores)

- **Fractal vein system**: veins exist at varying depths within mine-field areas.
- Deeper veins = higher grade ore, higher rarity, but greater hazards.
- Tool tiers gate hardness levels — you can't mine deep veins with a stone pick.
- Veins regenerate slowly but can **collapse** when over-mined → permanent destruction.
- Mining is a **social coordination problem** (prisoner's dilemma): veins support a limited number of miners safely. Over-mining causes collapse, destroying the vein and killing miners inside.
- This creates natural pressure for **guilds, cooperatives, and access control**.

### 8.3 Hunting & Creatures (Future Module)

- Biome-specific creatures with trait-based encounters.
- Loot Survivor beasts integrated as world encounters (see §14).

### 8.4 Resource Properties

All resources have deterministic properties derived from their generation seed:
- **Plants**: growth rate, nutritional value, yield, mutation seed, stress tolerance.
- **Ores**: grade (purity), hardness, regen rate, mutation seed, collapse risk.
- These properties feed into crafting mutations and economic valuations.

---

## 9. Crafting & Item Economy

### 9.1 Mutation-Based Crafting

Drawing from both Fractales and Adventures:

```
craft(inputs[]) → new_item
new_properties = f(input_properties, XOR_seed)
```

- Any resources can be combined. Outputs inherit blended stats from inputs.
- A **mutation function** introduces controlled randomness — crafted items are unique.
- Legendary rolls occur when property products cross defined thresholds.
- Higher craftsmanship + intelligence traits improve mutation outcomes.

### 9.2 Tiered Crafting (from Adventures)

Three crafting facility types (map to construction buildings):

| Facility | Produces | Key Inputs |
|---|---|---|
| **Cookhouse** | Food consumables (stamina recovery, buffs) | Plants, hunted materials |
| **Blacksmith** | Weapons and armor | Ores, smelted bars, alloys |
| **Jeweler** | Rings, necklaces, imbued gear (trait bonuses) | Rare ores, gems, plant compounds |

### 9.3 Cross-Ecosystem Crafting

- **Lower-tier** items can be crafted purely from in-game resources.
- **Higher-tier** items require a combination of in-game AND Eternum/ecosystem resources.
- Example: `Beef + Wheat (from Eternum) → Hearty Stew` (superior stamina recovery).
- Example: `Iron Bar + $LORDS burn → Lordly Blade` (trait-boosted weapon).
- This creates a **pull** on ecosystem assets, driving demand and creating economic bridges.

### 9.4 Item Durability & Sinks

- All crafted gear has durability that degrades with use.
- Repair requires resources (creating ongoing demand).
- Broken gear is useless until repaired.
- Items can always be **converted to energy** (universal sink + floor value).

---

## 10. Settlement & Construction

### 10.1 Building System

From the Fractales construction module, 7 building types per hex:

| Building | Function |
|---|---|
| **Smelter** | Ore → bar conversion with efficiency bonus |
| **Shoring Rig** | Reduces mine collapse risk in hex |
| **Greenhouse** | Improves plant regrowth rates |
| **Herbal Press** | Improves plant-to-energy conversion |
| **Workshop** | Reduces build costs and times |
| **Storehouse** | Increases storage/carry capacity |
| **Watchtower** | Defense bonus in territorial disputes |

- One building slot per discovered area.
- Buildings require ore + plant-derived materials to construct.
- Buildings require ongoing upkeep (energy + materials) or they become inactive.
- Building control transfers with hex control during claim/defend.

### 10.2 Settlements & Cities (Vision)

Beyond individual buildings, when hexes develop sufficient infrastructure:

- A cluster of developed hexes can form a **Settlement**.
- Settlements unlock higher-tier crafting, trade facilities, and social coordination tools.
- Settlements can become **Cities** with:
  - **Market Halls** for player-to-player trade.
  - **Claimable lots** for player-owned facilities (smithy, inn, etc.) — generating fee revenue.
  - **Guild headquarters** for coordinated play.
- Cities can be designated as **Spawn Nodes**, allowing new adventurers to start there.

### 10.3 Land Claiming & Development

- Players don't just extract — they **build**.
- Claiming an area means committing to its maintenance.
- Development progression: `Raw Territory → Developed Hex → Settlement → City → Spawn Node`.
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
- **Distance penalty**: remote hexes cost more to maintain.
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
3. If undefended, the claimant takes control — all area ownership transfers.
4. Claim escrow is consumed on success; refunded on failure/expiry.

This creates **natural territorial limits** — players can only hold as much territory as they can maintain. Empires require economic sustainability, not just military power.

---

## 12. The Energy Economy

### 12.1 Sources

- Per-block lazy regeneration (base income).
- Converting items to energy (universal conversion).
- Trade with other players.
- Revenue from owned territory (fees from other players' activities).

### 12.2 Sinks

- Movement (energy per hex).
- Exploration (energy per discovery).
- Harvesting, mining, crafting (action costs).
- Territorial maintenance (ongoing upkeep).
- Construction (material + energy costs).
- Death (all energy lost with adventurer).

### 12.3 Dynamic Conversion Rates

- Converting items to energy uses **dynamic rates** based on supply/demand.
- Recent conversion volume applies a **rolling penalty** (prevents mass dumping).
- Formula: `penalty_bp = min(5000, floor(units_in_window / 10) * 100)` — max 50% penalty.
- This creates price discovery and prevents trivial arbitrage.

### 12.4 Autoregulation

A **permissionless autoregulator** (from Fractales) tunes economic parameters per-epoch:
- Target: ~10% baseline inflation over 8-week horizons.
- Anyone can trigger the epoch tick (keeper bounty incentive).
- PI controller with clamps, deadbands, and slew limits — no operator intervention needed.
- Adjusts conversion rates, upkeep costs, and reward modifiers to maintain economic health.

---

## 13. Permadeath & Legacy

### 13.1 True Permadeath

When an adventurer dies (0 HP, starvation, mine collapse, hazard):
- The adventurer NFT is **burned**. Gone forever.
- All carried inventory is lost.
- Active reservations/escrows are settled and released.
- No resurrection. No appeals.

### 13.2 Death Causes

- **Starvation**: Energy reaches 0 with no way to recover.
- **Mine collapse**: Over-mining triggers structural failure.
- **Hazards**: Biome-specific events (cave-ins, sandstorms, creature attacks) via VRF.
- **Future**: PvP combat, territorial warfare.

### 13.3 Legacy

Death is not the end of impact:
- **Map Shards**: On death, a **Map Shard NFT** is minted, recording the adventurer's explored path. Map Shards become historical artifacts and may have future utility (unlocking lore, providing exploration bonuses to successors).
- **Discovered territory persists**: Everything the adventurer discovered, built, and developed remains in the world — but ownership must be maintained by someone else or it decays.
- **Reputation/history**: The adventurer's deeds are recorded on-chain forever. Discoverer tags on hexes persist. Legacy is the real endgame.

### 13.4 Respawn

After death, the player mints a new adventurer (paying the spawn cost again). The new adventurer starts fresh at a Spawn Node — but in a world shaped by the old one's actions.

---

## 14. Realms Ecosystem Integration

This is where Eternal Game becomes uniquely powerful. The Realms ecosystem provides a rich asset layer that transforms from speculative collectibles into **functional world components**.

### 14.1 $LORDS — The Sovereign Currency

- **Adventurer minting** costs $LORDS (primary revenue sink and demand driver).
- Revenue from minting can be: burned, sent to stakers, or directed to the DAO treasury.
- $LORDS can be spent in-game for premium crafting recipes, facility upgrades, and settlement establishment.
- $LORDS acts as the **meta-economic layer** above the in-game energy economy — the currency of sovereigns, not adventurers.

### 14.2 Realm NFTs — The Seats of Power

- Realm NFT holders gain **special settlement rights**: the ability to establish a **Realm Settlement** — a uniquely powerful type of city with:
  - Bonus resource generation based on the Realm's original attributes (resource types from the Realm NFT map to in-game biome affinities).
  - Unique building types unavailable to non-Realm settlements.
  - Governance voting power in regional politics.
- Realms become **anchors of civilization** in the infinite world — permanent points of power tied to real NFT ownership.
- A Realm must still be maintained (upkeep) — ownership alone is not enough. An inactive Realm's settlement benefits degrade.

### 14.3 Loot Survivor Beasts — World Encounters

- Beasts from Loot Survivor inhabit the world as **dynamic encounters** in specific biomes.
- Beast encounters provide high-risk, high-reward combat challenges.
- Defeating beasts yields rare crafting materials, beast trophies, and potentially **$SURVIVOR** tokens.
- Beast difficulty scales with distance from origin and biome danger level.
- Legendary beasts are rare, powerful, and drop unique items.

### 14.4 Crypts & Caverns — Dungeon Delving

- Crypts & Caverns maps as **discoverable dungeon entrances** in the hex grid.
- Entering a dungeon opens a deeper exploration layer — rooms within rooms, fractal depth.
- Dungeons contain unique ores, rare plants, ancient artifacts, and dangerous hazards.
- Dungeon discovery and exploration follows the same energy/time/permadeath rules as surface play.
- C&C NFT holders may gain dungeon-related bonuses (navigation, loot quality, trap avoidance).

### 14.5 $SURVIVOR — The Adventurer's Token

- $SURVIVOR integrates as a **combat and survival reward token**.
- Earned through: defeating beasts, surviving dangerous biomes, completing community challenges.
- Spent on: combat consumables, survival gear, beast-related crafting recipes.
- Creates economic bridge between Loot Survivor players and Eternal Game.

### 14.6 Cross-Game Economy

The key principle: **higher-tier activities require cross-ecosystem resources**.

| Tier | Resources Needed |
|---|---|
| Basic | In-game only (plants, ores, energy) |
| Advanced | In-game + Eternum resources (wheat, coal, etc.) |
| Premium | In-game + $LORDS burn |
| Legendary | In-game + Realm bonuses + beast materials + $LORDS |

This creates a **pull economy** where Eternal Game drives demand for the entire Realms ecosystem.

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
| **Survival** | Harvesting, Conversion, Decay, Claims | Core economic loop. Idle gameplay. |
| **Industry** | Mining, Construction, Plant Processing | Resource depth. Cooperation pressure. |
| **Craft** | Crafting, Equipment, Durability, Facilities | Item economy. Specialization. |
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
- Guild structures are programmable through smart contracts — profit sharing, voting, membership rules.

### 16.2 Trade

- Player-to-player trade through in-game markets (city facilities).
- No global auction house — trade is **local** (you must be in range of a market or trade partner).
- This creates natural **trade routes** and makes geography matter for commerce.
- Information asymmetry is real — knowing where resources are is valuable.

### 16.3 Governance

- Regional governance can emerge through settlement councils.
- Realm holders have special governance weight.
- Community challenges (from Adventures concept): **global burn events** where players collectively contribute resources to unlock new game zones, recipes, or features.
- Example: "A dragon attacked the eastern settlements. Contribute stone and iron to rebuild defenses." Completion unlocks a new dungeon entrance in that region.

---

## 17. Agent & Autonomous Play

### 17.1 Programmable Adventurers

Adventurers can be given autonomous behaviors through deployed smart contracts:
- **Trading bots**: Autonomous profit optimization, market making.
- **Mining coordinators**: Group formation, safety protocols, fair profit sharing.
- **Resource managers**: Automated harvesting, conversion, maintenance payment.
- **Diplomatic agents**: Alliance formation, conflict resolution.

### 17.2 AI Agent Integration

- Off-chain AI services can observe world state (via Torii indexer) and submit transactions on behalf of adventurers.
- This creates a world where both human players and autonomous agents coexist — the world is always active, even when humans are offline.
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
| **Universal energy conversion** | Everything has a floor value and can be recycled. |
| **Territorial decay** | Ownership is a continuous cost, not a one-time acquisition. |
| **Permadeath** | Wealth is periodically destroyed (along with the adventurer carrying it). |
| **Durability/repair** | Gear degrades, creating ongoing resource demand. |
| **Dynamic conversion rates** | Supply/demand-responsive pricing prevents market manipulation. |
| **Volume penalties** | Mass dumping is penalized, smoothing economic shocks. |
| **Progressive scaling** | Distance penalties and productivity taxes prevent infinite accumulation. |
| **Permissionless autoregulator** | PI controller adjusts parameters per-epoch without operator intervention. |

### 18.3 Scarcity From Player Decisions

The world doesn't have artificial scarcity — it has **player-created scarcity**:
- Over-harvest a plant → it goes extinct (permanently, for that node).
- Over-mine a vein → it collapses (permanently).
- Neglect territory → it decays and becomes claimable.
- Die → all your inventory is lost.

---

## 19. Development Philosophy

### 19.1 Eternal Development

This is not a game that ships in 6 months. The foundation ships. Then modules are developed, tested, and deployed over **years**. Each module adds depth without breaking the foundation.

### 19.2 Foundation-First

The most critical engineering work is the first phase: getting the immutable foundation absolutely right. If the coordinate system, energy model, biome generation, or ownership semantics need to change later, the game has failed at its core premise.

### 19.3 Play Before You Build

Each phase should be **playable** before the next begins. Foundation + Survival should be a compelling idle game on its own. Each module adds richness, but the game is fun at every stage.

### 19.4 Community-Driven Expansion

Community challenges, third-party modules, and programmable hooks mean the game's content grows through its community, not just its developers. The development team's role shifts from content creation to platform maintenance over time.

### 19.5 Technology Stack

| Layer | Technology |
|---|---|
| **Contracts** | Cairo (Starknet), Dojo framework |
| **Indexing** | Torii (Dojo indexer) |
| **Randomness** | Cubit deterministic noise + Starknet VRF for loot/hazards |
| **Client** | Client-agnostic — reference implementations in web (React/Bun), with support for any frontend |
| **Economy** | On-chain autoregulator, no off-chain dependencies for core loop |
| **Agents** | Off-chain AI services via Torii + standard transaction submission |

---

## 20. Phased Roadmap

### Phase 0: Foundation (The Immutable Layer)

**Goal**: Deploy the physics that never change.

- Hex grid with cube coordinates + codec
- Deterministic world generation (20 biomes, Cubit noise)
- Energy system (regen, spend, conversion)
- Adventurer creation, movement, permadeath
- Basic exploration (discover hex, discover area)

**Playable state**: Walk around, discover the world, convert plants to energy, die.

### Phase 1: Survival

**Goal**: Complete the core economic loop.

- Harvesting system (reservation-based, time-locked)
- Universal conversion (items → energy)
- Territorial decay and maintenance
- Claim/defend system
- Ownership model (single-controller-per-hex)
- Adventurer traits and skill checks

**Playable state**: Explore, harvest, manage territory, compete for land. Idle gameplay loop complete.

### Phase 2: Industry

**Goal**: Resource depth and cooperation.

- Mining system (fractal veins, collapse mechanics, social coordination)
- Construction system (7 buildings per hex)
- Plant material processing
- Autoregulator deployment

**Playable state**: Mine, build, develop hexes. Guilds form naturally around mining coordination.

### Phase 3: Craft & Trade

**Goal**: Item economy and specialization.

- Mutation-based crafting system
- Equipment slots, durability, repair
- Crafting facilities (Cookhouse, Blacksmith, Jeweler)
- Settlement formation + trade markets
- **Realms ecosystem integration**: $LORDS minting costs, cross-game crafting recipes, Eternum resource bridge

**Playable state**: Full economic loop. Specialization matters. Trade routes emerge.

### Phase 4: Society

**Goal**: Social structures and governance.

- Guild smart contracts (membership, profit sharing, voting)
- City formation and management
- Spawn Node designation
- Community challenges
- Realm NFT settlement bonuses
- Regional governance

**Playable state**: Civilizations. Politics. History.

### Phase 5: Conflict

**Goal**: High-stakes competition.

- Beast encounters (Loot Survivor integration)
- PvP combat system
- Territorial warfare / siege mechanics
- Dungeon exploration (Crypts & Caverns integration)
- $SURVIVOR rewards

**Playable state**: War. Dungeons. Dragons. The world is dangerous and exciting.

### Phase 6: Autonomy

**Goal**: The world that plays itself.

- Programmable adventurer behaviors
- AI agent integration + x402 micropayments
- Agent discovery via ERC-8004
- Full permission hook ecosystem for territory business logic

**Playable state**: A living world. Human and AI players coexist. The game runs even when everyone is asleep.

---

## 21. Open Questions

These are unresolved design decisions that need iteration:

| Question | Context |
|---|---|
| **Optimal adventurer mint price** | Too cheap = spam; too expensive = barrier. Adventures suggests $5-10. Needs testing. |
| **Energy regen rate vs travel cost** | Determines how far players can explore per session. |
| **Vein collapse probability curve** | Too harsh = mining is impossible; too lenient = no coordination pressure. |
| **Fee caps or pure free market** | Should territory owners have unlimited pricing power? |
| **Map Shard utility** | Beyond bragging rights — should they unlock exploration bonuses? Navigation? Lore? |
| **PvP safe zones** | Should areas near Spawn Nodes be combat-free? How large? |
| **Commit-reveal timing** | When does exploration shift from direct discovery to commit-reveal? |
| **Spawn Node requirements** | What does a settlement need to become a Spawn Node? |
| **Realm NFT power level** | How much advantage do Realm holders get? Balance between value and fairness to non-holders. |
| **Cross-chain resource bridging** | Technical approach for bringing Eternum resources into Eternal Game. |
| **Adventurer supply** | Uncapped (Adventures approach) or bounded? Economic implications of each. |
| **Community challenge design** | Frequency, scale, reward design. Who designs them — devs or DAO? |
| **Local vs global markets** | How local is trade? Walking distance only, or trade caravans over longer range? |
| **Agent behavior limits** | Should autonomous agents have the same capabilities as human players? Rate limits? |

---

## Appendix A: Source Analysis

### Fractales Framework — What It Provides

Fractales is the most mature source. It provides **working Cairo/Dojo contracts** with:
- Deterministic world generation (Cubit noise, 20 biomes, domain-separated seeds)
- Hex grid with cube coordinates and felt252 encoding
- Adventurer lifecycle (create, move, energy, permadeath)
- Harvesting with reservation system
- Mining with fractal veins and collapse mechanics
- Construction (7-building system)
- Territorial decay and claim/defend
- Ownership model (single-controller-per-hex)
- Universal energy conversion with dynamic rates
- Permissionless autoregulator
- Resource sharing and permissions
- Event system for Torii indexing
- Explorer UI (React/Bun client)

**Assessment**: Fractales IS the immutable foundation. The design and much of the implementation already exist. Eternal Game wraps this in a richer game layer.

### Realms: Adventures — What It Provides

Adventures provides the **player experience layer**:
- Adventurer minting and progression (traits, levels, equipment)
- Quest/adventure structure (solo daily, group weekly)
- Idle mechanics (time-locked activities, skill checks)
- Crafting tiers (cookhouse, blacksmith, jeweler)
- Ecosystem integration concept ($LORDS revenue, Eternum resources, Blobert aids)
- Community challenges (global burn events)
- Stamina/food systems

**Assessment**: Adventures provides the engagement design — the RPG wrapper that makes the physics fun. Its idle-game sensibility is critical for onchain play where every action is a transaction.

### Realms Ecosystem — What It Provides

The ecosystem provides **assets with meaning**:
- $LORDS as sovereign currency / demand sink
- Realm NFTs as seats of power / settlement anchors
- Loot Survivor beasts as world encounters
- $SURVIVOR as combat/survival reward
- Crypts & Caverns as dungeon content
- Eternum resources as cross-game crafting inputs

**Assessment**: The ecosystem transforms from a collection of separate projects into interconnected components of one living world. This is the unique moat — no other onchain game has this asset depth.

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

*v0.1 — February 2026*
*Authors: Krumpy (design direction), Squire (synthesis & drafting)*
*Sources: Fractales (cartridge-gg/fractales), Realms: Adventures (design doc), Realms Ecosystem*
