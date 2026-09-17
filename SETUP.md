# Samartha Sweets & Mess ERP — Setup

This is a fresh, complete copy of your app. All functionality is included and
unchanged: Dashboard with charts, Students, Attendance, Payments, Tiffin Log,
Sweets & Namkeen (production + sales + stock), Expenses, and Settings
(backup/restore, due date, low-stock thresholds, add product, dark mode, PIN
lock). Your Firebase config is already filled in — you do **not** need to
touch SETUP.md's old placeholder steps.

**One change from before:** if the app ever can't reach the shared database
within 12 seconds, it now shows a clear troubleshooting message instead of
spinning on "Loading your register…" forever. This won't fix a network block
by itself, but it tells you (and anyone helping you) what's actually
happening instead of nothing.

## Files in this bundle
- `index.html` — the whole app
- `manifest.json` — makes it installable as a home-screen app
- `sw.js` — lets it still open with a weak signal
- `icons/icon-192.png`, `icons/icon-512.png` — your Samarth Sweets logo, sized for app icons

## How to replace what's on GitHub

1. Go to your repo: `github.com/Ratnesh-07/Samarth-Sweets-and-Mess`
2. Click on `index.html` in the file list → click the **pencil (edit)** icon
3. Select all the existing content and delete it
4. Paste in the new `index.html` from this bundle
5. Scroll down, click **Commit changes**
6. Repeat the same steps for `manifest.json` and `sw.js`
7. For the icons: open `icons/icon-192.png` in your repo → there should be an
   option to upload a new version, or delete the old one and use
   **Add file → Upload files** to upload the new `icon-192.png` and
   `icon-512.png` from this bundle's `icons` folder (make sure they land
   inside the `icons/` folder in your repo, not the root)

GitHub Pages will redeploy automatically within a minute or two of your last
commit. Then reload `ratnesh-07.github.io/Samarth-Sweets-and-Mess/`.

## If it still gets stuck loading

The new timeout message will now tell you plainly if this happens again. Based
on everything checked so far — your Firebase config is correct, your Firestore
rule is valid until October 2026, and Anonymous auth isn't even used by this
code — the most likely remaining cause is a **network-level block** on this
device or network reaching Google's Firestore servers (a router filter,
antivirus, or ISP-level restriction). To confirm:

- Open the site on **mobile data** instead of Wi-Fi and see if it loads.
- Try a different Wi-Fi network if one is available.
- Check if any VPN, firewall, or antivirus software is active on this device.

If it loads fine on mobile data but not on your usual Wi-Fi, the fix is on
your router/network side, not in this app's code.
