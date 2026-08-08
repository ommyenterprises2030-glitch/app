# Ember & Ash POS — Installable App

This is a full PWA (Progressive Web App): installable to your home screen,
works offline after the first load, opens full-screen like a native app.

## Why you can't just open the file directly

"Add to Home Screen" + offline caching only work when the app is served
over `https://`. Opening `index.html` straight from your Files app
(`file://...`) will show the POS, but iOS/Android will refuse to install
it or cache it for offline use. It needs to be hosted somewhere first —
even for free, even from your phone, with no computer required.

## Fastest phone-only hosting: Netlify Drop

1. On your phone, go to **https://app.netlify.com/drop** in your browser
2. Tap the drop area — it opens your file picker
3. Select **all the files in this folder** (`index.html`, `manifest.json`,
   `sw.js`, `icon-192.png`, `icon-512.png`) — or select the zip if your
   file picker supports uploading a zip and Netlify unpacks it (if not,
   unzip first using your phone's Files app, then select the folder contents)
4. Netlify deploys instantly and gives you a live `https://something.netlify.app` URL
5. Open that URL in Safari (iPhone) or Chrome (Android)

## Installing to your home screen

**iPhone (Safari):**
Open the URL → tap the Share icon → **Add to Home Screen** → Add.

**Android (Chrome):**
Open the URL → tap the **⋮** menu → **Add to Home screen** / **Install app**.

Once installed, it opens full-screen with no browser bar, has its own
icon, and works with airplane mode on after the first visit.

## Alternative hosts (same idea, all free, all work from a phone browser)

- **Cloudflare Pages** — cloudflare.com, has a drag/drop deploy option
- **GitHub Pages** — requires a GitHub account; upload the files to a repo
  via the GitHub mobile web UI, then enable Pages in repo settings
- **Vercel** — vercel.com, similar drop-to-deploy flow

Any static host works — this app has no backend, no build step, no
server-side code.

## What's in this folder

```
pwa-pos/
├── index.html      — the app (markup + POS component + styles)
├── manifest.json    — name, icon, and install behavior
├── sw.js            — service worker, caches everything for offline use
├── icon-192.png     — home screen icon (small)
└── icon-512.png     — home screen icon (large / splash)
```

## Notes

- First load needs internet (to fetch React/Babel from their CDN and to
  cache the app shell). After that, the service worker serves everything
  from cache — including those CDN scripts — so it keeps working offline.
- Data (tables, orders, payments) is saved in the browser's local storage
  on your phone. It stays put across restarts, but it's local to this one
  device — reinstalling or clearing site data resets it.
- To update the app later, edit `index.html`, bump `CACHE_NAME` in `sw.js`
  (e.g. `ember-ash-pos-v2`) so the service worker knows to refetch, and
  redeploy to your host.
