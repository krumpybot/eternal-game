# Appendix D: Biome Table

> Phase 4a of the Eternal Game Base Module Quantification Plan.
> Dependencies: Phase 1 (units/constants), Phase 2 (action catalog), Phase 3 (resource catalog).
> All values are base-module defaults. The autoregulator may adjust multipliers within bounded ranges. Game Masters may propose changes via consensus.

---

## Biome Categories

The Eternal World contains **25 biomes** grouped into 6 categories. Biomes are determined at world generation via `biome = noise_function(global_seed, x, y, z)` and are **permanent and immutable** — no biome ever changes after deployment.

| Category | Biomes | Count |
|---|---|---|
| **Temperate** | Plains, Grassland, Forest, Highlands | 4 |
| **Arid** | Desert, Savanna, Steppe, Badlands, Canyon | 5 |
| **Wet** | Swamp, Wetlands, Mire, Jungle | 4 |
| **Cold** | Tundra, Taiga, Glacier | 3 |
| **Extreme** | Volcanic, Oasis | 2 |
| **Water** | Coast, Beach, Coastal Waters, Lake, Ocean, Deep Ocean | 6 |
| | **Total** | **25** |

> **Water biomes** (Ocean, Deep Ocean, Coastal Waters) are impassable in the base module. Traversal requires a future Maritime module. Coast, Beach, and Lake are land-accessible.

---

## 1. Biome Property Table

All modifiers are multiplicative against the base action costs defined in Phase 2. A modifier of `1.0×` means no change from base cost.

### Column Definitions

| Column | Description |
|---|---|
| **Move Cost** | Energy multiplier for the Travel action (base: 25–45 energy). Higher = harder to cross. |
| **Move Time** | Time-lock multiplier for Travel (base: 72–216 ticks). Higher = slower traversal. |
| **Explore Cost** | Energy multiplier for the Explore action (base: 40–60 energy). |
| **Explore Time** | Time-lock multiplier for Explore (base: 108–324 ticks). |
| **Decay Rate** | Multiplier on territorial decay (upkeep cost). Higher = more expensive to hold. |
| **Base Hazard** | Base encounter chance per action (%). Modified by hex development, time of day, action type. |
| **Fertility** | Base soil quality for farming/foraging. 0 = infertile. |
| **Traversable** | Whether adventurers can enter this hex in the base module. |

### Temperate Biomes

| Biome | Move Cost | Move Time | Explore Cost | Explore Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|---|
| **Plains** | 0.8× | 0.8× | 0.9× | 0.9× | 0.8× | 5% | High | ✓ |
| **Grassland** | 0.9× | 0.9× | 0.9× | 0.9× | 0.9× | 6% | High | ✓ |
| **Forest** | 1.2× | 1.2× | 1.1× | 1.2× | 1.0× | 12% | Medium | ✓ |
| **Highlands** | 1.3× | 1.3× | 1.2× | 1.3× | 1.1× | 10% | Medium | ✓ |

### Arid Biomes

| Biome | Move Cost | Move Time | Explore Cost | Explore Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|---|
| **Desert** | 1.5× | 1.4× | 1.4× | 1.3× | 1.5× | 8% | None | ✓ |
| **Savanna** | 1.0× | 1.0× | 1.0× | 1.0× | 1.0× | 9% | Medium | ✓ |
| **Steppe** | 0.9× | 0.9× | 1.0× | 1.0× | 1.0× | 7% | Low | ✓ |
| **Badlands** | 1.4× | 1.3× | 1.3× | 1.3× | 1.4× | 11% | None | ✓ |
| **Canyon** | 1.6× | 1.5× | 1.5× | 1.5× | 1.3× | 13% | None | ✓ |

### Wet Biomes

| Biome | Move Cost | Move Time | Explore Cost | Explore Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|---|
| **Swamp** | 1.5× | 1.5× | 1.3× | 1.4× | 1.6× | 15% | Low | ✓ |
| **Wetlands** | 1.3× | 1.3× | 1.2× | 1.2× | 1.4× | 10% | Medium | ✓ |
| **Mire** | 1.7× | 1.6× | 1.5× | 1.5× | 1.8× | 18% | None | ✓ |
| **Jungle** | 1.4× | 1.4× | 1.3× | 1.4× | 1.3× | 16% | High | ✓ |

