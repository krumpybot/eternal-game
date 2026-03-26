# Appendix F: Resource Catalog

> Complete resource enumeration for the base module. All resource IDs are assigned at deployment and are immutable. Resources not sourced by any base-module biome or action exist as registered IDs for future modules — they simply don't appear in the wild until a future module introduces a source.
>
> Resources are organized into **three layers**:
> 1. **Raw Resources** — gathered, mined, hunted, foraged, fished, or herded directly from the world. The input layer.
> 2. **Core Resources** — the 22 Eternum-heritage resources. Some are raw (usable directly), most are refined from raw precursors listed in Section 1.
> 3. **Refined & Processed Materials** — derived from raw resources through refining, crafting, or processing actions. The output layer that feeds into equipment, buildings, and consumables.
>
> Additionally: **Beast Parts** (generic, tiered by beast level).
>
> **Alchemy** is deferred to a future module. **Cooking** is part of the crafting system (Phase 7).

---

## Rarity System 🔒

Six tiers, applied consistently across all resource types.

| Tier | Colour | Availability | Core resource examples |
|---|---|---|---|
| **Common** | White | Abundant in most biomes | Wood, Stone, Coal |
| **Uncommon** | Green | Requires specific biomes or moderate effort | Copper, Obsidian, Silver, Cold Iron, Gold |
| **Rare** | Blue | Limited biomes, lower node density | Hartwood, Diamonds, Sapphire, Ruby |
| **Epic** | Purple | Specialized biomes, deep nodes, special areas | Deep Crystal, Ignium, Ethereal Silica |
| **Legendary** | Orange | Extremely rare, specific conditions | True Ice, Twilight Quartz, Alchemical Silver |
| **Mythic** | Red | Near-unique, endgame only | Adamantine, Mithral, Dragonhide |

> Only core resource raw precursors may be Mythic. Non-core raw resources cap at Legendary.

---

## Resource ID Registry

All resources are assigned permanent `u16` IDs at deployment. The full `u16` space is partitioned into major blocks. Sub-ranges within each block are soft guidelines — IDs may be assigned anywhere within their parent block.

**Core Resources are immutable. The 22 assigned IDs will not expand.**

All other categories have generous expansion room for future modules.

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

> **Utilisation**: 153 resources assigned out of 4096 addressable IDs (3.7%). Expansion headroom is generous across all categories.

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

Raw resources are gathered directly from the world through player actions. They are the foundation of the entire supply chain. Some are usable directly (Wood, Stone, Coal, Obsidian, Ironwood, Hartwood, Moonwood); most require refinement.

### 1a. Mining — Ores & Minerals

All entries ordered by rarity tier, then alphabetically within tier.

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| M01 | **Clay** | 2.0 | Common | Kiln → Bricks; Potter → Pottery | Construction, crafting. |
| M02 | **Coal** | 1.5 | Common | Usable directly as fuel | Primary smelting/refining fuel. Essential for all metal production. Also a Core Resource (C03). |
| M03 | **Flint** | 0.5 | Common | Usable directly | Fire-starting, primitive tools. |
| M04 | **Limestone** | 3.0 | Common | Kiln → Lime; grind → Flux | Mortar base, flux, plaster. |
| M05 | **Salt** | 0.5 | Common | Usable directly | Food preservation, trade good. |
| M06 | **Sand** | 2.0 | Common | Furnace → Glass | Glass production, mortar ingredient. |
| M07 | **Stone** | 3.0 | Common | Usable directly; cut → Stone Blocks | Ubiquitous. Construction, tools. Also a Core Resource (C02). |
| M08 | **Cold Iron Ore** | 3.0 | Uncommon | Smelter → Cold Iron (core) | The standard metal ore. "Iron" in the Eternal World is Cold Iron — the Eternum heritage name for the everyday working metal. |
| M09 | **Copper Ore** | 2.5 | Uncommon | Smelter → Copper (core) | T1 metal precursor. |
| M10 | **Gold Ore** | 4.0 | Uncommon | Smelter → Gold (core) | Precious, heavy. Jewelry, currency. |
| M11 | **Obsidian** | 2.0 | Uncommon | Usable directly; knap → Obsidian Blades | Volcanic glass. Sharp but brittle. Also a Core Resource (C05). |
| M12 | **Saltpeter** | 1.0 | Uncommon | Usable directly; future alchemy reagent | Nitrate mineral. |
| M13 | **Silver Ore** | 3.0 | Uncommon | Smelter → Silver (core) | Precious metal. Flagged: alchemy applications (future module). |
| M14 | **Sulfur** | 1.0 | Uncommon | Usable directly; future alchemy reagent | Yellow mineral. |
| M15 | **Amber** | 0.3 | Rare | Jewelry component (decorative); alchemy reagent (future) | Fossilized resin. Jewelry, trade. |
| M16 | **Rare Metals** | 1.0 | Rare | Crafting component — used in the crafting system to modify item properties | Low chance drop from **all** mining activities regardless of node. Represents trace alloy metals (tin, nickel, chromium, etc.). Combined with other metals and reagents during crafting to produce items with enhanced properties (alloys are emergent from the crafting system, not standalone resources). |
| M17 | **Rough Diamond** | 0.2 | Rare | Jeweler → Diamonds (core) | Gem precursor. |
| M18 | **Rough Ruby** | 0.2 | Rare | Jeweler → Ruby (core) | Gem precursor. |
| M19 | **Rough Sapphire** | 0.2 | Rare | Jeweler → Sapphire (core) | Gem precursor. |
| M20 | **Ethereal Silica Sand** | 0.3 | Epic | Specialized refinement → Ethereal Silica (core) | Translucent, semi-ethereal sand found in magical desert formations. |
| M21 | **Ignium Ore** | 1.0 | Epic | Specialized refinement → Ignium (core) | Fire-aspected volcanic ore. |
| M22 | **Rough Deep Crystal** | 0.5 | Epic | Specialized refinement → Deep Crystal (core) | Resonant crystal. Found in deep crystalline deposits. |
| M23 | **Voidstone** | 2.5 | Epic | Specialized refinement → Voidsand (refined) | Dark mineral from deep caves. Absorbs light and energy. Dimensionally unstable. When processed, yields a sand-like reagent that produces unnaturally hot fires. |
| M24 | **Alchemical Silver Ore** | 2.0 | Legendary | Complex refinement + reagents → Alchemical Silver (core) | Reactive, dangerous to refine. Anti-magical properties. |
| M25 | **Rough Twilight Quartz** | 0.4 | Legendary | Specialized refinement → Twilight Quartz (core) | Dimensional crystal that shifts colour. Found in special areas. |
| M26 | **Starmetal Fragment** | 2.0 | Legendary | Specialized refinement → Starmetal (refined) | Meteorite ore. Extremely heat-resistant. Found in impact craters and deep mines. Starmetal is used to forge top-tier smithing equipment — tools, anvils, and crucibles that can withstand the extreme temperatures required for endgame metalwork. |
| M27 | **True Ice Shard** | 1.5 | Legendary | Specialized refinement → True Ice (core) | Glacial ice that never melts. Found in polar and high-mountain biomes. |
| M28 | **Adamantine Ore** | 5.0 | Mythic | Master refinement → Adamantine (core) | Hardest known metal. Deepest mines only. Requires Voidsand heat + Starmetal equipment to smelt. |
| M29 | **Mithral Ore** | 1.5 | Mythic | Master refinement → Mithral (core) | Lightest strong metal. Deepest mines only. Requires Voidsand heat + Starmetal equipment to smelt. |
| M30 | **Unearthed Dragonhide** | 3.0 | Mythic | Specialized curing → Dragonhide (core) | Fossilised dragon hide, excavated from long-dead dragon remains. Dragons once ruled the land in an age before men; a cataclysm drove them to extinction. Their indestructible hide survives fossilisation and can be mined from ancient burial sites. |

