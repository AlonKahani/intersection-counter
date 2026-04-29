# OSM Network Analyzer

A browser-based tool for analyzing street networks from OpenStreetMap. Draw a rectangle or polygon on the map and get intersection counts, density, and four network centrality metrics — all computed client-side, no backend.

**Live:** https://alonkahani.github.io/intersection-counter/

## Features

- **Area summary**: intersection count + density (intersections / km²)
- **Network metrics** (color-coded streets, quantile-binned viridis):
  - Edge betweenness — through-traffic backbone
  - Closeness — most accessible intersections
  - Degree — local connectivity
  - Straightness — how direct routes are
- **Intersection consolidation** — merges nodes within 15m to collapse OSM dual carriageways into a single centerline (à la OSMnx)
- **Graph caching** — switching metrics on the same area is instant; only fetch + build runs once

## How it works

1. User draws a shape (≤2.5 km²)
2. Overpass API returns road geometry inside the shape
3. App builds a primal graph (intersections + dead-ends as nodes, road segments as edges, real distances as weights)
4. Optional: consolidates nearby intersections (15m clustering, OSMnx-style)
5. Selected centrality metric is computed (Brandes' algorithm for betweenness, Dijkstra for closeness/straightness)
6. Streets are colored on the map; stats panel shows counts and metric distribution

## Built with

- [Leaflet](https://leafletjs.com/) + [Leaflet.draw](https://github.com/Leaflet/Leaflet.draw)
- [Overpass API](https://overpass-api.de/) for OSM data (`z.overpass-api.de` primary, `overpass-api.de` fallback)
- [chroma-js](https://gka.github.io/chroma.js/) for color scales

## Constraints

- Max area: 2.5 km² (Brandes' algorithm is O(V·E))
- Max graph nodes: 1500
- Min 3-second interval between queries (Overpass fair use)

## Attribution

Map data © [OpenStreetMap contributors](https://www.openstreetmap.org/copyright)
