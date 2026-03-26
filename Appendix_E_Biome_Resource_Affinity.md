# Appendix E: Biome–Resource Affinity Matrix

> Phase 4b of the Eternal Game Base Module Quantification Plan.
> Dependencies: Phase 3 (resource catalog — Appendix F), Phase 4a (biome properties — Appendix D).
> This matrix defines which raw resources can appear in each biome. It drives world generation (resource node seeding) and production yields.

---

## How to Read This Matrix

- **✓** = Resource can appear in this biome (standard chance)
- **◆** = Primary biome — resource is common here (2× node frequency)
- **★** = Signature biome — resource strongly associated with this biome (3× node frequency, higher yield multiplier)
- **—** = Resource cannot appear in this biome
- Resources marked "universal" can appear in any biome that supports their gathering type

> Node frequency multipliers affect how often a resource node is seeded during world generation. Yield multipliers affect how much is gathered per action. Both are further modified by area rarity and individual node seed.

---

## 1. Mining Resources by Biome

Universal mining resources (appear wherever mining areas exist): **Coal, Flint, Limestone, Salt, Sand, Stone, Clay**.
Universal rare drop (any mining activity): **Rare Metals**.

| Resource | Rarity | Plains | Grass | Forest | High | Desert | Sav | Steppe | Bad | Canyon | Swamp | Wet | Mire | Jungle | Tundra | Taiga | Glacier | Volc | Oasis | Coast | Beach | Lake |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Cold Iron Ore** | U | ✓ | ✓ | ✓ | ◆ | ✓ | ✓ | ✓ | ◆ | ◆ | — | — | — | ✓ | ✓ | ✓ | ✓ | ✓ | — | — | — | — |
| **Copper Ore** | U | ✓ | ✓ | ✓ | ◆ | ✓ | ✓ | ✓ | ✓ | ✓ | — | — | — | ✓ | ✓ | ✓ | — | ✓ | — | — | — | — |
| **Gold Ore** | U | — | — | — | ✓ | ✓ | — | — | ✓ | ◆ | — | — | — | ✓ | — | — | — | ✓ | — | — | — | — |
| **Obsidian** | U | — | — | — | ✓ | — | — | — | ✓ | ✓ | — | — | — | — | — | — | — | ★ | — | — | — | — |
| **Saltpeter** | U | — | — | — | — | ◆ | — | ✓ | ✓ | ✓ | — | — | — | — | — | — | — | ✓ | — | — | — | — |
| **Silver Ore** | U | — | — | — | ◆ | — | — | — | ✓ | ◆ | — | — | — | — | ✓ | ✓ | — | — | — | — | — | — |
| **Sulfur** | U | — | — | — | — | — | — | — | ✓ | ✓ | ✓ | — | — | — | — | — | — | ★ | — | — | — | — |
| **Amber** | R | — | — | ✓ | — | — | — | — | — | — | ✓ | ✓ | — | ◆ | ✓ | ◆ | — | — | — | ✓ | — | — |
| **Rough Diamond** | R | — | — | — | ✓ | — | — | — | — | ◆ | — | — | — | — | ✓ | — | ✓ | ◆ | — | — | — | — |
| **Rough Ruby** | R | — | — | — | ✓ | ◆ | — | — | ✓ | ✓ | — | — | — | — | — | — | — | ◆ | — | — | — | — |
| **Rough Sapphire** | R | — | — | — | ◆ | — | — | — | — | ✓ | — | — | — | — | ✓ | — | ◆ | — | — | — | — | — |
| **Ethereal Silica Sand** | E | — | — | — | — | ★ | — | — | ✓ | — | — | — | — | — | — | — | — | — | ✓ | — | — | — |
| **Ignium Ore** | E | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | ★ | — | — | — | — |
| **Rough Deep Crystal** | E | — | — | — | ✓ | — | — | — | — | ◆ | — | — | — | — | — | — | ◆ | — | — | — | — | — |
| **Voidstone** | E | — | — | — | — | — | — | — | ✓ | ◆ | ✓ | — | ◆ | — | — | — | — | ✓ | — | — | — | — |
| **Alchemical Silver Ore** | L | — | — | — | ✓ | — | — | — | — | ✓ | ✓ | — | ◆ | — | — | — | — | — | — | — | — | — |
| **Rough Twilight Quartz** | L | — | — | — | — | — | — | — | — | ✓ | — | — | — | — | — | — | ◆ | — | — | — | — | — |
| **Starmetal Fragment** | L | — | — | — | ✓ | ✓ | — | — | ◆ | ✓ | — | — | — | — | ✓ | — | — | ✓ | — | — | — | — |
| **True Ice Shard** | L | — | — | — | ✓ | — | — | — | — | — | — | — | — | — | ◆ | — | ★ | — | — | — | — | — |
| **Adamantine Ore** | M | — | — | — | ✓ | — | — | — | — | ◆ | — | — | — | — | — | — | ✓ | ✓ | — | — | — | — |
| **Mithral Ore** | M | — | — | — | ✓ | — | — | — | — | ✓ | — | — | — | — | ✓ | — | ◆ | — | — | — | — | — |
| **Unearthed Dragonhide** | M | — | — | — | — | ✓ | — | — | ◆ | ✓ | — | — | — | — | — | — | — | ◆ | — | — | — | — |