> **Mining rarity summary**: 7 Common, 7 Uncommon, 5 Rare, 4 Epic, 4 Legendary, 3 Mythic = **30 mining raw resources**.
>
> **Rare Metals** is unique: it has no dedicated node. Instead, it is a low-chance roll from *any* mining activity regardless of the ore being mined. This makes it naturally rare without requiring specific biome placement.

### 1b. Logging — Timber & Forest Products

Wood resources are named simply: Wood, Ironwood, Hartwood, Moonwood. All are usable directly as raw resources — the rarer woods rarely appear in recipes in raw form, but can be refined into planks for construction and crafting.

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| L01 | **Resin** | 0.5 | Common | Refine → Pitch (waterproofing, fuel) | Sticky sap from conifers. |
| L02 | **Wood** | 3.0 | Common | Usable directly; Sawmill → Wood Planks (refined) | Universal construction and crafting material. Also a Core Resource (C01). |
| L03 | **Ironwood** | 6.0 | Uncommon | Usable directly; Sawmill → Ironwood Planks (refined) | Dense, iron-hard wood. Specific forest biomes. Also a Core Resource (C07). |
| L04 | **Hartwood** | 5.5 | Rare | Usable directly; Sawmill → Hartwood Planks (refined) | Ancient heartwood, resonant. Rare old-growth forests. Also a Core Resource (C10). |
| L05 | **Worldroot** | 0.3 | Rare | No firm refinement path — valued for its raw properties | Gnarled root-like growths occasionally discovered when felling trees. Part of an ancient subterranean root network that connects trees across vast distances — some say across the entire world. Low-chance discovery from **any** logging activity. Potent cooking ingredient and future alchemy reagent. |
| L06 | **Moonwood** | 4.0 | Epic | Usable directly; Specialized Sawmill → Moonwood Planks (refined) | Pale, silvery timber from ancient trees that have absorbed centuries of moonlight. Found only in the oldest forests where canopy gaps allow sustained direct moonlight. The wood has a faint luminescence and is cool to the touch. Prized for its light-aspected properties. |

> **Logging rarity summary**: 2 Common, 1 Uncommon, 2 Rare, 1 Epic = **6 logging raw resources**.
>
> **Worldroot** mirrors **Rare Metals** in acquisition: no dedicated node, low-chance roll from any logging activity. This creates a parallel between mining and logging — both have rare "universal discovery" resources.

