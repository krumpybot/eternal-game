# Eternal Game — Trait Table (Draft v0.1)

> **Review draft.** All traits listed with name, type, group (exclusivity), attribute modifier, and special effect.
> Attribute modifier shapes: `[+1]`, `[−1]`, `[+2]`, `[−2]`, `[+1, +1]`, `[+1, −1]`, `[−1, −1]`.
> **No-doubling rule**: attribute modifier must not amplify the same stat as the special effect.

---

## Trait Types

| Type | Slots at mint | Can be lost? | Notes |
|---|---|---|---|
| Personality | 2 | Replaced by opposite through major events | Almost all have an opposite |
| Physical | 1 | Replaced by opposite through events | Almost all have an opposite |
| Skill | 0 | **Never** (permanent once gained) | Positive from success, negative from failure; harder to gain as count rises |
| Injury | 0 | Yes — return to >100 health + complete any action | May convert to disability trait |
| Disability | 0 | **Never** (permanent) | Converted from severe injuries |

**Max traits**: 10. Gain chance decreases with current trait count.

---

## 1. Personality Traits

Personality traits define an adventurer's disposition. Almost all come in exclusive pairs (opposite). Gained at mint (2 traits); very rarely gained/lost during play through major life events.

| # | Trait | Opposite | Group | Modifier | Special Effect |
|---|---|---|---|---|---|
| P01 | **Brave** | Craven | courage | +1 STR | +10% success in beast encounters |
| P02 | **Craven** | Brave | courage | −1 STR | −10% success in beast encounters |
| P03 | **Studious** | Dull-witted | intellect | +1 INT | +5% XP gain from crafting/refining actions |
| P04 | **Dull-witted** | Studious | intellect | −1 INT | −5% XP gain from crafting/refining actions |
| P05 | **Ambitious** | Idle | drive | +1 END | +5% attribute gain rate from all actions |
| P06 | **Idle** | Ambitious | drive | −1 END | −5% attribute gain rate from all actions |
| P07 | **Cautious** | Reckless | risk | +1 WIS | +5% hazard avoidance (stacks with DEX) |
| P08 | **Reckless** | Cautious | risk | −1 WIS | −5% hazard avoidance; +10% rare loot chance from encounters |
| P09 | **Gregarious** | Solitary | social | +1 CHA | +1 max follower |
| P10 | **Solitary** | Gregarious | social | −1 CHA | −1 max follower (min 1); +5% explore/survey efficiency |
| P11 | **Generous** | Greedy | wealth | +1 CHA | −10% trade fees when selling |
| P12 | **Greedy** | Generous | wealth | −1 CHA | +10% resource pickup from all sources (rounding down) |
| P13 | **Patient** | Impulsive | tempo | +1 CRA | −10% crafting time-lock duration |
| P14 | **Impulsive** | Patient | tempo | −1 CRA | +10% movement speed (travel time reduction) |
| P15 | **Curious** | Incurious | wonder | +1 WIS | +10% chance to discover special/underworld areas on survey |
| P16 | **Incurious** | Curious | wonder | −1 WIS | +5% yield from already-surveyed production areas |
| P17 | **Tenacious** | Faint-hearted | resolve | +1 END | Survive one lethal hit per in-game day with 1 HP (once) |
| P18 | **Faint-hearted** | Tenacious | resolve | −1 END | −10% max health; +10% energy regen while health is full |
| P19 | **Devout** | Skeptical | faith | +1 VIT | +5% health regen rate |
| P20 | **Skeptical** | Devout | faith | −1 VIT | +5% crafting quality (mutation bonus) |
| P21 | **Commanding** | Meek | authority | +1 LEA | Followers gain +10% task throughput |
| P22 | **Meek** | Commanding | authority | −1 LEA | +5% personal production efficiency (solo bonus) |
| P23 | **Optimistic** | Pessimistic | outlook | +1 VIT | +5% food buff duration |
| P24 | **Pessimistic** | Optimistic | outlook | −1 VIT | +5% decay detection range (see hex decay before visiting) |
| P25 | **Methodical** | Scatterbrained | focus | +1 CRA | −5% repair resource cost |
| P26 | **Scatterbrained** | Methodical | focus | −1 CRA | +5% explore speed |

