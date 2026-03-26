# Appendix E: Biome–Resource Affinity Matrix

> Phase 4b of the Eternal Game Base Module Quantification Plan.
> Dependencies: Phase 3 (resource catalog — Appendix F), Phase 4a (biome properties — Appendix D).
> This matrix defines which raw resources can appear in each biome. It drives world generation (resource node seeding) and production yields.

---

## How to Read This Matrix

- **✓** = Resource can appear in this biome (standard chance, 1× node frequency)
- **◆** = Primary biome — resource is common here (2× node frequency)
- **★** = Signature biome — resource strongly associated with this biome (3× node frequency, higher yield multiplier)
- **—** = Resource cannot appear in this biome
- Resources marked "universal" can appear in any biome that supports their gathering type

> Node frequency multipliers affect how often a resource node is seeded during world generation. Yield multipliers affect how much is gathered per action. Both are further modified by area rarity and individual node seed.

> 🔓 **All affinity matrices are unlocked** — these are starting values. Game Masters may adjust resource-biome affinities via consensus. The resource catalog itself (Appendix F) and the biome list (Appendix D) are 🔒 locked.

---

## 1. Mining Resources by Biome 🔓

Universal mining resources (appear wherever mining areas exist): **Coal, Flint, Limestone, Salt, Sand, Stone, Clay**.
Universal rare drop (any mining activity): **Rare Metals**.

| Resource | Rar | Plns | Grs | For | Wdl | Rnf | Hgh | Scr | Dst | Sav | Stp | Bad | Can | Swp | Msh | Mre | Jun | Mgr | Tun | Tai | Glc | Sco | Gls | Blt | Cst | Lke |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Cold Iron Ore** | U | ✓ | ✓ | ✓ | ✓ | — | ◆ | ✓ | ✓ | ✓ | ✓ | ◆ | ◆ | — | — | — | ✓ | — | ✓ | ✓ | ✓ | ✓ | — | — | — | — |
| **Copper Ore** | U | ✓ | ✓ | ✓ | ✓ | — | ◆ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | — | — | — | ✓ | — | ✓ | ✓ | — | ✓ | — | — | — | — |
| **Gold Ore** | U | — | — | — | — | — | ✓ | — | ✓ | — | — | ✓ | ◆ | — | — | — | ✓ | — | — | — | — | ✓ | — | — | — | — |
| **Obsidian** | U | — | — | — | — | — | ✓ | — | — | — | — | ✓ | ✓ | — | — | — | — | — | — | — | — | ★ | ✓ | — | — | — |
| **Saltpeter** | U | — | — | — | — | — | — | — | ◆ | — | ✓ | ✓ | ✓ | — | — | — | — | — | — | — | — | ✓ | — | — | — | — |
| **Silver Ore** | U | — | — | — | — | — | ◆ | — | — | — | — | ✓ | ◆ | — | — | — | — | — | ✓ | ✓ | — | — | — | — | — | — |
| **Sulfur** | U | — | — | — | — | — | — | — | — | — | — | ✓ | ✓ | ✓ | — | — | — | — | — | — | — | ★ | — | ✓ | — | — |
| **Amber** | R | — | — | ✓ | ✓ | ◆ | — | — | — | — | — | — | — | ✓ | — | — | ◆ | — | ✓ | ◆ | — | — | — | — | ✓ | — |
| **Rough Diamond** | R | — | — | — | — | — | ✓ | — | — | — | — | — | ◆ | — | — | — | — | — | ✓ | — | ✓ | ◆ | ★ | — | — | — |
| **Rough Ruby** | R | — | — | — | — | — | ✓ | — | ◆ | — | — | ✓ | ✓ | — | — | — | — | — | — | — | — | ◆ | ★ | — | — | — |
| **Rough Sapphire** | R | — | — | — | — | — | ◆ | — | — | — | — | — | ✓ | — | — | — | — | — | ✓ | — | ◆ | — | ★ | — | — | — |
| **Eth. Silica Sand** | E | — | — | — | — | — | — | — | ★ | — | — | ✓ | — | — | — | — | — | — | — | — | — | — | ◆ | — | — | — |
| **Ignium Ore** | E | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | ★ | — | — | — | — |
| **Rough Deep Crystal** | E | — | — | — | — | — | ✓ | — | — | — | — | — | ◆ | — | — | — | — | — | — | — | ◆ | — | ★ | — | — | — |
| **Voidstone** | E | — | — | — | — | — | — | — | — | — | — | ✓ | ◆ | ✓ | — | ◆ | — | — | — | — | — | ✓ | — | ◆ | — | — |
| **Alch. Silver Ore** | L | — | — | — | — | — | ✓ | — | — | — | — | — | ✓ | ✓ | — | ◆ | — | — | — | — | — | — | — | ★ | — | — |
| **Rough Twi. Quartz** | L | — | — | — | — | — | — | — | — | — | — | — | ✓ | — | — | — | — | — | — | — | ◆ | — | ✓ | — | — | — |
| **Starmetal Frag.** | L | — | — | — | — | — | ✓ | — | ✓ | — | — | ◆ | ✓ | — | — | — | — | — | ✓ | — | — | ✓ | — | — | — | — |
| **True Ice Shard** | L | — | — | — | — | — | ✓ | — | — | — | — | — | — | — | — | — | — | — | ◆ | — | ★ | — | — | — | — | — |
| **Adamantine Ore** | M | — | — | — | — | — | ✓ | — | — | — | — | — | ◆ | — | — | — | — | — | — | — | ✓ | ✓ | — | — | — | — |
| **Mithral Ore** | M | — | — | — | — | — | ✓ | — | — | — | — | — | ✓ | — | — | — | — | — | ✓ | — | ◆ | — | — | — | — | — |
| **Unearth. Dragonhide** | M | — | — | — | — | — | — | — | ✓ | — | — | ◆ | ✓ | — | — | — | — | — | — | — | — | ◆ | — | — | — | — |

