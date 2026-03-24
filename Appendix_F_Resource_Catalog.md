# Appendix F: Resource Catalog

> Complete resource enumeration for the base module. All resource IDs are assigned at deployment and are immutable. Resources not sourced by any base-module biome or action exist as registered IDs for future modules — they simply don't appear in the wild until a future module introduces a source.
>
> Resources are organized into **three layers**:
> 1. **Raw Resources** — gathered, mined, hunted, foraged, or herded directly from the world. The input layer.
> 2. **Core Resources** — the 22 Eternum-heritage resources. Some are raw (usable directly), most are refined from raw precursors.
> 3. **Refined & Processed Materials** — derived from raw resources through refining, crafting, or processing actions. The output layer that feeds into equipment, buildings, and consumables.
>
> Additionally: **Beast Parts** (generic, tiered by beast level), **Food** (raw + processed), and **Special & Future** (alchemy, essence, reserved IDs).

---

## Resource ID Registry

All resources are assigned permanent `u16` IDs at deployment. Reserved ranges ensure future modules can add resources without base module changes.

```
0x0000–0x003F: Food — Raw (wild, farmed, hunted, fished, herded)
0x0040–0x007F: Food — Processed (cooked meals, preserved, baked)
0x0080–0x00FF: Food — Reserved future
0x0100–0x013F: Core Resources T1 (Common) + raw precursors
0x0140–0x017F: Core Resources T2 (Uncommon) + raw precursors
0x0180–0x01BF: Core Resources T3 (Rare) + raw precursors
0x01C0–0x01FF: Core Resources — Reserved future
0x0200–0x023F: Raw Materials — Mining (ores, minerals, stone)
0x0240–0x027F: Raw Materials — Logging (timber, bark, resin)
0x0280–0x02BF: Raw Materials — Gathering (fibers, herbs, clay)
0x02C0–0x02FF: Raw Materials — Animal (hides, bone, sinew)
0x0300–0x033F: Refined Materials — Metals (ingots, alloys)
0x0340–0x037F: Refined Materials — Textiles (cloth, leather, rope)
0x0380–0x03BF: Refined Materials — Building (planks, bricks, mortar)
0x03C0–0x03FF: Refined Materials — Fuels & Chemicals (charcoal, oil, flux)
0x0400–0x043F: Beast Parts (generic, tiered)
0x0440–0x047F: Alchemy & Reagents
0x0480–0x04FF: Essence & Magical Materials
0x0500–0x0FFF: Reserved (future modules)
```

---

## Resource Properties Schema

Every resource has the following properties:

| Property | Description |
|---|---|
| **ID** | Permanent `u16` identifier |
| **Name** | Display name |
| **Category** | Raw / Core / Refined / Beast Part / Food / Special |
| **Sub-category** | Specific grouping (ore, timber, textile, etc.) |
| **Weight** | kg per unit |
| **Rarity** | Common / Uncommon / Rare / Very Rare / Legendary |
| **Source action** | Which action(s) produce this resource |
| **Refinement path** | What this resource refines into (if any) |
| **Refined from** | What raw resource(s) produce this (if refined) |
| **Tags** | Searchable tags: fuel, food, building, weapon, armor, alchemy, trade, etc. |

---

## 1. Raw Resources

Raw resources are gathered directly from the world through player actions. They are the foundation of the entire supply chain. Some are usable directly (Stone, Coal); most require refinement to become Core or Refined materials.

### 1a. Mining — Ores & Minerals

