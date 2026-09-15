# 🦑 Squids Next Door Tracker

A real-time group habit and goal tracker built for a small squad to keep each other accountable. Everyone can set a daily or weekly goal, "ink" it when they complete it, and watch the whole squad's progress update live.

**Live app:** [evelasquez9.github.io/Squids-Next-Door-Tracker](https://evelasquez9.github.io/Squids-Next-Door-Tracker/)

## Features

- **Daily or weekly goals** — each member picks a goal type that fits what they're tracking
- **Dual goals** — members can track two goals at once (e.g. reading + working out)
- **Streaks** — daily goals build a day streak, weekly goals build a week streak, with milestone badges at 7, 14, and 30 days
- **Real-time sync** — when anyone inks a goal, every other open tab updates instantly, no refresh needed
- **Weekly calendar view** — a Mon–Sun dot grid shows exactly which days were completed, missed, or voided (after a weekly goal target is already hit)
- **Leaderboard ranking** — squad members are ranked by "effective days" (weekly streaks are weighted by their weekly target, so a 4-day/week goal counts more than a 1-day/week goal)
- **Squid of Shame** — a weekly callout for whoever missed the most with the lowest streak
- **PIN protection** — each member sets a 4-digit PIN so only they can ink or edit their own card
- **+yesterday** — a grace-period button to log a forgotten day, disabled once that day rolls into a new week
- **Underwater theme** — animated caustic light rays, bubbles, and an ink-splat sound/animation on every check-in

## Tech Stack

- **Frontend:** Single-page vanilla HTML/CSS/JavaScript — no build step, no framework
- **Backend:** [Firebase Firestore](https://firebase.google.com/docs/firestore) — all squad data lives in one Firestore document, synced live via `onSnapshot` and updated safely via `runTransaction` so concurrent check-ins never overwrite each other
- **Hosting:** [GitHub Pages](https://pages.github.com/)
- **Audio:** Ink splat sound effects embedded directly as base64 data URIs — no external audio files

## How it works

1. All squad data (members, goals, streaks, weekly logs) lives in a single Firestore document
2. A real-time listener pushes any change to every open device within a second
3. When someone inks a goal, a Firestore transaction reads the current state, applies the update, and writes it back atomically — so two people inking at the same moment can't clobber each other's data
4. Daily and weekly resets run client-side on load: missed days reset streaks, and a new week clears the calendar and miss counts

## Security note

This is a small, private app meant for a trusted group of friends. Firestore's security rules are currently permissive (`allow read, write: if true`), and PIN verification happens client-side rather than through Firebase Authentication. That's an intentional tradeoff for simplicity at this scale — not something you'd want for a public-facing or sensitive app.

## Local development

This is a static single HTML file — just open `index.html` in a browser, or serve it with any static file server:

```bash
python3 -m http.server 8000
```

You'll need your own Firebase project and Firestore database, with your config swapped into the `firebaseConfig` object in `index.html`.

---

🤖 Built with [Claude Code](https://claude.com/claude-code)
