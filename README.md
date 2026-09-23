[README-full-version.md](https://github.com/user-attachments/files/32578402/README-full-version.md)
# Thriller Society — Full Version

A dark-thriller-themed book club and mystery-tracking web app. Readers open a "case file" for whatever thriller or mystery they're currently reading, log a prime suspect and theory as they go, predict twists, and get a scored "verdict" comparing their detective work against the book's actual ending.

**Live artifact:** https://claude.ai/artifact/YTCUu9aD8hNSwWQmYmjjcz

## What it does

- **Sign up** with an email, display name, and your first book title/author — goes straight into building that first case.
- **Case wizard:** enter the book, mark how far you've read (0/25/50/75%), name your prime suspect and theory, and add as many additional suspects as you like, each with a theory and a 1–10 trust score.
- **50% lock-in:** once a case crosses the halfway mark, you're prompted to lock in one final suspect and theory. After that, the suspect board is frozen — no edits, no new names — so the record of your reasoning can't be revised in hindsight.
- **What's Really Going On:** a free-text prompt separate from the suspect board, for theories that aren't about naming one culprit (conspiracies, frame jobs, unreliable narrators, twins, etc.).
- **Predicted Twists:** log up to three twists you're calling before the book confirms or denies them.
- **Closing a case:** once you finish the book, enter the real culprit. The app builds a "verdict" — a solved/escaped banner, a list of strengths and weaknesses in your detective work (did you have the right suspect but doubt yourself? did you stay loyal to your first instinct? how wide was your suspect net?), and summary stats (suspects considered, average trust score, whether you stuck with your first pick).
- **Dashboard:** active and closed cases at a glance, with totals, in-progress count, solved/escaped counts, and an overall solve rate with a visual outcome bar.
- **Case history:** every case you've ever opened, sorted by most recently updated.
- **Real book covers:** each case automatically looks up a real cover image for the title/author via the Google Books API, falling back to Open Library if needed. If no match is found (or the image fails to load), the app's own generated placeholder cover — a color-coded design with a file number, title, and author — stays in place. This never blocks the app from rendering.
- **Account settings:** view your email, name, member-since date, and storage mode; deactivate (reversible) or permanently delete your account and all case data.
- **No case limit** — this is the full, unrestricted version.

## Storage

- If you're signed in through a Claude organization account, your profile and cases sync to the cloud and follow you across devices.
- Otherwise, everything is saved to `localStorage` in your current browser only (a small "local" badge appears in the top bar in this mode). Clearing your browser data or switching browsers will lose this data.

## Notes

- All content, layout, and design are original to this app; the real-cover lookup only pulls a small thumbnail image from public book-data APIs and never alters or claims authorship over the source book.
- Copyright © 2026. All rights reserved.
