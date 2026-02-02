# Hugo Maps

A [Hugo](https://gohugo.io/) module for adding beautiful, interactive maps to your site using [MapLibre GL JS](https://maplibre.org/).

![Hugo Maps Example](map.png)

## Features

- 🗺️ **Vector Maps** - Crisp, scalable vector tile maps
- 🎨 **Multiple Styles** - 7 built-in map styles (light, dark, 3D, and more)
- 📍 **Address Geocoding** - Automatic location lookup from addresses
- 🔄 **Multi-Provider Geocoding** - Support for Photon, Nominatim, and custom providers
- ✈️ **Fly-To Animation** - Smooth animated transitions to locations
- 📌 **Markers & Popups** - Add markers with customizable popups
- 🌓 **Dark Mode Support** - Responsive styling for light/dark themes
- ⚙️ **Highly Configurable** - Extensive parameter system with sensible defaults

## Quick Start

### 1. Install the Module

Add to your `config.yaml`:

```yaml
module:
  imports:
    - path: "github.com/kylehakala/hugo-maps"
```

Run:

```bash
hugo mod get
```

### 2. Configure a Map

Add a map configuration to your site's `config.yaml`:

```yaml
params:
  hugo-maps:
    # Default settings for all maps (optional)
    default:
      height: 400px
      navigationControl: true
      sources:
        openmaptiles:
          type: vector
          url: https://api.maptiler.com/tiles/v3/tiles.json?key=YOUR_KEY

    # Your first map
    my_map:
      style: osm-bright
      center: [-74.006, 40.7128]  # [longitude, latitude]
      zoom: 12
```

### 3. Use the Shortcode

In your content:

```markdown
{{</* map name="my_map" */>}}
```

---

## Configuration Reference

### Parameter Hierarchy

Parameters are merged in this order (later values override earlier):

1. **Module defaults** (`assets/default/mapparams.yaml`)
2. **Site defaults** (`params.hugo-maps.default` in config)
3. **Map-specific config** (`params.hugo-maps.your_map_name`)
4. **Page frontmatter** (for geocoding parameters)
5. **Shortcode attributes** (highest priority)

---

## Map Parameters

### Display

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `width` | string | `"100%"` | CSS width of the map container |
| `height` | string | `"300px"` | CSS height of the map container |
| `style` | string | `"osm-bright"` | Map style theme (see [Available Styles](#available-styles)) |

### View

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `center` | array | `[-0.127758, 51.507351]` | Map center as `[longitude, latitude]` |
| `zoom` | number | `11` | Initial zoom level (0-23) |
| `minZoom` | number | `0` | Minimum allowed zoom level |
| `maxZoom` | number | `23` | Maximum allowed zoom level |
| `bearing` | number | `0` | Map rotation in degrees (0-360) |
| `pitch` | number | `0` | Map tilt in degrees (0-60) |
| `minPitch` | number | `0` | Minimum allowed pitch |
| `maxPitch` | number | `60` | Maximum allowed pitch |

### Controls

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `interactive` | boolean | `true` | Enable pan/zoom interactions |
| `navigationControl` | boolean | `true` | Show zoom and compass controls |
| `attributionControl` | boolean | `true` | Show attribution |
| `customAttribution` | string | `""` | Additional attribution text |

### Rendering

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `antialias` | boolean | `false` | Enable antialiasing (better quality, slower) |
| `type` | string | `"remote"` | Map type: `"remote"` or `"local"` |

### Tile Sources

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `sources` | object | `{}` | Tile source definitions |
| `tilesMinZoom` | number | - | Minimum zoom for local tiles |
| `tilesMaxZoom` | number | - | Maximum zoom for local tiles |

---

## Available Styles

| Style | Description |
|-------|-------------|
| `osm-bright` | Clean, colorful OpenStreetMap style |
| `positron` | Light, minimal style (good for data overlays) |
| `darkmatter` | Dark theme |
| `toner` | High-contrast black and white |
| `maptiler-basic` | Simple, clean style |
| `maptiler-3d` | 3D buildings and terrain |
| `fjord-color` | Colorful Nordic-inspired theme |

---

## Geocoding

Hugo Maps can automatically geocode addresses to coordinates, displaying a marker at the location with a fly-to animation.

### Basic Usage

1. Add an address to your page frontmatter:

```yaml
---
title: "My Location"
address: "Empire State Building, New York, NY"
---
```

2. The map will automatically geocode and display a marker.

### Geocoding Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `geocodeParams` | string/array | `"address"` | Frontmatter field(s) to use for geocoding |
| `geocoders` | string | `"photon,nominatim"` | Geocoder provider(s) to use |
| `geocoderConfig` | object | `{}` | Provider-specific configuration |
| `flyToDuration` | number | `2500` | Animation duration in milliseconds |
| `addressZoom` | number | (uses `zoom`) | Zoom level after geocoding |

### Using Multiple Frontmatter Fields

Combine multiple fields for more accurate geocoding:

```yaml
# In config.yaml
params:
  hugo-maps:
    location_map:
      geocodeParams:
        - address
        - city
        - country
```

```yaml
# In page frontmatter
---
address: "123 Main Street"
city: "Seattle"
country: "USA"
---
```

This creates a geocode query of: `"123 Main Street, Seattle, USA"`

### Shortcode Overrides

Override geocoding settings per-shortcode:

```markdown
{{</* map name="my_map" address="Space Needle, Seattle" address-zoom="15" */>}}
```

---

## Geocoding Providers

### Built-in Providers

#### Photon (Default Primary)

Free geocoding service by Komoot. No API key required.

```yaml
geocoderConfig:
  photon:
    url: "https://photon.komoot.io"  # Optional, this is default
```

#### Nominatim (Default Fallback)

OpenStreetMap's geocoding service. Requires User-Agent header.

```yaml
geocoderConfig:
  nominatim:
    url: "https://nominatim.openstreetmap.org"
    userAgent: "my-hugo-site/1.0"
```

### Custom Providers (Template Geocoder)

Any geocoding API can be configured using the template system:

```yaml
geocoders: "my_provider,photon"  # Use custom provider with fallback
geocoderConfig:
  my_provider:
    url: "https://api.example.com/geocode"
    apiKey: "YOUR_API_KEY"
    params:
      q: "{query}"           # {query} is replaced with the search text
      key: "{apiKey}"        # {apiKey} is replaced with your API key
    # Response parsing (optional - many formats auto-detected)
    resultsPath: "results"   # Path to results array in response
    latPath: "geometry.lat"  # Path to latitude in each result
    lonPath: "geometry.lng"  # Path to longitude in each result
    namePath: "formatted"    # Path to place name in each result
```

### Template Placeholders

| Placeholder | Description |
|-------------|-------------|
| `{query}` | The geocoding search query |
| `{apiKey}` | The API key from `apiKey` field |

### Auto-Detected Response Formats

The template geocoder automatically detects these formats:

- **GeoJSON**: `features[].geometry.coordinates`
- **Nominatim-style**: `[].lat`, `[].lon`, `[].display_name`
- **Google-style**: `[].lat`, `[].lng`, `[].formatted_address`
- **Bounding box**: Calculates center from `[].bbox`

### Provider Examples

<details>
<summary><strong>geocode.maps.co</strong> (Free tier available)</summary>

```yaml
geocoderConfig:
  geocode_maps_co:
    url: "https://geocode.maps.co/search"
    apiKey: "YOUR_API_KEY"
    params:
      q: "{query}"
      api_key: "{apiKey}"
```

</details>

<details>
<summary><strong>LocationIQ</strong></summary>

```yaml
geocoderConfig:
  locationiq:
    url: "https://us1.locationiq.com/v1/search"
    apiKey: "YOUR_API_KEY"
    params:
      q: "{query}"
      key: "{apiKey}"
      format: "json"
```

</details>

<details>
<summary><strong>OpenCage</strong></summary>

```yaml
geocoderConfig:
  opencage:
    url: "https://api.opencagedata.com/geocode/v1/json"
    apiKey: "YOUR_API_KEY"
    params:
      q: "{query}"
      key: "{apiKey}"
    resultsPath: "results"
    latPath: "geometry.lat"
    lonPath: "geometry.lng"
    namePath: "formatted"
```

</details>

<details>
<summary><strong>Geoapify</strong></summary>

```yaml
geocoderConfig:
  geoapify:
    url: "https://api.geoapify.com/v1/geocode/search"
    apiKey: "YOUR_API_KEY"
    params:
      text: "{query}"
      apiKey: "{apiKey}"
      format: "json"
    resultsPath: "results"
    latPath: "lat"
    lonPath: "lon"
    namePath: "formatted"
```

</details>

<details>
<summary><strong>PositionStack</strong></summary>

```yaml
geocoderConfig:
  positionstack:
    url: "http://api.positionstack.com/v1/forward"
    apiKey: "YOUR_API_KEY"
    params:
      query: "{query}"
      access_key: "{apiKey}"
    resultsPath: "data"
    latPath: "latitude"
    lonPath: "longitude"
    namePath: "label"
```

</details>

---

## Complete Configuration Example

```yaml
# config.yaml
params:
  hugo-maps:
    # Global defaults
    default:
      height: 450px
      navigationControl: true
      flyToDuration: 2000
      geocoders: "photon,nominatim"
      sources:
        openmaptiles:
          type: vector
          url: https://api.maptiler.com/tiles/v3/tiles.json?key=YOUR_MAPTILER_KEY

    # Simple map centered on a location
    office_map:
      style: osm-bright
      center: [-122.4194, 37.7749]
      zoom: 14

    # 3D map with tilt
    city_3d:
      style: maptiler-3d
      center: [-74.006, 40.7128]
      zoom: 16
      pitch: 45
      bearing: -17
      antialias: true

    # Map that geocodes from frontmatter
    location_map:
      style: positron
      zoom: 15
      geocodeParams:
        - address
        - city
        - state
      addressZoom: 16
      flyToDuration: 3000

    # Dark themed map
    night_map:
      style: darkmatter
      center: [2.3522, 48.8566]
      zoom: 12
```

---

## Page Frontmatter

When using geocoding, add location data to your page frontmatter:

```yaml
---
title: "Company Headquarters"
address: "123 Tech Boulevard"
city: "San Francisco"
state: "CA"
addressZoom: 16  # Optional: override zoom for this page
---

Welcome to our office! Here's a map:

{{</* map name="location_map" */>}}
```

---

## Shortcode Reference

```markdown
{{</* map name="map_name" [address="..."] [address-zoom="..."] */>}}
```

| Attribute | Required | Description |
|-----------|----------|-------------|
| `name` | Yes | Name of the map configuration in `params.hugo-maps` |
| `address` | No | Override the geocoding address |
| `address-zoom` | No | Override the zoom level after geocoding |

---

## Tile Providers

Hugo Maps requires a vector tile source. Here are some options:

### MapTiler (Recommended)

Free tier available with generous limits.

```yaml
sources:
  openmaptiles:
    type: vector
    url: https://api.maptiler.com/tiles/v3/tiles.json?key=YOUR_KEY
```

Get a free key at [maptiler.com](https://www.maptiler.com/cloud/).

### Self-Hosted Tiles

For self-hosted tiles:

```yaml
type: local
sources:
  openmaptiles:
    type: vector
    url: /tiles/{z}/{x}/{y}.pbf
tilesMinZoom: 0
tilesMaxZoom: 14
```

---

## Styling

### Dark Mode Support

Hugo Maps automatically adapts popup styling to dark mode using CSS media queries. The popups use semi-transparent backgrounds that work well in both light and dark themes.

### Custom CSS

The map container has the class `hugo-maps-container` and a unique ID. You can add custom styles:

```css
.hugo-maps-container {
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}
```

---

## Browser Support

Hugo Maps uses MapLibre GL JS which supports:

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

WebGL support is required.

---

## License

MIT License - see [LICENSE](LICENSE) for details.

---

## Credits

- [MapLibre GL JS](https://maplibre.org/) - Map rendering
- [OpenStreetMap](https://www.openstreetmap.org/) - Map data
- [Photon](https://photon.komoot.io/) - Geocoding
- [OpenMapTiles](https://openmaptiles.org/) - Vector tile schema
