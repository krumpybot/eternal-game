# Appendix E: Biome–Resource Affinity Matrix ⚠️

> Phase 4b of the Eternal Game Base Module Quantification Plan.

> ⚠️ **Flagged for full review.** All affinity values are starting estimates pending validation against yield rates (Phase 6) and crafting recipes (Phase 7). Resource-biome relationships may need adjustment once production chains are quantified.
> Dependencies: Phase 3 (resource catalog — Appendix F), Phase 4a (biome properties — Appendix D).
> This matrix defines which raw resources can appear in each biome. It drives world generation (resource node seeding) and production yields.

---

## Biome consolidation note (27 → 24) 🔒

The base module biome set is **24** biomes.

Merges applied:
- **Plains → Grassland** (take better affinity)
- **Steppe → Scrubland** (take better affinity)
- **Marsh → Swamp** (take better affinity)

In every matrix below, the removed columns (Plns, Stp, Msh) have been merged into their destination biome column by taking the better affinity per resource (★ > ◆ > ✓ > —).

---

## How to Read This Matrix

- **✓** = Resource can appear in this biome (standard chance, 1× node frequency)
- **◆** = Primary biome — resource is common here (2× node frequency)
- **★** = Signature biome — resource strongly associated with this biome (3× node frequency, higher yield multiplier)
- **—** = Resource cannot appear in this biome
- Resources marked "universal" can appear in any biome that supports their gathering type

> Node frequency multipliers affect how often a resource node is seeded during world generation. Yield multipliers affect how much is gathered per action. Both are further modified by area rarity and individual node seed.

> **All affinity matrices are unlocked** — these are starting values. Game Masters may adjust resource-biome affinities via consensus. The resource catalog itself (Appendix F) and the biome list (Appendix D) are 🔒 locked.

---

## 1. Mining Resources by Biome

Universal mining resources (appear wherever mining areas exist): **Coal, Flint, Limestone, Salt, Sand, Stone, Clay**.
Universal rare drop (any mining activity): **Rare Metals**.

| Resource | Rar | Grs | For | Wdl | Rnf | Hgh | Scr | Dst | Sav | Bad | Can | Swp | Mre | Jun | Mgr | Tun | Tai | Glc | Sco | Gls | Blt | Cst | Lke |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Cold Iron Ore** | U | ✓ | ✓ | ✓ | — | ◆ | ✓ | ✓ | ✓ | ◆ | ◆ | — | — | ✓ | — | ✓ | ✓ | ✓ | ✓ | — | — | — | — |
| **Copper Ore** | U | ✓ | ✓ | ✓ | — | ◆ | ✓ | ✓ | ✓ | ✓ | ✓ | — | — | ✓ | — | ✓ | ✓ | — | ✓ | — | — | — | — |
| **Gold Ore** | U | — | — | — | — | ✓ | — | ✓ | — | ✓ | ◆ | — | — | ✓ | — | — | — | — | ✓ | — | — | — | — |
| **Obsidian** | U | — | — | — | — | ✓ | — | — | — | ✓ | ✓ | — | — | — | — | — | — | — | ★ | ✓ | — | — | — |
| **Saltpeter** | U | — | — | — | — | — | — | ◆ | — | ✓ | ✓ | — | — | — | — | — | — | — | ✓ | — | — | — | — |
| **Silver Ore** | U | — | — | — | — | ◆ | — | — | — | ✓ | ◆ | — | — | — | — | ✓ | ✓ | — | — | — | — | — | — |
| **Sulfur** | U | — | — | — | — | — | — | — | — | ✓ | ✓ | ✓ | — | — | — | — | — | — | ★ | — | ✓ | — | — |
| **Amber** | R | — | ✓ | ✓ | ◆ | — | — | — | — | — | — | ✓ | — | ◆ | — | ✓ | ◆ | — | — | — | — | ✓ | — |
| **Rough Diamond** | R | — | — | — | — | ✓ | — | — | — | — | ◆ | — | — | — | — | ✓ | — | ✓ | ◆ | ★ | — | — | — |
| **Rough Ruby** | R | — | — | — | — | ✓ | — | ◆ | — | ✓ | ✓ | — | — | — | — | — | — | — | ◆ | ★ | — | — | — |
| **Rough Sapphire** | R | — | — | — | — | ◆ | — | — | — | — | ✓ | — | — | — | — | ✓ | — | ◆ | — | ★ | — | — | — |
| **Eth. Silica Sand** | E | — | — | — | — | — | — | ★ | — | ✓ | — | — | — | — | — | — | — | — | — | ◆ | — | — | — |
| **Ignium Ore** | E | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | ★ | — | — | — | — |
| **Rough Deep Crystal** | E | — | — | — | — | ✓ | — | — | — | — | ◆ | — | — | — | — | — | — | ◆ | — | ★ | — | — | — |
| **Voidstone** | E | — | — | — | — | — | — | — | — | ✓ | ◆ | ✓ | ◆ | — | — | — | — | — | ✓ | — | ◆ | — | — |
| **Alch. Silver Ore** | L | — | — | — | — | ✓ | — | — | — | — | ✓ | ✓ | — | — | — | — | — | — | — | — | ★ | — | — |
| **Rough Twi. Quartz** | L | — | — | — | — | — | — | — | — | — | ✓ | — | — | — | — | — | — | ◆ | — | ✓ | — | — | — |
| **Starmetal Frag.** | L | — | — | — | — | ✓ | — | ✓ | — | ◆ | ✓ | — | — | — | — | ✓ | — | — | ✓ | — | — | — | — |
| **True Ice Shard** | L | — | — | — | — | ✓ | — | — | — | — | — | — | — | — | — | ◆ | — | ★ | — | — | — | — | — |
| **Adamantine Ore** | M | — | — | — | — | ✓ | — | — | — | — | ◆ | — | — | — | — | — | — | ✓ | ✓ | — | — | — | — |
| **Mithral Ore** | M | — | — | — | — | ✓ | — | — | — | — | ✓ | — | — | — | — | ✓ | — | ◆ | — | — | — | — | — |
| **Unearth. Dragonhide** | M | — | — | — | — | — | — | ✓ | — | ◆ | ✓ | — | — | — | — | — | — | — | ◆ | — | — | — | — |

