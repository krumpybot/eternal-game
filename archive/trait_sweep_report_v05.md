# Trait Sweep Report — v0.5 Changes

> Comprehensive analysis following the v0.4 → v0.5 update.
> Report generated 2026-03-20.

---

## 1. CHANGES APPLIED

All requested changes from the update file have been applied to `trait_table_draft.md`. Summary:

### Personality Traits
- **P01/P02 Brave/Craven**: beast encounter success ±10% → ±25%
- **P05/P06 Diligent/Idle**: attribute gain chance ±25% → ±10%
- **P07 Cautious**: hazard avoidance → +10% time-lock durations (travel/explore/survey/delve)
- **P08 Reckless**: rare loot chance → −10% time-lock durations (travel/explore/survey/delve)
- **P10 Solitary**: explore/survey energy → −10% all energy costs when no human followers
- **P11 Generous**: −10% upkeep → +10% follower food cost, +5% labourer throughput
- **P12 Greedy**: +10% resource pickup → −10% all upkeep costs
- **P13 Patient**: −10% crafting time → +5% to all success chances
- **P15 Curious**: +10% special area discovery → −10% explore/survey/delve energy cost
- **P17 Tenacious**: lethal-hit survival → +10% energy regen while health <75%
- **P18 Faint-hearted → Fickle**: energy regen +10% → +20% while health is full
- **P21 Commanding**: reworded to "+10% follower throughput" (no functional change)
- **P23 Optimistic**: food buff duration +5% → +10%
- **P24 Pessimistic**: decay detection → −10% food cost (rationing)
- **P25 Methodical**: −5% repair cost → +5% crafting/repair time-lock durations
- **P26 Scatterbrained**: −5% explore time → +5% all time-lock durations
- **P27 Fierce**: damage dealt +15% → +20%
- **P28 Composed**: clarified as "encounter outcomes"
- **P29 Forthright**: +25% → +10% positive skill trait gain
- **P30 Sly**: −25% → −20% negative skill trait gain
- **P31 Merciful**: 25% → 20% follower desertion slowdown; 25% → 20% trait gain chance
- **P32 Ruthless**: butchering yield → +20% hunting yield + 20% trait gain from hunting/beast
- **P33 Shrewd**: trade profit → −15% building upkeep cost
- **P34 Naive**: beginner's luck → +5% encounter success, +10% damage taken
- **P35 Ambitious**: +25% trait gain from notable/critical → +10% trait gain from all actions
- **P36 Content**: "at settlement" → "on a settlement hex" (minor wording)

