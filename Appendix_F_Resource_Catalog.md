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
| M23 | **Voidstone** | 2.5 | Epic | Specialized refinement → Refined Voidstone (refined) | Dark mineral from deep caves. Absorbs light and energy. Dimensionally unstable. |
| M24 | **Sunstone** | 0.5 | Epic | Specialized refinement → Radiant Sunstone (refined) | Golden crystal that stores and emits light energy. Found at high altitudes and in desert biomes. |
| M25 | **True Ice Shard** | 1.5 | Legendary | Specialized refinement → True Ice (core) | Glacial ice that never melts. Found in polar and high-mountain biomes. |
| M26 | **Rough Twilight Quartz** | 0.4 | Legendary | Specialized refinement → Twilight Quartz (core) | Dimensional crystal that shifts colour. Found in special areas. |
| M27 | **Alchemical Silver Ore** | 2.0 | Legendary | Complex refinement + reagents → Alchemical Silver (core) | Reactive, dangerous to refine. Anti-magical properties. |
| M28 | **Starmetal Fragment** | 2.0 | Legendary | Specialized refinement → Starmetal Ingot (refined) | Meteorite ore. Extremely heat-resistant. Found in impact craters and deep mines. |
| M29 | **Adamantine Ore** | 5.0 | Mythic | Master refinement → Adamantine (core) | Hardest known metal. Deepest mines only. |
| M30 | **Mithral Ore** | 1.5 | Mythic | Master refinement → Mithral (core) | Lightest strong metal. Deepest mines only. |
| M31 | **Raw Dragonhide** | 3.0 | Mythic | Curing process → Dragonhide (core) | Fossilised dragon hide, mined from long-dead dragon remains. Dragons once ruled the land; a cataclysm cut their numbers to endangered levels. Their indestructible hide can be mined from fossilised corpses. |

> **Mining rarity summary**: 7 Common, 7 Uncommon, 5 Rare, 5 Epic, 4 Legendary, 3 Mythic = **31 mining raw resources**.
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
| L07 | **Worldroot** | 4.0 | Epic | Specialized refinement → Refined Worldroot (refined) | Petrified wood from ancient magical trees. Resonates with life force. Found only in the oldest, deepest forests or special areas. |

> **Logging rarity summary**: 4 Common, 1 Uncommon, 1 Rare, 1 Epic = **7 logging raw resources**.

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
| H10 | **Exotic Pelt** | 1.5 | Rare | Furrier → Exotic Fur (refined) | Pelts from rare or unusual fauna — spotted cats, white wolves, jungle serpents. Superior to standard fur pelts. Luxury trade good, high-tier cold-weather gear. |

> **Hunting rarity summary**: 6 Common, 2 Uncommon, 2 Rare = **10 hunting raw resources**.
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
| Mining | 31 | 7 | 7 | 5 | 5 | 4 | 3 |
| Logging | 7 | 4 | 1 | 1 | 1 | 0 | 0 |
| Farming | 13 | 8 | 5 | 0 | 0 | 0 | 0 |
| Foraging | 12 | 6 | 3 | 2 | 1 | 0 | 0 |
| Hunting | 10 | 6 | 2 | 2 | 0 | 0 | 0 |
| Fishing | 1 | 1 | 0 | 0 | 0 | 0 | 0 |
| Herding | 12 | 7 | 5 | 0 | 0 | 0 | 0 |
| **Total** | **86** | **39** | **23** | **10** | **7** | **4** | **3** |

> **Rarity distribution by tier**: 39 Common (45%), 23 Uncommon (27%), 10 Rare (12%), 7 Epic (8%), 4 Legendary (5%), 3 Mythic (3%). The distribution tapers sharply — most of the world is common materials, with fantastical resources becoming exponentially rarer.

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

Produced from raw resources through Refining, Crafting, Cooking, or other processing actions. These are the building blocks of equipment, structures, and consumables. Nothing in this section appears in the world naturally — every refined material must be produced by a player.

### 3a. Metals & Alloys

