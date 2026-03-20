# Eternal Game — Trait Table (Draft v0.5)

> **Review draft.** All traits listed with name, type, group (exclusivity), attribute modifier, and special effect.
> Inspired by CK3's trait system — personality pairs, congenital physical traits, lifestyle skills, and injury/disability progression.
>
> **Attribute modifier shapes**: `[+1]`, `[−1]`, `[+2]`, `[−2]`, `[+1, +1]`, `[+1, −1]`, `[−1, −1]`.
> Dual-modifier shapes (`[+1, +1]`, `[+1, −1]`, `[−1, −1]`) are rarer than single-modifier traits.
>
> **No-doubling rule**: attribute modifier must not amplify the same stat as the special effect.
> **Some traits have only a modifier or only a special effect** — not every trait needs both.

---

## Progression System: Chance-Based (No XP Tracking)

All attribute and trait progression uses **per-action chance rolls** — no experience points, no historical data to track on-chain. This keeps the contract system simple: each action resolution includes a small chance calculation for attribute gain and trait gain.

### Attribute Gain

- Every action has a **small base chance** to increase a relevant attribute (e.g., successfully mining has a ~1–2% chance to increase STR or INT).
- The chance decreases **exponentially** with the current attribute level:
  - `gain_chance = base_chance × (1 − (level / 20))²`
  - At level 0: 100% of base chance
  - At level 5: 56% of base chance
  - At level 10: 25% of base chance
  - At level 15: 6.25% of base chance
  - At level 19: 0.25% of base chance
  - At level 20: **0%** (hard cap)
- **Attributes can never be lost.** They can only be **suppressed** by trait modifiers (e.g., a −1 STR trait reduces effective STR but does not remove the underlying point).
- Multiple attributes may have a chance to increase from a single action (e.g., travel may roll for both END and WIS).

### Trait Gain

- Every action or event has a **small base chance** to grant a trait.
- The **criticality of the event outcome** determines the chance:
  - Routine success (e.g., normal harvest): ~0.5% chance
  - Notable success (e.g., rare vein discovery): ~3% chance
  - Critical event (e.g., surviving a T3+ beast): ~8–15% chance
  - Severe failure (e.g., near-death): ~10–20% chance for negative traits
- Trait gain chance decreases **exponentially** with current trait count (matching the attribute gain curve):
  - `gain_chance = base_chance × (1 − (trait_count / 10))²`
  - At 0 traits: 100% of base chance
  - At 3 traits: 49% of base chance
  - At 5 traits: 25%
  - At 7 traits: 9%
  - At 9 traits: 1%
  - At 10 traits: **0%** (hard cap)
- Personality traits can be gained or replaced through **major life events** (very rare — surviving permadeath-level encounters, first settlement, etc.).
- Skill traits are gained through actions — each success/failure has a small chance.
- Injury traits are gained from damage events; severity scales with damage dealt.

### Trait Effects on Progression

Some traits modify the **chance of attribute or trait gain** rather than (or in addition to) providing direct mechanical bonuses. These use additive modifiers on top of the already-small base chances:
- "+25% attribute gain chance from crafting" means: if base chance is 1%, effective chance becomes 1.25%.
- "+50% trait gain chance from encounters" means: if base chance is 5%, effective chance becomes 7.5%.

---

## Trait Types

| Type | Slots at mint | Can be lost? | Notes |
|---|---|---|---|
| Personality | 2 | Replaced by opposite through major events | Almost all have an opposite |
| Physical | 1 | Replaced by opposite through major events | Almost all have an opposite |
| Skill | 0 | **Never** (permanent once gained) | Positive from success, negative from failure; harder to gain as count rises. **Once a skill trait is learned, its opposite (if any) can never be gained** — skill traits are permanent, so learning one locks out the other. |
| Injury | 0 | Yes — return to >75% of maximum health + complete any action | May convert to disability trait |
| Disability | 0 | **Never** (permanent) | Converted from severe injuries |

**Max traits**: 10. Gain chance decreases exponentially to 0 at max.

---

## 1. Personality Traits

Personality traits define an adventurer's disposition. Almost all come in exclusive pairs. Gained at mint (2 traits); very rarely gained/replaced through major life events (chance-based — e.g., surviving a near-death encounter may have a small chance to grant Brave or replace Craven).

Personality traits can affect **all attributes**, but are slightly more heavily weighted toward INT, WIS, CHA, CRA, and LEA. Physical attributes (STR, END, DEX, VIT, SUR) appear in personality modifiers less frequently. This weighting applies equally to positive and negative modifiers.