> **Key changes from v0.1:**
> - **Glassfields** is the new signature biome for Rough Diamond, Rough Ruby, Rough Sapphire, and Rough Deep Crystal. Also primary for Ethereal Silica Sand.
> - **Blight** is the new signature biome for Alchemical Silver Ore (anti-magical metal forms in corrupted terrain). Also primary for Voidstone.
> - **Scorched** absorbs all former Volcanic affinities (Obsidian, Sulfur, Ignium signature).
> - **Rainforest** gets Amber (primary — resin-rich ancient trees produce fossilised amber).
> - **Woodland** gets standard access to common ores and Amber.
> - **Mangrove** has no mining areas.
> - Oasis, Beach, Deep Ocean columns removed.

---

## 2. Logging Resources by Biome 🔓

Logging requires forestry areas. Biomes with no trees cannot generate forestry areas.

Universal rare drop (any logging activity): **Worldroot**.

| Resource | Rar | Plns | Grs | For | Wdl | Rnf | Hgh | Scr | Sav | Swp | Msh | Jun | Mgr | Tai | Blt |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Resin** | C | — | — | ◆ | ✓ | ◆ | ✓ | — | ✓ | ✓ | — | ◆ | ✓ | ★ | ✓ |
| **Wood** | C | ✓ | ✓ | ★ | ◆ | ◆ | ✓ | ✓ | ✓ | ✓ | — | ◆ | ✓ | ◆ | ✓ |
| **Ironwood** | U | — | — | ◆ | ✓ | ✓ | ✓ | — | — | — | — | ✓ | — | ◆ | — |
| **Hartwood** | R | — | — | ★ | ✓ | ◆ | ✓ | — | — | — | — | ✓ | — | ✓ | — |
| **Moonwood** | E | — | — | ✓ | — | ★ | — | — | — | — | — | ✓ | — | ✓ | — |

