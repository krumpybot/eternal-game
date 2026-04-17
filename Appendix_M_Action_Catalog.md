# Appendix M: Action Catalog ⚠️


> ⚠️ **Flagged for full review.** Action costs, time-locks, and energy values are base estimates. All values require validation against the biome modifier system (Phase 4), production chains (Phase 6), and the time budget analysis (Phase 13). Cost tier assignments should be re-evaluated once movement and exploration mechanics are quantified (Phase 5).
> Complete catalog of player-initiated actions in the base module. Each entry defines the action's properties using the standard template (Phase 2b). Energy costs and time-locks are **draft ranges** — exact values to be locked in their respective quantification phases.
>
> **Energy budget reference** (Phase 1a): Base regen = 0.1/tick (~864/in-game day). Base max = 100. An adventurer regenerates enough energy for many light actions or a handful of heavy ones per in-game day. Time-locks prevent action spam — even with energy, the adventurer is occupied.

---

## Action Cost Tiers (draft framework)

Actions fall into rough cost tiers. These establish the economy of effort — how an adventurer allocates their energy and time budget across a day.

| Tier | Energy cost range | Time-lock range (ticks) | Time-lock (real time) | Examples |
|---|---|---|---|---|
| **Trivial** | 0–5 | 0–12 | 0–2 min | Resting |
| **Light** | 5–15 | 12–36 | 2–6 min | Cooking, Trading, Salvaging, Stocking |
| **Medium** | 15–30 | 36–108 | 6–18 min | Survey, Foraging, Fishing, Repairing, Recruiting |
| **Heavy** | 25–45 | 72–216 | 12–36 min | Travel, Farming, Refining, Mining, Logging, Hunting, Crafting, Herding |
| **Extreme** | 35–60 | 144–360 | 24–60 min | Explore, Building, Delve, Demolishing |

> These tiers are indicative, not prescriptive. Individual actions may sit between tiers depending on balance needs. Biome modifiers, traits, and equipment shift effective costs up or down from these baselines.

---

## 1. Movement & Exploration

### Travel

| Property | Value |
|---|---|
| **Category** | Movement & Exploration |
| **Energy cost** | 25–45 (scales with biome difficulty; reduced by survey completeness and infrastructure) |
| **Time-lock** | 72–216 ticks (12–36 min real; scales with biome) |
| **Success formula** | Automatic — travel always succeeds if energy is sufficient. Efficiency (cost reduction) scales with WIS and gear. |
| **Failure outcomes** | None (travel cannot fail). Encounter may trigger during travel (separate resolution). |
| **Attribute gain chances** | END (~1%), WIS (~0.5%) per travel action |
| **Skill trait trigger** | Pathfinder / Footsore (travel domain) |
| **Biome modifiers** | Per-biome cost multiplier (Phase 4). Water biomes impassable without future module. |
| **Tool/building requirements** | None. Mount reduces energy cost and time-lock. |
| **Encounter exposure** | Beast, social. Chance scales with biome wildness and hex development level. |

### Explore

| Property | Value |
|---|---|
| **Category** | Movement & Exploration |
| **Energy cost** | 35–60 (higher than Travel — the hex is unknown) |
| **Time-lock** | 144–360 ticks (24–60 min real) |
| **Success formula** | Automatic — hex is always revealed. Quality of discovery (areas found, hazards mapped) scales with WIS, DEX. |
| **Failure outcomes** | None (exploration always reveals the hex). But encounters triggered during exploration can injure/kill. |
| **Attribute gain chances** | WIS (~1.5%), DEX (~1%), SUR (~0.5%) |
| **Skill trait trigger** | Trailblazer / Lost (exploring domain) |
| **Biome modifiers** | Per-biome cost multiplier. Hazardous biomes have higher encounter chance. |
| **Tool/building requirements** | None. |
| **Encounter exposure** | Beast, combat, social, special. Higher encounter chance than travel — unknown territory. |

### Survey

| Property | Value |
|---|---|
| **Category** | Movement & Exploration |
| **Energy cost** | 15–30 |
| **Time-lock** | 36–108 ticks (6–18 min real) |
| **Success formula** | Automatic — survey always completes. Depth of information (node quality, special area detection) scales with WIS, SUR. |
| **Failure outcomes** | None. |
| **Attribute gain chances** | WIS (~1%), SUR (~0.5%) |
| **Skill trait trigger** | Cartographer / Disoriented (survey domain) |
| **Biome modifiers** | May vary by area density — more areas to scan = longer time-lock. |
| **Tool/building requirements** | None. |
| **Encounter exposure** | Low. Surveying is observational, not invasive. |

