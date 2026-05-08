# CJRC Grand Challenges

Workshop tool for the CJRC research agenda day. A single-page web app, Firebase-backed, hosted on GitHub Pages.

Live at: https://[YOUR-GITHUB-USERNAME].github.io/cjrc-grand-challenges/

## What it does

**Morning session.** Four groups (1–4) work in pairs (1+2 on Half A, 3+4 on Half B) on a deterministic random split of the 57 questions taken forward. Each group can:

- Edit the wording of any question in their assigned half. Edits sync live across the paired group.
- View the paired group's progress in read-only mode.
- Revive up to 3 questions from the rejected pool of 57.
- Tag each question with one of four discipline themes for the afternoon.

**Afternoon session.** Four discipline groups work on questions tagged with their theme:

- CJ in practice
- Design, pairings & design
- Judges, multi-criteria & AI
- Learning, cognition & attention

Each group cuts roughly half their questions. A running counter shows the target.

**Export.** A facilitator can download a CSV of everything (current wording, edit history, theme tags, revival status, cuts) at any time.

## Repo contents

- `index.html` is the entire app. Open it in a browser, no build step.
- `firestore.rules` goes into the Firebase console (see Firebase setup below).
- `.nojekyll` tells GitHub Pages to serve files raw rather than running Jekyll over them. Without this, Jekyll's template engine can mangle the JavaScript curly braces in `index.html`.

## Enable GitHub Pages

After pushing this repo, go to **Settings > Pages** in the GitHub web UI. Under "Source", pick **Deploy from a branch**, select `main` and `/ (root)`, and click Save. Within about a minute the site will be live at the URL shown at the top of the Pages settings page.

## Firebase setup

The Firestore project `cjrc-gc` is already configured in `index.html`. Before workshop day, paste the contents of `firestore.rules` into the Firebase console under **Firestore Database > Rules** and click Publish. This replaces the default test-mode rules (which expire after 30 days) with proper validation:

- Reads and writes allowed to anyone (the workshop is public-by-design)
- Edit length capped at 5000 characters
- Revivals capped at 3 per group
- Auto-expires on 31 December 2026 (after which data becomes read-only forever)

If you want tighter security, add a shared passphrase check before any Firestore write.

## Pre-workshop checklist

1. Replace the `REJECTED` array in `index.html` with the actual wording of the 57 rejected questions. The current placeholders are dummy text generated for prototyping.
2. Confirm the four discipline themes match what you've briefed participants.
3. Test the full flow on two different devices: sign in as Group 1 on one and Group 2 on the other, edit a question, confirm it appears live on the other device.
4. Sign out, sign back in as a discipline group, confirm tagged questions appear in the cull view.
5. Publish the Firestore rules.
6. Send the URL to participants the morning of the workshop.

## During the workshop

Keep a browser tab open to the **Export** view yourself. Hit Download CSV at the end of each session as a backup.

## After the workshop

Edit `firestore.rules` to change all `allow write` lines to `allow write: if false;` and republish. This freezes the data permanently while keeping it readable.
