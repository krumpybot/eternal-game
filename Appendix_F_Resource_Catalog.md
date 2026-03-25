# Appendix F: Resource Catalog

> Complete resource enumeration for the base module. All resource IDs are assigned at deployment and are immutable. Resources not sourced by any base-module biome or action exist as registered IDs for future modules — they simply don't appear in the wild until a future module introduces a source.
>
> Resources are organized into **three layers**:
> 1. **Raw Resources** — gathered, mined, hunted, foraged, fished, or herded directly from the world. The input layer.
> 2. **Core Resources** — the 22 Eternum-heritage resources. Some are raw (usable directly), most are refined from raw precursors listed in Section 1.
> 3. **Refined & Processed Materials** — derived from raw resources through refining, crafting, or processing actions. The output layer that feeds into equipment, buildings, and consumables.
>
> Additionally: **Beast Parts** (generic, tiered by beast level), **Food** (raw + processed).
>
> **Alchemy** is deferred to a future module. Cooking fills basic consumable needs in the base module.

---

## Rarity System

Six tiers, applied consistently across all resource types.

| Tier | Colour | Availability | Core resource examples |
|---|---|---|---|
| **Common** | White | Abundant in most biomes | Wood, Stone, Coal |
| **Uncommon** | Green | Requires specific biomes or moderate effort | Copper, Obsidian, Silver, Iron, Gold |
| **Rare** | Blue | Limited biomes, lower node density | Hartwood, Diamonds, Sapphire, Ruby |
| **Epic** | Purple | Specialized biomes, deep nodes, special areas | Deep Crystal, Ignium, Ethereal Silica |
| **Legendary** | Orange | Extremely rare, specific conditions | True Ice, Twilight Quartz, Alchemical Silver |
| **Mythic** | Red | Near-unique, endgame only | Adamantine, Mithral, Dragonhide |

> Only core resource raw precursors may be Mythic. Non-core raw resources cap at Legendary.

---

## Resource ID Registry

All resources are assigned permanent `u16` IDs at deployment. Reserved ranges ensure future modules can add resources without base module changes.

```
0x0000–0x003F: Food — Raw (wild, farmed, hunted, fished, herded)
0x0040–0x007F: Food — Processed (cooked meals, preserved, baked)
0x0080–0x00FF: Food — Reserved future
0x0100–0x013F: Core Resources T1 (Common) + raw precursors
0x0140–0x017F: Core Resources T2 (Uncommon) + raw precursors
0x0180–0x01BF: Core Resources T3+ (Rare–Mythic) + raw precursors
0x01C0–0x01FF: Core Resources — Reserved future
0x0200–0x023F: Raw Materials — Mining (ores, minerals, stone)
0x0240–0x027F: Raw Materials — Logging (wood, resin)
0x0280–0x02BF: Raw Materials — Gathering (fibers, herbs, clay)
0x02C0–0x02FF: Raw Materials — Animal (hides, bone, sinew)
0x0300–0x033F: Refined Materials — Metals (ingots, alloys)
0x0340–0x037F: Refined Materials — Textiles (cloth, leather, rope)
0x0380–0x03BF: Refined Materials — Building (planks, bricks, mortar)
0x03C0–0x03FF: Refined Materials — Fuels & Chemicals (oil, flux, dyes)
0x0400–0x043F: Beast Parts (generic, tiered)
0x0440–0x04FF: Reserved (Alchemy & Essence — future module)
0x0500–0x0FFF: Reserved (future modules)
```

---

## Resource Properties Schema

| Property | Description |
|---|---|
| **ID** | Permanent `u16` identifier |
| **Name** | Display name |
| **Category** | Raw / Core / Refined / Beast Part / Food |
| **Sub-category** | Specific grouping (ore, timber, textile, etc.) |
| **Weight** | kg per unit |
| **Rarity** | Common / Uncommon / Rare / Epic / Legendary / Mythic |
| **Source action** | Which action(s) produce this resource |
| **Refinement path** | What this resource refines into (if any) |
| **Refined from** | What raw resource(s) produce this (if refined) |
| **Tags** | Searchable: fuel, food, building, weapon, armor, trade, etc. |

---

## 1. Raw Resources

Raw resources are gathered directly from the world through player actions. They are the foundation of the entire supply chain. Some are usable directly (Wood, Stone, Coal, Obsidian); most require refinement.