### Delve

| Property | Value |
|---|---|
| **Category** | Movement & Exploration |
| **Energy cost** | 35–60 (the most expensive exploration action) |
| **Time-lock** | 144–360 ticks (24–60 min real) |
| **Success formula** | Progress and loot scale with SUR, DEX. Deeper delves yield more but risk more. |
| **Failure outcomes** | Encounter damage, injury traits, potential death. Higher base danger than surface actions. |
| **Attribute gain chances** | SUR (~2%), DEX (~1.5%), STR (~1%) — higher base chances reflect higher risk |
| **Skill trait trigger** | No dedicated delve skill trait pair in base module. Multiple skill domains may trigger (combat, exploring). |
| **Biome modifiers** | Delve difficulty may scale with area type rather than biome. |
| **Tool/building requirements** | Recommended: weapon, armor, light source (future item types). |
| **Encounter exposure** | Beast, combat, special. Highest encounter chance of any action. Confined spaces amplify danger. |

---

## 2. Resource Gathering

### Mining

| Property | Value |
|---|---|
| **Category** | Resource Gathering |
| **Energy cost** | 25–40 |
| **Time-lock** | 72–180 ticks (12–30 min real) |
| **Success formula** | `yield = base_vein_yield × depth_multiplier × (1 + INT × 0.025) × tool_bonus`. Collapse avoidance: `avoid = base_stability × (1 + STR × 0.03)`. |
| **Failure outcomes** | Collapse: injury trait chance (Fractured, Bruised), health damage, vein temporarily blocked (clearing cost/time). Resource waste on partial extraction. |
| **Attribute gain chances** | STR (~1.5%), INT (~1%) |
| **Skill trait trigger** | Prospector / Jinxed Miner (mining discovery); Quarrier / Cave-in Survivor (mining safety) |
| **Biome modifiers** | Ore type availability is biome-dependent (Phase 4). Some biomes have no minable deposits. |
| **Tool/building requirements** | Pickaxe recommended (yield bonus). Mine building may improve safety/efficiency. |
| **Encounter exposure** | Low on surface. Collapse is the primary hazard. |

### Logging

| Property | Value |
|---|---|
| **Category** | Resource Gathering |
| **Energy cost** | 25–40 |
| **Time-lock** | 72–180 ticks (12–30 min real) |
| **Success formula** | `yield = base_tree_density × (1 + INT × 0.025) × tool_bonus`. Injury avoidance: `avoid = base_safety × (1 + DEX × 0.03)`. |
| **Failure outcomes** | Injury: health damage, injury trait chance (Bruised, Lacerated). Tree density depleted on success (regrowth over time). Over-logging degrades hex fauna/flora. |
| **Attribute gain chances** | STR (~1.5%), END (~1%) |
| **Skill trait trigger** | Master Feller / Timber-cursed (logging domain) |
| **Biome modifiers** | Forest biomes: high yield. Grassland/desert: no trees. Biome determines tree type and density. |
| **Tool/building requirements** | Hatchet/axe recommended (yield bonus). Sawmill building may improve processing. |
| **Encounter exposure** | Low–medium. Activity noise may attract beast encounters. |

### Farming

| Property | Value |
|---|---|
| **Category** | Resource Gathering |
| **Energy cost** | 25–45 (per phase of the crop cycle; total cost spread across sow, tend, harvest) |
| **Time-lock** | Sow: 72–144 ticks. Tend: passive (growth period, no adventurer time-lock). Harvest: 72–144 ticks. |
| **Success formula** | `yield = base_crop_yield × fertility × (1 + WIS × 0.03) × biome_affinity`. Crop failure chance at low fertility. |
| **Failure outcomes** | Crop failure: seeds lost, no yield. Fertility depleted per cycle. Low risk of injury. |
| **Attribute gain chances** | WIS (~1%), END (~0.5%) |
| **Skill trait trigger** | Green Thumb / Barren Touch (harvesting domain) |
| **Biome modifiers** | Biome affinity determines viable crops and yield multiplier. Fertile biomes (plains, grassland) are best. |
| **Tool/building requirements** | Sickle recommended for harvest bonus. Farm building may increase plot count/yield. |
| **Encounter exposure** | Very low. Farming on settled hexes is relatively safe. |

### Foraging

