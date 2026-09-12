# Paper Flashcards

[![CI](https://github.com/Aboubekrin999/paper-flashcards/actions/workflows/ci.yml/badge.svg)](https://github.com/Aboubekrin999/paper-flashcards/actions/workflows/ci.yml)

> Mobile companion to [paper-companion](https://github.com/Aboubekrin999/paper-companion). Pulls papers from your library, auto-generates spaced-repetition flashcards, schedules daily review. iOS + Android.

**Status:** Expo scaffold and CI only — no feature work has started. This is the least-built of the three repos; see [What's built today](#whats-built-today).

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
| Hot reload | Fast iteration on a small time budget |

## What it reuses from paper-companion

This is **the** payoff for splitting the [paper-companion](https://github.com/Aboubekrin999/paper-companion) backend out as a standalone FastAPI service:

- Supabase auth — same magic-link login
- The `papers` and `chunks` tables — the mobile app reads, doesn't re-ingest
- A new `/flashcards` endpoint on the FastAPI service, generated cards persisted to a `flashcards` table

Building this app on paper-companion's existing backend is what makes a two-week timeline realistic.

## What's built today

Honest state of the repo, so you can tell the code from the plan.

| Area | State |
|---|---|
| **Expo SDK 54 scaffold** with Expo Router and TypeScript | Built — typecheck clean |
| **CI** — typecheck on every PR | Built |
| **Architecture decisions** in [`docs/DECISIONS.md`](docs/DECISIONS.md) | Written |
| Supabase auth, library sync, card generation, SM-2 review loop, offline mode | Not built |

The `app/` directory is still close to the Expo template. Nothing in "What v1 does" above is implemented yet — it is the plan, and it is labelled as such.

Work paused in May 2026 while client delivery took priority. The backend dependency ([paper-companion](https://github.com/Aboubekrin999/paper-companion)'s `/flashcards` endpoint) is also still to be built.

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

Two-week plan in [`docs/ROADMAP.md`](docs/ROADMAP.md), compressed because the backend is reused from [paper-companion](https://github.com/Aboubekrin999/paper-companion) rather than rebuilt.

## Local development

```bash
npm ci
npm run typecheck     # clean
npm run lint
npm start             # Expo dev server — scan the QR with Expo Go
```

`npm run ios` / `npm run android` open a simulator directly. No environment variables are needed yet; `.env.example` lists the Supabase keys the auth work will require.

## Author

**Aboubekrin Mohamed Salem** — software engineer and MSc AI candidate, Paris. The mobile surface of a connected three-repo system: web ([paper-companion](https://github.com/Aboubekrin999/paper-companion)), research ([bilingual-section-classifier](https://github.com/Aboubekrin999/bilingual-section-classifier)), and this.

GitHub: [@Aboubekrin999](https://github.com/Aboubekrin999)