### 1a. Mining — Ores & Minerals

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| M01 | **Stone** | 3.0 | Common | Usable directly; cut → Stone Blocks | Ubiquitous. Construction, tools. Also a Core Resource (C02). |
| M02 | **Coal** | 1.5 | Common | Usable directly as fuel | Primary smelting/refining fuel. Essential for all metal production. Also a Core Resource (C03). |
| M03 | **Clay** | 2.0 | Common | Kiln → Bricks; Potter → Pottery | Construction, crafting. |
| M04 | **Sand** | 2.0 | Common | Furnace → Glass | Glass production, mortar ingredient. |
| M05 | **Limestone** | 3.0 | Common | Kiln → Lime; grind → Flux | Mortar base, flux, plaster. |
| M06 | **Flint** | 0.5 | Common | Usable directly | Fire-starting, primitive tools. |
| M07 | **Salt** | 0.5 | Common | Usable directly | Food preservation, trade good. |
| M08 | **Copper Ore** | 2.5 | Uncommon | Smelter → Copper (core) | T1 metal precursor. |
| M09 | **Obsidian** | 2.0 | Uncommon | Usable directly; knap → Obsidian Blades | Volcanic glass. Sharp but brittle. Also a Core Resource (C05). |
| M10 | **Silver Ore** | 3.0 | Uncommon | Smelter → Silver (core) | Precious metal. Flagged: alchemy applications (future module). |
| M11 | **Cold Iron Ore** | 3.0 | Uncommon | Smelter → Cold Iron (core) | The standard metal ore. "Iron" in the Eternal World is Cold Iron — the Eternum heritage name for the everyday working metal. |
| M12 | **Gold Ore** | 4.0 | Uncommon | Smelter → Gold (core) | Precious, heavy. Jewelry, currency. |
| M13 | **Sulfur** | 1.0 | Uncommon | Usable directly; future alchemy reagent | Yellow mineral. |
| M14 | **Saltpeter** | 1.0 | Uncommon | Usable directly; future alchemy reagent | Nitrate mineral. |
| M15 | **Rough Diamond** | 0.2 | Rare | Jeweler → Diamonds (core) | Gem precursor. |
| M16 | **Rough Sapphire** | 0.2 | Rare | Jeweler → Sapphire (core) | Gem precursor. |
| M17 | **Rough Ruby** | 0.2 | Rare | Jeweler → Ruby (core) | Gem precursor. |
| M18 | **Rare Metals** | 1.0 | Rare | Crafting component — used in the crafting system to modify item properties | Low chance drop from **all** mining activities regardless of node. Represents trace alloy metals (tin, nickel, chromium, etc.). Combined with other metals and reagents during crafting to produce items with enhanced properties (alloys are emergent from the crafting system, not standalone resources). |
| M19 | **Amber** | 0.3 | Rare | Jewelry component (decorative); alchemy reagent (future) | Fossilized resin. Jewelry, trade. |
| M20 | **Rough Deep Crystal** | 0.5 | Epic | Specialized refinement → Deep Crystal (core) | Resonant crystal. Found in deep crystalline deposits. |
| M21 | **Ignium Ore** | 1.0 | Epic | Specialized refinement → Ignium (core) | Fire-aspected volcanic ore. |
| M22 | **Ethereal Silica Sand** | 0.3 | Epic | Specialized refinement → Ethereal Silica (core) | Translucent, semi-ethereal sand found in magical desert formations. |
| M23 | **Voidstone** | 2.5 | Epic | Specialized refinement → Voidsand (refined) | Dark mineral from deep caves. Absorbs light and energy. Dimensionally unstable. When processed, yields a sand-like reagent that produces unnaturally hot fires. |
| M24 | **True Ice Shard** | 1.5 | Legendary | Specialized refinement → True Ice (core) | Glacial ice that never melts. Found in polar and high-mountain biomes. |
| M25 | **Rough Twilight Quartz** | 0.4 | Legendary | Specialized refinement → Twilight Quartz (core) | Dimensional crystal that shifts colour. Found in special areas. |
| M26 | **Alchemical Silver Ore** | 2.0 | Legendary | Complex refinement + reagents → Alchemical Silver (core) | Reactive, dangerous to refine. Anti-magical properties. |
| M27 | **Starmetal Fragment** | 2.0 | Legendary | Specialized refinement → Starmetal (refined) | Meteorite ore. Extremely heat-resistant. Found in impact craters and deep mines. Starmetal is used to forge top-tier smithing equipment — tools, anvils, and crucibles that can withstand the extreme temperatures required for endgame metalwork. |
| M28 | **Adamantine Ore** | 5.0 | Mythic | Master refinement → Adamantine (core) | Hardest known metal. Deepest mines only. Requires Voidsand heat + Starmetal equipment to smelt. |
| M29 | **Mithral Ore** | 1.5 | Mythic | Master refinement → Mithral (core) | Lightest strong metal. Deepest mines only. Requires Voidsand heat + Starmetal equipment to smelt. |
| M30 | **Unearthed Dragonhide** | 3.0 | Mythic | Specialized curing → Dragonhide (core) | Fossilised dragon hide, excavated from long-dead dragon remains. Dragons once ruled the land in an age before men; a cataclysm cut their numbers to extinction. Their indestructible hide survives fossilisation and can be mined from ancient burial sites. |

> **Mining rarity summary**: 7 Common, 7 Uncommon, 5 Rare, 4 Epic, 4 Legendary, 3 Mythic = **30 mining raw resources**.
>
> **Rare Metals** is unique: it has no dedicated node. Instead, it is a low-chance roll from *any* mining activity regardless of the ore being mined. This makes it naturally rare without requiring specific biome placement.

### 1b. Logging — Timber & Forest Products

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| L01 | **Wood** | 3.0 | Common | Usable directly; Sawmill → Planks (refined) | Universal construction and crafting material. Also a Core Resource (C01). |
| L02 | **Resin** | 0.5 | Common | Refine → Pitch (waterproofing, fuel) | Sticky sap from conifers. |
| L03 | **Ironwood Logs** | 6.0 | Uncommon | Sawmill → Ironwood (core) | Dense, iron-hard wood. Specific forest biomes. |
| L04 | **Hartwood Logs** | 5.5 | Rare | Sawmill → Hartwood (core) | Ancient heartwood, resonant. Rare old-growth forests. |
| L05 | **Worldroot** | 0.3 | Rare | No firm refinement path — valued for its raw properties | Gnarled root-like growths occasionally discovered when felling trees. Part of an ancient subterranean root network that connects trees across vast distances — some say across the entire world. Low-chance discovery from **any** logging activity. Potent cooking ingredient and future alchemy reagent. |
| L06 | **Moonwood Logs** | 4.0 | Epic | Specialized Sawmill → Moonwood (refined) | Pale, silvery timber from ancient trees that have absorbed centuries of moonlight. Found only in the oldest forests where canopy gaps allow sustained direct moonlight. The wood has a faint luminescence and is cool to the touch. Prized for its light-aspected properties. |

> **Logging rarity summary**: 2 Common, 1 Uncommon, 2 Rare, 1 Epic = **6 logging raw resources**.
>
> **Worldroot** mirrors **Rare Metals** in acquisition: no dedicated node, low-chance roll from any logging activity. This creates a parallel between mining and logging — both have rare "universal discovery" resources.

### 1c. Farming — Crops & Cultivated Plants

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| F01 | **Wheat** | 0.5 | Common | Mill → Flour → Bread, Pastry | The staple grain. Stalks also used for Thatch. |
| F02 | **Barley** | 0.5 | Common | Animal feed; cooking ingredient; brewing (future) | Grain, dual-use. |
| F03 | **Vegetables** | 0.8 | Common | Cooking ingredient; usable raw (low nutrition) | Generic crop category. |
| F04 | **Fruit** | 0.5 | Common | Cooking ingredient; usable raw; preserve → Jam | Orchard crops. |
| F05 | **Tubers** | 0.8 | Common | Cooking ingredient; usable raw | Potatoes, turnips, etc. |
| F06 | **Beans** | 0.4 | Common | Cooking ingredient; high protein | Legumes, dried for storage. |
| F07 | **Flax** | 0.3 | Common | Process → Linen (refined) | Primary textile fiber. |
| F08 | **Hemp** | 0.3 | Common | Twist → Rope; spin → Canvas | Cordage and textile. |
| F09 | **Raw Cotton** | 0.2 | Uncommon | Process → Cotton (refined) | Finer textile fiber, warmer biomes only. |
| F10 | **Sugar Cane** | 0.6 | Uncommon | Press → Sugar | Tropical crop. |
| F11 | **Spice Plants** | 0.2 | Uncommon | Dry/grind → Spices (cooking catalyst) | Pepper, cinnamon, cloves. |
| F12 | **Dye Plants** | 0.3 | Uncommon | Process → Dyes | Indigo, woad, madder. |
| F13 | **Hops** | 0.3 | Uncommon | Brewing ingredient (future) | Bittering agent. |

> **Farming rarity summary**: 8 Common, 5 Uncommon = **13 farming raw resources**.