| Property | Value |
|---|---|
| **Category** | Resource Gathering |
| **Energy cost** | 10–20 |
| **Time-lock** | 36–72 ticks (6–12 min real) |
| **Success formula** | `yield = base_wild_growth × (1 + SUR × 0.04) × biome_affinity`. Poison avoidance: `safe = base × (1 + WIS × 0.03)`. |
| **Failure outcomes** | Poisoning: health damage, Poisoned injury trait. Wild growth depleted on success (slow regrowth). Misidentification wastes time. |
| **Attribute gain chances** | SUR (~1%), WIS (~1%) |
| **Skill trait trigger** | Herbalist / Rash Forager (foraging domain) |
| **Biome modifiers** | Forest/jungle biomes: high wild growth. Desert/tundra: sparse. Biome determines plant types. |
| **Tool/building requirements** | None. Basket/satchel (future item) might increase carry yield. |
| **Encounter exposure** | Low–medium. Foraging in wild hexes can trigger beast encounters. |

### Hunting

| Property | Value |
|---|---|
| **Category** | Resource Gathering |
| **Energy cost** | 20–35 |
| **Time-lock** | 72–144 ticks (12–24 min real) |
| **Success formula** | `success = base × (1 + SUR × 0.05) × (1 + gear_bonus) × fauna_density_modifier`. On success: yield from fauna type table. |
| **Failure outcomes** | Failed hunt: energy and time consumed, no yield. Beast encounter may trigger (predator drawn by activity). Injury possible from dangerous prey. |
| **Attribute gain chances** | SUR (~1.5%), DEX (~1%) |
| **Skill trait trigger** | Born Hunter / Skittish (hunting domain) |
| **Biome modifiers** | Fauna density is biome-dependent. Over-hunting depletes density (slow regrowth). |
| **Tool/building requirements** | Bow/traps recommended (success bonus). |
| **Encounter exposure** | Medium–high. Hunting inherently risks beast encounters — predators compete for the same prey. |

### Fishing

| Property | Value |
|---|---|
| **Category** | Resource Gathering |
| **Energy cost** | 10–20 |
| **Time-lock** | 36–108 ticks (6–18 min real) |
| **Success formula** | `yield = base × (1 + SUR × 0.04) × biome_modifier × fauna_density × dock_bonus`. |
| **Failure outcomes** | Failed catch: energy and time consumed, no yield. Minimal injury risk. |
| **Attribute gain chances** | SUR (~1%), DEX (~0.5%) |
| **Skill trait trigger** | Angler / Unlucky Catch (fishing domain) — ⚠️ placeholder, flagged for Phase 13d trait balance review. |
| **Biome modifiers** | Must be on a fishing biome (Coast, Lake, Mangrove, Swamp). Coastal Waters and Ocean have affinity but are impassable without future Maritime module. |
| **Tool/building requirements** | Fishing rod/net recommended (yield bonus). Dock building on Coast hex increases yield. |
| **Encounter exposure** | Very low. Fishing is one of the safest actions. |

### Herding

| Property | Value |
|---|---|
| **Category** | Resource Gathering |
| **Energy cost** | 15–30 (per herding cycle — feeding, health checks, product collection) |
| **Time-lock** | 72–144 ticks (12–24 min real) |
| **Success formula** | `yield = base_livestock_yield × herd_size × (1 + CHA × 0.03) × biome_grazing_modifier`. Sickness avoidance: `healthy = base × (1 + SUR × 0.03)`. |
| **Failure outcomes** | Livestock sickness: reduced yields, potential herd decline. Neglect causes herd loss over time. Grazing depletes local fertility. |
| **Attribute gain chances** | CHA (~1%), SUR (~0.5%) |
| **Skill trait trigger** | Herdsman / Neglectful (livestock domain) |
| **Biome modifiers** | Grassland/plains: best grazing. Desert/tundra: poor. Some biomes cannot support livestock. |
| **Tool/building requirements** | Pasture/barn building recommended. Initial livestock stock required to start herding. |
| **Encounter exposure** | Low. Beast encounters may threaten herd (wolf attacks etc.) but don't directly endanger the adventurer during herding. |

---

## 3. Crafting & Refinement

### Refining

| Property | Value |
|---|---|
| **Category** | Crafting & Refinement |
| **Energy cost** | 25–45 |
| **Time-lock** | 72–216 ticks (12–36 min real) |
| **Success formula** | `quality_tier = f(CRA, INT, recipe_difficulty)`. Tiers: dirty/clean/pure — probability distribution shifts with skill. |
| **Failure outcomes** | Material waste: input consumed, no or partial output. No injury risk. Negative skill trait chance on repeated failures. |
| **Attribute gain chances** | INT (~1%), CRA (~0.5%) |
| **Skill trait trigger** | Refined Taste / Wasteful (refining domain) |
| **Biome modifiers** | None — refining occurs at a facility. |
| **Tool/building requirements** | Facility required: Smelter (ores), Tannery (hides), Workshop (general). Recipe-specific. |
| **Encounter exposure** | None. Indoor/facility action. |

