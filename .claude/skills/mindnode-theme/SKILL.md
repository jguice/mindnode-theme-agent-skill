---
name: mindnode-theme
description: Create MindNode themes from color palettes. Use when the user wants to create a MindNode theme, convert colors to MindNode format, or build a .mindnodetheme or .mindnodedynamictheme file.
---

# MindNode Theme Creation

Create custom themes for MindNode mind mapping application.

## Theme Types

### Static Themes (`.mindnodetheme`)
- ZIP archive with XML plist files
- Single appearance (light OR dark)
- Simpler format, no per-level styling

### Dynamic Themes (`.mindnodedynamictheme`) - RECOMMENDED
- ZIP archive with JSON (`Contents.json`)
- Supports light/dark mode switching
- **Supports per-level styling** via `levelOverrides`
- Shows in "Dynamic Themes" section of MindNode

## Color Format

### For Static Themes (XML plist)
RGBA string with values 0.0-1.0:
```
{R, G, B, A}
```

### For Dynamic Themes (JSON)
Object with RGBA keys:
```json
{"red": 0.5, "green": 0.5, "blue": 0.5, "alpha": 1}
```

### Converting Hex to MindNode Format

```python
def hex_to_rgba(hex_color):
    """Convert #RRGGBB to RGBA values 0-1"""
    hex_color = hex_color.lstrip('#')
    r = int(hex_color[0:2], 16) / 255
    g = int(hex_color[2:4], 16) / 255
    b = int(hex_color[4:6], 16) / 255
    return r, g, b, 1.0
```

---

# Dynamic Theme Format (Recommended)

## File Structure
```
theme-name.mindnodedynamictheme (ZIP archive)
└── Contents.json
```

## Shape Types
- `0` = No shape (text only with underline)
- `2` = Rounded rectangle

## Recommended Border Widths
- `mainNodeStyle`: 4 (bold root node border)
- `baseSubnodeStyle`: 6 (bold child node borders)
- Level 2 override: 6 (matches subnodes)
- Level 3+ override: 0 (no border for text-only nodes)

## Key JSON Structure

```json
{
    "version": 1,
    "id": "UUID-HERE",
    "name": "Theme Name",
    "backgroundColor": { "lightValue": {...}, "darkValue": {...} },
    "mainNodeStyle": { ... },
    "baseSubnodeStyle": { ... },
    "levelOverrides": [ level_num, style_obj, level_num, style_obj, ... ],
    "firstLevelColors": [ {...}, {...}, ... ],
    "connectionStyle": { ... },
    "tagColorStorage": [],
    "nodeBorderWidth": {"undefined": {}}
}
```

## Dynamic Color Format

Each color has light and dark variants:
```json
"backgroundColor": {
    "lightValue": {"red": 1.0, "green": 1.0, "blue": 1.0, "alpha": 1},
    "darkValue": {"red": 0.15, "green": 0.15, "blue": 0.15, "alpha": 1}
}
```

## Title Colors (special format)

Title colors use appearance-based keys:
```json
"titleColor": {
    "againstLightBackgroundColor": {"red": 0.2, "green": 0.2, "blue": 0.2, "alpha": 1},
    "againstDarkBackgroundColor": {"red": 0.9, "green": 0.9, "blue": 0.9, "alpha": 1},
    "highContrastAgainstLightBackgroundColor": {"red": 0, "green": 0, "blue": 0, "alpha": 1},
    "highContrastAgainstDarkBackgroundColor": {"red": 1, "green": 1, "blue": 1, "alpha": 1}
}
```

## Level Overrides (Per-Level Styling)

The `levelOverrides` array alternates between level numbers and style objects. **IMPORTANT:** The order in the array is typically higher levels first (e.g., 3, then 2).

**Key behavior:**
- **Omitting shapeType** = Level KEEPS its box (inherits shapeType 2 from baseSubnodeStyle)
- **Including shapeType: 0** = Level has NO box (text only with underline)

### Example: Boxes for root, level 1, level 2; text-only for level 3+
```json
"levelOverrides": [
    3,
    {
        "shapeType": 0,      // NO box - text only
        "borderWidth": 0,
        "fillColor": { "lightValue": {"inherited": {"_0": {"fill": {}}}}, "darkValue": {"inherited": {"_0": {"fill": {}}}} },
        "borderColor": { "lightValue": {"inherited": {"_0": {"border": {}}}}, "darkValue": {"inherited": {"_0": {"border": {}}}} },
        "branchColor": { "lightValue": {"inherited": {"_0": {"branch": {}}}}, "darkValue": {"inherited": {"_0": {"branch": {}}}} }
    },
    2,
    {
        "borderWidth": 1,    // HAS box - no shapeType means inherit shapeType 2 (rounded rect)
        "fillColor": { "lightValue": {"inherited": {"_0": {"fill": {}}}}, "darkValue": {"inherited": {"_0": {"fill": {}}}} },
        "borderColor": { "lightValue": {"inherited": {"_0": {"border": {}}}}, "darkValue": {"inherited": {"_0": {"border": {}}}} },
        "branchColor": { "lightValue": {"inherited": {"_0": {"branch": {}}}}, "darkValue": {"inherited": {"_0": {"branch": {}}}} }
    }
]
```