> **Rarity codes**: C=Common, U=Uncommon, R=Rare, E=Epic, L=Legendary, M=Mythic.
>
> **Design logic**:
> - Common ores (coal, stone, clay, etc.) are universal — they appear wherever mining areas exist.
> - Gem precursors concentrate in hard terrain: Highlands, Canyon, Glacier, Volcanic.
> - Volcanic is the signature biome for Obsidian, Sulfur, and Ignium — the fire-aspected minerals.
> - Glacier is the signature biome for True Ice and a primary for Twilight Quartz and Sapphire — cold preserves ancient crystals.
> - Desert is the signature biome for Ethereal Silica Sand — magical sand formations.
> - Canyon and Badlands are the richest mining biomes overall — deep cuts expose rare veins.
> - Mythic ores (Adamantine, Mithral, Dragonhide) appear only in the most extreme/deep biomes.
> - Unearthed Dragonhide is found in Badlands, Canyon, Desert, and Volcanic — ancient dragon burial sites in harsh, preserved terrain.

---

## 2. Logging Resources by Biome

Logging requires forestry areas. Biomes with no trees (Desert, Badlands, Canyon, Glacier, Volcanic, Beach, Ocean, etc.) cannot generate forestry areas.

Universal rare drop (any logging activity): **Worldroot**.

| Resource | Rarity | Plains | Grass | Forest | High | Sav | Steppe | Swamp | Wet | Jungle | Tundra | Taiga | Oasis | Coast | Lake |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Resin** | C | — | — | ◆ | ✓ | ✓ | — | ✓ | ✓ | ◆ | — | ★ | ✓ | — | — |
| **Wood** | C | ✓ | ✓ | ★ | ✓ | ✓ | — | ✓ | ✓ | ◆ | — | ◆ | ✓ | ✓ | ✓ |
| **Ironwood** | U | — | — | ◆ | ✓ | — | — | — | — | ✓ | — | ◆ | — | — | — |
| **Hartwood** | R | — | — | ★ | ✓ | — | — | — | — | ✓ | — | ✓ | — | — | — |
| **Moonwood** | E | — | — | ◆ | — | — | — | — | — | ✓ | — | ✓ | — | — | — |

> **Design logic**:
> - Wood is nearly universal where trees grow — Forest is its signature biome.
> - Resin is most common in coniferous forests — Taiga is the signature biome.
> - Ironwood grows in dense temperate and northern forests — Forest and Taiga are primary.
> - Hartwood is ancient heartwood — only the oldest forests (Forest signature, some Highlands and Jungle).
> - Moonwood requires centuries of moonlight exposure — only the oldest, most open forests. Forest is primary; sparse Jungle and Taiga have rare specimens.

---

## 3. Farming Resources by Biome

Farming requires fertile areas with fertility > 0. The fertility modifier (Appendix D §5) directly affects yield. Crops listed below are what **can grow** in each biome; actual node seeding uses these affinities.

Universal farming resources (appear in any fertile area with fertility ≥ Low): **Wheat, Barley, Beans, Vegetables, Tubers**.

| Resource | Rarity | Plains | Grass | Forest | High | Sav | Steppe | Swamp | Wet | Jungle | Taiga | Oasis | Coast | Lake |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Flax** | C | ◆ | ◆ | ✓ | ✓ | — | — | — | ✓ | — | — | ✓ | — | ✓ |
| **Fruit** | C | ✓ | ✓ | ◆ | — | ✓ | — | — | — | ★ | — | ★ | — | — |
| **Hemp** | C | ◆ | ✓ | ✓ | — | — | ✓ | — | ✓ | — | — | ✓ | — | — |
| **Raw Cotton** | U | — | — | — | — | ◆ | — | — | — | ★ | — | ◆ | — | — |
| **Dye Plants** | U | ✓ | ✓ | ✓ | — | ✓ | — | — | ✓ | ◆ | — | ✓ | — | — |
| **Hops** | U | ✓ | ◆ | ✓ | ✓ | — | — | — | — | — | — | — | — | — |
| **Spice Plants** | U | — | — | — | — | ✓ | — | — | — | ★ | — | ◆ | — | — |
| **Sugar Cane** | U | — | — | — | — | ✓ | — | ✓ | ✓ | ★ | — | ◆ | — | — |