### Crafting

| Property | Value |
|---|---|
| **Category** | Crafting & Refinement |
| **Energy cost** | 20–40 (scales with item complexity) |
| **Time-lock** | 72–216 ticks (12–36 min real; complex items take longer) |
| **Success formula** | `quality = f(CRA, recipe_difficulty, mutation_seed)`. Output properties determined by mutation function. Greatness range influenced by CRA. |
| **Failure outcomes** | Material waste: inputs consumed, no output. No injury risk. Negative skill trait chance on repeated failures. |
| **Attribute gain chances** | CRA (~1.5%), INT (~0.5%) |
| **Skill trait trigger** | Artificer / Fumble-fingers (crafting domain) |
| **Biome modifiers** | None — crafting occurs at a facility. |
| **Tool/building requirements** | Facility required: Forge (weapons/armor), Workshop (tools/items). Recipe-specific. |
| **Encounter exposure** | None. Indoor/facility action. |

### Cooking

| Property | Value |
|---|---|
| **Category** | Crafting & Refinement |
| **Energy cost** | 5–15 |
| **Time-lock** | 18–54 ticks (3–9 min real) |
| **Success formula** | `buff_value = base_recipe_buff × (1 + VIT × 0.02) × cooking_quality`. Quality scales with CRA. |
| **Failure outcomes** | Food waste: ingredients consumed, no or inferior output. Burned food has no buff value. |
| **Attribute gain chances** | VIT (~0.5%), CRA (~0.5%) |
| **Skill trait trigger** | Chef / Scorched Palate (cooking domain) |
| **Biome modifiers** | None — cooking occurs at a Campfire or Cookhouse. |
| **Tool/building requirements** | Campfire (basic recipes, temporary) or Cookhouse (full recipe list, permanent). |
| **Encounter exposure** | None (Cookhouse). Campfire in the wild: low encounter chance. |

### Repairing

| Property | Value |
|---|---|
| **Category** | Crafting & Refinement |
| **Energy cost** | 10–25 (scales with damage severity) |
| **Time-lock** | 36–108 ticks (6–18 min real) |
| **Success formula** | `durability_restored = f(damage_level, CRA, material_quality)`. CRA reduces repair cost by −4%/pt. |
| **Failure outcomes** | Material waste on partial repair. No injury risk. |
| **Attribute gain chances** | CRA (~0.5%), INT (~0.5%) |
| **Skill trait trigger** | No dedicated repair skill trait pair (repairs use general craftsmanship). |
| **Biome modifiers** | None — can be performed anywhere with materials, but Workshop provides efficiency bonus. |
| **Tool/building requirements** | Materials required (type depends on item). Workshop recommended for efficiency. |
| **Encounter exposure** | None if at facility. Low if repairing in the field. |

### Salvaging

| Property | Value |
|---|---|
| **Category** | Crafting & Refinement |
| **Energy cost** | 5–15 |
| **Time-lock** | 18–54 ticks (3–9 min real) |
| **Success formula** | `recovery_rate = base × (1 + CRA × 0.03) × item_condition_modifier`. Higher CRA and better item condition recover more materials. |
| **Failure outcomes** | None — salvaging always produces something (even if minimal). |
| **Attribute gain chances** | CRA (~0.5%) |
| **Skill trait trigger** | No dedicated salvaging skill trait pair in base module. |
| **Biome modifiers** | None. |
| **Tool/building requirements** | Workshop recommended for higher recovery rates. |
| **Encounter exposure** | None. |

---

## 4. Construction & Settlement

### Building

