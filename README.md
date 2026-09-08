IFF Far-Field Range Calculator — iPhone PWA
Five files, all in one folder. No build step, no dependencies, no CDN.
    index.html
    manifest.webmanifest
    sw.js
    icon-180.png  icon-192.png  icon-512.png  icon-512-maskable.png

The one hard requirement: HTTPS
iOS gives location access and service workers only to a secure origin. Opening
`index.html` from the Files app or a file:// path will not work — the app loads, but
the GPS button reports that it needs HTTPS and the offline cache never installs.
Any of these will do:
Option	Notes
Internal IIS / Apache site	Best if you already have one. Drop the folder in and browse to it.
SharePoint	Won't work — the sandboxed viewer blocks both geolocation and service workers.
GitHub Pages / Netlify / Cloudflare Pages	Free HTTPS in minutes, but the URL is public. Fine only if the coordinates aren't sensitive.
`localhost`	Treated as secure, so useful for testing on a laptop.
Installing on the iPhone
Open the URL in Safari (not Chrome — only Safari can install to the home screen).
Tap the Share button, then Add to Home Screen.
Launch it from the home screen. It opens full-screen with no browser chrome.
First launch caches everything. After that it works with no signal at all — the only
features needing a connection are the geojson.io link and Mail actually sending.
Location permission
The first time you tap Use phone GPS, iOS asks for permission. If it was declined
earlier: Settings › Privacy & Security › Location Services › Safari Websites, and
set to While Using the App. Also check Precise Location is on, or fixes come back
at roughly city-block accuracy.
The button shows the fix time and the reported accuracy, and flags anything worse than
±15 m. At a 1.2 km baseline a 5 m fix contributes about 0.24° of bearing error — inside
your <2° tolerance — but phone GPS is not a substitute for a surveyed RASS position you
intend to reuse.
Updating
Edit `index.html`, then bump `CACHE` in `sw.js` (`iff-range-v1` → `v2`). The service
worker fetches from the network first, so the change appears on the next launch with a
connection; the version bump clears out the stale copy.
