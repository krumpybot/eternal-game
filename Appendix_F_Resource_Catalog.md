# Appendix F: Resource Catalog

> Complete resource enumeration for the base module. All resource IDs are assigned at deployment and are immutable. Resources not sourced by any base-module biome or action exist as registered IDs for future modules — they simply don't appear in the wild until a future module introduces a source.
>
> Resources are organized into **three layers**:
> 1. **Raw Resources** — gathered, mined, hunted, foraged, fished, or herded directly from the world. The input layer.
> 2. **Core Resources** — the 22 Eternum-heritage resources. Some are raw (usable directly), most are refined from raw precursors.
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
0x0240–0x027F: Raw Materials — Logging (wood, bark, resin)
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
| M05 | **Limestone** | 3.0 | Common | Kiln → Lime; grind → powder | Mortar base, flux, plaster. |
| M06 | **Flint** | 0.5 | Common | Usable directly | Fire-starting, primitive tools. |
| M07 | **Salt** | 0.5 | Common | Usable directly | Food preservation, trade good. |
| M08 | **Copper Ore** | 2.5 | Uncommon | Smelter → Copper (core) | T1 metal precursor. |
| M09 | **Obsidian** | 2.0 | Uncommon | Usable directly; knap → Obsidian Blades | Volcanic glass. Sharp but brittle. Also a Core Resource (C05). |
| M10 | **Silver Ore** | 3.0 | Uncommon | Smelter → Silver (core) | Precious metal. Flagged: alchemy applications (future module). |
| M11 | **Iron Ore** | 3.0 | Uncommon | Smelter → Iron Ingot (refined); + Rare Metals → Cold Iron (core) | Most important base metal. Dual refinement path. |
| M12 | **Gold Ore** | 4.0 | Uncommon | Smelter → Gold (core) | Precious, heavy. Jewelry, currency. |
| M13 | **Sulfur** | 1.0 | Uncommon | Usable directly; future alchemy reagent | Yellow mineral. |
| M14 | **Saltpeter** | 1.0 | Uncommon | Usable directly; future alchemy reagent | Nitrate mineral. |
| M15 | **Rough Diamond** | 0.2 | Rare | Jeweler → Diamonds (core) | Gem precursor. |
| M16 | **Rough Sapphire** | 0.2 | Rare | Jeweler → Sapphire (core) | Gem precursor. |
| M17 | **Rough Ruby** | 0.2 | Rare | Jeweler → Ruby (core) | Gem precursor. |
| M18 | **Rare Metals** | 1.0 | Rare | Alloy component (Copper + Rare Metals → Bronze; Iron + Rare Metals → Cold Iron) | Low chance drop from **all** mining activities regardless of node. Represents trace alloy metals (tin, nickel, chromium, etc.). |
| M19 | **Amber** | 0.3 | Rare | Jeweler → Polished Amber; alchemy reagent (future) | Fossilized resin. Jewelry, trade. |
| M20 | **Rough Deep Crystal** | 0.5 | Epic | Specialized refinement → Deep Crystal (core) | Resonant crystal. Found in deep crystalline deposits. |
| M21 | **Ignium Ore** | 1.0 | Epic | Specialized refinement → Ignium (core) | Fire-aspected volcanic ore. |
| M22 | **Ethereal Silica Sand** | 0.3 | Epic | Specialized refinement → Ethereal Silica (core) | Translucent, semi-ethereal sand found in magical desert formations. |
| M23 | **Starmetal Fragment** | 2.0 | Epic | Specialized refinement → Starmetal Ingot (refined) | Meteorite ore. Extremely heat-resistant. Found in impact craters and deep mines. |
| M24 | **Bloodstone** | 1.5 | Epic | Specialized refinement → Purified Bloodstone (refined) | Crimson mineral pulsing with life force. Found in volcanic and swamp biomes. Life-aspected. |
| M25 | **Sunstone** | 0.5 | Epic | Specialized refinement → Radiant Sunstone (refined) | Golden crystal that stores and emits light energy. Found at high altitudes and in desert biomes. |
| M26 | **True Ice Shard** | 1.5 | Legendary | Specialized refinement → True Ice (core) | Glacial ice that never melts. Found in polar and high-mountain biomes. |
| M27 | **Rough Twilight Quartz** | 0.4 | Legendary | Specialized refinement → Twilight Quartz (core) | Dimensional crystal that shifts colour. Found in special areas. |
| M28 | **Alchemical Silver Ore** | 2.0 | Legendary | Complex refinement + reagents → Alchemical Silver (core) | Reactive, dangerous to refine. Anti-magical properties. |
| M29 | **Voidstone** | 2.5 | Legendary | Specialized refinement → Refined Voidstone (refined) | Dark mineral from the deepest caves. Absorbs light and energy. Dimensionally unstable. |
| M30 | **Adamantine Ore** | 5.0 | Mythic | Master refinement → Adamantine (core) | Hardest known metal. Deepest mines only. |
| M31 | **Mithral Ore** | 1.5 | Mythic | Master refinement → Mithral (core) | Lightest strong metal. Deepest mines only. |
| M32 | **Raw Dragonhide** | 3.0 | Mythic | Curing process → Dragonhide (core) | Fossilised dragon hide, mined from long-dead dragon remains. Dragons once ruled the land; a cataclysm cut their numbers to endangered levels. Their indestructible hide can be mined from fossilised corpses. |

