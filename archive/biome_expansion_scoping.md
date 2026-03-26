# Biome Expansion Scoping — Pre-Phase 4 Lock Review

> These biomes are onchain forever. This document reviews the current 25 against real-world biome science and proposes additions before the list is locked.

---

## Current 25 Biomes

| # | Category | Biome | Real-World Basis |
|---|---|---|---|
| 1 | Temperate | Plains | ✓ Temperate grassland (flat, open, fertile) |
| 2 | Temperate | Grassland | ✓ Temperate grassland (rolling, pasture) |
| 3 | Temperate | Forest | ⚠️ Generic — covers deciduous, mixed, and temperate evergreen |
| 4 | Temperate | Highlands | ⚠️ Conflates montane and alpine zones |
| 5 | Arid | Desert | ✓ Hot desert / xeric shrubland |
| 6 | Arid | Savanna | ✓ Tropical/subtropical grassland with scattered trees |
| 7 | Arid | Steppe | ✓ Semi-arid grassland (continental) |
| 8 | Arid | Badlands | ✓ Eroded arid terrain (geological, not a standard biome) |
| 9 | Arid | Canyon | ✓ Deep gorge terrain (geological feature, not a standard biome) |
| 10 | Wet | Swamp | ✓ Forested wetland |
| 11 | Wet | Wetlands | ⚠️ Generic — covers marshes, fens, floodplains |
| 12 | Wet | Mire | ✓ Boggy waterlogged terrain (peat-forming) |
| 13 | Wet | Jungle | ✓ Tropical rainforest (dense, wet, hot) |
| 14 | Cold | Tundra | ✓ Arctic/alpine treeless plain |
| 15 | Cold | Taiga | ✓ Boreal coniferous forest |
| 16 | Cold | Glacier | ✓ Permanent ice sheet |
| 17 | Extreme | Volcanic | ✓ Active volcanic terrain |
| 18 | Extreme | Oasis | ⚠️ Micro-biome, not a standard biome classification |
| 19 | Water | Coast | ✓ Littoral zone |
| 20 | Water | Beach | ⚠️ Sub-feature of coast, not a distinct biome |
| 21 | Water | Coastal Waters | ✓ Shallow marine / continental shelf |
| 22 | Water | Lake | ✓ Freshwater lentic |
| 23 | Water | Ocean | ✓ Pelagic marine |
| 24 | Water | Deep Ocean | ✓ Abyssal marine |
| 25 | Extreme | — | (slot available) |

---

## Gap Analysis: What's Missing from Real-World Biomes

### Critical Gaps (Major real-world biomes with no equivalent)

