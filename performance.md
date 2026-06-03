# Performance Optimization

On high-population servers or configurations utilizing complex placeholders, optimizing the `settings.yml` file is essential to maintain high server performance. Name tags periodically query PlaceholderAPI and refresh display packets. Small configuration adjustments can significantly reduce CPU overhead.

---

## Configuration Categories

Global optimization switches are grouped under three main sections in `settings.yml`:

* **`behavior`**: Defines task update intervals, client-side render distances, formatting engines, and camera alignment modes.
* **`visibility`**: Configures sneak transparency, raycast visibility checks, through-wall dimming, and self-view toggles.
* **`performance`**: Manages caching mechanisms, custom placeholder refresh rates, and relational placeholder processing.

If you are using an older configuration format, these options may appear at the root level of the file. The plugin will automatically restructure them into their designated categories upon startup.

```yaml
configVersion: 5

behavior:
  taskInterval: 20
  displayAnimationInterval: 1
  yOffset: 0.3
  viewDistance: 60
  compactDisplayGroupStack: false
  displayGroupLineHeightBlocks: 0.25
  disableDefaultNameTag: true
  forceDisableDefaultNameTag: false
  defaultBillboard: CENTER
  format: MINIMESSAGE
  removeEmptyLines: true

visibility:
  sneakOpacity: 70
  showWhileLooking: false
  showCurrentNameTag: false
  allowPerPlayerShowOwnWhenGlobalDisabled: false
  throughWallMode: SEE_THROUGH
  throughWallSettings:
    opacity: 55
    maxDistance: 48.0
    checkInterval: 5

performance:
  componentCaching: false
  placeholderCacheTime: 1
  enableRelationalPlaceholders: false
  placeholderUpdateRates: {}

nameTags:
  # your presets...
```

For a comprehensive explanation of every configuration setting, refer to the [Configuration Guide](configuration.md).

---

## Key Performance Factors

| System | Impact Description |
| :--- | :--- |
| **PlaceholderAPI** | Querying placeholders is a primary driver of CPU usage. Complex or poorly optimized expansions aggregate quickly when queried per player, per line, at frequent intervals. |
| **Refresh Intervals** | Defined by `behavior.taskInterval`. Lower values yield responsive tags but increase server load. |
| **Animations** | Updates display entity positions on a timer. These can be culled when players are distant or out of sight. |
| **Relational Placeholders** | Requires the plugin to evaluate placeholders on a per-viewer, per-target basis, multiplying computation costs. |
| **Text Formatting** | The `UNIVERSAL` formatter is rich but resource-heavy. `MINIMESSAGE` represents the optimal performance-to-feature ratio. |
| **Visibility Checks** | Line-of-sight checks ("Show While Looking" and "Through Walls") require active raycasting calculations. |
| **View Distance** | Defined by `behavior.viewDistance`. Decreasing client-side draw distances reduces packets sent to distant players. |

---

## `behavior.taskInterval`

The refresh interval in seconds is calculated as `taskInterval / 20`.

* **Higher Values** (e.g., `40`): Reduces CPU load. Economy balances, leaderboard positions, and placeholder updates will feel slower.
* **Lower Values** (e.g., `10`): Provides highly responsive tags but increases CPU utilization.
* **Default Recommended:** `20` (once per second) is optimal for most production servers.

---

## `behavior.displayAnimationInterval` & `animationInterval`

These settings control the refresh rate of moving display elements.
* **Global Rate:** `behavior.displayAnimationInterval` defines the global tick interval.
* **Local Override:** Setting an `animationInterval` within a specific display group overrides the global rate.
* **Disabled/Match:** Setting this to `0` forces animations to sync with the main `taskInterval`.

> [!NOTE]
> Rainbow text formatting tags (such as `#phase-mm#`) follow the main placeholder refresh rate (`taskInterval`) rather than the animation intervals.

For details, refer to the [Animations Guide](features/animations.md).

---

## `behavior.format`

Select the least complex formatter that meets your styling needs:

| Formatter | Recommended Use |
| :--- | :--- |
| **`MINIMESSAGE`** | **Highly Recommended**. Modern, highly optimized, and uses standard `<color>` syntax. |
| **`MINEDOWN`** | Use only if your configurations are specifically written in MineDown syntax. |
| **`LEGACY`** | Classic character-based styling (`&` codes). |
| **`UNIVERSAL`** | Parses both legacy and modern formats. **Consumes the most CPU**. |

---

## `behavior.viewDistance`

Defines client-side render distance. This value is scaled internally before transmission. Lowering this value ensures that client-side rendering stops at shorter distances, reducing packet overhead.

---

## Compact Stacking Options