---

## 2. Logging Resources by Biome

Logging requires forestry areas. Biomes with no trees cannot generate forestry areas.

Universal rare drop (any logging activity): **Worldroot**.

| Resource | Rar | Grs | For | Wdl | Rnf | Hgh | Scr | Sav | Swp | Jun | Mgr | Tai | Blt |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Resin** | C | — | ◆ | ✓ | ◆ | ✓ | — | ✓ | ✓ | ◆ | ✓ | ★ | ✓ |
| **Wood** | C | ✓ | ★ | ◆ | ◆ | ✓ | ✓ | ✓ | ✓ | ◆ | ✓ | ◆ | ✓ |
| **Ironwood** | U | — | ◆ | ✓ | ✓ | ✓ | — | — | — | ✓ | — | ◆ | — |
| **Hartwood** | R | — | ★ | ✓ | ◆ | ✓ | — | — | — | ✓ | — | ✓ | — |
| **Moonwood** | E | — | ✓ | — | ★ | — | — | — | — | ✓ | — | ✓ | — |

---

## 3. Farming Resources by Biome

Farming requires fertile areas with fertility > 0. Biomes with None fertility cannot farm.

Universal farming resources (appear in any fertile area with fertility ≥ Low): **Wheat, Barley, Beans, Vegetables, Tubers**.

| Resource | Rar | Grs | For | Wdl | Rnf | Hgh | Scr | Sav | Swp | Jun | Tai | Cst | Lke |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Flax** | C | ◆ | ✓ | ◆ | — | ✓ | — | — | ✓ | — | — | — | ✓ |
| **Fruit** | C | ✓ | ◆ | ◆ | — | — | ✓ | ✓ | — | ★ | — | — | — |
| **Hemp** | C | ✓ | ✓ | ✓ | — | — | ✓ | — | ✓ | — | — | — | — |
| **Raw Cotton** | U | — | — | — | — | — | — | ◆ | — | ★ | — | — | — |
| **Dye Plants** | U | ✓ | ✓ | ✓ | — | — | ★ | ✓ | ✓ | ◆ | — | — | — |
| **Hops** | U | ◆ | ✓ | ◆ | — | ✓ | — | — | — | — | — | — | — |
| **Spice Plants** | U | — | — | — | — | — | ✓ | ✓ | — | ★ | — | — | — |
| **Sugar Cane** | U | — | — | — | — | — | — | ✓ | ✓ | ★ | — | — | — |

---

## 4. Foraging Resources by Biome

Foraging gathers wild-growing resources. Some foraging is possible even in biomes with no fertility (mushrooms, lichen), but yields are minimal.

Universal rare drop (any foraging activity): **Unicorn Hair**.

