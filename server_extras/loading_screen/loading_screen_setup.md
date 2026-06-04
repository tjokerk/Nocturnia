# NOCTURNIA — Custom Loading Screen Setup Guide
**Server Admin Reference · DayZ Custom Loading Screen**

---

## Overview

DayZ loads a custom HTML file as the splash/loading screen displayed to players while the server map and mods stream in. This guide walks you through placing the file, configuring your server's `serverDZ.cfg`, and verifying it works.

> ✅ **What players will see:** The NOCTURNIA loading screen with animated stars, a live progress bar, rotating server tips, a terminal-style log stream, and your server's connection details — all visible from first connect.

---

## Requirements

**You need:**
- DayZ standalone server (any host)
- FTP / file manager / SSH access to server root
- The file: `loading_screen.html` (download it from this folder)

**You do NOT need:**
- A custom mod or PBO
- Any extra software
- A web server or external hosting

---

## Step-by-Step

### Step 01 — Upload the HTML file to your server

Place `loading_screen.html` in your server's **root directory** — the same folder that contains `serverDZ.cfg` and your `DayZServer_x64.exe`.

```
dayz_server_root/
├── DayZServer_x64.exe
├── serverDZ.cfg
└── loading_screen.html   ← goes here
```

> **Tip:** If your host uses a file manager panel (e.g. Nitrado, GTXGaming, G-Portal), navigate to the root of your server profile and upload there. If you have SSH/FTP, drop it directly alongside the server binary.

---

### Step 02 — Edit `serverDZ.cfg`

Open your `serverDZ.cfg` file and add or update the `loadingScreen` line. The value is the path to your HTML file, relative to the server root.

```cfg
// serverDZ.cfg — add or update this line:
loadingScreen = "loading_screen.html";
```

> ⚠️ **Watch out:** The filename is case-sensitive on Linux servers. Make sure it matches exactly: `loading_screen.html` — all lowercase.

---

### Step 03 — Restart the server

Save the config, then do a **full server restart** (not just a mission restart). The loading screen is loaded at server startup, not per mission cycle.

---

### Step 04 — Verify in-game

Connect to the server via the DZSA Launcher or the in-game browser. You should see the NOCTURNIA loading screen immediately when the connection handshake begins — before the game world loads.

> ✅ **Success check:** If the screen appears but with missing fonts or a broken layout, your server host may be blocking external font requests. See the troubleshooting section below.

---

## Reference — Minimal `serverDZ.cfg`

If you're not sure where to add the line, here's a minimal valid config showing placement in context:

```cfg
hostname        = "#1 NOCTURNIA | AiCompanion | SciFi | Quests | Traders | Hardcore";
maxPlayers      = 60;
password        = "";
passwordAdmin   = "yourpassword";
enableWhitelist = 0;

// ── Custom loading screen ──
loadingScreen   = "loading_screen.html";

motd[]          = {"Welcome to NOCTURNIA", "discord.gg/YJ8G2sGskk"};
motdInterval    = 60;
timeStampFormat = "Short";
```

---

## Troubleshooting

### ❌ Screen doesn't appear at all
Check the filename and path in `serverDZ.cfg`. Confirm the file is in the server root (same directory as the cfg). Do a full restart, not a soft reload.

---

### ❌ Fonts look wrong / default browser font shows
The screen loads Google Fonts over the internet. Some server hosts or players' network configs block external requests from the DayZ browser sandbox.

**Fix:** Download the three fonts locally and embed them.
1. Download from Google Fonts: `Orbitron`, `Share Tech Mono`, `Exo 2`
2. Place the `.woff2` files in a `fonts/` folder next to the HTML file
3. Replace the `<link>` tag at the top of the HTML with `@font-face` rules pointing to the local files

```
dayz_server_root/
├── loading_screen.html
└── fonts/
    └── Orbitron-Bold.woff2
```

---

### ❌ Screen shows but stays at 0% / bar doesn't move
This is **normal** if the server connects extremely fast — the bar is cosmetic and driven by a JavaScript timer. The screen will dismiss on its own once DayZ finishes loading the world. No action needed.

---

## Customizing the Screen

All editable content is clearly grouped in the HTML file. Open it in any text editor and look for these sections:

| Section | What to look for | What to edit |
|---|---|---|
| **Survivor Intel tips** | `const tips = [...]` near the bottom of the `<script>` block | Add, remove, or edit any tip string |
| **Log messages** | `const logs = [...]` just below tips | Edit the `t:` text field. Use `c: 'ok'` for green, `c: 'warn'` for red, `c: ''` for default |
| **Server info strip** | Search for `info-strip` in the HTML body | IP, Discord, map name — plain text inside `<span>` tags |
| **Ticker tape** | Search for `ticker-inner` | Edit text directly, separate items with `&nbsp;·&nbsp;` |

---

*NOCTURNIA · Hardcore PvE · Namalsk · discord.gg/YJ8G2sGskk · 65.7.2.44:2555*
