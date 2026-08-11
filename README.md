# Berries Café — Setup & Hosting Guide

Everything is a static file — no build step, no `npm install`. That's what makes it possible to edit content straight from GitHub's website with no code knowledge.

## 1. Folder structure — keep this exact layout

```
your-repo/
├── index.html
├── README.md
├── audio/
│   ├── dreamy-skyline.mp3
│   └── woods-of-peace.mp3
└── data/
    ├── founders.json     ← Max, Natsu, Aashi — "The Three Pillars" section
    ├── members.json      ← "Most Active Members" section
    ├── moments.json      ← "Precious Moments" gallery
    └── lore.json         ← the full 7-chapter lore, in the Lore section
```

## 2. One-time setup

Open `index.html`, find the `CONFIG` block near the top of the `<script>` tag:

```js
const CONFIG = {
  DISCORD_INVITE_URL: "https://discord.gg/Nks8f9UtWy",
  GITHUB_URL: "https://github.com/your-username/your-repo",  // <-- set this to YOUR repo
  MEMBER_COUNT: 128,  // shown only until the live number loads
  ONLINE_COUNT: 34,   // shown only until the live number loads
};
```

**You must set `GITHUB_URL` to your actual repo URL.** Two features depend on it:
- Auto-detecting songs in `/audio`
- Loading the editable `data/*.json` content

Everything else in the file works without touching any other code.

## 3. Push to GitHub

```bash
mkdir berries-cafe && cd berries-cafe
# copy index.html, README.md, the audio/ folder, and the data/ folder in here
git init
git add .
git commit -m "Berries Café site"
git branch -M main
git remote add origin https://github.com/your-username/your-repo.git
git push -u origin main
```

Make sure the repo is **public** — the live song list and content files load through GitHub's public API, which only works on public repos.

## 4. Turn on GitHub Pages

1. Repo → **Settings** → **Pages**
2. Source: `Deploy from a branch`. Branch: `main`, folder: `/ (root)`. Save.
3. Live in about a minute at `https://your-username.github.io/your-repo/`

Want `berriescafe.com` instead? Buy the domain from any registrar, point its DNS at GitHub Pages (a `CNAME` record to `your-username.github.io`), then set it as your custom domain under Settings → Pages. Ask me and I'll add the `CNAME` file for you.

---

## What's fully live and working

### 🎵 Music — just drop a file in, no code editing
Add any `.mp3`, `.wav`, `.ogg`, or `.m4a` file into the `audio/` folder on GitHub and push. The website automatically detects it and adds it to the playlist — the filename becomes the display title (`late-night-vibes.mp3` → "Late Night Vibes"). No JavaScript to touch.
> This detection uses GitHub's API and only works once the site is live on GitHub Pages (not when opening `index.html` directly on your computer) — in that case it falls back to the two starter tracks.

### 💬 Live Discord numbers — no bot required
The hero and Join section show real online/member counts, pulled straight from your invite link (`discord.gg/Nks8f9UtWy`) every time someone loads the page. No bot, no token, no server setup — just your public invite link.

### ✏️ Content you can edit straight from GitHub's website
Click into any file below on GitHub → pencil (edit) icon → change the text → commit. The site picks it up on next page load, no deploy step needed.

- **`data/founders.json`** — "The Three Pillars" section (Max, Natsu, Aashi). Each entry: `emoji`, `role`, `name`, `bio`.
- **`data/members.json`** — "Most Active Members." Each entry: `emoji`, `name`, `role`.
- **`data/moments.json`** — "Precious Moments" gallery. Each entry: `emoji`, `cap` (caption), `bg` (a CSS gradient — pick colors from the palette in the file, or copy an existing one).
- **`data/lore.json`** — the full lore chapters people can expand under "Café Lore." Each entry: `title`, `text`.

All four are plain JSON arrays — copy an existing entry, change the values, keep the commas. If a file is ever missing or has a typo, the site quietly falls back to its built-in defaults instead of breaking.

### 📱 Fully responsive
Rebuilt with fluid grids (`auto-fit`/`minmax`) instead of fixed column counts, so every section — features, gallery, menu, founders, members — reflows naturally on any screen width instead of snapping at a few breakpoints. Also tuned for:
- Phones in portrait **and** landscape (short-viewport hero adjusts automatically)
- Tablets, both orientations
- The floating music player shrinks and repositions on small screens so it never blocks content

### 🎭 Everything from before still works
Every button, the theme toggle (day/sunset/night), FAQ accordion, gallery lightbox, guestbook (per-visitor via localStorage), Aashi's click easter egg, the Konami code, and the Privacy/Credits modals.

---

## What still needs manual attention

- **Real photos**: the gallery is emoji placeholders on gradients. When you have real images, swap a `moments.json` entry's `bg`/`emoji` for an `<img>` — ask me and I'll wire that in.
- **Aashi's live status**: showing whether your actual bot is online in real time needs either the Server Widget (Server Settings → Widget, if your server has that option) or a small secure backend holding her token — never put a bot token in this file, since it's public. Ask if you want either built out.
- **Discord numbers accuracy**: the invite API returns *approximate* counts (Discord's own rounding), refreshed on each page load — this is normal and matches what Discord shows elsewhere.
