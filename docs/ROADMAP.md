# 2-Week Roadmap to v1

Target: TestFlight build that signs in, lists papers, generates and reviews flashcards.
Budget: ~10 hours per week, starting after [bilingual-section-classifier](https://github.com/Aboubekrin999/bilingual-section-classifier) ships (early June 2026).

Compressed timeline because the backend is already done. Mobile work is purely UI + SRS + offline.

---

## Week 1 — App shell, auth, library, card generation
*Tentative: June 1 – June 7*

- [ ] `npx create-expo-app` with Expo Router + TypeScript template
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

**Checkpoint.** Recruiter clicks the README, scans a TestFlight QR, signs in with magic link, does a real review session. v1 done.

---

## After v1

- FSRS algorithm (replace SM-2 — see [ADR-003](DECISIONS.md#adr-003))
- Card editing
- Tablet layout
- Quiz mode (multiple choice from card content)
- Integration with the bilingual-section-classifier: generate different card types per section (definition cards from Methods, claim cards from Results)
