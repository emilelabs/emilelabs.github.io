# Math Grid Idea Board — setup

The board page (`mathgrid/ideas/index.html`) is built and brand-matched. It runs on
**Firebase Realtime Database** (already enabled for Math Grid's online battle — no new
product needed) with **anonymous auth** (invisible to users — satisfies "no login").
Until the web config is pasted it shows a friendly "almost ready" banner and stays inert,
so it is safe to deploy in advance.

## 1. Register a Web app + paste the config (raryosu / owner)

Firebase console → **Math Grid project** → ⚙ **Project settings** → **Your apps** →
**Add app → Web** → nickname `mathgrid-web` → **Register app** → copy the `firebaseConfig`.

In `index.html`, replace the placeholder block (the values containing `PASTE_`):

```js
const firebaseConfig = {
  apiKey: "…",
  authDomain: "<project>.firebaseapp.com",
  databaseURL: "https://<project>-default-rtdb.firebaseio.com",   // REQUIRED (Realtime DB)
  projectId: "…",
  appId: "…"
};
```

These values are public by design (safe to commit). The moment real values are in,
the board goes live. Commit + push → GitHub Pages deploys.

> Anonymous auth must be enabled: Firebase console → **Authentication → Sign-in method →
> Anonymous → Enable** (Math Grid already uses it for battles, so it should be on).

## 2. Realtime Database security rules (deploy in the console → Realtime Database → Rules)

These keep the board **publicly readable**, allow **anyone (anonymously authed) to post a
valid idea** and **increment a vote by exactly 1**, and **forbid** editing text, deleting,
or changing the `hidden` flag from the client. Moderation (hiding bad entries) is done by
you in the console — that's the "post-moderation" model.

```json
{
  "rules": {
    "mathgrid_ideas": {
      ".read": true,
      "$id": {
        ".write": "auth != null && !data.exists()",
        ".validate": "newData.hasChildren(['text','votes','hidden','createdAt'])",
        "text":      { ".validate": "newData.isString() && newData.val().length >= 3 && newData.val().length <= 140" },
        "votes": {
          ".write":    "auth != null && newData.isNumber() && newData.val() === (data.val() || 0) + 1",
          ".validate": "newData.isNumber() && newData.val() >= 0"
        },
        "hidden":    { ".validate": "newData.isBoolean() && newData.val() === false" },
        "createdAt": { ".validate": "newData.val() === now" },
        "$other":    { ".validate": false }
      }
    }
  }
}
```

Notes:
- **Create**: client may only create a node where it doesn't already exist, with the four
  required fields, `text` 3–140 chars, `votes` 0, `hidden` false, `createdAt` = server time.
- **Vote**: the only field a client may change on an existing idea is `votes`, and only by
  +1 (one path: `mathgrid_ideas/$id/votes`). One-vote-per-device is enforced client-side
  via `localStorage` (a determined user can still re-vote by clearing storage — acceptable
  for a low-stakes idea board; tighten later with per-uid vote records if needed).
- **Hide/moderate**: set `hidden: true` on an entry in the console. The board hides it
  immediately for everyone. Clients cannot set or clear `hidden`.
- **Abuse**: client-side has a profanity gate + 45s cooldown + ~12/day soft cap + honeypot.
  The rules are the real backstop; the profanity list lives in `index.html` (`BLOCKED`).

## 3. Go-live checklist
- [ ] Web config pasted, anonymous auth on, rules deployed.
- [ ] Add the **privacy section** (see `PRIVACY-ADDENDUM.md`) to `mathgrid/privacy/`.
- [ ] Link the board from the site (add to footer / a "Ideas" nav item) once you want it discoverable.
- [ ] Post a couple of seed ideas so the board isn't empty at launch.
