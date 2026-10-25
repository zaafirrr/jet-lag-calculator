<div align="center">

<br />

# Flight Time & Jet Lag Calculator

**Pick two cities and watch the flight. See where it is night along the route, what the daylight is like when you land, and a plan to beat jet lag.**
<br />
*Disponible en français — Available in English and French.*

<br />

[![Live Demo](https://img.shields.io/badge/Live_Demo-Try_it_now-6366f1?style=for-the-badge&logo=googlechrome&logoColor=white)](https://zaafirrr.github.io/jet-lag-calculator/)
&nbsp;
[![Made with](https://img.shields.io/badge/Vanilla_HTML%2FCSS%2FJS-No_dependencies-10b981?style=for-the-badge)](https://github.com/zaafirrr/jet-lag-calculator)

<br />

</div>

---

## Why I built this

I love travelling, and I've often found myself wondering how long it actually takes to get somewhere, and how rough the jet lag will be once I land. So I thought, why not build something that answers both in one place.

---

## What it does

| | |
|---|---|
| ✈️ **Animated flight** | A plane flies the great-circle route on the map with play, pause and a scrubber. Live clocks show the local time at both ends as you fly |
| 🌗 **Live day and night** | The globe is shaded using the real position of the sun, so you can see the plane chase the sunset or cross into the dark |
| ☀️ **Daylight at your destination** | Sunrise, sunset and day length for the day you land, whether you arrive in daylight, and when to seek or avoid bright light to adjust faster |
| 🧳 **Multi-city trips** | Add up to five stops with a stay length for each. See a trip summary and switch between legs; every leg gets its own flight, daylight and sleep plan |
| 🌱 **Carbon estimate** | Per-passenger CO₂e for each flight and the whole trip, by cabin class, using published UK Government factors |
| 🌦️ **Weather at both ends** | The next 24 hours of forecast at the departure city (from take-off) and the destination (from landing), with temperature, conditions, rain chance and wind. Live data from Open-Meteo, available for about the next 16 days |
| 🕘 **Meeting time finder** | Gives the best time to talk between the departure and arrival cities for any day, shows both cities' working and waking hours on one timeline, and lets you drag across it to convert any time between the two |
| ⏰ **Clock-change alerts** | Warns when daylight saving starts or ends in a city while you are there, shortly before you leave, or just after you land |
| 🎫 **Boarding-pass summary** | Flight time, distance, local departure and arrival, and whether you land the same day |
| 😴 **Jet lag outlook** | Severity, time shift and estimated days to adjust |
| 📅 **Sleep plan** | A timeline of bedtimes for the 3 days before and after you fly, based on your usual bedtime |
| 🌍 **Interactive 3D globe** | Drag to rotate, zoom, and tap any city to fly there. Follows the plane on long routes |
| 🧩 **Widgets** | Jet lag, sleep plan, time gap, body clock, tips, daylight, weather, meeting finder, carbon and distance sit in side panels, in full-width and half-width cards that end level with the globe. Each has an expand button that opens the full detail with a smooth animation |
| 🕰️ **Body clock** | What your body will think the time is when you land, and what your usual bedtime becomes at the destination |
| 🇫🇷 **English & French** | Switch at any time. City names, dates and times follow the language |
| 🌙 **Dark and light themes** | Remembers your choice |

179 cities across 88 countries, grouped by country with national flags. Search by city, airport code or country, filter by region, or use the popular list.

---

## How it works

**Flight time** uses the haversine distance divided by an average speed that depends on route length, plus an allowance for taxi, climb and descent. It's an estimate, so real schedules will differ.

**Time zones** use the browser's `Intl` API, so daylight saving is handled correctly with no libraries.

**Sun and night shading** compute the sun's declination and longitude for any instant, then shade each point of the map by the sun's altitude on a canvas. Sunrise and sunset come from scanning the sun's altitude through the destination's local day.

**The globe** is an orthographic projection drawn on a canvas with [d3-geo](https://github.com/d3/d3-geo), using Natural Earth land data (via [world-atlas](https://github.com/topojson/world-atlas) TopoJSON) embedded in the file. The night side is a set of circles around the point opposite the sun. Flags are embedded SVGs from [circle-flags](https://github.com/HatScripts/circle-flags). The route is a great-circle arc computed in the browser, so no map tiles are needed.

**Weather** comes from the free [Open-Meteo](https://open-meteo.com/) forecast API, requested from the browser for each city and shown in that city's local time. Conditions use the standard WMO weather codes. If the forecast can't be reached, or the date is more than about 16 days away, the panel says so instead of showing anything made up.

**Meeting finder** assumes working hours you can change (09:00–17:00 by default) and waking hours of 07:00 to 22:00. It converts each hour with the browser's `Intl` time zone data, so daylight saving is respected.

**Clock changes** are found by checking each city's UTC offset through the browser's `Intl` API across the days you are there, so the dates and times come from the time zone database rather than a hand-written list.

**Carbon estimate** multiplies the great-circle distance by the UK Government (DESNZ) 2025 greenhouse gas conversion factors, in kg CO₂e per passenger-km. These factors already include a distance uplift for indirect routing, the extra warming from non-CO₂ effects at altitude (radiative forcing) and fuel production:

| Flight type | Economy | Premium economy | Business | First |
|---|---|---|---|---|
| UK domestic | 0.26278 (average, all cabins) | | | |
| Short-haul (up to 3,700 km) | 0.14825 | 0.14825 | 0.22236 | 0.22236 |
| Long-haul | 0.14165 | 0.22663 | 0.41077 | 0.56658 |

It is an estimate: real emissions depend on the aircraft, how full it is and the route flown. Check the factors against the official [UK Government GHG conversion factors](https://www.gov.uk/government/publications/greenhouse-gas-reporting-conversion-factors-2025) before relying on them, and update the `CARBON` object in `index.html` when a new year is published.

**Jet lag plan** follows the common guidance of shifting sleep about an hour a day before travel (earlier flying east, later flying west), then keeping your normal bedtime on local time after you land.

---

## Running locally

Open `index.html` in a browser. No install needed.

## Deploying to GitHub Pages

1. Push this repo to GitHub
2. **Settings → Pages**
3. Deploy from a branch → main / root
4. Live at `https://<your-username>.github.io/jet-lag-calculator/`

## Adding more cities

Add an entry to the `RAW` array in `index.html` (a new country also needs an entry in `COUNTRIES` and `FLAGS`):

```js
['Lisbon', 'Lisbonne', 'LIS', 'Europe/Lisbon', 38.7223, -9.1393, 'PT', 'eu'],
// [English name, French name, airport code, IANA time zone, lat, lng, country code, region]
```

Region codes: `eu`, `me`, `as`, `af`, `am`, `oc`.

---

## Structure

```
jet-lag-calculator/
└── index.html   — markup, styles, bundled d3-geo, land data and logic in one self-contained file
```

Vanilla HTML, CSS and JavaScript. No frameworks, no build step, no dependencies.
