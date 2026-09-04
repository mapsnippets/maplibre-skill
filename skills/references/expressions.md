# MapLibre Expressions Reference & Operator Encyclopedia 🧮

> The definitive technical dictionary of the **MapLibre Style Specification Expression DSL**. Expressions evaluate directly within GPU WebGL shaders and worker tile parsers, enabling dynamic, 60 FPS data-driven cartography.

---

## 1. Syntax Architecture & Evaluation Rules

An expression is defined as a JSON array where the first element is an **operator string**, followed by zero or more **arguments**:

```json
["operator", "argument_1", "argument_2", "..."]
```

Arguments may themselves be nested expressions, literals, or property references:
```json
[
  "interpolate",
  ["linear"],
  ["zoom"],
  10, ["*", ["get", "lanes"], 1.5],
  16, ["*", ["get", "lanes"], 4.0]
]
```

### Expression Evaluation Contexts
1. **Property Expressions**: Evaluated on feature attributes (`["get", "population"]`, `["geometry-type"]`).
2. **Camera Expressions**: Evaluated on camera state (`["zoom"]`, `["pitch"]`, `["distance-from-center"]`).
3. **Composite Expressions**: Evaluated across both feature data and camera state simultaneously.
4. **Feature-State Expressions**: Evaluated dynamically on GPU feature state (`["feature-state", "hover"]`) without re-parsing tiles.

---

## 2. Complete Operator Dictionary by Category

### A. Feature Data & State Inspection

| Operator | Signature | Return Type | Description & Example |
| :--- | :--- | :--- | :--- |
| **`get`** | `["get", property, object?]` | `Value` | Retrieves property value from current feature or object.<br>`["get", "class"]` |
| **`has`** | `["has", property, object?]` | `Boolean` | Returns `true` if property key exists.<br>`["has", "render_height"]` |
| **`id`** | `["id"]` | `Value` | Returns the feature's unique ID (integer or string).<br>`["==", ["id"], 1042]` |
| **`geometry-type`** | `["geometry-type"]` | `String` | Returns `"Point"`, `"MultiPoint"`, `"LineString"`, `"MultiLineString"`, `"Polygon"`, or `"MultiPolygon"`.<br>`["==", ["geometry-type"], "Polygon"]` |
| **`properties`** | `["properties"]` | `Object` | Returns the complete properties map of the feature. |
| **`feature-state`** | `["feature-state", stateKey]` | `Value` | Reads dynamic runtime state set via `map.setFeatureState()`.<br>`["boolean", ["feature-state", "hover"], false]` |

---

### B. Control Flow & Decision Logic

#### 1. `case` (If / Else-If / Else)
Evaluates conditions sequentially, returning the value of the first condition that evaluates to `true`:
```json
[
  "case",
  ["boolean", ["feature-state", "hover"], false], "#00D2FF",
  [">", ["get", "population"], 1000000], "#0084FF",
  [">", ["get", "population"], 250000], "#38bdf8",
  "#94a3b8" // Default fallback
]
```

#### 2. `match` (Switch / Case)
Compares an input expression against label values:
```json
[
  "match",
  ["get", "class"],
  ["motorway", "trunk"], "#ef4444",
  ["primary", "secondary"], "#f59e0b",
  "tertiary", "#3b82f6",
  "#64748b" // Fallback default
]
```

#### 3. `coalesce` (Nullish Coalescing)
Evaluates arguments in order and returns the first non-null value:
```json
[
  "coalesce",
  ["get", "name:en"],
  ["get", "name:latin"],
  ["get", "name"],
  "Unnamed Landmark"
]
```

---

### C. Comparison & Boolean Operators

| Operator | Signature | Return Type | Description |
| :--- | :--- | :--- | :--- |
| **`==`** | `["==", a, b]` | `Boolean` | Returns `true` if `a` equals `b`. |
| **`!=`** | `["!=", a, b]` | `Boolean` | Returns `true` if `a` does not equal `b`. |
| **`<`** | `["<", a, b]` | `Boolean` | Returns `true` if `a < b`. |
| **`<=`** | `["<=", a, b]` | `Boolean` | Returns `true` if `a <= b`. |
| **`>`** | `[">", a, b]` | `Boolean` | Returns `true` if `a > b`. |
| **`>=`** | `[">=", a, b]` | `Boolean` | Returns `true` if `a >= b`. |
| **`!`** | `["!", a]` | `Boolean` | Logical NOT inversion. |
| **`all`** | `["all", c1, c2, ...]` | `Boolean` | Logical AND. Returns `true` if all conditions evaluate to `true`. |
| **`any`** | `["any", c1, c2, ...]` | `Boolean` | Logical OR. Returns `true` if at least one condition evaluates to `true`. |

---

### D. Continuous Curves & Stepped Ramps