### 1d. Foraging — Wild-Gathered

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| G01 | **Wild Berries** | 0.3 | Common | Edible raw; cooking ingredient; preserve → Jam | Low nutrition but plentiful. |
| G02 | **Roots** | 0.5 | Common | Cooking ingredient; some have medicinal properties | Food and medicine. |
| G03 | **Mushrooms** | 0.2 | Common | Cooking ingredient; some poisonous | Risk of misidentification. |
| G04 | **Wild Herbs** | 0.1 | Common | Cooking catalyst | Sage, thyme, mint, rosemary. |
| G05 | **Seaweed** | 0.3 | Common | Cooking ingredient; fertilizer | Coastal foraging. |
| G06 | **Nuts** | 0.3 | Uncommon | Edible; cooking ingredient; press → Oil | Seasonal. Specific woodland biomes. |
| G07 | **Honey** | 0.5 | Uncommon | Cooking ingredient; preservative; sweetener | Wild hive foraging. |
| G08 | **Medicinal Herbs** | 0.1 | Uncommon | Healing poultice ingredient; cooking | Healing applications. |
| G09 | **Poisonous Plants** | 0.1 | Rare | Future alchemy reagent; weapon coating (future) | Dangerous but valuable. Requires knowledge to identify and handle safely. |
| G10 | **Truffles** | 0.1 | Rare | Cooking luxury; high trade value | Rare forest find, prized. |
| G11 | **Rare Herbs** | 0.1 | Rare | Future alchemy reagent (potent); high-value cooking | Powerful properties. |
| G12 | **Spiritbloom** | 0.1 | Epic | Specialized refinement → Spiritbloom Essence (refined) | Luminous magical flower. Grows only in special areas with residual magical energy — near ruins, spawn nodes, or ancient sites. Petals glow faintly. |
| G13 | **Spider Silk** | 0.1 | Rare | Process → Silk (refined) | Raw silk harvested from giant spider webs and nests in caves and deep forests. Dangerous to collect — the spiders are territorial. Strong, lightweight, and naturally adhesive. |
| G14 | **Sunsprout** | 0.1 | Epic | No firm refinement path — potent ingredient usable raw | A golden shoot that erupts from sun-baked earth in arid and high-altitude biomes. Thrives where other plants cannot — cracked stone, volcanic soil, desert hardpan. Radiates gentle warmth. Potent cooking ingredient and future alchemy reagent. |

> **Foraging rarity summary**: 5 Common, 3 Uncommon, 4 Rare, 2 Epic = **14 foraging raw resources**.

### 1e. Hunting — Wild Fauna Products

Products from hunting wild animals. Some products are shared with herding (marked with †).

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| H01 | **Game Meat** | 1.0 | Common | Cooking → Cooked Meat; cure → Dried Meat | Wild animal protein. Distinct from livestock meats. |
| H02 | **Raw Hide** † | 2.0 | Common | Tannery → Leather | Same resource from hunting or herding (butchered livestock). |
| H03 | **Raw Bone** † | 1.0 | Common | Carve → Bone Tools; grind → Bone Meal (fertilizer) | Same resource from hunting or herding. |
| H04 | **Raw Fat** † | 0.8 | Common | Render → Tallow (fuel, soap, candles) | Same resource from hunting or herding. |
| H05 | **Sinew** | 0.3 | Common | Dry → Sinew Thread | Natural cordage. Bowstrings, stitching, armour binding. |
| H06 | **Feathers** † | 0.1 | Uncommon | Arrow fletching; stuffing | Bird hunting byproduct. Also from poultry herding. |
| H07 | **Fur Pelt** | 1.5 | Uncommon | Furrier → Fur (refined) | Cold-climate gear, trade good. |
| H08 | **Venom Sac** | 0.2 | Rare | Future alchemy reagent; weapon coating (future) | Snakes, spiders, scorpions. Requires careful extraction. |
| H09 | **Ivory** | 1.0 | Rare | Carve → Ivory Components; decorative; trade | Large fauna only (tusked beasts). |
| H10 | **Exotic Pelt** | 1.5 | Epic | Furrier → Exotic Fur (refined) | Pelts from rare or unusual fauna — spotted cats, white wolves, jungle serpents. Superior to standard fur pelts. Luxury trade good, high-tier cold-weather gear. |
| H11 | **Raw Demonhide** | 2.5 | Legendary | Specialized tannery → Demonhide (refined) | Hide stripped from slain demons. Tough, dark, and faintly warm to the touch. Demons are rare and dangerous prey — hunting one is an extreme feat. The hide retains residual infernal energy and must be carefully cured to be workable. |

> **Hunting rarity summary**: 5 Common, 2 Uncommon, 2 Rare, 1 Epic, 1 Legendary = **11 hunting raw resources**.
>
> † **Shared resources**: Raw Hide, Raw Bone, Raw Fat, and Feathers are the same item regardless of whether they come from hunting wild fauna or herding livestock. The distinction is in the source action, not the material. Four shared items total.

### 1f. Fishing — Aquatic Products

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| FI01 | **Fish** | 0.8 | Common | Cooking → Cooked Fish; cure → Dried Fish; press → Fish Oil (refined) | Core food resource. Eternum heritage. Requires water-adjacent hex. |

> **Fishing rarity summary**: 1 Common = **1 fishing raw resource**.
>
> Fishing currently produces only Fish. Additional aquatic resources (shellfish, pearls, coral) are candidates for a future Maritime module.

### 1g. Herding — Livestock Products

Herding produces products from four distinct livestock types. Some products are shared with hunting (marked with †). Unlike other livestock products, **Horses and Donkeys are the livestock themselves** — living animals used as adventurer followers for transport and carrying capacity.

#### Livestock Types

| Livestock | Primary products | Secondary products |
|---|---|---|
| **Cattle** | Beef, Milk | Raw Hide †, Raw Bone †, Raw Fat †, Manure |
| **Sheep** | Mutton, Raw Wool | Raw Hide †, Raw Bone †, Raw Fat †, Manure |
| **Poultry** | Poultry, Eggs | Feathers †, Raw Bone †, Raw Fat †, Manure |
| **Horses & Donkeys** | Horse (follower), Donkey (follower) | Horsemeat (low value), Raw Hide †, Raw Bone †, Raw Fat †, Manure |

#### Herding Product Table

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| K01 | **Milk** | 0.5 | Common | Churn → Butter/Cheese; cooking ingredient | Cattle. |
| K02 | **Eggs** | 0.3 | Common | Cooking ingredient; usable raw (low nutrition) | Poultry. |
| K03 | **Raw Wool** | 0.8 | Common | Process → Wool (refined) | Sheep shearing. |
| K04 | **Manure** | 1.0 | Common | Usable directly as fertilizer | All livestock produce manure. Accelerates farming fertility recovery. |
| K05 | **Beef** | 1.5 | Common | Cooking → Roast Beef; cure → Dried Meat | Cattle. Higher nutrition than game meat. |
| K06 | **Mutton** | 1.2 | Common | Cooking → Roast Mutton; cure → Dried Meat | Sheep. |
| K07 | **Poultry** | 0.8 | Common | Cooking → Roast Poultry | Poultry (chickens, ducks, geese). |
| K08 | **Horsemeat** | 1.5 | Uncommon | Cooking ingredient (low-value use case) | Horses & donkeys. Destroying a valuable transport animal for food is wasteful but possible in desperation. |
| K09 | **Horn** | 0.5 | Uncommon | Carve → Horn Components; powder (future alchemy) | Cattle, goats. |
| K10 | **Down** | 0.2 | Uncommon | Stuffing → Bedding, insulation | Goose/duck feathers. |
| K11 | **Horse** | — | Uncommon | N/A — living follower | Mount follower. Raised through herding, bound via Recruiting. −20% travel energy/time, +20 kg carry. Max 1 mount per adventurer. |
| K12 | **Donkey** | — | Uncommon | N/A — living follower | Pack follower. Raised through herding, bound via Recruiting. +50 kg carry capacity. |