### 1c. Farming — Crops & Cultivated Plants

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| F01 | **Barley** | 0.5 | Common | Animal feed; cooking ingredient; brewing (future) | Grain, dual-use. |
| F02 | **Beans** | 0.4 | Common | Cooking ingredient; high protein | Legumes, dried for storage. |
| F03 | **Flax** | 0.3 | Common | Process → Linen (refined) | Primary textile fiber. |
| F04 | **Fruit** | 0.5 | Common | Cooking ingredient; usable raw; preserve → Jam | Orchard crops. |
| F05 | **Hemp** | 0.3 | Common | Twist → Rope; weave → Canvas | Cordage and textile. |
| F06 | **Tubers** | 0.8 | Common | Cooking ingredient; usable raw | Potatoes, turnips, etc. |
| F07 | **Vegetables** | 0.8 | Common | Cooking ingredient; usable raw (low nutrition) | Generic crop category. |
| F08 | **Wheat** | 0.5 | Common | Mill → Flour → Bread, Pastry | The staple grain. |
| F09 | **Raw Cotton** | 0.2 | Uncommon | Process → Cotton (refined) | Finer textile fiber, warmer biomes only. |
| F10 | **Dye Plants** | 0.3 | Uncommon | Process → Dyes | Indigo, woad, madder. |
| F11 | **Hops** | 0.3 | Uncommon | Brewing ingredient (future) | Bittering agent. |
| F12 | **Spice Plants** | 0.2 | Uncommon | Dry/grind → Spices (cooking catalyst) | Pepper, cinnamon, cloves. |
| F13 | **Sugar Cane** | 0.6 | Uncommon | Press → Sugar | Tropical crop. |

> **Farming rarity summary**: 8 Common, 5 Uncommon = **13 farming raw resources**.

### 1d. Foraging — Wild-Gathered

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| G01 | **Mushrooms** | 0.2 | Common | Cooking ingredient; some poisonous | Risk of misidentification. |
| G02 | **Roots** | 0.5 | Common | Cooking ingredient; some have medicinal properties | Food and medicine. |
| G03 | **Seaweed** | 0.3 | Common | Cooking ingredient; fertilizer | Coastal foraging. |
| G04 | **Wild Berries** | 0.3 | Common | Edible raw; cooking ingredient; preserve → Jam | Low nutrition but plentiful. |
| G05 | **Wild Herbs** | 0.1 | Common | Cooking catalyst | Sage, thyme, mint, rosemary. |
| G06 | **Honey** | 0.5 | Uncommon | Cooking ingredient; preservative; sweetener | Wild hive foraging. |
| G07 | **Medicinal Herbs** | 0.1 | Uncommon | Healing poultice ingredient; cooking | Healing applications. |
| G08 | **Nuts** | 0.3 | Uncommon | Edible; cooking ingredient; press → Oil | Seasonal. Specific woodland biomes. |
| G09 | **Poisonous Plants** | 0.1 | Rare | Future alchemy reagent; weapon coating (future) | Dangerous but valuable. Requires knowledge to identify and handle safely. |
| G10 | **Rare Herbs** | 0.1 | Rare | Future alchemy reagent (potent); high-value cooking | Powerful properties. |
| G11 | **Silkworms** | 0.1 | Rare | Process → Silk (refined) | Silk-spinning larvae harvested from cocoons in caves and deep forests. The creatures are reclusive and their nests well-hidden. Raw material for the finest textile. |
| G12 | **Truffles** | 0.1 | Rare | Cooking luxury; high trade value | Rare forest find, prized. |
| G13 | **Spiritbloom** | 0.1 | Epic | Specialized refinement → Spiritbloom Essence (refined) | Luminous magical flower. Grows only in special areas with residual magical energy — near ruins, spawn nodes, or ancient sites. Petals glow faintly. |
| G14 | **Unicorn Hair** | 0.1 | Epic | Process → Goldenthread (refined) | Golden threads shed from the tails and manes of unicorns — creatures no mortal has ever seen. Low-chance discovery from **any** foraging activity. The hairs shimmer faintly and are impossibly strong. |

> **Foraging rarity summary**: 5 Common, 3 Uncommon, 4 Rare, 2 Epic = **14 foraging raw resources**.

### 1e. Hunting — Wild Fauna Products

Products from hunting wild animals. Some products are shared with herding (marked with †).

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| H01 | **Game Meat** | 1.0 | Common | Cooking ingredient; cure → Dried Meat | Wild animal protein. Distinct from livestock meats. |
| H02 | **Raw Bone** † | 1.0 | Common | Carve → Bone Tools; grind → Bone Meal (fertilizer) | Same resource from hunting or herding. |
| H03 | **Raw Fat** † | 0.8 | Common | Render → Tallow (fuel, soap, candles) | Same resource from hunting or herding. |
| H04 | **Raw Hide** † | 2.0 | Common | Tannery → Leather | Same resource from hunting or herding (butchered livestock). |
| H05 | **Sinew** | 0.3 | Common | Dry → Sinew Thread | Natural cordage. Bowstrings, stitching, armour binding. |
| H06 | **Feathers** † | 0.1 | Uncommon | Arrow fletching; stuffing | Bird hunting byproduct. Also from poultry herding. |
| H07 | **Fur Pelt** | 1.5 | Uncommon | Furrier → Fur (refined) | Cold-climate gear, trade good. |
| H08 | **Ivory** | 1.0 | Rare | Carve → Ivory Components; decorative; trade | Large fauna only (tusked beasts). |
| H09 | **Venom Sac** | 0.2 | Rare | Future alchemy reagent; weapon coating (future) | Snakes, spiders, scorpions. Requires careful extraction. |
| H10 | **Exotic Pelt** | 1.5 | Epic | Furrier → Exotic Fur (refined) | Pelts from rare or unusual fauna — spotted cats, white wolves, jungle serpents. Superior to standard fur pelts. Luxury trade good, high-tier cold-weather gear. |
| H11 | **Raw Demonhide** | 2.5 | Legendary | Specialized tannery → Demonhide (refined) | Hide stripped from slain demons. Tough, dark, and faintly warm to the touch. Demons are rare and dangerous prey — hunting one is an extreme feat. The hide retains residual infernal energy and must be carefully cured to be workable. |