Base metals, alloys, and fantastical metal refinements. All metal smelting requires **Coal** as fuel.

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| R01 | **Iron Ingot** | 2.5 | Common | Iron Ore (M11) + Coal | Smelter | The workhorse metal. Weapons, armour, tools, building fittings. |
| R02 | **Bronze Ingot** | 2.5 | Uncommon | Copper (C04) + Rare Metals (M18) | Smelter | Harder than copper. Early-mid tier weapons and armour. |
| R03 | **Steel Ingot** | 2.5 | Uncommon | Iron Ingot (R01) + Coal (double fuel) | Smelter (higher temp) | Superior to iron. Mid-tier weapons and armour. Requires double coal. |
| R04 | **Electrum Ingot** | 3.0 | Rare | Gold (C09) + Silver (C06) | Smelter | Gold-silver alloy. Decorative, coinage, jewelry settings. |
| R05 | **Refined Voidstone** | 2.0 | Epic | Voidstone (M23) | Crystal Workshop | Dark, light-absorbing material. Enchantment substrate, stealth-aspected gear, dimensional tools. |
| R06 | **Radiant Sunstone** | 0.4 | Epic | Sunstone (M24) | Crystal Workshop | Polished light-storing crystal. Light sources, warm gear, illumination tools. |
| R07 | **Starmetal Ingot** | 2.0 | Legendary | Starmetal Fragment (M28) + Coal | Master Forge | Meteorite metal. Extremely heat-resistant. Endgame weapons and armour. |

### 3b. Textiles & Fibers

Spun, woven, and treated materials for clothing, armour, cordage, and utility items.

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| T01 | **Linen Thread** | 0.1 | Common | Flax (F07) | Spinning Wheel | Spun fiber, intermediate. |
| T02 | **Linen Cloth** | 0.5 | Common | Linen Thread (T01) | Loom | Light, breathable fabric. Clothing, bandages, wrappings. |
| T03 | **Wool Yarn** | 0.2 | Common | Raw Wool (K03) | Spinning Wheel | Spun fiber, intermediate. |
| T04 | **Wool Cloth** | 0.6 | Common | Wool Yarn (T03) | Loom | Warm, insulating. Clothing, cold-weather gear. |
| T05 | **Rope** | 0.5 | Common | Hemp (F08) | Twist (no facility required) | Essential utility. Construction, binding, climbing, rigging. |
| T06 | **Canvas** | 0.8 | Common | Hemp (F08) or Flax (F07) | Loom | Heavy cloth. Tents, bags, covers, sails (future). |
| T07 | **Sinew Thread** | 0.2 | Common | Sinew (H05) | Dry + twist (no facility) | Strong natural binding. Bowstrings, stitching, armour binding. |
| T08 | **Leather** | 1.5 | Common | Raw Hide (H02) + Bark (L02) as tanning agent | Tannery | Armour, bags, belts, straps, sheaths. The core animal product. |
| T09 | **Cotton Thread** | 0.1 | Uncommon | Cotton (F09) | Spinning Wheel | Fine fiber, intermediate. |
| T10 | **Cotton Cloth** | 0.4 | Uncommon | Cotton Thread (T09) | Loom | Soft, comfortable. Clothing, fine garments. |
| T11 | **Hardened Leather** | 1.8 | Uncommon | Leather (T08) + Resin (L03) or Tallow (U01) | Tannery | Boiled/treated leather. Better armour rating than standard leather. |
| T12 | **Fur** | 1.0 | Uncommon | Fur Pelt (H07) | Furrier (Workshop) | Cold-weather gear, luxury trade good. Lining, cloaks, trim. |
| T13 | **Exotic Fur** | 1.0 | Rare | Exotic Pelt (H10) | Furrier (Workshop) | Superior cold-weather gear, luxury trade. Spotted, white, or unusual pelts. |
| T14 | **Silk** | 0.2 | Rare | Reserved (future: rare foraging or special area) | Specialized | Luxury textile. Fine garments, enchantment substrate. |

### 3c. Wood & Plant Refined Products

Processed wood and plant-derived refined materials beyond core resources.

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| W01 | **Planks** | 2.0 | Common | Wood (C01) | Sawmill | Shaped boards. Primary wooden construction material. |
| W02 | **Refined Worldroot** | 3.0 | Epic | Worldroot (L07) | Crystal Workshop / Specialized Sawmill | Life-resonant processed wood. Living structures, healing items, nature-aspected gear. Retains a faint warmth. |
| W03 | **Spiritbloom Essence** | 0.1 | Epic | Spiritbloom (G12) | Workshop (distillation) | Concentrated magical flower extract. Enchantment catalyst, healing consumables, buff ingredients. Glows faintly. |

### 3d. Building Materials