> **Herding rarity summary**: 7 Common, 5 Uncommon = **12 herding products** (including 2 living follower products).
>
> † Raw Hide, Raw Bone, Raw Fat, and Feathers are shared with hunting — same items regardless of source. They are listed under Hunting (§1e) as the primary entry and are not duplicated here.
>
> **Horses and Donkeys** have no weight — they are living animals, not inventory items. They occupy follower slots, not inventory. Breeding rates and maturation times are defined in Phase 6g (Herding mechanics).

---

### Raw Resource Summary

| Category | Count | Common | Uncommon | Rare | Epic | Legendary | Mythic |
|---|---|---|---|---|---|---|---|
| Mining | 30 | 7 | 7 | 5 | 4 | 4 | 3 |
| Logging | 6 | 2 | 1 | 2 | 1 | 0 | 0 |
| Farming | 13 | 8 | 5 | 0 | 0 | 0 | 0 |
| Foraging | 14 | 5 | 3 | 4 | 2 | 0 | 0 |
| Hunting | 11 | 5 | 2 | 2 | 1 | 1 | 0 |
| Fishing | 1 | 1 | 0 | 0 | 0 | 0 | 0 |
| Herding | 12 | 7 | 5 | 0 | 0 | 0 | 0 |
| **Total** | **87** | **35** | **23** | **13** | **8** | **5** | **3** |

> **Rarity distribution by tier**: 35 Common (40%), 23 Uncommon (26%), 13 Rare (15%), 8 Epic (9%), 5 Legendary (6%), 3 Mythic (3%). The distribution tapers sharply — most of the world is common materials, with fantastical resources becoming exponentially rarer.

---

## 2. Core Resources (The Canonical 22)

The 22 Eternum-heritage resources, native to this world. Some are raw resources usable directly (Wood, Stone, Coal, Obsidian); most are refined from raw precursors listed in Section 1.

### Common (3)

Base-level development and production can be achieved with these alone. No refinement required — these are raw resources that serve double duty as core resources.

| # | Resource | Weight | Raw precursor | Refinement | Notes |
|---|---|---|---|---|---|
| C01 | **Wood** | 3.0 | Wood (L01) — same | None (usable directly); Sawmill → Planks | Construction, crafting, fuel. |
| C02 | **Stone** | 3.0 | Stone (M01) — same | None (usable directly); cut → Stone Blocks | Construction, tools. |
| C03 | **Coal** | 1.5 | Coal (M02) — same | None (usable directly as fuel) | Primary smelting fuel. Essential for all metal refining and production. |

### Uncommon (6)

Require smelting or sawmill processing. Enable mid-game tools, weapons, and structures.

| # | Resource | Weight | Raw precursor | Refinement | Notes |
|---|---|---|---|---|---|
| C04 | **Copper** | 2.0 | Copper Ore (M08) | Smelter: Copper Ore + Coal → Copper | Base metal. Tools, weapons. Properties modifiable via crafting system (e.g. combining with Rare Metals or other reagents). |
| C05 | **Obsidian** | 2.0 | Obsidian (M09) — same | None (usable directly); knap → blades | Volcanic glass. Sharp but brittle. |
| C06 | **Silver** | 2.5 | Silver Ore (M10) | Smelter: Silver Ore + Coal → Silver | Precious metal. Jewelry, currency. Flagged: alchemy applications (future). |
| C07 | **Ironwood** | 3.0 | Ironwood Logs (L03) | Sawmill: Ironwood Logs → Ironwood | Dense hardwood, iron-hard. Structural, weapon handles. |
| C08 | **Cold Iron** | 3.0 | Cold Iron Ore (M11) | Smelter: Cold Iron Ore + Coal → Cold Iron | The standard working metal. "Iron" in the Eternal World is Cold Iron — the Eternum heritage name. Weapons, armour, tools, building fittings, structural reinforcement. The backbone of mid-game production. |
| C09 | **Gold** | 4.0 | Gold Ore (M12) | Smelter: Gold Ore + Coal → Gold | Precious. Jewelry, currency, decoration. |

### Rare (4)

Scarce, valuable. Enable advanced crafting and high-tier equipment.

| # | Resource | Weight | Raw precursor | Refinement | Notes |
|---|---|---|---|---|---|
| C10 | **Hartwood** | 2.5 | Hartwood Logs (L04) | Sawmill: Hartwood Logs → Hartwood | Ancient heartwood. Resonant, semi-magical. |
| C11 | **Diamonds** | 0.1 | Rough Diamond (M15) | Jeweler: Rough Diamond → Diamonds | Hardest gemstone. Cutting tools, jewelry. |
| C12 | **Sapphire** | 0.1 | Rough Sapphire (M16) | Jeweler: Rough Sapphire → Sapphire | Blue gemstone. Jewelry, optics. |
| C13 | **Ruby** | 0.1 | Rough Ruby (M17) | Jeweler: Rough Ruby → Ruby | Red gemstone. Jewelry, heat applications. |

### Epic (3)

Specialized biomes and deep nodes only. Enable high-tier and specialised crafting.

| # | Resource | Weight | Raw precursor | Refinement | Notes |
|---|---|---|---|---|---|
| C14 | **Deep Crystal** | 0.3 | Rough Deep Crystal (M20) | Specialized: Crystal Workshop → Deep Crystal | Resonant crystal. Optics, enchantment substrate. |
| C15 | **Ignium** | 0.5 | Ignium Ore (M21) | Specialized: Fire-resistant furnace → Ignium | Fire-aspected. Heat-based crafting, fire-resistant materials. |
| C16 | **Ethereal Silica** | 0.2 | Ethereal Silica Sand (M22) | Specialized: Crystal Workshop → Ethereal Silica | Translucent, semi-ethereal. Magical lens, enchantment. |

### Legendary (3)

Extremely rare, specific conditions. Endgame materials.

| # | Resource | Weight | Raw precursor | Refinement | Notes |
|---|---|---|---|---|---|
| C17 | **True Ice** | 0.8 | True Ice Shard (M24) | Specialized: Cold forge (paradoxical — requires absence of heat) → True Ice | Never melts. Cold-based applications, preservation. |
| C18 | **Twilight Quartz** | 0.2 | Rough Twilight Quartz (M25) | Specialized: Crystal Workshop → Twilight Quartz | Dimensional, shifts colour. Enchantment, optics. |
| C19 | **Alchemical Silver** | 1.5 | Alchemical Silver Ore (M26) | Complex: Ore + reagents + Silver catalyst → Alchemical Silver | Reactive, dangerous. Anti-magical properties. |