| Resource | Rar | Grs | For | Wdl | Rnf | Hgh | Scr | Dst | Sav | Bad | Swp | Mre | Jun | Mgr | Tun | Tai | Sco | Gls | Blt | Cst | Lke |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Mushrooms** | C | ✓ | ★ | ✓ | ◆ | ✓ | — | — | — | — | ◆ | — | ◆ | — | — | ◆ | ✓ | — | ✓ | — | — |
| **Roots** | C | ✓ | ◆ | ✓ | ✓ | ✓ | ✓ | — | ✓ | — | ✓ | — | ✓ | — | ✓ | ✓ | — | — | ✓ | — | — |
| **Seaweed** | C | — | — | — | — | — | — | — | — | — | — | — | — | ★ | — | — | — | — | — | ◆ | ✓ |
| **Wild Berries** | C | ◆ | ★ | ◆ | ✓ | ✓ | ✓ | — | — | — | ✓ | — | ✓ | — | — | ◆ | — | — | — | — | — |
| **Wild Herbs** | C | ◆ | ✓ | ✓ | ✓ | ✓ | ★ | — | ✓ | — | ✓ | — | ✓ | ✓ | — | ✓ | — | — | ✓ | ✓ | ✓ |
| **Honey** | U | ◆ | ★ | ◆ | ✓ | ✓ | ◆ | — | ✓ | — | ✓ | — | ✓ | — | — | ✓ | — | — | — | — | — |
| **Med. Herbs** | U | ✓ | ◆ | ✓ | ★ | ✓ | ✓ | — | ✓ | — | ◆ | — | ◆ | ✓ | — | ✓ | — | — | ✓ | — | — |
| **Nuts** | U | ✓ | ★ | ◆ | ✓ | ✓ | — | — | — | — | — | — | ✓ | — | — | ✓ | — | — | — | — | — |
| **Poisonous Plants** | R | — | ✓ | — | — | — | — | — | — | — | ★ | ★ | ◆ | — | — | — | — | — | ★ | — | — |
| **Rare Herbs** | R | — | ◆ | — | ★ | ✓ | ✓ | — | — | — | ✓ | — | ◆ | — | — | — | — | — | ✓ | — | — |
| **Silkworms** | R | — | ◆ | — | ✓ | — | — | — | — | — | ✓ | — | ★ | — | — | — | — | — | — | — | — |
| **Truffles** | R | — | ★ | ✓ | ✓ | — | — | — | — | — | — | — | ✓ | — | — | ✓ | — | — | — | — | — |
| **Spiritbloom** | E | — | ✓ | — | ✓ | ✓ | — | — | — | — | ✓ | ✓ | ✓ | — | — | — | ✓ | — | ★ | — | — |

---

## 5. Hunting Fauna by Biome

Hunting targets wild fauna. Fauna type and density vary by biome. Beast parts yielded depend on beast tier (see Appendix D §3 and Appendix F §4).

| Resource | Rar | Grs | For | Wdl | Rnf | Hgh | Scr | Dst | Sav | Bad | Can | Swp | Mre | Jun | Mgr | Tun | Tai | Glc | Sco | Gls | Blt | Cst | Lke |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Game Meat** | C | ◆ | ★ | ◆ | ✓ | ✓ | ✓ | ✓ | ★ | ✓ | ✓ | ✓ | ✓ | ★ | ✓ | ✓ | ✓ | ✓ | — | ✓ | ✓ | ✓ | ✓ |
| **Raw Bone** | C | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ◆ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | — | ✓ | ✓ | ✓ | ✓ |
| **Raw Fat** | C | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | — | ✓ | ✓ | ✓ | ✓ |
| **Raw Hide** | C | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ★ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ◆ | ✓ | ✓ | — | ✓ | ✓ | — | — |
| **Sinew** | C | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | — | ✓ | ✓ | — | — |
| **Feathers** | U | ◆ | ✓ | ◆ | ✓ | ◆ | ✓ | — | ✓ | — | ✓ | ✓ | — | ✓ | ✓ | — | ✓ | — | — | — | ✓ | ✓ | ✓ |
| **Fur Pelt** | U | — | ◆ | ✓ | ✓ | ◆ | — | — | — | — | — | — | — | — | ★ | ★ | ✓ | — | — | — | — | — | — |
| **Ivory** | R | — | — | — | — | — | — | — | ★ | — | — | — | — | ◆ | ✓ | — | — | — | — | — | — | — | — |
| **Venom Sac** | R | — | ✓ | — | ✓ | — | ✓ | ◆ | — | ◆ | ✓ | ★ | ◆ | ◆ | ✓ | — | — | — | — | — | ◆ | — | — |
| **Exotic Pelt** | E | — | ✓ | — | ✓ | — | — | — | ✓ | — | — | — | — | ★ | ✓ | ✓ | — | — | — | — | — | — | — |
| **Raw Demonhide** | L | — | — | — | — | — | — | — | — | ✓ | ✓ | ✓ | ◆ | — | — | — | — | — | ★ | — | ✓ | — | — |

---

## 6. Fishing by Biome

Fishing requires a water biome or water-adjacent hex. Only Coast, Lake, Mangrove, and Swamp support fishing in the base module.

| Resource | Rarity | Coast | Lake | Mangrove | Swamp |
|---|---|---|---|---|---|
| **Fish** | C | ★ | ★ | ★ | ✓ |

---

## 7. Herding by Biome

Herding requires fertile areas converted from native vegetation. Biome determines which livestock types are viable and their productivity multiplier.

| Livestock | Grs | For | Wdl | Hgh | Scr | Sav | Swp | Jun | Tai | Cst | Lke |
|---|---|---|---|---|---|---|---|---|---|---|---|
| **Cattle** | ★ | ☆ | ✓ | ✓ | ☆ | ◆ | — | — | — | — | — |
| **Sheep** | ◆ | ☆ | ✓ | ★ | ✓ | ✓ | — | — | — | — | — |
| **Poultry** | ◆ | ✓ | ◆ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| **Horses/Donkeys** | ★ | ☆ | ✓ | ✓ | ☆ | ◆ | — | — | — | — | — |

> **Legend**: ★ = ideal, ◆ = good, ✓ = viable, ☆ = marginal (50% yield penalty), — = not viable.

---

*Phase 4b of the Eternal Game Base Module Quantification Plan.*
*v0.2.1 — April 2026*