Construction materials for buildings and settlement structures. Most buildings require a combination of these.

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| B01 | **Stone Blocks** | 4.0 | Common | Stone (C02) | Stonecutter (Workshop) | Cut and dressed stone. Walls, foundations, pillars. |
| B02 | **Bricks** | 2.5 | Common | Clay (M03) + Coal (fuel) | Kiln | Fired clay. Walls, hearths, chimneys, ovens. |
| B03 | **Mortar** | 1.5 | Common | Lime (U06) + Sand (M04) | Workshop | Binds stone and brick. Essential for masonry. |
| B04 | **Nails** | 0.1 | Common | Iron Ingot (R01) | Forge | Fasteners. Required for most wooden construction. |
| B05 | **Iron Fittings** | 0.5 | Common | Iron Ingot (R01) | Forge | Hinges, brackets, reinforcements, locks. |
| B06 | **Glass** | 0.5 | Uncommon | Sand (M04) + Coal (fuel) | Furnace | Windows, containers, lenses, optics. |
| B07 | **Thatch** | 0.5 | Common | Wheat stalks (F01 byproduct) or foraged reeds | None (gathered as byproduct) | Basic roofing. Cheapest but least durable. |
| B08 | **Pitch** | 0.5 | Common | Resin (L03) | Kiln / distill | Waterproofing, caulking, torches, fire arrows. |

### 3e. Fuels, Chemicals & Utility

Processed substances used as fuel, in production, or as crafting catalysts.

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| U01 | **Tallow** | 0.5 | Common | Raw Fat (H04) | Render (Cookhouse or Workshop) | Candles, soap ingredient, waterproofing, lamp fuel. |
| U02 | **Oil** | 0.4 | Common | Raw Fat (H04), Nuts (G05), or Fish (W01) | Press (Workshop) | Lamp fuel, cooking oil, lubrication, leather treatment. |
| U03 | **Fish Oil** | 0.3 | Common | Fish (W01) | Press (Workshop) | Lamp fuel, waterproofing, leather treatment. Lower quality than nut/fat oil. |
| U04 | **Lye** | 0.3 | Uncommon | Wood ash (from burning Wood) + Water | Workshop | Soap-making, hide processing, cleaning agent. |
| U05 | **Soap** | 0.2 | Uncommon | Tallow (U01) + Lye (U04) | Workshop | Cleaning, hide processing, textile finishing. |
| U06 | **Lime** | 1.0 | Common | Limestone (M05) + Coal (fuel) | Kiln | Mortar base, plaster, whitewash, water purification. |
| U07 | **Flux** | 0.5 | Common | Limestone (M05) | Grind (Workshop) | Smelting additive. Removes impurities from ore during refining. Improves metal quality. |
| U08 | **Dyes** | 0.2 | Uncommon | Dye Plants (F12), minerals (various) | Workshop | Colouring for textiles, leather, and items. Affects cosmetic appearance. |
| U09 | **Vinegar** | 0.3 | Common | Fruit (F04) — fermented | Cookhouse | Preservation, cleaning, hide treatment. |
| U10 | **Bone Meal** | 0.5 | Common | Raw Bone (H03) | Grind (Workshop) | Fertiliser. Accelerates farming fertility recovery when applied to soil. |

### 3f. Processed Food

Intermediate food products used as cooking ingredients or long-lasting provisions. These sit between raw food (Section 1) and cooked meals (Section 5).

| # | Resource | Weight | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| P01 | **Flour** | 0.5 | Common | Wheat (F01) | Mill (Workshop variant) | Bread, pastry, batter base. Staple ingredient. |
| P02 | **Butter** | 0.3 | Common | Milk (K01) | Churn (Cookhouse) | Cooking ingredient, baking, flavouring. Trade good. |
| P03 | **Cheese** | 0.4 | Common | Milk (K01) | Aging (Cookhouse) | Long-lasting food. High nutrition. Improves with age. |
| P04 | **Dried Meat** | 0.5 | Common | Game Meat (H01) or Beef/Mutton (K05/K06) + Salt (M07) | Smoke/dry (Cookhouse) | Preserved protein. Long shelf life. Travel food. |
| P05 | **Dried Fish** | 0.4 | Common | Fish (W01) + Salt (M07) | Smoke/dry (Cookhouse) | Preserved. Compact travel food. |
| P06 | **Jam** | 0.3 | Common | Fruit (F04) or Wild Berries (G01) + Sugar (P09) or Honey (G07) | Cookhouse | Preserved fruit. Energy-dense. |
| P07 | **Spices** | 0.1 | Uncommon | Spice Plants (F11) | Dry/grind (no facility) | Cooking catalyst. Increases food buff quality when added to recipes. |
| P08 | **Syrup** | 0.3 | Common | Sap (L04) | Cookhouse (boil/reduce) | Sweetener, cooking ingredient. |
| P09 | **Sugar** | 0.3 | Uncommon | Sugar Cane (F10) | Press + refine (Workshop) | Sweetener, preservation, jam-making, baking. |

