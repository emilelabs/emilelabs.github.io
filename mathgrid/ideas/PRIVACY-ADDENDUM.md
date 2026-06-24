# Privacy addendum — Idea Board (add to mathgrid/privacy/ at go-live)

The current Math Grid privacy policy scopes Firebase narrowly to Online Battle. The Idea
Board is a **new, separate data activity** and must be disclosed before it goes live.
Suggested section to add (matches the existing page's plain, parent-friendly tone):

---

## Idea Board

Our website has an optional **Idea Board** (`/mathgrid/ideas/`) where anyone can suggest
features and upvote others' ideas. It is part of the **website**, not the app, and your
child does not encounter it while playing.

- **What's collected:** only the **text of an idea you choose to submit**. Submissions are
  **public** — please don't include names, contact details, or any personal information.
- **No account, no name, no email.** To prevent spam, your browser signs in to Firebase
  **anonymously** (a random, throwaway identifier that isn't linked to you and that we
  cannot use to identify you). Your upvotes are remembered **only on your own device**
  (in your browser's local storage) and are not sent to us as personal data.
- **Where it's stored:** Google Firebase (Realtime Database), over an encrypted connection,
  the same provider used for the app's online battles.
- **No tracking, no ads, no analytics** — consistent with the rest of Math Grid.
- **Moderation & removal:** we may hide ideas that are inappropriate. To request removal of
  something you posted, email **support@emilelabs.com**.

---

Also update the third-party / data-types table on the privacy page if it enumerates
Firebase usage, to note "Idea Board — public idea text, anonymous auth."