> **Hunting rarity summary**: 5 Common, 2 Uncommon, 2 Rare, 1 Epic, 1 Legendary = **11 hunting raw resources**.
>
> † **Shared resources**: Raw Hide, Raw Bone, Raw Fat, and Feathers are the same item regardless of whether they come from hunting wild fauna or herding livestock. The distinction is in the source action, not the material. Four shared items total.

### 1f. Fishing — Aquatic Products

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| FI01 | **Fish** | 0.8 | Common | Cooking ingredient; cure → Dried Fish; press → Oil (refined) | Core food resource. Eternum heritage. Requires water-adjacent hex. |

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
| K01 | **Beef** | 1.5 | Common | Cooking ingredient; cure → Dried Meat | Cattle. Higher nutrition than game meat. |
| K02 | **Eggs** | 0.3 | Common | Cooking ingredient; usable raw (low nutrition) | Poultry. |
| K03 | **Manure** | 1.0 | Common | Usable directly as fertilizer | All livestock produce manure. Accelerates farming fertility recovery. |
| K04 | **Milk** | 0.5 | Common | Churn → Butter/Cheese; cooking ingredient | Cattle. |
| K05 | **Mutton** | 1.2 | Common | Cooking ingredient; cure → Dried Meat | Sheep. |
| K06 | **Poultry** | 0.8 | Common | Cooking ingredient | Poultry (chickens, ducks, geese). |
| K07 | **Raw Wool** | 0.8 | Common | Process → Wool (refined) | Sheep shearing. |
| K08 | **Donkey** | — | Uncommon | N/A — living follower | Pack follower. Raised through herding, bound via Recruiting. +50 kg carry capacity. |
| K09 | **Down** | 0.2 | Uncommon | Stuffing → Bedding, insulation | Goose/duck feathers. |
| K10 | **Horn** | 0.5 | Uncommon | Carve → Horn Components; powder (future alchemy) | Cattle, goats. |
| K11 | **Horse** | — | Uncommon | N/A — living follower | Mount follower. Raised through herding, bound via Recruiting. −20% travel energy/time, +20 kg carry. Max 1 mount per adventurer. |
| K12 | **Horsemeat** | 1.5 | Uncommon | Cooking ingredient (low-value use case) | Horses & donkeys. Destroying a valuable transport animal for food is wasteful but possible in desperation. |

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

> **Rarity distribution by tier**: 35 Common (40%), 23 Uncommon (26%), 13 Rare (15%), 8 Epic (9%), 5 Legendary (6%), 3 Mythic (3%).

---

## 2. Core Resources (The Canonical 22) 🔒

The 22 Eternum-heritage resources, native to this world. Some are raw resources usable directly (Wood, Stone, Coal, Obsidian, Ironwood, Hartwood); most are refined from raw precursors listed in Section 1.

### Common (3)

Base-level development and production can be achieved with these alone. No refinement required — these are raw resources that serve double duty as core resources.

| # | Resource | Weight | Raw precursor | Refinement | Notes |
|---|---|---|---|---|---|
| C01 | **Wood** | 3.0 | Wood (L02) — same | None (usable directly); Sawmill → Wood Planks | Construction, crafting, fuel. |
| C02 | **Stone** | 3.0 | Stone (M07) — same | None (usable directly); cut → Stone Blocks | Construction, tools. |
| C03 | **Coal** | 1.5 | Coal (M02) — same | None (usable directly as fuel) | Primary smelting fuel. Essential for all metal refining and production. |

### Uncommon (6)

Require smelting or are usable directly. Enable mid-game tools, weapons, and structures.

| # | Resource | Weight | Raw precursor | Refinement | Notes |
|---|---|---|---|---|---|
| C04 | **Copper** | 2.0 | Copper Ore (M09) | Smelter: Copper Ore + Coal → Copper | Base metal. Tools, weapons. Properties modifiable via crafting system (e.g. combining with Rare Metals or other reagents). |
| C05 | **Obsidian** | 2.0 | Obsidian (M11) — same | None (usable directly); knap → blades | Volcanic glass. Sharp but brittle. |
| C06 | **Silver** | 2.5 | Silver Ore (M13) | Smelter: Silver Ore + Coal → Silver | Precious metal. Jewelry, currency. Flagged: alchemy applications (future). |
| C07 | **Ironwood** | 3.0 | Ironwood (L03) — same | None (usable directly); Sawmill → Ironwood Planks | Dense hardwood, iron-hard. Structural, weapon handles. |
| C08 | **Cold Iron** | 3.0 | Cold Iron Ore (M08) | Smelter: Cold Iron Ore + Coal → Cold Iron | The standard working metal. "Iron" in the Eternal World is Cold Iron — the Eternum heritage name. Weapons, armour, tools, building fittings, structural reinforcement. The backbone of mid-game production. |
| C09 | **Gold** | 4.0 | Gold Ore (M10) | Smelter: Gold Ore + Coal → Gold | Precious. Jewelry, currency, decoration. |

### Rare (4)

Scarce, valuable. Enable advanced crafting and high-tier equipment.

| # | Resource | Weight | Raw precursor | Refinement | Notes |
|---|---|---|---|---|---|
| C10 | **Hartwood** | 2.5 | Hartwood (L04) — same | None (usable directly); Sawmill → Hartwood Planks | Ancient heartwood. Resonant, semi-magical. |
| C11 | **Diamonds** | 0.1 | Rough Diamond (M17) | Jeweler: Rough Diamond → Diamonds | Hardest gemstone. Cutting tools, jewelry. |
| C12 | **Sapphire** | 0.1 | Rough Sapphire (M19) | Jeweler: Rough Sapphire → Sapphire | Blue gemstone. Jewelry, optics. |
| C13 | **Ruby** | 0.1 | Rough Ruby (M18) | Jeweler: Rough Ruby → Ruby | Red gemstone. Jewelry, heat applications. |