> **Design logic**:
> - Universal crops (wheat, barley, beans, vegetables, tubers) grow anywhere fertile — they're the staples.
> - Tropical crops (sugar cane, spice plants, raw cotton, fruit) concentrate in Jungle and Oasis with some Savanna spillover.
> - Temperate textile crops (flax, hemp) concentrate in Plains and Grassland.
> - Hops favour cool-temperate Grassland.
> - Biomes with None fertility cannot farm at all — no farming rows exist for Desert, Badlands, Canyon, Mire, Tundra, Glacier, Volcanic, Beach.

---

## 4. Foraging Resources by Biome

Foraging gathers wild-growing resources. Some foraging is possible even in biomes with no fertility (mushrooms in caves, lichen on rocks), but yields are minimal.

Universal rare drop (any foraging activity): **Unicorn Hair**.

| Resource | Rarity | Plains | Grass | Forest | High | Desert | Sav | Steppe | Bad | Canyon | Swamp | Wet | Mire | Jungle | Tundra | Taiga | Glacier | Volc | Oasis | Coast | Beach | Lake |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Mushrooms** | C | ✓ | ✓ | ★ | ✓ | — | — | — | — | — | ◆ | ✓ | — | ◆ | — | ◆ | — | ✓ | — | — | — | — |
| **Roots** | C | ✓ | ✓ | ◆ | ✓ | — | ✓ | ✓ | — | — | ✓ | ✓ | — | ✓ | ✓ | ✓ | — | — | ✓ | — | — | — |
| **Seaweed** | C | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | ★ | ◆ | ✓ |
| **Wild Berries** | C | ✓ | ◆ | ★ | ✓ | — | — | ✓ | — | — | — | ✓ | — | ✓ | — | ◆ | — | — | ✓ | — | — | — |
| **Wild Herbs** | C | ◆ | ◆ | ✓ | ✓ | — | ✓ | ✓ | — | — | ✓ | ✓ | — | ✓ | — | ✓ | — | — | ◆ | ✓ | — | ✓ |
| **Honey** | U | ✓ | ◆ | ★ | ✓ | — | ✓ | — | — | — | — | ✓ | — | ✓ | — | ✓ | — | — | ◆ | — | — | — |
| **Medicinal Herbs** | U | ✓ | ✓ | ◆ | ✓ | — | ✓ | — | ◆ | — | — | ✓ | — | ◆ | — | ✓ | — | — | ✓ | — | — | — |
| **Nuts** | U | — | ✓ | ★ | ✓ | — | — | — | — | — | — | — | — | ✓ | — | ✓ | — | — | ✓ | — | — | — |
| **Poisonous Plants** | R | — | — | ✓ | — | — | — | — | — | — | ◆ | ✓ | ★ | ◆ | — | — | — | — | — | — | — | — |
| **Rare Herbs** | R | — | — | ◆ | ✓ | — | — | — | — | — | ✓ | — | — | ◆ | — | — | — | — | ★ | — | — | — |
| **Silkworms** | R | — | — | ◆ | — | — | — | — | — | ✓ | ✓ | — | — | ★ | — | — | — | — | — | — | — | — |
| **Truffles** | R | — | — | ★ | — | — | — | — | — | — | — | — | — | ✓ | — | ✓ | — | — | — | — | — | — |
| **Spiritbloom** | E | — | — | ✓ | ✓ | — | — | — | — | ✓ | ✓ | — | ✓ | ✓ | — | — | — | ✓ | — | — | — | — |

> **Design logic**:
> - Forest is the richest foraging biome — berries, mushrooms, honey, nuts, truffles, herbs, silkworms.
> - Jungle is second — tropical foraging with silkworms, poisonous plants, and rare herbs.
> - Mire is the signature biome for Poisonous Plants — toxic wetland flora.
> - Truffles are Forest signature — rare underground fungi found by experienced foragers.
> - Spiritbloom appears near magical sites — distributed across several biomes at low rates, not tied to one.
> - Seaweed is coastal only — Coast signature, with Beach and Lake access.
> - Oasis is the rare herbs signature — concentrated magical biodiversity in tiny desert pockets.

---

## 5. Hunting Fauna by Biome

Hunting targets wild fauna in forestry areas and some other area types. Fauna type and density vary by biome. Beast parts yielded depend on beast tier (see Appendix D §3: Fauna Tier and Appendix F §4: Beast Parts).