### Mythic (3)

Near-unique, endgame-defining. The rarest materials in existence.

| # | Resource | Weight | Raw precursor | Refinement | Notes |
|---|---|---|---|---|---|
| C20 | **Adamantine** | 3.0 | Adamantine Ore (M28) | Master Forge + Voidsand heat + Starmetal equipment → Adamantine | Hardest metal. Near-indestructible. Requires endgame infrastructure to refine. |
| C21 | **Mithral** | 1.0 | Mithral Ore (M29) | Master Forge + Voidsand heat + Starmetal equipment → Mithral | Lightest strong metal. Legendary armour and weapons. Requires endgame infrastructure to refine. |
| C22 | **Dragonhide** | 1.5 | Unearthed Dragonhide (M30) | Specialized curing process (specialized tannery) → Dragonhide | Fossilised dragon hide, excavated from ancient burial sites. Rarest material in the game. |

### Core Resource Summary

| Tier | Count | Resources |
|---|---|---|
| Common | 3 | Wood, Stone, Coal |
| Uncommon | 6 | Copper, Obsidian, Silver, Ironwood, Cold Iron, Gold |
| Rare | 4 | Hartwood, Diamonds, Sapphire, Ruby |
| Epic | 3 | Deep Crystal, Ignium, Ethereal Silica |
| Legendary | 3 | True Ice, Twilight Quartz, Alchemical Silver |
| Mythic | 3 | Adamantine, Mithral, Dragonhide |
| **Total** | **22** | |

---

## 3. Refined & Processed Materials

Produced from raw resources through Refining, Crafting, Cooking, or other processing actions. These are the building blocks of equipment, structures, and consumables. Nothing in this section appears in the world naturally — every refined material must be produced by a player.

### 3a. Metals

Most metals are Core Resources (Section 2) produced directly from ores via smelting. **Alloys are not standalone fungible resources** — they emerge from the crafting system, where combining different metals and reagents during item creation produces items with specific properties. For example, a blade forged with Cold Iron + Ignium + Voidsand might yield a "Firesteel Longsword" with fire-aspected damage. This is defined in the crafting system (Phase 7).

Only one refined metal exists as a standalone resource:

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| R01 | **Starmetal** | 2.0 | Legendary | Starmetal Fragment (M27) + Coal | Master Forge | Meteorite metal. Extremely heat-resistant. Used to forge **top-tier smithing equipment** — anvils, crucibles, tongs, and tools that can withstand the extreme temperatures required for Mythic-tier metalwork. The key that unlocks Adamantine and Mithral production. |

> **Why only one refined metal?** All other metals are captured as Core Resources (Copper, Cold Iron, Silver, Gold, etc.) produced at the smelter. Alloys (bronze, steel, electrum, etc.) are not separate inventory items — they are properties of crafted items determined by the combination of metals, reagents, and techniques used during crafting. This keeps the resource layer clean while enabling a highly dynamic crafting system.

### 3b. Textiles & Fibers

Spun, woven, and treated materials for clothing, armour, cordage, and utility items. Spinning and weaving steps are abstracted into a single refinement action per textile (e.g. Flax → Linen covers spinning and weaving in one process).

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| T01 | **Linen** | 0.5 | Common | Flax (F07) | Spinning Wheel + Loom | Light, breathable fabric. Clothing, bandages, wrappings. |
| T02 | **Wool** | 0.6 | Common | Raw Wool (K03) | Spinning Wheel + Loom | Warm, insulating. Clothing, cold-weather gear. |
| T03 | **Rope** | 0.5 | Common | Hemp (F08) | Twist (no facility required) | Essential utility. Construction, binding, climbing, rigging. |
| T04 | **Canvas** | 0.8 | Common | Hemp (F08) or Flax (F07) | Loom | Heavy cloth. Tents, bags, covers, sails (future). |
| T05 | **Sinew Thread** | 0.2 | Common | Sinew (H05) | Dry + twist (no facility) | Strong natural binding. Bowstrings, stitching, armour binding. |
| T06 | **Leather** | 1.5 | Common | Raw Hide (H02) | Tannery | Armour, bags, belts, straps, sheaths. The core animal product. Tannery facility handles the tanning process. |
| T07 | **Cotton** | 0.4 | Uncommon | Raw Cotton (F09) | Spinning Wheel + Loom | Soft, comfortable. Clothing, fine garments. Warmer biomes only. |
| T08 | **Hardened Leather** | 1.8 | Uncommon | Leather (T06) + Resin (L02) or Tallow (U01) | Tannery | Boiled/treated leather. Better armour rating than standard leather. |
| T09 | **Fur** | 1.0 | Uncommon | Fur Pelt (H07) | Furrier (Workshop) | Cold-weather gear, luxury trade good. Lining, cloaks, trim. |
| T10 | **Silk** | 0.2 | Rare | Spider Silk (G13) | Specialized Loom | Luxury textile. Fine garments, enchantment substrate. Lightweight, strong, and naturally lustrous. |
| T11 | **Exotic Fur** | 1.0 | Epic | Exotic Pelt (H10) | Furrier (Workshop) | Superior cold-weather gear, luxury trade. Spotted, white, or unusual pelts. |
| T12 | **Demonhide** | 2.0 | Legendary | Raw Demonhide (H11) | Specialized Tannery | Cured demon hide. Dark, tough, and faintly warm. Retains residual infernal energy that provides natural heat resistance. High-tier armour material, superior to any standard leather. The curing process requires skilled craftsmanship to neutralise the hide's latent hostility without destroying its properties. |

### 3c. Wood & Plant Refined Products

Processed wood and plant-derived refined materials beyond core resources.

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| W01 | **Planks** | 2.0 | Common | Wood (C01) | Sawmill | Shaped boards. Primary wooden construction material. |
| W02 | **Moonwood** | 3.0 | Epic | Moonwood Logs (L06) | Specialized Sawmill | Processed pale-silver timber. Retains its faint luminescence — structures and items made from Moonwood glow softly at night. Light-aspected. Used in high-tier construction, enchantment substrates, and prestige items. Cool to the touch; doesn't burn easily. |
| W03 | **Spiritbloom Essence** | 0.1 | Epic | Spiritbloom (G12) | Workshop (distillation) | Concentrated magical flower extract. Enchantment catalyst, healing consumables, buff ingredients. Glows faintly. |

### 3d. Building Materials

