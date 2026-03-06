<div align="center">

```
███████╗ █████╗  ██████╗
██╔════╝██╔══██╗██╔═══██╗
█████╗  ███████║██║   ██║
██╔══╝  ██╔══██║██║▄▄ ██║
██║     ██║  ██║╚██████╔╝
╚═╝     ╚═╝  ╚═╝ ╚═▀▀═╝
```

**PrivaBrowse — Frequently Asked Questions**

[![Version](https://img.shields.io/badge/version-1.1.0-6366f1?style=flat-square)](RELEASE_NOTES_v1.1.0.md)
[![License](https://img.shields.io/badge/license-MIT-a6e3a1?style=flat-square)](LICENSE.md)

</div>

---

## Table of Contents

**General**
- [What is PrivaBrowse?](#what-is-privabrowse)
- [Is PrivaBrowse free?](#is-privabrowse-free)
- [What platforms are supported?](#what-platforms-are-supported)
- [Why Electron? Isn't Chrome/Chromium part of the problem?](#why-electron-isnt-chromechromium-part-of-the-problem)
- [How do I install it?](#how-do-i-install-it)
- [Is there a portable version?](#is-there-a-portable-version)
- [How do I update PrivaBrowse?](#how-do-i-update-privabrowse)
- [Can I build from source?](#can-i-build-from-source)

**Privacy & Security**
- [What data does PrivaBrowse collect?](#what-data-does-privabrowse-collect)
- [Is my data stored anywhere?](#is-my-data-stored-anywhere)
- [Can I export or import my data?](#can-i-export-or-import-my-data)
- [How is this different from uBlock Origin + Firefox?](#how-is-this-different-from-ublock-origin--firefox)
- [How is this different from Brave?](#how-is-this-different-from-brave)
- [Does PrivaBrowse break websites?](#does-privabrowse-break-websites)
- [Does YouTube work?](#does-youtube-work)
- [What is data poisoning and why do you do it?](#what-is-data-poisoning-and-why-do-you-do-it)
- [Does data poisoning slow down my browsing?](#does-data-poisoning-slow-down-my-browsing)
- [What is "smart header poisoning"?](#what-is-smart-header-poisoning)
- [What is fingerprint spoofing?](#what-is-fingerprint-spoofing)
- [What are supercookies and how does PrivaBrowse protect against them?](#what-are-supercookies-and-how-does-privabrowse-protect-against-them)
- [What is bounce tracking and how is it blocked?](#what-is-bounce-tracking-and-how-is-it-blocked)
- [What is the Privacy Sandbox and why do you block it?](#what-is-the-privacy-sandbox-and-why-do-you-block-it)
- [What is network state partitioning?](#what-is-network-state-partitioning)
- [Why did you remove Google, Bing, and Yahoo?](#why-did-you-remove-google-bing-and-yahoo)
- [Does PrivaBrowse protect me from malware?](#does-privabrowse-protect-me-from-malware)
- [Does PrivaBrowse replace a VPN?](#does-privabrowse-replace-a-vpn)
- [Is PrivaBrowse as private as Tor Browser?](#is-privabrowse-as-private-as-tor-browser)
- [How can I verify PrivaBrowse is doing what it claims?](#how-can-i-verify-privabrowse-is-doing-what-it-claims)

**Features**
- [What search engines are available?](#what-search-engines-are-available)
- [How does the ad blocker work?](#how-does-the-ad-blocker-work)
- [How many domains are blocked?](#how-many-domains-are-blocked)
- [What URL tracking parameters are stripped?](#what-url-tracking-parameters-are-stripped)
- [What is the password vault? Is it secure?](#what-is-the-password-vault-is-it-secure)
- [What is PrivaForge?](#what-is-privaforge)
- [What is the Tracker Graveyard?](#what-is-the-tracker-graveyard)
- [What are achievements?](#what-are-achievements)
- [What is DNS-over-HTTPS (DoH)?](#what-is-dns-over-https-doh)
- [What is Focus Mode?](#what-is-focus-mode)
- [What is Data Saver Mode?](#what-is-data-saver-mode)
- [What is Reader Mode?](#what-is-reader-mode)
- [Can I use extensions?](#can-i-use-extensions)
- [Can I inject custom CSS or JavaScript?](#can-i-inject-custom-css-or-javascript)

**Troubleshooting**
- [A website is broken. What do I do?](#a-website-is-broken-what-do-i-do)
- [Ctrl+C and Ctrl+V aren't working](#ctrlc-and-ctrlv-arent-working)
- [A yellow bar appears when I select text](#a-yellow-bar-appears-when-i-select-text)
- [Videos won't play](#videos-wont-play)
- [The browser feels slow](#the-browser-feels-slow)
- [A site says I need to disable my ad blocker](#a-site-says-i-need-to-disable-my-ad-blocker)
- [How do I clear all my data?](#how-do-i-clear-all-my-data)
- [How do I reset to default settings?](#how-do-i-reset-to-default-settings)

---

## General

### What is PrivaBrowse?

PrivaBrowse is a **privacy-first Chromium browser** built with Electron for Windows x64. It ships with aggressive ad blocking (2,100+ domains), tracker annihilation, fingerprint spoofing (60+ vectors), data poisoning, DNS-over-HTTPS, custom developer tools (PrivaForge), and over 90 configurable settings — all enabled out of the box. No telemetry, no data collection, no phone-home.

### Is PrivaBrowse free?

Yes. PrivaBrowse is free and open source under the [MIT License](LICENSE.md). No premium tier, no paid features, no subscriptions.

### What platforms are supported?

Currently **Windows 10/11 (x64)** only. The codebase is Electron-based, so macOS and Linux builds are theoretically possible but are not officially supported or tested at this time.

### Why Electron? Isn't Chrome/Chromium part of the problem?

Chromium is the rendering engine — it's how web pages display. The tracking infrastructure built on top of it (Google Sync, Safe Browsing telemetry, usage metrics) is what we strip out. We chose Electron because it gives us full control over the network stack via `webRequest` APIs, which is how all our blocking, poisoning, and header modification works. Chromium's built-in telemetry is disabled at the Electron configuration level. No Google services are contacted.

### How do I install it?

Download `PrivaBrowse Setup 1.1.0.exe` from the [releases page](https://github.com/omfghello/priva-browser/releases/tag/v_1_0_release), run it, and follow the prompts. It creates desktop and Start Menu shortcuts.

### Is there a portable version?

Yes. Download `PrivaBrowse 1.1.0.exe` from the releases page — no installation required. Run it from anywhere, including USB drives. Settings are stored alongside the executable.

### How do I update PrivaBrowse?

Download the latest installer or portable executable from the releases page and run it. Your settings, bookmarks, and vault data persist across updates because they're stored in Electron's user data directory, not inside the application folder.

### Can I build from source?

Yes. Clone the repo, run `npm install`, then `npm start` for development or `npm run dist:win` to build the installer and portable executable. All dependencies are listed in `package.json` and are auditable via `npm audit`. See the [README](README.md) for full build instructions.

---

## Privacy & Security

### What data does PrivaBrowse collect?

**None. Zero. Nothing.**

- No telemetry
- No crash reporting (no Sentry, no Bugsnag, no New Relic)
- No usage statistics
- No analytics endpoints
- No A/B testing
- No "anonymous" metrics
- No update tracking with identifying data
- No remote configuration or feature flags
- No data sales (we have no data to sell)
- No partnerships with ad companies

There is no server. There is no backend. There is nothing to phone home to.

### Is my data stored anywhere?

Your browsing data, history, bookmarks, settings, and password vault entries are stored **locally on your machine** in Electron's standard user data directory. Nothing is sent anywhere. Nothing is synced. If you delete the app, the data goes with it.

### Can I export or import my data?

Settings and blocklists are stored in standard JSON format in your user data directory. They're yours to back up, move, or delete at any time. The password vault is encrypted locally and can be backed up by copying the store file.

### How is this different from uBlock Origin + Firefox?

uBlock Origin is excellent at blocking. PrivaBrowse blocks **and** poisons. When a tracker slips past the blocklist (and some will), it collects garbage data instead of your real information. The data poisoning engine, smart header poisoning, 60+ fingerprint spoofing vectors, network state partitioning, Privacy Sandbox blocking, bounce tracking detection, HSTS/favicon supercookie protection, and stale cookie cleanup are features no extension can replicate at the same depth because extensions don't have access to the network stack the way Electron's main process does.

Additionally, PrivaBrowse ships with custom developer tools (PrivaForge), a built-in encrypted password vault, ambient sounds, achievements, a tracker graveyard, and 22 built-in pages — none of which are possible with a browser extension.

### How is this different from Brave?

Brave is a solid privacy browser, but PrivaBrowse goes further in several areas:

| Feature | PrivaBrowse | Brave |
|---|:---:|:---:|
| Data poisoning engine | **Yes** | No |
| Smart header poisoning | **Yes** | No |
| 60+ fingerprint spoofing vectors | **Yes** | Partial |
| Privacy Sandbox API blocking | **Yes** | Partial |
| HSTS supercookie protection | **Yes** | No |
| Favicon supercookie protection | **Yes** | No |
| Network state partitioning | **Yes** | No |
| Stale cookie auto-cleanup | **Yes** | No |
| Local CDN privacy | **Yes** | No |
| WebSocket/EventSource interception | **Yes** | No |
| Surveillance search engines removed | **Yes** | No |
| Zero telemetry | **Yes** | No (has opt-out telemetry) |
| Custom dev tools (PrivaForge) | **Yes** | No |
| Built-in password vault | **Yes** | No |
| Achievements & gamification | **Yes** | No |

Brave also includes its own cryptocurrency (BAT) and ad reward system, which PrivaBrowse considers a conflict of interest for a privacy browser. We have no advertising model of any kind.

### Does PrivaBrowse break websites?

Occasionally, yes. Aggressive blocking and fingerprint spoofing can interfere with sites that depend heavily on trackers for core functionality (ironic, but real). Common breakage scenarios:

- **CAPTCHA challenges** — Some may require multiple attempts due to fingerprint spoofing
- **Payment processors** — Stripe/PayPal may occasionally need a retry
- **Single sign-on** — OAuth flows through Google/Facebook may behave differently
- **Sites that require cookies** — Third-party cookie blocking can break some login flows

For these cases, PrivaBrowse provides per-site settings so you can relax protections on specific domains without compromising your global defaults.

### Does YouTube work?

**Yes.** YouTube received special handling to ensure compatibility:

- Video playback works normally
- Search works normally
- Comments, likes, and subscriptions work normally
- YouTube's tracking endpoints are still fully poisoned — YouTube does not get a free pass on tracking
- Cookie consent flows are preserved (earlier versions accidentally blocked them)

If you experience YouTube issues, try hard-reloading the page with `Ctrl+Shift+R`.

### What is data poisoning and why do you do it?

Blocking tells a tracker "this user has an ad blocker." That itself is a data point. **Poisoning** tells the tracker nothing useful — every field looks plausible but is completely fabricated. The tracker's dataset becomes polluted, their models degrade, and they can't distinguish real users from noise.

PrivaBrowse intercepts every major data exfiltration API (`fetch()`, `XMLHttpRequest`, `navigator.sendBeacon()`, image pixels, form submissions, WebSocket payloads) and injects 55+ fake data fields including: email addresses, full names, phone numbers, IP addresses, device IDs, session IDs, fingerprint hashes, screen resolutions, languages, timezones, geographic coordinates, and more.

**No company is excluded from data poisoning. Not Google. Not Facebook. Not Microsoft. Not Amazon. No one.**

### Does data poisoning slow down my browsing?

No measurable impact. The poisoning logic intercepts requests that are already being made to tracker endpoints and rewrites their payloads. It doesn't add new network requests — it corrupts existing ones. Earlier versions injected 200+ headers on every tracker request, which caused performance issues; this was refactored to a "smart" approach that only randomizes headers the request already carries.

### What is "smart header poisoning"?

Instead of injecting hundreds of new HTTP headers on every tracker request (which caused performance degradation and website breakage), PrivaBrowse now uses a **smart** approach:

- Only randomizes headers the request **already carries** (User-Agent, Accept-Language, Referer, Origin)
- Spoofs IP-related headers (X-Forwarded-For, X-Real-IP, etc.) only if the tracker already sends them
- Poisons correlation/trace IDs (X-Request-ID, Traceparent, Sentry-Trace, etc.) only if present
- Strips cookies, authorization tokens, and Sec-Fetch headers from all tracker requests
- Adds cache-busting headers to prevent ETag fingerprinting

This is stealthier (trackers can't detect unusual header counts), faster (no bulk injection overhead), and avoids breaking websites.

### What is fingerprint spoofing?

Browser fingerprinting is a technique where trackers collect dozens of unique data points about your browser (canvas rendering, WebGL info, audio processing, screen size, installed fonts, etc.) to create a unique identifier that persists even after you clear cookies.

PrivaBrowse spoofs **60+ fingerprinting vectors** per session, including:

- Canvas (`toDataURL`, `toBlob`, `getImageData`) — subtle noise injection
- WebGL (renderer/vendor strings, shader precision)
- AudioContext (frequency data noise)
- Navigator (hardwareConcurrency, deviceMemory, platform, plugins)
- Screen dimensions, color depth, pixel depth
- ClientRects (sub-pixel noise)
- Timing precision reduction
- Font enumeration blocking
- MediaDevices randomization
- And 50+ more

Every vector is randomized per-session so you can't be tracked across sites.

### What are supercookies and how does PrivaBrowse protect against them?

Supercookies are tracking mechanisms that survive cookie clearing. Common types:

| Type | How It Works | PrivaBrowse Defense |
|---|---|---|
| **ETag** | Server stores a unique ID in the ETag header and reads it back | ETag headers stripped from both requests and responses |
| **HSTS** | Tracker sets HSTS on a matrix of subdomains to encode a unique ID | HSTS resolver cache cleared on startup + every 30 minutes |
| **Favicon** | Favicon cache entries encode tracking bits across visits | Favicon cache cleared periodically |
| **Client Hints** | `Sec-CH-*` headers reveal device details | All Client Hints stripped/spoofed |

### What is bounce tracking and how is it blocked?

Bounce tracking is when a tracker redirects you through their domain for just long enough to set a cookie, then sends you to your actual destination. You barely see it happen — it's a fraction-of-a-second redirect.

PrivaBrowse detects this by tracking navigation timestamps. If a domain redirects within **1.5 seconds** without user interaction, it's flagged as a bounce tracker, and all cookies and storage for that domain are immediately purged.

### What is the Privacy Sandbox and why do you block it?

Google's Privacy Sandbox is marketed as "privacy-preserving" advertising technology, but it still enables interest-based ad targeting within the browser itself. PrivaBrowse blocks all Privacy Sandbox APIs:

- **FLEDGE / Protected Audience** — On-device ad auctions
- **Topics API** — Browser-assigned interest categories based on your browsing
- **Attribution Reporting** — Cross-site conversion tracking
- **Shared Storage** — Cross-site data storage for ad purposes
- **Fenced Frames** — Isolated frames for rendering ads
- **Private Aggregation** — Aggregate measurement for ads

These are blocked via Chromium `disable-features` flags and `Permissions-Policy` response headers. We believe advertising infrastructure does not belong inside a browser, regardless of how it's packaged.

### What is network state partitioning?

Normally, browsers share network state (caches, connection pools, DNS lookups, SSL sessions) across all websites. This means Site A can detect whether you've visited Site B by probing shared resources.

PrivaBrowse enables Chromium's isolation features so that all network state is **partitioned per top-level site**:

- HTTP cache isolated per site
- TCP/QUIC connections isolated per site
- SSL sessions isolated per site
- DNS cache isolated per site
- Network Error Logging isolated per site

This prevents a wide class of cross-site tracking attacks.

### Why did you remove Google, Bing, and Yahoo?

Their search result pages load dozens of tracking scripts that conflict with our blocking engine and degrade the browsing experience. More fundamentally, a privacy browser should not route your most intimate data — your search queries — through the world's largest advertising companies.

Available engines: DuckDuckGo (default), Brave Search, Startpage, Qwant, SearXNG, Mojeek, MetaGer, Swisscows, Yep, Ecosia, and PrivaBrowse's own PrivaSearch.

### Does PrivaBrowse protect me from malware?

**No.** PrivaBrowse is a privacy tool, not an antivirus. It blocks trackers, not malicious payloads. You should still run proper security software. PrivaBrowse does, however, block 84+ cryptocurrency mining domains to prevent cryptojacking.

### Does PrivaBrowse replace a VPN?

**No.** PrivaBrowse encrypts your DNS queries (via DNS-over-HTTPS) and prevents browser-level tracking, but your ISP can still see which domains you connect to. For network-level privacy, use a VPN or Tor alongside PrivaBrowse.

### Is PrivaBrowse as private as Tor Browser?

**No.** Tor Browser routes all traffic through the Tor network, providing network-level anonymity. PrivaBrowse focuses on **browser-level** privacy — blocking trackers, spoofing fingerprints, poisoning data, and preventing cross-site tracking. For maximum privacy, use both: Tor for anonymity, PrivaBrowse for anti-tracking on your daily browsing.

### How can I verify PrivaBrowse is doing what it claims?

Don't take our word for it. Verify everything yourself:

1. **[Cover Your Tracks](https://coveryourtracks.eff.org/)** by the EFF — Test your browser's fingerprint uniqueness
2. **[BrowserLeaks](https://browserleaks.com/)** — Detailed fingerprint analysis for Canvas, WebGL, Audio, Fonts, and more
3. **[AmIUnique](https://amiunique.org/)** — Check your fingerprint uniqueness
4. **[CanvasBlocker Test](https://canvasblocker.kkapsner.de/test/)** — Verify canvas protection
5. **[WebRTC Leak Test](https://browserleaks.com/webrtc)** — Confirm no IP leaks
6. **[DNS Leak Test](https://dnsleaktest.com/)** — Verify DNS-over-HTTPS is working

You can also audit the code directly — every protection is implemented in plain JavaScript with no obfuscation. See the [Transparency Report](TRANSPARENCY.md) for a file-by-file audit guide.

---

## Features

### What search engines are available?

All privacy-respecting, no surveillance-based engines:

| Engine | Notes |
|---|---|
| **DuckDuckGo** (default) | No tracking, no profile building |
| **Brave Search** | Independent index, no Big Tech dependency |
| **Startpage** | Anonymous proxy to search results |
| **Qwant** | EU-based privacy-first engine |
| **SearXNG** | Open-source metasearch aggregator |
| **Mojeek** | Own crawler, no tracking |
| **MetaGer** | German non-profit metasearch |
| **Swisscows** | Swiss privacy, family-safe |
| **Yep** | Independent index by Ahrefs |
| **Ecosia** | Plants trees, minimal data collection |
| **PrivaSearch** | PrivaBrowse's own privacy search |

### How does the ad blocker work?

PrivaBrowse uses a multi-layer blocking system:

1. **Fast-path host blocking** — O(1) `Set` lookup against 740+ domains
2. **Extended domain lists** — 408 ad domains, 446 tracker domains, 358+ aggressive domains
3. **Social tracker blocking** — 112 Facebook/Twitter/LinkedIn/Google tracking domains
4. **Crypto miner blocking** — 84 mining domains
5. **URL pattern matching** — 100+ regex patterns for ad network URLs
6. **YouTube ad interception** — Blocks pre-roll ad requests and tracking pixels
7. **Cosmetic filtering** — Injected script removes ad containers from the rendered page
8. **CDN tracking blocking** — Known tracking scripts served via CDNs are detected and blocked
9. **Script injection** — Ad-nuking scripts for elements that bypass network blocking

All blocking happens at the network level in the main process before requests leave your machine.

### How many domains are blocked?

**2,100+ unique domains** across all lists:

- 740+ fast-block hosts (O(1) lookup)
- 408 ad-serving domains
- 446 tracker domains
- 358+ aggressive-mode domains
- 112 social tracker domains
- 84 crypto miner domains

Plus 100+ URL patterns for ad network detection and CDN tracking script patterns.

### What URL tracking parameters are stripped?

**110+ tracking parameters** are stripped from URLs on both navigation and in-page link clicks:

`utm_source`, `utm_medium`, `utm_campaign`, `utm_term`, `utm_content`, `utm_id`, `fbclid`, `gclid`, `gclsrc`, `msclkid`, `mc_eid`, `oly_enc_id`, `oly_anon_id`, `__hssc`, `__hstc`, `__hsfp`, `_hsenc`, `vero_id`, `wickedid`, `_ttp` (TikTok), `tt_medium`, `tt_content`, `twclid` (Twitter), `rdt_cid` (Reddit), `li_fat_id` (LinkedIn), `tag`/`ascsubtag` (Amazon), `epik` (Pinterest), and 80+ more.

Parameters are stripped from:
- Top-level navigations (`will-navigate` handler)
- In-page link clicks (via `click`/`auxclick` event hooks + `MutationObserver` on dynamically added links)

### What is the password vault? Is it secure?

PrivaBrowse includes a built-in encrypted password vault:

- **Encryption** — All entries encrypted locally with a master password
- **Zero-knowledge** — Your master password never leaves your machine; we cannot recover it
- **No cloud sync** — Vault data stays on your device
- **No third-party dependencies** — Implemented entirely within PrivaBrowse, not outsourced to an extension

The vault is accessible from the hamburger menu and stores credentials in Electron's encrypted user data store.

### What is PrivaForge?

**PrivaForge** is PrivaBrowse's custom developer tools panel — a completely original, cyberpunk-themed toolkit that replaces Chrome DevTools. Opens with **F12** or **Ctrl+Shift+I**.

| Panel | What It Does |
|---|---|
| **Terminal** | Hacker-style JS console. Built-in commands: `dom.count`, `dom.links`, `perf.timing`, `privacy.scan`, `storage.local`, `fun.rainbow`, `fun.party`, and more. Arrow-key history, tab-autocomplete |
| **X-Ray** | DOM inspector with collapsible node tree. **Pick Element** mode with neon green overlay — click to inspect box model, computed styles, attributes |
| **Pulse** | Real-time network monitor via PerformanceObserver. Visual waterfall bars, filterable by resource type |
| **Vault** | Storage explorer for localStorage, sessionStorage, and cookies. Search, inspect, delete |
| **Paint** | Live CSS editor with instant injection. Quick snippets: Box Outlines, Dark Mode, Rainbow, Hover Zoom |
| **Scan** | Site audit scoring Performance, Accessibility, Privacy, and SEO (0–100 each) |

Design: Dark cyberpunk aesthetic (`#08090d` + neon `#00ffaa`), scanline animations, monospace fonts (Cascadia Code, JetBrains Mono), resizable panel.

### What is the Tracker Graveyard?

A visual dashboard (`privabrowse://graveyard`) showing every tracker domain that PrivaBrowse has blocked, with per-domain request counts. It's a graveyard of dead trackers — see exactly how many tracking requests were stopped and from which sites.

### What are achievements?

PrivaBrowse turns privacy into a game. **38 achievements** across 5 categories:

| Category | Count | Examples |
|---|---|---|
| Privacy | 12 | Block 10/100/1K/10K/50K ads, achieve A+ privacy score |
| Streaks | 5 | Use PrivaBrowse 3/7/30/100/365 days in a row |
| Explorer | 7 | Save bookmarks, use reader mode, take screenshots |
| Power User | 12 | Use split view, PiP, focus mode, ambient sounds, PrivaForge |
| Secrets | 2 | Hidden — keep exploring |

Achievements have rarity tiers (Common, Rare, Epic, Legendary) with distinct visual treatments, and a leveling system from Newcomer to Mythic.

### What is DNS-over-HTTPS (DoH)?

Standard DNS queries are sent in plaintext, allowing your ISP and network operators to see every domain you visit. **DNS-over-HTTPS** encrypts these queries so only your chosen DNS provider can see them.

PrivaBrowse supports: Cloudflare (`1.1.1.1`), Quad9, NextDNS, or a custom DoH URL. DoH providers can be changed at runtime without restarting the browser.

### What is Focus Mode?

A productivity feature that blocks distracting websites for a set time period. Define your own blocklist of distracting domains, set a timer, and the sites become inaccessible until it expires. Enforced at the network level — no workarounds.

### What is Data Saver Mode?

Reduces page weight for slow connections by blocking:
- Third-party fonts (also a privacy win — font loading reveals your OS)
- Animated images
- Autoplay media
- Large third-party resources

### What is Reader Mode?

Strips pages to pure text for distraction-free reading. Removes ads, sidebars, navigation, and other clutter, leaving only the article content. Also includes an **RSVP speed reader** for reading articles word-by-word at configurable speeds.

### Can I use extensions?

Not currently. PrivaBrowse is built on Electron, which does not support Chrome/Firefox extensions. However, most functionality that extensions provide (ad blocking, fingerprint protection, password management, cookie control) is already built into PrivaBrowse at a deeper level than any extension can achieve.

### Can I inject custom CSS or JavaScript?

Yes. Go to **Settings → Advanced**:

- **Custom CSS** — Write CSS rules that get injected into every page you visit
- **User Scripts** — Inject custom JavaScript into pages

This is useful for custom styling, accessibility tweaks, or site-specific fixes.

---

## Troubleshooting

### A website is broken. What do I do?

Try these steps in order:

1. **Hard reload** — `Ctrl+Shift+R` clears cached scripts
2. **Check PrivaForge's Scan tab** — Open with F12 and run a site audit to see what's being blocked
3. **Temporarily lower shields** — Some sites require third-party cookies or specific APIs to function
4. **Clear site data** — Go to Settings → Data → Clear browsing data for that specific site
5. **Report the issue** — File a GitHub issue with the URL and what's broken so we can add a compatibility fix

### Ctrl+C and Ctrl+V aren't working

**This was fixed in v1.1.0.** The issue was that `Menu.setApplicationMenu(null)` in the main process removed Electron's default Edit menu, which provides the native Ctrl+C/V/X/A accelerators. The fix replaced the null menu with a proper hidden menu that includes all Edit role items.

If you're still experiencing this on an older version, update to v1.1.0.

### A yellow bar appears when I select text

**This was fixed in v1.1.0.** The `PAGE_ANNOTATE_SCRIPT` was being auto-injected into every page and wrapping any selected text (3–500 characters) in a yellow `<mark>` element. The auto-injection was removed.

If you're still seeing this on an older version, update to v1.1.0.

### Videos won't play

If videos aren't playing:

1. **Check if the site is blocked** — Some video CDNs may be caught by aggressive blocking
2. **Disable Data Saver Mode** — Settings → Network → Data Saver Mode (this blocks autoplay media)
3. **Check autoplay settings** — Settings → Blocking → Block Autoplay may be preventing playback
4. **Try hard reload** — `Ctrl+Shift+R`

YouTube videos specifically should work without issues. If YouTube is broken, please file a bug report.

### The browser feels slow

Performance was significantly improved in v1.1.0 with these optimizations:

- **Blocklist lookups** optimized from O(n) iteration to O(1) `Set` lookups for hostnames and a single compiled `RegExp` for URL patterns
- **Header poisoning** refactored from injecting 200+ headers to only randomizing existing ones
- **URL parsing** consolidated to eliminate redundant `new URL()` calls per request

If performance is still an issue:

1. **Close unused tabs** — Tab sleep helps, but closing is better
2. **Check RAM usage** — Settings → Performance → RAM limit
3. **Disable ambient sounds** if running
4. **Try clearing cache** — Settings → Data → Clear Cache

### A site says I need to disable my ad blocker

Some sites detect ad blockers. PrivaBrowse's approach of poisoning rather than just blocking makes detection harder, but some sites still notice missing ad containers. Options:

1. **Ignore it** — Many sites still work fine despite the warning
2. **Try Reader Mode** — `Ctrl+Shift+R` from address bar for article sites
3. **Temporarily adjust settings** for that specific site

### How do I clear all my data?

Go to **Settings → Data** and use the clear options:

- Clear browsing history
- Clear cookies and site data
- Clear cache
- Clear form data
- Clear downloads

Or enable **Clear on Exit** options to automatically wipe data when you close the browser.

You can also use the keyboard shortcut or go to `privabrowse://settings` and search for "clear."

### How do I reset to default settings?

Delete the settings file from Electron's user data directory. The location is typically:

```
%APPDATA%\privabrowse\config.json
```

On next launch, PrivaBrowse will recreate the settings file with all defaults.

---

<div align="center">

**Still have questions?**

**[Report a Bug](https://github.com/omfghello/priva-browser/issues) · [Request a Feature](https://github.com/omfghello/priva-browser/issues) · [Transparency Report](TRANSPARENCY.md) · [Release Notes](RELEASE_NOTES_v1.1.0.md)**

<br/>

<em>Last updated: March 2026 · Version 1.1.0</em>

</div>


