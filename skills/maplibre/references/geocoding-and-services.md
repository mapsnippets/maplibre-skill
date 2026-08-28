# Geospatial REST Services & APIs Reference

This reference provides direct REST API specifications for address search, autocomplete, reverse geocoding, static map images, IP geolocation, and elevation queries without requiring wrapper SDKs.

---

## 1. Forward Geocoding & Address Autocomplete

Search for addresses, cities, countries, or points of interest.

### Endpoint:
```text
GET https://api.maptiler.com/geocoding/{query}.json?key=YOUR_API_KEY
```

### Common Query Parameters:
| Parameter | Type | Example | Description |
| :--- | :--- | :--- | :--- |
| `proximity` | `lng,lat` | `14.4378,50.0755` | Bias results near user coordinates |
| `bbox` | `minX,minY,maxX,maxY` | `14.2,49.9,14.7,50.2` | Restrict search to bounding box |
| `country` | String (ISO 3166-1) | `us,cz,de` | Filter results to specific countries |
| `types` | Comma-separated | `address,poi,city` | Filter feature categories (`country,region,subregion,city,postal_code,poi,address`) |
| `language` | String | `en` or `de` | Preferred language for names |
| `limit` | Integer | `5` | Maximum number of results (1–10) |
| `autocomplete` | Boolean | `true` | Enable prefix matching for search inputs |

### Example JavaScript Fetch:
```javascript
async function searchAddress(query, userLng, userLat, apiKey) {
  const url = `https://api.maptiler.com/geocoding/${encodeURIComponent(query)}.json?key=${apiKey}&proximity=${userLng},${userLat}&limit=5&autocomplete=true`;
  const response = await fetch(url);
  const data = await response.json();
  return data.features.map(f => ({
    name: f.place_name,
    coordinates: f.center, // [lng, lat]
    bbox: f.bbox
  }));
}
```

---

## 2. Reverse Geocoding

Find the address or location details at a given GPS coordinate:

### Endpoint:
```text
GET https://api.maptiler.com/geocoding/{longitude},{latitude}.json?key=YOUR_API_KEY
```

### Example JavaScript Fetch:
```javascript
async function reverseGeocode(lng, lat, apiKey) {
  const url = `https://api.maptiler.com/geocoding/${lng},${lat}.json?key=${apiKey}&limit=1`;
  const response = await fetch(url);
  const data = await response.json();
  return data.features[0]?.place_name ?? "Unknown Location";
}
```

---

## 3. Static Maps API (Non-Interactive Image Generator)

Generate static PNG / JPEG images of maps for emails, social share cards (og:image), or PDF reports:

### Center & Zoom Endpoint:
```text
GET https://api.maptiler.com/maps/{styleId}/static/{longitude},{latitude},{zoom}/{width}x{height}@2x.png?key=YOUR_API_KEY
```

### Auto-Fitted with Markers:
```text
GET https://api.maptiler.com/maps/streets-v4/static/auto/600x400@2x.png?markers=14.4378,50.0755,red|14.412,50.088,blue&key=YOUR_API_KEY
```

### Parameters:
* `styleId`: `streets-v4`, `outdoor-v4`, `satellite-v4`, `dataviz-v4-dark`
* `width` / `height`: Pixel dimensions (max 2048px)
* `scale`: `@2x` for retina high-DPI
* `markers`: `lng,lat,color` (e.g. `14.43,50.07,red`)
* `path`: Polyline styling (e.g. `stroke:blue|width:3|14.41,50.08|14.43,50.07`)

---

## 4. Point Elevation Lookup

Query the exact elevation (in meters) for any coordinate on Earth:

### Endpoint:
```text
GET https://api.maptiler.com/elevation/lookup/{longitude},{latitude}.json?key=YOUR_API_KEY
```

### Example Response:
```json
{
  "elevation": 245.5,
  "unit": "meters"
}
```

---

## 5. IP Geolocation API

Determine the user's approximate location (city, country, coordinates) based on client IP:

### Endpoint:
```text
GET https://api.maptiler.com/geolocation/ip.json?key=YOUR_API_KEY
```

### Example Response:
```json
{
  "country": "Czechia",
  "country_code": "CZ",
  "city": "Prague",
  "latitude": 50.0755,
  "longitude": 14.4378
}
```
