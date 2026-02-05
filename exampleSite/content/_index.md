---
title: Hugo Maps Examples
address: "Space Needle, Seattle"
addressZoom: 13
---

# Hugo Maps Examples

Welcome to **hugo-maps**, a Hugo module that makes it easy to add beautiful, interactive maps to your website!

## What is hugo-maps?

hugo-maps is a modern Hugo module powered by [MapLibre GL](https://maplibre.org/) that allows you to embed interactive vector maps with minimal configuration.

### Key Features

- 🗺️ Beautiful vector maps with multiple built-in styles
- 🎨 Responsive design that works on all devices
- 📍 Address geocoding with markers and popups
- 🎬 3D maps with pitch and bearing controls
- ⚡ Fast, lightweight, and privacy-focused
- 🔧 Works with Hugo 0.155+

---

## Map Examples

### Example 1: Simple Map (NYC)

{{< map name="simple_map" >}}

A basic map using the OSM Bright theme, centered on New York City.

---

### Example 2: 3D Map (London)

{{< map name="map_3d" >}}

A 3D map with terrain visualization. Drag to pan, scroll to zoom, and hold Shift + drag to rotate!

---

### Example 3: Dark Theme (Paris)

{{< map name="dark_map" >}}

The same style, but with a dark theme for better visibility in low-light conditions.

---

### Example 4: Address Geocoding (Space Needle)

{{< map name="address_map" >}}

This map uses the `address` field from the page frontmatter ("Space Needle, Seattle"). The geocoding is performed at **build time**, so no client-side API calls are needed!

---

## Getting Started

### 1. Add to Your Hugo Project

Add hugo-maps as a module in your `config.yaml`:

```yaml
module:
  imports:
    - path: "github.com/kylehakala/hugo-maps"
```

### 2. Configure Your Maps

Define map configurations in your site config:

```yaml
params:
  hugo-maps:
    default:
      height: 500px
      navigationControl: true
    
    my_map:
      style: osm-bright
      center: [-74.006, 40.7128]
      zoom: 13
```

### 3. Embed in Content

Use the map shortcode in your Markdown:

```markdown
{{< map name="my_map" >}}
```

---

## Available Map Styles

hugo-maps includes several beautiful built-in map styles:

| Style | Description |
|-------|-------------|
| **osm-bright** | Classic, clean OpenStreetMap style |
| **positron** | Light, minimal style perfect for dashboards |
| **darkmatter** | Dark theme for night mode viewing |
| **toner** | High-contrast black and white style |
| **fjord-color** | Nordic-inspired colorful style |
| **maptiler-3d** | 3D terrain and building visualization |

---

## Configuration Options

Maps support extensive customization. See the full [README](https://github.com/kylehakala/hugo-maps) for all options.

### Basic Map Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `style` | string | osm-bright | Map theme/style |
| `center` | array | [0, 0] | [longitude, latitude] coordinates |
| `zoom` | number | 12 | Initial zoom level (0-23) |
| `pitch` | number | 0 | Camera pitch in degrees (0-60) |
| `bearing` | number | 0 | Camera bearing in degrees (0-360) |
| `height` | string | 300px | Map container height |

### Geocoding Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `geocodeParams` | string/array | "address" | Frontmatter field(s) to geocode |
| `geocoders` | string | "photon,nominatim" | Geocoder provider(s) |
| `flyToDuration` | number | 2500 | Fly-to animation duration (ms) |
| `addressZoom` | number | zoom | Zoom level after geocoding |

### Geocoding from Frontmatter

Add an address to your page frontmatter:

```yaml
---
title: "My Location"
address: "Empire State Building, New York"
addressZoom: 15
---
```

The map will automatically geocode the address and display a marker with the page title.

---

## Learn More

- **GitHub**: [kylehakala/hugo-maps](https://github.com/kylehakala/hugo-maps)
- **MapLibre GL**: [maplibre.org](https://maplibre.org/)
- **Hugo**: [gohugo.io](https://gohugo.io/)

---

## License

hugo-maps is open source and available under the MIT License.

Happy mapping! 🗺️