> **Key changes from v0.1:**
> - **Rainforest** is the new signature biome for Moonwood (ancient temperate forests with centuries of moonlight exposure).
> - **Forest** drops from Moonwood primary to standard — Rainforest is older and wetter, better for moonlight-soaked timber.
> - **Woodland** gets standard Wood (primary), Ironwood, and Hartwood access — open-canopy timber.
> - **Blight** has distorted logging — Wood (standard), Resin (standard). Twisted trees yield usable but unsettling timber.
> - **Mangrove** has logging — dense salt-resistant wood, Resin from mangrove sap.
> - **Scrubland** has minimal logging — scrubby trees yield standard Wood only.
> - **Marsh** has no logging — reeds and grasses, no trees.

---

## 3. Farming Resources by Biome 🔓

Farming requires fertile areas with fertility > 0. Biomes with None fertility cannot farm.

Universal farming resources (appear in any fertile area with fertility ≥ Low): **Wheat, Barley, Beans, Vegetables, Tubers**.

| Resource | Rar | Plns | Grs | For | Wdl | Rnf | Hgh | Scr | Sav | Stp | Swp | Msh | Jun | Tai | Cst | Lke |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Flax** | C | ◆ | ◆ | ✓ | ◆ | — | ✓ | — | — | — | — | ✓ | — | — | — | ✓ |
| **Fruit** | C | ✓ | ✓ | ◆ | ◆ | — | — | ✓ | ✓ | — | — | — | ★ | — | — | — |
| **Hemp** | C | ◆ | ✓ | ✓ | ✓ | — | — | — | — | ✓ | — | ✓ | — | — | — | — |
| **Raw Cotton** | U | — | — | — | — | — | — | — | ◆ | — | — | — | ★ | — | — | — |
| **Dye Plants** | U | ✓ | ✓ | ✓ | ✓ | — | — | ★ | ✓ | — | — | ✓ | ◆ | — | — | — |
| **Hops** | U | ✓ | ◆ | ✓ | ◆ | — | ✓ | — | — | — | — | — | — | — | — | — |
| **Spice Plants** | U | — | — | — | — | — | — | ✓ | ✓ | — | — | — | ★ | — | — | — |
| **Sugar Cane** | U | — | — | — | — | — | — | — | ✓ | — | ✓ | ✓ | ★ | — | — | — |

> **Key changes from v0.1:**
> - **Woodland** gets primary Flax, Hops, and Fruit — open-canopy clearings allow more crop variety than dense Forest.
> - **Scrubland** is the new signature biome for Dye Plants (indigo, woad, madder thrive in Mediterranean climates).
> - **Rainforest** has no farming — too wet, too dense, too much shade. Foraging compensates.
> - Oasis column removed (biome removed). Jungle inherits its tropical crop signatures.

---

## 4. Foraging Resources by Biome 🔓

Foraging gathers wild-growing resources. Some foraging is possible even in biomes with no fertility (mushrooms, lichen), but yields are minimal.

Universal rare drop (any foraging activity): **Unicorn Hair**.