Construction materials for buildings and settlement structures. Most buildings require a combination of these.

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| B01 | **Stone Blocks** | 4.0 | Common | Stone (C02) | Stonecutter (Workshop) | Cut and dressed stone. Walls, foundations, pillars. |
| B02 | **Bricks** | 2.5 | Common | Clay (M03) + Coal (fuel) | Kiln | Fired clay. Walls, hearths, chimneys, ovens. |
| B03 | **Mortar** | 1.5 | Common | Lime (U06) + Sand (M04) | Workshop | Binds stone and brick. Essential for masonry. |
| B04 | **Nails** | 0.1 | Common | Cold Iron (C08) | Forge | Fasteners. Required for most wooden construction. |
| B05 | **Iron Fittings** | 0.5 | Common | Cold Iron (C08) | Forge | Hinges, brackets, reinforcements, locks. |
| B06 | **Glass** | 0.5 | Uncommon | Sand (M04) + Coal (fuel) | Furnace | Windows, containers, lenses, optics. |
| B07 | **Thatch** | 0.5 | Common | Wheat stalks (F01 byproduct) or foraged reeds | None (gathered as byproduct) | Basic roofing. Cheapest but least durable. |
| B08 | **Pitch** | 0.5 | Common | Resin (L02) | Kiln / distill | Waterproofing, caulking, torches, fire arrows. |

### 3e. Fuels, Chemicals & Utility

Processed substances used as fuel, in production, or as crafting catalysts.

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| U01 | **Tallow** | 0.5 | Common | Raw Fat (H04) | Render (Cookhouse or Workshop) | Candles, soap ingredient, waterproofing, lamp fuel. Also used in Hardened Leather production. |
| U02 | **Oil** | 0.4 | Common | Raw Fat (H04), Nuts (G06), or Fish (FI01) | Press (Workshop) | Lamp fuel, cooking oil, lubrication, leather treatment. |
| U03 | **Fish Oil** | 0.3 | Common | Fish (FI01) | Press (Workshop) | Lamp fuel, waterproofing, leather treatment. Lower quality than nut/fat oil. |
| U04 | **Lye** | 0.3 | Uncommon | Wood ash (from burning Wood) + Water | Workshop | Soap-making, hide processing, cleaning agent. |
| U05 | **Soap** | 0.2 | Uncommon | Tallow (U01) + Lye (U04) | Workshop | Cleaning, hide processing, textile finishing. |
| U06 | **Lime** | 1.0 | Common | Limestone (M05) + Coal (fuel) | Kiln | Mortar base, plaster, whitewash, water purification. |
| U07 | **Flux** | 0.5 | Common | Limestone (M05) | Grind (Workshop) | Smelting additive. Removes impurities from ore during refining. Improves metal quality. |
| U08 | **Dyes** | 0.2 | Uncommon | Dye Plants (F12), minerals (various) | Workshop | Colouring for textiles, leather, and items. Affects cosmetic appearance. |
| U09 | **Vinegar** | 0.3 | Common | Fruit (F04) — fermented | Cookhouse | Preservation, cleaning, hide treatment. |
| U10 | **Bone Meal** | 0.5 | Common | Raw Bone (H03) | Grind (Workshop) | Fertiliser. Accelerates farming fertility recovery when applied to soil. |
| U11 | **Voidsand** | 1.5 | Epic | Voidstone (M23) | Crystal Workshop (grinding + stabilisation) | High-tier smelting reagent. When added to a forge fire, Voidsand produces unnaturally intense heat — far beyond what Coal alone can achieve. **Required for Mythic-tier metalwork**: smelting Adamantine Ore and Mithral Ore is impossible without it. Also enables advanced alloy processes. The sand absorbs ambient light when heated, causing the forge to darken even as temperatures soar. |

### 3f. Processed Food

Intermediate food products used as cooking ingredients or long-lasting provisions. These sit between raw food (Section 1) and cooked meals (Section 5).

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| P01 | **Flour** | 0.5 | Common | Wheat (F01) | Mill (Workshop variant) | Bread, pastry, batter base. Staple ingredient. |
| P02 | **Butter** | 0.3 | Common | Milk (K01) | Churn (Cookhouse) | Cooking ingredient, baking, flavouring. Trade good. |
| P03 | **Cheese** | 0.4 | Common | Milk (K01) | Aging (Cookhouse) | Long-lasting food. High nutrition. Improves with age. |
| P04 | **Dried Meat** | 0.5 | Common | Game Meat (H01) or Beef/Mutton (K05/K06) + Salt (M07) | Smoke/dry (Cookhouse) | Preserved protein. Long shelf life. Travel food. |
| P05 | **Dried Fish** | 0.4 | Common | Fish (FI01) + Salt (M07) | Smoke/dry (Cookhouse) | Preserved. Compact travel food. |
| P06 | **Jam** | 0.3 | Common | Fruit (F04) or Wild Berries (G01) + Sugar (P08) or Honey (G07) | Cookhouse | Preserved fruit. Energy-dense. |
| P07 | **Spices** | 0.1 | Uncommon | Spice Plants (F11) | Dry/grind (no facility) | Cooking catalyst. Increases food buff quality when added to recipes. |
| P08 | **Sugar** | 0.3 | Uncommon | Sugar Cane (F10) | Press + refine (Workshop) | Sweetener, preservation, jam-making, baking. |

---

### Refined Resource Summary

| Sub-category | Count | Common | Uncommon | Rare | Epic | Legendary |
|---|---|---|---|---|---|---|
| Metals | 1 | 0 | 0 | 0 | 0 | 1 |
| Textiles & Fibers | 12 | 5 | 3 | 1 | 1 | 2 |
| Wood & Plant Refined | 3 | 1 | 0 | 0 | 2 | 0 |
| Building Materials | 8 | 7 | 1 | 0 | 0 | 0 |
| Fuels, Chemicals & Utility | 11 | 6 | 3 | 0 | 1 | 0 |
| Processed Food | 8 | 6 | 2 | 0 | 0 | 0 |
| **Total Refined** | **43** | **25** | **9** | **1** | **4** | **3** |

> **Rarity observation**: Refined resources skew heavily Common/Uncommon because they are produced from raw materials that are themselves common or uncommon. The high-rarity refined items (Epic/Legendary) derive from fantastical raw resources and serve as endgame crafting components.
>
> **Why so few refined metals?** Most metals are Core Resources (Section 2) — smelted directly from ore. Alloys (bronze, steel, electrum, etc.) are **emergent properties of the crafting system**, not standalone inventory items. When a smith combines Cold Iron + Ignium + Voidsand at a forge, the resulting weapon inherits fire-aspected properties — the "alloy" is a property of the crafted item, not a separate resource. This is defined in Phase 7 (Crafting).
>
> **Endgame crafting chain**: Voidstone → Voidsand (the heat) + Starmetal Fragment → Starmetal (the equipment) = the two prerequisites for refining Adamantine and Mithral. This creates a natural endgame bottleneck requiring both Epic and Legendary resources.

---

## 4. Beast Parts