### Cold Biomes

| Biome | Move Cost | Move Time | Explore Cost | Explore Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|---|
| **Tundra** | 1.3× | 1.3× | 1.2× | 1.3× | 1.2× | 8% | None | ✓ |
| **Taiga** | 1.2× | 1.2× | 1.1× | 1.2× | 1.1× | 10% | Low | ✓ |
| **Glacier** | 1.8× | 1.7× | 1.6× | 1.6× | 2.0× | 6% | None | ✓ |

### Extreme Biomes

| Biome | Move Cost | Move Time | Explore Cost | Explore Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|---|
| **Volcanic** | 1.6× | 1.5× | 1.5× | 1.5× | 1.7× | 14% | None | ✓ |
| **Oasis** | 0.9× | 0.9× | 0.8× | 0.8× | 0.7× | 4% | Very High | ✓ |

### Water Biomes

| Biome | Move Cost | Move Time | Explore Cost | Explore Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|---|
| **Coast** | 1.1× | 1.1× | 1.0× | 1.0× | 1.1× | 7% | Low | ✓ |
| **Beach** | 1.0× | 1.0× | 0.9× | 0.9× | 1.0× | 5% | None | ✓ |
| **Lake** | 1.2× | 1.2× | 1.1× | 1.1× | 1.0× | 6% | Low | ✓ |
| **Coastal Waters** | — | — | — | — | — | — | None | ✗ |
| **Ocean** | — | — | — | — | — | — | None | ✗ |
| **Deep Ocean** | — | — | — | — | — | — | None | ✗ |

> **Impassable biomes** have no action modifiers — adventurers cannot enter them. These hexes exist on the map and are visible, but function as barriers until a Maritime module enables water traversal.

---

## 2. Encounter Type Distribution

Each biome has a weighted distribution of encounter types when an encounter triggers. Weights sum to 100%.

| Biome | Beast | Combat | Social | Special |
|---|---|---|---|---|
| **Plains** | 40% | 20% | 30% | 10% |
| **Grassland** | 45% | 15% | 30% | 10% |
| **Forest** | 55% | 15% | 15% | 15% |
| **Highlands** | 50% | 20% | 15% | 15% |
| **Desert** | 45% | 25% | 15% | 15% |
| **Savanna** | 55% | 15% | 20% | 10% |
| **Steppe** | 40% | 25% | 25% | 10% |
| **Badlands** | 50% | 30% | 5% | 15% |
| **Canyon** | 45% | 25% | 10% | 20% |
| **Swamp** | 60% | 15% | 5% | 20% |
| **Wetlands** | 50% | 10% | 20% | 20% |
| **Mire** | 65% | 15% | 0% | 20% |
| **Jungle** | 60% | 10% | 10% | 20% |
| **Tundra** | 35% | 20% | 20% | 25% |
| **Taiga** | 50% | 15% | 15% | 20% |
| **Glacier** | 25% | 10% | 5% | 60% |
| **Volcanic** | 40% | 15% | 5% | 40% |
| **Oasis** | 20% | 10% | 55% | 15% |
| **Coast** | 35% | 20% | 30% | 15% |
| **Beach** | 25% | 15% | 45% | 15% |
| **Lake** | 40% | 10% | 30% | 20% |

> **Encounter type definitions** (from Appendix M):
> - **Beast**: Wildlife attack. Scales with biome fauna tier and adventurer position.
> - **Combat**: Hostile NPC or environmental combat. Ambushes, bandits, territorial fauna.
> - **Social**: Non-violent encounters. Traders, travellers, lost adventurers, mysterious strangers.
> - **Special**: Rare events. Discoveries, anomalies, lore reveals, unique one-time occurrences.

### Design Notes