### Example: Boxes for root and level 1 only; text-only for level 2+
```json
"levelOverrides": [
    2,
    {
        "shapeType": 0,
        "borderWidth": 0,
        "fillColor": { "lightValue": {"inherited": {"_0": {"fill": {}}}}, "darkValue": {"inherited": {"_0": {"fill": {}}}} },
        "borderColor": { "lightValue": {"inherited": {"_0": {"border": {}}}}, "darkValue": {"inherited": {"_0": {"border": {}}}} },
        "branchColor": { "lightValue": {"inherited": {"_0": {"branch": {}}}}, "darkValue": {"inherited": {"_0": {"branch": {}}}} }
    }
]
```

**Level numbering:**
- Level 0 = Root (use `mainNodeStyle`)
- Level 1 = First children (use `baseSubnodeStyle` + `firstLevelColors`)
- Level 2+ = Deeper levels (use `levelOverrides`)

**Inherited colors:** Use the `{"inherited": {"_0": {"fill/border/branch": {}}}}` pattern to inherit colors from the parent branch.

## First Level Colors (Branch Rainbow)

Array of color sets for level 1 branches. Each branch cycles through these colors:
```json
"firstLevelColors": [
    {
        "fillColor": { "lightValue": {...}, "darkValue": {...} },
        "borderColor": { "lightValue": {...}, "darkValue": {...} },
        "branchColor": { "lightValue": {...}, "darkValue": {...} }
    },
    // ... more color sets for rainbow effect
]
```

## Node Style Properties

Both `mainNodeStyle` and `baseSubnodeStyle` support these properties:

| Property | Type | Description |
|----------|------|-------------|
| `shapeType` | int | `0` = text only, `2` = rounded rectangle |
| `borderWidth` | int | Border thickness (0-10, recommend 4-6 for bold) |
| `borderDash` | int | `0` = solid, other values for dashed |
| `branchDash` | int | `0` = solid branches |
| `fillColor` | object | Background color with `lightValue`/`darkValue` |
| `borderColor` | object | Border color with `lightValue`/`darkValue` |
| `branchColor` | object | Branch/line color with `lightValue`/`darkValue` |
| `titleColor` | object | Text color (uses special 4-key format) |
| `titleFont` | object | `{"fontName": "Helvetica", "pointSize": 24}` |

## Connection Style (Cross-Links)

Style for connections between non-adjacent nodes:
```json
"connectionStyle": {
    "lineWidth": 2,
    "dashType": 0,
    "startDelimiter": {"line": {}},
    "endDelimiter": {"triangle": {}},
    "connectionColor": { "lightValue": {...}, "darkValue": {...} },
    "titleColor": { ... },  // 4-key format
    "titleFont": {"fontName": "Helvetica", "pointSize": 14}
}
```

**Delimiter types:** `{"line": {}}`, `{"triangle": {}}`, `{"circle": {}}`

## Creating a Dynamic Theme

1. **Generate UUID**: `uuidgen`
2. **Create Contents.json** (see template below)
3. **Validate JSON**: `python3 -m json.tool Contents.json > /dev/null`
4. **Package as ZIP**:
   ```bash
   zip theme-name.mindnodedynamictheme Contents.json
   ```

## Templates

- `examples/dynamic-theme-template.json` - Basic template with per-level styling
- `examples/gruvbox-dynamic.json` - Complete Gruvbox theme with light/dark support and 7 branch colors

---

# Static Theme Format (Legacy)

## File Structure
```
theme-name.mindnodetheme (ZIP archive)
├── contents.xml
└── metadata.plist
```

**IMPORTANT**: The `id` in metadata.plist MUST be a valid UUID (e.g., `C114C305-5ED2-4536-B7B4-FD5CFD69F4E4`). Invalid UUIDs crash MindNode.

## Creating a Static Theme

1. **Create temp directory**: `mkdir -p theme-name`
2. **Generate UUID**: `uuidgen`
3. **Create metadata.plist** and **contents.xml**
4. **Validate**: `plutil -lint contents.xml && plutil -lint metadata.plist`
5. **Package**: `cd theme-name && zip -r ../theme-name.mindnodetheme contents.xml metadata.plist`

See `examples/gruvbox-dark-contents.xml` for a complete static theme example.

---

# Installation

Double-click the `.mindnodetheme` or `.mindnodedynamictheme` file to install.

**Note**: If updating an existing theme, delete the old version from MindNode first (right-click → Delete) to avoid caching issues.