| Resource | Rarity | Plains | Grass | Forest | High | Desert | Sav | Steppe | Bad | Canyon | Swamp | Wet | Mire | Jungle | Tundra | Taiga | Glacier | Volc | Oasis | Coast | Beach | Lake |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Game Meat** | C | ◆ | ◆ | ★ | ✓ | ✓ | ★ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ★ | ✓ | ✓ | ✓ | — | ✓ | ✓ | — | ✓ |
| **Raw Bone** | C | ◆ | ✓ | ✓ | ✓ | ✓ | ◆ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | — | ✓ | ✓ | — | ✓ |
| **Raw Fat** | C | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | — | ✓ | ✓ | — | ✓ |
| **Raw Hide** | C | ◆ | ✓ | ✓ | ✓ | ✓ | ★ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ◆ | ✓ | ✓ | — | ✓ | — | — | — |
| **Sinew** | C | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | — | ✓ | — | — | — |
| **Feathers** | U | ✓ | ◆ | ✓ | ◆ | — | ✓ | ◆ | — | ✓ | ✓ | ◆ | — | ✓ | — | ✓ | — | — | ✓ | ✓ | ✓ | ✓ |
| **Fur Pelt** | U | — | — | ◆ | ◆ | — | — | — | — | — | — | — | — | — | ★ | ★ | ✓ | — | — | — | — | — |
| **Ivory** | R | — | — | — | — | — | ★ | — | — | — | — | — | — | ◆ | ✓ | — | — | — | — | — | — | — |
| **Venom Sac** | R | — | — | ✓ | — | ◆ | — | — | ◆ | ✓ | ★ | ✓ | ◆ | ◆ | — | — | — | — | — | — | — | — |
| **Exotic Pelt** | E | — | — | ✓ | — | — | ✓ | — | — | — | — | — | — | ★ | ✓ | ✓ | — | — | — | — | — | — |
| **Raw Demonhide** | L | — | — | — | — | — | — | — | ✓ | ✓ | ✓ | — | ◆ | — | — | — | — | ★ | — | — | — | — |

> **Design logic**:
> - Common hunting products (meat, bone, fat, hide, sinew) are universal where fauna exist.
> - Fur Pelt concentrates in cold biomes — Tundra and Taiga are signature.
> - Ivory comes from large-tusked savanna and jungle megafauna.
> - Venom Sac concentrates in Swamp (signature), Desert, Badlands, Jungle — poisonous creature habitat.
> - Exotic Pelt is Jungle signature — rare jungle cats, serpents, and unique creatures.
> - Raw Demonhide is Volcanic signature — demons lurk near fire and brimstone. Also found in Mire (primary), Badlands, Canyon, and Swamp where dark creatures dwell.
> - Beach has no hunting (minimal fauna). Volcanic has no standard hunting (fire fauna handled by encounters, not hunting action).

---

## 6. Fishing by Biome

Fishing requires a water-adjacent hex or a water biome. Only Coast, Beach, Lake, Swamp, and Wetlands support fishing in the base module.

| Resource | Rarity | Coast | Beach | Lake | Swamp | Wetlands |
|---|---|---|---|---|---|---|
| **Fish** | C | ★ | ◆ | ★ | ✓ | ✓ |

> Fishing currently produces only Fish. Coast and Lake are signature biomes. Future Maritime module may expand aquatic resources.

---

## 7. Herding by Biome

Herding requires fertile areas converted from native vegetation. Biome determines which livestock types are viable and their productivity multiplier.

| Livestock | Plains | Grass | Forest | High | Sav | Steppe | Wet | Jungle | Taiga | Oasis | Coast | Lake |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Cattle** | ★ | ★ | ☆ | ✓ | ◆ | ✓ | ☆ | — | — | ✓ | — | — |
| **Sheep** | ✓ | ◆ | ☆ | ★ | ✓ | ◆ | — | — | — | ✓ | — | — |
| **Poultry** | ◆ | ◆ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ◆ | ✓ | ✓ |
| **Horses/Donkeys** | ★ | ★ | ☆ | ✓ | ◆ | ◆ | — | — | — | ✓ | — | — |

> **Legend**: ★ = ideal, ◆ = good, ✓ = viable, ☆ = marginal (50% yield penalty), — = not viable.
>
> **Design logic**:
> - Cattle and horses need open grassland — Plains and Grassland are ideal.
> - Sheep favour high-altitude grazing — Highlands signature, with Grassland and Steppe.
> - Poultry are the most adaptable — viable almost anywhere with fertility.
> - No herding in infertile biomes (Desert, Badlands, Canyon, Mire, Tundra, Glacier, Volcanic, Beach).
> - Forest herding is marginal — dense trees restrict grazing.

---

*Phase 4b of the Eternal Game Base Module Quantification Plan.*
*v0.1.0 — March 2026*