- **Mire** has 0% social encounters — no travellers willingly enter the mire.
- **Glacier** and **Volcanic** have very high special encounter rates — these extreme environments hold ancient mysteries (True Ice deposits, Ignium veins, dragon burial sites).
- **Oasis** is the most social biome — a natural meeting point for travellers crossing the desert.
- **Beach** has high social rate — coastal meeting points, shipwrecked travellers (future maritime tie-in).
- Higher hazard biomes skew toward beast encounters. Civilised biomes (Plains, Grassland, Oasis) skew toward social.

---

## 3. Fauna Tier by Biome

Defines the maximum beast tier that can appear in each biome. Beast tiers range from T1 (common nuisance) to T5 (mythic apex predator). Higher tiers yield better beast parts (see Appendix F §4: Beast Parts).

| Biome | Max Beast Tier | Typical Fauna | Notes |
|---|---|---|---|
| **Plains** | T2 | Wolves, boar, wild dogs | Open terrain, herding beasts |
| **Grassland** | T2 | Prairie cats, bison, snakes | Open, mild threat |
| **Forest** | T3 | Bears, wolves, great stags, spiders | Dense cover, ambush predators |
| **Highlands** | T3 | Mountain cats, eagles, rams, wyverns | Elevation advantage for fauna |
| **Desert** | T3 | Scorpions, sandworms, vipers | Dehydration compounds danger |
| **Savanna** | T3 | Lions, hyenas, great cats, rhinos | Pack and apex predators |
| **Steppe** | T2 | Wolves, wild horses, hawks | Open, less threatening |
| **Badlands** | T3 | Basilisks, rock drakes, scorpions | Ambush terrain, venomous |
| **Canyon** | T4 | Rocs, basilisks, cave trolls | Vertical danger, lair terrain |
| **Swamp** | T4 | Hydras, bog serpents, crocodiles, leeches | Treacherous, poisonous |
| **Wetlands** | T3 | Crocodiles, snapping turtles, herons | Semi-aquatic threats |
| **Mire** | T4 | Mire beasts, will-o-wisps, bog horrors | The worst wetland has to offer |
| **Jungle** | T4 | Jungle cats, serpents, apes, spiders | Dense canopy, ambush + poison |
| **Tundra** | T3 | Dire wolves, snow bears, mammoths | Cold-adapted apex predators |
| **Taiga** | T3 | Bears, wolves, moose, lynx | Northern forest predators |
| **Glacier** | T5 | Ice wyrms, frost giants, ancient things | Mythic creatures survive here |
| **Volcanic** | T5 | Fire elementals, lava serpents, dragons | Extreme fauna for extreme terrain |
| **Oasis** | T1 | Scorpions, small vipers | Safe haven in the desert |
| **Coast** | T2 | Seabirds (aggressive), crabs, sea serpents | Mild coastal fauna |
| **Beach** | T1 | Crabs, seagulls | Minimal threat |
| **Lake** | T2 | Lake serpents, snapping turtles | Mild freshwater fauna |

> **Beast tier → Beast part quality**: T1→Common parts, T2→Uncommon, T3→Rare, T4→Epic, T5→Legendary. Mythic beast parts require Mythic beasts which are encounter-specific (not biome-spawned) and may be introduced by Game Masters.

---

## 4. Biome Clustering & Generation

Biome placement uses **simplex noise** with domain-separated layers to produce natural terrain transitions. This section defines the generation rules.

### 4.1 Noise Layers

| Layer | Seed | Purpose | Scale |
|---|---|---|---|
| **Temperature** | `hash(global_seed, "TEMP_V1", x, y, z)` | Hot ↔ Cold gradient | Large (64–128 hex wavelength) |
| **Moisture** | `hash(global_seed, "MOIST_V1", x, y, z)` | Wet ↔ Dry gradient | Large (64–128 hex wavelength) |
| **Elevation** | `hash(global_seed, "ELEV_V1", x, y, z)` | Sea level ↔ Mountain | Medium (32–64 hex wavelength) |
| **Variation** | `hash(global_seed, "VAR_V1", x, y, z)` | Local microclimate/terrain variation | Small (8–16 hex wavelength) |

### 4.2 Biome Determination

The four noise values map to a biome via a **lookup table**. Each biome occupies a region in the 4D noise space (temperature × moisture × elevation × variation). Boundaries are defined to produce coherent clustering:

