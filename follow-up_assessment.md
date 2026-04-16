# Follow-up Assessment (post mid-design response)

Date: 2026-04-16

## 1) Summary of changes made

- **Biome consolidation (27 → 24)**
  - Removed **Plains**, **Steppe**, **Marsh**.
  - Merged affinities and roles: **Plains→Grassland**, **Steppe→Scrubland**, **Marsh→Swamp**.
  - Updated Base §7 biome list and materials sub-type weights.
  - Updated Appendix D (biome categories, profiles removed/merged, Nexus forced to Grassland, lookup table cleaned).
  - Rebuilt Appendix E to 24-biome matrices (columns removed, merges applied).

- **Attribute formula updates (C2)**
  - WIS: `max(0.25, base × (1 − WIS × 0.025))`.
  - CHA: `max(0.4, base × (1 − CHA × 0.02))`.
  - Clarified progression: natural attribute cap 20, effective can exceed via gear/traits up to hard clamp 30.
  - Clarified gain chance uses **natural** attribute only.
  - Updated quantification plan Phase 1b to match.

- **Buildings require use + zero-pop staffing edge (C3)**
  - Understaffing upkeep multiplier updated to:
    `if staffing_ratio == 0 then 3.0 else clamp(1 / staffing_ratio, 1, 3)`.
  - Added explicit note: buildings do not generate automatically, at 0 population buildings cannot be used and incur 3× upkeep.

- **Energy/Health reconciliation (C6 + A8)**
  - Energy regen is now **constant** (includes during time-lock actions).
  - Health no longer passively regenerates, recovery is via **Rest** action only.
  - Updated Appendix M Resting: energy 0, time-lock 6–12 ticks, restores 5–15 HP (VIT + food buffs), repeatable, low encounter chance.

- **Wallet-based ownership clarity (C7)**
  - Added clarification in Settlement + Permadeath: ownership of settlements/hexes/buildings is tied to **wallet address**, not adventurer.
  - Deeds are linking items, not ownership tickets. Destroying deed prevents future linking but does not end ownership.

- **Fishing section added (I2)**
  - Added Base §19 “Production: Fishing” and renumbered subsequent sections + updated TOC.

- **Sustainability thresholds (I3)**
  - Added sustainability-threshold paragraphs to Forestry, Hunting, Foraging.

- **Exchange action (I4)**
  - Added **Exchange** action to Appendix M (instant, free, direct transfer + market listing/purchase semantics).
  - Updated quantification plan to **24 actions**, Social & Economic count = 2.

- **Settlement benefits expanded (A5)**
  - Added benefits table (shared storage, synergies, defensive bonus, transport efficiency, travel discount, trade hub hook).

- **Season hook (A6)**
  - Added cosmetic season counter note in Future Module Placeholders section.

- **Registry alignment (C1)**
  - Base §13 registry replaced with Appendix F-aligned block registry summary + “Appendix F is authoritative” note.

- **Commit-reveal / randomness security (C4+C5 + T2)**
  - Documented reveal-block hash entropy requirement for adventurer seed + discovery seed.
  - Documented craft_seed must include reveal-block hash.

- **Flags / technical notes added (A3, I5, I6, I8, T1–T4)**
  - Permission hook gas-bound + fail-open (A3).
  - Matrix hardcode/packing + noise implementation notes (I5, T3).
  - Autoregulator running counters note (I6).
  - Campfire lifetime flagged for Phase 9b (I8).
  - Saturating arithmetic / lazy death realization note in energy section (T4).

- **Document reorg (A7)**
  - `ETERNAL_GAME.md` moved to `archive/Eternal_Game_Vision.md` and marked frozen (2026-04-16).

- **Quantification plan updates**
  - Updated biome counts/references (24).
  - Added Phase 12c gas-limit/staged computation note.
  - Added Phase 13d meta-diversity note.
  - Added mid-design response status checklist (C/I/A/T items).

## 2) New balance concerns introduced

- **Health recovery loop**: With Rest as the only recovery, encounter damage pacing becomes extremely sensitive to Rest yield (5–15 HP) and time-lock (6–12 ticks). If hazards can spike damage frequently, players may enter Rest spam loops.
- **Energy constant regen**: Constant regen during time-lock reduces the effective cost of long actions relative to the earlier “idle only” model. This likely increases overall throughput and may require later re-tuning of action costs/time-locks.
- **WIS/CHA soft caps**: New floors (0.25/0.4) are strong clamps. High attributes may feel less rewarding after mid levels, depending on base costs. Hard clamp at 30 must be consistently applied across all systems.
- **Biome merges**: Removing Plains/Steppe/Marsh reduces “low hazard, high fertility” granularity and may compress early-settlement terrain variety. Grassland and Swamp now carry extra thematic and mechanical weight.

## 3) Design holes or missing systems

- **Settlement succession mechanics**: Ownership clarified as wallet-based, but the concrete mechanism for multi-adventurer wallets, handoff, or “who can act” if the original adventurer dies still needs explicit rules in Phase 13h.
- **Market implementation**: Exchange action defines market listing needs, but an actual minimal onchain listing model, expiry, and storage strategy must be designed (flagged).
- **Action randomness source**: Commit-reveal is documented for mint/discovery/crafting, but per-action resolution randomness (encounters/yields/trait gain) still needs a precise seed spec (Phase 13f/13g).

## 4) Is the game firm for continuing quantification?

Yes. The critical contradictions called out in the mid-design assessment are now documented and reconciled in the spec. The design is coherent enough to proceed into Phases 5–13, with remaining risks mostly in implementation constraints and balance tuning rather than missing primitives.

## 5) Remaining items needing resolution

- Campfire lifetime (Phase 9b).
- Settlement succession and orphan recovery rules (Phase 13h).
- Action-outcome randomness seed specification + manipulation assumptions (Phase 13f/13g).
- Market listing storage, expiry, and fee model (Exchange action follow-up).
