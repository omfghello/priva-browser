<p align="center">
  <strong>PRIVABROWSE</strong><br>
  <em>Transparency Report</em>
</p>

<p align="center">
  <img alt="Version" src="https://img.shields.io/badge/version-1.1.0-blue?style=flat-square" />
  <img alt="License" src="https://img.shields.io/badge/license-MIT-green?style=flat-square" />
  <img alt="Electron" src="https://img.shields.io/badge/electron-40.6.1-9feaf9?style=flat-square" />
  <img alt="Domains Blocked" src="https://img.shields.io/badge/domains%20blocked-1%2C942-red?style=flat-square" />
  <img alt="Fingerprint Vectors" src="https://img.shields.io/badge/fingerprint%20vectors-50%2B-orange?style=flat-square" />
</p>

---

> **PrivaBrowse believes you deserve to know exactly what your browser does.**
> This document discloses every protection mechanism, every blocked domain, and every line of defense — no secrets, no fine print.

---

## Table of Contents

- [Mission](#mission)
- [Threat Model](#threat-model)
- [What We Block](#what-we-block)
- [What We Spoof](#what-we-spoof)
- [What We Strip](#what-we-strip)
- [Data Poisoning Engine](#data-poisoning-engine)
- [HTTPS Enforcement](#https-enforcement)
- [Cookie Consent & Annoyance Removal](#cookie-consent--annoyance-removal)
- [Cosmetic Ad Filtering](#cosmetic-ad-filtering)
- [YouTube Ad Blocking](#youtube-ad-blocking)
- [Search Engine Policy](#search-engine-policy)
- [Password Vault](#password-vault)
- [Focus Mode](#focus-mode)
- [Data Saver Mode](#data-saver-mode)
- [What We Do NOT Do](#what-we-do-not-do)
- [How We Compare](#how-we-compare)
- [How to Verify](#how-to-verify)
- [Architecture Overview](#architecture-overview)
- [Third-Party Dependencies](#third-party-dependencies)
- [Frequently Asked Questions](#frequently-asked-questions)
- [Changelog](#changelog)

---

## Mission

PrivaBrowse exists for one reason: **your browsing data belongs to you and only you.**

We reject the surveillance-capitalist model where browsers silently funnel your habits, preferences, and identity to advertising networks. Every feature in PrivaBrowse is designed to make mass tracking technically impossible — not just discouraged, but actively poisoned.

Privacy is not a feature we bolt on. It is the architecture.

---

## Threat Model

PrivaBrowse is engineered to defend against the following threats:

| Threat | Defense |
|---|---|
| **Cross-site tracking** | Domain blocking, cookie isolation, referrer stripping, URL parameter cleaning |
| **Browser fingerprinting** | 50+ spoofed vectors randomized per session |
| **Supercookie tracking** | ETag stripping, HSTS cache limiting, Client Hints removal |
| **Data exfiltration** | Nuclear data poisoning of every tracker endpoint |
| **IP address leaks** | WebRTC policy enforcement, IP header spoofing on tracker requests |
| **Cryptojacking** | 84 miner domains blocked at the network level |
| **Social media surveillance** | 112 social tracker domains blocked unconditionally |
| **Search profiling** | Surveillance-based search engines removed entirely |
| **Cookie consent fatigue** | Automatic banner dismissal — you shouldn't have to click "reject" 50 times a day |
| **Newsletter/paywall harassment** | Popup overlays automatically removed |

### What We Do NOT Defend Against

Transparency means honesty about limits:

- **State-level adversaries** — PrivaBrowse is not Tor. Your ISP can still see which domains you connect to (use a VPN or Tor for that layer).
- **Malware/phishing** — We are not an antivirus. We block trackers, not malicious payloads.
- **Compromised endpoints** — If the website itself is hostile (e.g., a keylogger on a login page), browser-level protection has limits.
- **Physical access** — If someone has your machine, disk encryption is your defense, not a browser.

---

## What We Block

### Domain-Level Blocking

| Category | Count | Method |
|---|---|---|
| Fast-Block Hosts | **634 domains** | O(1) `Set` lookup on every request — zero performance overhead |
| Extended Blocklist (ads) | **408 domains** | Loaded from `src/blocklist.js` |
| Extended Blocklist (trackers) | **446 domains** | Loaded from `src/blocklist.js` |
| Extended Blocklist (aggressive) | **258 domains** | Loaded from `src/blocklist.js` |
| Social Tracker Hosts | **112 domains** | Facebook, Twitter, LinkedIn, Pinterest, Instagram, ShareThis, AddToAny, etc. |
| Crypto Miner Hosts | **84 domains** | Coinhive, CryptoLoot, JSEcoin, Mineralt, WebminePool, etc. |
| **Combined Total** | **1,942 domains** | All lists are deduplicated and auditable |

### Pattern-Based Blocking

- **75+ regex patterns** matching ad-serving URL structures (`/ads/`, `/adserv/`, `/pagead/`, etc.)
- **Tracking pixel detection** — 1x1 images, clear GIFs, spacer GIFs, beacon endpoints
- **Script filename blocking** — `ads.js`, `tracking.js`, `analytics.min.js`, `gtag.js`, `fbevents.js`, `pixel.js`, `tracker.min.js`
- **Third-party iframe blocking** — prevents cross-origin frames from loading tracker content (with a whitelist for ReCAPTCHA, YouTube embeds, Stripe payments, and other functional iframes)

### What Gets Blocked (Examples)

<details>
<summary><strong>Click to expand the full list of blocked services</strong></summary>

**Advertising Networks:** Google Ads, DoubleClick, Facebook Ads, Twitter Ads, LinkedIn Ads, TikTok Ads, Amazon Ad System, Taboola, Outbrain, Criteo, Bing BAT, Yahoo Ads, AdRoll, MediaMath, The Trade Desk, AppNexus

**Analytics & Tracking:** Google Analytics, Hotjar, Clarity, Mixpanel, Segment, Amplitude, Chartbeat, Parsely, Piano, Permutive, Heap Analytics, Kissmetrics, Crazy Egg

**Tag Managers & Pixels:** Facebook Pixel, Google Tag Manager, LinkedIn Insight Tag, TikTok Pixel, Pinterest Tag, Snapchat Pixel

**Session Recording:** FullStory, Mouseflow, LogRocket, Smartlook, Lucky Orange

**Marketing Automation:** HubSpot, Marketo, Pardot, Salesforce, Mailchimp tracking, ActiveCampaign

**A/B Testing:** Optimizely, VWO, Google Optimize, AB Tasty

**Attribution & Mobile:** AppFlyer, Adjust, Branch, Kochava, Singular

**Error Tracking:** New Relic, Datadog, Sentry (blocked as tracker — errors stay on your machine)

**Crypto Miners:** Coinhive, CryptoLoot, JSEcoin, Mineralt, WebminePool, CoinImp, and 78 more

</details>

### Tracker Graveyard

PrivaBrowse maintains a per-domain count of every blocked request. You can inspect the **Tracker Graveyard** from the browser's toolbar to see exactly how many trackers were stopped on each site you visit — full transparency into what's being blocked and where.

---

## What We Spoof

### Fingerprint Protection (50+ Vectors)

Every vector is randomized per-session to prevent cross-site linkage:

| Category | Vectors |
|---|---|
| **Canvas** | `toDataURL()`, `toBlob()`, `getImageData()` — subtle noise injection |
| **WebGL** | Renderer/vendor strings randomized, `getParameter()` spoofed, shader precision varied |
| **AudioContext** | `getFloatFrequencyData()` noise, `createAnalyser()` fuzz |
| **Navigator** | `hardwareConcurrency`, `deviceMemory`, `platform`, `vendor`, `plugins`, `mimeTypes` |
| **Screen/Window** | `screen.width/height`, `availWidth/Height`, `colorDepth`, `pixelDepth`, `outerWidth/Height` |
| **ClientRects** | `getBoundingClientRect()`, `getClientRects()` — sub-pixel noise |
| **Timing** | `performance.now()` precision reduced, `Date` precision reduced |
| **Fonts** | Font enumeration blocked, `measureText()` fuzzed |
| **MediaDevices** | `enumerateDevices()` returns empty or randomized list |
| **Storage** | `localStorage`/`sessionStorage` quotas fuzzed |
| **Permissions API** | Always returns `"prompt"` regardless of actual state |
| **Language/DNT/GPC** | Randomized `Accept-Language`, `navigator.language`, `Do-Not-Track: 1`, `Sec-GPC: 1` |
| **Timezone** | Randomized `Intl.DateTimeFormat` resolved options |
| **Math** | Subtle variations in `Math.tan()`, `Math.log()`, etc. |
| **WebRTC** | IP leak prevention, `RTCPeerConnection` restricted via `disable_non_proxied_udp` policy |
| **WebGPU** | Adapter info spoofed |
| **CSS matchMedia** | Randomized `prefers-color-scheme`, `prefers-reduced-motion` |

### Blocked APIs

These APIs are completely disabled to prevent fingerprinting:

<details>
<summary><strong>Full list of disabled APIs</strong></summary>

Battery Status, Gamepad, Speech Synthesis, Keyboard Layout Map, USB, Bluetooth, Serial, HID, XR (WebVR/WebXR), Sensor APIs (Accelerometer, Gyroscope, Magnetometer, AmbientLight), MIDI, Idle Detection, Device Motion/Orientation, Presentation, CSS Paint Worklet, Web Share, Payment Request, Speech Recognition, Contact Picker, Wake Lock, Installed Apps, DRM (EME) fingerprinting

</details>

### User-Agent Randomization

PrivaBrowse rotates your `User-Agent` string either per-session or per-site (configurable). The randomized UA is drawn from a pool of common browser/OS combinations so you blend into the crowd rather than standing out with an unusual string.

---

## What We Strip

### From Outgoing Requests

- **ETag supercookies** — `If-None-Match` header deleted on every request
- **Client Hints** — All 10+ `Sec-CH-*` headers stripped/spoofed (`Sec-CH-UA`, `Sec-CH-UA-Mobile`, `Sec-CH-UA-Platform`, `Sec-CH-UA-Full-Version-List`, etc.)
- **URL tracking parameters** — `utm_source`, `utm_medium`, `utm_campaign`, `utm_term`, `utm_content`, `fbclid`, `gclid`, `msclkid`, `mc_eid`, `oly_enc_id`, `oly_anon_id`, `__hssc`, `__hstc`, `__hsfp`, `_hsenc`, `vero_id`, `wickedid`, and more
- **Referrer header** — Reduced to origin-only or stripped entirely (configurable)

### From Incoming Responses

- **ETag headers** — stripped to prevent supercookie tracking
- **`Accept-CH`** and **`Critical-CH`** headers — deleted to prevent servers from requesting Client Hints
- **Tracking cookies** — cookies matching known tracker patterns are stripped

### From Links You Click

Every link on every page is cleaned in real-time. Before you navigate, PrivaBrowse strips `utm_*`, `fbclid`, `gclid`, and other tracking parameters from the destination URL so the receiving site never sees where the click came from.

---

## Data Poisoning Engine

When fingerprint protection is enabled, PrivaBrowse doesn't just block trackers — it **actively poisons** every piece of data they try to collect.

### Why Poison Instead of Just Block?

Blocking tells a tracker "this user has an ad blocker." That itself is a data point. Poisoning tells the tracker nothing useful — every field looks plausible but is completely fabricated. The tracker's dataset becomes polluted, their models degrade, and they can't distinguish real users from noise.

### JavaScript-Level Interception

Every major data exfiltration API is intercepted and fed randomized garbage:

| Interception Point | What Gets Poisoned |
|---|---|
| `fetch()` | URL parameters + request body rewritten with fake data |
| `XMLHttpRequest` | URL parameters + request body rewritten |
| `navigator.sendBeacon()` | Payload replaced with poisoned version |
| `HTMLImageElement.src` | Tracking pixel URLs get fake parameters |
| `HTMLFormElement.submit()` | Hidden form fields filled with random data |
| `navigator.geolocation` | Returns randomized coordinates worldwide |

### 40+ Poisoned Data Fields

Every tracker request gets injected with fake: email addresses, full names, phone numbers, IP addresses, user agents, device IDs, session IDs, fingerprint hashes, Google Analytics client IDs, screen resolutions, languages, timezones, referrers, geographic coordinates, and more.

### HTTP Header Poisoning (Main Process)

For every request matching a tracker domain or URL pattern, PrivaBrowse injects **40+ randomized HTTP headers**:

- **IP Spoofing**: `X-Forwarded-For`, `X-Real-IP`, `CF-Connecting-IP`, `True-Client-IP`, `X-Client-IP`, `X-Cluster-Client-IP`, `Forwarded`, `X-Original-Forwarded-For`
- **Trace ID Chaos**: `X-Amzn-Trace-Id`, `X-Cloud-Trace-Context`, `X-B3-TraceId`, `X-B3-SpanId`, `Traceparent`
- **Identity Scramble**: Randomized `User-Agent`, `Accept-Language`, `Referer`, `Origin`
- **Device Hints**: Fake `Viewport-Width`, `DPR`, `Device-Memory`, `RTT`, `Downlink`
- **Privacy Signals**: `DNT: 1`, `Sec-GPC: 1`
- **Cache Busting**: `Cache-Control: no-cache`, `Pragma: no-cache`
- **Token Stripping**: `Cookie`, `Authorization`, `X-CSRF-Token`, `X-XSRF-TOKEN`, `X-Requested-With` — all deleted

### No Exclusions

**No company is excluded from data poisoning. Not Google. Not Facebook. Not Microsoft. Not Amazon. No one.**

The only exception is YouTube's video playback — we don't break video streaming. But YouTube's tracker endpoints still get fully poisoned data, just like every other company.

---

## HTTPS Enforcement

PrivaBrowse can automatically upgrade all `http://` navigations to `https://`, preventing passive eavesdropping on unencrypted connections. This is configurable per the user's preference and applies to top-level navigations and subresource requests alike.

---

## Cookie Consent & Annoyance Removal

The web is drowning in cookie banners. PrivaBrowse automatically detects and dismisses them:

- **Cookie consent popups** — Detected via common selectors and class names, auto-dismissed with "reject all" preference
- **Newsletter signup overlays** — Email capture modals are identified and removed before they interrupt your reading
- **"Are you still there?" dialogs** — Idle-detection prompts are auto-dismissed
- **Overlay scroll locks** — When a popup locks page scrolling, PrivaBrowse restores it

This runs entirely client-side via injected scripts. No network requests, no external services.

---

## Cosmetic Ad Filtering

Beyond network-level blocking, PrivaBrowse includes a **Universal Ad Nuker** that removes ad containers from the rendered page:

- Hides elements matching known ad selectors (`[class*="ad-"]`, `[id*="google_ads"]`, `ins.adsbygoogle`, etc.)
- Removes empty placeholder divs left behind by blocked ad scripts
- Runs on `DOMContentLoaded` and observes DOM mutations for dynamically injected ads
- Purely cosmetic — no network requests are made, these elements are simply hidden from view

---

## YouTube Ad Blocking

YouTube receives special handling because its ad delivery is tightly integrated with video playback:

- **Pre-roll and mid-roll ads** — Detected and skipped at the player level
- **Ad overlay banners** — Removed from the video player
- **Tracker endpoints** — Still fully poisoned (YouTube does not get a free pass on tracking)
- **Video playback** — Preserved and unbroken

---

## Search Engine Policy

PrivaBrowse only ships privacy-respecting search engines:

| Engine | Privacy Level | Notes |
|---|---|---|
| **PrivaBrowse Search** | Highest | Our own index, zero third-party tracking |
| **DuckDuckGo** (default) | High | No tracking, no profile building |
| **Brave Search** | High | Independent index, no Big Tech dependency |
| **Startpage** | High | Anonymous proxy to search results |
| **Qwant** | High | EU-based privacy-first engine |
| **SearXNG** | High | Open-source metasearch aggregator |
| **Mojeek** | High | Own crawler, no tracking |
| **MetaGer** | High | German non-profit metasearch |
| **Swisscows** | High | Swiss privacy, family-safe |
| **Yep** | High | Independent index by Ahrefs |
| **Ecosia** | Medium | Plants trees, minimal data collection |

**Removed engines**: Google, Bing, and Yahoo were removed because their search pages load tracking scripts that conflict with our blocking and because we don't believe a privacy browser should normalize surveillance-based search.

---

## Password Vault

PrivaBrowse includes a built-in encrypted password vault so you don't need to trust a third-party extension with your credentials:

- **Encryption** — All entries are encrypted locally with a master password
- **Zero-knowledge** — Your master password never leaves your machine; we cannot recover it
- **No cloud sync** — Vault data stays on your device, period
- **No third-party dependencies** — The vault is implemented entirely within PrivaBrowse, not outsourced to an extension or external service

---

## Focus Mode

PrivaBrowse includes a **Focus Mode** that lets you temporarily block distracting websites for a set period of time:

- Define your own blocklist of distracting domains
- Set a timer — the sites are inaccessible until it expires
- No workarounds, no "just 5 more minutes" — the block is enforced at the network level

This is a productivity feature, not a privacy feature, but it reflects our belief that a browser should work *for* you.

---

## Data Saver Mode

When bandwidth matters, Data Saver Mode reduces page weight by:

- Blocking third-party fonts (also a privacy win — font loading reveals your OS and language)
- Blocking animated images
- Optionally blocking all images entirely
- Reducing unnecessary subresource requests

---

## What We Do NOT Do

To be absolutely clear:

- **No telemetry** — We never phone home. Zero analytics endpoints exist in the codebase.
- **No crash reporting** — Errors stay on your machine. No Sentry, no Bugsnag, no New Relic.
- **No usage statistics** — We don't know how many users we have, let alone what they browse.
- **No update tracking** — Update checks use standard Electron mechanisms, no identifying data sent.
- **No A/B testing** — Every user gets the same features.
- **No data sales** — We have no data to sell.
- **No "anonymous" metrics** — There is no such thing as truly anonymous analytics. We collect nothing.
- **No partnerships with ad companies** — We block them all.
- **No acceptable ads program** — All ads are treated equally: blocked.
- **No remote configuration** — There is no server-side feature flag system. What ships in the binary is what runs.
- **No third-party account requirement** — You never need to sign in to anything to use PrivaBrowse.

---

## How We Compare

| Feature | PrivaBrowse | Brave | Firefox (strict) | Chrome |
|---|:---:|:---:|:---:|:---:|
| Built-in ad blocking | Yes | Yes | Partial | No |
| Tracker domain blocking (1,900+) | Yes | Yes | Partial | No |
| Fingerprint spoofing (50+ vectors) | Yes | Partial | Partial | No |
| Data poisoning engine | **Yes** | No | No | No |
| HTTP header poisoning | **Yes** | No | No | No |
| ETag supercookie stripping | Yes | No | No | No |
| Client Hints removal | Yes | Partial | No | No |
| URL parameter cleaning | Yes | Yes | No | No |
| Cookie consent auto-dismiss | Yes | No | No | No |
| Surveillance search engines removed | **Yes** | No | No | No |
| Zero telemetry | Yes | No | No | No |
| Built-in password vault | Yes | No | Partial | Partial |
| Open source | Yes | Yes | Yes | No |

---

## How to Verify

Don't take our word for it. Verify everything yourself:

### Test Your Protection

1. Visit [Cover Your Tracks](https://coveryourtracks.eff.org/) by the EFF
2. Visit [BrowserLeaks](https://browserleaks.com/) for detailed fingerprint analysis
3. Visit [AmIUnique](https://amiunique.org/) to check your fingerprint uniqueness
4. Visit [CanvasBlocker Test](https://canvasblocker.kkapsner.de/test/) for canvas protection
5. Visit [WebRTC Leak Test](https://browserleaks.com/webrtc) to confirm no IP leaks
6. Visit [DNS Leak Test](https://dnsleaktest.com/) to check for DNS leaks

### Audit the Code

Every protection is implemented in plain JavaScript, fully readable, no obfuscation:

| Protection Layer | File | What to Search For |
|---|---|---|
| Request blocking | `main.js` | `FAST_BLOCK_HOSTS`, `AD_URL_PATTERNS`, `onBeforeRequest` |
| Domain lists | `src/blocklist.js` | `ads`, `trackers`, `aggressive` arrays |
| Fingerprint spoofing | `src/renderer/renderer.js` | `FINGERPRINT_SPOOF_SCRIPT` |
| Data poisoning | `src/renderer/renderer.js` | `DATA_POISONER_SCRIPT` |
| Header manipulation | `main.js` | `onBeforeSendHeaders`, `onHeadersReceived` |
| Cookie banner removal | `src/renderer/renderer.js` | `COOKIE_BANNER_SCRIPT` |
| Newsletter popup killer | `src/renderer/renderer.js` | `NEWSLETTER_KILLER_SCRIPT` |
| Cosmetic ad filtering | `src/renderer/renderer.js` | `injectUniversalAdNuker` |
| YouTube ad blocking | `src/renderer/renderer.js` | `injectYouTubeAdBlocker` |
| Password vault | `src/vault.js` | Entire file |
| HTTPS upgrade | `main.js` | `forceHTTPS` |

### Reproducible Builds

The `package.json` pins all dependencies. Clone the repo, run `npm install && npm run build`, and compare the output to the distributed binary. If they differ, something is wrong and you should not trust it.

---

## Architecture Overview

```
User's Request
    │
    ├─► main.js: onBeforeRequest
    │       ├─ FAST_BLOCK_HOSTS (Set, O(1) lookup)
    │       ├─ blocklist.js domains (ads / trackers / aggressive)
    │       ├─ AD_URL_PATTERNS (75+ regex patterns)
    │       ├─ Social tracker hosts (112 domains)
    │       ├─ Crypto miner hosts (84 domains)
    │       ├─ Tracking pixel detection
    │       ├─ Third-party iframe blocking (with functional whitelist)
    │       ├─ YouTube ad endpoint blocking
    │       └─ HTTPS upgrade (if enabled)
    │
    ├─► main.js: onBeforeSendHeaders
    │       ├─ ETag supercookie removal
    │       ├─ Client Hints stripping / spoofing
    │       ├─ URL parameter cleaning (utm_*, fbclid, gclid, etc.)
    │       ├─ Referrer policy enforcement
    │       ├─ User-Agent randomization
    │       └─ Nuclear header poisoning (40+ headers on tracker requests)
    │
    ├─► main.js: onHeadersReceived
    │       ├─ ETag removal
    │       ├─ Accept-CH / Critical-CH removal
    │       └─ Tracking cookie stripping
    │
    └─► renderer.js: Injected Scripts
            ├─ FINGERPRINT_SPOOF_SCRIPT (50+ vectors)
            ├─ DATA_POISONER_SCRIPT (fetch, XHR, beacon, form, pixel, geo)
            ├─ Cookie consent auto-dismissal
            ├─ Newsletter popup killer
            ├─ Idle dialog dismissal
            ├─ Cosmetic ad filtering (Universal Ad Nuker)
            ├─ YouTube ad blocker
            └─ Link URL cleaner (strips tracking params from hrefs)
```

---

## Third-Party Dependencies

PrivaBrowse is built on Electron. We acknowledge this dependency and its implications:

- **Electron 40.6.1** — Provides the Chromium rendering engine and Node.js runtime. We apply restrictive `webPreferences` (context isolation, disabled Node integration, sandboxing) to minimize Electron's attack surface.
- **No third-party analytics SDKs** — Zero. None. Not one.
- **No third-party ad SDKs** — We would sooner shut down.
- **No CDN-loaded scripts** — Everything ships in the binary. No external JavaScript is loaded at runtime by PrivaBrowse itself.
- **Chromium telemetry** — Disabled at the Electron configuration level. No Google services are contacted.

All dependencies are listed in `package.json` and are auditable via `npm audit`.

---

## Frequently Asked Questions

<details>
<summary><strong>Does PrivaBrowse break websites?</strong></summary>

Occasionally, yes. Aggressive blocking and fingerprint spoofing can interfere with sites that depend heavily on trackers for functionality (ironic, but real). PrivaBrowse provides per-site toggles so you can relax protections on specific domains without compromising your defaults.
</details>

<details>
<summary><strong>Why Electron? Isn't Chrome/Chromium part of the problem?</strong></summary>

Chromium is the rendering engine — it's how web pages display. The tracking infrastructure built on top of it (Google sync, Safe Browsing telemetry, usage metrics) is what we remove. We chose Electron because it gives us full control over the network stack via <code>webRequest</code> APIs, which is how all our blocking and poisoning works.
</details>

<details>
<summary><strong>Is my data stored anywhere?</strong></summary>

Your browsing data, history, bookmarks, and vault entries are stored locally on your machine in Electron's standard user data directory. Nothing is sent anywhere. Nothing is synced. If you delete the app, the data goes with it.
</details>

<details>
<summary><strong>Can I export/import my data?</strong></summary>

Settings and blocklists are stored in standard JSON format in your user data directory. They're yours to back up, move, or delete at any time.
</details>

<details>
<summary><strong>Why did you remove Google, Bing, and Yahoo?</strong></summary>

Their search result pages load dozens of tracking scripts that conflict with our blocking engine and degrade the experience. More fundamentally, a privacy browser should not route your most intimate data — your search queries — through the world's largest advertising companies.
</details>

<details>
<summary><strong>Does data poisoning slow down my browsing?</strong></summary>

No measurable impact. The poisoning logic intercepts requests that are already being made to tracker endpoints and rewrites their payloads. It doesn't add new network requests — it corrupts existing ones.
</details>

<details>
<summary><strong>How is this different from uBlock Origin + Firefox?</strong></summary>

uBlock Origin is excellent at blocking. PrivaBrowse blocks <em>and</em> poisons. When a tracker slips past the blocklist (and some will), it collects garbage data instead of your real information. The data poisoning engine, HTTP header poisoning, and 50+ fingerprint spoofing vectors are features no extension can replicate at the same depth because extensions don't have access to the network stack the way Electron's main process does.
</details>

---

## Changelog

### v1.1.0 — March 2026

- Expanded blocklist to 1,942 domains
- Added Nuclear Data Poisoning Engine (40+ fields, 40+ headers)
- Removed Google, Bing, Yahoo search engines
- Added SearXNG, Mojeek, MetaGer, Swisscows, Yep
- Expanded fingerprint protection to 50+ vectors
- Removed all company exclusions from data poisoning
- Added cookie consent auto-dismissal
- Added newsletter popup killer
- Added cosmetic ad filtering (Universal Ad Nuker)
- Added YouTube-specific ad blocking
- Added link URL cleaning (strips tracking params from hrefs)
- Added Tracker Graveyard (per-domain blocked request stats)
- Added Data Saver Mode
- Added Focus Mode
- Added HTTPS enforcement
- Added encrypted password vault

---

## Contact & Reporting

Found a tracker we missed? A site that breaks? A privacy concern in our code?

- **GitHub Issues**: Report bugs, broken sites, or missed trackers
- **Pull Requests**: Every contribution is reviewed for privacy implications before merge
- **Security Disclosures**: If you find a security vulnerability, please report it responsibly via GitHub's private vulnerability reporting

---

<p align="center">
  <em>This document is updated whenever PrivaBrowse's protection mechanisms change.</em><br>
  <strong>Last updated: March 5, 2026 &middot; Version 1.1.0</strong>
</p>