### Epic (3)

Specialized biomes and deep nodes only. Enable high-tier and specialised crafting.

| # | Resource | Weight | Raw precursor | Refinement | Notes |
|---|---|---|---|---|---|
| C14 | **Deep Crystal** | 0.3 | Rough Deep Crystal (M22) | Specialized: Crystal Workshop → Deep Crystal | Resonant crystal. Optics, enchantment substrate. |
| C15 | **Ignium** | 0.5 | Ignium Ore (M21) | Specialized: Fire-resistant furnace → Ignium | Fire-aspected. Heat-based crafting, fire-resistant materials. |
| C16 | **Ethereal Silica** | 0.2 | Ethereal Silica Sand (M20) | Specialized: Crystal Workshop → Ethereal Silica | Translucent, semi-ethereal. Magical lens, enchantment. |

### Legendary (3)

Extremely rare, specific conditions. Endgame materials.

| # | Resource | Weight | Raw precursor | Refinement | Notes |
|---|---|---|---|---|---|
| C17 | **True Ice** | 0.8 | True Ice Shard (M27) | Specialized: Cold forge (paradoxical — requires absence of heat) → True Ice | Never melts. Cold-based applications, preservation. |
| C18 | **Twilight Quartz** | 0.2 | Rough Twilight Quartz (M25) | Specialized: Crystal Workshop → Twilight Quartz | Dimensional, shifts colour. Enchantment, optics. |
| C19 | **Alchemical Silver** | 1.5 | Alchemical Silver Ore (M24) | Complex: Ore + reagents + Silver catalyst → Alchemical Silver | Reactive, dangerous. Anti-magical properties. |

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

Produced from raw resources through Refining, Crafting, or other processing actions. These are the building blocks of equipment, structures, and consumables. Nothing in this section appears in the world naturally — every refined material must be produced by a player.

### 3a. Metals

Most metals are Core Resources (Section 2) produced directly from ores via smelting. **Alloys are not standalone fungible resources** — they emerge from the crafting system, where combining different metals and reagents during item creation produces items with specific properties. For example, a blade forged with Cold Iron + Ignium + Voidsand might yield a "Firesteel Longsword" with fire-aspected damage. This is defined in the crafting system (Phase 7).

Only one refined metal exists as a standalone resource:

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| R01 | **Starmetal** | 2.0 | Legendary | Starmetal Fragment (M26) + Coal | Master Forge | Meteorite metal. Extremely heat-resistant. Used to forge **top-tier smithing equipment** — anvils, crucibles, tongs, and tools that can withstand the extreme temperatures required for Mythic-tier metalwork. The key that unlocks Adamantine and Mithral production. |

> **Why only one refined metal?** All other metals are captured as Core Resources (Copper, Cold Iron, Silver, Gold, etc.) produced at the smelter. Alloys (bronze, steel, electrum, firesteel, etc.) are not separate inventory items — they are properties of crafted items determined by the combination of metals, reagents, and techniques used during crafting. This keeps the resource layer clean while enabling a highly dynamic crafting system.

### 3b. Textiles & Fibers

Spun, woven, and treated materials for clothing, armour, cordage, and utility items. Spinning and weaving steps are abstracted into a single refinement action per textile (e.g. Flax → Linen covers spinning and weaving in one process).

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| T01 | **Canvas** | 0.8 | Common | Hemp (F05) or Flax (F03) | Loom | Heavy cloth. Tents, bags, covers, sails (future). |
| T02 | **Leather** | 1.5 | Common | Raw Hide (H04) | Tannery | Armour, bags, belts, straps, sheaths. The core animal product. Tannery facility handles the tanning process. |
| T03 | **Linen** | 0.5 | Common | Flax (F03) | Spinning Wheel + Loom | Light, breathable fabric. Clothing, bandages, wrappings. |
| T04 | **Rope** | 0.5 | Common | Hemp (F05) | Twist (no facility required) | Essential utility. Construction, binding, climbing, rigging. |
| T05 | **Sinew Thread** | 0.2 | Common | Sinew (H05) | Dry + twist (no facility) | Strong natural binding. Bowstrings, stitching, armour binding. |
| T06 | **Wool** | 0.6 | Common | Raw Wool (K07) | Spinning Wheel + Loom | Warm, insulating. Clothing, cold-weather gear. |
| T07 | **Cotton** | 0.4 | Uncommon | Raw Cotton (F09) | Spinning Wheel + Loom | Soft, comfortable. Clothing, fine garments. Warmer biomes only. |
| T08 | **Fur** | 1.0 | Uncommon | Fur Pelt (H07) | Furrier (Workshop) | Cold-weather gear, luxury trade good. Lining, cloaks, trim. |
| T09 | **Hardened Leather** | 1.8 | Uncommon | Leather (T02) + Resin (L01) or Tallow (U01) | Tannery | Boiled/treated leather. Better armour rating than standard leather. |
| T10 | **Silk** | 0.2 | Rare | Silkworms (G11) | Specialized Loom | Luxury textile. Fine garments, enchantment substrate. Lightweight, strong, and naturally lustrous. |
| T11 | **Exotic Fur** | 1.0 | Epic | Exotic Pelt (H10) | Furrier (Workshop) | Superior cold-weather gear, luxury trade. Spotted, white, or unusual pelts. |
| T12 | **Goldenthread** | 0.1 | Epic | Unicorn Hair (G14) | Specialized Loom | Unbreakable magical thread spun from unicorn hair. Used in high-tier clothing and bowstrings. Garments woven with Goldenthread are impossibly strong yet weightless; bows strung with it never lose tension. Shimmers faintly gold. |
| T13 | **Demonhide** | 2.0 | Legendary | Raw Demonhide (H11) | Specialized Tannery | Cured demon hide. Dark, tough, and faintly warm. Retains residual infernal energy that provides natural heat resistance. High-tier armour material, superior to any standard leather. The curing process requires skilled craftsmanship to neutralise the hide's latent hostility without destroying its properties. |