| Property | Value |
|---|---|
| **Category** | Construction & Settlement |
| **Energy cost** | 35–60 (the most energy-intensive settlement action) |
| **Time-lock** | 144–360 ticks (24–60 min real; varies by building type and size) |
| **Success formula** | Automatic — building always completes if resources and energy are sufficient. Construction time scales with INT. Followers (labourers) reduce time-lock. |
| **Failure outcomes** | None (construction doesn't fail, it just takes time and resources). |
| **Attribute gain chances** | INT (~1%), STR (~0.5%) |
| **Skill trait trigger** | Master Builder / Shoddy Builder (building domain) |
| **Biome modifiers** | Some buildings require specific area types. Biome may affect construction difficulty. |
| **Tool/building requirements** | Building materials (planks, bricks, nails, etc.). Hammer recommended (speed bonus). Building slot required in area. |
| **Encounter exposure** | Very low on developed hexes. Noise may attract encounters on undeveloped territory. |

### Demolishing

| Property | Value |
|---|---|
| **Category** | Construction & Settlement |
| **Energy cost** | 20–35 |
| **Time-lock** | 108–216 ticks (18–36 min real) |
| **Success formula** | Automatic — demolition always completes. Material recovery rate: `recovery = base_rate × (1 + STR × 0.02)`. More careful demolition recovers more but takes longer. |
| **Failure outcomes** | None. Some materials always lost in demolition (recovery < 100%). |
| **Attribute gain chances** | STR (~0.5%), END (~0.5%) |
| **Skill trait trigger** | No dedicated demolition skill trait pair. |
| **Biome modifiers** | None. |
| **Tool/building requirements** | None required. Hammer may speed demolition. |
| **Encounter exposure** | None. Demolishing occurs on developed hexes. |

### Upkeep

| Property | Value |
|---|---|
| **Category** | Construction & Settlement |
| **Energy cost** | 10–30 (per hex per in-game day; scales with building count, biome difficulty, and territory size) |
| **Time-lock** | 18–54 ticks (3–9 min real) |
| **Success formula** | Automatic — spending the required energy + resources prevents decay. CHA reduces cost. |
| **Failure outcomes** | Neglect: building condition degrades, territorial decay increases. Buildings below condition threshold become inactive. |
| **Attribute gain chances** | CHA (~0.5%), INT (~0.5%) |
| **Skill trait trigger** | No dedicated upkeep skill trait pair. |
| **Biome modifiers** | Biome affects base upkeep cost (harsh biomes cost more). |
| **Tool/building requirements** | Resources consumed (type depends on buildings maintained). |
| **Encounter exposure** | None. Settlement maintenance action. |

### Stocking

| Property | Value |
|---|---|
| **Category** | Construction & Settlement |
| **Energy cost** | 5–15 |
| **Time-lock** | 12–36 ticks (2–6 min real) |
| **Success formula** | Automatic — food is deposited. Amount deposited limited by carry capacity and available food. |
| **Failure outcomes** | None. |
| **Attribute gain chances** | None. |
| **Skill trait trigger** | None. |
| **Biome modifiers** | None. |
| **Tool/building requirements** | Settlement with food reserve storage. Food in adventurer inventory. |
| **Encounter exposure** | None. |

---

## 5. Followers

### Recruiting

| Property | Value |
|---|---|
| **Category** | Followers |
| **Energy cost** | 15–30 |
| **Time-lock** | 54–108 ticks (9–18 min real) |
| **Success formula** | Labourer recruitment: `success = base × (1 + LEA × 0.04) × (1 + CHA × 0.03) × settlement_population_modifier`. Animal binding: automatic if herded animal is available. |
| **Failure outcomes** | Failed recruitment: energy and time consumed, no follower gained. |
| **Attribute gain chances** | LEA (~1%), CHA (~0.5%) |
| **Skill trait trigger** | Tamer / Unruly (follower recruitment domain); Shepherd / Harsh Keeper (follower management domain) |
| **Biome modifiers** | Labourers only available in settled hexes with sufficient population. |
| **Tool/building requirements** | Settlement with population (for labourers). Herded animal available (for binding). Follower slot available. |
| **Encounter exposure** | Social encounters may occur during the recruitment process. |

---

## 6. Social & Economic

### Trading

| Property | Value |
|---|---|
| **Category** | Social & Economic |
| **Energy cost** | 5–10 |
| **Time-lock** | 12–36 ticks (2–6 min real) |
| **Success formula** | Trade is consensual — no failure state for the exchange itself. CHA influences offered rates: `rate_bonus = CHA × 3%`. |
| **Failure outcomes** | None for the exchange. Travelling to a trade partner/market has its own costs and risks. |
| **Attribute gain chances** | CHA (~0.5%), INT (~0.5%) |
| **Skill trait trigger** | No dedicated trading skill trait pair in base module (candidate for future economic module). |
| **Biome modifiers** | None for the trade action itself. Market buildings may provide better rates. |
| **Tool/building requirements** | Market building (for reliable trading) or social encounter (for opportunistic trading). |
| **Encounter exposure** | None at a market. Social encounter context if trading during exploration. |

### Exchange

| Property | Value |
|---|---|
| **Category** | Social & Economic |
| **Energy cost** | 0 (free action) |
| **Time-lock** | 0 (instant) |
| **Requirements** | Two adventurers on the same hex, or adventurer interacting with a market listing |
| **Success formula** | Automatic for direct transfers. Market transactions resolve instantly when both parties agree. |
| **Description** | The single baseline action for all trade and transfer in the base module. Supports: (1) **Direct transfer** — move goods/currency between adventurers on the same hex, (2) **Market listing** — set prices on inventory items, making them visible to other adventurers, (3) **Market purchase** — accept another adventurer's listed items at the listed price. |
| **CHA interaction** | CHA provides fee discounts on market transactions. Relevant traits (Merchant, Shrewd, Silver Tongue) grant additional trade benefits (better prices, reduced fees, wider visibility for listings). |
| **Attribute gain chances** | CHA (~0.5%) per market transaction |
| **Skill trait trigger** | Merchant / Swindled (trade domain) |
| **Encounter exposure** | None |

> ⚠️ **Implementation concerns**: Market listings require on-chain state (item ID, price, seller address, listing timestamp). Storage cost per listing must be evaluated. Listing expiry/cleanup mechanism needed. Price discovery is emergent — no central price oracle. Flag for contracts specialist and Phase 5/6 quantification follow-up.

---

## 7. Recovery

### Resting

| Property | Value |
|---|---|
| **Category** | Recovery |
| **Energy cost** | 0 (free) |
| **Time-lock** | 12 ticks (2 min real) |
| **Success formula** | Restores health (2–3 HP base, modified by VIT and food buffs). |
| **Failure outcomes** | None. |
| **Attribute gain chances** | None (resting doesn't develop skills or attributes). |
| **Skill trait trigger** | None. |
| **Biome modifiers** | Food buffs may improve Rest effectiveness. |
| **Tool/building requirements** | None. |
| **Encounter exposure** | Low encounter chance. |

> Note: Can be repeated indefinitely. This is the ONLY way to recover health in the base module (besides food/item buffs that enhance rest). After heavy damage, an adventurer may need to rest continuously for an hour or more to fully recover — this is intentional. Health recovery is a meaningful time investment, creating a genuine choice between productive work and healing.

---

## Action–Trait Cross-Reference (Phase 2c)

Every trait special effect mapped to the action(s) it modifies. This validates that no trait references a non-existent action and no action is orphaned from the trait system.

### Trait → Action Mapping

#### Movement & Exploration actions

| Trait | Effect on action | Actions affected |
|---|---|---|
| Cautious (P07) | +25% time-lock duration | Travel, Explore, Survey, Delve |
| Reckless (P08) | −25% time-lock duration | Travel, Explore, Survey, Delve |
| Impulsive (P14) | −25% travel time | Travel |
| Zoophilist (P37) | −25% travel time-lock with mount | Travel |
| Lean (H11) | −25% travel/explore energy cost | Travel, Explore |
| Delicate (H02) | −10% travel time | Travel |
| Perceptive (H05) | −25% explore/survey/delve energy cost | Explore, Survey, Delve |
| Scatterbrained (P26) | +10% all time-lock durations | All actions with time-locks |
| One-eyed (D01) | +10% explore/survey/delve energy cost | Explore, Survey, Delve |
| Lame (D02) | +25% explore/travel energy cost; +25% time | Explore, Travel |
| Pathfinder (S11) | −25% travel time in explored hexes | Travel |
| Footsore (S12) | +25% travel/explore energy cost | Travel, Explore |
| Trailblazer (S09) | −10% explore energy cost | Explore |
| Lost (S10) | +10% explore energy cost | Explore |
| Cartographer (S25) | −25% survey energy cost | Survey |
| Disoriented (S26) | +25% survey energy cost | Survey |
| Skeptical (P20) | −10% delve energy cost | Delve |

#### Resource Gathering actions

| Trait | Effect on action | Actions affected |
|---|---|---|
| Tall (H09) | +25% logging yield | Logging |
| Short (H10) | +25% mining yield | Mining |
| Broad (H12) | +10% logging/mining/harvesting yields | Logging, Mining, Farming |
| Keen-nosed (H13) | −10% foraging/hunting/delving energy cost | Foraging, Hunting, Delve |
| Dull-nosed (H14) | +10% harvesting/livestock yields | Farming, Herding |
| Green Thumb (S01) | +25% harvest yield | Farming, Foraging |
| Barren Touch (S02) | −25% harvest yield | Farming, Foraging |
| Born Hunter (S03) | +25% hunting success | Hunting |
| Skittish (S04) | −25% hunting success | Hunting |
| Prospector (S05) | +10% mining success; +25% attribute gain from mining | Mining |
| Jinxed Miner (S06) | +10% mine collapse chance | Mining |
| Master Feller (S07) | +25% logging yield | Logging |
| Timber-cursed (S08) | −25% logging yield; +10% injury chance | Logging |
| Herbalist (S35) | +25% foraging yield; +50% foraged food regen | Foraging |
| Rash Forager (S36) | −25% foraging yield; +25% poisoning chance | Foraging |
| Herdsman (S21) | +25% livestock yield | Herding |
| Neglectful (S22) | −10% livestock yield; +10% sickness chance | Herding |
| Ruthless (P32) | +25% hunting yield | Hunting |
| Zoophobe (P38) | −25% hunting energy cost | Hunting |
| Quarrier (S33) | −25% mine collapse chance | Mining |
| Cave-in Survivor (S34) | +10% mine collapse chance; +10% mining energy cost | Mining |
| Ambidextrous (H23) | +10% all personal yields | All resource gathering |

#### Crafting & Refinement actions

| Trait | Effect on action | Actions affected |
|---|---|---|
| Studious (P03) | +25% attribute gain from crafting/refining | Crafting, Refining |
| Dull-witted (P04) | −25% attribute gain from crafting/refining | Crafting, Refining |
| Skeptical (P20) | +10% crafting quality | Crafting |
| Methodical (P25) | −25% equipment durability decay rate | Repairing (extends time between repairs) |
| Artificer (S13) | +25% crafting quality | Crafting |
| Fumble-fingers (S14) | −25% crafting quality | Crafting |
| Refined Taste (S15) | +10% refining yield | Refining |
| Wasteful (S16) | −10% refining yield | Refining |
| Chef (S27) | +25% food buff effectiveness | Cooking |
| Scorched Palate (S28) | −25% food buff effectiveness | Cooking |
| Keen-nosed (H13) | +25% cooking success chance | Cooking |
| Shrewd (P33) | −10% building upkeep cost | Upkeep (indirect — reduces material needs) |
| Burned (I08) | −25% crafting quality; −25% production efficiency | Crafting, Refining |
| Addled (D05) | −25% refining/crafting quality | Refining, Crafting |
| Nerve-dead (D06) | −25% crafting quality | Crafting |
| Missing Fingers (D09) | −10% all success chances | All crafting actions |
| Concussed (I04) | −25% crafting/refining quality | Crafting, Refining |

#### Construction & Settlement actions

| Trait | Effect on action | Actions affected |
|---|---|---|
| Brawny (H01) | −10% construction time-lock | Building |
| Stocky (H19) | −10% construction energy cost | Building |
| Master Builder (S19) | −25% building construction time | Building |
| Shoddy Builder (S20) | +25% building construction time | Building |
| Shrewd (P33) | −10% building upkeep cost | Upkeep |
| Greedy (P12) | −10% all upkeep costs | Upkeep |
| Comely (H21) | −10% upkeep cost | Upkeep |
| Fractured (I03) | Cannot perform logging/mining/building | Building |

#### Follower actions

| Trait | Effect on action | Actions affected |
|---|---|---|
| Sociable (P09) | +1 max follower | Recruiting (expands capacity) |
| Tamer (S29) | +1 max animal follower | Recruiting (expands animal capacity) |
| Commanding (P21) | +25% follower throughput | All actions using followers |
| Meek (P22) | −10% follower throughput | All actions using followers |
| Generous (P11) | +25% labourer throughput | All actions using labourers |
| Merciful (P31) | −25% follower desertion rate | Recruiting (retention) |
| Shepherd (S23) | −10% follower food consumption; +10% throughput | All actions using followers |
| Harsh Keeper (S24) | +10% follower food consumption; +10% desertion | Recruiting (retention) |
| Unruly (S30) | +50% follower desertion rate | Recruiting (retention) |
| Broken Spirit (D10) | −1 max follower | Recruiting (reduces capacity) |

#### Encounter resolution

| Trait | Effect on encounter | Encounter types |
|---|---|---|
| Brave (P01) | +25% beast encounter success | Beast |
| Craven (P02) | −25% beast encounter success | Beast |
| Fierce (P27) | +25% damage in beast encounters | Beast |
| Composed (P28) | −50% negative trait gain chance | Beast |
| Beast Slayer (S17) | +25% beast encounter success | Beast |
| Prey (S18) | −25% beast encounter success | Beast |
| Nimble (H03) | +25% hazard avoidance | Beast, Combat |
| Clumsy (H04) | −25% hazard avoidance | Beast, Combat |
| Resilient (H17) | −10% health loss from encounters | Beast, Combat |
| Comely (H21) | +25% social encounter success | Social |
| Ugly (H22) | −25% social encounter success | Social |
| Scarred (D04) | +10% beast encounter success | Beast |
| Deaf (D08) | −25% beast encounter success | Beast |
| Naive (P34) | +10% encounter success; +25% damage taken | Beast, Combat, Social |

#### Global / cross-cutting

| Trait | Effect | Scope |
|---|---|---|
| Patient (P13) | +10% all success chances | All actions with success rolls |
| Diligent (P05) | +10% attribute gain from all actions | All actions |
| Idle (P06) | −10% attribute gain from all actions | All actions |
| Ambitious (P35) | +25% trait gain from all actions | All actions |
| Solitary (P10) | −10% all energy costs (no human followers) | All actions |
| Incurious (P16) | −10% energy cost on settlement hexes | All actions on settlement hexes |
| Content (P36) | +10% energy regen on settlement hexes | Passive (all settlement actions benefit) |
| Voracious (H15) | −10% success chance below 50% food | All actions with success rolls |
| Blinded (I10) | −25% all success chances | All actions with success rolls |
| Bruised (I01) | −25% production efficiency | All resource gathering |
| Winded (I06) | −25% energy regen rate | All actions (indirect — reduces available energy) |
| Gashed (I07) | −100% health regen; +25% all energy costs | All actions |
| Traumatised (I13) | −50% attribute gain chance | All actions |

### Actions without dedicated skill trait pairs

The following actions have no exclusive skill trait pair in the base module. They are candidates for future module expansion:

| Action | Notes |
|---|---|
| Fishing | **Angler / Unlucky Catch** — placeholder pair added, flagged for Phase 13d review |
| Salvaging | Could pair with crafting domain or get its own (e.g., Scrapper / Wastrel) |
| Demolishing | Low-priority — demolition is infrequent |
| Upkeep | Could pair with a stewardship domain (e.g., Steward / Negligent) |
| Stocking | Low-priority — trivial action |
| Trading | Candidate for economic module (e.g., Merchant / Swindled) |
| Resting | Low-priority — no skill element |
| Delve | Could get its own pair in a dungeon module (e.g., Delver / Claustrophobic) |

---

## Time Budget Analysis (Phase 2d)

### Energy budget per in-game day

| Metric | Value |
|---|---|
| Base energy regen | 0.1/tick = 36/hour = 864/in-game day |
| Base max energy | 100 (storage cap) |
| END bonus to regen | +2%/pt (at END 5: ~950/day; at END 10: ~1,037/day) |
| END bonus to max | +10/pt (at END 5: 150 max; at END 10: 200 max) |

### Typical action day (draft worked example)

**Scenario**: A mid-progression adventurer (attributes ~3–5 average, basic tools, 1 labourer, no mount) operating from a settled hex.

| Time block | Actions | Energy spent | Time-locks (ticks) |
|---|---|---|---|
| Morning | Travel to resource hex (35), Mine ore (35) | ~70 | ~290 |
| Midday | Travel back (35), Refine ore (35), Cook meal (10) | ~80 | ~320 |
| Afternoon | Farm (harvest phase) (35), Forage (15) | ~50 | ~190 |
| Evening | Upkeep (20), Stock settlement (10), Repair gear (20) | ~50 | ~120 |
| Night | Rest (0 energy, restores 2–3 HP per 12-tick rest) | 0 | Variable |
| **Daily total** | **~9 actions** | **~250 energy** | **~920 ticks** |

**Analysis**:
- 250 of 864 available energy used = ~29% of daily budget. Headroom for additional actions, encounter recovery, and exploration days — but less generous than before.
- 920 of 1,440 ticks occupied = ~64% of the in-game day. With heavier time-locks, the adventurer has ~520 ticks (~87 min real) of idle/regen time.
- On an **exploration day** (Travel + Explore + Delve), energy spend reaches 130–165 per cycle with time-locks consuming 360–720 ticks. A single explore–delve cycle could consume half the in-game day.
- **Key insight**: With heavier action costs, adventurers make fewer but more consequential decisions per day. Travel is now a significant commitment — you don't casually hop between hexes. Exploration is a major undertaking. This reinforces the value of settlement infrastructure and planning.

### Actions per real-time day

| Metric | Value |
|---|---|
| In-game days per real day | 6 |
| Actions per in-game day (typical) | 6–10 |
| Actions per real day (typical) | 36–60 |
| Significant decisions per real day | ~10–15 (each action is more meaningful at heavier costs) |

> **Note**: These figures are draft estimates. Exact values depend on Phase 4 (biome modifiers), Phase 5 (movement costs), and Phase 6 (production costs). The purpose of this analysis is to establish the **order of magnitude** and flag the time-lock vs energy dynamic for balance attention. The heavier cost tiers mean each action carries more weight — supporting the design goal of consequential choices.

---

*Appendix M — March 2026*
*Phase 2 — Draft action properties. Exact values to be locked in Phases 3–13.*
