# Follow-up Assessment (post mid-design response)

Date: 2026-04-16
Updated: 2026-04-17

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
  - Updated Appendix M Resting: energy 0, time-lock **12 ticks (2 min)**, restores **2–3 HP** (VIT + food buffs), repeatable indefinitely, low encounter chance.
  - Deliberate slow recovery: after heavy damage, adventurers may need to rest continuously for an hour or more. This is intentional — health recovery is a meaningful time investment.

- **Wallet-based ownership & succession (C7, resolved)**
  - Ownership of settlements, hexes, and buildings tied to **wallet address**, not adventurer.
  - Deeds are linking items, not ownership tickets.
  - **Succession model**: Any adventurer (any wallet) can upkeep any hex/settlement. Owning wallet sets activity rules via permission hooks. If last adventurer dies: mint a new one, rely on allies, or let decay/claim cycle handle it. No complex handover mechanics needed.
  - Phase 13h marked resolved.

- **Fishing section added (I2)**
  - Added Base §19 "Production: Fishing" and renumbered subsequent sections + updated TOC.

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
  - Base §13 registry replaced with Appendix F-aligned block registry summary + "Appendix F is authoritative" note.

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

## 2) Balance concerns — resolution status

| Concern | Status | Resolution |
|---|---|---|
| Health recovery loop / Rest spam | ✅ Resolved | By design. Rest yields 2–3 HP per 2 min. Extended recovery after heavy damage is intentional gameplay. |
| Constant energy regen throughput | ✅ Flagged | Action costs and time-locks flagged for Phase 13c review. These remain unlocked/GM-adjustable. |
| WIS/CHA soft cap feel | ✅ Not a concern | Designer confirms no issue. |
| Biome merge terrain variety | ✅ Not a concern | Designer confirms no issue. |

## 3) Design holes — resolution status

| Hole | Status | Resolution |
|---|---|---|
| Settlement succession | ✅ Resolved | Wallet-based ownership + permissionless upkeep + decay/claim cycle. Documented in Base §23 & §27. Phase 13h resolved. |
| Market implementation | ⏳ Deferred | Flagged for future quantification. Storage/expiry/fee model to be designed when Exchange action is quantified. |
| Action randomness source | ⏳ Deferred | Flagged for contracts specialist (Phase 13f/13g). |
| Campfire lifetime | ⏳ Deferred | To be resolved when buildings are quantified (Phase 9b). |

## 4) Is the game firm for continuing quantification?

**Yes.** All critical and important items from the mid-design assessment are resolved or appropriately deferred. The design is coherent, internally consistent, and ready to proceed into Phases 5–13.

## 5) Remaining items (appropriately deferred)

| Item | Phase | Notes |
|---|---|---|
| Campfire lifetime | 9b | Resolve when buildings are quantified |
| Market listing model | Exchange follow-up | Storage, expiry, fees — quantify with trade system |
| Action randomness seeds | 13f/13g | Contracts specialist input needed |
| Action cost/time-lock rebalance | 13c | Review all costs accounting for constant energy regen |