### 3c. Wood & Plant Refined Products

Planks are the standard refined form of wood resources — used in construction, furniture, and crafting. Rarer woods produce planks with enhanced properties.

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| W01 | **Wood Planks** | 2.0 | Common | Wood (L02/C01) | Sawmill | Shaped boards. Primary wooden construction material. |
| W02 | **Ironwood Planks** | 4.0 | Uncommon | Ironwood (L03/C07) | Sawmill | Dense, heavy planks. Structural reinforcement, fortifications. Nearly as hard as iron. |
| W03 | **Hartwood Planks** | 3.5 | Rare | Hartwood (L04/C10) | Sawmill | Resonant, semi-magical boards. High-tier construction, fine furniture, instrument-making. |
| W04 | **Moonwood Planks** | 3.0 | Epic | Moonwood (L06) | Specialized Sawmill | Pale-silver boards that retain faint luminescence. Structures and items made from Moonwood Planks glow softly at night. Light-aspected. High-tier construction, prestige items. Cool to the touch; doesn't burn easily. |
| W05 | **Spiritbloom Essence** | 0.1 | Epic | Spiritbloom (G13) | Workshop (distillation) | Concentrated magical flower extract. Enchantment catalyst, healing consumables, buff ingredients. Glows faintly. |

### 3d. Building Materials

Construction materials for buildings and settlement structures. Most buildings require a combination of these.

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| B01 | **Bricks** | 2.5 | Common | Clay (M01) + Coal (fuel) | Kiln | Fired clay. Walls, hearths, chimneys, ovens. |
| B02 | **Iron Fittings** | 0.5 | Common | Cold Iron (C08) | Forge | Hinges, brackets, reinforcements, locks. |
| B03 | **Mortar** | 1.5 | Common | Lime (U03) + Sand (M06) | Workshop | Binds stone and brick. Essential for masonry. |
| B04 | **Nails** | 0.1 | Common | Cold Iron (C08) | Forge | Fasteners. Required for most wooden construction. |
| B05 | **Pitch** | 0.5 | Common | Resin (L01) | Kiln / distill | Waterproofing, caulking, torches, fire arrows. |
| B06 | **Stone Blocks** | 4.0 | Common | Stone (M07/C02) | Stonecutter (Workshop) | Cut and dressed stone. Walls, foundations, pillars. |
| B07 | **Glass** | 0.5 | Uncommon | Sand (M06) + Coal (fuel) | Furnace | Windows, containers, lenses, optics. |

### 3e. Fuels, Chemicals & Utility

Processed substances used as fuel, in production, or as crafting catalysts.

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| U01 | **Bone Meal** | 0.5 | Common | Raw Bone (H02) | Grind (Workshop) | Fertiliser. Accelerates farming fertility recovery when applied to soil. |
| U02 | **Flux** | 0.5 | Common | Limestone (M04) | Grind (Workshop) | Smelting additive. Removes impurities from ore during refining. Improves metal quality. |
| U03 | **Lime** | 1.0 | Common | Limestone (M04) + Coal (fuel) | Kiln | Mortar base, plaster, whitewash, water purification. |
| U04 | **Oil** | 0.4 | Common | Raw Fat (H03), Nuts (G08), or Fish (FI01) | Press (Workshop) | Lamp fuel, cooking oil, lubrication, leather treatment, waterproofing. |
| U05 | **Tallow** | 0.5 | Common | Raw Fat (H03) | Render (Cookhouse or Workshop) | Candles, soap ingredient, waterproofing, lamp fuel. Also used in Hardened Leather production. |
| U06 | **Vinegar** | 0.3 | Common | Fruit (F04) — fermented | Cookhouse | Preservation, cleaning, hide treatment. |
| U07 | **Dyes** | 0.2 | Uncommon | Dye Plants (F10), minerals (various) | Workshop | Colouring for textiles, leather, and items. Affects cosmetic appearance. |
| U08 | **Lye** | 0.3 | Uncommon | Wood ash (from burning Wood) + Water | Workshop | Soap-making, hide processing, cleaning agent. |
| U09 | **Soap** | 0.2 | Uncommon | Tallow (U05) + Lye (U08) | Workshop | Cleaning, hide processing, textile finishing. |
| U10 | **Voidsand** | 1.5 | Epic | Voidstone (M23) | Crystal Workshop (grinding + stabilisation) | High-tier smelting reagent. When added to a forge fire, Voidsand produces unnaturally intense heat — far beyond what Coal alone can achieve. **Required for Mythic-tier metalwork**: smelting Adamantine Ore and Mithral Ore is impossible without it. Also enables advanced alloy processes. The sand absorbs ambient light when heated, causing the forge to darken even as temperatures soar. |

### 3f. Processed Food

