# HazardWatch — Live Disaster & Hazard Intelligence

Every earthquake. Every storm warning. One live map.

## What it does

HazardWatch is a real-time hazard map that pulls live data directly from official US government sources — just the browser talking straight to the source:

- **USGS** - every earthquake of magnitude 1.5+ worldwide from the past hour and past 24 hours
- **NWS (National Weather Service)** — every active severe weather alert in the US right now: tornado warnings, flash flood warnings, hurricane watches, red flag fire warnings, and more

Each incident appears on the map with a distinct icon for its hazard type: wave lines for floods, a cloud for weather alerts, a seismic line for earthquakes, and a flame for fires. Color-coded urgency rings (green to red) show severity at a glance. A live feed on the side lets you filter by urgency or hazard type, click into any incident for full details, and jump straight to the original government record.

## Why this matters

During any major emergency, the real data already exists — it's just scattered across a dozen different government websites, each with its own clunky interface. HazardWatch pulls it all into one live, unified view.

## How it works
Browser (index.html)
├── fetch() → USGS GeoJSON feeds (hourly + daily)
├── fetch() → NWS active alerts API
├── Normalize both into a common incident schema
├── Geocode NWS polygon alerts to centroids
├── Classify urgency (magnitude-based / NWS event-type-based)
├── Dedupe + sort by recency
└── Render: Leaflet.js map + live feed + stats panel

The two source APIs have completely different data shapes — USGS returns clean GeoJSON points, while NWS alerts come back as full geographic polygons. HazardWatch computes centroids from those polygons on the fly, and normalizes two incompatible severity systems (numeric earthquake magnitude vs. NWS's named event types like "Tornado Warning") into one consistent urgency scale.

## Stack

| Layer | Tech |
|-------|------|
| Earthquake data | USGS Earthquake Hazards Program API (free, no key) |
| Weather data | National Weather Service API (free, no key) |
| Map | Leaflet.js + OpenStreetMap tiles |
| Frontend | Vanilla HTML/CSS/JS — zero frameworks, zero build step |
| Hosting | Any static host (Netlify, Vercel, GitHub Pages) |


## Known limitations

- US-focused — NWS only covers the US; earthquake data is global
- NWS occasionally returns zero active alerts if nothing severe is happening at that exact moment — this is expected and means the data is accurate, not broken
- No historical data — shows only the live current state, not a timeline

## Future extensions

- A lightweight serverless proxy to safely reintroduce FEMA disaster declarations (currently blocked by CORS for direct browser access)
- Wildfire perimeter data from NASA FIRMS
- Saved regions with push notifications for critical alerts near a user's location

## Built with

HTML, CSS, JavaScript, Leaflet.js, USGS Earthquake Hazards Program API, National Weather Service (NWS) API, OpenStreetMap, Netlify, Claude (Anthropic)
