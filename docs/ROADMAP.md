# 2-Week Roadmap to v1

Target: TestFlight build that signs in, lists papers, generates and reviews flashcards.
> **This is the original plan, written in April 2026.** No feature work has
> started. For the real state of the repo, see the
> [project README](../README.md#whats-built-today).

Planned around roughly 10 hours a week, starting after [bilingual-section-classifier](https://github.com/Aboubekrin999/bilingual-section-classifier) ships.

The timeline is compressed because the backend is reused rather than rebuilt. Mobile work is UI, the SRS scheduler, and offline support.

---

## Week 1 — App shell, auth, library, card generation
*Tentative: June 1 – June 7*

- [x] `npx create-expo-app` with Expo Router + TypeScript template
- [ ] Tailwind-equivalent styling (NativeWind) configured
- [ ] Supabase magic-link auth flow (sign in / sign out / session persistence)
- [ ] Library screen: pulls user's papers from paper-companion's API, list view
- [ ] Card generation: tap a paper → POST `/flashcards/generate` → store generated cards in SQLite
- [ ] Add the `/flashcards` endpoint family to the paper-companion API (separate PR there)

**Checkpoint.** Sign in on a real phone via Expo Go, see papers, tap one, see 5–10 cards generated and stored locally.

---

## Week 2 — SRS, daily review, offline, ship
*Tentative: June 8 – June 14*

- [ ] SM-2 algorithm in `lib/srs.ts` with unit tests
- [ ] Daily review screen: due card count → swipe through cards → rate again/hard/good/easy
- [ ] Offline-first: SQLite is source of truth; sync deltas to server when online
- [ ] Streak tracking + due-card badge on the home screen
- [ ] App icon + splash screen
- [ ] EAS Build → TestFlight (iOS) + Internal Track (Android)
- [ ] README updated with TestFlight invite link + 30-second screen recording

**Checkpoint.** A tester scans the TestFlight QR, signs in with a magic link, and completes a real review session. v1 done.

---

## After v1

- FSRS algorithm (replace SM-2 — see [ADR-003](DECISIONS.md#adr-003))
- Card editing
- Tablet layout
- Quiz mode (multiple choice from card content)
- Integration with the bilingual-section-classifier: generate different card types per section (definition cards from Methods, claim cards from Results)
