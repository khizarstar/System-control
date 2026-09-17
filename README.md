# Sky Hosting Control — Android wrapper

A minimal Android app that opens your existing owner panel
(`https://system.skyhosting.qzz.io/systemcontrol/`) inside a native app
shell — same login, same SQL console, same env editor, same file
manager. It does not reimplement any of the panel's security; it just
gives you an app icon instead of a browser tab.

## ⚠️ Before you build: the IP allowlist

`services/ownerPanelIpAllowlist.js` gates the panel before anything
else — an unlisted IP gets a plain 404, before the login page even
loads. If `OWNER_PANEL_ALLOWED_IPS` is set on your server, add your
phone's IP (or your mobile carrier's / home Wi-Fi's public IP) to that
list, or the app will just show a 404 forever. If you're on mobile
data, your carrier IP can change, so you may want a VPN with a static
IP, or to loosen the allowlist for this use case.

## One-time setup

1. Create a new **private** GitHub repo (e.g. `sky-hosting-control-app`).
2. Push everything in this folder to it:
   ```
   git init
   git add .
   git commit -m "Initial Android wrapper"
   git branch -M main
   git remote add origin https://github.com/<you>/sky-hosting-control-app.git
   git push -u origin main
   ```
3. GitHub Actions builds automatically on every push (see
   `.github/workflows/android-build.yml`). Go to the repo's **Actions**
   tab → the latest run → **Artifacts** → download
   `sky-hosting-control-debug-apk`.
4. Unzip it, copy `app-debug.apk` to your phone, and install it
   (you'll need to allow "install unknown apps" for whatever app you
   use to open it — this is a debug build, not from the Play Store).

## Changing the panel URL

If the panel ever moves, edit `BASE_URL` in
`app/src/main/java/com/skyhosting/control/MainActivity.kt`, commit,
and push — Actions rebuilds the APK for you.

## What this does NOT do

- It doesn't add biometric lock on top of the panel's own login —
  the panel's 5-step login (System Identifier, Identity Key, 2x TOTP,
  Primary Password) still runs every time your session expires.
- It doesn't change the IP allowlist, rate limits, or anything else
  server-side. All of that is exactly as it is today.
- This is a **debug** build (unsigned). Fine for your own phone; if
  you want it on the Play Store or signed properly later, that's a
  separate step (a release keystore + signing config).
