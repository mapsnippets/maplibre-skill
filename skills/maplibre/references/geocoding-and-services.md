# Geocoding & Geospatial REST Services Reference

This reference provides direct REST API specifications for address search, autocomplete, reverse geocoding, and elevation queries without external SDK dependencies.

---

## 1. Forward Geocoding & Autocomplete

Search for addresses, cities, countries, or points of interest:

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

## 3. Point Elevation Lookup

Query the exact elevation (in meters) for a geographic location:

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