### Physical Traits
- **H01**: Strong-backed → **Brawny**
- **H04 Clumsy**: removed mining yield bonus; now −10% hazard avoidance only
- **H05/H06**: Eagle-eyed/Dim-sighted → **Perceptive/Oblivious**. Perceptive: −10% explore/survey/delve energy cost. Oblivious: −5% chance of negative trait gain.
- **H08 Ailing**: disease resistance → −10% health regen rate
- **H09 Tall**: logging yield +5% → +10%
- **H10 Short**: mining yield +5% → +15%
- **H11/H12**: Fleet-footed/Heavy-footed → **Lean/Broad**. Broad: +5% logging/mining/harvesting yields, +5% damage in encounters.
- **H13 Keen-nosed**: +10% foraging → +10% foraging/hunting success
- **H14 Dull-nosed**: poison resistance → +10% harvesting/livestock yields
- **H15/H16**: Iron-stomached/Weak-stomached → **Voracious/Squeamish**. Voracious: +10% food cost, −5% success when hungry. Squeamish: −15% food cost.
- **H17/H18**: Thick-skinned/Thin-skinned → **Resilient/Sensitive**. Sensitive: +10% chance of negative trait gain.
- **H19/H20**: Towering/Stunted → **Stocky/Wiry**. Group changed to "constitution". Stocky: +10% max health. Wiry: −10% max health, +5% energy regen.
- **H21/H22**: Comely/Plain → Comely/**Ugly**. Comely: −5% upkeep, +25% social encounter success. Ugly: +25% positive skill trait gain, cannot gain labourer followers.
- **H23 Ambidextrous**: added +5% to all personal yields

---

## 2. ATTRIBUTE × SPECIAL EFFECT OVERLAP (No-Doubling Rule Violations)

**Rule**: A trait's attribute modifier must not amplify the same stat as its special effect (e.g., +STR shouldn't also give +carry capacity, since STR = carry capacity).

### 🚩 FLAGGED VIOLATIONS

| # | Trait | Modifier | Special Effect | Overlap |
|---|---|---|---|---|
| **P15** | **Curious** | +1 WIS | −10% explore/survey/delve energy cost | ⚠️ **WIS = explore/survey energy efficiency.** The +1 WIS already reduces explore/survey energy cost via `cost = base × (1 − WIS × 0.04)`. The special effect doubles down on the same stat domain. |
| **H05** | **Perceptive** | +1 DEX | −10% explore/survey/delve energy cost | ✅ Clean — DEX is hazard avoidance, not energy cost. No overlap. |
| **H07** | **Hardy** | +1 STR | +10% health regen rate | ✅ Clean — STR is carry capacity, not health regen. |
| **H19** | **Stocky** | +1 STR, +1 END | +10% max health | ⚠️ **Borderline.** END increases max energy (not max health), and STR is carry capacity. VIT is the max health attribute. Technically clean, but thematically the "constitution" group + END + max health feels like double-dipping on "toughness". **Recommend flagging for review.** |
| **H13** | **Keen-nosed** | +1 SUR | +10% foraging/hunting success | ⚠️ **SUR = hunting/foraging success.** `hunt_success += SUR × 5%` and `forage_success += SUR × 5%`. The +1 SUR already boosts hunting/foraging via attribute formula. Special effect stacks on the same domain. |
| **P12** | **Greedy** | −1 CHA | −10% all upkeep costs | ⚠️ **CHA = upkeep energy efficiency.** `cost = base × (1 − CHA × 0.03)`. The −1 CHA increases upkeep cost, while the special effect reduces all upkeep cost — these are offsetting rather than doubling, but they're operating on the exact same stat. The intent is clearly to offset the −CHA penalty, which is a reasonable design choice, but it technically touches the same domain. |

### Summary of Overlaps

| Severity | Count | Details |
|---|---|---|
| **Clear violation** | 1 | P15 Curious (+WIS + explore energy reduction) |
| **Same-domain overlap** | 1 | H13 Keen-nosed (+SUR + foraging/hunting success) |
| **Intentional offset** | 1 | P12 Greedy (−CHA offset by upkeep reduction) |
| **Borderline thematic** | 1 | H19 Stocky (END + max health — different formulas but same "toughness" theme) |

**Recommendation for P15 Curious**: Change special effect to something WIS-adjacent but not directly energy cost — e.g., "+10% chance to discover special/underworld areas on survey" (the original effect) or "+10% survey quality" to avoid overlap with WIS's explore energy formula.

**Recommendation for H13 Keen-nosed**: Consider changing to a different domain — e.g., "+10% herb/rare material yield from foraging" rather than raw success chance, which is directly what SUR provides.

---

## 3. DUPLICATE / NEAR-DUPLICATE SPECIAL EFFECTS

### 🚩 FLAGGED DUPLICATES

| Effect Category | Traits | Issue |
|---|---|---|
| **Explore/survey/delve energy cost reduction** | P15 Curious (−10%), H05 Perceptive (−10%) | ⚠️ **Identical effect and magnitude.** Both give exactly −10% explore/survey/delve energy cost. One is personality, one is physical, so they could stack — but the effects are not unique. |
| **Positive trait gain chance boost** | P29 Forthright (+10% positive skill trait from success), P31 Merciful (+20% positive trait from helping/healing), P32 Ruthless (+20% positive trait from hunting/beast), P35 Ambitious (+10% trait gain from all actions), H22 Ugly (+25% positive skill trait gain) | These are thematically differentiated by trigger condition (success/healing/hunting/all/always). **Acceptable as-is** — the trigger context makes them unique. |
| **Attribute gain chance** | P05 Diligent (+10% all), P03 Studious (+25% crafting/refining) | ✅ Different scope — acceptable. |
| **Negative trait gain chance** | H06 Oblivious (−5%), H18 Sensitive (+10%), P30 Sly (−20% from failures) | ✅ Different directions and triggers — acceptable. |
| **Time-lock duration increases** | P07 Cautious (+10% travel/explore/survey/delve), P25 Methodical (+5% crafting/repair), P26 Scatterbrained (+5% all) | ⚠️ **Scatterbrained (+5% all) strictly contains Methodical (+5% crafting/repair) and overlaps with Cautious (+10% travel).** A character with both Scatterbrained and Cautious effectively gets +15% to travel time-locks and +5% to everything else. Scatterbrained is supposed to be negative (paired with Methodical), but Methodical's effect (+5% to crafting/repair time-locks) is also a *penalty* — so both sides of the focus pair are penalties? |
| **Upkeep cost reduction** | P12 Greedy (−10% all upkeep), P33 Shrewd (−15% building upkeep), H21 Comely (−5% upkeep) | ⚠️ **Three traits reducing upkeep costs.** Greedy is broadest (all), Shrewd is building-specific, Comely is general. Stacking all three gives massive upkeep savings. Different scopes, but the upkeep domain is over-represented. |
| **Follower throughput** | P11 Generous (+5% labourer throughput), P21 Commanding (+10% follower throughput) | ⚠️ **Same effect, different magnitudes.** Generous gives +5% labourer throughput; Commanding gives +10% follower throughput (which includes labourers). Commanding strictly supersedes Generous's throughput component. |
| **Energy regen rate** | P17 Tenacious (+10% while health <75%), P18 Fickle (+20% while health full), P36 Content (+10% on settlement hex), H20 Wiry (+5% always) | ✅ Different conditions — acceptable variety. |
| **Damage dealt/taken in encounters** | P27 Fierce (+20% damage dealt), P28 Composed (−10% damage taken), H12 Broad (+5% damage in encounters), P34 Naive (+10% damage taken) | ✅ Different directions and magnitudes — acceptable. |
| **Health loss / damage taken from encounters** | P28 Composed (−10% damage taken from encounter outcomes), H17 Resilient (−10% health loss from encounters) | ⚠️ **Potentially identical effect.** "Damage taken from encounter outcomes" and "health loss from encounters" may be the same mechanic unless encounters have non-health damage. **Clarification needed** — are these mechanically distinct? If not, same effect at same magnitude. |
| **Food cost** | P11 Generous (+10% follower food cost), P24 Pessimistic (−10% food cost rationing), H15 Voracious (+10% personal food cost), H16 Squeamish (−15% personal food cost) | ✅ Different targets (follower vs personal) and directions. Acceptable. |
| **Hazard avoidance** | H03 Nimble (+10%), H04 Clumsy (−10%) | ✅ Opposite pair — expected. |

### Key Duplicate Concerns

1. **P15 Curious and H05 Perceptive are identical** (−10% explore/survey/delve energy cost). Recommend differentiating — e.g., make Perceptive about survey quality or rare discovery chance instead.
2. **P28 Composed and H17 Resilient may be identical** (both −10% from encounters). Needs mechanical clarification.
3. **Upkeep reduction is triple-stacked** (Greedy/Shrewd/Comely). Consider if this is intentional.

---

## 4. METHODICAL / SCATTERBRAINED DESIGN ISSUE

**Both Methodical and Scatterbrained appear to be penalties** (increasing time-lock durations):
- Methodical: "+5% to crafting/repair time-lock durations" (slower crafting)
- Scatterbrained: "+5% to all time-lock durations" (slower everything)

This breaks the expected pattern where one trait in a pair is beneficial and the other is detrimental. Methodical (+1 CRA) gets a crafting *slowdown* as its special effect, which feels contradictory — a methodical crafter taking longer but producing higher quality would be thematic, but the +CRA modifier already gives craft quality via the attribute formula, so the time-lock increase is just a penalty.

**Recommendation**: Consider making Methodical's effect a clear benefit — e.g., "+5% crafting quality" or "−5% crafting resource cost" — to create the expected positive/negative asymmetry. Alternatively, if the intent is that Methodical people are *slow but thorough*, lean into that by giving a quality bonus alongside the time increase: "+5% crafting/repair time-lock durations; +5% crafting quality."

---

## 5. GAME MECHANICS COVERAGE ANALYSIS

### Mechanics from ETERNAL_GAME.md and their trait coverage:

| Game Mechanic | Traits Affecting It | Coverage Assessment |
|---|---|---|
| **Carry capacity** (STR-based) | H01 Brawny (+10kg) | ✅ Light touch |
| **Max energy** (END-based) | — (no special effects) | ✅ Appropriately left to END attribute |
| **Energy regen** | P17 Tenacious, P18 Fickle, P36 Content, H20 Wiry | ✅ Well covered with varied conditions |
| **Energy cost (general)** | P10 Solitary (−10% all, conditional) | ✅ Unique conditional |
| **Energy cost (explore/survey/delve)** | P15 Curious, H05 Perceptive | ⚠️ Duplicate — see above |
| **Energy cost (travel)** | H11 Lean (−10%) | ✅ |
| **Hazard avoidance** (DEX-based) | H03 Nimble (+10%), H04 Clumsy (−10%) | ✅ |
| **Max health** (VIT-based) | P18 Fickle (−10%), H19 Stocky (+10%), H20 Wiry (−10%) | ✅ |
| **Health regen** | P19 Devout (+5%), H07 Hardy (+10%), H08 Ailing (−10%) | ✅ |
| **Health loss from encounters** | P28 Composed (−10%), H17 Resilient (−10%), P34 Naive (+10%) | ⚠️ Composed/Resilient potentially identical |
| **Damage dealt** | P27 Fierce (+20%), H12 Broad (+5%) | ✅ |
| **Resource yield (general)** | H23 Ambidextrous (+5% all personal yields), H16 Incurious (+5% surveyed areas) | ✅ |
| **Logging yield** | H09 Tall (+10%), H12 Broad (+5%) | ✅ |
| **Mining yield** | H10 Short (+15%), H12 Broad (+5%) | ✅ |
| **Harvesting yield** | H12 Broad (+5%), H14 Dull-nosed (+10%) | ✅ |
| **Foraging/hunting success** | H13 Keen-nosed (+10%) | ✅ |
| **Hunting yield** | P32 Ruthless (+20%) | ✅ |
| **Livestock yields** | H14 Dull-nosed (+10%) | ✅ |
| **Crafting quality** (CRA-based) | P20 Skeptical (+5%) | ✅ |
| **Time-lock durations** | P07 Cautious (+10%), P08 Reckless (−10%), P25 Methodical (+5%), P26 Scatterbrained (+5%) | ⚠️ See Methodical issue above |
| **Travel time** | P14 Impulsive (−10%), H02 Delicate (−5%) | ✅ |
| **Upkeep costs** | P12 Greedy (−10% all), P33 Shrewd (−15% building), H21 Comely (−5%) | ⚠️ Over-represented |
| **Food system** | P11 Generous (+10% follower food), P23 Optimistic (+10% buff duration), P24 Pessimistic (−10% food cost), H15 Voracious, H16 Squeamish | ✅ Well covered |
| **Follower management** | P09 Sociable (+1 max), P21 Commanding (+10% throughput), P11 Generous (+5% throughput), P31 Merciful (−20% desertion) | ✅ |
| **Beast encounter success** | P01 Brave (+25%), P02 Craven (−25%) | ✅ |
| **Success chances (general)** | P13 Patient (+5% all), P34 Naive (+5% encounter) | ✅ |
| **Attribute gain** | P03/P04 Studious/Dull-witted (±25% craft), P05/P06 Diligent/Idle (±10% all) | ✅ |
| **Trait gain** | P29 Forthright (+10%), P30 Sly (−20%), P31 Merciful (+20% healing), P32 Ruthless (+20% hunting), P35 Ambitious (+10% all), H06 Oblivious (−5%), H18 Sensitive (+10%), H22 Ugly (+25%) | ✅ Very well covered |
| **Territorial decay/detection** | — | ❌ **No longer covered** (old Pessimistic effect removed) |
| **Trade/barter** | — | ❌ **No longer covered** (old Shrewd effect removed) |
| **Special area discovery** | — | ❌ **No longer covered** (old Curious effect removed) |
| **Construction time** | — | Only via skill trait S10 Master Builder. No personality/physical trait affects it. |
| **Repair mechanics** | P25 Methodical (time-lock only) | Minimal coverage |
| **Mount/donkey management** | — | ❌ No traits affect mount mechanics |
| **Biome interactions** | — | ❌ No traits affect biome-specific bonuses/penalties |
| **Territorial defense** | — | ❌ **No longer covered** (old Heavy-footed effect removed) |
| **Survey quality** | — | ❌ **No longer covered** (old Eagle-eyed effect removed) |
| **Cooking/food buff** | P23 Optimistic (+10% duration) | Minimal — only duration, not quality |
| **Gear durability/effectiveness** | — | ❌ No traits affect gear mechanics |
| **Refining** | — | Only via attribute (INT) and skill traits. No personality/physical trait directly. |
| **Building construction** | — | Only via skill trait. |
| **Permadeath threshold / lethal encounters** | — | ❌ **No longer covered** (old Tenacious lethal-hit survival removed) |

### 🔍 MISSING COVERAGE — Recommendations for New Special Effects

1. **Territorial decay**: No trait now affects hex decay rate or detection. Consider adding to a trait like Pessimistic or Incurious.
2. **Trade/barter**: Shrewd lost its trade profit bonus. Consider adding trade effects to another trait or restoring as secondary.
3. **Survey quality**: Perceptive replaced Eagle-eyed's survey quality bonus. This is a distinct mechanic worth covering.
4. **Biome interaction**: No traits modify biome penalties/bonuses. A physical trait like "Adaptable" or an effect like "−5% biome movement penalties" could add flavour.
5. **Gear durability**: No traits slow or accelerate gear degradation. Methodical would be a natural fit ("−5% gear degradation rate").
6. **Territorial defense**: Broad replaced Heavy-footed's defense bonus. Settlement/territory defense has no trait coverage now.
7. **Special/underworld discovery**: Curious lost its discovery bonus. This was a unique and flavourful mechanic.
8. **Permadeath resistance**: Tenacious's lethal-hit survival was a powerful and thematic effect. Nothing replaces it. Consider adding to a skill trait earned from surviving near-death.
9. **Cooking quality**: Only Optimistic affects food buffs (duration). No trait affects cooking success or quality directly (that's the Cook skill trait's domain, but a personality effect could supplement it).
10. **Mount effectiveness**: No traits enhance or penalize mount bonuses. Lean would be a natural fit for improved mount synergy.

---

## 6. BALANCE CONCERNS

### Magnitude Flags

| Trait | Effect | Concern |
|---|---|---|
| **P01 Brave** | +25% beast encounter success | **HIGH.** Brave gives +1 STR *and* +25% success — that's a huge swing. At SUR=2 (10% from attribute) + 25% from trait = 35% bonus total. Beast encounters may become trivially safe for Brave adventurers. Old value (10%) was conservative; 25% is very aggressive. |
| **P02 Craven** | −25% beast encounter success | **HIGH.** −25% is devastating. Combined with low SUR, a Craven adventurer may have near-zero beast success. This could make the trait a death sentence in exploration-heavy play. |
| **H10 Short** | +15% mining yield | **Moderate.** Combined with INT yield bonus and Prospector skill trait, this stacks significantly for mining specialists. But mining is a specific niche, so probably fine. |
| **P15 Curious** | −10% explore/survey/delve energy cost | With +1 WIS (which already reduces explore energy cost by 4%), this is double-dipping. At WIS=3 with Curious: 12% from WIS + 10% from trait = 22% reduction — significant but not broken. |
| **P18 Fickle** | +20% energy regen while health full | **Moderate.** 20% is substantial — effectively a permanent boost for cautious players who stay healthy. Could incentivize overly conservative play. |
| **H22 Ugly** | Cannot gain labourer followers | **HIGH mechanical impact.** This is a hard gate, not a percentage modifier. An Ugly adventurer is permanently locked out of labourer throughput bonuses. Combined with −1 CHA (affecting upkeep and desertion), this is very punishing. The +25% positive skill trait gain is compensation, but the labourer restriction is a qualitative gameplay limitation. |
| **P32 Ruthless** | +20% hunting yield + 20% trait gain from hunting/beast | **Moderate-high.** Two bonuses at 20% each is quite generous for a negative-VIT trait. Compare to Merciful which also has two effects at 20% each — but Merciful is positive-VIT. The pair feels balanced against each other. |
| **Upkeep stacking** | Greedy (−10%) + Shrewd (−15%) + Comely (−5%) | **Concern.** Total: −30% upkeep cost if all three are active. That's nearly a third off all maintenance costs. Unlikely to have all three (different trait types + exclusivity), but Greedy + Comely is achievable (one personality, one physical) = −15% upkeep, which is significant. |
| **P35 Ambitious** | +10% trait gain from all actions | **Moderate.** This is a broad multiplier on the trait gain chance formula. At low trait counts (where base chances are highest), this accelerates trait accumulation meaningfully. Combined with Forthright (+10% positive skill gain), an Ambitious/Forthright adventurer gains traits ~20% faster than baseline. |

### Stacking Scenarios (Worst Case)

**Explore Energy Specialist**: Curious (−10%) + Perceptive (−10%) + WIS 5 (−20%) + Trailblazer skill (−15%) = **−55% explore energy cost.** This is approaching the theoretical floor. Very efficient explorers become possible.

**Beast Encounter Monster**: Brave (+25%) + SUR 4 (20% hunt, 10% beast) + Beast Slayer skill (+15%) = **~50%+ beast encounter success bonus.** Beast encounters may become near-guaranteed.

**Mining Specialist**: Short (+15%) + INT 4 (+10%) + Prospector skill (+10%) = **+35% mining yield.** Strong but not broken for a specialized build.

---

## 7. GROUP CONFLICT: CONSTITUTION

H07/H08 (Hardy/Ailing) and H19/H20 (Stocky/Wiry) now share the **"constitution"** group. This means all four traits are mutually exclusive — an adventurer can only have one of Hardy, Ailing, Stocky, or Wiry.

**This is likely intentional** (all relate to physical constitution), but worth confirming:
- Previously Hardy/Ailing were constitution and Towering/Stunted were size — two separate groups allowing an adventurer to be both Hardy and Towering.
- Now a Stocky adventurer cannot also be Hardy. This reduces physical trait combinations.

**Recommendation**: If the intent is full mutual exclusivity, this is fine. If Hardy/Stocky should be stackable (different aspects of constitution), use distinct group names (e.g., "health" for Hardy/Ailing, "constitution" for Stocky/Wiry).

---

## 8. SUMMARY OF FLAGS

### Must-Address (Clear Issues)
1. **P15 Curious**: +WIS modifier overlaps with explore/survey energy cost special effect (same stat domain)
2. **H13 Keen-nosed**: +SUR modifier overlaps with foraging/hunting success special effect (same stat domain)
3. **P15 Curious = H05 Perceptive**: Identical special effects (−10% explore/survey/delve energy cost)
4. **P28 Composed ≈ H17 Resilient**: Potentially identical effects (−10% from encounters) — clarify distinction

### Should-Address (Design Concerns)
5. **P25/P26 Methodical/Scatterbrained**: Both effects are penalties (increased time-locks) — breaks positive/negative pair pattern
6. **P11 Generous + P21 Commanding**: Follower throughput overlap (5% vs 10%) — Commanding supersedes
7. **Upkeep cost triple-stack**: Greedy + Shrewd + Comely = up to −30%
8. **Brave/Craven at ±25%**: Very high magnitude — may trivialise/devastate beast encounters
9. **H22 Ugly**: Hard gate (no labourers) is qualitatively different from percentage modifiers elsewhere

### Consider Adding (Missing Coverage)
10. Territorial decay detection
11. Trade/barter profit
12. Survey quality
13. Biome interaction modifiers
14. Gear durability
15. Territorial defense bonus
16. Special/underworld area discovery
17. Near-death survival mechanic (lost from Tenacious)
18. Mount synergy effects

---

*End of sweep report.*