Generic material drops from beast encounters. 12 part types with **quality tiers based on beast tier**. A T1 beast drops Common-quality parts; a T5 beast drops Legendary-quality parts.

### Beast Part Quality Tiers

| Beast Tier | Part Quality | Rarity | Drop chance modifier | Crafting power |
|---|---|---|---|---|
| T1 (Common beasts) | **Common** | Common | Base | Basic crafting |
| T2 (Uncommon beasts) | **Fine** | Uncommon | 0.8× | Mid-tier crafting |
| T3 (Rare beasts) | **Superior** | Rare | 0.6× | Advanced crafting |
| T4 (Very Rare beasts) | **Exceptional** | Epic | 0.4× | Endgame crafting |
| T5 (Legendary beasts) | **Legendary** | Legendary | 0.2× | Legendary crafting |

### Beast Part Types

Each beast drops 1–3 part types depending on its lore/physiology (defined in Beast Catalog, Phase 12a). Not every beast drops every part type.

| # | Part | Weight | Use | Typical sources |
|---|---|---|---|---|
| BP01 | **Beast Hide** | 1.5 | Tannery → Beast Leather (superior to normal leather at higher tiers) | Mammals, reptiles, drakes |
| BP02 | **Beast Bone** | 1.0 | Carve → weapons, armour reinforcement; grind → Beast Bone Meal | Most beasts with skeletons |
| BP03 | **Beast Fang** | 0.3 | Weapon crafting (dagger, spear tips); jewelry | Predators, serpents |
| BP04 | **Beast Claw** | 0.3 | Weapon crafting (gauntlets, daggers); tool tips | Predators, birds of prey |
| BP05 | **Beast Eye** | 0.2 | Enchantment component (future); trade | Most beasts |
| BP06 | **Beast Horn** | 0.5 | Weapon/armour crafting; instrument; powder | Horned beasts, demons |
| BP07 | **Beast Sinew** | 0.3 | Superior bowstring; armour binding | Large beasts |
| BP08 | **Beast Blood** | 0.3 | Future alchemy reagent; ink; dye | Most beasts (collected in vial) |
| BP09 | **Beast Dust** | 0.1 | Enchantment catalyst (future); trade | Magical/elemental beasts |
| BP10 | **Beast Scale** | 0.4 | Armour crafting (scale mail); decorative | Reptiles, fish, drakes |
| BP11 | **Beast Venom** | 0.2 | Future alchemy reagent; weapon coating (future) | Serpents, spiders, scorpions |
| BP12 | **Beast Feather** | 0.1 | Arrow fletching (superior); decoration | Birds, griffins, winged beasts |

> **Naming convention**: Displayed with quality prefix: "Fine Beast Fang", "Superior Beast Hide", "Legendary Beast Bone", etc.
>
> **Dragonhide** is NOT a beast part — it is a Core Resource (C22) obtained by mining fossilised dragon remains (Unearthed Dragonhide, M30), not from beast encounters.
>
> **Demonhide** is NOT a beast part — it is a Hunting product (Raw Demonhide, H11) obtained from hunting demons, refined via a specialized tannery into Demonhide (T15).

---

## 5. Cooked Food & Meals

Produced via the Cooking action at a Campfire or Cookhouse. Cooked food provides **energy buffs** and **enhanced nutrition** compared to raw food.

### Meal Tiers

| Tier | Facility | Energy buff | Duration | Typical ingredients |
|---|---|---|---|---|
| **Basic** | Campfire | +5% energy regen | Short (1 in-game hour) | Single raw food + fuel |
| **Standard** | Cookhouse | +10% energy regen | Medium (3 in-game hours) | 2–3 ingredients + fuel + spices optional |
| **Fine** | Cookhouse + Chef trait | +15% energy regen, +5% max energy | Long (6 in-game hours) | 3–4 ingredients + spices + butter/oil |
| **Feast** | Cookhouse + Chef trait + rare ingredients | +20% energy regen, +10% max energy, +5% attribute buff | Full day | 4+ ingredients including rare (honey, truffles, spices) |

### Example Meals

| # | Meal | Tier | Key ingredients | Special effect |
|---|---|---|---|---|
| MF01 | **Roast Meat** | Basic | Game Meat or Beef + fuel | +5% energy regen |
| MF02 | **Grilled Fish** | Basic | Fish + fuel | +5% energy regen |
| MF03 | **Bread** | Standard | Flour + Water + fuel | +10% energy regen; long shelf life |
| MF04 | **Stew** | Standard | Game Meat + Vegetables + Water + fuel | +10% energy regen; +5% health regen |
| MF05 | **Fish Pie** | Standard | Fish + Flour + Butter + fuel | +10% energy regen |
| MF06 | **Hunter's Feast** | Fine | Game Meat + Mushrooms + Wild Herbs + Spices + Butter | +15% energy regen; +5% max energy |
| MF07 | **Honeyed Pastry** | Fine | Flour + Honey + Eggs + Butter + Fruit | +15% energy regen; +25% food buff duration |
| MF08 | **Grand Banquet** | Feast | Multiple meats + Vegetables + Spices + Truffles + Honey + Cheese | +20% energy regen; +10% max energy; +1 to random attribute for duration |

> Full recipe list to be defined in Phase 7c (Food Recipes).

---

## Summary Statistics

| Category | Count | Weight range (kg) | Rarity range |
|---|---|---|---|
| Raw — Mining | 30 | 0.2–5.0 | Common–Mythic |
| Raw — Logging | 6 | 0.3–6.0 | Common–Epic |
| Raw — Farming | 13 | 0.2–0.8 | Common–Uncommon |
| Raw — Foraging | 14 | 0.1–0.5 | Common–Epic |
| Raw — Hunting | 11 | 0.1–2.5 | Common–Legendary |
| Raw — Fishing | 1 | 0.8 | Common |
| Raw — Herding | 12 | 0.2–1.5 | Common–Uncommon |
| **Subtotal Raw** | **87** | | |
| Core T1 (Common) | 3 | 1.5–3.0 | Common |
| Core T2 (Uncommon) | 6 | 0.1–4.0 | Uncommon |
| Core T3 (Rare) | 4 | 0.1–2.5 | Rare |
| Core T4 (Epic) | 3 | 0.2–0.5 | Epic |
| Core T5 (Legendary) | 3 | 0.2–1.5 | Legendary |
| Core T6 (Mythic) | 3 | 1.0–3.0 | Mythic |
| **Subtotal Core** | **22** | | |
| Refined — Metals | 1 | 2.0 | Legendary |
| Refined — Textiles & Fibers | 12 | 0.2–2.0 | Common–Legendary |
| Refined — Wood & Plant | 3 | 0.1–3.0 | Common–Epic |
| Refined — Building Materials | 8 | 0.1–4.0 | Common–Uncommon |
| Refined — Fuels/Chemicals/Utility | 11 | 0.2–1.5 | Common–Epic |
| Refined — Processed Food | 8 | 0.1–0.5 | Common–Uncommon |
| **Subtotal Refined** | **43** | | |
| Beast Parts | 12 (×5 quality tiers = 60 effective) | 0.1–1.5 | Common–Legendary |
| Cooked Meals | 8 examples (full list Phase 7c) | — | — |