| # | Resource | Weight (kg) | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| M01 | **Stone** | 3.0 | Common | Usable directly; can be cut → Stone Blocks | Ubiquitous. Construction, tools. |
| M02 | **Coal** | 1.5 | Common | Usable directly as fuel; can process → Charcoal (higher efficiency) | Fuel for smelting, heating. |
| M03 | **Copper Ore** | 2.5 | Common | Smelter → Copper (core) | T1 metal precursor. |
| M04 | **Silver Ore** | 3.0 | Common | Smelter → Silver (core) | T1 precious metal. |
| M05 | **Obsidian** | 2.0 | Common | Usable directly; can knap → Obsidian Shards | Volcanic glass. Weapons, tools. |
| M06 | **Iron Ore** | 3.0 | Uncommon | Smelter → Iron Ingot (refined); special process → Cold Iron (core) | Most important base metal. |
| M07 | **Gold Ore** | 4.0 | Uncommon | Smelter → Gold (core) | Precious, heavy. |
| M08 | **Rough Diamond** | 0.2 | Uncommon | Jeweler → Diamonds (core) | Gem precursor. |
| M09 | **Rough Sapphire** | 0.2 | Uncommon | Jeweler → Sapphire (core) | Gem precursor. |
| M10 | **Rough Ruby** | 0.2 | Uncommon | Jeweler → Ruby (core) | Gem precursor. |
| M11 | **Raw Deep Crystal** | 0.5 | Rare | Specialized refinement → Deep Crystal (core) | T3 crystalline material. |
| M12 | **Ignium Deposit** | 1.0 | Rare | Specialized refinement → Ignium (core) | Volcanic, fire-aspected. |
| M13 | **Raw Ethereal Silica** | 0.3 | Rare | Specialized refinement → Ethereal Silica (core) | Translucent, magical. |
| M14 | **Raw True Ice** | 1.5 | Rare | Specialized refinement → True Ice (core) | Glacial, never melts. |
| M15 | **Raw Twilight Quartz** | 0.4 | Rare | Specialized refinement → Twilight Quartz (core) | Dimensional, shifts color. |
| M16 | **Alchemical Silver Ore** | 2.0 | Rare | Complex refinement → Alchemical Silver (core) | Reactive, dangerous to refine. |
| M17 | **Adamantine Ore** | 5.0 | Very Rare | Master refinement → Adamantine (core) | Hardest known metal. |
| M18 | **Mithral Ore** | 1.5 | Very Rare | Master refinement → Mithral (core) | Light, strong, magical. |
| M19 | **Sulfur** | 1.0 | Uncommon | Usable directly; alchemy reagent | Yellow mineral. Fuel, alchemy. |
| M20 | **Saltpeter** | 1.0 | Uncommon | Usable directly; alchemy reagent | Nitrate mineral. Alchemy, preservation. |
| M21 | **Salt** | 0.5 | Common | Usable directly | Food preservation, trade good, alchemy. |
| M22 | **Clay** | 2.0 | Common | Kiln → Bricks (refined); Potter → Pottery | Construction, crafting. |
| M23 | **Sand** | 2.0 | Common | Furnace → Glass (refined) | Glass production, construction. |
| M24 | **Limestone** | 3.0 | Common | Kiln → Quickite; grind → Calcium powder | Mortar ingredient, flux. |
| M25 | **Tin Ore** | 2.0 | Uncommon | Smelter → Tin Ingot; combine with Copper → Bronze | Alloy component. |
| M26 | **Flint** | 0.5 | Common | Usable directly | Fire-starting, primitive tools. |
| M27 | **Chalk** | 0.5 | Common | Usable directly | Marking, alchemy. |
| M28 | **Ite** | 1.5 | Uncommon | Refinement → various pigments | Mineral dye base. |
| M29 | **Amber** | 0.3 | Rare | Usable directly; jeweler → Polished Amber | Fossilized resin. Jewelry, alchemy. |
| M30 | **Gemite** | 0.3 | Uncommon | Jeweler → Minor Gemstone | Low-tier gem, filler for jewelry. |

### 1b. Logging — Timber & Forest Products

| # | Resource | Weight (kg) | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| L01 | **Timber** | 5.0 | Common | Sawmill → Wood (core: planks, beams, boards) | The universal wood precursor. |
| L02 | **Ironwood Logs** | 6.0 | Uncommon | Sawmill → Ironwood (core) | Dense, iron-hard wood. |
| L03 | **Hartwood Logs** | 5.5 | Uncommon | Sawmill → Hartwood (core) | Ancient heartwood, resonant. |
| L04 | **Bark** | 1.0 | Common | Tanning agent; can dry → Cork | Byproduct of logging. |
| L05 | **Resin** | 0.5 | Common | Refine → Pitch (waterproofing, fuel); alchemy reagent | Sticky sap from conifers. |
| L06 | **Sap** | 0.3 | Common | Refine → Syrup (food); alchemy reagent | Tree sap (maple, birch). |
| L07 | **Charcoal** | 0.8 | Common | Usable directly as fuel | Produced from burning timber in a kiln. Superior fuel. |

### 1c. Farming — Crops & Cultivated Plants

| # | Resource | Weight (kg) | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| F01 | **Wheat** | 0.5 | Common | Mill → Flour → Bread, Pastry | The staple grain. Eternum heritage. |
| F02 | **Barley** | 0.5 | Common | Mill → Malt → Ale (future); animal feed | Grain, dual-use. |
| F03 | **Vegetables** | 0.8 | Common | Cooking ingredient; usable raw (low nutrition) | Generic crop category. |
| F04 | **Fruit** | 0.5 | Common | Cooking ingredient; usable raw; preserve → Jam | Orchard crops. |
| F05 | **Flax** | 0.3 | Common | Spin → Linen Thread → Linen Cloth | Primary textile fiber. |
| F06 | **Hemp** | 0.3 | Common | Twist → Rope; spin → Coarse Cloth | Cordage and textile. |
| F07 | **Cotton** | 0.2 | Uncommon | Spin → Cotton Thread → Cotton Cloth | Finer textile, warmer biomes. |
| F08 | **Tubers** | 0.8 | Common | Cooking ingredient; usable raw | Potatoes, turnips, etc. |
| F09 | **Beans** | 0.4 | Common | Cooking ingredient; high protein | Legumes, dried for storage. |
| F10 | **Sugar Cane** | 0.6 | Uncommon | Press → Sugar; ferment → Rum (future) | Tropical crop. |
| F11 | **Spice Plants** | 0.2 | Uncommon | Dry/grind → Spices (cooking catalyst) | Pepper, cinnamon, cloves. |
| F12 | **Dye Plants** | 0.3 | Uncommon | Process → Dyes | Indigo, woad, madder. |
| F13 | **Hops** | 0.3 | Uncommon | Brewing ingredient (future) | Bittering agent. |