#### 1. `interpolate` (Smooth Curves)
Computes smooth transitions between input-output pairs.
```json
[
  "interpolate",
  ["interpolation-type"],
  ["input-expression"],
  stop_input_1, stop_output_1,
  stop_input_2, stop_output_2,
  "..."
]
```

* **Interpolation Types:**
  * `["linear"]`: Straight-line linear interpolation between stops.
  * `["exponential", base]`: Exponential curve where `base` dictates curvature (e.g. `["exponential", 1.5]`).
  * `["cubic-bezier", x1, y1, x2, y2]`: Custom cubic bezier timing curve.

* **Example: Zoom-dependent Line Width:**
```json
"line-width": [
  "interpolate",
  ["exponential", 1.4],
  ["zoom"],
  5, 0.5,
  10, 2.0,
  14, 6.0,
  18, 16.0
]
```

#### 2. `step` (Discontinuous Stepped Ramps)
Produces stepped discrete values without blending. Ideal for discrete class buckets:
```json
"circle-color": [
  "step",
  ["get", "point_count"],
  "#00D2FF",   // Default (count < 10)
  10, "#0084FF", // 10 <= count < 50
  50, "#ef4444"  // count >= 50
]
```

---

### E. Mathematical & Arithmetic Operators

| Operator | Syntax | Description |
| :--- | :--- | :--- |
| **`+`** | `["+", a, b, ...]` | Sum of arguments. |
| **`-`** | `["-", a, b]` | Subtraction (`a - b`) or negation `["-", a]`. |
| **`*`** | `["*", a, b, ...]` | Multiplication of arguments. |
| **`/`** | `["/", a, b]` | Floating point division (`a / b`). |
| **`%`** | `["%", a, b]` | Modulo remainder (`a % b`). |
| **`^`** | `["^", a, b]` | Power exponentiation ($a^b$). |
| **`sqrt`** | `["sqrt", a]` | Square root of $a$. |
| **`abs`** | `["abs", a]` | Absolute value of $a$. |
| **`min`** | `["min", a, b, ...]` | Minimum argument value. |
| **`max`** | `["max", a, b, ...]` | Maximum argument value. |
| **`round`** | `["round", a]` | Nearest integer rounding. |
| **`floor`** | `["floor", a]` | Floor rounding down. |
| **`ceil`** | `["ceil", a]` | Ceiling rounding up. |
| **`ln`** | `["ln", a]` | Natural logarithm ($\ln a$). |
| **`log10`** | `["log10", a]` | Base-10 logarithm ($\log_{10} a$). |
| **`sin` / `cos` / `tan`** | `["sin", rad]` | Trigonometric functions in radians. |

---

### F. String & Typography Formatting

#### 1. `concat`
Concatenates strings into a single string:
```json
"text-field": [
  "concat",
  ["get", "name"],
  " (Elev: ",
  ["to-string", ["get", "ele"]],
  "m)"
]
```

#### 2. `format` (Rich Multi-Style Text)
Combines text segments with distinct fonts, colors, and font scales inside a single label:
```json
"text-field": [
  "format",
  ["get", "name"], { "font-scale": 1.2, "text-color": "#0084FF" },
  "
", {},
  ["concat", "Pop: ", ["to-string", ["get", "population"]]], {
    "font-scale": 0.85,
    "text-color": "#64748b"
  }
]
```

---

### G. Type Assertions & Conversions

| Operator | Syntax | Description |
| :--- | :--- | :--- |
| **`to-number`** | `["to-number", val, fallback?]` | Converts string/boolean to number. |
| **`to-string`** | `["to-string", val]` | Converts value to string representation. |
| **`to-boolean`** | `["to-boolean", val]` | Converts value to boolean. |
| **`to-color`** | `["to-color", val, fallback?]` | Parses CSS color string into WebGL Color. |
| **`typeof`** | `["typeof", val]` | Returns `"string"`, `"number"`, `"boolean"`, `"null"`, `"array"`, `"object"`. |

---

## 3. Production Recipe Patterns

### Dynamic 3D Building Heights with Fallbacks
```json
"fill-extrusion-height": [
  "interpolate", ["linear"], ["zoom"],
  14, 0,
  14.5, [
    "coalesce",
    ["get", "render_height"],
    ["*", ["coalesce", ["get", "levels"], 1], 3.5]
  ]
]
```

### 60 FPS GPU Hover Highlight via Feature State
```json
"fill-opacity": [
  "case",
  ["boolean", ["feature-state", "hover"], false], 0.85,
  ["boolean", ["feature-state", "selected"], false], 0.95,
  0.25
]
```

### Multi-Color Route Gradient (`line-gradient`)
```json
"line-gradient": [
  "interpolate",
  ["linear"],
  ["line-progress"],
  0.0, "#00D2FF",
  0.5, "#0084FF",
  1.0, "#ef4444"
]
```