> **Mining rarity summary**: 7 Common, 7 Uncommon, 5 Rare, 6 Epic, 4 Legendary, 3 Mythic = **32 mining raw resources**.
>
> **Rare Metals** is unique: it has no dedicated node. Instead, it is a low-chance roll from *any* mining activity regardless of the ore being mined. This makes it naturally rare without requiring specific biome placement.

### 1b. Logging — Timber & Forest Products

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| L01 | **Wood** | 3.0 | Common | Usable directly; Sawmill → Planks (refined) | Universal construction and crafting material. Also a Core Resource (C01). |
| L02 | **Bark** | 1.0 | Common | Tanning agent; dry → Cork | Logging byproduct. |
| L03 | **Resin** | 0.5 | Common | Refine → Pitch (waterproofing, fuel) | Sticky sap from conifers. |
| L04 | **Sap** | 0.3 | Common | Refine → Syrup (food sweetener) | Tree sap (maple, birch). |
| L05 | **Ironwood Logs** | 6.0 | Uncommon | Sawmill → Ironwood (core) | Dense, iron-hard wood. Specific forest biomes. |
| L06 | **Hartwood Logs** | 5.5 | Rare | Sawmill → Hartwood (core) | Ancient heartwood, resonant. Rare old-growth forests. |
| L07 | **Worldroot** | 4.0 | Legendary | Specialized refinement → Refined Worldroot (refined) | Petrified wood from ancient magical trees. Resonates with life force. Extremely rare; found only in the oldest, deepest forests or special areas. |

> **Logging rarity summary**: 4 Common, 1 Uncommon, 1 Rare, 0 Epic, 1 Legendary = **7 logging raw resources**.

### 1c. Farming — Crops & Cultivated Plants

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| F01 | **Wheat** | 0.5 | Common | Mill → Flour → Bread, Pastry | The staple grain. |
| F02 | **Barley** | 0.5 | Common | Mill → Malt; animal feed | Grain, dual-use. |
| F03 | **Vegetables** | 0.8 | Common | Cooking ingredient; usable raw (low nutrition) | Generic crop category. |
| F04 | **Fruit** | 0.5 | Common | Cooking ingredient; usable raw; preserve → Jam | Orchard crops. |
| F05 | **Tubers** | 0.8 | Common | Cooking ingredient; usable raw | Potatoes, turnips, etc. |
| F06 | **Beans** | 0.4 | Common | Cooking ingredient; high protein | Legumes, dried for storage. |
| F07 | **Flax** | 0.3 | Common | Spin → Linen Thread → Linen Cloth | Primary textile fiber. |
| F08 | **Hemp** | 0.3 | Common | Twist → Rope; spin → Coarse Cloth | Cordage and textile. |
| F09 | **Cotton** | 0.2 | Uncommon | Spin → Cotton Thread → Cotton Cloth | Finer textile, warmer biomes only. |
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
| G04 | **Wild Herbs** | 0.1 | Common | Cooking catalyst; dry → Dried Herbs | Sage, thyme, mint, rosemary. |
| G05 | **Nuts** | 0.3 | Common | Edible; cooking ingredient; press → Nut Oil | Seasonal. |
| G06 | **Seaweed** | 0.3 | Common | Cooking ingredient; fertilizer | Coastal foraging. |
| G07 | **Honey** | 0.5 | Uncommon | Cooking ingredient; preservative; sweetener | Wild hive foraging. |
| G08 | **Medicinal Herbs** | 0.1 | Uncommon | Healing poultice ingredient; cooking | Healing applications. |
| G09 | **Poisonous Plants** | 0.1 | Uncommon | Future alchemy reagent; weapon coating (future) | Dangerous but valuable. |
| G10 | **Truffles** | 0.1 | Rare | Cooking luxury; high trade value | Rare forest find, prized. |
| G11 | **Rare Herbs** | 0.1 | Rare | Future alchemy reagent (potent); high-value cooking | Powerful properties. |
| G12 | **Spiritbloom** | 0.1 | Epic | Specialized refinement → Spiritbloom Essence (refined) | Luminous magical flower. Grows only in special areas with residual magical energy — near ruins, spawn nodes, or ancient sites. Petals glow faintly. |