### 1d. Foraging — Wild-Gathered

| # | Resource | Weight (kg) | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| G01 | **Wild Berries** | 0.3 | Common | Edible raw; cooking ingredient; preserve → Jam | Low nutrition but plentiful. |
| G02 | **Roots** | 0.5 | Common | Cooking ingredient; some have alchemy properties | Medicinal and food. |
| G03 | **Mushrooms** | 0.2 | Common | Cooking ingredient; some poisonous; alchemy reagent | Risk of misidentification. |
| G04 | **Wild Herbs** | 0.1 | Common | Cooking catalyst; Alchemy reagent; dry → Dried Herbs | Broad category: sage, thyme, mint, rosemary. |
| G05 | **Medicinal Herbs** | 0.1 | Uncommon | Alchemy reagent; poultice ingredient | Healing applications. |
| G06 | **Wild Flowers** | 0.1 | Common | Dye source; alchemy reagent; decorative | Some are poisonous. |
| G07 | **Moss** | 0.2 | Common | Poultice ingredient; insulation; tinder | Damp forests. |
| G08 | **Seaweed** | 0.3 | Common | Cooking ingredient; alchemy; fertilizer | Coastal foraging. |
| G09 | **Nuts** | 0.3 | Common | Edible; cooking ingredient; press → Nut Oil | Seasonal. |
| G10 | **Honey** | 0.5 | Uncommon | Cooking ingredient; preservative; mead (future) | Wild hive foraging. |
| G11 | **Beeswax** | 0.3 | Uncommon | Candles; waterproofing; alchemy sealant | Byproduct of honey gathering. |
| G12 | **Truffles** | 0.1 | Rare | Cooking luxury; high trade value | Rare forest find, prized. |
| G13 | **Rare Herbs** | 0.1 | Rare | Alchemy reagent (potent) | High-value alchemy ingredients. |
| G14 | **Poisonous Plants** | 0.1 | Uncommon | Alchemy reagent (toxins); weapon coating (future) | Dangerous but valuable. |

### 1e. Hunting — Animal Products (from wild fauna)

| # | Resource | Weight (kg) | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| H01 | **Raw Meat** | 1.0 | Common | Cooking → Cooked Meat; cure → Dried Meat | Primary protein. |
| H02 | **Game Fowl** | 0.8 | Common | Cooking → Roast Fowl | Bird meat. |
| H03 | **Raw Hide** | 2.0 | Common | Tannery → Leather (refined) | Tanning required for crafting use. |
| H04 | **Raw Bone** | 1.0 | Common | Carve → Bone Tools/Crafting component; grind → Bone Meal | Structural, fertilizer. |
| H05 | **Sinew** | 0.3 | Common | Dry → Bowstring; thread → Sinew Thread | Natural cordage, strong. |
| H06 | **Antler** | 0.5 | Uncommon | Carve → Antler Tools; decorative; alchemy | Seasonal drop (deer, elk). |
| H07 | **Feathers** | 0.1 | Common | Arrow fletching; stuffing; quill | Bird hunting byproduct. |
| H08 | **Animal Fat** | 0.8 | Common | Render → Tallow (fuel, soap, candles); cook → Lard | Versatile fat product. |
| H09 | **Fur Pelt** | 1.5 | Uncommon | Furrier → Fur (refined) | Cold-climate gear, trade good. |
| H10 | **Ivory/Tusk** | 1.0 | Rare | Carve → Ivory Components; decorative; trade | Large fauna only. |
| H11 | **Venom Sac** | 0.2 | Uncommon | Alchemy reagent; weapon coating (future) | Snakes, spiders, etc. |

### 1f. Fishing — Aquatic Products

| # | Resource | Weight (kg) | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| W01 | **Fish** | 0.8 | Common | Cooking → Cooked Fish; cure → Dried Fish; press → Fish Oil | Eternum heritage. Core food. |
| W02 | **Shellfish** | 0.5 | Common | Cooking ingredient; dye source (murex) | Coastal/lake. |
| W03 | **Pearl** | 0.1 | Rare | Jeweler → Polished Pearl | Rare drop from shellfish. |
| W04 | **Coral** | 0.3 | Uncommon | Decorative; alchemy; grind → Coral Powder | Coastal only. |
| W05 | **Fish Oil** | 0.3 | Common | Fuel; alchemy; waterproofing | Rendered from fish. |

### 1g. Herding — Livestock Products

