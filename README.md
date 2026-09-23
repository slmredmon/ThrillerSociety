# Thriller Society

A dark-thriller-themed book club and mystery-tracking web app. Readers open a "case file" for whatever thriller or mystery they're currently reading, log a prime suspect and theory as they go, predict twists, and get a scored "verdict" comparing their detective work against the book's actual ending.

## Features

- **Sign up** with an email, display name, and your first book title/author — goes straight into building that first case.
- **Case wizard:** enter the book, mark how far you've read (0/25/50/75%), name your prime suspect and theory, and add as many additional suspects as you like, each with a theory and a 1–10 trust score.
- **50% lock-in:** once a case crosses the halfway mark, you're prompted to lock in one final suspect and theory. After that, the suspect board is frozen — no edits, no new names — so the record of your reasoning can't be revised in hindsight.
- **What's Really Going On:** a free-text prompt separate from the suspect board, for theories that aren't about naming one culprit (conspiracies, frame jobs, unreliable narrators, twins, etc.).
- **Predicted Twists:** log up to three twists you're calling before the book confirms or denies them.
- **Closing a case:** once you finish the book, enter the real culprit. The app builds a "verdict" — a solved/escaped banner, a list of strengths and weaknesses in your detective work, and summary stats (suspects considered, average trust score, whether you stuck with your first pick).
- **Dashboard:** active and closed cases at a glance, with totals, in-progress count, solved/escaped counts, and an overall solve rate with a visual outcome bar.
- **Case history:** every case you've ever opened, sorted by most recently updated.
- **Real book covers:** each case automatically looks up a real cover image for the title/author via the Google Books API, falling back to Open Library if needed. If no match is found (or the image fails to load), the app's own generated placeholder cover stays in place. Lookups never block the app from rendering.
- **Account settings:** view your email, name, member-since date, and storage mode; log out, deactivate (reversible), or permanently delete your account and all case data.

## Files

| File | Purpose |
|---|---|
| `index.html` | Page shell — loads fonts, the Firebase SDK, `firebase-config.js`, `style.css`, and `script.js`, and holds the empty `#app` mount point. |
| `style.css` | All visual styling (dark thriller theme, layout, components, responsive rules). |
| `script.js` | The entire application: rendering, state, the case wizard, the verdict engine, book-cover lookup, authentication, and data persistence. |
| `logo.png` | The Thriller Society logo, used in the top bar and on the sign-up screen. |
| `firebase-config.js` | Your Firebase project's client config. Ships with placeholder values — see **Connecting Firebase** below. |
| `.gitignore` | Keeps genuinely sensitive files (Admin SDK keys, `.env` files) and editor/OS cruft out of the repo. |
| `README.md` | This file. |

The app has no build step and no framework — just static files plus the Firebase SDK loaded from Google's CDN in `index.html`. Open `index.html` in a browser (or serve the folder with any static file server) and it runs.

## Data & storage

This app stores real, persistent entries — but which kind depends on whether Firebase is connected:

- **Without Firebase set up** (the state it ships in): `script.js` detects that `firebase-config.js` still has placeholder values and automatically runs in **local mode**. Signing up just writes your profile and cases to this browser's `localStorage`. It persists across reloads, but only on this one device/browser — nothing syncs, nothing is backed up, and a "local" badge appears in the top bar to make this visible. Anyone else opening the site gets their own empty local copy.
- **With Firebase connected** (see below): the app switches to **cloud mode** automatically. Sign-up creates a real account (Firebase Authentication, email + password) and all profile/case data is written to Cloud Firestore under that account. The same login works from any browser or device, and the sign-up screen gains a password field plus a "Log in" link for returning users; Settings gains a "Log out" button.

Every read and write in the app goes through one object near the top of `script.js` called `DAL` (`getProfile`, `setProfile`, `listCases`, `getCase`, `createCase`, `updateCase`, `deleteCase`, `wipeEverything`) — that's the only place data access happens, whichever mode is active.

## Connecting Firebase

Firebase's free tier ("Spark plan") is enough to run this app for a normal amount of personal or small-project use.

1. **Create a project.** Go to [console.firebase.google.com](https://console.firebase.google.com), create a new project (Google Analytics is optional, you can skip it).
2. **Register a web app.** In the project overview, click the `</>` (web) icon to add a web app. Give it any nickname — you don't need Firebase Hosting for this. It will show you a config object (`apiKey`, `authDomain`, `projectId`, etc.).
3. **Fill in `firebase-config.js`.** Open that file in this repo and replace the placeholder strings with the real values from step 2.
4. **Turn on email/password sign-in.** In the console, go to **Build → Authentication → Sign-in method**, and enable the **Email/Password** provider.
5. **Create a Firestore database.** Go to **Build → Firestore Database → Create database**. Start in production mode and pick any region.
6. **Set security rules.** Still in Firestore, open the **Rules** tab and replace the contents with:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /users/{userId} {
         allow read, write: if request.auth != null && request.auth.uid == userId;
         match /cases/{caseId} {
           allow read, write: if request.auth != null && request.auth.uid == userId;
         }
       }
     }
   }
   ```

   This ensures each signed-in user can only ever read or write their own profile and cases — nobody else's.

7. **Reload the site.** The sign-up form will now show a password field and a "Log in" link, and every account is stored in Firestore instead of the browser.

A couple of things worth knowing:

- The values in `firebase-config.js` are not secret — Firebase's client-side config is meant to be public. Your data is protected by the security rules in step 6, not by hiding this file. What *is* secret is an Admin SDK service-account key, which this app never needs and which `.gitignore` already guards against if you add one later.
- "Delete everything" in Settings wipes all of that account's Firestore data and signs them out, but — since this app never uses the Admin SDK — it doesn't delete the underlying Firebase Authentication login itself. That would require a small Cloud Function; it's a reasonable next step if you need it, but isn't included here.

## Book cover lookups

`script.js` calls two free, public, keyless APIs directly from the browser:

- `https://www.googleapis.com/books/v1/volumes` (primary)
- `https://openlibrary.org/search.json` and `https://covers.openlibrary.org` (fallback)

No API key is required for either, so there's nothing to add to a `.env` file for this feature to work. If you later add any service that *does* require a key (analytics, a real backend, etc.), keep it out of `script.js` and out of version control — that's what `.gitignore` is set up to protect.

## Deploying

Since this is a static site with no build step, it deploys anywhere that serves static files as-is, for example:

- **GitHub Pages:** push this repo, then enable Pages on the `main` branch (root folder) in the repo's Settings.
- **Netlify / Vercel / Cloudflare Pages:** point any of these at the repo with no build command and `/ ` (root) as the publish directory.

## License

Copyright © 2026. All rights reserved.
