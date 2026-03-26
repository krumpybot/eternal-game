# Appendix D: Biome Table

> Phase 4a of the Eternal Game Base Module Quantification Plan.
> Dependencies: Phase 1 (units/constants), Phase 2 (action catalog), Phase 3 (resource catalog).
> All values are base-module defaults. The autoregulator may adjust multipliers within bounded ranges. Game Masters may propose changes via consensus (§19 of Design Scope).

---

## Biome Categories 🔒

The Eternal World contains **27 biomes** grouped into 6 categories. Biomes are determined at world generation via `biome = noise_function(global_seed, x, y, z)` and are **permanent and immutable** — no biome ever changes after deployment.

| Category | Biomes | Count |
|---|---|---|
| **Temperate** | Plains, Grassland, Forest, Woodland, Rainforest, Highlands, Scrubland | 7 |
| **Arid** | Desert, Savanna, Steppe, Badlands, Canyon | 5 |
| **Wet** | Swamp, Marsh, Mire, Jungle, Mangrove | 5 |
| **Cold** | Tundra, Taiga, Glacier | 3 |
| **Extreme** | Scorched, Glassfields, Blight | 3 |
| **Water** | Coast, Coastal Waters, Lake, Ocean | 4 |
| | **Total** | **27** |

> **Impassable biomes**: Ocean and Coastal Waters cannot be entered in the base module. Traversal requires a future Maritime module. Coast and Lake are land-accessible.

### Lock Status Legend

| Symbol | Meaning |
|---|---|
| 🔒 | **Locked** — immutable after deployment. Cannot be changed by Game Masters or the autoregulator. |
| 🔓 | **Unlocked** — starting values only. Modifiable by Game Masters via consensus (§19 of Design Scope) and/or the autoregulator within bounded ranges. |
| ⚠️ | **Flagged for contracts specialist review** — section contains implementation-critical logic that must be validated before deployment. |

---

## 1. Biome Profiles 🔒

Each biome is described with its theme, gameplay identity, and quantified properties. All modifiers are multiplicative against the base action costs defined in Phase 2. A modifier of `1.0×` means no change from base cost.

### Column Definitions

| Column | Lock | Description |
|---|---|---|
| **Move Cost** | 🔒 | Energy multiplier for the Travel action (base: 25–45 energy). Higher = harder to cross. |
| **Move Time** | 🔒 | Time-lock multiplier for Travel (base: 72–216 ticks). Higher = slower traversal. |
| **Survey Cost** | 🔒 | Energy multiplier for the Survey action (base: 30–50 energy). Biome is already known when surveying. |
| **Survey Time** | 🔒 | Time-lock multiplier for Survey (base: 108–324 ticks). Applied after hex exploration reveals the biome. |
| **Decay Rate** | 🔓 | Multiplier on territorial decay (upkeep cost). Higher = more expensive to hold. Autoregulator-tunable within bounds. |
| **Base Hazard** | 🔓 | Base encounter chance per action (%). Modified by hex development, time of day, action type. GM-adjustable. |
| **Fertility** | 🔒 | Base soil quality for farming/foraging. 0 = infertile. |
| **Traversable** | 🔒 | Whether adventurers can enter this hex in the base module. |

---

### Temperate Biomes

#### Plains

> *Flat, open, endless horizon. The golden fields that feed nations. Wind ripples through wheat and wild grass as far as the eye can see. This is where civilisations are born — not because the land is exciting, but because it is reliable.*

The most hospitable biome. Easiest movement, lowest decay, highest farming yield among common biomes. The breadbasket of the Eternal World and the natural site for early settlements. Low hazard, but also low resource diversity — Plains reward stability over adventure.

**Gameplay identity**: Safe, fertile, settlement-friendly. The place you build your home.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 0.8× | 0.8× | 0.9× | 0.9× | 0.8× | 2% | High (1.2×) | ✓ |

#### Grassland

> *Rolling hills of thick pasture, dotted with wildflowers and grazing herds. The wind carries the scent of clover and distant rain. Not as flat as the plains — the terrain undulates, creating natural paddocks and sheltered valleys.*

Similar to Plains but with a pastoral character. Slightly higher hazard from roaming predators drawn to livestock. The premier herding biome — sheep on the hills, cattle in the valleys. Good foraging for berries, honey, and wild herbs along the hedgerows.

**Gameplay identity**: Herding heartland. Rolling pasture with better foraging than Plains.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 0.9× | 0.9× | 0.9× | 0.9× | 0.9× | 3% | High (1.2×) | ✓ |

#### Forest

> *Dense canopy of oak, elm, and beech. Shafts of sunlight pierce the leaf cover, illuminating carpets of moss and fallen leaves. The air is cool, damp, and alive with birdsong. Paths wind between ancient trunks — visibility is limited, and the undergrowth hides both treasures and teeth.*

The richest foraging biome in the game — mushrooms, berries, honey, nuts, truffles, silkworms, and herbs all thrive under the canopy. Excellent logging (the signature biome for Hartwood), strong hunting, but limited farming (shade inhibits crops). Higher hazard from ambush predators using the dense cover. Settlements are viable but constrained by the canopy.

**Gameplay identity**: Forager's paradise. Dense, biodiverse, dangerous. Best logging.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.2× | 1.2× | 1.1× | 1.2× | 1.0× | 8% | Medium (1.0×) | ✓ |

#### Woodland

> *Scattered trees with open grass between them. Dappled sunlight reaches the ground everywhere. You can see the sky — and more importantly, you can see what's coming. This is forest with breathing room: oak parkland, olive groves, clearings full of wildflowers.*

The bridge between Forest and Grassland. Open canopy means better visibility (lower hazard than Forest), some farming in clearings, decent hunting (prey is easier to spot but so are you), and moderate logging. Less foraging diversity than dense Forest, but a more balanced biome overall. Good for settlements that want some timber without sacrificing agriculture.