| # | Resource | Weight (kg) | Rarity | Refinement path | Notes |
|---|---|---|---|---|---|
| K01 | **Milk** | 0.5 | Common | Churn → Butter/Cheese; cooking ingredient | Dairy livestock. |
| K02 | **Eggs** | 0.3 | Common | Cooking ingredient; usable raw (low nutrition) | Poultry. |
| K03 | **Raw Wool** | 0.8 | Common | Spin → Wool Yarn → Wool Cloth | Sheep/goat shearing. |
| K04 | **Manure** | 1.0 | Common | Usable directly as fertilizer | Byproduct of all livestock. |
| K05 | **Animal Hide** | 2.0 | Common | Tannery → Leather (same as Raw Hide from hunting) | From butchered livestock. |
| K06 | **Livestock Meat** | 1.5 | Common | Cooking → Cooked Meat; cure → Dried Meat | From butchered livestock (beef, pork, mutton). |
| K07 | **Livestock Bone** | 1.0 | Common | Same refinement as Raw Bone | Byproduct of butchering. |
| K08 | **Livestock Fat** | 0.8 | Common | Same as Animal Fat (→ Tallow) | Byproduct of butchering. |
| K09 | **Horn** | 0.5 | Uncommon | Carve → Horn Components; powder → Alchemy | Cattle, goat. |
| K10 | **Down** | 0.2 | Uncommon | Stuffing → Bedding, insulation | Goose/duck feathers. |

> **Note on livestock vs hunting products:** Many animal products overlap (hide, bone, fat, meat). The game treats them as **the same resource** regardless of source — Raw Hide from hunting and Animal Hide from herding produce the same Leather. The distinction is in **how they're obtained** (hunting action vs herding action), not what they are.

---

## 2. Core Resources (The Canonical 22)

The 22 Eternum-heritage resources, native to this world. These are the **finished/usable** forms — most are refined from raw precursors listed in Section 1.

### Tier 1 — Common

Mineable or harvestable with basic tools. T1 resources form the backbone of early-game economy.

| # | Resource | Weight (kg) | Raw precursor | Refinement | Notes |
|---|---|---|---|---|---|
| C01 | **Wood** | 2.0 | Timber (L01) | Sawmill: Timber → Wood (planks/beams) | Universal building and crafting material. |
| C02 | **Stone** | 3.0 | Stone (M01) — same | None (usable raw); can cut → Stone Blocks | Construction, tools. |
| C03 | **Coal** | 1.5 | Coal (M02) — same | None (usable raw as fuel) | Smelting fuel, heating. |
| C04 | **Copper** | 2.0 | Copper Ore (M03) | Smelter: Copper Ore → Copper | Base metal. Tools, weapons, alloys. |
| C05 | **Obsidian** | 2.0 | Obsidian (M05) — same | None (usable raw); knap → blades | Volcanic glass. Sharp but brittle. |
| C06 | **Silver** | 2.5 | Silver Ore (M04) | Smelter: Silver Ore → Silver | Precious metal. Jewelry, currency, alchemy. |

### Tier 2 — Uncommon

Require refinement or specialized harvesting. T2 resources enable mid-game crafting and building.

| # | Resource | Weight (kg) | Raw precursor | Refinement | Notes |
|---|---|---|---|---|---|
| C07 | **Ironwood** | 3.0 | Ironwood Logs (L02) | Sawmill: Ironwood Logs → Ironwood | Dense hardwood, iron-hard. Structural, handles. |
| C08 | **Cold Iron** | 3.0 | Iron Ore (M06) — special process | Smelter (cold forge): Iron Ore + catalyst → Cold Iron | Magical resistance, anti-beast properties. |
| C09 | **Gold** | 4.0 | Gold Ore (M07) | Smelter: Gold Ore → Gold | Precious. Jewelry, currency, decoration. |
| C10 | **Hartwood** | 2.5 | Hartwood Logs (L03) | Sawmill: Hartwood Logs → Hartwood | Ancient heartwood. Resonant, semi-magical. |
| C11 | **Diamonds** | 0.1 | Rough Diamond (M08) | Jeweler: Rough Diamond → Diamonds | Gemstone. Jewelry, cutting tools. |
| C12 | **Sapphire** | 0.1 | Rough Sapphire (M09) | Jeweler: Rough Sapphire → Sapphire | Gemstone. Jewelry, optics. |
| C13 | **Ruby** | 0.1 | Rough Ruby (M10) | Jeweler: Rough Ruby → Ruby | Gemstone. Jewelry, heat applications. |

### Tier 3 — Rare

All require specialized refinement. T3 resources enable endgame crafting. Extremely limited in supply.

| # | Resource | Weight (kg) | Raw precursor | Refinement | Notes |
|---|---|---|---|---|---|
| C14 | **Deep Crystal** | 0.3 | Raw Deep Crystal (M11) | Specialized: Raw → Deep Crystal | Resonant crystal. Optics, enchantment. |
| C15 | **Ignium** | 0.5 | Ignium Deposit (M12) | Specialized: Deposit → Ignium | Fire-aspected. Heat-based crafting. |
| C16 | **Ethereal Silica** | 0.2 | Raw Ethereal Silica (M13) | Specialized: Raw → Ethereal Silica | Translucent, semi-ethereal. |
| C17 | **True Ice** | 0.8 | Raw True Ice (M14) | Specialized: Raw → True Ice | Never melts. Cold-based applications. |
| C18 | **Twilight Quartz** | 0.2 | Raw Twilight Quartz (M15) | Specialized: Raw → Twilight Quartz | Dimensional, shifts color. |
| C19 | **Alchemical Silver** | 1.5 | Alchemical Silver Ore (M16) | Complex: Ore + reagents → Alchemical Silver | Reactive, dangerous. Anti-magical properties. |
| C20 | **Adamantine** | 3.0 | Adamantine Ore (M17) | Master: Ore → Adamantine (highest difficulty) | Hardest metal. Near-indestructible. |
| C21 | **Mithral** | 1.0 | Mithral Ore (M18) | Master: Ore → Mithral (highest difficulty) | Lightest strong metal. Legendary. |
| C22 | **Dragonhide** | 1.5 | Dragonhide — same | None (obtained in finished form from T5 beast encounters or legendary special areas) | Rarest material in the game. |