Intermediate food products used as cooking ingredients or long-lasting provisions. These sit between raw food (Section 1) and cooked meals (defined in Phase 7 — Crafting).

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| P01 | **Butter** | 0.3 | Common | Milk (K04) | Churn (Cookhouse) | Cooking ingredient, baking, flavouring. Trade good. |
| P02 | **Cheese** | 0.4 | Common | Milk (K04) | Aging (Cookhouse) | Long-lasting food. High nutrition. Improves with age. |
| P03 | **Dried Fish** | 0.4 | Common | Fish (FI01) + Salt (M05) | Smoke/dry (Cookhouse) | Preserved. Compact travel food. |
| P04 | **Dried Meat** | 0.5 | Common | Game Meat (H01) or Beef/Mutton (K01/K05) + Salt (M05) | Smoke/dry (Cookhouse) | Preserved protein. Long shelf life. Travel food. |
| P05 | **Flour** | 0.5 | Common | Wheat (F08) | Mill (Workshop variant) | Bread, pastry, batter base. Staple ingredient. |
| P06 | **Jam** | 0.3 | Common | Fruit (F04) or Wild Berries (G04) + Sugar (P08) or Honey (G06) | Cookhouse | Preserved fruit. Energy-dense. |
| P07 | **Spices** | 0.1 | Uncommon | Spice Plants (F12) | Dry/grind (no facility) | Cooking catalyst. Increases food buff quality when added to recipes. |
| P08 | **Sugar** | 0.3 | Uncommon | Sugar Cane (F13) | Press + refine (Workshop) | Sweetener, preservation, jam-making, baking. |

---

### Refined Resource Summary

| Sub-category | Count | Common | Uncommon | Rare | Epic | Legendary |
|---|---|---|---|---|---|---|
| Metals | 1 | 0 | 0 | 0 | 0 | 1 |
| Textiles & Fibers | 13 | 6 | 3 | 1 | 2 | 1 |
| Wood & Plant Refined | 5 | 1 | 1 | 1 | 2 | 0 |
| Building Materials | 7 | 6 | 1 | 0 | 0 | 0 |
| Fuels, Chemicals & Utility | 10 | 6 | 3 | 0 | 1 | 0 |
| Processed Food | 8 | 6 | 2 | 0 | 0 | 0 |
| **Total Refined** | **44** | **25** | **10** | **2** | **5** | **2** |

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
| BP01 | **Beast Blood** | 0.3 | Future alchemy reagent; ink; dye | Most beasts (collected in vial) |
| BP02 | **Beast Bone** | 1.0 | Carve → weapons, armour reinforcement; grind → Beast Bone Meal | Most beasts with skeletons |
| BP03 | **Beast Claw** | 0.3 | Weapon crafting (gauntlets, daggers); tool tips | Predators, birds of prey |
| BP04 | **Beast Dust** | 0.1 | Enchantment catalyst (future); trade | Magical/elemental beasts |
| BP05 | **Beast Eye** | 0.2 | Enchantment component (future); trade | Most beasts |
| BP06 | **Beast Fang** | 0.3 | Weapon crafting (dagger, spear tips); jewelry | Predators, serpents |
| BP07 | **Beast Feather** | 0.1 | Arrow fletching (superior); decoration | Birds, griffins, winged beasts |
| BP08 | **Beast Hide** | 1.5 | Tannery → Beast Leather (superior to normal leather at higher tiers) | Mammals, reptiles, drakes |
| BP09 | **Beast Horn** | 0.5 | Weapon/armour crafting; instrument; powder | Horned beasts, demons |
| BP10 | **Beast Scale** | 0.4 | Armour crafting (scale mail); decorative | Reptiles, fish, drakes |
| BP11 | **Beast Sinew** | 0.3 | Superior bowstring; armour binding | Large beasts |
| BP12 | **Beast Venom** | 0.2 | Future alchemy reagent; weapon coating (future) | Serpents, spiders, scorpions |

> **Naming convention**: Displayed with quality prefix: "Fine Beast Fang", "Superior Beast Hide", "Legendary Beast Bone", etc.
>
> **Dragonhide** is NOT a beast part — it is a Core Resource (C22) obtained by mining fossilised dragon remains (Unearthed Dragonhide, M30), not from beast encounters.
>
> **Demonhide** is NOT a beast part — it is a Hunting product (Raw Demonhide, H11) obtained from hunting demons, refined via a specialized tannery into Demonhide (T13).

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
| Refined — Textiles & Fibers | 13 | 0.1–2.0 | Common–Legendary |
| Refined — Wood & Plant | 5 | 0.1–4.0 | Common–Epic |
| Refined — Building Materials | 7 | 0.1–4.0 | Common–Uncommon |
| Refined — Fuels/Chemicals/Utility | 10 | 0.2–1.5 | Common–Epic |
| Refined — Processed Food | 8 | 0.1–0.5 | Common–Uncommon |
| **Subtotal Refined** | **44** | | |
| Beast Parts | 12 (×5 quality tiers = 60 effective) | 0.1–1.5 | Common–Legendary |

---

## Cross-Reference: Supply Chain Validation

Every refined resource traces back to at least one raw resource. Every raw resource with a named refinement path has a corresponding entry in Sections 2 or 3.

### Endgame Crafting Chain