---

## 2. Physical Traits

Physical traits describe an adventurer's bodily characteristics. Most come in exclusive pairs. Gained at mint (1 trait); very rarely gained/lost during play.

| # | Trait | Opposite | Group | Modifier | Special Effect |
|---|---|---|---|---|---|
| H01 | **Strong-backed** | Frail | build_str | +1 END | +10 kg carry capacity |
| H02 | **Frail** | Strong-backed | build_str | −1 END | −10 kg carry capacity; +5% movement speed |
| H03 | **Nimble** | Clumsy | agility | +1 STR | +10% hazard avoidance |
| H04 | **Clumsy** | Nimble | agility | −1 STR | −5% hazard avoidance; +10% mining yield (brute force) |
| H05 | **Eagle-eyed** | Dim-sighted | vision | +1 SUR | +10% survey reveal quality (better area rolls for areas not yet surveyed — visual range only) |
| H06 | **Dim-sighted** | Eagle-eyed | vision | −1 SUR | −10% survey quality; +10% crafting quality (feel over sight) |
| H07 | **Hardy** | Sickly | constitution | +1 STR | +10% health regen rate |
| H08 | **Sickly** | Hardy | constitution | −1 STR | −10% health regen rate; +1 INT |
| H09 | **Tall** | Short | stature | +1 END | +5% logging yield |
| H10 | **Short** | Tall | stature | −1 END | +5% mining yield (low clearance advantage) |
| H11 | **Fleet-footed** | Heavy-footed | movement | +1 DEX | −10% travel energy cost |
| H12 | **Heavy-footed** | Fleet-footed | movement | −1 DEX | +10% territorial defense bonus |
| H13 | **Keen-nosed** | Dull-nosed | senses | +1 SUR | +10% foraging success |
| H14 | **Dull-nosed** | Keen-nosed | senses | −1 SUR | +5% poison/hazard resistance |
| H15 | **Iron-stomached** | Weak-stomached | digestion | +1 VIT | Food debuffs reduced by 50% |
| H16 | **Weak-stomached** | Iron-stomached | digestion | −1 VIT | Food buffs +20% effectiveness |
| H17 | **Thick-skinned** | Thin-skinned | resilience | +1 END | −10% health loss from encounters |
| H18 | **Thin-skinned** | Thick-skinned | resilience | −1 END | +10% energy regen rate |
| H19 | **Ambidextrous** | — | handedness | +1 DEX, +1 CRA | — (no special; this is a rare mint-only trait with no opposite) |

---

## 3. Skill Traits (Positive)

Positive skill traits are gained through **successful actions**. Once gained, they cannot be lost. They become harder to acquire as total skill trait count increases.

| # | Trait | Group | Modifier | Special Effect | Gained from |
|---|---|---|---|---|---|
| S01 | **Green Thumb** | — | +1 WIS | +10% harvest yield from fertile areas | Repeated successful harvesting |
| S02 | **Born Hunter** | — | +1 DEX | +10% hunting success | Repeated successful hunts |
| S03 | **Prospector** | — | +1 WIS | +10% chance to find rare veins on survey | Repeated successful mining |
| S04 | **Master Feller** | — | +1 END | +10% logging yield | Repeated successful logging |
| S05 | **Trailblazer** | — | +1 DEX | −15% explore energy cost | Discovering many hexes |
| S06 | **Pathfinder** | — | +1 WIS | −10% travel time in explored hexes | Extensive travel across many hexes |
| S07 | **Forgeborn** | — | +1 END | +10% weapon/armor crafting quality | Repeated successful forging |
| S08 | **Alchemist's Touch** | — | +1 INT | +10% refining yield | Repeated successful refining |
| S09 | **Beast Slayer** | — | +1 STR | +15% beast encounter success | Surviving many beast encounters |
| S10 | **Architect** | — | +1 INT | −10% building construction time | Constructing many buildings |
| S11 | **Herdsman** | — | +1 CHA | +10% livestock yield | Sustained livestock management |
| S12 | **Shepherd** | — | +1 SUR | Follower food consumption −15% | Maintaining followers over many in-game days |
| S13 | **Cartographer** | — | +1 WIS | +10% survey efficiency (energy) | Surveying many areas |
| S14 | **Cook** | — | +1 CRA | +15% food buff effectiveness from cooking | Repeated successful cooking |
| S15 | **Tamer** | — | +1 LEA | +1 max follower | Recruiting/maintaining many followers |
| S16 | **Survivor** | — | +1 VIT | +10% health regen rate | Surviving with <20% health multiple times |
| S17 | **Quarrier** | — | +1 STR | −10% mine collapse chance | Extensive mining without collapses |
| S18 | **Herbalist** | — | +1 SUR | +10% herb yield; herbal foods give +5% regen bonus | Repeated herb gathering |