> **T3 refinement hierarchy**: Deep Crystal through Twilight Quartz require a specialized facility (Crystal Workshop or equivalent). Alchemical Silver requires an Alchemist's Lab + reagents. Adamantine and Mithral require a Master Forge (highest-tier smelter) + Coal/Charcoal in quantity. Dragonhide is the sole material obtained in finished form — it needs no refinement but may need curing.

---

## 3. Refined & Processed Materials

Produced from raw resources through Refining, Crafting, or other processing actions. These are the building blocks of equipment, structures, and consumables.

### 3a. Metals & Alloys

| # | Resource | Weight (kg) | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| R01 | **Iron Ingot** | 2.5 | Common | Iron Ore (M06) | Smelter | The workhorse metal. Weapons, armor, tools, nails. |
| R02 | **Steel Ingot** | 2.5 | Uncommon | Iron Ingot + Coal/Charcoal | Smelter (higher temp) | Superior to iron. Mid-tier weapons and armor. |
| R03 | **Bronze Ingot** | 2.5 | Common | Copper (C04) + Tin Ore (M25) | Smelter | Early alloy. Harder than copper. |
| R04 | **Tin Ingot** | 2.0 | Uncommon | Tin Ore (M25) | Smelter | Soft metal, primarily for alloying. |
| R05 | **Electrum Ingot** | 3.0 | Uncommon | Gold (C09) + Silver (C06) | Smelter | Gold-silver alloy. Decorative, coinage. |

### 3b. Textiles & Fibers

| # | Resource | Weight (kg) | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| T01 | **Linen Thread** | 0.1 | Common | Flax (F05) | Spinning Wheel | Spun fiber, intermediate. |
| T02 | **Linen Cloth** | 0.5 | Common | Linen Thread (T01) | Loom | Light, breathable fabric. Clothing, bandages. |
| T03 | **Wool Yarn** | 0.2 | Common | Raw Wool (K03) | Spinning Wheel | Spun fiber, intermediate. |
| T04 | **Wool Cloth** | 0.6 | Common | Wool Yarn (T03) | Loom | Warm, insulating. Clothing, cold-weather gear. |
| T05 | **Cotton Thread** | 0.1 | Uncommon | Cotton (F07) | Spinning Wheel | Fine fiber, intermediate. |
| T06 | **Cotton Cloth** | 0.4 | Uncommon | Cotton Thread (T05) | Loom | Soft, comfortable. Clothing, sails (future). |
| T07 | **Rope** | 0.5 | Common | Hemp (F06) | Twist (no facility) | Essential utility item. Construction, binding. |
| T08 | **Leather** | 1.5 | Common | Raw Hide (H03) or Animal Hide (K05) | Tannery | Armor, bags, belts, straps. |
| T09 | **Hardened Leather** | 1.8 | Uncommon | Leather (T08) + Resin (L05) or Beeswax (G11) | Tannery | Boiled/treated. Better armor rating. |
| T10 | **Fur** | 1.0 | Uncommon | Fur Pelt (H09) | Furrier (Workshop) | Cold-weather gear, luxury trade. |
| T11 | **Sinew Thread** | 0.2 | Common | Sinew (H05) | Dry + twist (no facility) | Bowstring, binding, stitching. |
| T12 | **Silk** | 0.2 | Rare | Reserved (future: silkworm herding or rare foraging) | Specialized | Luxury textile. |
| T13 | **Canvas** | 0.8 | Common | Hemp (F06) or Flax (F05) | Loom | Heavy cloth. Tents, sails, bags. |

### 3c. Building Materials

| # | Resource | Weight (kg) | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| B01 | **Planks** | 2.0 | Common | Timber (L01) → Wood (C01) → Planks | Sawmill | Shaped boards for construction. |
| B02 | **Stone Blocks** | 4.0 | Common | Stone (M01/C02) | Stonecutter (Workshop) | Cut stone for walls, foundations. |
| B03 | **Bricks** | 2.5 | Common | Clay (M22) | Kiln | Fired clay. Walls, hearths, chimneys. |
| B04 | **Mortar** | 1.5 | Common | Limestone (M24) + Sand (M23) + Water | Kiln/Workshop | Binds stone and brick. |
| B05 | **Nails** | 0.1 | Common | Iron Ingot (R01) | Forge | Fasteners. Needed for most wooden construction. |
| B06 | **Iron Fittings** | 0.5 | Common | Iron Ingot (R01) | Forge | Hinges, brackets, reinforcements. |
| B07 | **Glass** | 0.5 | Uncommon | Sand (M23) + fuel | Furnace | Windows, containers, optics. |
| B08 | **Thatch** | 0.5 | Common | Wheat stalks (F01 byproduct) or Reeds | None (gathered) | Basic roofing. |
| B09 | **Lime** | 1.0 | Common | Limestone (M24) | Kiln | Mortar base, plaster, whitewash. |
| B10 | **Pitch** | 0.5 | Common | Resin (L05) | Kiln/distill | Waterproofing, caulking, torches. |