> **Foraging rarity summary**: 6 Common, 3 Uncommon, 2 Rare, 1 Epic = **12 foraging raw resources**.

### 1e. Hunting — Wild Fauna Products

Products from hunting wild animals. Some products are shared with herding (marked with †).

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| H01 | **Game Meat** | 1.0 | Common | Cooking → Cooked Meat; cure → Dried Meat | Wild animal protein. Distinct from livestock meats. |
| H02 | **Raw Hide** † | 2.0 | Common | Tannery → Leather | Same resource from hunting or herding (butchered livestock). |
| H03 | **Raw Bone** † | 1.0 | Common | Carve → Bone Tools; grind → Bone Meal (fertilizer) | Same resource from hunting or herding. |
| H04 | **Raw Fat** † | 0.8 | Common | Render → Tallow (fuel, soap, candles) | Same resource from hunting or herding. |
| H05 | **Sinew** | 0.3 | Common | Dry → Bowstring; thread → Sinew Thread | Natural cordage. |
| H06 | **Feathers** | 0.1 | Common | Arrow fletching; stuffing | Bird hunting byproduct. Also from poultry herding. |
| H07 | **Fur Pelt** | 1.5 | Uncommon | Furrier → Fur (refined) | Cold-climate gear, trade good. |
| H08 | **Venom Sac** | 0.2 | Uncommon | Future alchemy reagent; weapon coating (future) | Snakes, spiders, etc. |
| H09 | **Ivory** | 1.0 | Rare | Carve → Ivory Components; decorative; trade | Large fauna only (tusked beasts). |

> **Hunting rarity summary**: 6 Common, 2 Uncommon, 1 Rare = **9 hunting raw resources**.
>
> † **Shared resources**: Raw Hide, Raw Bone, and Raw Fat are the same item regardless of whether they come from hunting wild fauna or butchering herded livestock. The distinction is in the source action, not the material.

### 1f. Fishing — Aquatic Products

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| W01 | **Fish** | 0.8 | Common | Cooking → Cooked Fish; cure → Dried Fish; press → Fish Oil (refined) | Core food resource. Eternum heritage. Requires water-adjacent hex. |

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
| **Poultry** | Poultry, Eggs | Feathers, Raw Bone †, Raw Fat †, Manure |
| **Horses & Donkeys** | Horse (follower), Donkey (follower) | Horsemeat (low value), Raw Hide †, Raw Bone †, Raw Fat †, Manure |

#### Herding Product Table

| # | Resource | Weight | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| K01 | **Milk** | 0.5 | Common | Churn → Butter/Cheese; cooking ingredient | Cattle. |
| K02 | **Eggs** | 0.3 | Common | Cooking ingredient; usable raw (low nutrition) | Poultry. |
| K03 | **Raw Wool** | 0.8 | Common | Spin → Wool Yarn → Wool Cloth | Sheep shearing. |
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
| Mining | 32 | 7 | 7 | 5 | 6 | 4 | 3 |
| Logging | 7 | 4 | 1 | 1 | 0 | 1 | 0 |
| Farming | 13 | 8 | 5 | 0 | 0 | 0 | 0 |
| Foraging | 12 | 6 | 3 | 2 | 1 | 0 | 0 |
| Hunting | 9 | 6 | 2 | 1 | 0 | 0 | 0 |
| Fishing | 1 | 1 | 0 | 0 | 0 | 0 | 0 |
| Herding | 12 | 7 | 5 | 0 | 0 | 0 | 0 |
| **Total** | **86** | **39** | **23** | **9** | **7** | **5** | **3** |