```
biome = biome_lookup(temperature, moisture, elevation, variation)
```

| Temperature | Moisture | Elevation | Biome | Variation Override |
|---|---|---|---|---|
| Hot | Dry | Low | Desert | — |
| Hot | Dry | Medium | Badlands | Canyon (high variation) |
| Hot | Dry | High | Canyon | — |
| Hot | Medium | Low | Savanna | — |
| Hot | Medium | Medium | Steppe | — |
| Hot | Wet | Low | Jungle | — |
| Hot | Wet | Medium | Swamp | Mire (high variation) |
| Hot | Wet (pocket) | Low (surrounded by Desert) | Oasis | Requires dry neighbours |
| Temperate | Dry | Low | Plains | — |
| Temperate | Dry | Medium | Grassland | — |
| Temperate | Medium | Low | Grassland | Plains (low variation) |
| Temperate | Medium | Medium | Forest | — |
| Temperate | Medium | High | Highlands | — |
| Temperate | Wet | Low | Wetlands | — |
| Temperate | Wet | Medium | Forest | Swamp (high variation) |
| Cold | Dry | Low | Tundra | — |
| Cold | Dry | Medium | Steppe | Tundra (low variation) |
| Cold | Medium | Low | Taiga | — |
| Cold | Medium | Medium | Taiga | — |
| Cold | Wet | Low | Wetlands | Mire (cold + wet + high variation) |
| Cold | Any | High | Glacier | — |
| Extreme Hot | Any | Low–Medium | Volcanic | — |
| Any | Any | Sea level | Coast / Beach / Lake | Determined by water body size |
| Any | Any | Below sea level | Coastal Waters / Ocean / Deep Ocean | Depth determines sub-type |

> **Oasis** is the rarest biome — it requires a specific conjunction: hot temperature, wet moisture pocket, low elevation, surrounded by at least 4 desert/badlands neighbours. This is checked post-noise and overrides the otherwise-dry classification.

### 4.3 Water Body Classification

Water biomes are determined by elevation and extent:

| Condition | Biome |
|---|---|
| Sea-level hex with ≤2 adjacent sea-level hexes | **Lake** |
| Sea-level hex with 3+ adjacent sea-level hexes, ≤1 hex from land | **Coast** |
| Sea-level hex adjacent to Coast, low elevation gradient | **Beach** |
| Below sea level, adjacent to Coast | **Coastal Waters** |
| Below sea level, not adjacent to Coast | **Ocean** |
| Below sea level, no Coast within 3 hexes | **Deep Ocean** |

> Water body classification is evaluated after the base noise pass, using neighbour analysis. This creates natural coastlines, lakes, and ocean depth gradients.

### 4.4 Distribution Targets

The noise function parameters are tuned to produce approximately these global biome frequencies. Actual distribution will vary by region — the world is not uniform.

| Category | Target % | Notes |
|---|---|---|
| **Temperate** | ~30% | The most common and habitable terrain |
| **Arid** | ~18% | Large deserts with scattered oases |
| **Wet** | ~12% | Clustered, harder to develop |
| **Cold** | ~10% | Northern/polar regions |
| **Extreme** | ~3% | Volcanic islands, rare oases |
| **Water** | ~27% | Oceans, coasts, lakes |
| | **100%** | |

Within categories, approximate internal distributions:

| Biome | Approx % of world | Biome | Approx % of world |
|---|---|---|---|
| Plains | 10% | Tundra | 4% |
| Grassland | 9% | Taiga | 4% |
| Forest | 7% | Glacier | 2% |
| Highlands | 4% | Volcanic | 2% |
| Desert | 7% | Oasis | 1% |
| Savanna | 4% | Coast | 5% |
| Steppe | 3% | Beach | 3% |
| Badlands | 2% | Coastal Waters | 4% |
| Canyon | 2% | Lake | 3% |
| Swamp | 3% | Ocean | 8% |
| Wetlands | 3% | Deep Ocean | 4% |
| Mire | 2% | | |
| Jungle | 4% | | |