### 3d. Fuels & Chemicals

| # | Resource | Weight (kg) | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| U01 | **Charcoal** | 0.8 | Common | Timber (L01) | Charcoal Kiln | Superior smelting fuel. Burns hotter/cleaner than coal. |
| U02 | **Tallow** | 0.5 | Common | Animal Fat (H08/K08) | Render (Cookhouse or Workshop) | Candles, soap, waterproofing, fuel. |
| U03 | **Oil** | 0.4 | Common | Animal Fat, Nuts (G09), or Fish (W01) | Press (Workshop) | Fuel for lamps, cooking, lubrication. |
| U04 | **Lye** | 0.3 | Uncommon | Ash (from burning wood) + Water | Workshop | Soap-making, hide processing, cleaning. |
| U05 | **Flux** | 0.5 | Common | Limestone (M24) or Borite | Grind (Workshop) | Smelting addite that removes impurities. |
| U06 | **Dyes** | 0.2 | Uncommon | Dye Plants (F12), Shellfish (W02), Minerals (M28) | Workshop | Coloring for textiles and items. |
| U07 | **Vinegar** | 0.3 | Common | Fermented Fruit (F04) | Cookhouse | Preservation, cleaning, alchemy. |
| U08 | **Soap** | 0.2 | Common | Tallow (U02) + Lye (U04) | Workshop | Cleaning, hide processing. |
| U09 | **Wax** | 0.2 | Uncommon | Beeswax (G11) | Melt (Cookhouse) | Candles, waterproofing, sealant. |
| U10 | **Gunite Powder** | 0.3 | Rare | Sulfur (M19) + Saltpeter (M20) + Charcoal (U01) | Alchemy Lab | Reserved for future modules (firearms, explosives). |

### 3e. Processed Food

| # | Resource | Weight (kg) | Rarity | Refined from | Facility | Notes |
|---|---|---|---|---|---|---|
| P01 | **Flour** | 0.5 | Common | Wheat (F01) | Mill (Workshop variant) | Bread, pastry base. |
| P02 | **Butter** | 0.3 | Common | Milk (K01) | Churn (Cookhouse) | Cooking ingredient, trade good. |
| P03 | **Cheese** | 0.4 | Common | Milk (K01) | Aging (Cookhouse) | Long-lasting food, high nutrition. |
| P04 | **Dried Meat** | 0.5 | Common | Raw Meat (H01) or Livestock Meat (K06) + Salt (M21) | Smoke/dry (Cookhouse) | Preserved protein. Long shelf life. |
| P05 | **Dried Fish** | 0.4 | Common | Fish (W01) + Salt (M21) | Smoke/dry (Cookhouse) | Preserved. Travel food. |
| P06 | **Jam** | 0.3 | Common | Fruit (F04) or Wild Berries (G01) + Sugar/Honey | Cookhouse | Preserved fruit. Energy buff. |
| P07 | **Spices** | 0.1 | Uncommon | Spice Plants (F11) | Dry/grind (no facility) | Cooking catalyst. Increases food buff quality. |
| P08 | **Sugar** | 0.3 | Uncommon | Sugar Cane (F10) | Press + refine (Workshop) | Sweetener, preservation, cooking. |
| P09 | **Bone Meal** | 0.5 | Common | Raw Bone (H04) or Livestock Bone (K07) | Grind (Workshop) | Fertilizer. Accelerates farming fertility recovery. |

---

## 4. Beast Parts

Generic material drops from beast encounters. Rather than 75 beast-specific resources, beast parts are **generic categories** with **quality tiers based on beast tier**. A T1 beast drops T1-quality parts; a T5 beast drops T5-quality parts.

### Beast Part Quality Tiers

| Beast Tier | Part Quality | Rarity | Drop chance modifier | Crafting power |
|---|---|---|---|---|
| T1 (Common beasts) | **Common** | Common | Base | Basic crafting |
| T2 (Uncommon beasts) | **Fine** | Uncommon | 0.8× | Mid-tier crafting |
| T3 (Rare beasts) | **Superior** | Rare | 0.6× | Advanced crafting |
| T4 (Very Rare beasts) | **Exceptional** | Very Rare | 0.4× | Endgame crafting |
| T5 (Legendary beasts) | **Legendary** | Legendary | 0.2× | Legendary crafting |

> Drop chance decreases with tier — higher-tier beasts are rarer and their parts are more valuable. A T5 beast always drops *something*, but the chance of getting the rarest part types is lower.

### Beast Part Types