---

### Refined Resource Summary

| Sub-category | Count | Common | Uncommon | Rare | Epic | Legendary |
|---|---|---|---|---|---|---|
| Metals & Alloys | 7 | 1 | 2 | 1 | 2 | 1 |
| Textiles & Fibers | 14 | 7 | 4 | 2 | 0 | 0 |
| Wood & Plant Refined | 3 | 1 | 0 | 0 | 2 | 0 |
| Building Materials | 8 | 7 | 1 | 0 | 0 | 0 |
| Fuels, Chemicals & Utility | 10 | 6 | 3 | 0 | 0 | 0 |
| Processed Food | 9 | 7 | 2 | 0 | 0 | 0 |
| **Total Refined** | **51** | **29** | **12** | **3** | **4** | **1** |

> **Rarity observation**: Refined resources skew heavily Common/Uncommon because they are produced from raw materials that are themselves common or uncommon. The high-rarity refined items (Epic/Legendary) derive from the fantastical raw resources and serve as endgame crafting components.

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
| Raw — Mining | 31 | 0.2–5.0 | Common–Mythic |
| Raw — Logging | 7 | 0.3–6.0 | Common–Epic |
| Raw — Farming | 13 | 0.2–0.8 | Common–Uncommon |
| Raw — Foraging | 12 | 0.1–0.5 | Common–Epic |
| Raw — Hunting | 10 | 0.1–2.0 | Common–Rare |
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
| Refined — Metals & Alloys | 7 | 0.4–3.0 | Common–Legendary |
| Refined — Textiles & Fibers | 14 | 0.1–1.8 | Common–Rare |
| Refined — Wood & Plant | 3 | 0.1–3.0 | Common–Epic |
| Refined — Building Materials | 8 | 0.1–4.0 | Common–Uncommon |
| Refined — Fuels/Chemicals/Utility | 10 | 0.2–1.0 | Common–Uncommon |
| Refined — Processed Food | 9 | 0.1–0.5 | Common–Uncommon |
| **Subtotal Refined** | **51** | | |
| Cooked Meals | 8 examples (full list Phase 7c) | — | — |

---

## Design Notes

1. **Eternum heritage**: The 22 core resources retain their Eternum names and relative rarity structure, mapped to the 6-tier rarity system.

2. **Raw-first principle**: Every core and refined resource traces back to a raw precursor gathered through a player action. Base-level development uses common raw resources directly (Wood, Stone, Coal) — no refinement required to get started.

3. **Supply chain depth**: Higher-tier materials require progressively more complex refinement. Common: use directly. Uncommon: basic smelter/sawmill. Rare: jeweler + specialized tools. Epic: specialized workshops. Legendary: extreme conditions. Mythic: master-level facilities + rare catalysts.

4. **Rare Metals as universal alloy**: Instead of multiple real-world alloy metals (tin, nickel, etc.), a single "Rare Metals" resource represents trace metals found at low probability during any mining activity. This keeps the resource list clean while enabling alloys (Bronze = Copper + Rare Metals; Cold Iron = Iron + Rare Metals).

5. **Dragonhide lore**: Dragons once ruled the land in a manner similar to dinosaurs. A cataclysm reduced them to endangered levels. Their indestructible hide can be mined from long-fossilised corpses — making Raw Dragonhide a mining resource, not a beast drop.

6. **Fantastical raw resources**: Epic resources (Voidstone, Sunstone, Worldroot, Spiritbloom) and Legendary resources (Starmetal Fragment) fill the upper rarity tiers alongside core resource precursors, enabling advanced crafting beyond the 22 core Eternum resources.

7. **Livestock as followers**: Horses and Donkeys are unique herding products — the product IS the living animal, used as a follower. They can be butchered for horsemeat in desperation, but this destroys a high-value transport asset.

8. **Shared animal products**: Raw Hide, Raw Bone, Raw Fat, and Feathers are the same resource regardless of hunting or herding source. This avoids duplicate entries while supporting multiple acquisition paths.

9. **Coal as universal fuel**: Coal is the essential, common fuel for all smelting and production. This keeps the fuel economy simple and makes coal a high-demand trade resource. Steel requires double coal (reflecting higher-temperature smelting).

10. **Future-proofing**: Alchemy, Essence, and magical material IDs are registered but have no base-module source. Future modules activate them.

---

*Appendix F — March 2026*
*Phase 3 — Sections 1–3 complete. 86 raw + 22 core + 51 refined = 159 defined resources + 60 effective beast part variants.*
