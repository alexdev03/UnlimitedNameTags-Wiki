# Animations

**UnlimitedNameTags** supports two primary categories of visual effects to make player name tags dynamic:

1. **Text Phase Placeholders**: Inline text tokens (e.g., `#phase-mm#`) that shift text color or gradients periodically.
2. **Display Group Motion**: Configured via the `animation:` block of a display group, causing the physical display entity to move (e.g., rotate, bob, pulse, or orbit).

---

## Text Phase Placeholders

Phase placeholders are shortcodes defined directly within a display group's text lines. These are parsed inline and update on each placeholder refresh cycle:

| Token | Formatting Engine | Effect |
| :--- | :--- | :--- |
| **`#phase-mm#`** | MiniMessage | Standard color transition phase. |
| **`#phase-mm-g#`** | MiniMessage | Multi-color gradient transition phase. |
| **`#phase-md#`** | MineDown | MineDown formatting color transition phase. |
| **`#-phase-mm#`** | MiniMessage | Reverse-direction color transition phase. |
| **`#-phase-mm-g#`** | MiniMessage | Reverse-direction gradient transition phase. |
| **`#-phase-md#`** | MineDown | Reverse-direction MineDown color transition phase. |

### PlaceholderAPI Integration
You can query phase values using the following PlaceholderAPI expansion variables:
- `%unt_phase-mm%`
- `%unt_phase-md%`
- `%unt_phase-mm-g%`
- `%unt_-phase-mm%`
- `%unt_-phase-md%`

> [!NOTE]
> There is no `%unt_-phase-mm-g%` placeholder. To render reverse-direction gradient transitions, insert the inline token **`#-phase-mm-g#`** directly into the text.

> [!IMPORTANT]
> The speed of text phase transitions is directly linked to the global **`taskInterval`** setting. Lowering the interval speeds up the color cycle but increases CPU utilization. For details, refer to the [Performance Tuning Guide](../performance.md).

---

## Animation Intervals

Animation update frequencies are controlled by the following parameters:

| Parameter | Scope | Description |
| :--- | :--- | :--- |
| **`behavior.taskInterval`** | Global | Defines the refresh frequency for placeholders and text phase placeholders. |
| **`behavior.displayAnimationInterval`** | Global | Defines the tick interval between entity pose updates for all groups lacking a specific interval override. Set to `0` to match `taskInterval`. |
| **`animationInterval`** | Local | Set within a display group to override the global update frequency for that specific group. |

- **Higher Values**: Decreases packet payload frequency, reducing server CPU overhead and
  network traffic.
- **Lower Values**: Yields smoother physical animations at the cost of higher server payload
  tracking.

---

## Display Group Animations (`animation:`)

Display group animations modify the visual translation or scale of an entity. The parent `animation:` block contains several general configuration fields:

| Field | Default | Description |
| :--- | :--- | :--- |
| **`enabled`** | `true` | When set to `false`, the animation is disabled and does not consume resources. |
| **`speed`** | `1.0` | Motion speed multiplier. |
| **`cullBeyondBlocks`** | `0` | Radius (in blocks) within which at least one viewer must be present to update the animation. Set to `0` to disable culling. |
| **`customProperties`** | `{}` | Key-value pairs for third-party developer integrations. |

> [!TIP]
> **Animation Culling:** To optimize performance, always configure **`cullBeyondBlocks`** on complex animations. If no players are within the specified radius, the server will stop sending position update packets for that entity.

---

## Animation Types

### 1. `rotate`
Rotates the display group entity around the player.

| Option | Default | Description |
| :--- | :--- | :--- |
| **`axis`** | `"Y"` | Rotation axis. `Y` creates a flat spin; `X` or `Z` tilts the alignment plane; `XYZ` tumbles on all axes. |
| **`degreesPerSecond`** | `90` | Degrees of rotation per second at a speed of `1.0`. |

### 2. `bob`
Causes the display group to slide vertically in a smooth sine wave.

| Option | Default | Description |
| :--- | :--- | :--- |
| **`amplitude`** | `0.06` | Maximum height displacement (in blocks) from the origin. |
| **`bobsPerSecond`** | `1.0` | Full up-and-down cycles completed per second at a speed of `1.0`. |

### 3. `dvd_bounce`
Translates the display group along a rectangular 2D plane (horizontal screensaver style bounce).

| Option | Default | Description |
| :--- | :--- | :--- |
| **`halfWidth`** | `0.12` | Half-width of the horizontal bounce bounds (in blocks, local X). |
| **`halfDepth`** | `0.1` | Half-depth of the horizontal bounce bounds (in blocks, local Z). |
| **`pace`** | `0.35` | Travel speed of the entity (blocks per second at a speed of `1.0`). |

### 4. `pulse_scale`
Cycles the scale of the display group in a breathing rhythm.

| Option | Default | Description |
| :--- | :--- | :--- |
| **`minMultiplier`** | `0.92` | Minimum scale factor relative to the display group's configured scale. |
| **`maxMultiplier`** | `1.08` | Maximum scale factor relative to the display group's configured scale. |
| **`pulsesPerSecond`** | `1.0` | Full breathing cycles completed per second at a speed of `1.0`. |

### 5. `wiggle`
Tilts the display group back and forth along its rotation axis.

| Option | Default | Description |
| :--- | :--- | :--- |
| **`amplitudeDegrees`** | `10` | Peak rotation deflection (in degrees). |
| **`wigglesPerSecond`** | `2.0` | Full back-and-forth cycles completed per second at a speed of `1.0`. |

### 6. `orbit`
Translates the display group in a circular orbit around the central axis.

| Option | Default | Description |
| :--- | :--- | :--- |
| **`radius`** | `0.1` | Distance radius of the orbit (in blocks, local XZ plane). |
| **`rotationsPerSecond`** | `0.5` | Full orbital rotations completed per second at a speed of `1.0`. |

### 7. `custom`
Hooks into custom animation loops registered programmatically by developers.

| Option | Default | Description |
| :--- | :--- | :--- |
| **`id`** | `""` | The unique ID of the handler registered via `UNTPaperAPI` or `UNTAPI`. |

#### Custom Animation YAML Syntax
```yaml
animation:
  type: custom
  id: my_plugin_animation
  speed: 1.0
```

---

## Configuration Examples

### Example: Rotating Item Display
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

### Example: Pulsing Text Display
```yaml
displayGroups:
  - lines:
      - text: '<gold>VIP</gold>'
    scale: 1.0
    yOffset: 0.0
    animation:
      type: pulse_scale
      minMultiplier: 0.9
      maxMultiplier: 1.1
      pulsesPerSecond: 0.8
      speed: 1.0
      cullBeyondBlocks: 24
```

---

## See Also

* [Display Groups Guide](display-groups.md)
* [Performance Tuning Guide](../performance.md)