* **`behavior.compactDisplayGroupStack`**: When set to `true`, hidden or empty rows do not reserve vertical space, ensuring name tags pack tightly.
* **`behavior.displayGroupLineHeightBlocks`** (Default: `0.25`): The estimated vertical size (in blocks) of a text line, used by the compact stack calculations. Adjust this if you use custom text scales or non-standard fonts.
* **`behavior.removeEmptyLines`**: When set to `true`, empty text lines (resulting from empty placeholders) are stripped from the packet, reducing processing costs.

---

## Raycast Visibility Features

* **`visibility.showWhileLooking`**: Only displays name tags to a viewer looking directly at the owner. (See [Show While Looking Guide](features/show-while-looking.md)).
* **`visibility.throughWallMode`**: Direct line-of-sight visibility mode (`SEE_THROUGH`, `OBSCURED`, `HIDE`). (See [Show While Looking Guide](features/show-while-looking.md)).
* **`visibility.throughWallSettings.checkInterval`** (Default: `5`): Ticks between line-of-sight checks.

> [!WARNING]
> Line-of-sight and through-wall checks perform raycasts on the **primary server thread**. If you configure `throughWallMode` to `OBSCURED` or `HIDE` on a high-population server, it is highly recommended to increase `throughWallSettings.checkInterval` to `10` or `20` ticks to prevent performance degradation.

---

## PlaceholderAPI Optimizations

### `performance.placeholderCacheTime`
Defines the cache duration (in ticks) for individual placeholder results. Increasing this value reduces repetitive queries to external plugins at the cost of slight display delays for dynamic data.

### `performance.componentCaching`
Caches parsed text components. While helpful for static or complex gradient text styles, it may cause display anomalies when used with highly dynamic placeholder data. Keep disabled unless specifically needed for optimization testing.

---

## `performance.placeholderUpdateRates`

Defines custom, longer caching intervals for specific, heavy placeholders (e.g., economy balances, level stats, guild names) that do not require tick-by-tick updates.

```yaml
performance:
  placeholderUpdateRates:
    "%vault_eco_balance%": 100
    "%some_heavy_placeholder%": 80
```

> [!IMPORTANT]
> The plugin will never update a placeholder faster than the global `behavior.taskInterval`. If `taskInterval` is set to `20` ticks, a placeholder update rate of `5` will still wait a minimum of `20` ticks.

---

## `performance.enableRelationalPlaceholders`

Set to `false` by default. Enable only if you use viewer-dependent placeholders (such as `%rel_...%`).

> [!TIP]
> **Relational Performance Optimization:** Name tag rendering for relational placeholders has been heavily optimized using Adventure's `replaceText` API. The plugin parses the formatting (like MiniMessage) **once** per owner and caches it, then performs a fast, lightweight replacement of relational placeholders for each viewer. This drastically reduces CPU overhead compared to older versions where formatting had to be parsed from scratch for every single viewer.

---

## Animation Distance Culling: `cullBeyondBlocks`

You can add `cullBeyondBlocks` within any `animation:` block. If no players are within the specified block radius, the server skips pose updates for that animation. This is highly effective for reducing unnecessary packet updates in unoccupied areas.

---

## Layout Optimization Guidelines

* Keep the number of active `displayGroup` rows to a minimum.
* Use the group-level `when:` field to disable rendering of display groups when they are not relevant (see [Display Groups Guide](features/display-groups.md)).

---

## Busy-Server Checklist

For servers experiencing high CPU usage, verify the following configuration parameters:

1. **`behavior.taskInterval`**: Set to at least `20` ticks.
2. **`behavior.format`**: Set to `MINIMESSAGE`.
3. **`performance.enableRelationalPlaceholders`**: Set to `false` if viewer-dependent placeholders are not in use.
4. **Animations**: Add `cullBeyondBlocks` or increase animation intervals.
5. **Visibility Settings**: Disable `showWhileLooking` and set `throughWallMode` to `SEE_THROUGH` if raycast features are not actively needed.
6. **`behavior.viewDistance`**: Reduce the client render distance to limit packet delivery.
7. **`performance.placeholderUpdateRates`**: Define custom, longer intervals for heavy placeholders.
8. **`visibility.throughWallSettings.checkInterval`**: If through-wall occlusion (`OBSCURED` or `HIDE`) is enabled, increase the interval to `10` or `20` ticks.
9. **`behavior.compactDisplayGroupStack`**: Enable (`true`) if many rows are hidden conditionally, reducing payload overhead.

---

## See Also

* [Configuration Guide](configuration.md)
* [Animations Guide](features/animations.md)
* [Display Groups Guide](features/display-groups.md)