| Missing Biome | WWF Category | Why It Matters | Current Nearest |
|---|---|---|---|
| **Scrubland** (Mediterranean / Chaparral) | Mediterranean Forests, Woodlands & Scrub | One of WWF's 14 major biomes. Warm dry summers, mild wet winters. Aromatic herbs, olive-type trees, fire-adapted. Think Mediterranean coast, California, South Africa fynbos. Distinct from both Desert and Forest. | Forest (too wet/dense) or Steppe (too dry/treeless) |
| **Woodland** (Open canopy forest) | Tropical/Subtropical Dry Broadleaf | Open-canopy tree cover with grass understory. Distinct from dense Forest — more light, different fauna, different resources. Think English oak woodland, African miombo. | Forest (too dense) |
| **Mangrove** | Mangroves (WWF #14) | Tidal coastal forest with prop-rooted trees in brackish water. Unique ecosystem: coastal protection, nursery habitat, tannin-rich wood. Found in every tropical/subtropical coast globally. | Swamp (not coastal/tidal) or Coast (no trees) |
| **Alpine** (above tree line) | Montane Grasslands & Shrublands | Treeless high-altitude terrain with hardy grasses, lichen, and extreme winds. Distinct from Highlands (which implies habitable upland) and Tundra (which is latitude-driven, not altitude). Mountain peaks. | Highlands (too habitable) or Tundra (wrong mechanism) |
| **Bog** / Peatland | Flooded Grasslands & Savannas (partial) | Acidic, waterlogged, peat-accumulating. Sphagnum moss, carnivorous plants, preserved bodies. Distinct from Mire (which is generically boggy) — though Mire could be renamed. | Mire (close but not identical) |
| **Marsh** (freshwater emergent) | Flooded Grasslands | Shallow freshwater with reeds, cattails, wading birds. Distinct from Swamp (which has trees). Productive but flood-prone. | Wetlands (generic) |

### Moderate Gaps (Recognisable terrain types that add gameplay variety)

| Missing Biome | Description | Gameplay Value |
|---|---|---|
| **Moorland / Heath** | Open, wind-swept, low shrubland on acidic soil. Heather, gorse, bracken. Think Scottish Highlands, Dartmoor, Scandinavian vidda. | Distinct aesthetic; poor farming but unique foraging (heather honey, peat); low-tier hunting; atmospheric encounters |
| **Temperate Rainforest** | Moss-draped ancient forest. Dense, wet, temperate. Pacific Northwest, Chilean Valdivian, New Zealand. Distinct from Jungle (cold, mossy vs hot, tropical). | Highest-quality logging (ancient trees), unique foraging, extremely dense canopy |
| **Karst** | Limestone terrain with sinkholes, caves, underground rivers. Think Guilin, Slovenian karst, Yucatan cenotes. | Natural underground access; unique mining; cave systems; distinct from Canyon (which is surface-level) |
| **Salt Flat** | Flat mineral-encrusted terrain. Blinding white. Think Bonneville, Salar de Uyuni, Atacama. | Salt production, extreme conditions, minimal life, mineral extraction, disorienting travel |
| **River / Riverbank** | Flowing freshwater corridor with riparian vegetation. | Trade routes, irrigation, fishing, distinct fauna, natural barriers. Currently no flowing-water biome. |
| **Dunes** | Sand dune fields (erg). Shifting terrain, different from flat desert. Think Saharan erg, Namib sand sea. | Difficult traversal, sand-specific resources, disorientation, burial hazard |
| **Reef** | Shallow marine with coral structures. | Future maritime content; unique resources (coral, pearls, shellfish); dangerous currents |
| **Estuary** | Where river meets sea. Brackish, nutrient-rich, muddy. | High fishing yield, salt production, trade hub potential, mangrove adjacency |

### Minor Gaps (Could be sub-types or left to Game Masters)

| Terrain | Notes |
|---|---|
| **Mesa / Plateau** | Flat-topped elevation. Could fold into Highlands or Badlands. |
| **Ravine** | Smaller canyon. Probably covered by Canyon. |
| **Archipelago / Island** | Isolated land mass — more a geographic feature than a biome. |
| **Floodplain** | Seasonal flooding near rivers. Could be a Wetlands sub-type. |
| **Chaparral** (specifically) | Mediterranean scrubland. Covered by Scrubland suggestion above. |
| **Temperate Deciduous Forest** vs **Temperate Evergreen Forest** | Currently both covered by "Forest". Could split, but may be over-specific for a hex game. |
| **Shrubland** | Low woody plants. Covered by Scrubland/Moorland suggestions. |

---

## Naming Issues with Current Biomes

| Current Name | Issue | Suggestion |
|---|---|---|
| **Forest** | Too generic — covers deciduous, evergreen, mixed, temperate rainforest | Keep as-is (the generic temperate forest) but add **Woodland** (open canopy) and **Temperate Rainforest** as distinct biomes. Or rename to **Deciduous Forest** for precision. |
| **Highlands** | Implies habitable upland terrain, but real highlands range from Scottish moors to Andean altiplano. Works well for gameplay as "elevated but liveable". | Keep. Add **Alpine** for the harsh treeless peaks above. |
| **Mire** | Accurate for waterlogged boggy terrain, but overlaps with Bog/Peatland concepts. | Keep as-is. Mire covers the "worst wetland" niche well. Could rename to **Bog** if preferred — more evocative. |
| **Wetlands** | Too generic — a category, not a specific biome. | Rename to **Marsh** (freshwater emergent wetland) for specificity. |
| **Beach** | Very narrow scope — a sub-feature of coastline. | Keep for gameplay (fishing access, social encounters, safe harbour). It works as the "easy coast" vs Coast being the "productive coast". |
| **Badlands** | Geological term, not a standard biome. | Keep — it reads well for a game and provides distinct mining terrain. |
| **Canyon** | Geological feature, not a standard biome. | Keep — same reasoning as Badlands. Deep vertical terrain with unique gameplay. |
| **Oasis** | Micro-feature, not a standard biome at any classification scale. | Keep — rare, valuable, creates strategic focal points. Its rarity (1% of world) makes it feel like a discovery. |
| **Plains** vs **Grassland** | Considerable overlap. Both are temperate grasslands. | Keep both — Plains (flat, agricultural) vs Grassland (rolling, pastoral) is a useful gameplay distinction even if ecologically similar. Plains = breadbasket, Grassland = herding. |

---

## Recommended Real-World Additions

### Tier 1: Strong recommendations (fill major gaps)

| # | Biome | Category | Rationale |
|---|---|---|---|
| 1 | **Scrubland** | Temperate | Fills the Mediterranean/chaparral gap. Aromatic herbs, olive-type trees, fire-adapted ecology. One of WWF's 14 major biomes — significant omission. Moderate fertility, good foraging (herbs, honey), poor logging, light mining. |
| 2 | **Woodland** | Temperate | Open-canopy tree cover. Bridges the gap between Forest and Grassland. Good hunting (visibility + fauna), moderate logging, some farming (clearings). Different tactical feel from dense Forest. |
| 3 | **Alpine** | Cold | Treeless high peaks. Harsh, windswept, minimal resources but unique minerals and encounters. Distinct from Highlands (below tree line, habitable) and Tundra (latitude-driven). Extreme movement cost, very high special encounter rate. |
| 4 | **Mangrove** | Water/Wet | Tidal coastal forest. Unique hybrid of coast + swamp. Rich fishing, tannin wood, shelter from storms. Required if we want believable tropical coastlines. |
| 5 | **Marsh** | Wet | Rename **Wetlands** → **Marsh**. Freshwater emergent wetland with reeds and wading birds. More specific, more evocative. Same gameplay stats, better name. |

### Tier 2: Good additions (add variety and fill moderate gaps)

| # | Biome | Category | Rationale |
|---|---|---|---|
| 6 | **Moorland** | Temperate | Open wind-swept heath. Peat, heather, gorse. Poor farming but unique foraging. Atmospheric. Fills the shrubland/heath gap. Think Wuthering Heights. |
| 7 | **Karst** | Extreme | Limestone cave terrain. Natural underground access, sinkholes, underground rivers. Unique mining (gems, crystals in cave systems). Distinct from Canyon (surface) and any future Underworld module. |
| 8 | **Salt Flat** | Arid | Blinding white mineral crust. Salt production, extreme dehydration hazard, disorientation. Minimal life. Distinct from Desert (which has some fauna/flora). |
| 9 | **Dunes** | Arid | Shifting sand terrain. Harder movement than flat desert, burial hazard, sand-specific resources (glass sand abundant). Distinct feel from general Desert. |
| 10 | **River** | Water | Flowing freshwater. Trade corridors, irrigation for adjacent hexes, fishing, distinct riparian fauna. Currently no flowing-water concept. |

### Tier 3: Consider (nice to have, could leave to sub-types)

| # | Biome | Category | Rationale |
|---|---|---|---|
| 11 | **Temperate Rainforest** | Wet/Temperate | Moss-draped ancient forest. Highest-quality timber, unique foraging. Distinct from Jungle (cold) and Forest (drier). Small global footprint (~2%). |
| 12 | **Estuary** | Water | River-sea junction. Brackish, nutrient-rich. High fishing, salt, trade hub. Could be handled by Coast + River adjacency instead. |
| 13 | **Reef** | Water | Shallow coral marine. Unique future-maritime resources. Currently impassable anyway — could defer. |
| 14 | **Plateau** | Arid | Flat-topped elevation. Distinct from Highlands (greener) and Badlands (eroded). Could fold into either. |

---

## Fantastical Biome Suggestions

These draw from Lootverse lore, the resource catalog (dragon burial sites, voidstone, spiritbloom, etc.), and classic fantasy worldbuilding. They should feel like natural extensions of the physics engine — unusual terrain, not breaking the rules.

### Tier 1: Strong thematic fit

| # | Biome | Category | Description | Gameplay Value |
|---|---|---|---|---|
| F1 | **Ashlands** | Extreme | Post-catastrophic volcanic devastation. Ash-covered terrain stretching far beyond active volcanos. Grey, lifeless, choking dust. Think Mordor's ashen plains or post-Pompeii landscape. Distinct from Volcanic (which is active, fiery). | Unique mining (buried minerals under ash layers), no fertility, extremely hazardous, traces of ancient civilisation beneath the ash. Links to dragon extinction lore. |
| F2 | **Petrified Forest** | Extreme | Ancient forest turned to stone over millennia. Stone trees, crystallised sap, mineral-rich terrain. Not dead — transformed. Eerie and beautiful. | Hybrid mining + forestry: "logging" yields stone-like materials, mining yields crystals trapped in petrified wood. Unique encounter flavour. Worldroot lore connection (the ancient root network, fossilised). |
| F3 | **Fungal Depths** | Extreme | Massive underground mushroom forests in cavern systems exposed at the surface. Bioluminescent, damp, alien ecology. Giant fungi replace trees. Think Underdark / Blackreach. | Unique foraging (rare mushrooms, medicinal fungi, poisonous varieties), no sunlight farming, unusual fauna (cave creatures), links to Underworld areas. |
| F4 | **Shadowmere** | Extreme | Terrain where reality thins — perpetually dim, colours muted, sound dampened. Not magical in origin but a natural phenomenon of the Eternal World's physics: areas where the world's fabric is stretched thin. | Voidstone and Twilight Quartz signature biome. High special encounter rate. Disorienting (movement penalties). Spiritbloom grows here. Links to the lore of dimensional instability. |
| F5 | **Dragon's Rest** | Extreme | Ancient dragon graveyards — vast open terrain littered with fossilised dragon remains. The land itself is infused with residual draconic energy. Bones the size of hills. | **Signature Unearthed Dragonhide biome.** Currently Dragonhide is spread across Badlands/Canyon/Desert/Volcanic — concentrating it in a dedicated biome makes the lore richer and the resource more thematic. Extremely rare (~0.5% of world). |

### Tier 2: Good thematic additions

| # | Biome | Category | Description | Gameplay Value |
|---|---|---|---|---|
| F6 | **Crystalline Waste** | Extreme | Open terrain dominated by natural crystal formations — quartz pillars, crystal caves, prismatic light. The ground crunches underfoot. | Deep Crystal and Ethereal Silica signature biome. Beautiful but dangerous (sharp terrain, disorientation from light refraction). Jeweller's paradise. |
| F7 | **Blightland** | Extreme | Magically corrupted terrain. Warped flora, toxic soil, mutated fauna. Something went wrong here long ago and it never recovered. | Poisonous Plants and Venom Sac signature. Alchemical Silver Ore. Dangerous foraging and hunting with high-value yields. Links to future alchemy module. |
| F8 | **Thornwood** | Wet/Extreme | Dense, impenetrable thorn forest. Massive briar tangles, razor-sharp vegetation, choked paths. Think Sleeping Beauty's curse made real by nature. | Extreme movement cost, high beast encounter (things hide in thorns), but rare herbs and medicinal plants thrive in the protected interior. Unique logging (thorned wood for defences). |
| F9 | **Sunken Ruins** | Water | Partially submerged ancient structures in shallow water. Columns, arches, flooded chambers. Not a player-built settlement — something from before. | High special encounter rate, unique mining (worked stone, ancient metals), links to pre-game world lore. Could be traversable in base module (shallow enough to wade). |

### Tier 3: Evocative but possibly too niche

| # | Biome | Category | Description |
|---|---|---|---|
| F10 | **Mistwood** | Temperate | Perpetually fog-shrouded forest. Visibility near zero. | Navigation hazard, ambush encounters, atmospheric. Could be a Forest sub-type. |
| F11 | **Ember Fields** | Extreme | Smouldering grassland — underground coal seams that have burned for centuries. | Think Centralia, PA or Burning Mountain, Australia. Unique coal/fuel access. |
| F12 | **Skyreach** | Extreme | Impossibly high mountain peaks where the air thins. | Could fold into Alpine. Mythic encounter territory. |

---

## Proposed Revised Biome List

If we add all Tier 1 real-world + Tier 1 fantastical, the list grows from 25 to ~35:

| # | Category | Biome | Status |
|---|---|---|---|
| 1 | Temperate | Plains | Keep |
| 2 | Temperate | Grassland | Keep |
| 3 | Temperate | Forest | Keep (generic temperate deciduous/mixed) |
| 4 | Temperate | Highlands | Keep |
| 5 | Temperate | **Woodland** | **NEW** — open canopy |
| 6 | Temperate | **Scrubland** | **NEW** — Mediterranean/chaparral |
| 7 | Temperate | **Moorland** | **NEW** — open heath |
| 8 | Arid | Desert | Keep |
| 9 | Arid | Savanna | Keep |
| 10 | Arid | Steppe | Keep |
| 11 | Arid | Badlands | Keep |
| 12 | Arid | Canyon | Keep |
| 13 | Arid | **Salt Flat** | **NEW** |
| 14 | Arid | **Dunes** | **NEW** |
| 15 | Wet | Swamp | Keep |
| 16 | Wet | **Marsh** | **RENAME** (was Wetlands) |
| 17 | Wet | Mire | Keep (bog/peatland niche) |
| 18 | Wet | Jungle | Keep |
| 19 | Wet | **Mangrove** | **NEW** — tidal coastal forest |
| 20 | Cold | Tundra | Keep |
| 21 | Cold | Taiga | Keep |
| 22 | Cold | Glacier | Keep |
| 23 | Cold | **Alpine** | **NEW** — above tree line |
| 24 | Extreme | Volcanic | Keep |
| 25 | Extreme | Oasis | Keep |
| 26 | Extreme | **Karst** | **NEW** — limestone cave terrain |
| 27 | Extreme | **Ashlands** | **NEW** ✦ fantastical |
| 28 | Extreme | **Petrified Forest** | **NEW** ✦ fantastical |
| 29 | Extreme | **Fungal Depths** | **NEW** ✦ fantastical |
| 30 | Extreme | **Shadowmere** | **NEW** ✦ fantastical |
| 31 | Extreme | **Dragon's Rest** | **NEW** ✦ fantastical |
| 32 | Water | Coast | Keep |
| 33 | Water | Beach | Keep |
| 34 | Water | Coastal Waters | Keep (impassable) |
| 35 | Water | Lake | Keep |
| 36 | Water | Ocean | Keep (impassable) |
| 37 | Water | Deep Ocean | Keep (impassable) |
| 38 | Water | **River** | **NEW** — flowing freshwater |

> **Total: 38 biomes** (25 original + 1 rename + 12 new).

### With Tier 2 additions (if we want to go bigger):

| # | Biome | Notes |
|---|---|---|
| 39 | **Crystalline Waste** | ✦ Deep Crystal / gem signature |
| 40 | **Blightland** | ✦ Corrupted terrain, alchemy link |
| 41 | **Thornwood** | ✦ Impenetrable briar |
| 42 | **Sunken Ruins** | ✦ Shallow-water archaeology |
| 43 | **Temperate Rainforest** | Ancient moss-draped forest |
| 44 | **Estuary** | River-sea junction |
| 45 | **Reef** | Coral marine (impassable without Maritime module) |

> **Maximum proposed: 45 biomes.**

---

## Biome Budget Considerations

| Count | Tradeoff |
|---|---|
| **25** (current) | Lean. Some real-world gaps. Every biome gets detailed attention. |
| **30–35** | Sweet spot. Covers all major real-world biomes + a few fantastical signature biomes. Manageable for affinity matrices and balance. |
| **38–45** | Comprehensive. Excellent coverage but the affinity matrix becomes large (45 × 87 = 3,915 cells). Phase 4 rework is substantial. |

**Recommendation**: 35–38 biomes. Covers all major real-world gaps, adds the strongest fantastical biomes, stays manageable. The u8 biome field supports 256 values, so storage is not a constraint.

---

## Summary of Changes by Priority

### Must-add (unanimous recommendation)
1. **Scrubland** — major WWF biome completely missing
2. **Alpine** — distinct from Highlands and Tundra
3. **Rename Wetlands → Marsh** — more specific, more evocative

### Should-add (strong recommendation)
4. **Woodland** — fills forest-grassland transition
5. **Mangrove** — unique coastal wetland, tropical coastline completeness
6. **Karst** — limestone terrain with natural cave access
7. **Ashlands** — fantastical post-volcanic devastation (dragon lore)
8. **Dragon's Rest** — fantastical dragon graveyard (Dragonhide signature)

### Nice-to-add (good variety)
9. **Moorland** — heath/shrubland gap
10. **Salt Flat** — extreme arid sub-type
11. **Dunes** — shifting sand, distinct from flat desert
12. **River** — flowing freshwater (trade corridors)
13. **Petrified Forest** — fantastical stone trees
14. **Fungal Depths** — fantastical underground ecology
15. **Shadowmere** — fantastical reality-thin terrain

---

*Scoping document — not yet integrated into Phase 4. Awaiting Lord Krump's review before any biome list changes.*