> These are **tuning targets**, not hard constraints. The noise function is calibrated to approach these percentages across large sample areas (>10,000 hexes). Local regions will deviate naturally — a player exploring northward will encounter predominantly cold biomes; equatorial exploration will be hot and wet.

### 4.5 Nexus Region

The origin hex `(0,0,0)` — The Nexus — is forced to **Plains** biome regardless of noise output. The surrounding ring (6 adjacent hexes) is biased toward temperate biomes (Plains, Grassland, Forest) to ensure new players have a hospitable starting region.

Beyond the first ring, the noise function operates normally with no overrides.

---

## 5. Fertility Ratings

Fertility determines base farming and foraging yields. Each biome has a fertility classification that translates to a numerical modifier.

| Rating | Modifier | Meaning |
|---|---|---|
| **Very High** | 1.5× | Exceptional growing conditions |
| **High** | 1.2× | Good soil, adequate water |
| **Medium** | 1.0× | Baseline — functional but not special |
| **Low** | 0.6× | Poor soil or harsh conditions; limited crops viable |
| **None** | 0.0× | Cannot farm. Foraging yields minimal wild plants only (mushrooms, lichen). |

### Fertility by Biome

| Biome | Fertility | Farmable Crops | Foraging Quality |
|---|---|---|---|
| **Plains** | High (1.2×) | All grain, vegetables, flax, hemp | Moderate wild herbs |
| **Grassland** | High (1.2×) | All grain, vegetables, hemp | Good wild herbs, berries |
| **Forest** | Medium (1.0×) | Limited (shade crops) | Excellent — mushrooms, herbs, nuts, berries, truffles, silkworms |
| **Highlands** | Medium (1.0×) | Hardy crops (tubers, barley) | Moderate — herbs, roots |
| **Desert** | None (0.0×) | None | Minimal — desert lichen, rare cacti |
| **Savanna** | Medium (1.0×) | Drought-tolerant (sorghum, beans) | Moderate — wild grains, roots |
| **Steppe** | Low (0.6×) | Hardy grasses, barley | Sparse — roots, wild herbs |
| **Badlands** | None (0.0×) | None | Minimal — hardy scrub only |
| **Canyon** | None (0.0×) | None | Minimal — canyon ferns, lichen |
| **Swamp** | Low (0.6×) | Rice (unique), tubers | Good — mushrooms, herbs, medicinal herbs |
| **Wetlands** | Medium (1.0×) | Rice, tubers, vegetables | Good — reeds, herbs, wild berries |
| **Mire** | None (0.0×) | None | Poor — poisonous plants dominant |
| **Jungle** | High (1.2×) | Tropical (sugar cane, spice plants, fruit) | Excellent — wild fruit, herbs, rare herbs, spiritbloom |
| **Tundra** | None (0.0×) | None | Sparse — lichen, hardy roots |
| **Taiga** | Low (0.6×) | Very limited (tubers only) | Moderate — mushrooms, berries, pine nuts |
| **Glacier** | None (0.0×) | None | None |
| **Volcanic** | None (0.0×) | None | Minimal — heat-tolerant fungi |
| **Oasis** | Very High (1.5×) | All crops viable; tropical crops thrive | Excellent — concentrated biodiversity |
| **Coast** | Low (0.6×) | Salt-tolerant crops (limited) | Good — seaweed, wild herbs |
| **Beach** | None (0.0×) | None | Sparse — seaweed, driftwood |
| **Lake** | Low (0.6×) | Lakeside crops (limited) | Moderate — reeds, wild herbs |

---

## 6. Biome Suitability Summaries

A quick-reference for each biome's overall suitability for different activities.

### Rating Scale

| Symbol | Meaning |
|---|---|
| ★★★ | Excellent — this biome is ideal for this activity |
| ★★☆ | Good — viable with moderate effort |
| ★☆☆ | Poor — possible but inefficient or limited |
| ☆☆☆ | Impossible or negligible — not viable |

### Suitability Matrix