> **Note:** Additional skill traits (e.g., Haggler, Pioneer, etc.) can be introduced by future modules. Only base-module actions generate skill traits at launch.

---

## 4. Skill Traits (Negative)

Negative skill traits are gained from **failures and defeats**. Once gained, they cannot be lost. They represent lasting psychological or habitual impacts from bad experiences.

| # | Trait | Group | Modifier | Special Effect | Gained from |
|---|---|---|---|---|---|
| S21 | **Skittish** | — | −1 SUR | −10% hunting success | Repeated hunting failures |
| S22 | **Jinxed Miner** | — | −1 WIS | +10% mine collapse chance | Repeated mine collapses |
| S23 | **Fumble-fingers** | — | −1 CRA | −10% crafting quality | Repeated crafting failures |
| S24 | **Lost** | — | −1 WIS | +10% explore energy cost | Getting injured repeatedly while exploring |
| S25 | **Beast-shy** | — | −1 STR | −10% beast encounter success | Repeated near-death beast encounters |
| S26 | **Wasteful** | — | −1 INT | −5% refining yield | Repeated failed refining attempts |
| S27 | **Timber-cursed** | — | −1 DEX | −10% logging yield; +5% logging injury chance | Logging injuries |
| S28 | **Shaky Hands** | — | −1 DEX | −5% bow/ranged equipment effectiveness | Accumulated combat trauma |

> **Note:** Additional negative skill traits can be introduced by future modules (e.g., combat-specific trauma traits, social failures, etc.).

---

## 5. Injury Traits

Injury traits are temporary debuffs gained from encounter damage or activity mishaps. They persist until the adventurer **returns to >100 health and completes any action**, at which point the injury either:
- **Heals completely** (trait removed), or
- **Converts to a disability trait** (probability scales with severity).

**Severity scales with damage dealt**, not fixed per injury type. The same injury (e.g., Lacerated) can be low, medium, or high severity depending on how much health damage caused it:

| Damage range | Severity | Disability conversion chance |
|---|---|---|
| 1–15 | Low | 0% (always heals) |
| 16–35 | Medium | 25% |
| 36–60 | High | 50% |
| 61+ | Critical | 75% |

Conversion probabilities are autoregulator-tunable.

| # | Trait | Modifier | Special Effect | Possible disability conversion |
|---|---|---|---|---|
| I01 | **Bruised** | −1 STR | −5% production efficiency | Crooked (if high/critical) |
| I02 | **Lacerated** | −1 DEX | −10% hazard avoidance | Scarred |
| I03 | **Fractured** | −1 STR, −1 DEX | Cannot perform heavy actions (mining, logging, construction) | Lame, Crooked |
| I04 | **Concussed** | −1 INT | −15% crafting/refining quality; −10% survey efficiency | Addled, Deaf (if critical) |
| I05 | **Sprained** | −1 DEX | +20% travel energy cost | Lame (if high/critical) |
| I06 | **Winded** | −1 END | −20% energy regen rate | — (typically heals; Broken Spirit if accumulated) |
| I07 | **Gashed** | −1 VIT | −10% health regen rate; bleeding (−1 health/100 ticks) | Scarred |
| I08 | **Burned** | −1 CRA | −15% crafting quality; −10% production efficiency | Scarred, Nerve-dead, Missing Fingers |
| I09 | **Dislocated** | −1 STR | −20% carry capacity | Crooked |
| I10 | **Blinded (temporary)** | −2 DEX | −25% all success rates; cannot survey | One-eyed |
| I11 | **Frostbitten** | −1 END | −15% energy regen; +20% cold biome energy costs | Nerve-dead, Missing Fingers |
| I12 | **Poisoned** | −1 VIT | −20% health regen; health loss 1/50 ticks until resolved | Sickened |