> **Rarity distribution by tier**: 39 Common (45%), 23 Uncommon (27%), 9 Rare (10%), 7 Epic (8%), 5 Legendary (6%), 3 Mythic (3%). The distribution tapers sharply — most of the world is common materials, with fantastical resources becoming exponentially rarer.

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
| C04 | **Copper** | 2.0 | Copper Ore (M08) | Smelter: Copper Ore + Coal → Copper | Base metal. Tools, weapons. Combine with Rare Metals → Bronze. |
| C05 | **Obsidian** | 2.0 | Obsidian (M09) — same | None (usable directly); knap → blades | Volcanic glass. Sharp but brittle. |
| C06 | **Silver** | 2.5 | Silver Ore (M10) | Smelter: Silver Ore + Coal → Silver | Precious metal. Jewelry, currency. Flagged: alchemy applications (future). |
| C07 | **Ironwood** | 3.0 | Ironwood Logs (L05) | Sawmill: Ironwood Logs → Ironwood | Dense hardwood, iron-hard. Structural, weapon handles. |
| C08 | **Cold Iron** | 3.0 | Iron Ore (M11) + Rare Metals (M18) | Smelter (special process): Iron Ore + Rare Metals + Coal → Cold Iron | Anti-beast properties. Superior to standard iron. |
| C09 | **Gold** | 4.0 | Gold Ore (M12) | Smelter: Gold Ore + Coal → Gold | Precious. Jewelry, currency, decoration. |

### Rare (4)

Scarce, valuable. Enable advanced crafting and high-tier equipment.

| # | Resource | Weight | Raw precursor | Refinement | Notes |
|---|---|---|---|---|---|
| C10 | **Hartwood** | 2.5 | Hartwood Logs (L06) | Sawmill: Hartwood Logs → Hartwood | Ancient heartwood. Resonant, semi-magical. |
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
| C17 | **True Ice** | 0.8 | True Ice Shard (M26) | Specialized: Cold forge (paradoxical — requires absence of heat) → True Ice | Never melts. Cold-based applications, preservation. |
| C18 | **Twilight Quartz** | 0.2 | Rough Twilight Quartz (M27) | Specialized: Crystal Workshop → Twilight Quartz | Dimensional, shifts colour. Enchantment, optics. |
| C19 | **Alchemical Silver** | 1.5 | Alchemical Silver Ore (M28) | Complex: Ore + reagents + Silver catalyst → Alchemical Silver | Reactive, dangerous. Anti-magical properties. |

### Mythic (3)

Near-unique, endgame-defining. The rarest materials in existence.

| # | Resource | Weight | Raw precursor | Refinement | Notes |
|---|---|---|---|---|---|
| C20 | **Adamantine** | 3.0 | Adamantine Ore (M30) | Master Forge: Ore → Adamantine (highest difficulty) | Hardest metal. Near-indestructible. |
| C21 | **Mithral** | 1.0 | Mithral Ore (M31) | Master Forge: Ore → Mithral (highest difficulty) | Lightest strong metal. Legendary armour and weapons. |
| C22 | **Dragonhide** | 1.5 | Raw Dragonhide (M32) | Curing process (specialized tannery) → Dragonhide | Fossilised dragon hide, mined from ancient remains. Rarest material in the game. |

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

> ⏳ **To be quantified next.** Section will cover: metals & alloys (Iron Ingot, Steel, Bronze, Electrum, + refined forms of Epic/Legendary raw materials like Starmetal Ingot, Purified Bloodstone, Radiant Sunstone, Refined Voidstone, Refined Worldroot, Spiritbloom Essence), textiles, building materials, fuels/chemicals, and processed food.
>
> Key changes from prior draft:
> - Charcoal removed — Coal is the universal fuel
> - Tin removed — Rare Metals replaces specific alloy metals
> - Fish Oil added as a refined resource (from Fish)
> - Bronze = Copper + Rare Metals (not Copper + Tin)
> - Cold Iron = Iron Ore + Rare Metals (special smelting process)
> - New refined materials needed for: Starmetal Fragment, Bloodstone, Sunstone, Voidstone, Worldroot, Spiritbloom

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
> **Dragonhide** is NOT a beast part — it is a Core Resource (C22) obtained by mining fossilised dragon remains, not from beast encounters.

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
| MF06 | **Hunter's Feast** | Fine | Game Meat + Mushrooms + Herbs + Spices + Butter | +15% energy regen; +5% max energy |
| MF07 | **Honeyed Pastry** | Fine | Flour + Honey + Eggs + Butter + Fruit | +15% energy regen; +25% food buff duration |
| MF08 | **Grand Banquet** | Feast | Multiple meats + Vegetables + Spices + Truffles + Honey + Cheese | +20% energy regen; +10% max energy; +1 to random attribute for duration |