Each beast may drop 1–3 part types depending on its lore/physiology (defined in the Beast Catalog, Phase 12a). Not every beast drops every part type.

| # | Part | Weight (kg) | Use | Which beasts drop it |
|---|---|---|---|---|
| BP01 | **Beast Hide** | 1.5 | Tannery → Beast Leather (superior to normal leather at higher tiers) | Mammals, reptiles, drakes |
| BP02 | **Beast Bone** | 1.0 | Carve → weapons, armor reinforcement, tools; grind → Beast Bone Meal (potent fertilizer/alchemy) | Most beasts with skeletons |
| BP03 | **Beast Fang** | 0.3 | Weapon crafting (dagger, spear tips); jewelry; alchemy reagent | Predators, serpents |
| BP04 | **Beast Claw** | 0.3 | Weapon crafting (gauntlets, daggers); tool tips; alchemy | Predators, birds of prey |
| BP05 | **Beast Eye** | 0.2 | Alchemy reagent (vision, perception); enchantment component | Most beasts |
| BP06 | **Beast Horn** | 0.5 | Weapon/armor crafting; instrument; powder → alchemy | Horned beasts, demons |
| BP07 | **Beast Sinew** | 0.3 | Superior bowstring; armor binding; stitching | Large beasts |
| BP08 | **Beast Blood** | 0.3 | Alchemy reagent (potent); ink; dye | Most beasts (collected in vial) |
| BP09 | **Beast Dust** | 0.1 | Alchemy reagent (elemental); enchantment catalyst | Magical/elemental beasts |
| BP10 | **Beast Scale** | 0.4 | Armor crafting (scale mail); decorative; alchemy | Reptiles, fish, drakes |
| BP11 | **Beast Venom** | 0.2 | Alchemy reagent (toxins); weapon coating (future) | Serpents, spiders, scorpions |
| BP12 | **Beast Feather** | 0.1 | Arrow fletching (superior); quill; decoration | Birds, griffins, winged beasts |

> **Naming convention**: In-game, these display with their quality prefix: "Fine Beast Fang", "Superior Beast Hide", "Legendary Beast Bone", etc. The quality tier determines the crafting power — a Legendary Beast Hide produces armor far superior to a Common Beast Hide.
>
> **Dragonhide (C22)** is the exception: it is NOT a beast part but a Core Resource. Only T5 dragon-type beasts drop Dragonhide, and it is always obtained in finished form at Legendary quality.

---

## 5. Cooked Food & Meals

Produced via the Cooking action at a Campfire or Cookhouse. Cooked food provides **energy buffs** and **enhanced nutrition** compared to raw food. Higher-tier meals require more ingredients and better facilities.

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
| MF01 | **Roast Meat** | Basic | Raw Meat + fuel | +5% energy regen |
| MF02 | **Grilled Fish** | Basic | Fish + fuel | +5% energy regen |
| MF03 | **Bread** | Standard | Flour + Water + fuel | +10% energy regen; long shelf life |
| MF04 | **Stew** | Standard | Meat + Vegetables + Water + fuel | +10% energy regen; +5% health regen |
| MF05 | **Fish Pie** | Standard | Fish + Flour + Butter + fuel | +10% energy regen; +5% SUR buff |
| MF06 | **Hunter's Feast** | Fine | Venison + Mushrooms + Herbs + Spices + Butter | +15% energy regen; +5% max energy |
| MF07 | **Honeyed Pastry** | Fine | Flour + Honey + Eggs + Butter + fruit | +15% energy regen; +25% food buff duration |
| MF08 | **Grand Banquet** | Feast | Multiple meats + Vegetables + Spices + Truffles + Honey + Cheese | +20% energy regen; +10% max energy; +1 to random attribute for duration |

> Full recipe list to be defined in Phase 7c (Food Recipes). These examples illustrate the tier structure and ingredient scaling.

---

## 6. Alchemy & Reagents

Alchemy reagents are gathered from multiple sources and combined to produce consumables. The base module **registers alchemy resource IDs** and defines raw reagent sources, but the full alchemy crafting system (recipes, effects, potion types) is **reserved for a future Alchemy & Essence module**.

### Base Module Alchemy Reagents

These are raw materials with alchemy applications, already listed in other sections:

| Reagent source | Resources | Section |
|---|---|---|
| Foraging | Wild Herbs (G04), Medicinal Herbs (G05), Mushrooms (G03), Rare Herbs (G13), Poisonous Plants (G14), Roots (G02), Wild Flowers (G06) | §1d |
| Mining | Sulfur (M19), Saltpeter (M20), Chalk (M27), Minerals (M28) | §1a |
| Beast encounters | Beast Eye (BP05), Beast Blood (BP08), Beast Dust (BP09), Beast Venom (BP11) | §4 |
| Hunting | Venom Sac (H11), Antler (H06) | §1e |
| Fishing | Coral (W04), Seaweed (G08) | §1f |
| Logging | Resin (L05), Sap (L06), Bark (L04) | §1b |
| Herding | Horn (K09) | §1g |

### Alchemy Products (future module — IDs registered)