---

## 6. Disability Traits

Disability traits are permanent. They are converted from severe injury traits when an injury resolves poorly. They cannot be lost.

| # | Trait | Modifier | Special Effect | Converted from |
|---|---|---|---|---|
| D01 | **One-eyed** | −1 DEX | −10% ranged/bow effectiveness; +10% intimidation (beast encounter flee chance) | Blinded (temporary) |
| D02 | **Lame** | −1 DEX | +15% travel energy cost; +15% travel time | Fractured |
| D03 | **Crooked** | −1 STR | −10 kg carry capacity | Fractured, Dislocated |
| D04 | **Scarred** | +1 CHA | +5% beast encounter success (intimidation) | Lacerated, Gashed, Burned |
| D05 | **Addled** | −1 INT | −10% refining/crafting quality | Concussed |
| D06 | **Nerve-dead** | −1 CRA | −10% crafting quality; immunity to pain-based effects | Burned, Frostbitten |
| D07 | **Sickened** | −1 VIT | −5% max health; +5% poison resistance (built immunity) | Poisoned |
| D08 | **Deaf** | −1 SUR | −10% beast encounter detection; immune to sound-based hazards | Concussed (severe) |
| D09 | **Missing Fingers** | −1 CRA, −1 DEX | −15% crafting quality; −10% tool effectiveness | Frostbitten (severe), Burned (severe) |
| D10 | **Broken Spirit** | −1 LEA | −1 max follower (min 1); −10% follower morale | Accumulated trauma (3+ injuries healed) |

---

## Summary Statistics

| Type | Count | Exclusive pairs | Mint allocation | Extensible? |
|---|---|---|---|---|
| Personality | 26 (13 pairs) | 13 groups | 2 at mint | Base module only |
| Physical | 19 (9 pairs + 1 solo) | 9 groups | 1 at mint | Base module only |
| Skill (positive) | 18 | None | 0 at mint | Future modules can add more |
| Skill (negative) | 8 | None | 0 at mint | Future modules can add more |
| Injury | 12 | None | 0 at mint | Future modules can add more |
| Disability | 10 | None | 0 at mint | Future modules can add more |
| **Total (base)** | **93** | **22 groups** | **3 at mint** | — |

---

## Design Notes

1. **No-doubling enforcement**: Every trait's attribute modifier targets a *different* stat than its primary special effect. E.g., Strong-backed grants +10 kg carry (a STR-domain effect) but modifies +1 END (not STR).

2. **Trait gain difficulty**: `gain_chance = base_chance × (1 − current_trait_count × 0.08)`. At 10 traits, gain chance approaches 0. This formula is autoregulator-tunable.

3. **Injury → Disability conversion**: Severity scales with **damage dealt** (not fixed per injury type):
   - **1–15 damage (Low)**: always heals.
   - **16–35 damage (Medium)**: 25% convert to disability.
   - **36–60 damage (High)**: 50% convert to disability.
   - **61+ damage (Critical)**: 75% convert to disability.
   Conversion probabilities are autoregulator-tunable.

4. **Encounters and events** need detailed definition tables (event name, description, outcome calculation, potential outcomes, attribute gates). This is tracked in the quantification plan (Phase 11c).

5. **Future module extensibility**: Skill, injury, and disability traits can be introduced by future modules. Only personality and physical traits are fully scoped at base module deployment. The trait registry uses `u16` IDs with reserved ranges for future modules.

---

*Draft v0.1 — March 2026*
*For review by Lord Krump*
