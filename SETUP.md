# Setting up Samartha Sweets & Mess ERP as a shared, installable app

This turns your app into a real website that every family member can open,
install on their phone's home screen, and see the same live data on —
updates from one phone appear on everyone else's within a second or two.

It takes about 15–20 minutes the first time. You only do this once.

---

## Part 1 — Create the free live database (Firebase)

1. Go to **https://console.firebase.google.com** and sign in with any Google account.
2. Click **"Add project"** (or "Create a project"). Name it anything, e.g. `samartha-erp`.
3. You can turn OFF Google Analytics for this project — not needed. Click **Create project**, then **Continue** once it's ready.
4. In the left sidebar, click **Build → Firestore Database**.
5. Click **Create database**.
   - Choose **Start in test mode** (we'll tighten this in Part 2).
   - Pick the location closest to you (any Asia region is fine) → **Enable**.
6. Now click the **gear icon (⚙️) → Project settings** (top-left, next to "Project Overview").
7. Scroll down to **"Your apps"**. Click the **`</>`** (web) icon to add a web app.
8. Give it a nickname (e.g. "erp-web") → **Register app**. Do NOT check "Firebase Hosting."
9. You'll now see a code block that looks like this:

   ```js
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "samartha-erp.firebaseapp.com",
     projectId: "samartha-erp",
     storageBucket: "samartha-erp.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef123456"
   };
   ```

   **Copy these 6 values.**

10. Open **`index.html`** from this folder in any text editor (even Notepad).
    Find this section near the top of the `<script>` block:

    ```js
    const FIREBASE_CONFIG = {
      apiKey: "PASTE_API_KEY_HERE",
      authDomain: "PASTE_PROJECT_ID.firebaseapp.com",
      projectId: "PASTE_PROJECT_ID",
      storageBucket: "PASTE_PROJECT_ID.appspot.com",
      messagingSenderId: "PASTE_SENDER_ID",
      appId: "PASTE_APP_ID"
    };
    ```

    Replace each `PASTE_...` value with what you copied in step 9. Save the file.

---

## Part 2 — Lock down the database (important — do this)

By default "test mode" lets **anyone on the internet** read/write your data for 30 days,
then it stops working entirely. Fix both problems at once:

1. In Firebase Console → **Firestore Database → Rules** tab.
2. Replace the contents with:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /messShared/{doc} {
         allow read, write: if true;
       }
     }
   }
   ```

   This keeps it simple (no login system) but only works because your app already
   has its own PIN lock (set one in the app's **Settings** tab!). Anyone with your
   website link *and* your Firestore project ID could technically still reach the
   database directly — for a family mess/sweets business this level of risk is
   normally fine, but don't share the link publicly.

3. Click **Publish**.

---

## Part 3 — Put it on the internet (GitHub Pages)

1. Go to **https://github.com** and log in (you said you already have an account).
2. Click the **+** icon (top-right) → **New repository**.
3. Name it e.g. `samartha-erp` → set it to **Public** → **Create repository**.
4. On the new repo page, click **"uploading an existing file"** (or Add file → Upload files).
5. Drag in **all the files from this folder**: `index.html`, `manifest.json`, `sw.js`,
   and the whole `icons` folder (with both PNGs inside).
6. Scroll down, click **Commit changes**.
7. Go to the repo's **Settings** tab → **Pages** (left sidebar).
8. Under "Build and deployment" → Source: **Deploy from a branch**.
   Branch: **main**, folder: **/ (root)** → **Save**.
9. Wait 1–2 minutes, then refresh — GitHub will show your live link, like:
   `https://yourusername.github.io/samartha-erp/`

That link is your app. Anyone with it can open it in a phone browser.

---

## Part 4 — Install it on each family member's phone

**On Android (Chrome):**
1. Open the link above in Chrome.
2. Tap the **⋮** menu → **"Add to Home screen"** → **Install**.
3. The app icon (your logo) now appears on the home screen like a normal app.

**On iPhone (Safari):**
1. Open the link in Safari (must be Safari, not Chrome, for this to work).
2. Tap the **Share** icon (square with an arrow) → **"Add to Home Screen"** → **Add**.

Do this on every family member's phone using the same link. Everyone now sees
the same live data — mark a payment on one phone, it appears on all the others
within a couple of seconds.

---

## Notes

- **If two people edit at the exact same second**, the second save can overwrite
  the first (this app doesn't merge conflicting edits). In practice, for a small
  family operation this almost never causes real problems.
- **To update the app later** (e.g. if I make more changes for you), just replace
  `index.html` in the GitHub repo with the new version — the Upload files button
  works the same way. Your data stays in Firebase, untouched.
- **Your PIN lock** (set it in the Settings tab if you haven't) is your main
  protection now that the app is on the open internet — don't skip it.
