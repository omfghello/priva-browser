
# PrivaBrowse

<p align="center">
  <strong>PrivaBrowse</strong>
</p>
<p align="center">
  A privacy-focused Chromium-based desktop browser. Blocks ads & trackers. No telemetry. Free forever.
</p>
<p align="center">
  <a href="#what-is-privabrowse">About</a> •
  <a href="#download">Download</a> •
  <a href="#features">Features</a> •
  <a href="#quick-start">Quick Start</a> •
  <a href="#build">Build</a> •
  <a href="#project-structure">Structure</a>
</p>

---

## What is PrivaBrowse?

**PrivaBrowse** is a desktop browser built with Electron and Chromium. It behaves like a normal browser (tabs, address bar, bookmarks, history) but is focused on privacy and control.

### Core idea

- **Blocks ads and trackers** before they load (Off / Standard / Aggressive)
- **No telemetry** — the app does not send data to any server
- **Clear on exit** — optionally wipe cookies, cache, and storage when you close
- **AnimeVault** — search and download anime episodes (SUB/DUB)
- **MangaVault** — read manga from MangaDex in a built-in reader
- **Password Vault** — encrypted password storage, data stays on your device
- **Userscripts** — run custom JavaScript on sites (like Greasemonkey)

No account. No premium tier. No data collection. **Completely free.**

### Who is it for?

- Users who want ad and tracker blocking without extensions
- People who prefer local control over their data
- Anyone who wants a simple, privacy-focused browser
- Users who want built-in anime downloads and manga reading

---

## Download

| Platform | File | Link |
|----------|------|------|
| Windows 64-bit | Installer | [Releases](https://github.com/omfghello/priva-browser/releases/tag/installer) → `PrivaBrowse Setup 1.0.0.exe` |
| Windows 64-bit | Portable |  [Releases](https://github.com/omfghello/priva-browser/releases/tag/installer) → `PrivaBrowse 1.0.0.exe` |

**Installer:** Standard setup wizard, desktop shortcut, Start Menu entry.  
**Portable:** Single `.exe`, no install; run from a folder or USB drive.

---

## Features

### Privacy & security

| Feature | Description |
|---------|-------------|
| **Shields** | Block ads, trackers, social widgets, crypto miners. Three levels: Off, Standard, Aggressive. |
| **Tracker Graveyard** | Log of every blocked domain and how many times it was blocked. |
| **Clear on exit** | Optionally clear cookies, cache, and storage when the browser closes. |
| **Do Not Track** | Sends DNT header with requests. |
| **Prefer HTTPS** | Upgrades HTTP to HTTPS when possible. |
| **Third-party cookies** | Option to block third-party cookies. |

### Built-in tools

| Feature | Description |
|---------|-------------|
| **Password Vault** | Encrypted password storage with a master password. Data stays on your device. |
| **Sessions** | Save and restore named sets of tabs. |
| **Focus Mode** | Block distracting sites for a set time (e.g. 25 min). |
| **Reader Mode** | Strip clutter from articles for clean reading. |
| **Screenshot** | Capture full page or visible area. |
| **Games** | Built-in Snake, 2048, Wordle. |
| **Speed Test** | In-browser bandwidth test. |

### Customization

| Feature | Description |
|---------|-------------|
| **Userscripts** | Add custom JavaScript to run on matching sites (like Greasemonkey). |
| **Custom CSS** | Inject CSS rules per site. |
| **Search engine** | DuckDuckGo, Google, Bing, Ecosia, Startpage, Qwant, Brave Search, Yahoo. |
| **Home page** | Set any URL as the home page. |
| **Permissions** | Toggle camera, microphone, notifications per site. |

---

## Quick Start

**Requirements:** Windows 10/11 (64-bit), Node.js 18+, npm
