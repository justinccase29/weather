# Bike Weather 🚴

A single-file, personal weather app tuned for bike commuting. Everything lives in
[index.html](index.html) — no build step, no API keys, no account.

## Data sources (all free / open)

| What | Source | Notes |
|---|---|---|
| Forecast, nowcast, history | [Open-Meteo](https://open-meteo.com) | Open data, no key, non-commercial use |
| Radar HD + forecast (Canada) | [MSC GeoMet](https://eccc-msc.github.io/open-data/msc-geomet/readme_en/) WMS | 1 km radar (−3 h) + HRDPS model precip (+12 h), full zoom |
| Global radar | [RainViewer](https://rainviewer.com) | Free tiles (native zoom 7, upscaled beyond; −2 h + 30 min nowcast) |
| Animated wind map | [Windy](https://windy.com) embed | Has its own ± hours timeline |
| Reverse geocoding | BigDataCloud client API | Only used to name your GPS position |

## Features

The app is split into four tabs (bottom bar): **Now · Forecast · Map · History**.

- **Location**: opens on Montréal by default (first run), then upgrades to GPS in the background if available. GPS button, city search, save any number of locations (star button → chips under the header). Last location is remembered.
- **Everything is remembered** between sessions (localStorage): saved locations, last location, active tab, hourly/daily choice, radar/wind mode, and history range. Note: clearing the browser's site data resets it, and storage is per-origin — if you move the file to a different host, re-save your locations once.
- **Now card**: temperature, feels-like (labels humidex vs wind chill), wind + gusts + direction arrow, humidity, current precipitation.
- **Next 3 hours**: MinuteCast-style 15-min precipitation bars with a plain-language summary ("starting in ~30 min…").
- **Forecast**: 48 h hourly (with feels-like, precip % + mm, wind/gusts) ⇄ 14-day daily toggle. The colored dot on each hour is a bike-comfort indicator (green/yellow/red from rain, gusts ≥ 32/45 km/h, temp ≤ −8° or ≥ 32°).
- **Map**: three modes — **Radar HD** (Environment Canada 1 km radar, continuous timeline from −3 h observed into +12 h model forecast; frames past the last scan are labeled "forecast"), **Global** (RainViewer, −2 h → +30 min, works worldwide), and **Wind** (Windy embed with its own multi-hour scrubber). Slider + play button.
- **Past weather**: meteogram (temp / precip / wind) over 24 h, 48 h, 7 d or 31 d, plus rain accumulation cards for today, yesterday, last 7 and last 30 days.

## Using it on Android

The app is just a web page, but it needs to be *served* (not opened as a bare
`file://`) for GPS to work in Chrome. Easiest options:

1. **Host it** (best): put `index.html` on any static host (GitHub Pages,
   Cloudflare Pages, your own server). Open it in Chrome → menu → **Add to
   Home screen**. It then behaves like an app, GPS included.
2. **Local server app**: install a tiny HTTP server app on the phone and point
   it at the file.
3. **No GPS needed?** Just open the file directly and use search + saved
   locations — everything else works fine from `file://`.

Internet access is required at runtime (live weather APIs).

## Tweaking

Everything is in `index.html`:

- Bike-comfort thresholds: `bikeClass()` function.
- Hourly window: `forecast_hours=48` in the API URL (up to 384).
- Daily window: `forecast_days=14` (up to 16).
- History depth: `past_days=31` (up to 92).
