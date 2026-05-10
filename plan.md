# BrainHive Build Plan

This is a very large spec (8 full games, multiplayer, anti-cheat, ELO, AI, Coach analysis, dictionary bundle). Building all of it in one shot would produce shallow, buggy games. I'll ship it in phases, each phase a working, polished increment you can play and review before we continue.

## Phase 1 — Shell & Design System (this turn)
- Dark-mode-first theme in `src/styles.css` (amber/gold accent, oklch tokens, per-game accent colors)
- Typography: Plus Jakarta Sans (body) + a display font for headers
- Shared layout: Hub home screen with 8 game cards (status: ✅/🔄/⭕), bottom tab nav, Settings, Stats
- Shared infra: Zustand stores (`settings`, `stats`, `daily`), `useLocalStorage`, `useVisibility`, seeded RNG, emoji-share util, Toast/Modal/Button primitives
- Routes via TanStack Start: `/`, `/settings`, `/stats`, `/wordsmith`, `/clusters`, `/threadz`, `/hexhive`, `/chainbox`, `/matchstack`, `/dominogrid`, `/wordduel` (placeholders for unbuilt games)
- Settings: dark/light toggle, color-blind mode, sound, reduced motion, reset stats
- Framer Motion installed; reduced-motion respected

## Phase 2 — WordSmith (next turn)
Full game: 7×5 grid, on-screen + physical keyboard, flip animations, hint token (+1 guess cost), streak shield (1 per 30 days), rarity indicator, Casual/Standard/Scholar modes, Daily (seeded) + Unlimited, share grid with 💡, full stats panel. Bundled common-word answer list + larger guess-validation list (lazy-loaded JSON).

## Phase 3 — WordDuel (the flagship, 2-3 turns)
- 13×13 board, 6-tile rack, custom point values, symmetric DL/TL/DW/TW layout
- Tile bag, drag-to-place, word validation (ENABLE/TWL06 lazy-loaded), cross-word scoring, equal-turns rule
- Power Tiles (every 5 turns: DW / Extra Turn / Peek)
- **Definition Hint system**: enumerates all valid plays from rack+board, shows definitions only (definitions sourced from a bundled compact dictionary — WordNet-derived, open license), 3/game, logged for opponent
- 5-level AI with fair tile draws + transparent suboptimal-play behavior at low levels
- Casual vs Competitive modes; Competitive uses `document.visibilityState` for screen-lock with warning → skip → forfeit ladder; chat disabled mid-turn
- Blitz / Standard / Marathon turn timers
- Local hot-seat + AI for MVP (true async multiplayer needs a backend — see Open Questions)
- ELO updates on competitive games, leaderboard (local for MVP)
- Post-game **Coach** analysis: best vs actual play per turn

## Phase 4 — Clusters, Threadz, HexHive (1-2 turns)
Word-based games sharing the dictionary bundle. Decoy group, Bridge Word, power-up letter, rank tiers, all difficulty modes, share grids.

## Phase 5 — ChainBox, MatchStack, DominoGrid (1-2 turns)
Pentagon chain w/ wildcard; layered tile matching w/ Freeze + color-blind patterns; domino logic with 5 difficulties + Expert constraint shift.

## Phase 6 — Polish
Stats dashboards per game, accessibility audit (ARIA, keyboard nav, WCAG AA), bundle-size pass, animation polish, share-format finalization.

## Technical notes
- 100% client-side, localStorage persistence, seeded daily puzzles via mulberry32 + date key
- Dictionary: ENABLE (~170k words) split into letter-prefix chunks, lazy-loaded per game; definitions for WordDuel from a compact open-license source (WordNet 3.0 short glosses), also chunked
- No NYT names, terms, or assets anywhere

## Open questions (need your call before Phase 3 multiplayer)
1. **WordDuel real multiplayer**: spec says "no backend for MVP", but async human-vs-human + ELO + Marathon (24h turns) require a backend. For MVP I'll ship **AI + local hot-seat only** and stub online matchmaking. OK? Or enable Lovable Cloud now for true async multiplayer?
2. **Dictionary size**: ENABLE + WordNet glosses is ~8-12 MB even chunked/compressed. The <2MB bundle target only works if we lazy-load these on first game launch (not in initial bundle). Confirming this is fine.
3. **Scope confirmation**: ship phases incrementally as above, or do you want a different order (e.g., all 4 word games first)?

If you approve, I'll start Phase 1 immediately.