| Biome | Mining | Logging | Farming | Foraging | Hunting | Fishing | Herding | Settlement |
|---|---|---|---|---|---|---|---|---|
| **Plains** | ★☆☆ | ☆☆☆ | ★★★ | ★★☆ | ★☆☆ | ☆☆☆ | ★★★ | ★★★ |
| **Grassland** | ★☆☆ | ☆☆☆ | ★★★ | ★★☆ | ★☆☆ | ☆☆☆ | ★★★ | ★★★ |
| **Forest** | ★☆☆ | ★★★ | ★☆☆ | ★★★ | ★★★ | ☆☆☆ | ☆☆☆ | ★★☆ |
| **Highlands** | ★★★ | ★☆☆ | ★★☆ | ★★☆ | ★★☆ | ☆☆☆ | ★★☆ | ★★☆ |
| **Desert** | ★★☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ | ★☆☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ |
| **Savanna** | ★☆☆ | ★☆☆ | ★★☆ | ★★☆ | ★★★ | ☆☆☆ | ★★☆ | ★★☆ |
| **Steppe** | ★☆☆ | ☆☆☆ | ★☆☆ | ★☆☆ | ★★☆ | ☆☆☆ | ★★☆ | ★☆☆ |
| **Badlands** | ★★★ | ☆☆☆ | ☆☆☆ | ☆☆☆ | ★☆☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ |
| **Canyon** | ★★★ | ☆☆☆ | ☆☆☆ | ☆☆☆ | ★★☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ |
| **Swamp** | ★☆☆ | ★☆☆ | ★☆☆ | ★★★ | ★★☆ | ★★☆ | ☆☆☆ | ☆☆☆ |
| **Wetlands** | ★☆☆ | ★☆☆ | ★★☆ | ★★☆ | ★★☆ | ★★☆ | ☆☆☆ | ★☆☆ |
| **Mire** | ★☆☆ | ☆☆☆ | ☆☆☆ | ★☆☆ | ★★☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ |
| **Jungle** | ★☆☆ | ★★☆ | ★★☆ | ★★★ | ★★★ | ☆☆☆ | ☆☆☆ | ★☆☆ |
| **Tundra** | ★★☆ | ☆☆☆ | ☆☆☆ | ★☆☆ | ★★☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ |
| **Taiga** | ★★☆ | ★★★ | ☆☆☆ | ★★☆ | ★★☆ | ☆☆☆ | ☆☆☆ | ★☆☆ |
| **Glacier** | ★★☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ | ★☆☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ |
| **Volcanic** | ★★★ | ☆☆☆ | ☆☆☆ | ☆☆☆ | ★☆☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ |
| **Oasis** | ☆☆☆ | ★☆☆ | ★★★ | ★★★ | ★☆☆ | ★☆☆ | ★★☆ | ★★★ |
| **Coast** | ★☆☆ | ☆☆☆ | ★☆☆ | ★★☆ | ★☆☆ | ★★★ | ☆☆☆ | ★★☆ |
| **Beach** | ☆☆☆ | ☆☆☆ | ☆☆☆ | ★☆☆ | ☆☆☆ | ★★★ | ☆☆☆ | ★☆☆ |
| **Lake** | ★☆☆ | ☆☆☆ | ★☆☆ | ★★☆ | ★☆☆ | ★★★ | ☆☆☆ | ★★☆ |

### Design Notes

- **Mining** is best in hard terrain: Highlands, Badlands, Canyon, Volcanic. These biomes are hard to reach and hold — mining requires commitment.
- **Logging** is best in Forest, Taiga, and Jungle — where trees are dense.
- **Farming** concentrates in Plains, Grassland, and Oasis — the breadbaskets of the world. Jungle supports tropical crops.
- **Foraging** is richest in biodiverse biomes: Forest, Jungle, Oasis, Swamp.
- **Hunting** is best where fauna density is high: Forest, Savanna, Jungle.
- **Fishing** requires water adjacency: Coast, Beach, Lake, with some Swamp/Wetlands access.
- **Herding** needs open grazing: Plains, Grassland. Highlands and Savanna are secondary.
- **Settlement** favours hospitable, productive biomes: Plains, Grassland, Oasis. Extreme and barren biomes are settlement-hostile.

---

*Phase 4a of the Eternal Game Base Module Quantification Plan.*
*v0.1.0 — March 2026*