---

## Cross-Reference: Supply Chain Validation

Every refined resource traces back to at least one raw resource. Every raw resource with a named refinement path has a corresponding entry in Sections 2 or 3.

### Endgame Crafting Chain

```
Voidstone (M23, Epic) ──→ Voidsand (U11, Epic)
                              │  [unnaturally hot forge fire]
                              ▼
Starmetal Fragment (M27, Leg) → Starmetal (R01, Leg)
                              │  [heat-resistant forging equipment]
                              ▼
                    ┌─────────┴─────────┐
                    ▼                   ▼
Adamantine Ore (M28, Mythic)    Mithral Ore (M29, Mythic)
         → Adamantine (C20)            → Mithral (C21)
```

Both Mythic-tier core resources require **Voidsand** (for heat) and **Starmetal equipment** (for durability) to refine. This creates a natural endgame bottleneck: players must acquire Epic + Legendary materials before they can process Mythic ores.

### Alloys & the Crafting System

Alloys (bronze, steel, electrum, firesteel, etc.) are **not standalone resources**. They are emergent outcomes of the crafting system (Phase 7). When a smith forges an item, the combination of metals, reagents, fuel, and technique determines the resulting item's properties:

| Crafting combination (example) | Likely outcome |
|---|---|
| Cold Iron + Coal | Standard iron weapon/armour |
| Cold Iron + Rare Metals + Coal | Weapon with enhanced durability (bronze-like alloy) |
| Cold Iron + Ignium + Voidsand | Fire-aspected item ("firesteel") |
| Gold + Silver | Electrum jewelry or decorative component |
| Cold Iron + Coal (double) | Hardened steel-equivalent item |

This approach keeps the **resource layer clean** (fewer fungible items to track onchain) while enabling a **highly dynamic crafting system** where material choices matter.

### Universal Discovery Resources

Two "universal discovery" resources exist — low-chance drops from any activity within their category, requiring no dedicated node:

| Resource | Category | Rarity | Drops from |
|---|---|---|---|
| **Rare Metals** (M18) | Mining | Rare | Any mining activity |
| **Worldroot** (L05) | Logging | Rare | Any logging activity |

### Shared Resources Between Actions

Four resources are identical regardless of source action:

| Resource | Hunting source | Herding source |
|---|---|---|
| **Raw Hide** (H02) | Wild fauna | Butchered cattle, sheep, horses |
| **Raw Bone** (H03) | Wild fauna | Butchered livestock (all types) |
| **Raw Fat** (H04) | Wild fauna | Butchered livestock (all types) |
| **Feathers** (H06) | Bird hunting | Poultry herding |

### Resources With No Firm Refinement Path

These raw resources are valued for their raw properties and serve as ingredients rather than feedstock for named refined products:

| Resource | Rarity | Use case |
|---|---|---|
| **Worldroot** (L05) | Rare | Cooking ingredient, future alchemy reagent |
| **Sunsprout** (G14) | Epic | Cooking ingredient, future alchemy reagent |

---

## Design Notes

1. **Eternum heritage**: The 22 core resources retain their Eternum names and relative rarity structure, mapped to the 6-tier rarity system.

2. **Raw-first principle**: Every core and refined resource traces back to a raw precursor gathered through a player action. Base-level development uses common raw resources directly (Wood, Stone, Coal) — no refinement required to get started.

3. **Supply chain depth**: Higher-tier materials require progressively more complex refinement. Common: use directly. Uncommon: basic smelter/sawmill. Rare: jeweler + specialized tools. Epic: specialized workshops. Legendary: extreme conditions. Mythic: master-level facilities + Voidsand heat + Starmetal equipment.

4. **Rare Metals as crafting component**: Instead of multiple real-world alloy metals (tin, nickel, etc.), a single "Rare Metals" resource represents trace metals found at low probability during any mining activity. Used in the crafting system as a reagent to modify item properties — alloys emerge from crafting choices, not as standalone resources.

5. **Worldroot as universal logging discovery**: Mirrors the Rare Metals pattern — a rare resource with no dedicated node, discoverable from any logging activity. Gnarled root growths from an ancient subterranean network.

6. **Dragonhide lore**: Dragons once ruled the land in a prehistoric age. A cataclysm drove them to extinction. Their indestructible hide survives fossilisation and can be excavated from ancient burial sites — making Unearthed Dragonhide a mining resource, not a beast drop.

7. **Demonhide lore**: Demons are rare, dangerous prey. Their hide retains residual infernal energy and must be carefully cured at a specialized tannery. Raw Demonhide is the only Legendary hunting product — reflecting the extreme difficulty of killing a demon.

8. **Moonwood lore**: Pale, silvery timber from trees that have absorbed centuries of moonlight. Found in the oldest forests where canopy gaps allow sustained direct moonlight. Retains faint luminescence after processing — items and structures made from Moonwood glow softly at night.

9. **Voidsand + Starmetal synergy**: The endgame crafting chain requires two non-core fantastical resources working in tandem. Voidsand (from Voidstone, Epic) provides unnaturally intense heat. Starmetal equipment (from Starmetal Fragment, Legendary) can withstand that heat. Together they unlock Mythic-tier metalwork. This creates meaningful endgame progression without gating behind a single bottleneck.

10. **Alloys as crafted properties, not resources**: Bronze, steel, electrum, firesteel, and other alloy types are not standalone fungible resources. They emerge from the crafting system based on which metals, reagents, and techniques a smith uses. This keeps the onchain resource layer lean (fewer token types) while enabling a deeply dynamic crafting experience where material combinations matter. Defined in Phase 7.

11. **Livestock as followers**: Horses and Donkeys are unique herding products — the product IS the living animal, used as a follower. They can be butchered for horsemeat in desperation, but this destroys a high-value transport asset.

12. **Shared animal products**: Raw Hide, Raw Bone, Raw Fat, and Feathers are the same resource regardless of hunting or herding source. This avoids duplicate entries while supporting multiple acquisition paths.

13. **Coal as universal fuel**: Coal is the essential, common fuel for all smelting and production. Voidsand supplements coal for Mythic-tier work — it doesn't replace it.

14. **Cold Iron is iron**: "Iron" in the Eternal World is Cold Iron — the Eternum heritage name. There is no separate "regular iron". Cold Iron Ore is smelted into Cold Iron at a standard smelter. It is the everyday working metal for weapons, armour, tools, and building fittings.

15. **Future-proofing**: Alchemy, Essence, and magical material IDs are registered but have no base-module source. Future modules activate them.

---

*Appendix F — March 2026*
*Phase 3 — Sections 1–3 complete. 87 raw + 22 core + 43 refined = 152 defined resources + 60 effective beast part variants.*