| # | Trait | Opposite | Group | Modifier | Special Effect |
|---|---|---|---|---|---|
| P01 | **Brave** | Craven | courage | +1 STR | +25% success in beast encounters |
| P02 | **Craven** | Brave | courage | −1 STR | −25% success in beast encounters |
| P03 | **Studious** | Dull-witted | intellect | +1 INT | +25% chance of attribute gain from crafting/refining actions |
| P04 | **Dull-witted** | Studious | intellect | −1 INT | −25% chance of attribute gain from crafting/refining actions |
| P05 | **Diligent** | Idle | drive | +1 INT | +10% chance of attribute gain from all actions |
| P06 | **Idle** | Diligent | drive | −1 END, −1 INT | −10% chance of attribute gain from all actions |
| P07 | **Cautious** | Reckless | risk | +1 WIS | +10% to travel/explore/survey/delve time-lock durations |
| P08 | **Reckless** | Cautious | risk | +1 DEX, −1 WIS | −10% to travel/explore/survey/delve time-lock durations |
| P09 | **Sociable** | Solitary | social | +1 CHA | +1 max follower |
| P10 | **Solitary** | Sociable | social | −1 CHA | −10% to all energy costs when adventurer has no human followers (labourers only in base module; donkeys & mounts don't prevent the effect) |
| P11 | **Generous** | Greedy | wealth | +1 CHA | +10% follower food cost, +5% labourer follower throughput |
| P12 | **Greedy** | Generous | wealth | −1 CHA | −10% all upkeep costs, +20% personal food cost |
| P13 | **Patient** | Impulsive | tempo | +1 CRA | +5% to all success chances |
| P14 | **Impulsive** | Patient | tempo | −1 CRA | −10% travel time (rush movement) |
| P15 | **Curious** | Incurious | wonder | +1 WIS | +25% chance of attribute gain from explore/survey/delve actions |
| P16 | **Incurious** | Curious | wonder | −1 WIS | −10% energy cost while on a settlement hex |
| P17 | **Tenacious** | Fickle | resolve | +1 END, +1 CRA | +10% energy regen while health <75% |
| P18 | **Fickle** | Tenacious | resolve | −1 END, −1 CRA | −10% max health; +20% energy regen while health is full |
| P19 | **Devout** | Skeptical | faith | +1 VIT | +5% health regen rate |
| P20 | **Skeptical** | Devout | faith | −1 VIT | +5% crafting quality (mutation bonus) |
| P21 | **Commanding** | Meek | authority | +1 LEA | +10% follower throughput |
| P22 | **Meek** | Commanding | authority | −1 LEA | +5% personal production efficiency (solo bonus) |
| P23 | **Optimistic** | Pessimistic | outlook | +1 VIT | +10% food buff duration |
| P24 | **Pessimistic** | Optimistic | outlook | −1 VIT | −10% food cost (rationing) |
| P25 | **Methodical** | Scatterbrained | focus | +1 CRA | −15% equipment durability decay rate |
| P26 | **Scatterbrained** | Methodical | focus | −1 CRA | +5% to all time-lock durations |
| P27 | **Fierce** | Composed | temper | +1 STR, −1 WIS | +20% damage dealt in beast encounters |
| P28 | **Composed** | Fierce | temper | +1 WIS | −25% chance of negative trait gain from beast encounters |
| P29 | **Forthright** | Sly | honesty | +1 SUR | +10% chance of positive skill trait gain from successful actions |
| P30 | **Sly** | Forthright | honesty | −1 SUR | −20% chance of negative skill trait gain from failures |
| P31 | **Merciful** | Ruthless | empathy | +1 VIT | Followers desert 20% slower; +20% chance of positive trait gain from helping/healing events |
| P32 | **Ruthless** | Merciful | empathy | −1 VIT | +20% yield from hunting, +20% chance of positive trait gain from hunting/beast encounters |
| P33 | **Shrewd** | Naive | cunning | +1 INT | −15% building upkeep cost |
| P34 | **Naive** | Shrewd | cunning | −1 INT | +5% encounter success chance, +10% damage taken from encounters |
| P35 | **Ambitious** | Content | aspiration | +1 LEA | +10% chance of trait gain from all actions |
| P36 | **Content** | Ambitious | aspiration | −1 LEA | +10% energy regen while on a settlement hex |
| P37 | **Zoophilist** | Zoophobe | speciesism | +1 WIS, −1 CHA | −25% travel time-lock duration with a mount, +10% health regen rate with an animal follower |
| P38 | **Zoophobe** | Zoophilist | speciesism | −1 WIS | −25% hunting energy cost, +15% to all energy costs with an animal follower |

> **Notes:**
> - P05 Diligent uses `[+1]` (INT — diligent application sharpens the mind through study and practice).
> - P06 Idle uses `[−1, −1]` (END/INT — idleness atrophies both body and mind).
> - P08 Reckless uses the `[+1, −1]` shape (DEX/WIS trade-off).
> - P17 Tenacious uses `[+1, +1]` (END/CRA — tenacity sustains both stamina and perseverance in craft).
> - P18 Fickle (renamed from Faint-hearted) uses `[−1, −1]` (END/CRA — fickleness saps both stamina and patience for careful work).
> - P27 Fierce uses the `[+1, −1]` shape (STR/WIS trade-off).
> - P29–P32 are CK3-inspired additions (Forthright/Sly, Merciful/Ruthless).
> - P33–P36 are new pairs added for attribute balance (Shrewd/Naive, Ambitious/Content).
> - Some traits share CK3 names where no comfortable alternative exists (Brave/Craven, Generous/Greedy, Diligent, Patient/Impulsive).
> - Renamed from CK3 originals: Gregarious→Sociable, Wrathful→Fierce, Calm→Composed, Honest→Forthright, Deceitful→Sly, Compassionate→Merciful, Callous→Ruthless.

---

## 2. Physical Traits

Physical traits describe an adventurer's bodily characteristics. Most come in exclusive pairs. Gained at mint (1 trait); very rarely gained/replaced through major physical events.

Physical traits **never affect INT or WIS** — they represent bodily characteristics, not mental ones. Physical modifiers are slightly more weighted toward STR, END, DEX, VIT, and SUR, with only occasional CHA, CRA, or LEA effects where thematically justified.

| # | Trait | Opposite | Group | Modifier | Special Effect |
|---|---|---|---|---|---|
| H01 | **Sturdy** | Frail | build | +1 END | +10 kg carry capacity, −5% construction time-lock duration |
| H02 | **Frail** | Sturdy | build | −1 END | −10 kg carry capacity; −5% travel time (light frame) |
| H03 | **Nimble** | Clumsy | agility | +1 SUR | +10% hazard avoidance |
| H04 | **Clumsy** | Nimble | agility | −1 SUR | −10% hazard avoidance |
| H05 | **Perceptive** | Oblivious | vision | +1 DEX | −10% explore/survey/delve energy cost |
| H06 | **Oblivious** | Perceptive | vision | −1 DEX | −5% chance of negative trait gain |
| H07 | **Hardy** | Ailing | constitution | +1 STR | +10% health regen rate |
| H08 | **Ailing** | Hardy | constitution | −1 STR | −10% health regen rate |
| H09 | **Tall** | Short | stature | +1 LEA | +10% logging yield |
| H10 | **Short** | Tall | stature | −1 DEX, −1 LEA | +15% mining yield |
| H11 | **Lean** | Broad | movement | +1 DEX | −10% travel energy cost |
| H12 | **Broad** | Lean | movement | −1 DEX | +5% logging/mining/harvesting yields, +5% damage in encounters |
| H13 | **Keen-nosed** | Dull-nosed | senses | +1 SUR | −10% foraging/hunting/delving energy cost, +10% cooking success chance |
| H14 | **Dull-nosed** | Keen-nosed | senses | −1 SUR | +10% harvesting/livestock yields |
| H15 | **Voracious** | Squeamish | digestion | +1 VIT | +10% personal food cost, −5% to success chances when below 50% maximum food |
| H16 | **Squeamish** | Voracious | digestion | −1 VIT | −15% personal food cost |
| H17 | **Resilient** | Sensitive | resilience | +1 END | −10% health loss from encounters |
| H18 | **Sensitive** | Resilient | resilience | −1 END | +10% chance of negative trait gain |
| H19 | **Robust** | Slight | size | +1 STR, +1 END | +10% max health, −10% construction energy cost |
| H20 | **Slight** | Robust | size | +1 SUR, −1 STR | −10% max health, +5% energy regen rate |
| H21 | **Comely** | Ugly | appearance | +1 CHA | −5% upkeep cost, +25% success chance in social encounters |
| H22 | **Ugly** | Comely | appearance | −1 CHA | +25% chance of positive skill trait gain, cannot gain labourer followers |
| H23 | **Ambidextrous** | — | handedness | +1 DEX, +1 CRA | +5% to all personal yields |

> **Notes:**
> - H01 Sturdy (renamed from Strong-backed).
> - H04 Clumsy: simplified special effect to −10% hazard avoidance only.
> - H05/H06 Perceptive/Oblivious (renamed from Eagle-eyed/Dim-sighted).
> - H08 Ailing has a single `[−1]` modifier with −10% health regen rate.
> - H09 Tall uses `[+1]` (LEA — tall stature commands natural authority in a medieval world). Logging yield increased to +10%.
> - H10 Short uses `[−1, −1]` (DEX/LEA — short stature means less reach and less imposing presence). Mining yield increased to +15%.
> - H11/H12 Lean/Broad (renamed from Fleet-footed/Heavy-footed). Broad's special effect changed to production yields + encounter damage.
> - H15/H16 Voracious/Squeamish (renamed from Iron-stomached/Weak-stomached).
> - H17/H18 Resilient/Sensitive (renamed from Thick-skinned/Thin-skinned).
> - H19/H20 Robust/Slight (renamed from Towering/Stunted) — group remains 'size'. CK3-inspired congenital traits with `[+1, +1]` and `[+1, −1]` shapes.
> - H21/H22 Comely/Ugly (renamed Plain→Ugly). Ugly's special effect includes +25% positive skill trait gain but cannot gain labourer followers.
> - H23 Ambidextrous remains the sole trait with no opposite. Now has special effect: +5% to all personal yields.
> - **Physical traits never modify INT or WIS.** Mental attributes are exclusively the domain of personality traits.
> - Renamed from CK3/previous originals: Strong-backed→Sturdy, Eagle-eyed→Perceptive, Dim-sighted→Oblivious, Fleet-footed→Lean, Heavy-footed→Broad, Iron-stomached→Voracious, Weak-stomached→Squeamish, Thick-skinned→Resilient, Thin-skinned→Sensitive, Towering→Robust, Stunted→Slight, Plain→Ugly.

---

## 3. Skill Traits

Skill traits represent permanent specialization earned through gameplay. **Positive** skill traits are gained from **successful actions** (chance-based per action resolution). **Negative** skill traits are gained from **failures and defeats** (more severe failures have higher trigger chance). Once gained, a skill trait **cannot be lost**.

Skill traits become harder to acquire as total trait count rises (same exponential `(1 − count/10)²` formula). More critical successes/failures have higher base chances.

**Once a skill trait is learned, its opposite can never be gained** — learning a positive trait permanently locks out the corresponding negative, and vice versa. This creates meaningful divergence between adventurers as they specialize.

All skill traits are organized in exclusive pairs grouped by activity domain. Each pair shares a domain and the lock-out rule: gaining one permanently prevents the other.

| # | Trait | Opposite | Domain | Modifier | Special Effect | Gained from |
|---|---|---|---|---|---|---|
| S01 | **Green Thumb** | Barren Touch | harvesting | +1 WIS | +10% harvest yield | Successful harvesting/foraging |
| S02 | **Barren Touch** | Green Thumb | harvesting | −1 WIS | −10% harvest yield | Harvesting/foraging failures |
| S03 | **Born Hunter** | Skittish | hunting | +1 DEX | +10% hunting success | Successful hunts |
| S04 | **Skittish** | Born Hunter | hunting | −1 DEX | −10% hunting success | Hunting failures |
| S05 | **Prospector** | Jinxed Miner | mining (discovery) | — | +10% mining success chance, +25% chance of attribute gain from mining | Successful mining (rare vein discovery) |
| S06 | **Jinxed Miner** | Prospector | mining (discovery) | — | +10% mine collapse chance; −25% chance of attribute gain from mining | Mine collapses |
| S07 | **Master Feller** | Timber-cursed | logging | +1 END | +10% logging yield | Successful logging |
| S08 | **Timber-cursed** | Master Feller | logging | −1 END | −10% logging yield; +5% logging injury chance | Logging injuries |
| S09 | **Trailblazer** | Lost | exploring | — | −15% explore energy cost; +25% chance of attribute gain from exploring | Exploring new hexes |
| S10 | **Lost** | Trailblazer | exploring | — | +15% explore energy cost; −25% chance of attribute gain from exploring | Injuries while exploring |
| S11 | **Pathfinder** | Footsore | travel | +1 WIS | −10% travel time in explored hexes | Extensive travel |
| S12 | **Footsore** | Pathfinder | travel | −1 WIS | +10% travel energy cost in explored hexes | Travel injuries/exhaustion |
| S13 | **Artificer** | Fumble-fingers | crafting | +1 CRA | +10% crafting quality | Crafting success |
| S14 | **Fumble-fingers** | Artificer | crafting | −1 CRA | −10% crafting quality | Crafting failures |
| S15 | **Refined Taste** | Wasteful | refining | +1 INT | +10% refining yield | Successful refining |
| S16 | **Wasteful** | Refined Taste | refining | −1 INT | −10% refining yield | Failed refining attempts |
| S17 | **Beast Slayer** | Prey | combat (beasts) | +1 STR | +10% beast encounter success | Surviving T2+ beast encounters |
| S18 | **Prey** | Beast Slayer | combat (beasts) | −1 STR | −10% beast encounter success | Near-death beast encounters |
| S19 | **Master Builder** | Shoddy Builder | building | +1 INT | −10% building construction time | Constructing buildings |
| S20 | **Shoddy Builder** | Master Builder | building | −1 INT | +10% building construction time | Building failures/collapses |
| S21 | **Herdsman** | Neglectful | livestock | +1 CHA | +10% livestock yield | Sustained livestock management |
| S22 | **Neglectful** | Herdsman | livestock | −1 CHA | −10% livestock yield; +10% livestock sickness chance | Livestock deaths/neglect |
| S23 | **Shepherd** | Harsh Keeper | followers | +1 SUR | Follower food consumption −15%, +5% follower task throughput | Gaining new followers (increasingly higher chance the more followers gained) |
| S24 | **Harsh Keeper** | Shepherd | followers | −1 SUR | Followers consume +15% food; +10% follower desertion rate | Follower starvation/deaths |
| S25 | **Cartographer** | Disoriented | survey | — | −10% survey energy cost | Surveying many areas |
| S26 | **Disoriented** | Cartographer | survey | — | +10% survey energy cost | Getting lost during surveys |
| S27 | **Chef** | Scorched Palate | cooking | +1 VIT | +15% food buff effectiveness from cooking | Successful cooking |
| S28 | **Scorched Palate** | Chef | cooking | −1 VIT | −15% food buff effectiveness | Cooking failures |
| S29 | **Tamer** | Unruly | follower recruitment | +1 LEA | +1 max follower | Recruiting/maintaining many followers |
| S30 | **Unruly** | Tamer | follower recruitment | −1 LEA | Followers desert 25% faster | Follower desertions |
| S31 | **Survivor** | Shaken | endurance | +1 VIT | +10% health regen rate | Surviving at <20% health |
| S32 | **Shaken** | Survivor | endurance | −1 VIT | −10% health regen rate; −5% success chance when health <50% | Accumulated near-death experiences |
| S33 | **Quarrier** | Cave-in Survivor | mining (safety) | +1 STR | −10% mine collapse chance | Mining without collapses |
| S34 | **Cave-in Survivor** | Quarrier | mining (safety) | −1 STR | +10% mine collapse chance; +5% mining energy cost | Severe mine collapses |
| S35 | **Herbalist** | Rash Forager | herb gathering | +1 SUR | +10% herb yield; herbal foods +5% regen bonus | Gathering herbs |
| S36 | **Rash Forager** | Herbalist | herb gathering | −1 SUR | −10% herb yield; +10% chance of poisoning from gathered food | Foraging poisonings |


> **Notes:**
> **Notes:**
> - S05/S06 Prospector/Jinxed Miner, S09/S10 Trailblazer/Lost, and S25/S26 Cartographer/Disoriented have **no attribute modifier** — their special effects are sufficient value.
> - Quarrier and Prospector are separate pairs (Quarrier ↔ Cave-in Survivor; Prospector ↔ Jinxed Miner) — mining discovery and mining safety are distinct skill domains.
> - S13 Artificer (renamed from Forgeborn). S15 Refined Taste (renamed from Alchemist's Touch). S18 Prey (renamed from Beast-shy). S27 Chef (renamed from Cook).
> - S37/S38 Marksman/Shaky Hands removed.
> - Additional skill traits can be introduced by future modules.

---

## 5. Injury Traits

Injury traits are temporary debuffs gained from encounter damage or activity mishaps. They persist until the adventurer **returns to >75% of maximum health and completes any action**, at which point the injury either:
- **Heals completely** (trait removed), or
- **Converts to a disability trait** (probability scales with severity).

**Severity scales with damage dealt**, not fixed per injury type. The same injury (e.g., Lacerated) can be low or critical severity depending on how much health damage caused it:

| Damage range | Severity | Disability conversion chance |
|---|---|---|
| 1–15 | Low | 0% (always heals) |
| 16–35 | Medium | 10% |
| 36–60 | High | 25% |
| 61+ | Critical | 50% |

Conversion probabilities are autoregulator-tunable.

| # | Trait | Modifier | Special Effect | Possible disability conversion |
|---|---|---|---|---|
| I01 | **Bruised** | −1 STR | −5% production efficiency | Crooked |
| I02 | **Lacerated** | −1 DEX | −10% hazard avoidance | Scarred |
| I03 | **Fractured** | −1 STR, −1 DEX | Cannot perform heavy actions (mining, logging, construction) | Lame, Crooked |
| I04 | **Concussed** | −1 INT | −15% crafting/refining quality; −10% survey efficiency | Addled, Deaf |
| I05 | **Sprained** | −1 DEX | +20% travel energy cost | Lame |
| I06 | **Winded** | −1 END | −20% energy regen rate | Broken Spirit |
| I07 | **Gashed** | −1 VIT | −100% health regen rate (must have positive regen buff to heal) | Scarred |
| I08 | **Burned** | −1 CRA | −15% crafting quality; −10% production efficiency | Scarred, Nerve-dead, Missing Fingers |
| I09 | **Dislocated** | −1 STR | −20% carry capacity | Crooked |
| I10 | **Blinded (temporary)** | −2 DEX | −25% all success rates; cannot survey | One-eyed |
| I11 | **Frostbitten** | −1 END | −15% energy regen; +20% cold biome energy costs | Nerve-dead, Missing Fingers |
| I12 | **Poisoned** | −1 VIT | −100% health regen rate (must have positive regen buff to heal) | Sickened |
| I13 | **Traumatised** | −1 CHA | −50% chance of attribute gain | Broken Spirit |

---

## 6. Disability Traits

Disability traits are permanent. They are converted from severe injury traits when an injury resolves poorly. They cannot be lost. Disability trait modifiers **suppress** attributes (they reduce the effective value but the underlying attribute points remain — if the disability were hypothetically removed by a future module, the points would return).

| # | Trait | Modifier | Special Effect | Converted from |
|---|---|---|---|---|
| D01 | **One-eyed** | −1 DEX | +10% explore/survey/delve energy cost | Blinded (temporary) |
| D02 | **Lame** | −1 DEX | +15% travel energy cost; +15% travel time | Fractured, Sprained |
| D03 | **Crooked** | −1 STR | −10 kg carry capacity | Fractured, Dislocated, Bruised |
| D04 | **Scarred** | — | +5% beast encounter success (intimidation); +25% chance of CHA gain from social events | Lacerated, Gashed, Burned |
| D05 | **Addled** | −1 INT | −10% refining/crafting quality | Concussed |
| D06 | **Nerve-dead** | −1 CRA | −10% crafting quality; immunity to pain-based injury effects | Burned, Frostbitten |
| D07 | **Sickened** | −1 VIT | −5% max health; +5% poison resistance (built immunity) | Poisoned |
| D08 | **Deaf** | −1 SUR | −10% beast encounter detection; immune to sound-based hazards | Concussed (critical) |
| D09 | **Missing Fingers** | −1 CRA, −1 DEX | −15% crafting quality; −10% tool effectiveness | Frostbitten (severe), Burned (severe) |
| D10 | **Broken Spirit** | −1 LEA | −1 max follower (min 1); −25% chance of positive trait gain | Winded, Traumatised |

> **Notes:**
> - D04 Scarred has **no attribute modifier** — its effects are a mixed blessing (intimidation + social growth).
> - D09 Missing Fingers uses the `[−1, −1]` shape.
> - D10 Broken Spirit reduces **future trait gain chance** — a meta-penalty reflecting cumulative trauma.

---

## Attribute Modifier Balance

The following tables show the distribution of attribute modifiers across all personality and physical traits. The goal is **rough parity** — no attribute should be dramatically over- or under-represented.

### Personality Trait Modifier Tally (38 traits, 19 pairs)

| Attribute | Positive (+) | Negative (−) | Net | Count |
|---|---|---|---|---|
| STR | 2 (P01, P27) | 1 (P02) | +1 | 3 |
| END | 1 (P17) | 2 (P06, P18) | −1 | 3 |
| DEX | 1 (P08) | 0 | +1 | 1 |
| VIT | 3 (P19, P23, P31) | 3 (P20, P24, P32) | 0 | 6 |
| INT | 3 (P03, P05, P33) | 3 (P04, P06, P34) | 0 | 6 |
| WIS | 4 (P07, P15, P28, P37) | 4 (P08, P16, P27, P38) | 0 | 8 |
| CHA | 2 (P09, P11) | 3 (P10, P12, P37) | −1 | 5 |
| SUR | 1 (P29) | 1 (P30) | 0 | 2 |
| CRA | 3 (P13, P17, P25) | 3 (P14, P18, P26) | 0 | 6 |
| LEA | 2 (P21, P35) | 2 (P22, P36) | 0 | 4 |

> Personality traits are weighted toward INT, WIS, CHA, CRA, LEA (4–8 instances each). P05 Diligent (+1 INT) and P06 Idle (−1 END, −1 INT) use dual/shifted modifiers to represent how diligence sharpens the mind while idleness atrophies both body and mind. P17 Tenacious (+1 END, +1 CRA) and P18 Fickle (−1 END, −1 CRA) use dual shapes to link physical endurance with persistence in craft. P37 Zoophilist uses the `[+1, −1]` shape (WIS/CHA trade-off). P38 Zoophobe uses `[−1]` (WIS). CHA now has a slight negative skew (−1 net) from the new speciesism pair.

### Physical Trait Modifier Tally (23 traits, 11 pairs + 1 solo)

| Attribute | Positive (+) | Negative (−) | Net | Count |
|---|---|---|---|---|
| STR | 2 (H07, H19 Robust) | 2 (H08, H20 Slight) | 0 | 4 |
| END | 3 (H01 Sturdy, H17 Resilient, H19 Robust) | 2 (H02, H18 Sensitive) | +1 | 5 |
| DEX | 3 (H05, H11, H23) | 3 (H06, H10, H12) | 0 | 6 |
| VIT | 1 (H15) | 1 (H16) | 0 | 2 |
| INT | 0 | 0 | 0 | 0 |
| WIS | 0 | 0 | 0 | 0 |
| CHA | 1 (H21) | 1 (H22) | 0 | 2 |
| SUR | 3 (H03, H13, H20) | 2 (H04, H14) | +1 | 5 |
| CRA | 1 (H23) | 0 | +1 | 1 |
| LEA | 1 (H09) | 1 (H10) | 0 | 2 |

> Physical traits are weighted toward STR, END, DEX, SUR (4–6 instances). INT and WIS are intentionally excluded. H09 Tall (+1 LEA) represents commanding stature; H10 Short (−1 DEX, −1 LEA) represents reduced reach and less imposing presence. Minor imbalances: END, SUR, CRA each +1 net — within tolerance.

### Combined (Personality + Physical)

| Attribute | Total + | Total − | Net | Total instances |
|---|---|---|---|---|
| STR | 4 | 3 | +1 | 7 |
| END | 4 | 4 | 0 | 8 |
| DEX | 4 | 3 | +1 | 7 |
| VIT | 4 | 4 | 0 | 8 |
| INT | 3 | 3 | 0 | 6 |
| WIS | 4 | 4 | 0 | 8 |
| CHA | 3 | 4 | −1 | 7 |
| SUR | 4 | 3 | +1 | 7 |
| CRA | 4 | 3 | +1 | 7 |
| LEA | 3 | 3 | 0 | 6 |

> All attributes within 6–8 total instances. Maximum net deviation is ±1 (STR, DEX, SUR, CRA at +1; CHA at −1). CHA's slight negative skew comes from Zoophilist's `[+1 WIS, −1 CHA]` shape where the positive WIS is the primary modifier and CHA is the trade-off cost — thematically, someone who bonds deeply with animals may be less socially adept with people. No attribute exceeds ±1 net.

### Skill Trait Modifier Tally (36 traits: 18 pairs across 18 activity domains)

Skill traits are earned through gameplay (not minted). They represent permanent specialization and consequences. Each pair shares an activity domain and is mutually exclusive. Some traits have no modifier (special effect only).

**Positive Skill Traits (18)**

| Attribute | Traits | Count |
|---|---|---|
| STR | S17 Beast Slayer, S33 Quarrier | 2 |
| END | S07 Master Feller | 1 |
| DEX | S03 Born Hunter | 1 |
| VIT | S27 Chef, S31 Survivor | 2 |
| INT | S15 Refined Taste, S19 Master Builder | 2 |
| WIS | S01 Green Thumb, S11 Pathfinder | 2 |
| CHA | S21 Herdsman | 1 |
| SUR | S23 Shepherd, S35 Herbalist | 2 |
| CRA | S13 Artificer | 1 |
| LEA | S29 Tamer | 1 |
| — (no modifier) | S05 Prospector, S09 Trailblazer, S25 Cartographer | 3 |

**Negative Skill Traits (18)**

| Attribute | Traits | Count |
|---|---|---|
| STR | S18 Prey, S34 Cave-in Survivor | 2 |
| END | S08 Timber-cursed | 1 |
| DEX | S04 Skittish | 1 |
| VIT | S28 Scorched Palate, S32 Shaken | 2 |
| INT | S16 Wasteful, S20 Shoddy Builder | 2 |
| WIS | S02 Barren Touch, S12 Footsore | 2 |
| CHA | S22 Neglectful | 1 |
| SUR | S24 Harsh Keeper, S36 Rash Forager | 2 |
| CRA | S14 Fumble-fingers | 1 |
| LEA | S30 Unruly | 1 |
| — (no modifier) | S06 Jinxed Miner, S10 Lost, S26 Disoriented | 3 |

**Skill Trait Net Balance**

| Attribute | Positive (+) | Negative (−) | Net | Total |
|---|---|---|---|---|
| STR | 2 | 2 | 0 | 4 |
| END | 1 | 1 | 0 | 2 |
| DEX | 1 | 1 | 0 | 2 |
| VIT | 2 | 2 | 0 | 4 |
| INT | 2 | 2 | 0 | 4 |
| WIS | 2 | 2 | 0 | 4 |
| CHA | 1 | 1 | 0 | 2 |
| SUR | 2 | 2 | 0 | 4 |
| CRA | 1 | 1 | 0 | 2 |
| LEA | 1 | 1 | 0 | 2 |

> **Analysis:**
>
> All attributes are at **net zero** — every positive skill modifier has a corresponding negative. 3 positive and 3 negative traits have no modifier — their special effects are sufficient value. Representation ranges from 2 (END, DEX, CHA, CRA, LEA) to 4 (STR, VIT, INT, WIS, SUR).
>
> **Remaining improvement potential (future modules):**
>
> 1. **CRA is light** (1 pair). Advanced crafting modules (enchanting, inscription, alchemy mastery) could add CRA skill pairs.
> 2. **LEA is light** (1 pair). Diplomatic or military leadership modules could add LEA skill pairs.
> 3. **DEX is light** (1 pair). Ranged combat or agility-based modules could expand this.
> 4. **END is light** (1 pair). Endurance-testing activities could expand this.
> 5. **CHA is light** (1 pair). Social/trade modules could add CHA skill pairs.

---

## Summary Statistics

| Type | Count | Exclusive pairs | Mint allocation | Extensible? |
|---|---|---|---|---|
| Personality | 38 (19 pairs) | 19 groups | 2 at mint | Base module only |
| Physical | 23 (11 pairs + 1 solo) | 11 groups | 1 at mint | Base module only |
| Skill (positive) | 18 | 18 pairs (exclusive) | 0 at mint | Future modules can add more |
| Skill (negative) | 18 | 18 pairs (exclusive) | 0 at mint | Future modules can add more |
| Injury | 13 | None | 0 at mint | Future modules can add more |
| Disability | 10 | None | 0 at mint | Future modules can add more |
| **Total (base)** | **120** | **48 groups** | **3 at mint** | — |

**Modifier shape distribution:**

| Shape | Count | Notes |
|---|---|---|
| `[+1]` | ~47 | Most common |
| `[−1]` | ~36 | Most common (negative) |
| `[+2]` | 0 | — |
| `[−2]` | 1 | I10 Blinded (temporary) has −2 DEX |
| `[+1, +1]` | 4 | P17 Tenacious, H19 Robust, H23 Ambidextrous |
| `[+1, −1]` | 4 | P08 Reckless, P27 Fierce, P37 Zoophilist, H20 Slight |
| `[−1, −1]` | 6 | P06 Idle, P18 Fickle, H10 Short, I03 Fractured, D09 Missing Fingers |
| No modifier | 10 | S05, S06, S09, S10, S25, S26, D04, and others — special effect only |

---

## Design Notes

1. **No-doubling enforcement**: Every trait's attribute modifier targets a *different* stat than its primary special effect where both are present.

2. **Chance-based progression** (no XP tracking):
   - **Attribute gain**: `gain_chance = base_chance × (1 − (level / 20))²` — exponential decrease, 0% at level 20. Attributes can never be lost, only suppressed by trait modifiers.
   - **Trait gain**: `gain_chance = base_chance × (1 − (trait_count / 10))²` — **exponential** decrease (matching attribute gain curve), 0% at 10 traits. Event criticality determines base chance.
   - Some traits **boost or penalize these chances** as their special effect (e.g., Diligent: +10% attribute gain chance; Broken Spirit: −25% trait gain chance).

3. **Injury → Disability conversion**: Severity scales with **damage dealt** (not fixed per injury type):
   - **1–15 damage (Low)**: always heals.
   - **16–35 damage (Medium)**: 10% convert to disability.
   - **36–60 damage (High)**: 25% convert to disability.
   - **61+ damage (Critical)**: 50% convert to disability.
   Conversion probabilities are autoregulator-tunable.

4. **Attribute modifier balance**: Personality and physical traits are designed for rough parity across all 10 attributes. Physical traits never affect INT or WIS. Personality traits are weighted toward INT, WIS, CHA, CRA, LEA. Combined, all attributes fall within 6–8 total instances and no attribute deviates by more than ±1 from net zero. Dual modifier shapes (`[+1, +1]`, `[−1, −1]`) are used on select traits (Diligent/Idle, Tenacious/Fickle, Tall/Short) to improve cross-attribute coverage without inflating trait count. See Attribute Modifier Balance section for full tally.

5. **Skill trait permanency and exclusivity**: Skill traits can never be lost. All 18 skill domains have both a positive and negative trait organized as exclusive pairs. Once either trait in a pair is learned, the opposite can never be gained. This creates meaningful divergence between adventurers as they specialize.

6. **Encounters and events** need detailed definition tables (event name, description, outcome calculation, attribute gain chances, trait gain chances, potential outcomes). This is tracked in the quantification plan (Phase 11c).

7. **Future module extensibility**: Skill, injury, and disability traits can be introduced by future modules. Only personality and physical traits are fully scoped at base module deployment. The trait registry uses `u16` IDs with reserved ranges for future modules.

8. **CK3 inspiration**: Trait system draws from Crusader Kings 3's categories — personality pairs, congenital physical traits, lifestyle skills earned through actions, and tiered injury/disability progression. Adapted for onchain constraints (chance-based, no complex state tracking). Traits sharing CK3 names have been renamed where natural synonyms exist (e.g., Gregarious→Sociable, Wrathful→Fierce, Giant→Robust, Dwarf→Slight, Compassionate→Merciful) to differentiate. Some names kept where no comfortable alternative exists or the common term is preferred (e.g., Brave/Craven, Generous/Greedy, Scarred, Herbalist).

---

*Draft v0.5 — March 2026*
*For review by Lord Krump*