**Gameplay identity**: Balanced, versatile. Forest-lite with farming viability.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.0× | 1.0× | 1.0× | 1.0× | 0.9× | 5% | Medium (1.0×) | ✓ |

#### Rainforest

> *Moss-draped giants rising from a carpet of ferns. Everything is wet — the air, the bark, the ground. Ancient trees wider than houses, their branches heavy with lichen and epiphytes. The canopy is so thick that rain takes minutes to reach the forest floor. This is the oldest, wettest, most primordial temperate forest in the world.*

Distinct from both Forest (drier, deciduous) and Jungle (tropical, hot). Rainforest is cold, damp, and ancient. The signature biome for Moonwood — trees that have stood for centuries absorbing moonlight. Exceptional logging quality but slow regrowth (old-growth doesn't come back quickly). Rich foraging (rare herbs, medicinal plants thrive in the humidity). Very high movement cost from tangled undergrowth and constant wet terrain.

**Gameplay identity**: Ancient timber, rare foraging. Slow, wet, and rewarding for the patient.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.4× | 1.3× | 1.3× | 1.3× | 1.2× | 9% | Medium (1.0×) | ✓ |

#### Highlands

> *Elevated terrain of rock, heather, and hardy scrub. The air thins. Valleys cut deep between ridgelines, exposing seams of ore in cliff faces. Mountain goats navigate paths too narrow for a loaded donkey. The views are magnificent — and so is the mining.*

The premier mining biome among habitable terrain. Exposed rock faces yield iron, copper, silver, and gem precursors. Moderate farming (only hardy crops survive — tubers, barley) and decent herding (sheep thrive at altitude). Wind and elevation increase movement costs. Settlements are possible but require more upkeep.

**Gameplay identity**: Mining heartland. Elevated, exposed, mineral-rich.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.3× | 1.3× | 1.2× | 1.3× | 1.1× | 6% | Medium (1.0×) | ✓ |

#### Scrubland

> *Sun-baked earth, aromatic bushes, twisted olive trees. The air smells of thyme, rosemary, and hot stone. Cicadas drone in the midday heat. The terrain is dry but not dead — fire-adapted plants cling tenaciously to rocky soil, and bees swarm the fragrant blossoms. A landscape that rewards knowledge over brute force.*

The Mediterranean biome. Low rainfall but not barren — the plants that survive here are intensely flavourful and medicinally potent. Signature biome for Wild Herbs and aromatic foraging. Some farming possible (drought-tolerant crops, dye plants), poor logging (scrubby trees only), light mining. Fire risk is an ambient hazard. Moderate movement — the terrain is rocky but passable.

**Gameplay identity**: Herb garden of the world. Aromatic foraging, moderate farming, fire hazard.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.1× | 1.0× | 1.0× | 1.0× | 1.0× | 4% | Low (0.6×) | ✓ |

---

### Arid Biomes

#### Desert

> *An ocean of sand and stone under a merciless sun. Heat shimmers distort the horizon. Water is a memory. Life is sparse, venomous, and very good at hiding. The sand conceals mineral wealth — if you can survive long enough to dig.*

Extreme dehydration environment. No farming, minimal foraging (desert lichen, rare cacti). But the sands hide Ethereal Silica Sand (signature biome) and decent mining beneath the surface. High movement cost from heat and shifting terrain. High decay rate — the desert reclaims abandoned structures quickly. Fauna are venomous and nocturnal.

**Gameplay identity**: Harsh mineral extraction. High risk, high reward mining. Survival territory.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.5× | 1.4× | 1.4× | 1.3× | 1.5× | 6% | None (0.0×) | ✓ |

#### Savanna

> *Tall golden grass under a wide sky, punctuated by flat-topped acacias. Herds of massive fauna move in waves across the landscape — and the predators follow. This is big-game country: lions, rhinos, hyenas, and the tusked megafauna that yield ivory.*

The premier hunting biome. High fauna density, large and dangerous animals. Signature biome for Raw Hide and Ivory. Moderate farming (drought-tolerant crops), good herding (cattle territory). Some scattered trees allow limited logging. Social encounters are common — Savanna is natural trade-route territory connecting disparate settlements.

**Gameplay identity**: Big-game hunting. Open, dangerous fauna, trade corridor.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.0× | 1.0× | 1.0× | 1.0× | 1.0× | 6% | Medium (1.0×) | ✓ |

#### Steppe

> *Endless grass under a vast sky, whipped by relentless wind. No trees break the horizon. The land is flat, the soil is thin, and the winters are brutal. Nomadic riders thrive here — speed and mobility matter more than walls.*

The continental interior grassland. Drier and harsher than Grassland, with brutal winters. Best traversal speed among arid biomes (the flat terrain and lack of obstacles make for fast movement). Good herding country (horses and sheep), but limited farming. Light mining. The open terrain means encounters tend toward roaming bands (higher combat encounter rate) and nomadic traders.

**Gameplay identity**: Nomad country. Fast movement, horse-herding, harsh winters.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 0.9× | 0.9× | 1.0× | 1.0× | 1.0× | 4% | Low (0.6×) | ✓ |

#### Badlands

> *Eroded mesas and twisted rock formations under a baking sky. The wind has carved the stone into alien shapes — spires, arches, pillars. The terrain is brutal to cross, but the exposed strata reveal deep mineral veins that more hospitable biomes keep buried.*

One of the two premier deep-mining biomes (alongside Canyon). Almost entirely infertile — no farming, no foraging. The geological exposure is the point: layers of rock laid bare by erosion, rich in rare ores, gem precursors, and Starmetal Fragments. High hazard from ambush fauna (basilisks, rock drakes) that use the terrain for cover. Very hostile to settlement.

**Gameplay identity**: Exposed geology. Deep mining, extreme terrain, no life support.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.4× | 1.3× | 1.3× | 1.3× | 1.4× | 8% | None (0.0×) | ✓ |

#### Canyon

> *Sheer cliff faces plunge into shadowed depths. Narrow paths wind along ledges above drops that would kill anything that falls. Down in the gorge, water has carved through millennia of stone, exposing veins of gold, crystal, and things that should have stayed buried.*

The richest mining biome in the game. Deep vertical cuts expose the widest variety of rare and epic-tier ores — gold, deep crystal, adamantine, and more. But the terrain is punishing: highest movement cost among arid biomes, high hazard from lair-dwelling fauna (rocs, cave trolls), and a high special encounter rate (ancient things in the depths). Canyon hexes are strategic prizes — worth enormous effort to reach and hold.

**Gameplay identity**: The deepest mine. Vertical terrain, maximum mineral variety, lair encounters.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.6× | 1.5× | 1.5× | 1.5× | 1.3× | 9% | None (0.0×) | ✓ |

---

### Wet Biomes

#### Swamp

> *Dark water between gnarled cypress trees. The ground squelches and sinks. Insects cloud the air. Something large moves beneath the murky surface. The canopy filters the sun into a green twilight. It smells of rot and growth in equal measure.*

A forested wetland — trees standing in water. Rich foraging (mushrooms, medicinal herbs) and decent fishing in the murky channels. Good hunting for those brave enough (crocodiles, bog serpents — T4 fauna). Some farming possible (unique rice cultivation, tubers). Very high decay rate — the damp rots structures aggressively. No herding (livestock would drown).

**Gameplay identity**: Treacherous wetland. Rich foraging, dangerous hunting, punishing decay.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.5× | 1.5× | 1.3× | 1.4× | 1.6× | 13% | Low (0.6×) | ✓ |

#### Marsh

> *Open water threaded between reed beds and mudflats. Wading birds stalk the shallows. The horizon is flat and hazy. Firm ground is rare — paths must be learned, not assumed. But the water is rich: fish, reeds, wild rice, and waterfowl.*

A freshwater emergent wetland — reeds and grasses, no trees. More productive for farming than Swamp (rice, tubers, vegetables thrive). Fishing in the channels. Good foraging (reeds, herbs, wild berries). Lower hazard than Swamp (fewer ambush predators in open water). Moderate decay. Marginal settlement potential — flood-prone but usable.

**Gameplay identity**: Productive wetland. Reed-beds, rice paddies, wading birds. Swamp's friendlier cousin.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.3× | 1.3× | 1.2× | 1.2× | 1.4× | 7% | Medium (1.0×) | ✓ |

#### Mire

> *The ground is not ground. It is a skin of moss and peat over bottomless black water. Step wrong and you sink. Poisonous flowers bloom in vivid colours. Will-o-wisps flicker at the edge of vision. No traveller enters the mire by choice — and some never leave.*

The most hostile wetland. Covers bog, peatland, and fen terrain. Highest movement cost and decay rate among wet biomes. Zero social encounters — nobody comes here willingly. Signature biome for Poisonous Plants. Very high beast encounter rate (mire beasts, bog horrors — T4 fauna). Minimal farming, poor foraging outside of toxic plants. The mire is a place you enter for specific, high-value targets — Alchemical Silver, Voidstone, and future alchemy reagents.

**Gameplay identity**: Toxic wasteland. Poisonous foraging, dark creatures, avoid unless you need what's here.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.7× | 1.6× | 1.5× | 1.5× | 1.8× | 15% | None (0.0×) | ✓ |

#### Jungle

> *A wall of green from ground to canopy. Vines thick as arms hang from trees that disappear into the clouds. Parrots scream. Snakes coil in the branches. Every surface is alive — covered in moss, lichen, insects, orchids. The air is thick enough to drink. This is the tropics in full, unrestrained fury.*

The tropical rainforest. Second-richest foraging biome after Forest (rare herbs, silkworms — signature biome, exotic fruit, spiritbloom). Best tropical farming (sugar cane, spice plants, raw cotton — all signature). Strong hunting (Exotic Pelt signature — jungle cats, serpents). Dense canopy creates high hazard (ambush + poison, T4 fauna). High fertility but difficult to develop — the jungle fights back.

**Gameplay identity**: Tropical abundance. Silkworms, spices, exotic pelts. Rich but dangerous.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.4× | 1.4× | 1.3× | 1.4× | 1.3× | 11% | High (1.2×) | ✓ |

#### Mangrove

> *Prop-rooted trees standing in saltwater, their tangled roots forming a labyrinth above and below the waterline. Crabs scuttle across exposed mud. Fish dart between submerged roots. The air is salt and decay. This is where land meets sea — neither fully one nor the other.*

The tidal coastal forest. A unique hybrid: fishing access (sheltered waters between roots), foraging (seaweed, medicinal herbs), and some logging (mangrove wood — dense, salt-resistant, excellent for waterproofing). No farming (saltwater soil), no herding, no mining. High hazard from semi-aquatic predators. Very high decay rate — saltwater is corrosive. Settlements are impractical but the fishing and coastal access are valuable.

**Gameplay identity**: Tidal borderland. Fishing, salt-wood logging, coastal access. Impractical to settle.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.5× | 1.4× | 1.3× | 1.3× | 1.7× | 8% | None (0.0×) | ✓ |

---

### Cold Biomes

#### Tundra

> *Frozen earth stretching to a colourless horizon. The wind never stops. Hardy lichen and dwarf shrubs cling to ground that thaws for only weeks each year. Permafrost locks ancient things beneath the surface. Dire wolves and snow bears patrol in packs, their white fur invisible against the snow until they're too close.*

Treeless, flat, frozen. No farming, minimal foraging (lichen, hardy roots). Good mining — the frozen ground preserves mineral veins and ice-locked deposits. Prime fur-pelt territory (Tundra signature alongside Taiga). Moderate movement cost — the ground is hard and flat but the cold saps energy. True Ice Shards appear as primary deposits. Starkly beautiful and deeply inhospitable.

**Gameplay identity**: Frozen mining frontier. Fur pelts, True Ice, dire fauna.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.3× | 1.3× | 1.2× | 1.3× | 1.2× | 5% | None (0.0×) | ✓ |

#### Taiga

> *Endless coniferous forest under a steel-grey sky. Pine, spruce, and fir as far as the eye can see. The air smells of resin. Snow sits heavy on boughs in winter; in summer, mosquitoes rule. Bears fish in cold streams. Wolves howl at dusk.*

The boreal forest. Signature biome for Resin. Excellent logging (Ironwood primary, dense coniferous timber). Good hunting (bears, wolves, moose — T3 fauna, prime Fur Pelt territory). Moderate foraging (mushrooms, berries, pine nuts). Minimal farming (tubers only). Lower hazard than tropical forests — the fauna is dangerous but not ambush-oriented. Cold but habitable — taiga settlements are harsh but functional.

**Gameplay identity**: Northern timber country. Resin, ironwood, fur pelts, cold but workable.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.2× | 1.2× | 1.1× | 1.2× | 1.1× | 6% | Low (0.6×) | ✓ |

#### Glacier

> *A world of blue-white ice. Crevasses split the surface without warning. The silence is absolute — broken only by the distant crack of calving ice. Nothing grows. Nothing should live here. But something does, deep in the ice: ancient things, frozen since before the age of men, waiting.*

The most extreme traversable biome. Highest movement cost, highest decay rate (ice shifts and crushes structures). No farming, no foraging, no logging. Mining is the only productive activity — and it's extraordinary: True Ice Shard (signature biome), Mithral Ore (primary), Rough Sapphire, Rough Twilight Quartz, Rough Deep Crystal. The fauna tier is T5 — mythic creatures (ice wyrms, frost giants). Glacier hexes have the highest special encounter rate in the game (60%) — the ice holds secrets.

**Gameplay identity**: Mythic endgame territory. T5 fauna, legendary minerals, extreme survival.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.8× | 1.7× | 1.6× | 1.6× | 2.0× | 13% | None (0.0×) | ✓ |

---

### Extreme Biomes

#### Scorched

> *The earth is cracked and black. Fissures glow orange. Lava pools bubble in hollows. The air shimmers with heat and reeks of sulfur. Ash drifts like grey snow. This is land that burned — some of it still burning, some of it ancient devastation from the cataclysm that destroyed the dragons. Forges of nature, where the hottest fires meet the hardest stone.*

Covers both active volcanic terrain and the vast ash-covered wastelands that surround them (the Eternum "Scorched" biome). Signature biome for Obsidian, Sulfur, and Ignium Ore — the fire-aspected minerals. Also primary for Unearthed Dragonhide (the cataclysm that scorched this land also killed the dragons; their remains fossilise here). Raw Demonhide signature — demons thrive in brimstone. T5 fauna (fire elementals, lava serpents). No farming, no foraging, minimal logging. Pure extraction territory.

**Gameplay identity**: Fire and ash. Ignium, obsidian, dragonhide, demons. The forge of the world.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.6× | 1.5× | 1.5× | 1.5× | 1.7× | 17% | None (0.0×) | ✓ |

#### Glassfields

> *Pillars of crystal erupt from fractured earth. Prismatic light refracts in every direction — beautiful and disorienting. The ground crunches underfoot, sharp enough to cut through boot leather. In places, the crystal formations are taller than trees, humming faintly with resonant energy. Jewellers speak of the Glassfields the way miners speak of gold veins.*

A fantastical biome: vast natural crystal formations covering the terrain. Signature biome for Rough Deep Crystal and Ethereal Silica Sand. Primary for gems (Rough Diamond, Rough Sapphire, Rough Ruby). The crystal terrain is treacherous — sharp surfaces, disorienting light refraction, and resonant vibrations that attract unusual fauna. High special encounter rate (crystalline anomalies, light-based hazards). No farming, no logging, no foraging. Pure mining paradise.

**Gameplay identity**: Crystal mining paradise. Gems, deep crystal, silica. Beautiful and dangerous.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.5× | 1.4× | 1.4× | 1.4× | 1.5× | 15% | None (0.0×) | ✓ |

#### Blight

> *Something is wrong here. The trees are twisted into unnatural shapes. The soil is black and oily. Flowers bloom in colours that don't exist elsewhere — vivid, pulsing, wrong. Animals are malformed — extra limbs, too many eyes, impossible anatomies. The air tastes metallic. Whatever happened to this land, it happened long ago, and the corruption has had time to settle in and make itself at home.*

Magically corrupted terrain. The source of the corruption is ancient and unknown — perhaps a failed magical experiment, a cursed battlefield, or a tear in the world's fabric. Signature biome for Poisonous Plants (alongside Mire) and Venom Sac. Primary for Alchemical Silver Ore (the anti-magical metal forms in reaction to magical corruption). Spiritbloom grows here — feeding on residual magical energy. Future alchemy module tie-in: Blight is expected to be the primary source of alchemical reagents. High hazard from mutated fauna (T4). Moderate foraging (toxic but valuable plants), some distorted logging, minimal mining.

**Gameplay identity**: Corrupted wilderness. Alchemy reagents, venom, alchemical silver. Toxic but invaluable for future magic systems.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.4× | 1.3× | 1.3× | 1.3× | 1.5× | 20% | None (0.0×) | ✓ |

---

### Water Biomes

#### Coast

> *Where land meets sea. Rocky headlands, sandy coves, tidal pools, driftwood-strewn shores. Seabirds wheel overhead. The salt wind carries the sound of waves. Fishermen mend nets on the strand while traders scan the horizon for approaching sails. The coast is the gateway to the ocean — and for now, the ocean's edge is as far as anyone can go.*

Encompasses all coastal terrain — rocky shores, sandy beaches, harbour coves, cliff edges. The premier fishing biome. Good foraging (seaweed signature, wild herbs, shellfish potential for future Maritime module). Some farming (salt-tolerant crops), light mining. The social encounter rate reflects coastal life — traders, travellers, shipwrecked strangers. Moderate hazard from seabirds, crabs, and the occasional sea serpent.

**Gameplay identity**: Maritime gateway. Fishing, trade, coastal foraging. The edge of the known world.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.1× | 1.1× | 1.0× | 1.0× | 1.1× | 4% | Low (0.6×) | ✓ |

#### Lake

> *Still water reflecting sky. Reeds fringe the shallows. Fish jump at dawn. The lakeshore is a natural gathering point — freshwater, fishing, and a defensible position with water on at least one side.*

Freshwater body. Fishing signature (alongside Coast). Moderate lakeside farming (fertile shore areas support a range of crops), moderate foraging (reeds, wild herbs). Fishing is the primary draw. Lower hazard than Coast (freshwater fauna is milder). Good settlement potential — lakeside towns have water access, fishing, and natural defence.

**Gameplay identity**: Freshwater fishing. Lakeside settlement, natural defence, calm.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| 1.2× | 1.2× | 1.1× | 1.1× | 1.0× | 3% | Medium (1.0×) | ✓ |

#### Coastal Waters

> *Shallow sea over sand and rock. Visible from shore but unreachable on foot. Kelp forests sway in the current. Fish school in vast numbers. The potential is obvious — but reaching it requires more than legs.*

Impassable in the base module. Exists on the map as a visible barrier. Future Maritime module will enable traversal, fishing, and resource extraction (coral, pearls, deep-sea resources).

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| — | — | — | — | — | — | None | ✗ |

#### Ocean

> *Open water to the horizon. Deep blue, vast, featureless. Currents move beneath the surface. Occasionally something breaches — something very large. The ocean is the world's greatest unexplored frontier, and it will remain so until someone builds a ship.*

Impassable in the base module. Covers all open water — shallow seas, deep ocean, and everything between. The ocean separates landmasses and creates the geography that makes coastal territory valuable.

| Move Cost | Move Time | Survey Cost | Survey Time | Decay Rate | Base Hazard | Fertility | Traversable |
|---|---|---|---|---|---|---|---|
| — | — | — | — | — | — | None | ✗ |

---

## 2. Encounter Type Distribution 🔓

Each biome has a weighted distribution of encounter types when an encounter triggers. Weights sum to 100%.

| Biome | Beast | Combat | Social | Special |
|---|---|---|---|---|
| **Plains** | 40% | 20% | 30% | 10% |
| **Grassland** | 45% | 15% | 30% | 10% |
| **Forest** | 55% | 15% | 15% | 15% |
| **Woodland** | 45% | 15% | 25% | 15% |
| **Rainforest** | 50% | 10% | 15% | 25% |
| **Highlands** | 50% | 20% | 15% | 15% |
| **Scrubland** | 35% | 20% | 30% | 15% |
| **Desert** | 45% | 25% | 15% | 15% |
| **Savanna** | 55% | 15% | 20% | 10% |
| **Steppe** | 40% | 25% | 25% | 10% |
| **Badlands** | 50% | 30% | 5% | 15% |
| **Canyon** | 45% | 25% | 10% | 20% |
| **Swamp** | 60% | 15% | 5% | 20% |
| **Marsh** | 50% | 10% | 20% | 20% |
| **Mire** | 65% | 15% | 0% | 20% |
| **Jungle** | 60% | 10% | 10% | 20% |
| **Mangrove** | 50% | 15% | 15% | 20% |
| **Tundra** | 35% | 20% | 20% | 25% |
| **Taiga** | 50% | 15% | 15% | 20% |
| **Glacier** | 25% | 10% | 5% | 60% |
| **Scorched** | 40% | 15% | 5% | 40% |
| **Glassfields** | 30% | 10% | 5% | 55% |
| **Blight** | 55% | 20% | 0% | 25% |
| **Coast** | 30% | 15% | 40% | 15% |
| **Lake** | 40% | 10% | 30% | 20% |

> **Encounter type definitions** (from Appendix M):
> - **Beast**: Wildlife attack. Scales with biome fauna tier and adventurer position.
> - **Combat**: Hostile NPC or environmental combat. Ambushes, bandits, territorial fauna.
> - **Social**: Non-violent encounters. Traders, travellers, lost adventurers, mysterious strangers.
> - **Special**: Rare events. Discoveries, anomalies, lore reveals, unique one-time occurrences.

### Design Notes

- **Mire** and **Blight** have 0% social encounters — nobody enters these places willingly; there are no travellers to meet.
- **Glacier**, **Scorched**, and **Glassfields** have very high special encounter rates — extreme environments hold ancient mysteries and anomalies.
- **Coast** has the highest social encounter rate — natural meeting point, trade hub, travellers arriving by shore.
- **Scrubland** has relatively high social encounters — Mediterranean crossroads, trade routes between regions.
- **Rainforest** has high special rate (25%) — ancient forests hold secrets in their depths.
- Higher hazard biomes skew toward beast encounters. Civilised biomes (Plains, Grassland, Coast) skew toward social.

---

## 3. Fauna Tier by Biome 🔓

Defines the maximum beast tier that can appear in each biome. Beast tiers range from T1 (common nuisance) to T5 (mythic apex predator). Higher tiers yield better beast parts (see Appendix F §4: Beast Parts).

| Biome | Max Beast Tier | Typical Fauna | Notes |
|---|---|---|---|
| **Plains** | T2 | Wolves, boar, wild dogs | Open terrain, herding beasts |
| **Grassland** | T2 | Prairie cats, bison, snakes | Open, mild threat |
| **Forest** | T3 | Bears, wolves, great stags, spiders | Dense cover, ambush predators |
| **Woodland** | T2 | Foxes, deer, boar, hawks | Open canopy reduces ambush danger |
| **Rainforest** | T3 | Giant spiders, serpents, forest apes | Ancient forest, deep canopy, damp ambush |
| **Highlands** | T3 | Mountain cats, eagles, rams, wyverns | Elevation advantage for fauna |
| **Scrubland** | T2 | Vipers, wild cats, hawks, scorpions | Small but venomous |
| **Desert** | T3 | Scorpions, sandworms, vipers | Dehydration compounds danger |
| **Savanna** | T3 | Lions, hyenas, great cats, rhinos | Pack and apex predators |
| **Steppe** | T2 | Wolves, wild horses, hawks | Open, less threatening |
| **Badlands** | T3 | Basilisks, rock drakes, scorpions | Ambush terrain, venomous |
| **Canyon** | T4 | Rocs, basilisks, cave trolls | Vertical danger, lair terrain |
| **Swamp** | T4 | Hydras, bog serpents, crocodiles, leeches | Treacherous, poisonous |
| **Marsh** | T3 | Crocodiles, snapping turtles, herons | Semi-aquatic threats |
| **Mire** | T4 | Mire beasts, will-o-wisps, bog horrors | The worst wetland has to offer |
| **Jungle** | T4 | Jungle cats, serpents, apes, spiders | Dense canopy, ambush + poison |
| **Mangrove** | T3 | Crocodiles, sea snakes, giant crabs | Semi-aquatic ambush predators |
| **Tundra** | T3 | Dire wolves, snow bears, mammoths | Cold-adapted apex predators |
| **Taiga** | T3 | Bears, wolves, moose, lynx | Northern forest predators |
| **Glacier** | T5 | Ice wyrms, frost giants, ancient things | Mythic creatures survive here |
| **Scorched** | T5 | Fire elementals, lava serpents, demons | Extreme fauna for extreme terrain |
| **Glassfields** | T4 | Crystal golems, refraction beasts, prismatic serpents | Resonance-adapted creatures |
| **Blight** | T4 | Mutated fauna, blighted wolves, corruption spawns | Twisted, unpredictable |
| **Coast** | T2 | Seabirds (aggressive), crabs, sea serpents | Mild coastal fauna |
| **Lake** | T2 | Lake serpents, snapping turtles | Mild freshwater fauna |

> **Beast tier → Beast part quality**: T1→Common parts, T2→Uncommon, T3→Rare, T4→Epic, T5→Legendary. Mythic beast parts require Mythic beasts which are encounter-specific (not biome-spawned) and may be introduced by Game Masters.

---

## 4. Biome Clustering & Generation 🔒 ⚠️

> ⚠️ **Contracts specialist review required.** This section defines the on-chain noise functions, seed derivation, domain separation, and deterministic biome assignment logic. All algorithms must be validated for gas efficiency, determinism, and collision resistance before deployment. Particular attention needed for the Anomaly layer override logic and the Mangrove post-pass rule.

Biome placement uses **simplex noise** with domain-separated layers to produce natural terrain transitions. This section defines the generation rules.

### 4.1 Noise Layers

| Layer | Seed | Purpose | Scale |
|---|---|---|---|
| **Temperature** | `hash(global_seed, "TEMP_V1", x, y, z)` | Hot ↔ Cold gradient | Large (64–128 hex wavelength) |
| **Moisture** | `hash(global_seed, "MOIST_V1", x, y, z)` | Wet ↔ Dry gradient | Large (64–128 hex wavelength) |
| **Elevation** | `hash(global_seed, "ELEV_V1", x, y, z)` | Sea level ↔ Mountain | Medium (32–64 hex wavelength) |
| **Variation** | `hash(global_seed, "VAR_V1", x, y, z)` | Local microclimate/terrain variation | Small (8–16 hex wavelength) |
| **Anomaly** | `hash(global_seed, "ANOM_V1", x, y, z)` | Extreme biome placement | Very small (4–8 hex wavelength), sparse |

> **Anomaly layer** is new — it governs the placement of Extreme biomes (Scorched, Glassfields, Blight). These biomes don't follow standard temperature/moisture logic; they are geological or magical anomalies that can appear in any climate zone at low frequency.

### 4.2 Biome Determination

The noise values map to a biome via a **lookup table**. Each biome occupies a region in the noise space. Boundaries are defined to produce coherent clustering:

```
biome = biome_lookup(temperature, moisture, elevation, variation, anomaly)
```

**Standard biomes** (determined by temperature, moisture, elevation):

| Temperature | Moisture | Elevation | Biome | Variation Override |
|---|---|---|---|---|
| Hot | Dry | Low | Desert | — |
| Hot | Dry | Medium | Badlands | Canyon (high variation) |
| Hot | Dry | High | Canyon | — |
| Hot | Medium | Low | Savanna | — |
| Hot | Medium | Medium | Scrubland | Steppe (low moisture edge) |
| Hot | Wet | Low | Jungle | Mangrove (coastal adjacency) |
| Hot | Wet | Medium | Swamp | Mire (high variation) |
| Temperate | Dry | Low | Plains | — |
| Temperate | Dry | Medium | Grassland | Steppe (low moisture edge) |
| Temperate | Medium | Low | Woodland | — |
| Temperate | Medium | Medium | Forest | Rainforest (high moisture) |
| Temperate | Medium | High | Highlands | — |
| Temperate | Wet | Low | Marsh | — |
| Temperate | Wet | Medium | Rainforest | Forest (low moisture edge) |
| Cold | Dry | Low | Tundra | — |
| Cold | Dry | Medium | Steppe | Tundra (low variation) |
| Cold | Medium | Low | Taiga | — |
| Cold | Medium | Medium | Taiga | — |
| Cold | Wet | Low | Marsh | Mire (high variation) |
| Cold | Any | High | Glacier | — |
| Any | Any | Sea level | Coast / Lake | Determined by water body size |
| Any | Any | Below sea level | Coastal Waters / Ocean | Depth determines sub-type |

**Extreme biomes** (anomaly layer override — checked before standard lookup):

| Anomaly Value | Result | Placement Rule |
|---|---|---|
| > 0.95 (fire) | **Scorched** | Clusters in 2–5 hex patches. More common in hot/dry regions but can appear anywhere. |
| > 0.95 (crystal) | **Glassfields** | Clusters in 1–3 hex patches. No climate preference — geological anomaly. |
| > 0.95 (corruption) | **Blight** | Clusters in 2–4 hex patches. More common in wet regions but can appear anywhere. |

> The anomaly layer uses a separate noise channel with domain separation for each extreme type (`ANOM_FIRE_V1`, `ANOM_CRYSTAL_V1`, `ANOM_CORRUPT_V1`). Extreme biomes override the standard biome determination when their threshold is met. They cluster naturally due to noise coherence.

**Mangrove placement** is a post-pass rule: any Jungle or Swamp hex adjacent to Coast or Coastal Waters has a 40% chance of being converted to Mangrove. This creates natural tropical coastline transitions.

### 4.3 Water Body Classification

Water biomes are determined by elevation and extent:

| Condition | Biome |
|---|---|
| Sea-level hex with ≤2 adjacent sea-level hexes | **Lake** |
| Sea-level hex with 3+ adjacent sea-level hexes, ≤1 hex from land | **Coast** |
| Below sea level, adjacent to Coast | **Coastal Waters** |
| Below sea level, not adjacent to Coast | **Ocean** |

### 4.4 Distribution Targets

The noise function parameters are tuned to produce approximately these global biome frequencies. Actual distribution will vary by region — the world is not uniform.

| Category | Target % | Notes |
|---|---|---|
| **Temperate** | ~35% | The most common and habitable terrain |
| **Arid** | ~17% | Large deserts with scattered scrub |
| **Wet** | ~13% | Clustered, harder to develop |
| **Cold** | ~9% | Northern/polar regions |
| **Extreme** | ~3% | Rare anomalies scattered across the world |
| **Water** | ~23% | Oceans, coasts, lakes |
| | **100%** | |

Within categories, approximate internal distributions:

| Biome | Approx % | Biome | Approx % |
|---|---|---|---|
| Plains | 8% | Tundra | 4% |
| Grassland | 7% | Taiga | 3% |
| Forest | 6% | Glacier | 2% |
| Woodland | 5% | Scorched | 1.5% |
| Rainforest | 3% | Glassfields | 0.5% |
| Highlands | 4% | Blight | 1% |
| Scrubland | 2% | Coast | 6% |
| Desert | 6% | Coastal Waters | 4% |
| Savanna | 4% | Lake | 3% |
| Steppe | 3% | Ocean | 10% |
| Badlands | 2% | | |
| Canyon | 2% | | |
| Swamp | 3% | | |
| Marsh | 3% | | |
| Mire | 2% | | |
| Jungle | 3% | | |
| Mangrove | 2% | | |

> These are **tuning targets**, not hard constraints. The noise function is calibrated to approach these percentages across large sample areas (>10,000 hexes). Local regions will deviate naturally — a player exploring northward will encounter predominantly cold biomes; equatorial exploration will be hot and wet. Extreme biomes are rare everywhere but can appear in any region.

### 4.5 Nexus Region

The origin hex `(0,0,0)` — The Nexus — is forced to **Plains** biome regardless of noise output. The surrounding ring (6 adjacent hexes) is biased toward temperate biomes (Plains, Grassland, Woodland, Forest) to ensure new players have a hospitable starting region. Extreme biomes (Scorched, Glassfields, Blight) cannot generate within 3 hexes of the Nexus.

Beyond the first ring, the noise function operates normally with no overrides (except the Nexus exclusion zone for extreme biomes).

---

## 5. Fertility Ratings 🔒

Fertility determines base farming and foraging yields. Each biome has a fertility classification that translates to a numerical modifier.

| Rating | Modifier | Meaning |
|---|---|---|
| **High** | 1.2× | Good soil, adequate water |
| **Medium** | 1.0× | Baseline — functional but not special |
| **Low** | 0.6× | Poor soil or harsh conditions; limited crops viable |
| **None** | 0.0× | Cannot farm. Foraging yields minimal wild plants only (mushrooms, lichen). |

### Fertility by Biome

| Biome | Fertility | Farmable Crops | Foraging Quality |
|---|---|---|---|
| **Plains** | High (1.2×) | All grain, vegetables, flax, hemp | Moderate — wild herbs |
| **Grassland** | High (1.2×) | All grain, vegetables, hemp | Good — wild herbs, berries, honey |
| **Forest** | Medium (1.0×) | Limited (shade crops) | Excellent — mushrooms, herbs, nuts, berries, truffles, silkworms |
| **Woodland** | Medium (1.0×) | Moderate (clearings allow most crops) | Good — herbs, berries, nuts, honey |
| **Rainforest** | Medium (1.0×) | Limited (canopy shade, wet soil) | Excellent — rare herbs, medicinal herbs, mushrooms |
| **Highlands** | Medium (1.0×) | Hardy crops (tubers, barley) | Moderate — herbs, roots |
| **Scrubland** | Low (0.6×) | Drought-tolerant (barley, dye plants) | Good — aromatic herbs (signature), honey |
| **Desert** | None (0.0×) | None | Minimal — desert lichen, rare cacti |
| **Savanna** | Medium (1.0×) | Drought-tolerant (beans, sorghum) | Moderate — wild grains, roots |
| **Steppe** | Low (0.6×) | Hardy grasses, barley | Sparse — roots, wild herbs |
| **Badlands** | None (0.0×) | None | Minimal — hardy scrub only |
| **Canyon** | None (0.0×) | None | Minimal — canyon ferns, lichen |
| **Swamp** | Low (0.6×) | Rice (unique), tubers | Good — mushrooms, herbs, medicinal herbs |
| **Marsh** | Medium (1.0×) | Rice, tubers, vegetables | Good — reeds, herbs, wild berries |
| **Mire** | None (0.0×) | None | Poor — poisonous plants dominant |
| **Jungle** | High (1.2×) | Tropical (sugar cane, spice plants, fruit) | Excellent — wild fruit, herbs, rare herbs, silkworms, spiritbloom |
| **Mangrove** | None (0.0×) | None (saltwater soil) | Moderate — seaweed, medicinal herbs in root systems |
| **Tundra** | None (0.0×) | None | Sparse — lichen, hardy roots |
| **Taiga** | Low (0.6×) | Very limited (tubers only) | Moderate — mushrooms, berries, pine nuts |
| **Glacier** | None (0.0×) | None | None |
| **Scorched** | None (0.0×) | None | Minimal — heat-tolerant fungi |
| **Glassfields** | None (0.0×) | None | None — crystal terrain, no organic growth |
| **Blight** | None (0.0×) | None (corrupted soil) | Moderate — poisonous plants, rare herbs, spiritbloom |
| **Coast** | Low (0.6×) | Salt-tolerant crops (limited) | Good — seaweed (signature), wild herbs |
| **Lake** | Medium (1.0×) | Lakeside crops | Moderate — reeds, wild herbs |

---

## 6. Biome Suitability Summaries 🔓

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
| **Woodland** | ★☆☆ | ★★☆ | ★★☆ | ★★☆ | ★★☆ | ☆☆☆ | ★☆☆ | ★★★ |
| **Rainforest** | ★☆☆ | ★★★ | ★☆☆ | ★★★ | ★★☆ | ☆☆☆ | ☆☆☆ | ★☆☆ |
| **Highlands** | ★★★ | ★☆☆ | ★★☆ | ★★☆ | ★★☆ | ☆☆☆ | ★★☆ | ★★☆ |
| **Scrubland** | ★☆☆ | ★☆☆ | ★☆☆ | ★★★ | ★☆☆ | ☆☆☆ | ★☆☆ | ★★☆ |
| **Desert** | ★★☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ | ★☆☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ |
| **Savanna** | ★☆☆ | ★☆☆ | ★★☆ | ★★☆ | ★★★ | ☆☆☆ | ★★☆ | ★★☆ |
| **Steppe** | ★☆☆ | ☆☆☆ | ★☆☆ | ★☆☆ | ★★☆ | ☆☆☆ | ★★☆ | ★☆☆ |
| **Badlands** | ★★★ | ☆☆☆ | ☆☆☆ | ☆☆☆ | ★☆☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ |
| **Canyon** | ★★★ | ☆☆☆ | ☆☆☆ | ☆☆☆ | ★★☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ |
| **Swamp** | ★☆☆ | ★☆☆ | ★☆☆ | ★★★ | ★★☆ | ★★☆ | ☆☆☆ | ☆☆☆ |
| **Marsh** | ★☆☆ | ☆☆☆ | ★★☆ | ★★☆ | ★★☆ | ★★☆ | ☆☆☆ | ★☆☆ |
| **Mire** | ★☆☆ | ☆☆☆ | ☆☆☆ | ★☆☆ | ★★☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ |
| **Jungle** | ★☆☆ | ★★☆ | ★★☆ | ★★★ | ★★★ | ☆☆☆ | ☆☆☆ | ★☆☆ |
| **Mangrove** | ☆☆☆ | ★☆☆ | ☆☆☆ | ★★☆ | ★★☆ | ★★★ | ☆☆☆ | ☆☆☆ |
| **Tundra** | ★★☆ | ☆☆☆ | ☆☆☆ | ★☆☆ | ★★☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ |
| **Taiga** | ★★☆ | ★★★ | ☆☆☆ | ★★☆ | ★★☆ | ☆☆☆ | ☆☆☆ | ★☆☆ |
| **Glacier** | ★★☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ | ★☆☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ |
| **Scorched** | ★★★ | ☆☆☆ | ☆☆☆ | ☆☆☆ | ★☆☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ |
| **Glassfields** | ★★★ | ☆☆☆ | ☆☆☆ | ☆☆☆ | ★☆☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ |
| **Blight** | ★☆☆ | ★☆☆ | ☆☆☆ | ★★☆ | ★★☆ | ☆☆☆ | ☆☆☆ | ☆☆☆ |
| **Coast** | ★☆☆ | ☆☆☆ | ★★☆ | ★★☆ | ★☆☆ | ★★★ | ☆☆☆ | ★★☆ |
| **Lake** | ★☆☆ | ☆☆☆ | ★★☆ | ★★☆ | ★☆☆ | ★★★ | ☆☆☆ | ★★☆ |

### Design Notes

- **Every biome has at least one ★★☆+ activity** — no biome is completely useless. Even Glacier has ★★☆ mining and ★☆☆ hunting.
- **No two biomes share the same suitability profile** — each has a unique gameplay fingerprint.
- **Woodland** is the most balanced biome — ★★☆ in five activities. Jack of all trades, master of none.
- **Scrubland** is the foraging specialist among habitable biomes — ★★★ foraging with ★★☆ settlement potential.
- **Mangrove** is the only non-water biome with ★★★ fishing — its unique tidal ecology makes it the fishing specialist outside Coast/Lake.
- **Blight** has moderate foraging (★★☆) despite None fertility — toxic plants are valuable, not edible.
- **Glassfields** is the mining-only biome — ★★★ mining with nothing else above ★☆☆.

---

*Phase 4a of the Eternal Game Base Module Quantification Plan.*
*v0.2.0 — March 2026*
