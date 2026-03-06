<p align="center">
  <strong>PRIVABROWSE</strong><br>
  <em>Transparency Report</em>
</p>

<p align="center">
  <img alt="Version" src="https://img.shields.io/badge/version-1.1.0-blue?style=flat-square" />
  <img alt="License" src="https://img.shields.io/badge/license-MIT-green?style=flat-square" />
  <img alt="Electron" src="https://img.shields.io/badge/electron-40.6.1-9feaf9?style=flat-square" />
  <img alt="Domains Blocked" src="https://img.shields.io/badge/domains%20blocked-2%2C100%2B-red?style=flat-square" />
  <img alt="Fingerprint Vectors" src="https://img.shields.io/badge/fingerprint%20vectors-60%2B-orange?style=flat-square" />
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
  - [Link Click Parameter Stripping](#link-click-parameter-stripping)
  - [Local CDN Privacy](#local-cdn-privacy-decentraleyes-style)
  - [Privacy Sandbox API Blocking](#privacy-sandbox-api-blocking)
  - [Network State Partitioning](#network-state-partitioning)
  - [HSTS Supercookie Protection](#hsts-supercookie-protection)
  - [Favicon Supercookie Protection](#favicon-supercookie-protection)
  - [Bounce Tracking Detection](#bounce-tracking-detection--purging)
  - [Stale Cookie Cleanup](#stale-cookie-cleanup)
- [HTTPS Enforcement](#https-enforcement)
- [Cookie Consent & Annoyance Removal](#cookie-consent--annoyance-removal)
- [Cosmetic Ad Filtering](#cosmetic-ad-filtering)
- [YouTube Ad Blocking](#youtube-ad-blocking)
- [Search Engine Policy](#search-engine-policy)
- [Password Vault](#password-vault)
- [PrivaForge Developer Tools](#privaforge-developer-tools)
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
| **Cross-site tracking** | Domain blocking, cookie isolation, referrer stripping, URL parameter cleaning, network state partitioning |
| **Browser fingerprinting** | 60+ spoofed vectors randomized per session |
| **Supercookie tracking** | ETag stripping, HSTS cache clearing (startup + every 30 min), favicon cache clearing, Client Hints removal |
| **Data exfiltration** | Nuclear data poisoning — 55+ API fields + smart header poisoning on tracker endpoints |
| **IP address leaks** | WebRTC policy enforcement, IP header spoofing on tracker requests |
| **Privacy Sandbox surveillance** | FLEDGE, Topics, Attribution Reporting, Shared Storage, Fenced Frames all disabled via flags + Permissions-Policy |
| **Network cross-site leakage** | HTTP cache, connections, SSL sessions, DNS all partitioned per top-level site |
| **Bounce tracking** | Rapid redirect chains (< 1.5s) detected; bounce tracker cookies/storage auto-purged |
| **Stale cookie persistence** | Cookies for domains not visited in 7+ days automatically deleted |
| **CDN-based tracking** | Decentraleyes-style CDN privacy — cookies/referrer stripped from CDN requests, tracking scripts blocked |
| **Cryptojacking** | 84+ miner domains blocked at the network level |
| **Social media surveillance** | 112+ social tracker domains blocked unconditionally |
| **Search profiling** | Surveillance-based search engines removed entirely |
| **Cookie consent fatigue** | Automatic banner dismissal — you shouldn't have to click "reject" 50 times a day |
| **Newsletter/paywall harassment** | Popup overlays automatically removed |
| **Redirect tracking** | Redirect tracker bypass — t.co, l.facebook.com, google.com/url resolved directly |
| **Storage-based tracking** | 60+ tracker localStorage keys and IndexedDB databases purged every 60 seconds |
| **WebSocket/EventSource tracking** | WS payloads poisoned, SSE connections blocked for tracker hosts |
| **Anchor ping tracking** | HTML ping attributes stripped, document.referrer sanitized to origin-only |
| **Link click tracking** | 110+ tracking parameters stripped from `<a>` hrefs in real-time before clicks |

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
| Fast-Block Hosts | **740+ domains** | O(1) `Set` lookup on every request — zero performance overhead |
| Extended Blocklist (ads) | **408 domains** | Loaded from `src/blocklist.js` |
| Extended Blocklist (trackers) | **446 domains** | Loaded from `src/blocklist.js` |
| Extended Blocklist (aggressive) | **358+ domains** | Loaded from `src/blocklist.js` |
| Social Tracker Hosts | **112 domains** | Facebook, Twitter, LinkedIn, Pinterest, Instagram, ShareThis, AddToAny, etc. |
| Crypto Miner Hosts | **84 domains** | Coinhive, CryptoLoot, JSEcoin, Mineralt, WebminePool, etc. |
| **Combined Total** | **2,100+ domains** | All lists are deduplicated and auditable |

### Pattern-Based Blocking

- **100+ regex patterns** matching ad-serving URL structures (`/ads/`, `/adserv/`, `/pagead/`, etc.)
- **Tracking pixel detection** — 1x1 images, clear GIFs, spacer GIFs, beacon endpoints
- **Script filename blocking** — `ads.js`, `tracking.js`, `analytics.min.js`, `gtag.js`, `fbevents.js`, `pixel.js`, `tracker.min.js`
- **Third-party iframe blocking** — prevents cross-origin frames from loading tracker content (with a whitelist for ReCAPTCHA, YouTube embeds, Stripe payments, and other functional iframes)
- **Web Vitals / Reporting API endpoint blocking** — `web-vitals.js`, `/.well-known/attribution-reporting`
- **Fingerprint script blocking** — `fp.js`, `fingerprint*.js`

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

### Fingerprint Protection (60+ Vectors)

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
| **Navigator Extended** | `pdfViewerEnabled`, `cookieEnabled`, `javaEnabled()`, `mimeTypes`, `plugins` |
| **Performance** | `performance.memory` heap size limits spoofed |
| **CSS** | `CSS.supports()` return values randomized |
| **DOM** | `document.elementsFromPoint()`, `document.visibilityState` spoofed |
| **Network** | `navigator.connection` (effectiveType, downlink, rtt) randomized |
| **Encoding** | `TextEncoder`/`TextDecoder` behavior normalized |
| **Observers** | `IntersectionObserver` timing fuzzed |

### Blocked APIs

These APIs are completely disabled to prevent fingerprinting:

<details>
<summary><strong>Full list of disabled APIs</strong></summary>

Battery Status, Gamepad, Speech Synthesis, Keyboard Layout Map, USB, Bluetooth, Serial, HID, XR (WebVR/WebXR), Sensor APIs (Accelerometer, Gyroscope, Magnetometer, AmbientLight), MIDI, Idle Detection, Device Motion/Orientation, Presentation, CSS Paint Worklet, Web Share, Payment Request, Speech Recognition, Contact Picker, Wake Lock, Installed Apps, DRM (EME) fingerprinting, Navigator.pdfViewerEnabled, Navigator.cookieEnabled, Notification.maxActions, Performance.memory, Screen.orientation

</details>

### User-Agent Randomization

PrivaBrowse rotates your `User-Agent` string either per-session or per-site (configurable). The randomized UA is drawn from a pool of common browser/OS combinations so you blend into the crowd rather than standing out with an unusual string.

---

## What We Strip

### From Outgoing Requests

- **ETag supercookies** — `If-None-Match` header deleted on every request
- **Client Hints** — All 10+ `Sec-CH-*` headers stripped/spoofed (`Sec-CH-UA`, `Sec-CH-UA-Mobile`, `Sec-CH-UA-Platform`, `Sec-CH-UA-Full-Version-List`, etc.)
- **URL tracking parameters** — **110+ tracking parameters** including `utm_source`, `utm_medium`, `utm_campaign`, `utm_term`, `utm_content`, `fbclid`, `gclid`, `msclkid`, `mc_eid`, `oly_enc_id`, `oly_anon_id`, `__hssc`, `__hstc`, `__hsfp`, `_hsenc`, `vero_id`, `wickedid`, TikTok (`_ttp`, `tt_*`), Twitter (`twclid`), Reddit (`rdt_cid`), LinkedIn (`li_fat_id`), Amazon (`tag`, `ascsubtag`), Pinterest (`epik`), and more
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

### 55+ Poisoned Data Fields

Every tracker request gets injected with fake: email addresses, full names, phone numbers, IP addresses, user agents, device IDs, session IDs, fingerprint hashes, Google Analytics client IDs, screen resolutions, languages, timezones, referrers, geographic coordinates, browser/OS version, GPU vendor/renderer, heap size, connection type, page URL/title, font list hash, canvas/WebGL hashes, and more.

### Smart HTTP Header Poisoning (Main Process)

PrivaBrowse uses a **smart header poisoning** strategy: instead of injecting hundreds of new headers (which degraded performance and broke websites), it only **randomizes headers the tracker request already carries**. This approach is stealthier, faster, and avoids detection.

For every request matching a tracker domain or URL pattern:

- **User-Agent randomization**: Rotated from a pool of 5 common browser/OS combinations
- **Accept-Language spoofing**: Randomized from 6 common locale combinations
- **Referer/Origin spoofing**: Replaced with common referrers (Google, DuckDuckGo, Reddit) or stripped
- **IP header spoofing**: X-Forwarded-For, X-Real-IP, CF-Connecting-IP, True-Client-IP, and 6 more — only randomized if the request already carries them
- **Correlation/trace ID poisoning**: X-Request-ID, X-Correlation-ID, X-Trace-ID, X-Amzn-Trace-Id, X-B3-TraceId, Traceparent, Sentry-Trace, Datadog — fake values replace existing ones
- **Device/session ID poisoning**: X-Device-ID, X-Session-ID, X-Visitor-ID, X-Device-Fingerprint, X-GA-Client-ID, X-Analytics-ID, X-Tracking-ID, Newrelic, Baggage — poisoned if present
- **Sec-Fetch stripping**: All Sec-Fetch-Dest/Mode/Site/User headers deleted (leaks cross-site behavior)
- **Token stripping**: Cookie, Authorization, X-CSRF-Token, X-XSRF-TOKEN, X-Requested-With, Proxy-Authorization all deleted from tracker requests
- **Cache-busting**: Cache-Control set to `no-cache, no-store`; If-None-Match randomized to prevent ETag fingerprinting

**Why smart instead of bulk?** The previous approach of injecting 200+ headers on every tracker request caused measurable performance degradation and broke websites like YouTube. The new approach poisons only what exists, making PrivaBrowse undetectable to tracker servers that monitor for unusual header counts.

### No Exclusions

**No company is excluded from data poisoning. Not Google. Not Facebook. Not Microsoft. Not Amazon. No one.**

The only exception is YouTube's video playback — we don't break video streaming. But YouTube's tracker endpoints still get fully poisoned data, just like every other company.

### WebSocket Interception

WebSocket connections to tracker hosts have their `send()` method intercepted. JSON payloads are parsed and every tracking field is replaced with fake data. The connection itself is preserved (to avoid detection) but the data is garbage.

### EventSource Blocking

Server-Sent Events (EventSource) connections to tracker hosts are replaced with a fake stub that does nothing. The page thinks it connected; the tracker receives nothing.

### Redirect Tracker Bypass

Links through bounce trackers (t.co, l.facebook.com, google.com/url, click.redditmail.com, safelinks.protection.outlook.com, lnks.gd, youtube.com/redirect, href.li, steamcommunity.com/linkfilter) are intercepted and resolved directly to the real destination URL.

### Periodic Storage Cleanup

Every 60 seconds, PrivaBrowse purges 60+ known tracker localStorage keys (_ga, _fbp, amplitude_id, intercom, drift, hubspot, etc.) and deletes tracking IndexedDB databases (Firebase, Sentry, Amplitude).

### Anchor Ping Stripping & Referrer Sanitization

HTML `<a ping=...>` attributes are stripped from all links via MutationObserver. `document.referrer` is overridden to return origin-only. `window.name` is cleared on every navigation to prevent cross-site data leaking. The Reporting API (ReportingObserver) is neutralized.

### Link Click Parameter Stripping

Beyond stripping tracking parameters on navigation, PrivaBrowse also cleans links **in real-time within the page**:

- Hooks `click` and `auxclick` events on every `<a>` tag
- Uses a `MutationObserver` to clean dynamically added links as they appear
- Strips 110+ tracking parameters (`utm_*`, `fbclid`, `gclid`, `msclkid`, TikTok, Twitter, Reddit, LinkedIn, Amazon, Pinterest, and more) from `href` attributes before the browser navigates
- Works on both left-click and middle-click (new tab) navigation

### Local CDN Privacy (Decentraleyes-Style)

Requests to CDN hosts (cdnjs.cloudflare.com, cdn.jsdelivr.net, unpkg.com, etc.) receive special handling:

- **Cookie stripping**: Cookies are never sent to CDN hosts
- **Referrer stripping**: Referer header is removed to prevent CDNs from knowing which site loaded their resources
- **Tracking script blocking**: Known tracking scripts disguised as CDN resources are detected and blocked via pattern matching

This is defined in `src/cdn-replacements.js` and applied in `main.js`.

### Privacy Sandbox API Blocking

Google's Privacy Sandbox APIs (marketed as "privacy-preserving" but still enabling interest-based advertising) are comprehensively disabled:

| API | How Blocked |
|---|---|
| **FLEDGE / Protected Audience** | `disable-features` Chromium flag + Permissions-Policy |
| **Topics API / Browsing Topics** | `disable-features` Chromium flag + Permissions-Policy |
| **Attribution Reporting** | `disable-features` Chromium flag + Permissions-Policy |
| **Fenced Frames** | `disable-features` Chromium flag + Permissions-Policy |
| **Shared Storage** | `disable-features` Chromium flag + Permissions-Policy |
| **Private Aggregation** | Permissions-Policy header |
| **Interest Group Storage** | `disable-features` Chromium flag |
| **Conversion Measurement** | `disable-features` Chromium flag |

These are blocked via both `app.commandLine.appendSwitch('disable-features', ...)` in the main process and by injecting a `Permissions-Policy` response header on every page load.

### Network State Partitioning

To prevent cross-site tracking via shared network state, PrivaBrowse enables Chromium's isolation features:

| State | Chromium Flag |
|---|---|
| HTTP cache | `SplitCacheByNetworkIsolationKey` |
| TCP/QUIC connections | `PartitionConnectionsByNetworkIsolationKey` |
| SSL sessions | `PartitionSSLSessionsByNetworkIsolationKey` |
| DNS cache | `PartitionDomainReliabilityByNetworkIsolationKey` |
| Network Error Logging | `PartitionNelAndReporting` |
| Third-party storage | `ThirdPartyStoragePartitioning` |
| Expect-CT state | `PartitionExpectCTStateByNetworkIsolationKey` |

This means Site A cannot detect whether you've visited Site B by probing shared caches, connection pools, or DNS state.

### HSTS Supercookie Protection

HSTS (HTTP Strict Transport Security) can be abused to store a unique identifier across browser sessions by selectively setting HSTS on a matrix of subdomains. PrivaBrowse defends against this by:

- Clearing the HSTS resolver cache on startup
- Clearing it again every 30 minutes while running
- Using `session.defaultSession.clearHostResolverCache()`

### Favicon Supercookie Protection

Similar to HSTS supercookies, favicon caching can be exploited to track users across sessions. PrivaBrowse clears the favicon cache periodically alongside HSTS cache clearing using `session.defaultSession.clearCache()`.

### Bounce Tracking Detection & Purging

"Bounce tracking" is a technique where trackers redirect you through their domain for just long enough to set a cookie, then bounce you to your actual destination. PrivaBrowse detects this:

- Tracks navigation timestamps per webContents
- If a domain redirects within 1.5 seconds without user interaction, it's flagged as a bounce tracker
- All cookies and storage for flagged domains are immediately purged
- The system is integrated into the `onBeforeRequest` handler for main frame navigations

### Stale Cookie Cleanup

Even without bounce tracking, cookies can accumulate from domains you visited once and never returned to. PrivaBrowse tracks domain visit timestamps and periodically:

- Scans all cookies in the session
- Compares each cookie's domain against the last-visited timestamp
- Deletes cookies from domains not visited within a configurable threshold (default: 7 days)
- Runs on startup and can be triggered manually

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

## PrivaForge Developer Tools

PrivaBrowse ships with **PrivaForge** — a completely custom developer tools panel built from scratch. This is not Chrome DevTools. It's a cyberpunk-themed toolkit designed to be fun and practical.

### Why Custom Dev Tools?

Chrome DevTools are powerful but generic. PrivaForge is purpose-built for PrivaBrowse users who want to inspect, debug, and audit websites with a tool that matches the browser's identity. It also includes a **Privacy Scan** tool that no standard dev tools provide.

### 6 Tool Panels

| Panel | What It Does |
|---|---|
| **Terminal** | Hacker-style JavaScript console with a `▶` prompt. Execute any JS in the page context. Built-in commands include `dom.count`, `dom.links`, `perf.timing`, `perf.memory`, `privacy.scan`, `storage.local`, `fun.rainbow`, `fun.flip`, `fun.party`. Full arrow-key command history and tab-autocomplete |
| **X-Ray** | DOM inspector that scans the page tree structure. Collapsible node tree with color-coded tags, IDs, and classes. **Pick Element** mode with neon green highlight overlay — click any element to see box model, computed styles, and attributes |
| **Pulse** | Real-time network monitor via PerformanceObserver. Shows resource name, type, size, timing, and a visual waterfall bar. Filter by type (JS, CSS, Img, Fetch, Other). Record/pause toggle |
| **Vault** | Storage explorer for localStorage, sessionStorage, and cookies. Search/filter keys, click to inspect values, delete individual entries or clear all |
| **Paint** | Live CSS editor with line numbers and instant injection. Quick Inject snippets: Box Outlines, Grayscale Images, Comic Sans, Dark Mode, Rainbow Mode, Hover Zoom. Pull computed styles from hovered elements |
| **Scan** | Full site audit scoring **Performance**, **Accessibility**, **Privacy**, and **SEO** (0–100 each). Checks load time, DOM node count, lazy loading, alt text, HTTPS, trackers, meta tags, structured data, and more |

### Privacy Implications

PrivaForge runs entirely locally. It does not send any data anywhere. The Terminal executes JavaScript within the page's webview context using `executeJavaScript()`. No external services, no telemetry, no analytics.

### Access

- **F12** or **Ctrl+Shift+I** — Toggle PrivaForge
- **Right-click context menu** — "PrivaForge" option
- **Command palette** (`Ctrl+Shift+P`) — "PrivaForge" command

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
| Tracker domain blocking (2,100+) | Yes | Yes | Partial | No |
| Fingerprint spoofing (60+ vectors) | Yes | Partial | Partial | No |
| Data poisoning engine | **Yes** | No | No | No |
| Smart header poisoning | **Yes** | No | No | No |
| ETag supercookie stripping | Yes | No | No | No |
| Client Hints removal | Yes | Partial | No | No |
| URL parameter cleaning (navigation + link clicks) | **Yes** | Partial | No | No |
| Privacy Sandbox API blocking | **Yes** | Partial | No | No |
| Network state partitioning | **Yes** | No | Partial | Partial |
| HSTS supercookie protection | **Yes** | No | Partial | No |
| Favicon supercookie protection | **Yes** | No | No | No |
| Bounce tracking detection | **Yes** | Partial | Partial | No |
| Stale cookie auto-cleanup | **Yes** | No | No | No |
| Local CDN privacy | **Yes** | No | No | No |
| Cookie consent auto-dismiss | Yes | No | No | No |
| Surveillance search engines removed | **Yes** | No | No | No |
| Zero telemetry | Yes | No | No | No |
| Built-in password vault | Yes | No | Partial | Partial |
| Open source | Yes | Yes | Yes | No |
| WebSocket/EventSource interception | **Yes** | No | No | No |
| Redirect tracker bypass | **Yes** | No | No | No |
| Storage cleanup (60+ keys) | **Yes** | No | No | No |

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
| CDN tracking lists | `src/cdn-replacements.js` | `CDN_TRACKING_HOSTS`, `isCdnTrackingScript` |
| Fingerprint spoofing | `src/renderer/renderer.js` | `FINGERPRINT_SPOOF_SCRIPT` |
| Data poisoning | `src/renderer/renderer.js` | `DATA_POISONER_SCRIPT` |
| Smart header poisoning | `main.js` | `onBeforeSendHeaders`, `TRACKER_RE`, `isTracker` |
| Link click param stripping | `src/renderer/renderer.js` | `LINK_CLICK_PARAM_STRIP_SCRIPT` |
| Privacy Sandbox blocking | `main.js` | `disable-features`, `Permissions-Policy` in `onHeadersReceived` |
| Network partitioning | `main.js` | `enable-features` Chromium flags |
| HSTS/favicon supercookie protection | `main.js` | `startSupercookieProtection`, `clearSupercookies` |
| Bounce tracking detection | `main.js` | `recordNavigation`, `checkBounceTracker` |
| Stale cookie cleanup | `main.js` | `trackDomainVisit`, `runStaleCookieCleanup` |
| Cookie banner removal | `src/renderer/renderer.js` | `COOKIE_BANNER_SCRIPT` |
| Newsletter popup killer | `src/renderer/renderer.js` | `NEWSLETTER_KILLER_SCRIPT` |
| Cosmetic ad filtering | `src/renderer/renderer.js` | `injectUniversalAdNuker` |
| YouTube ad blocking | `src/renderer/renderer.js` | `injectYouTubeAdBlocker` |
| Password vault | `src/vault.js` | Entire file |
| PrivaForge dev tools | `src/renderer/renderer.js` | `privaforge`, `togglePrivaForge`, `pfTerminal`, `pfXRay`, `pfPulse` |
| Clipboard shortcuts fix | `main.js` | `Menu.buildFromTemplate`, Edit role items |
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
    │       ├─ AD_URL_PATTERNS (100+ regex patterns)
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
    │       ├─ Smart header poisoning (randomizes existing headers only)
    │       ├─ CDN cookie/referrer stripping
    │       ├─ Bounce tracking detection
    │       └─ Redirect tracker bypass (9 bounce domains)
    │
    ├─► main.js: onHeadersReceived
    │       ├─ ETag removal
    │       ├─ Accept-CH / Critical-CH removal
    │       ├─ Privacy Sandbox Permissions-Policy injection
    │       └─ Tracking cookie stripping
    │
    ├─► main.js: Background Tasks
    │       ├─ HSTS supercookie cache clearing (startup + every 30 min)
    │       ├─ Favicon cache clearing (periodic)
    │       ├─ Stale cookie cleanup (domains not visited in 7+ days)
    │       └─ Network state partitioning (via Chromium enable-features flags)
    │
    └─► renderer.js: Injected Scripts
            ├─ FINGERPRINT_SPOOF_SCRIPT (60+ vectors)
            ├─ DATA_POISONER_SCRIPT (fetch, XHR, beacon, form, pixel, geo)
            ├─ STORAGE_CLEANUP_SCRIPT (60+ tracker keys purged every 60s)
            ├─ MISC_PRIVACY_SCRIPT (anchor pings, referrer, sendBeacon, ReportingObserver)
            ├─ Cookie consent auto-dismissal
            ├─ Newsletter popup killer
            ├─ Idle dialog dismissal
            ├─ Cosmetic ad filtering (Universal Ad Nuker)
            ├─ YouTube ad blocker
            ├─ Link URL cleaner (strips tracking params from hrefs)
            ├─ Link click param stripping (MutationObserver + click/auxclick hooks)
            └─ PrivaForge dev tools (Terminal, X-Ray, Pulse, Vault, Paint, Scan)
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

No measurable impact. The poisoning logic intercepts requests that are already being made to tracker endpoints and rewrites their payloads. It doesn't add new network requests — it corrupts existing ones. Earlier versions injected 200+ headers on every tracker request, which caused performance degradation; this was refactored to a "smart" approach that only randomizes headers the request already carries.
</details>

<details>
<summary><strong>How is this different from uBlock Origin + Firefox?</strong></summary>

uBlock Origin is excellent at blocking. PrivaBrowse blocks <em>and</em> poisons. When a tracker slips past the blocklist (and some will), it collects garbage data instead of your real information. The data poisoning engine, smart header poisoning, 60+ fingerprint spoofing vectors, network state partitioning, Privacy Sandbox blocking, bounce tracking detection, and supercookie protection are features no extension can replicate at the same depth because extensions don't have access to the network stack the way Electron's main process does.
</details>

---

## Changelog

### v1.1.0 — March 2026

**Privacy & Anti-Tracking:**
- Expanded blocklist to 2,100+ domains
- Added Nuclear Data Poisoning Engine (55+ fields on tracker API requests)
- Refactored header poisoning from 200+ bulk-injected headers to smart approach (randomizes existing headers only) — fixes performance issues and website breakage
- Added Privacy Sandbox API blocking (FLEDGE, Topics, Attribution Reporting, Shared Storage, Fenced Frames) via Chromium flags + Permissions-Policy
- Added network state partitioning (HTTP cache, connections, SSL sessions, DNS isolated per site)
- Added HSTS supercookie protection (cache cleared on startup + every 30 minutes)
- Added favicon supercookie protection (periodic cache clearing)
- Added bounce tracking detection (< 1.5s redirects detected, cookies/storage purged)
- Added stale cookie cleanup (domains not visited in 7+ days have cookies deleted)
- Added local CDN privacy (Decentraleyes-style cookie/referrer stripping + tracking script blocking)
- Added link click parameter stripping (MutationObserver + click hooks clean 110+ params from `<a>` hrefs in real-time)
- Expanded URL parameter stripping to 110+ params (navigation + in-page links)
- Removed all company exclusions from data poisoning
- Expanded fingerprint protection to 60+ vectors
- Added WebSocket payload poisoning for tracker hosts
- Added EventSource (SSE) blocking for tracker hosts
- Added redirect tracker bypass (t.co, l.facebook.com, google.com/url, etc.)
- Added periodic storage cleanup (60+ tracker localStorage keys every 60s)
- Added anchor ping stripping and document.referrer sanitization

**Search & Browsing:**
- Removed Google, Bing, Yahoo search engines
- Added SearXNG, Mojeek, MetaGer, Swisscows, Yep

**UI & Features:**
- Added PrivaForge — custom cyberpunk developer tools with 6 panels (Terminal, X-Ray, Pulse, Vault, Paint, Scan)
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

**Security:**
- Added process sandboxing and 30+ Chromium security flags
- Added strict Content Security Policy
- Added IPC input validation and rate limiting
- Added navigation guards and webview hardening

**Bug Fixes:**
- Fixed Ctrl+C/V/X/A not working (replaced null application menu with proper hidden Edit menu)
- Fixed yellow annotation bar appearing on text selection (disabled auto-injection of PAGE_ANNOTATE_SCRIPT)
- Fixed YouTube breakage caused by overly broad tracker detection regex
- Added safe-host bypass for data poisoner to prevent first-party site interference

**Performance:**
- Optimized blocklist lookups: O(1) Set for hostnames, single compiled RegExp for URL patterns
- Consolidated URL parsing in network handlers to reduce redundant `new URL()` calls
- Tightened tracker detection regex to use word boundaries and exact domain matching

---

## Contact & Reporting

Found a tracker we missed? A site that breaks? A privacy concern in our code?

- **GitHub Issues**: Report bugs, broken sites, or missed trackers
- **Pull Requests**: Every contribution is reviewed for privacy implications before merge
- **Security Disclosures**: If you find a security vulnerability, please report it responsibly via GitHub's private vulnerability reporting

---

<p align="center">
  <em>This document is updated whenever PrivaBrowse's protection mechanisms change.</em><br>
  <strong>Last updated: March 6, 2026 &middot; Version 1.1.0</strong>
</p>