```
Voidstone (M23, Epic) ──→ Voidsand (U10, Epic)
                              │  [unnaturally hot forge fire]
                              ▼
Starmetal Fragment (M26, Leg) → Starmetal (R01, Leg)
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
| Cold Iron + Coal (high heat) | Hardened steel-equivalent item |

This approach keeps the **resource layer clean** (fewer fungible items to track onchain) while enabling a **highly dynamic crafting system** where material choices matter.

### Universal Discovery Resources

Three "universal discovery" resources exist — low-chance drops from any activity within their category, requiring no dedicated node:

| Resource | Category | Rarity | Drops from |
|---|---|---|---|
| **Rare Metals** (M16) | Mining | Rare | Any mining activity |
| **Worldroot** (L05) | Logging | Rare | Any logging activity |
| **Unicorn Hair** (G14) | Foraging | Epic | Any foraging activity |

### Shared Resources Between Actions

Four resources are identical regardless of source action:

| Resource | Hunting source | Herding source |
|---|---|---|
| **Raw Hide** (H04) | Wild fauna | Butchered cattle, sheep, horses |
| **Raw Bone** (H02) | Wild fauna | Butchered livestock (all types) |
| **Raw Fat** (H03) | Wild fauna | Butchered livestock (all types) |
| **Feathers** (H06) | Bird hunting | Poultry herding |

---

## Design Notes

1. **Eternum heritage**: The 22 core resources retain their Eternum names and relative rarity structure, mapped to the 6-tier rarity system.

2. **Raw-first principle**: Every core and refined resource traces back to a raw precursor gathered through a player action. Base-level development uses common raw resources directly (Wood, Stone, Coal) — no refinement required to get started.

3. **Supply chain depth**: Higher-tier materials require progressively more complex refinement. Common: use directly. Uncommon: basic smelter/sawmill. Rare: jeweler + specialized tools. Epic: specialized workshops. Legendary: extreme conditions. Mythic: master-level facilities + Voidsand heat + Starmetal equipment.

4. **Rare Metals as crafting component**: Instead of multiple real-world alloy metals (tin, nickel, etc.), a single "Rare Metals" resource represents trace metals found at low probability during any mining activity. Used in the crafting system as a reagent to modify item properties — alloys emerge from crafting choices, not as standalone resources.

5. **Worldroot as universal logging discovery**: Mirrors the Rare Metals pattern — a rare resource with no dedicated node, discoverable from any logging activity. Gnarled root growths from an ancient subterranean network.

6. **Dragonhide lore**: Dragons once ruled the land in a prehistoric age. A cataclysm drove them to extinction. Their indestructible hide survives fossilisation and can be excavated from ancient burial sites — making Unearthed Dragonhide a mining resource, not a beast drop.

7. **Demonhide lore**: Demons are rare, dangerous prey. Their hide retains residual infernal energy and must be carefully cured at a specialized tannery. Raw Demonhide is the only Legendary hunting product — reflecting the extreme difficulty of killing a demon.

8. **Moonwood lore**: Pale, silvery timber from trees that have absorbed centuries of moonlight. Found in the oldest forests where canopy gaps allow sustained direct moonlight. Retains faint luminescence after processing — items and structures made from Moonwood Planks glow softly at night.

9. **Voidsand + Starmetal synergy**: The endgame crafting chain requires two non-core fantastical resources working in tandem. Voidsand (from Voidstone, Epic) provides unnaturally intense heat. Starmetal equipment (from Starmetal Fragment, Legendary) can withstand that heat. Together they unlock Mythic-tier metalwork. This creates meaningful endgame progression without gating behind a single bottleneck.

10. **Alloys as crafted properties, not resources**: Bronze, steel, electrum, firesteel, and other alloy types are not standalone fungible resources. They emerge from the crafting system based on which metals, reagents, and techniques a smith uses. This keeps the onchain resource layer lean (fewer token types) while enabling a deeply dynamic crafting experience where material combinations matter. Defined in Phase 7.

11. **Wood resources as a unified family**: Wood, Ironwood, Hartwood, and Moonwood all follow the same pattern — named simply, usable directly as raw resources, refinable into planks for construction and crafting. The rarer woods rarely appear in recipes in raw form but can be used directly in principle.

12. **Unicorn Hair / Goldenthread**: Unicorns are mythical creatures never seen by mortals. They shed golden hairs from their tails and manes, discoverable as a low-chance drop from any foraging activity — the third universal discovery resource (alongside Rare Metals for mining and Worldroot for logging). Goldenthread spun from these hairs is unbreakable magical thread — used for high-tier clothing and bowstrings.

13. **Livestock as followers**: Horses and Donkeys are unique herding products — the product IS the living animal, used as a follower. They can be butchered for horsemeat in desperation, but this destroys a high-value transport asset.

14. **Shared animal products**: Raw Hide, Raw Bone, Raw Fat, and Feathers are the same resource regardless of hunting or herding source. This avoids duplicate entries while supporting multiple acquisition paths.

15. **Coal as universal fuel**: Coal is the essential, common fuel for all smelting and production. Voidsand supplements coal for Mythic-tier work — it doesn't replace it.

16. **Cold Iron is iron**: "Iron" in the Eternal World is Cold Iron — the Eternum heritage name. There is no separate "regular iron". Cold Iron Ore is smelted into Cold Iron at a standard smelter. It is the everyday working metal for weapons, armour, tools, and building fittings.

17. **Cooking is crafting**: Cooking recipes (food → meals with energy buffs) are part of the crafting system defined in Phase 7, not a separate food system. Processed food intermediates (flour, butter, cheese, etc.) remain in this catalog as refined resources.

18. **Future-proofing**: Alchemy, Essence, and magical material IDs are registered but have no base-module source. Future modules activate them. The Resource ID Registry allocates generous expansion room for all categories except Core Resources (fixed at 22).

---

*Appendix F — March 2026*
*Phase 3 — Sections 1–3 complete. 87 raw + 22 core + 44 refined = 153 defined resources + 60 effective beast part variants.*
