# Animations

Two families of effects:

1. **Text animations** — **phase** placeholders that change colours/gradients in lines (MiniMessage, MineDown, …), updated with placeholder refresh.
2. **Display animations** — pose updates on **display entities** (`animation:` under each `displayGroup`).

---

## Phase placeholders (in lines)

These are **not** YAML `animation:` blocks — they are **inline tokens** in TEXT lines, replaced each placeholder refresh.

| Token | Role |
|-------|------|
| `#phase-mm#` | MiniMessage rainbow / phase value |
| `#phase-mm-g#` | MiniMessage gradient phase |
| `#phase-md#` | MineDown phase |
| `#-phase-mm#` | Negative phase (MiniMessage) |
| `#-phase-mm-g#` | Negative phase (gradient) |
| `#-phase-md#` | Negative phase (MineDown) |

PlaceholderAPI expansion **`unt`** exposes: `%unt_phase-mm%`, `%unt_phase-md%`, `%unt_phase-mm-g%`, `%unt_-phase-mm%`, `%unt_-phase-md%`. There is **no** `%unt_-phase-mm-g%` — use the inline `#-phase-mm-g#` in lines instead.

Perceived **speed** ties to **`taskInterval`**: more frequent refresh = faster colour cycling (higher CPU). See [Performance](../performance.md).

---

## Refresh rate vs display animations

| Setting | Controls |
|---------|----------|
| **`taskInterval`** | How often placeholders and text refresh (including `#phase-mm#`). |
| **`displayAnimationInterval`** | Ticks between **pose** updates if the row has no `animationInterval`. `0` = follow `taskInterval`. |
| **`animationInterval`** (per row) | Override for that row. |

**Higher** values = slower motion/refresh and usually **less load**.

---

## Display animations (`animation:`)

Every `animation:` block has a **`type`** and optional fields shared by **all** types:

| Field | Default | Notes |
|-------|---------|--------|
| `enabled` | `true` | If `false`, animation does nothing. |
| `speed` | `1.0` | Tempo multiplier (1 = default speed for that type). |
| `cullBeyondBlocks` | `0` | If &gt; 0, pose updates may be skipped when no viewer is within this distance (blocks). `0` = no culling. |
| `customProperties` | `{}` | String map for API / integrations; built-in animators ignore unknown keys. |

Then each **`type`** adds its own fields (defaults below).

### `rotate`

| Field | Default | Notes |
|-------|---------|--------|
| `axis` | `"Y"` | `Y` = turntable spin; `X` / `Z` = tilt plane; `XYZ` = slow tumble (combined axes). |
| `degreesPerSecond` | `90` | Degrees per second at `speed` 1. |

### `bob`

| Field | Default | Notes |
|-------|---------|--------|
| `amplitude` | `0.06` | Vertical motion amplitude (blocks, local Y). |
| `bobsPerSecond` | `1.0` | Full up–down cycles per second at `speed` 1. |

### `dvd_bounce`

| Field | Default | Notes |
|-------|---------|--------|
| `halfWidth` | `0.12` | Half-width of the bounce box along local X (blocks). |
| `halfDepth` | `0.1` | Half-depth along local Z (blocks); DVD-style motion in the horizontal plane above the tag. |
| `pace` | `0.35` | Travel speed scale (blocks/s at `speed` 1). |

### `pulse_scale`

| Field | Default | Notes |
|-------|---------|--------|
| `minMultiplier` | `0.92` | Minimum scale multiplier (relative to the row’s scale). |
| `maxMultiplier` | `1.08` | Maximum scale multiplier. |
| `pulsesPerSecond` | `1.0` | Full breathe cycles per second at `speed` 1. |

### `wiggle`

| Field | Default | Notes |
|-------|---------|--------|
| `amplitudeDegrees` | `10` | Peak tilt amplitude (degrees). |
| `wigglesPerSecond` | `2.0` | Wiggle cycles per second at `speed` 1. |

### `orbit`

| Field | Default | Notes |
|-------|---------|--------|
| `radius` | `0.1` | Orbit radius (blocks, local XZ plane). |
| `rotationsPerSecond` | `0.5` | Full orbits per second at `speed` 1. |

### `custom`

| Field | Default | Notes |
|-------|---------|--------|
| `id` | `""` | Must match a handler id registered with **`registerNametagCustomAnimation`** (`UNTAPI` / `UnlimitedNameTagsPlugin`). |

In YAML:

```yaml
animation:
  type: custom
  id: my_plugin_animation
  speed: 1.0
```

---

## Example: rotating item

```yaml
displayGroups:
  - displayType: ITEM
    itemMaterial: DIAMOND
    scale: 1.0
    yOffset: 0.2
    animationInterval: 2
    animation:
      type: rotate
      axis: Y
      degreesPerSecond: 120
      speed: 1.0
      cullBeyondBlocks: 32
```

---

## Summary: all built-in `type` values

| `type` | Purpose |
|--------|---------|
| `rotate` | Spin / tilt / tumble |
| `bob` | Vertical bob |
| `dvd_bounce` | Horizontal plane screensaver bounce |
| `pulse_scale` | Breathing scale |
| `wiggle` | Tilt wiggle |
| `orbit` | Circular motion in XZ |
| `custom` | Your handler via API |

---

## See also

- [Display groups](display-groups.md)  
- [Performance](../performance.md)  
