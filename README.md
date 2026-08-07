# ⚽ Pitch Perfect — Football Trivia & World Cup History

A mobile-first football trivia game and World Cup browser. Single-file web app —
no build step, no dependencies, no external requests.

**▶️ Play: https://astki-kr.github.io/fifawc/**

## Install on your iPad / iPhone

1. Open the link above in **Safari**.
2. Tap the **Share** button, then **Add to Home Screen**.
3. It launches full-screen with its own icon, like a native app.

On **Android**, open the link in Chrome and tap **Install app** / **Add to Home screen**.

## Features

### 🎯 Daily Challenge
One "guess the country" question per day, drawn from a bank of World Cup 2026 facts.
The question is chosen by a **date-seeded** pseudo-random generator, so everyone
opening the app on the same calendar day gets the same question — with no server.
Answers are matched case-insensitively, with accent stripping (`España` → Spain),
common aliases (`Holland`, `USA`, `Korea Republic`) and one-typo tolerance.
Tracks your streak, best streak and total solved.

### 🌎 World Cup 2026
Complete results for **all 104 matches** of the Canada / Mexico / USA tournament:

- Group stage — standings tables for all 12 groups (computed from the match data)
  plus every group match
- Knockouts — Round of 32 through the Final, with extra-time and penalty scores
- Tap any match for the full goal timeline, with minutes, penalties and own goals
- Top scorers, the four golden awards, and flags for all 48 finalists

Champion: **🇪🇸 Spain**, beating Argentina 1–0 after extra time (Ferran Torres, 106').

### 🏆 World Cup History
All 22 tournaments from 1930 to 2022 — host, winner, runner-up, final score,
top scorer, goals and attendance figures — searchable and filterable, with a
ranked breakdown of titles won by country.

## Data sources

- **2026 results** — the public-domain [openfootball/worldcup.json](https://github.com/openfootball/worldcup.json)
  dataset, cross-checked against the official FIFA record (104 matches, 308 goals).
- **1930–2022** — compiled tournament records embedded directly in the page.

Both datasets are plain JSON-shaped constants inside `index.html`, so either can be
swapped for a `fetch()` without touching any other code.

## Files

| File | Purpose |
|------|---------|
| `index.html` | The entire app — HTML, CSS and JS in one file |
| `manifest.json` | PWA manifest (Android install, standalone display) |
| `icon-*.png` | Home-screen icons |

## Architecture notes

All persistence goes through a `Store` module sitting behind an **async storage
adapter**. It currently writes to `localStorage`; moving to a cloud database is a
single call:

```js
Store.use({
  async load()      { return (await db.get(uid)).data; },
  async save(state) { await db.set(uid, state); }
});
```

Nothing else in the app touches storage directly.