| # | Product | Description | Status |
|---|---|---|---|
| A01 | **Healing Poultice** | Restores health over time | Base module candidate (simple recipe) |
| A02 | **Antidote** | Cures Poisoned injury trait | Base module candidate (simple recipe) |
| A03 | **Energy Tonic** | Restores energy | Base module candidate (simple recipe) |
| A04–A20 | Various potions, elixirs, bombs, coatings | Complex effects | Future Alchemy module |

> **Base module scope question**: Should basic alchemy products (Healing Poultice, Antidote, Energy Tonic) be included in the base module as simple Cookhouse/Workshop recipes, or deferred entirely to the Alchemy module? These three fill clear gameplay needs (injury recovery, poisoning cure, energy management) and use only base module reagents.

---

## 7. Essence & Magical Materials

| # | Resource | Weight (kg) | Rarity | Source | Notes |
|---|---|---|---|---|---|
| E01 | **Essence** | 0.1 | Legendary | No base-module source | ID registered. Future Alchemy & Essence module. |
| E02–E10 | Reserved magical materials | — | — | — | IDs registered for future modules. |

---

## Summary Statistics

| Category | Count | Weight range (kg) | Rarity range |
|---|---|---|---|
| Raw — Mining | 30 | 0.2–5.0 | Common–Very Rare |
| Raw — Logging | 7 | 0.3–6.0 | Common–Uncommon |
| Raw — Farming | 13 | 0.2–0.8 | Common–Uncommon |
| Raw — Foraging | 14 | 0.1–0.5 | Common–Rare |
| Raw — Hunting | 11 | 0.1–2.0 | Common–Rare |
| Raw — Fishing | 5 | 0.1–0.8 | Common–Rare |
| Raw — Herding | 10 | 0.2–2.0 | Common–Uncommon |
| **Subtotal Raw** | **90** | | |
| Core T1 | 6 | 1.5–3.0 | Common |
| Core T2 | 7 | 0.1–4.0 | Uncommon |
| Core T3 | 9 | 0.2–3.0 | Rare–Very Rare |
| **Subtotal Core** | **22** | | |
| Refined — Metals | 5 | 2.0–3.0 | Common–Uncommon |
| Refined — Textiles | 13 | 0.1–1.8 | Common–Rare |
| Refined — Building | 10 | 0.5–4.0 | Common–Uncommon |
| Refined — Fuels/Chemicals | 10 | 0.2–0.8 | Common–Rare |
| Refined — Processed Food | 9 | 0.1–0.5 | Common–Uncommon |
| **Subtotal Refined** | **47** | | |
| Beast Parts | 12 (×5 quality tiers = 60 effective) | 0.1–1.5 | Common–Legendary |
| Cooked Meals | 8 examples (full list Phase 7c) | — | — |
| Alchemy (registered) | 20 IDs reserved | — | — |
| Essence/Magical | 10 IDs reserved | — | — |
| **Total registered IDs** | **~210+** (incl. reserved) | | |
| **Total defined resources** | **~171** (90 raw + 22 core + 47 refined + 12 beast parts) | | |

---

## Design Notes

1. **Eternum heritage**: The 22 core resources retain their Eternum names and relative rarity tiers. They are native to this world, not bridged. Players familiar with Eternum will recognize the resource names and tier structure.

2. **Supply chain depth**: Every core and refined resource traces back to a raw precursor gathered through a player action. There are no resources that appear from nowhere. This creates meaningful production chains: `Copper Ore → Copper → Bronze (+ Tin Ore)` or `Flax → Linen Thread → Linen Cloth`.

3. **Generic beast parts**: 12 part types × 5 quality tiers = 60 effective variants without bloating the resource list. Beast lore determines which parts drop (a serpent drops Fang, Venom, Scale, Blood; a wolf drops Hide, Bone, Fang, Sinew). Quality scales with beast tier.

4. **Resource overlap**: Hunting and herding produce the same material types (hide, bone, fat, meat). The game treats these as identical resources regardless of source. This keeps the resource list clean while supporting multiple acquisition paths.

5. **Future-proofing**: Alchemy products, Essence, magical materials, Silk, and other reserved resources have IDs registered but no base-module source. Future modules activate them without changing the base resource registry.

6. **Facility requirements**: Most raw→refined processing requires a specific building or facility (Smelter, Tannery, Sawmill, etc.). This creates natural progression gates and incentivizes settlement building. Some basic processing (Rope from Hemp, Charcoal from Timber) can be done at a Campfire or with no facility.

7. **Weight as balancing lever**: Resource weight affects carry capacity and therefore the logistics of production. Heavy resources (Stone 3kg, Iron Ore 3kg, Adamantine Ore 5kg) require careful inventory management and incentivize settlement proximity, donkeys, and mounts.

8. **Onchain considerations**: The resource registry uses `u16` IDs with generous reserved ranges. Quality tiers for beast parts can be encoded as `(base_id, quality)` tuples rather than separate IDs, keeping the registry compact. Food buffs and meal effects can be lookup tables rather than per-resource storage.

---

*Appendix F — March 2026*
*Phase 3 — Draft resource catalog. To be refined during Phases 6–7 (production + recipes).*
