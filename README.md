# Paper Flashcards

> Mobile companion to [paper-companion](https://github.com/Aboubekrin999/paper-companion). Pulls papers from your library, auto-generates spaced-repetition flashcards, schedules daily review. iOS + Android.

**Status:** Planning — build starts June 2026, after Project 2 ships

---

## The problem

Reading a paper deeply once isn't enough. Active recall is the most-evidenced study technique, but extracting flashcard-worthy claims is tedious. Existing flashcard apps (Anki, Quizlet) require manual card creation, which is the friction that kills the habit.

This app generates cards from your saved papers automatically, then schedules them with a spaced-repetition algorithm. You open the app, you do today's review, you close it. That's it.

## Who it's for

Same user as [paper-companion](https://github.com/Aboubekrin999/paper-companion): students and researchers reading 5+ papers per week who want their reading to *stick* without the overhead of manual card creation.

Built first for the author's own use during AI master's coursework.

## What v1 does

**In scope:**
- Sign in with the same Supabase account as [paper-companion](https://github.com/Aboubekrin999/paper-companion)
- Pull papers from the user's library
- Generate 5–10 flashcards per paper (key claims, definitions, results) — done once per paper, cached server-side
- Daily review session using SM-2 spaced repetition
- Offline mode — review the day's cached cards without network
- Progress tracking: streak, due-card count, papers studied

**Explicitly out for v1:**
- Manual card editing (auto-generated only; user rates "easy/good/hard/again")
- Sharing decks
- Tablet-optimized layout
- Web version (use [paper-companion](https://github.com/Aboubekrin999/paper-companion) on desktop)

## Why React Native + Expo

| | |
|---|---|
| One codebase, iOS + Android | No duplicate work |
| Expo Router | File-based routing matches Next.js mental model — same author, same patterns |
| EAS Build | TestFlight + Internal Track distribution without local Xcode hell |
| Hot reload | 10-hour-per-week budget — fast iteration matters |

## What it reuses from paper-companion

This is **the** payoff for splitting the [paper-companion](https://github.com/Aboubekrin999/paper-companion) backend out as a standalone FastAPI service:

- Supabase auth — same magic-link login
- The `papers` and `chunks` tables — the mobile app reads, doesn't re-ingest
- A new `/flashcards` endpoint on the FastAPI service, generated cards persisted to a `flashcards` table

Building Project 3 on Project 1's backend is what makes a 2-week timeline realistic.

## Tech stack

| Layer | Choice |
|---|---|
| Framework | Expo SDK 54+ (React Native) |
| Language | TypeScript |
| Routing | Expo Router (file-based) |
| Data fetching | TanStack Query (React Query) |
| Auth | `@supabase/supabase-js` |
| Local storage | AsyncStorage + SQLite (`expo-sqlite`) for offline cards |
| SRS algorithm | SM-2 (Anki-style) |
| Distribution | EAS Build → TestFlight (iOS) + Internal Track (Android) |

Detailed reasoning in [`docs/DECISIONS.md`](docs/DECISIONS.md).

## Roadmap

2-week plan in [`docs/ROADMAP.md`](docs/ROADMAP.md). Compressed because the backend is reused from [paper-companion](https://github.com/Aboubekrin999/paper-companion).

## Local development

> Documented after the Expo scaffold lands (week 1, day 1).

## Author

**Aboubekrin Mohamed Salem** — AI Master's student. Building the third piece of a connected portfolio: web (paper-companion) + research (bilingual-section-classifier) + mobile (this).

GitHub: [@Aboubekrin999](https://github.com/Aboubekrin999)