> Full recipe list to be defined in Phase 7c (Food Recipes).

---

## Summary Statistics

| Category | Count | Weight range (kg) | Rarity range |
|---|---|---|---|
| Raw — Mining | 32 | 0.2–5.0 | Common–Mythic |
| Raw — Logging | 7 | 0.3–6.0 | Common–Legendary |
| Raw — Farming | 13 | 0.2–0.8 | Common–Uncommon |
| Raw — Foraging | 12 | 0.1–0.5 | Common–Epic |
| Raw — Hunting | 9 | 0.1–2.0 | Common–Rare |
| Raw — Fishing | 1 | 0.8 | Common |
| Raw — Herding | 12 | 0.2–1.5 | Common–Uncommon |
| **Subtotal Raw** | **86** | | |
| Core T1 (Common) | 3 | 1.5–3.0 | Common |
| Core T2 (Uncommon) | 6 | 0.1–4.0 | Uncommon |
| Core T3 (Rare) | 4 | 0.1–2.5 | Rare |
| Core T4 (Epic) | 3 | 0.2–0.5 | Epic |
| Core T5 (Legendary) | 3 | 0.2–1.5 | Legendary |
| Core T6 (Mythic) | 3 | 1.0–3.0 | Mythic |
| **Subtotal Core** | **22** | | |
| Beast Parts | 12 (×5 quality tiers = 60 effective) | 0.1–1.5 | Common–Legendary |
| Refined Materials | ⏳ TBD (Section 3 pending) | — | — |
| Cooked Meals | 8 examples (full list Phase 7c) | — | — |

---

## Design Notes

1. **Eternum heritage**: The 22 core resources retain their Eternum names and relative rarity structure, mapped to the 6-tier rarity system.

2. **Raw-first principle**: Every core and refined resource traces back to a raw precursor gathered through a player action. Base-level development uses common raw resources directly (Wood, Stone, Coal) — no refinement required to get started.

3. **Supply chain depth**: Higher-tier materials require progressively more complex refinement. Common: use directly. Uncommon: basic smelter/sawmill. Rare: jeweler + specialized tools. Epic: specialized workshops. Legendary: extreme conditions. Mythic: master-level facilities + rare catalysts.

4. **Rare Metals as universal alloy**: Instead of multiple real-world alloy metals (tin, nickel, etc.), a single "Rare Metals" resource represents trace metals found at low probability during any mining activity. This keeps the resource list clean while enabling alloys (Bronze = Copper + Rare Metals; Cold Iron = Iron + Rare Metals).

5. **Dragonhide lore**: Dragons once ruled the land in a manner similar to dinosaurs. A cataclysm reduced them to endangered levels. Their indestructible hide can be mined from long-fossilised corpses — making Raw Dragonhide a mining resource, not a beast drop.

6. **Fantastical raw resources**: Four new Epic resources (Starmetal Fragment, Bloodstone, Sunstone, Spiritbloom) and two new Legendary resources (Worldroot, Voidstone) fill the upper rarity tiers with materials that enable advanced crafting beyond the 22 core Eternum resources.

7. **Livestock as followers**: Horses and Donkeys are unique herding products — the product IS the living animal, used as a follower. They can be butchered for horsemeat in desperation, but this destroys a high-value transport asset.

8. **Shared animal products**: Raw Hide, Raw Bone, Raw Fat, and Feathers are the same resource regardless of hunting or herding source. This avoids duplicate entries while supporting multiple acquisition paths.

9. **Coal as universal fuel**: Charcoal is removed. Coal retains its position as the essential, common fuel for all smelting and production. This keeps the fuel economy simple and makes coal a high-demand trade resource.

10. **Future-proofing**: Alchemy, Essence, and magical material IDs are registered but have no base-module source. Future modules activate them.

---

*Appendix F — March 2026*
*Phase 3 — Sections 1 & 2 complete. Section 3 (Refined & Processed Materials) next.*