| Resource | Rar | Plns | Grs | For | Wdl | Rnf | Hgh | Scr | Dst | Sav | Stp | Bad | Swp | Msh | Mre | Jun | Mgr | Tun | Tai | Sco | Gls | Blt | Cst | Lke |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Mushrooms** | C | ✓ | ✓ | ★ | ✓ | ◆ | ✓ | — | — | — | — | — | ◆ | ✓ | — | ◆ | — | — | ◆ | ✓ | — | ✓ | — | — |
| **Roots** | C | ✓ | ✓ | ◆ | ✓ | ✓ | ✓ | ✓ | — | ✓ | ✓ | — | ✓ | ✓ | — | ✓ | — | ✓ | ✓ | — | — | ✓ | — | — |
| **Seaweed** | C | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | ★ | — | — | — | — | — | ◆ | ✓ |
| **Wild Berries** | C | ✓ | ◆ | ★ | ◆ | ✓ | ✓ | ✓ | — | — | ✓ | — | — | ✓ | — | ✓ | — | — | ◆ | — | — | — | — | — |
| **Wild Herbs** | C | ◆ | ◆ | ✓ | ✓ | ✓ | ✓ | ★ | — | ✓ | ✓ | — | ✓ | ✓ | — | ✓ | ✓ | — | ✓ | — | — | ✓ | ✓ | ✓ |
| **Honey** | U | ✓ | ◆ | ★ | ◆ | ✓ | ✓ | ◆ | — | ✓ | — | — | — | ✓ | — | ✓ | — | — | ✓ | — | — | — | — | — |
| **Med. Herbs** | U | ✓ | ✓ | ◆ | ✓ | ★ | ✓ | ✓ | — | ✓ | — | — | ◆ | ✓ | — | ◆ | ✓ | — | ✓ | — | — | ✓ | — | — |
| **Nuts** | U | — | ✓ | ★ | ◆ | ✓ | ✓ | — | — | — | — | — | — | — | — | ✓ | — | — | ✓ | — | — | — | — | — |
| **Poisonous Plants** | R | — | — | ✓ | — | — | — | — | — | — | — | — | ◆ | ✓ | ★ | ◆ | — | — | — | — | — | ★ | — | — |
| **Rare Herbs** | R | — | — | ◆ | — | ★ | ✓ | ✓ | — | — | — | — | ✓ | — | — | ◆ | — | — | — | — | — | ✓ | — | — |
| **Silkworms** | R | — | — | ◆ | — | ✓ | — | — | — | — | — | — | ✓ | — | — | ★ | — | — | — | — | — | — | — | — |
| **Truffles** | R | — | — | ★ | ✓ | ✓ | — | — | — | — | — | — | — | — | — | ✓ | — | — | ✓ | — | — | — | — | — |
| **Spiritbloom** | E | — | — | ✓ | — | ✓ | ✓ | — | — | — | — | — | ✓ | — | ✓ | ✓ | — | — | — | ✓ | — | ★ | — | — |

> **Key changes from v0.1:**
> - **Scrubland** is the new signature biome for Wild Herbs — aromatic Mediterranean herbs are the defining feature.
> - **Rainforest** is the new signature biome for Rare Herbs and Medicinal Herbs — ancient, wet, biodiverse forests harbour potent plants.
> - **Blight** is the new signature biome for Spiritbloom (feeds on residual magical corruption) and co-signature for Poisonous Plants (alongside Mire).
> - **Woodland** gets primary Wild Berries, Honey, and Nuts — open canopy allows flowering and fruiting.
> - **Mangrove** gets signature Seaweed and standard Medicinal Herbs and Wild Herbs.
> - **Glassfields** has no foraging — crystal terrain, no organic growth.

---

## 5. Hunting Fauna by Biome 🔓

Hunting targets wild fauna. Fauna type and density vary by biome. Beast parts yielded depend on beast tier (see Appendix D §3 and Appendix F §4).

