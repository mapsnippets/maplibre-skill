# MapLibre GL JS — Expressions Reference

Expressions are the core of data-driven styling in MapLibre. They are JSON arrays that compute values at render time based on feature properties, zoom level, and other inputs.

> [Expression Specification](https://maplibre.org/maplibre-style-spec/expressions/)

---

## Syntax

Expressions are arrays: `[operator, ...arguments]`

```js
// Simple: get a property value
['get', 'name']

// Nested: interpolate color by zoom
['interpolate', ['linear'], ['zoom'], 10, '#0000ff', 15, '#ff0000']
```

---

## Data Access

### get

Read feature property.

```js
['get', 'name']                    // → feature.properties.name
['get', 'value', ['properties']]   // explicit object access
```

### has

Check if property exists.

```js
['has', 'point_count']   // true for clustered features
```

### at / length

Array access.

```js
['at', 0, ['get', 'colors']]   // first element
['length', ['get', 'tags']]     // array length
```

### geometry-type / id

```js
['geometry-type']   // 'Point', 'LineString', 'Polygon'
['id']              // feature id
```

---

## Feature State

Access state set by `map.setFeatureState()`.

```js
['feature-state', 'hover']     // read 'hover' state
['feature-state', 'selected']
```

Usage in layer paint:

```js
paint: {
  'circle-color': [
    'case',
    ['boolean', ['feature-state', 'hover'], false],
    '#ff0000',   // hovered
    '#0000ff'    // default
  ]
}
```

---

## Lookup / Decision

### case

If/else chain.

```js
[
  'case',
  ['==', ['get', 'type'], 'park'], '#00ff00',
  ['==', ['get', 'type'], 'water'], '#0000ff',
  '#cccccc'   // fallback
]
```

### match

Switch on exact values (more efficient than case for many values).

```js
[
  'match', ['get', 'category'],
  'food', '#ff0000',
  'shop', '#0000ff',
  'hotel', '#00ff00',
  '#888888'   // fallback
]
```

### step

Discrete value ranges (no interpolation).

```js
[
  'step', ['get', 'point_count'],
  '#51bbd6',       // default: count < 100
  100, '#f1f075',  // 100 <= count < 750
  750, '#f28cb1'   // count >= 750
]
```

### coalesce

First non-null value.

```js
['coalesce', ['get', 'name_en'], ['get', 'name'], 'Unknown']
```

---

## Interpolation

### interpolate

Smooth transition between stops.

```js
// Linear interpolation by zoom
[
  'interpolate', ['linear'], ['zoom'],
  10, 2,    // zoom 10 → value 2
  15, 20    // zoom 15 → value 20
]

// Exponential interpolation
[
  'interpolate', ['exponential', 1.5], ['zoom'],
  10, 2,
  15, 20
]

// Cubic bezier
[
  'interpolate', ['cubic-bezier', 0.42, 0, 0.58, 1], ['zoom'],
  10, 2,
  15, 20
]
```

### interpolate-hcl / interpolate-lab

Color space interpolation (smoother color transitions).

```js
[
  'interpolate-hcl', ['linear'], ['get', 'temperature'],
  0, '#0000ff',
  50, '#ff0000'
]
```

---

## Zoom and Heatmap

### zoom

Current map zoom level. Only available as input to `interpolate` and `step` at the top level.

```js
['interpolate', ['linear'], ['zoom'], 10, 5, 15, 20]
```

### heatmap-density

Value from 0-1 representing heatmap kernel density. Only for heatmap-color.

```js
[
  'interpolate', ['linear'], ['heatmap-density'],
  0, 'transparent',
  0.5, 'yellow',
  1, 'red'
]
```

### line-progress

Value from 0-1 along a line. Only for line-gradient.

```js
[
  'interpolate', ['linear'], ['line-progress'],
  0, 'blue',
  1, 'red'
]
```

---

## Comparison

```js
['==', ['get', 'type'], 'park']       // equals
['!=', ['get', 'type'], 'park']       // not equals
['>', ['get', 'population'], 1000000] // greater than
['>=', ['get', 'population'], 1000000]
['<', ['get', 'year'], 2000]
['<=', ['get', 'year'], 2000]
```

---

## Boolean Logic

```js
['all', expr1, expr2]     // AND
['any', expr1, expr2]     // OR
['!', expr]               // NOT
['boolean', value, fallback]  // coerce to boolean
```

### Filters using expressions

```js
// Filter: only show restaurants
filter: ['==', ['get', 'type'], 'restaurant']

// Multiple conditions
filter: ['all',
  ['==', ['get', 'type'], 'restaurant'],
  ['>', ['get', 'rating'], 4]
]

// In a set of values
filter: ['in', ['get', 'type'], ['literal', ['restaurant', 'cafe', 'bar']]]

// Clustered features filter
filter: ['has', 'point_count']       // only clusters
filter: ['!', ['has', 'point_count']] // only unclustered
```

---

## Math

```js
['+', a, b]          // addition
['-', a, b]          // subtraction
['*', a, b]          // multiplication
['/', a, b]          // division
['%', a, b]          // modulo
['^', a, b]          // exponent
['abs', value]
['ceil', value]
['floor', value]
['round', value]
['min', a, b, c]
['max', a, b, c]
['sqrt', value]
['log2', value]
['log10', value]
['ln', value]
['sin', value]       // radians
['cos', value]
['tan', value]
['asin', value]
['acos', value]
['atan', value]
['e']                // Euler's number
['pi']
```

---

## String

```js
['concat', 'Hello ', ['get', 'name']]
['upcase', ['get', 'name']]
['downcase', ['get', 'name']]
['to-string', ['get', 'value']]
['number-format', ['get', 'price'], { 'min-fraction-digits': 2, 'max-fraction-digits': 2 }]
['slice', ['get', 'name'], 0, 3]    // substring
['index-of', 'a', ['get', 'name']]  // find character
```

---

## Type Conversion

```js
['to-number', value]
['to-string', value]
['to-boolean', value]
['to-color', value]
['typeof', value]          // 'string', 'number', 'boolean', etc.
['to-rgba', color]         // [r, g, b, a] array
['rgb', r, g, b]           // color from components
['rgba', r, g, b, a]
```

---

## Common Expression Recipes

### Circle size by zoom + property

```js
'circle-radius': [
  'interpolate', ['linear'], ['zoom'],
  10, ['*', ['get', 'magnitude'], 1],
  15, ['*', ['get', 'magnitude'], 4]
]
```

### Color by category

```js
'fill-color': [
  'match', ['get', 'landuse'],
  'residential', '#e8d8c8',
  'commercial', '#c8d8e8',
  'industrial', '#d8c8c8',
  'park', '#c8e8c8',
  '#f0f0f0'   // default
]
```

### Opacity by zoom (fade in)

```js
'fill-opacity': [
  'interpolate', ['linear'], ['zoom'],
  10, 0,       // invisible at zoom 10
  12, 0.5,     // half opacity at 12
  14, 1        // full opacity at 14
]
```

### Conditional text label

```js
'text-field': [
  'case',
  ['has', 'name_en'], ['get', 'name_en'],
  ['has', 'name'], ['get', 'name'],
  'No name'
]
```

### Dynamic icon based on property

```js
'icon-image': [
  'match', ['get', 'amenity'],
  'restaurant', 'restaurant-icon',
  'hospital', 'hospital-icon',
  'school', 'school-icon',
  'default-icon'
]
```
