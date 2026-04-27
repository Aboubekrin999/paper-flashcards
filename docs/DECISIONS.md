# Architecture Decisions

Lightweight ADRs. Each entry: context → decision → consequences.

---

## ADR-001 — Reuse paper-companion's backend instead of a standalone API

**Date:** 2026-04-27
**Status:** Accepted

**Context.** Could build a dedicated FastAPI service for this mobile app.

**Decision.** Reuse [paper-companion](https://github.com/Aboubekrin999/paper-companion)'s FastAPI service. Add one new endpoint family (`/flashcards`) and one table (`flashcards`).

**Why.**
- Same user, same papers, same auth — duplicating infrastructure would be wasteful.
- Validates the original [paper-companion ADR-001](https://github.com/Aboubekrin999/paper-companion/blob/main/docs/DECISIONS.md) decision to split web/api.
- Mobile development time stays focused on the mobile-specific work (UI, SRS, offline).

**Consequences.** Mobile project's progress is gated on paper-companion v1 being live. Acceptable — that's the planned dependency order.

---

## ADR-002 — Expo (managed workflow) over bare React Native

**Date:** 2026-04-27
**Status:** Accepted

**Context.** Bare React Native gives more control. Expo managed workflow trades some control for a vastly simpler dev cycle.

**Decision.** Expo SDK 54+ managed workflow.

**Why.**
- ~10 hr/week budget — Expo's hot reload + EAS Build saves days vs. local Xcode/Android Studio.
- v1 has no native modules outside Expo's prebuilt list.
- Author has an existing Expo build pipeline (`jourfi` developer account) for distribution.

**Consequences.** If a native module need emerges, can `expo prebuild` to bare workflow. Reversible.

---

## ADR-003 — SM-2 spaced repetition, not FSRS

**Date:** 2026-04-27
**Status:** Accepted

**Context.** FSRS (Free Spaced Repetition Scheduler) is more modern and demonstrably better than SM-2 in head-to-head studies. Anki's default is moving toward FSRS.

**Decision.** Implement SM-2 first.

**Why.**
- v1 success criterion is "user opens the app daily and reviews." A *good enough* algorithm with a shipped app beats a *better* algorithm in a planning doc.
- SM-2 is ~50 lines of TypeScript. FSRS requires parameter optimization on per-user history.
- Migration to FSRS later is a pure scheduler swap; card data is unchanged.

**Consequences.** v1 reviews will be slightly less efficient than they could be. Acceptable trade for ship velocity.

---

## ADR-004 — TanStack Query, not Redux/Zustand for server state

**Date:** 2026-04-27
**Status:** Accepted

**Context.** Multiple state libraries available (Redux Toolkit, Zustand, Jotai, TanStack Query).

**Decision.** TanStack Query for server state. Plain React state for UI state. No global state library.

**Why.**
- Most state in this app is *server* state (papers, cards, due reviews). TanStack Query handles caching, refetching, optimistic updates, offline persistence with `persistQueryClient`.
- Adding Redux/Zustand for the rare bit of UI state is overkill.

**Consequences.** If app-wide UI state needs grow (theme, user preferences), revisit. Not expected for v1.

---

## ADR-005 — SQLite for offline card cache

**Date:** 2026-04-27
**Status:** Accepted

**Context.** Could store the day's review cards in AsyncStorage as JSON.

**Decision.** Use `expo-sqlite` for cards; AsyncStorage only for small key/value (auth token, settings).

**Why.**
- Cards have structure (paper_id, ease_factor, interval, due_date) and need queries ("give me today's due cards").
- SQLite handles this natively. AsyncStorage would force in-memory filtering on every load.
- `expo-sqlite` has zero native-build pain.

**Consequences.** One more dependency. Worth it for query ergonomics.
