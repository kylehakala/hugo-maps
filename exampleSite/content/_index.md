---
title: Hugo Maps Examples
---

# Hugo Maps Examples

Welcome to **hugo-maps**, a Hugo module that makes it easy to add beautiful, interactive maps to your website!

## What is hugo-maps?

hugo-maps is a modern Hugo module powered by [MapLibre GL](https://maplibre.org/) that allows you to embed interactive vector maps with minimal configuration.

### Key Features

- 🗺️ Beautiful vector maps with multiple built-in styles
- 🎨 Responsive design that works on all devices
- 📍 Custom markers and annotations
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

### Example 4: Map with Marker (Los Angeles)

{{< map name="map_with_marker" >}}

Demonstrates custom markers with popup annotations.

---

### Example 5: Interactive Map (Oslo)

{{< map name="interactive_map" >}}

A fully interactive map with all controls enabled.

---

### Example 6: Auto-Geocoding (Address Lookup)

{{< map name="simple_map" address="Space Needle, Seattle" >}}

This map demonstrates automatic address geocoding. Pass an `address` parameter to any map shortcode and it will automatically search for that location using OpenStreetMap's Nominatim service and fly to it!

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

Maps support extensive customization:

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `style` | string | osm-bright | Map theme/style |
| `center` | array | [0, 0] | [longitude, latitude] coordinates |
| `zoom` | number | 12 | Initial zoom level (0-20) |
| `minZoom` | number | 0 | Minimum zoom level |
| `maxZoom` | number | 20 | Maximum zoom level |
| `pitch` | number | 0 | Camera pitch in degrees (0-60) |
| `bearing` | number | 0 | Camera bearing in degrees (0-359) |
| `antialias` | bool | false | Enable antialiasing for smoother edges |
| `navigationControl` | bool | true | Show zoom/rotation controls |
| `interactive` | bool | true | Allow user interaction |
| `attributionControl` | bool | true | Show attribution |
| `height` | string | 300px | Map container height |
| `width` | string | 100% | Map container width |
| `markers` | array | [] | Array of marker objects |

### Marker Configuration

Add markers with popup text:

```yaml
markers:
  - location: [-74.006, 40.7128]
    text: "New York City"
  - location: [-118.243, 34.052]
    text: "Los Angeles"
```

---

## Browser Support

hugo-maps works on all modern browsers:

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

---

## Learn More

For more information, documentation, and advanced usage:

- **GitHub**: [kylehakala/hugo-maps](https://github.com/kylehakala/hugo-maps)
- **MapLibre GL**: [maplibre.org](https://maplibre.org/)
- **Hugo**: [gohugo.io](https://gohugo.io/)

---

## License

hugo-maps is open source and available under the Apache 2.0 License.

Happy mapping! 🗺️