| Resource | Rar | Plns | Grs | For | Wdl | Rnf | Hgh | Scr | Dst | Sav | Stp | Bad | Can | Swp | Msh | Mre | Jun | Mgr | Tun | Tai | Glc | Sco | Gls | Blt | Cst | Lke |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Game Meat** | C | ◆ | ◆ | ★ | ◆ | ✓ | ✓ | ✓ | ✓ | ★ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ★ | ✓ | ✓ | ✓ | ✓ | — | ✓ | ✓ | ✓ | ✓ |
| **Raw Bone** | C | ◆ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ◆ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | — | ✓ | ✓ | ✓ | ✓ |
| **Raw Fat** | C | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | — | ✓ | ✓ | ✓ | ✓ |
| **Raw Hide** | C | ◆ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ★ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ◆ | ✓ | ✓ | — | ✓ | ✓ | — | — |
| **Sinew** | C | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | — | ✓ | ✓ | — | — |
| **Feathers** | U | ✓ | ◆ | ✓ | ◆ | ✓ | ◆ | ✓ | — | ✓ | ◆ | — | ✓ | ✓ | ◆ | — | ✓ | ✓ | — | ✓ | — | — | — | ✓ | ✓ | ✓ |
| **Fur Pelt** | U | — | — | ◆ | ✓ | ✓ | ◆ | — | — | — | — | — | — | — | — | — | — | — | ★ | ★ | ✓ | — | — | — | — | — |
| **Ivory** | R | — | — | — | — | — | — | — | — | ★ | — | — | — | — | — | — | ◆ | — | ✓ | — | — | — | — | — | — | — |
| **Venom Sac** | R | — | — | ✓ | — | ✓ | — | ✓ | ◆ | — | — | ◆ | ✓ | ★ | ✓ | ◆ | ◆ | ✓ | — | — | — | — | — | ◆ | — | — |
| **Exotic Pelt** | E | — | — | ✓ | — | ✓ | — | — | — | ✓ | — | — | — | — | — | — | ★ | — | ✓ | ✓ | — | — | — | — | — | — |
| **Raw Demonhide** | L | — | — | — | — | — | — | — | — | — | — | ✓ | ✓ | ✓ | — | ◆ | — | — | — | — | — | ★ | — | ✓ | — | — |

> **Key changes from v0.1:**
> - **Woodland** gets hunting across all common products + Feathers (primary) — open canopy means good visibility for hunting.
> - **Rainforest** gets standard hunting access — ancient forest fauna, some Fur Pelt (wet-climate variants), Venom Sac.
> - **Scrubland** gets Venom Sac (standard) — vipers and scorpions in the scrub.
> - **Mangrove** gets standard hunting — crocodiles, sea snakes, crabs. Also Venom Sac (standard).
> - **Blight** gets Venom Sac (primary) — mutated venomous fauna. Also Raw Demonhide (standard) — corruption attracts dark creatures.
> - **Glassfields** gets standard common hunting products — crystal-adapted fauna.
> - **Scorched** has no standard hunting — fire fauna handled via encounters only. Raw Demonhide signature.

---

## 6. Fishing by Biome 🔓

Fishing requires a water biome or water-adjacent hex. Only Coast, Lake, Mangrove, Swamp, and Marsh support fishing in the base module.

| Resource | Rarity | Coast | Lake | Mangrove | Swamp | Marsh |
|---|---|---|---|---|---|---|
| **Fish** | C | ★ | ★ | ★ | ✓ | ✓ |

> **Mangrove** is the third signature fishing biome — tidal root systems create sheltered nurseries that concentrate fish.

---

## 7. Herding by Biome 🔓

Herding requires fertile areas converted from native vegetation. Biome determines which livestock types are viable and their productivity multiplier.

| Livestock | Plns | Grs | For | Wdl | Hgh | Scr | Sav | Stp | Msh | Jun | Tai | Cst | Lke |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Cattle** | ★ | ★ | ☆ | ✓ | ✓ | ☆ | ◆ | ✓ | ☆ | — | — | — | — |
| **Sheep** | ✓ | ◆ | ☆ | ✓ | ★ | ✓ | ✓ | ◆ | — | — | — | — | — |
| **Poultry** | ◆ | ◆ | ✓ | ◆ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| **Horses/Donkeys** | ★ | ★ | ☆ | ✓ | ✓ | ☆ | ◆ | ◆ | — | — | — | — | — |

> **Legend**: ★ = ideal, ◆ = good, ✓ = viable, ☆ = marginal (50% yield penalty), — = not viable.
>
> **Key changes from v0.1:**
> - **Woodland** gets viable herding for all livestock types — open canopy allows some grazing.
> - **Scrubland** gets marginal Cattle/Horses (rocky terrain limits grazing) and viable Sheep/Poultry.
> - No herding in infertile biomes (Desert, Badlands, Canyon, Mire, Tundra, Glacier, Scorched, Glassfields, Blight, Mangrove).

---

*Phase 4b of the Eternal Game Base Module Quantification Plan.*
*v0.2.0 — March 2026*
