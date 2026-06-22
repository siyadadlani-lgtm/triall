[README.md](https://github.com/user-attachments/files/29216482/README.md)
# Dad's Emergency Vada Pav Locator 🥔

A tiny, joke-filled web app that finds the nearest vada pav shop and
gives you walking directions — made with love by Siya for Dad.

This README explains everything in plain language. No coding
experience needed.

---

## What's in this folder?

```
dad-vada-pav/
├── index.html          → The app itself (structure + all styling)
├── app.js               → The "brain": location, search, button logic
├── jokes.js              → The list of 50+ dad jokes
├── manifest.json         → Tells phones "this can be installed as an app"
├── sw.js                 → Service worker (makes the app work offline)
├── vercel.json            → Settings used when deploying to Vercel
├── generate_icons.py      → One-time script that drew the app icon
└── icons/
    ├── icon-192.png
    ├── icon-512.png
    ├── icon-maskable-192.png
    └── icon-maskable-512.png
```

That's it — no database, no backend server, no API keys. Everything
runs directly in the phone's browser.

---

## How the app works (plain English)

1. **Start screen** — a big "Find My Vada Pav" button.
2. **Loading screen** — while we get your location, we show a random
   terrible dad joke every ~1.4 seconds (see `jokes.js`).
3. **Search** — the app asks a free, public map service called
   **Overpass API** (built on OpenStreetMap, like a free Google Maps
   database) for nearby vada pav stalls, fast food spots, and Indian
   restaurants within 2 km.
4. **Result screen** — the closest match is shown in a big card with
   distance, estimated walking time, and a "Get Directions" button.
   That button opens Google Maps with walking directions already
   filled in. A few backup options are listed below it too.

No personal data is stored or sent anywhere except a single, one-time
location request to the map search service — nothing is saved,
logged, or shared.

---

## 1. How to run it on your computer (before deploying)

You don't need to install anything complicated. Just a way to "serve"
the files locally, because browsers block some app features
(like the service worker) when you just double-click an HTML file.

**Easiest option — if you have Python installed (Mac/Linux usually do):**

1. Open the "Terminal" app.
2. Type `cd ` (with a space after it), then drag the `dad-vada-pav`
   folder into the Terminal window, and press Enter. This moves you
   into the project folder.
3. Type this and press Enter:
   ```
   python3 -m http.server 8000
   ```
4. Open a web browser and go to: `http://localhost:8000`
5. You should see the app! Press `Ctrl+C` in Terminal when you're done.

**If you don't have Python**, install the free [VS Code](https://code.visualstudio.com/)
editor, open the folder in it, install the "Live Server" extension,
then right-click `index.html` → "Open with Live Server."

> Note: GPS location requires either `localhost` or a secure (`https://`)
> address — it won't work if you just double-click the file directly.

---

## 2. How to deploy it for free on Vercel

Vercel is a free hosting service — once deployed, your dad can open a
real website link from anywhere, on any phone.

**Option A — No coding tools at all (drag and drop):**

1. Go to [vercel.com](https://vercel.com) and sign up for a free account
   (you can sign up with just an email or a Google account).
2. Once logged in, click **"Add New" → "Project."**
3. Look for an option that says something like **"Deploy without Git"**
   or use the **Vercel CLI drag-and-drop** — if your dashboard only
   shows "Import Git Repository," use Option B below instead, since
   Vercel's interface changes over time.
4. Upload the entire `dad-vada-pav` folder (all the files at once).
5. Click **Deploy**. Within a minute, Vercel gives you a live link
   like `https://dads-vada-pav.vercel.app`.

**Option B — Using GitHub (more reliable, still beginner-friendly):**

1. Go to [github.com](https://github.com) and create a free account.
2. Click **"New repository"**, name it something like `dad-vada-pav`,
   and create it.
3. On the new repository page, click **"uploading an existing file"**
   and drag in all the files from the `dad-vada-pav` folder.
4. Commit the files (there's a green "Commit changes" button).
5. Go back to [vercel.com](https://vercel.com), click **"Add New" → "Project,"**
   and choose **"Import Git Repository."** Select the repository you
   just created.
6. Leave all settings as default (this app needs no build step) and
   click **Deploy**.
7. Vercel gives you a live URL in about 30–60 seconds.

Either way, **no environment variables or API keys are needed** —
the map search service (Overpass API) is free and doesn't require an
account.

---

## 3. Environment variables / API keys

**None required.** This app intentionally uses only free, key-less
services:

- **Browser Geolocation API** — built into every modern phone browser,
  no setup needed.
- **Overpass API** (`overpass-api.de`) — a free, public OpenStreetMap
  search service, no account or key needed.
- **Google Maps deep link** — just opens a normal `maps.google.com`
  URL for directions, no API key needed.

If you ever want to swap in a paid service (like the official Google
Places API) for more shop coverage, that would require an API key —
but it is not necessary for this app to work well.

---

## 4. How to install it on a phone (turn it into a real app icon)

Once deployed (e.g. to your `https://....vercel.app` link):

**On Android (Chrome):**
1. Open the link in Chrome.
2. Tap the **⋮ (three dots)** menu in the top right.
3. Tap **"Add to Home screen"** or **"Install app."**
4. Confirm — the Vada Pav icon now appears on the home screen and
   opens full-screen, just like a normal app.

**On iPhone (Safari):**
1. Open the link in Safari (must be Safari, not Chrome, for this step
   on iPhone).
2. Tap the **Share icon** (square with an arrow pointing up).
3. Scroll down and tap **"Add to Home Screen."**
4. Tap **Add** in the top right.
5. The icon now appears on the home screen.

From then on, your dad can just tap the icon — no browser bar, no
typing a URL, just a real app experience.

---

## 5. How to update the app later

Whenever you want to add jokes, fix text, or change colors:

1. Edit the relevant file:
   - New jokes → `jokes.js`
   - Text, layout, or colors → `index.html`
   - Search behavior or button logic → `app.js`
2. **Important:** open `sw.js` and change this line near the top:
   ```js
   const CACHE_NAME = 'vada-pav-locator-v1';
   ```
   to:
   ```js
   const CACHE_NAME = 'vada-pav-locator-v2';
   ```
   (just bump the number each time you update). This tells phones
   that already installed the app to fetch the new version instead
   of using an old saved copy.
3. Re-deploy:
   - **If using GitHub + Vercel:** upload the changed files to your
     GitHub repository (there's an "Add file → Upload files" button on
     the repository page). Vercel automatically redeploys within
     about a minute.
   - **If using drag-and-drop on Vercel:** go to your project on
     Vercel, click **"Deployments,"** and upload the updated folder
     again.
4. On your dad's phone, the installed app will pick up the update
   automatically the next time it's opened with an internet
   connection (it may take one extra reopen to fully refresh).

---

## Adding your own dad jokes

Open `jokes.js` and you'll see a long list like this:

```js
const DAD_JOKES = [
  "Vada you waiting for?",
  "This is a starch emergency.",
  // ... more jokes ...
];
```

Just add a new line with your joke in quotes, followed by a comma:

```js
  "Your new terrible joke goes here.",
```

Save the file, redeploy (see Section 5), and it'll show up in the
random rotation.

---

Made with love by Siya, for Dad. ❤️
