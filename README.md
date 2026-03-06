<div align="center">

```
██████╗ ██████╗ ██╗██╗   ██╗ █████╗ ██████╗ ██████╗  ██████╗ ██╗    ██╗███████╗███████╗
██╔══██╗██╔══██╗██║██║   ██║██╔══██╗██╔══██╗██╔══██╗██╔═══██╗██║    ██║██╔════╝██╔════╝
██████╔╝██████╔╝██║██║   ██║███████║██████╔╝██████╔╝██║   ██║██║ █╗ ██║███████╗█████╗
██╔═══╝ ██╔══██╗██║╚██╗ ██╔╝██╔══██║██╔══██╗██╔══██╗██║   ██║██║███╗██║╚════██║██╔══╝
██║     ██║  ██║██║ ╚████╔╝ ██║  ██║██████╔╝██║  ██║╚██████╔╝╚███╔███╔╝███████║███████╗
╚═╝     ╚═╝  ╚═╝╚═╝  ╚═══╝  ╚═╝  ╚═╝╚═════╝ ╚═╝  ╚═╝ ╚═════╝  ╚══╝╚══╝ ╚══════╝╚══════╝
```

**Privacy-First Chromium Browser — Electron — Windows x64**

[![Version](https://img.shields.io/badge/version-1.1.0-6366f1?style=for-the-badge&logo=electron&logoColor=white)](https://github.com/omfghello/priva-browser/releases/tag/v_1_0_release)
[![Electron](https://img.shields.io/badge/electron-40-47848F?style=for-the-badge&logo=electron&logoColor=white)](https://www.electronjs.org/)
[![Platform](https://img.shields.io/badge/platform-Windows%20x64-0078D4?style=for-the-badge&logo=windows&logoColor=white)](https://github.com/omfghello/priva-browser/releases/tag/v_1_0_release)
[![License](https://img.shields.io/badge/license-MIT-a6e3a1?style=for-the-badge)](LICENSE.md)
[![Build](https://img.shields.io/badge/build-passing-a6e3a1?style=for-the-badge&logo=github-actions&logoColor=white)](../../actions)

<br/>

*A fully custom privacy-focused browser that blocks ads, nukes trackers, spoofs fingerprints, and looks damn good doing it — without phoning home, ever.*

<br/>

### [⬇ Download PrivaBrowse v1.1.0](https://github.com/omfghello/priva-browser/releases/tag/v_1_0_release)

`PrivaBrowse Setup 1.1.0.exe` · Electron 40 · Chromium-based · x64

</div>

---

## Table of Contents

- [Overview](#-overview)
- [Features](#-features)
  - [Privacy & Blocking](#-privacy--blocking)
  - [Browsing](#-browsing)
  - [Tools & Utilities](#-tools--utilities)
  - [Customization](#-customization)
  - [Achievements & Gamification](#-achievements--gamification)
- [Screenshots](#-screenshots)
- [Internal Pages](#-internal-pages)
- [Keybindings](#-keybindings)
- [Architecture](#-architecture)
- [Project Structure](#-project-structure)
- [Building from Source](#-building-from-source)
- [System Requirements](#-system-requirements)
- [License](#-license)

---

## 🔍 Overview

PrivaBrowse is a **privacy-first Chromium browser** built with Electron. It ships with aggressive ad blocking, tracker annihilation, fingerprint spoofing, and DNS-over-HTTPS — all enabled out of the box. No telemetry. No data collection. No phone-home. Just browsing.

<table>
<tr>
<td><b>Engine</b></td><td>Chromium (via Electron 40)</td>
</tr>
<tr>
<td><b>Framework</b></td><td>Electron + electron-builder</td>
</tr>
<tr>
<td><b>Platform</b></td><td>Windows x64 (10/11)</td>
</tr>
<tr>
<td><b>UI</b></td><td>Custom dark glass design system with light mode</td>
</tr>
<tr>
<td><b>Ad Blocking</b></td><td>9-layer system with 2,100+ blocked domains, pattern matching, and script injection</td>
</tr>
<tr>
<td><b>Tracker Blocking</b></td><td>Curated domain lists + social tracker isolation + crypto miner blocking</td>
</tr>
<tr>
<td><b>Fingerprint Protection</b></td><td>60+ vectors — Canvas, WebGL, Audio, Navigator, Screen, Timing, Fonts, Math, MIDI, Sensors, and more</td>
</tr>
<tr>
<td><b>Data Poisoning</b></td><td>200+ fake HTTP headers + 55+ poisoned data fields on every tracker request</td>
</tr>
<tr>
<td><b>Security Hardening</b></td><td>Process sandboxing, CSP, IPC validation, navigation guards, permission handlers</td>
</tr>
<tr>
<td><b>DNS</b></td><td>DNS-over-HTTPS (Cloudflare, Quad9, NextDNS, or custom)</td>
</tr>
<tr>
<td><b>Storage</b></td><td>electron-store (local only, never synced)</td>
</tr>
<tr>
<td><b>Settings</b></td><td>90+ configurable options across 15 categories</td>
</tr>
<tr>
<td><b>License</b></td><td>MIT</td>
</tr>
</table>

---

## ✨ Features

### 🔒 Privacy & Blocking

> PrivaBrowse is hardened by default. Every privacy feature is enabled out of the box — no configuration required.

<details>
<summary><b>Multi-Layer Ad Blocking</b></summary>

| Layer | How It Works |
|---|---|
| **Fast-path host blocking** | O(1) hash lookup against 740+ fast-block domains (2,100+ total across all lists) |
| **Ad domain blocking** | 408 dedicated ad-serving domains blocked |
| **Tracker domain blocking** | 446 tracker domains blocked at the network level |
| **Aggressive blocking** | 358+ additional domains for aggressive mode |
| **Social tracker blocking** | 112 social media tracking domains isolated |
| **Crypto miner blocking** | 84 in-browser mining domains blocked |
| **URL pattern matching** | Regex-based blocking for 100+ ad network URL patterns |
| **YouTube ad interception** | Blocks pre-roll ad requests, ad-serving scripts, and tracking pixels |
| **Script injection** | Injects ad-nuking scripts into pages for elements that bypass network-level blocking |
| **Resource type filtering** | Blocks ad-related scripts, images, iframes, and XHR requests by type |
| **Data poisoning** | Injects 55+ fake data fields into tracker requests to corrupt surveillance profiles |
| **HTTP header poisoning** | Adds 200+ fake HTTP headers across 13 categories on every tracker request |
| **Storage hygiene** | Periodic cleanup of 60+ tracker localStorage keys every 60 seconds |
| **Redirect tracker bypass** | Resolves t.co, l.facebook.com, google.com/url redirects directly |

</details>

<details>
<summary><b>Tracker & Fingerprint Protection</b></summary>

| Feature | Description |
|---|---|
| **Tracker domain blocking** | Curated blocklists of 446+ tracker domains, updated and aggressive |
| **Social tracker isolation** | Blocks 112 Facebook, Twitter, LinkedIn, and Google tracking domains |
| **Crypto miner blocking** | Blocks 84 Coin-Hive and derivative in-browser mining domains |
| **Fingerprint spoofing** | 60+ vectors randomized per session — Canvas, WebGL, Audio, Navigator, Screen, Timing, Fonts, Math, MIDI, Device Motion, WebGPU, CSS matchMedia, Permissions, Storage, and more |
| **Referrer trimming** | Strips referrer headers to origin-only |
| **Do Not Track** | Sends DNT header on every request |
| **Third-party cookie blocking** | Blocks cross-origin cookies |
| **Tracking parameter stripping** | Removes 110+ tracking params from URLs (utm, fbclid, gclid, TikTok, Twitter, Reddit, LinkedIn, Amazon, Pinterest, and more) |
| **Nuclear data poisoning** | 55+ fake data fields injected into every tracker request to corrupt surveillance profiles |
| **HTTP header poisoning** | 200+ poisoned HTTP headers across 13 categories on every tracker request |
| **WebSocket interception** | Poisons WebSocket payloads sent to tracker hosts |
| **EventSource blocking** | Tracker SSE connections blocked entirely |
| **Redirect tracker bypass** | t.co, l.facebook.com, google.com/url resolved directly |
| **Periodic storage cleanup** | 60+ tracker localStorage keys purged every 60 seconds |
| **Anchor ping stripping** | Removes HTML ping attributes from all links |
| **document.referrer sanitization** | Referrer reduced to origin-only via script injection |

</details>

<details>
<summary><b>Anti-Annoyance</b></summary>

| Feature | Description |
|---|---|
| **Cookie banner removal** | Auto-dismisses cookie consent popups across 20+ consent frameworks (smart exclusions for Google/YouTube) |
| **Newsletter popup killing** | Removes email signup overlays and modals |
| **Idle dialog dismissal** | Auto-dismisses "are you still there?" and paywall soft-locks |
| **Auto cookie cleanup** | Clears site cookies when you close a tab (optional) |
| **App install banner blocking** | Blocks "Install our app" and "Open in app" banners |
| **Google Sign-In nag blocking** | Removes Google's persistent sign-in prompts |

</details>

<details>
<summary><b>Network Privacy</b></summary>

| Feature | Description |
|---|---|
| **DNS-over-HTTPS** | All DNS queries encrypted — choose from Cloudflare, Quad9, NextDNS, or enter a custom DoH URL |
| **Force HTTPS** | Automatically upgrades all HTTP requests to HTTPS |
| **Data saver mode** | Blocks autoplay media, animated images, web fonts, and large third-party resources |
| **Incognito mode** | Separate session with full isolation and auto-cleanup on close |
| **WebSocket/EventSource interception** | Poisons tracker WebSocket payloads and blocks tracker SSE connections |
| **Redirect tracker bypass** | Resolves t.co, l.facebook.com, google.com/url redirect chains directly |

</details>

<details>
<summary><b>Security Hardening</b></summary>

| Feature | Description |
|---|---|
| **Process sandboxing** | `app.enableSandbox()` — all renderer processes sandboxed |
| **17 Chromium flags** | Command-line flags to disable risky features (WebBluetooth, WebUSB, WebSerial, etc.) |
| **Strict CSP** | Content Security Policy blocks inline scripts and restricts resource origins |
| **Context isolation** | Full context isolation enabled; node integration disabled in all renderers |
| **Webview hardening** | Webview attachment validated — only whitelisted preloads, node integration forced off |
| **Navigation guards** | Blocks `javascript:` and `data:` URL navigation attempts |
| **Permission handlers** | Blocks dangerous API permissions (camera, microphone, geolocation, MIDI, sensors, etc.) |
| **IPC validation** | All IPC inputs validated and rate-limited to prevent renderer-to-main abuse |

</details>

---

### 🌐 Browsing

<details>
<summary><b>Tab Management</b></summary>

| Feature | Description |
|---|---|
| **Multi-tab interface** | Tabbed browsing with drag-to-reorder |
| **Tab sleep** | Automatically suspends inactive tabs to save memory |
| **Tab snooze** | Snooze a tab for later — it reopens at the scheduled time |
| **Duplicate tab detection** | Warns before opening a URL that's already in another tab |
| **Tab preview on hover** | Shows a tooltip preview of the tab's title and URL |
| **Close Other Tabs** | Right-click context menu: close other tabs, close tabs to the right |
| **Crash recovery** | Periodically saves open tabs; restores them after unexpected exits |

</details>

<details>
<summary><b>Navigation</b></summary>

| Feature | Description |
|---|---|
| **Smart address bar** | Auto-detects URLs vs search queries, normalizes input |
| **Search engine switching** | Privacy-focused only — DuckDuckGo, Brave, Startpage, Qwant, SearXNG, Mojeek, MetaGer, Swisscows, Yep, Ecosia |
| **Reader mode** | Strips pages to pure text for distraction-free reading |
| **RSVP speed reader** | Rapid serial visual presentation — speed-read any article word by word |
| **Page translation** | Built-in translate feature for foreign-language pages |
| **Connection timeout** | Configurable timeout for slow-loading pages |

</details>

---

### 🛠 Tools & Utilities

<details>
<summary><b>Built-in Tools</b></summary>

| Tool | Description |
|---|---|
| **Screenshot** | Capture full-page or viewport screenshots |
| **Screen recorder** | Record your browser session as video |
| **Split view** | Browse two pages side by side |
| **Picture-in-Picture** | Pop out any video into a floating player |
| **Notes** | Built-in notepad synced to local storage |
| **Clipboard history** | Browse and re-copy recent clipboard entries |
| **API tester** | REST API testing tool — send requests, view responses |
| **Speed test** | Built-in network speed test |
| **Password vault** | Encrypted local password manager with master password |
| **Breach checker** | Check if your email appears in known data breaches (via HIBP) |
| **Focus mode** | Block distracting sites for a set time period |
| **Ambient sounds** | Mix rain, forest, cafe, and other ambient sounds for focus |
| **Reading list** | Save articles to read later |
| **Tab workspaces** | Save and restore named sets of tabs |
| **Fake identity generator** | Generate random identities for form filling |
| **Parental controls** | URL blocking and content filtering for supervised browsing |

</details>

---

### 🎨 Customization

<details>
<summary><b>Themes & Appearance</b></summary>

| Option | Description |
|---|---|
| **6+ built-in themes** | Dark, Midnight, Ocean, Forest, Sunset, Lavender, and more |
| **Dark / Light mode** | Full light mode with dedicated CSS variable set |
| **Accent colors** | Pick any accent color — applied globally |
| **Custom fonts** | Change the browser font family |
| **Custom wallpapers** | Set any image URL as the new tab background |
| **Custom CSS injection** | Write CSS rules that get injected into every page |
| **User scripts** | Inject custom JavaScript into pages |
| **New tab layouts** | Choose between full dashboard, minimal, or blank new tab |

</details>

<details>
<summary><b>90+ Settings</b></summary>

Settings are organized into 15 categories with a searchable, collapsible interface:

| Category | Examples |
|---|---|
| **Search** | Default engine, search suggestions |
| **Appearance** | Theme, accent color, font, dark/light mode |
| **Privacy** | Shields level, fingerprint protection, DoH, Do Not Track |
| **Blocking** | Ad blocking, tracker blocking, social trackers, crypto miners |
| **Anti-Annoyance** | Cookie banners, newsletters, idle dialogs, paywall bypass |
| **Downloads** | Download shelf, auto-open safe files, clear on exit |
| **Tabs** | Sleep, snooze, preview, duplicate detection |
| **Data** | Clear on exit, cookie cleanup, form data, cache |
| **Network** | Force HTTPS, data saver, connection timeout, proxy |
| **Performance** | RAM limit, tab sleep thresholds |
| **New Tab** | Layout, wallpaper, quick links |
| **Accessibility** | Font size, zoom, language spoofing |
| **Startup** | Welcome page, crash recovery, restore tabs |
| **Advanced** | User scripts, custom CSS, developer mode |
| **Shortcuts** | Full keyboard shortcut reference |

</details>

---

### 🏆 Achievements & Gamification

> PrivaBrowse turns privacy into a game. Block ads, build streaks, explore features — and earn achievements for all of it.

<details>
<summary><b>Achievement System</b></summary>

**38 achievements** across 5 categories:

| Category | Count | Examples |
|---|---|---|
| 🛡️ **Privacy** | 12 | Block 10 / 100 / 1K / 10K / 50K ads, block trackers, enable fingerprint protection, achieve A+ privacy score |
| 🔥 **Streaks** | 5 | Use PrivaBrowse 3 / 7 / 30 / 100 / 365 days in a row |
| 🗺️ **Explorer** | 7 | Save bookmarks, write notes, use reader mode, take screenshots |
| ⚡ **Power User** | 12 | Use split view, PiP, focus mode, ambient sounds, games, password vault, workspaces |
| 🔎 **Secrets** | 2 | Hidden achievements — keep exploring to find them |

**Rarity tiers** with distinct visual treatments:

| Rarity | Color | Difficulty |
|---|---|---|
| **Common** | Gray | Basic feature usage |
| **Rare** | 🔵 Blue | Moderate milestones |
| **Epic** | 🟣 Purple | Significant dedication |
| **Legendary** | 🟡 Gold | Extreme commitment |

**Leveling system** — your rank increases as you unlock more:

```
Level 0  Newcomer       (0 unlocks)
Level 1  Initiate       (1+ unlocks)
Level 2  Adventurer     (5+ unlocks)
Level 3  Veteran        (10+ unlocks)
Level 4  Expert         (15+ unlocks)
Level 5  Master         (20+ unlocks)
Level 6  Grandmaster    (25+ unlocks)
Level 7  Mythic         (30+ unlocks)
```

Plus: animated SVG progress ring, per-category stat counters, particle effects, shimmer animations on unlocked cards, and toast notifications when you earn new achievements.

</details>

<details>
<summary><b>Other Gamification</b></summary>

| Feature | Description |
|---|---|
| **Privacy score** | A+ through F grade calculated from your active privacy settings |
| **Daily challenges** | Fresh challenges on the new tab page |
| **XP & leveling** | Earn XP from blocking ads, trackers, and maintaining streaks |
| **Usage streaks** | Track consecutive days of use |
| **Stats dashboard** | Total ads blocked, trackers killed, fingerprints spoofed, pages loaded |
| **Tracker graveyard** | Visual graveyard of every tracker domain you've killed |
| **Built-in games** | Browser games for when you need a break |

</details>

---

## 📸 Screenshots

> *Coming soon — screenshots of the new UI will be added here.*

---

## 📄 Internal Pages

PrivaBrowse ships with **22 built-in pages**, all accessible via the `privabrowse://` protocol:

<details>
<summary><b>View all internal pages</b></summary>

| Page | URL | Description |
|---|---|---|
| New Tab | `privabrowse://newtab` | Quick links, search, daily challenges, level bar |
| Welcome | `privabrowse://welcome` | Interactive onboarding tutorial |
| Settings | `privabrowse://settings` | Full browser configuration (90+ settings) |
| History | `privabrowse://history` | Browsing history with search |
| Stats | `privabrowse://stats` | Privacy dashboard — ads blocked, trackers killed |
| Speed Test | `privabrowse://speedtest` | Built-in network speed test |
| Sessions | `privabrowse://sessions` | Save and restore tab workspaces |
| Games | `privabrowse://games` | Built-in browser games |
| About | `privabrowse://about` | Version info, update checker |
| Ad Blocking | `privabrowse://adblocking` | Ad blocking stats and controls |
| Tracker Graveyard | `privabrowse://graveyard` | Visual graveyard of every tracker killed |
| Docs | `privabrowse://docs` | Built-in documentation |
| API Tester | `privabrowse://api-tester` | REST API testing tool |
| Recorder | `privabrowse://recorder` | Screen recording utility |
| Puzzle | `privabrowse://puzzle` | Built-in puzzle game |
| Parental | `privabrowse://parental` | Parental controls panel |
| PrivaSearch | `privabrowse://privasearch` | Privacy-respecting search |
| Terms | `privabrowse://terms` | Terms of service |
| Privacy | `privabrowse://privacy` | Privacy policy |
| Contact | `privabrowse://contact` | Contact page |
| Transparency | `privabrowse://transparency` | Transparency report — what PrivaBrowse blocks and why |
| Fingerprinting | `privabrowse://fingerprinting` | Fingerprint protection details — 60+ spoofed vectors |

</details>

---

## ⌨️ Keybindings

> All shortcuts work out of the box. No configuration needed.

<details>
<summary><b>General</b></summary>

| Shortcut | Action |
|---|---|
| `Ctrl + T` | New tab |
| `Ctrl + W` | Close tab |
| `Ctrl + Shift + T` | Reopen closed tab |
| `Ctrl + Tab` | Next tab |
| `Ctrl + Shift + Tab` | Previous tab |
| `Ctrl + L` | Focus address bar |
| `Ctrl + D` | Bookmark page |
| `Ctrl + H` | History |
| `Ctrl + J` | Downloads |
| `F5` | Reload |
| `Ctrl + Shift + R` | Hard reload |
| `F11` | Fullscreen |
| `F12` | DevTools |

</details>

<details>
<summary><b>Privacy & Tools</b></summary>

| Shortcut | Action |
|---|---|
| `Ctrl + Shift + N` | Incognito window |
| `Ctrl + Shift + P` | Command palette |
| `Ctrl + Shift + S` | Screenshot |
| `Ctrl + Shift + B` | Toggle bookmarks |

</details>

---

## 🏗 Architecture

```
┌──────────────────────────────────────────────────────────┐
│                     Main Process                         │
│                      (main.js)                           │
│                                                          │
│  ┌─────────────┐  ┌──────────────┐  ┌────────────────┐  │
│  │ electron-   │  │  Session &   │  │   Protocol     │  │
│  │ store       │  │  webRequest  │  │   Handler      │  │
│  │ (settings,  │  │  (blocking,  │  │   (privabrowse │  │
│  │  stats,     │  │   200+ hdr   │  │   :// pages)   │  │
│  │  vault)     │  │   poisoning) │  │                │  │
│  └─────────────┘  └──────────────┘  └────────────────┘  │
│                         │                                │
│                    IPC Bridge                             │
│               (ipcMain.handle)                           │
└────────────────────────┬─────────────────────────────────┘
                         │
                   contextBridge
                   (preload.js)
                         │
┌────────────────────────┴─────────────────────────────────┐
│                   Renderer Process                        │
│                                                          │
│  ┌──────────────────────────────────────────────────┐    │
│  │                  index.html                       │    │
│  │  ┌──────────┐ ┌───────────┐ ┌──────────────────┐│    │
│  │  │ Tab Bar  │ │ Address   │ │ Hamburger Menu   ││    │
│  │  │          │ │ Bar       │ │ (tools, panels)  ││    │
│  │  └──────────┘ └───────────┘ └──────────────────┘│    │
│  │  ┌──────────────────────────────────────────────┐│    │
│  │  │              <webview>                       ││    │
│  │  │   (web pages / internal privabrowse:// )     ││    │
│  │  └──────────────────────────────────────────────┘│    │
│  └──────────────────────────────────────────────────┘    │
│                                                          │
│  renderer.js — UI logic, tabs, settings, achievements,   │
│               60+ fingerprint vectors, storage cleanup,  │
│               STORAGE_CLEANUP_SCRIPT, MISC_PRIVACY_SCRIPT│
│  styles.css  — dark glass design system                  │
└──────────────────────────────────────────────────────────┘
```

---

## 📁 Project Structure

```
privabrowse/
├── main.js                    Main process — app lifecycle, IPC, blocking engine,
│                              sessions, downloads, protocol handler
├── preload.js                 Context bridge — exposes window.privabrowse API
├── package.json               Dependencies, build config, electron-builder
│
├── src/
│   ├── renderer/
│   │   ├── index.html         Browser shell — tab bar, address bar, panels
│   │   ├── renderer.js        All renderer logic — tabs, settings, achievements
│   │   ├── styles.css         Dark glass design system + light mode
│   │   ├── welcome.html       Interactive onboarding tutorial
│   │   ├── settings.html      Searchable settings panel (90+ options)
│   │   ├── newtab.html        New tab page — search, challenges, level bar
│   │   ├── about.html         About page + update checker
│   │   ├── stats.html         Privacy stats dashboard
│   │   ├── history.html       Browsing history
│   │   ├── sessions.html      Tab workspace manager
│   │   ├── games.html         Built-in browser games
│   │   ├── speedtest.html     Network speed test
│   │   ├── graveyard.html     Tracker graveyard visualization
│   │   ├── adblocking.html    Ad blocking controls
│   │   ├── docs.html          Documentation
│   │   ├── api-tester.html    REST API testing tool
│   │   ├── recorder.html      Screen recorder
│   │   ├── puzzle.html        Puzzle game
│   │   ├── parental.html      Parental controls
│   │   ├── privasearch.html   Privacy-respecting search
│   │   ├── terms.html         Terms of service
│   │   ├── privacy.html       Privacy policy
│   │   ├── contact.html       Contact page
│   │   ├── transparency.html  Transparency report
│   │   └── fingerprinting.html Fingerprint protection details
│   │
│   └── blocklist.js           Ad/tracker domain lists and URL patterns
│
├── build/
│   └── license.txt            Installer license text
│
├── LICENSE.md                 MIT License + EULA
├── TRANSPARENCY.md            Transparency report source
└── RELEASE_NOTES_v1.1.0.md   v1.1.0 release notes
```

---

## 🔨 Building from Source

### Requirements

- [Node.js](https://nodejs.org/) 18+
- [npm](https://www.npmjs.com/) 9+
- Windows 10/11 x64 (for building Windows targets)

### Install & Run

```bash
git clone https://github.com/omfghello/priva-browser.git
cd priva-browser
npm install
npm start
```

### Build Installer + Portable

```bash
# Build all Windows targets (installer + portable + unpacked)
npm run dist:win

# Build installer only
npm run dist:installer

# Output: dist/PrivaBrowse Setup 1.1.0.exe
#         dist/PrivaBrowse 1.1.0.exe (portable)
```

### Build Scripts

| Command | Description |
|---|---|
| `npm start` | Run in development mode |
| `npm run dist` | Build for current platform |
| `npm run dist:win` | Build all Windows targets |
| `npm run dist:installer` | Build NSIS installer only |

---

## 💻 System Requirements

| | Minimum | Recommended |
|---|---|---|
| **OS** | Windows 10 (x64) | Windows 10/11 (x64) |
| **CPU** | Dual core, 1.5 GHz | Quad core, 2.5 GHz+ |
| **RAM** | 4 GB | 8 GB+ |
| **Disk** | 500 MB | 1 GB+ |
| **GPU** | DirectX 11 capable | Hardware acceleration supported |
| **Network** | Required for browsing | — |

---

## 📥 Installation

### Installer (Recommended)

Download **[PrivaBrowse Setup 1.1.0.exe](https://github.com/omfghello/priva-browser/releases/tag/v_1_0_release)** from the releases page. Run it, choose your install directory, and you're done. Creates desktop and Start Menu shortcuts.

### Portable

Download **[PrivaBrowse 1.1.0.exe](https://github.com/omfghello/priva-browser/releases/tag/v_1_0_release)** — no installation required. Runs from anywhere, including USB drives. Settings are stored in the same directory.

---

## 📝 License

PrivaBrowse is licensed under the **MIT License**. See [LICENSE.md](LICENSE.md) for the full license text.

```
MIT License — Copyright (c) 2025-2026 PrivaBrowse Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, subject to the condition that the copyright notice
and permission notice are included in all copies or substantial portions.
```

---

<div align="center">

Built with caffeine, paranoia, and an unreasonable number of `rgba()` values.

**[Download](https://github.com/omfghello/priva-browser/releases/tag/v_1_0_release) · [Report a Bug](../../issues) · [Request a Feature](../../issues) · [Release Notes](RELEASE_NOTES_v1.1.0.md)**

<br/>

[![MIT License](https://img.shields.io/badge/license-MIT-a6e3a1?style=flat-square)](LICENSE.md)
[![Latest Release](https://img.shields.io/badge/latest-v1.1.0-6366f1?style=flat-square)](https://github.com/omfghello/priva-browser/releases/tag/v_1_0_release)

</div>
