# Show While Looking & Through-Wall Dimming

These features provide advanced visibility control for player name tags based on a viewer's line of
sight and crosshair target direction.

---

## 🎯 Show While Looking

When `showWhileLooking` is enabled, player name tags are only rendered to a viewer if they aim their
crosshair directly at the target player. The name tag despawns the moment the viewer looks away. All
standard visibility filters (permissions, vanish integrations, world list constraints) still apply.

![Visual demonstration of the name tag appearing only when the player's crosshair is aimed directly at the target player](../assets/show-while-looking.gif)

### Enabling Show While Looking
```yaml
visibility:
  showWhileLooking: true
```

---

## 🔍 How Visibility Checks Work

The plugin performs synchronous raycast calculations on the main server thread at intervals defined by
`taskInterval` for every active viewer/player pair within tracking distance.

> [!WARNING]
> Because raycasting is performed on the primary server thread, enabling `showWhileLooking` on
> high-population servers increases CPU load. If server TPS decreases after enabling this option,
> refer to the [Performance Tuning Guide](../performance.md) — raising the global `taskInterval`
> value is the primary method to mitigate load.

---

## Companion Feature: Through-Wall Occlusion (`throughWallMode`)

The `throughWallMode` setting provides advanced visibility check options when the line of sight
between the viewer and target is blocked by solid blocks. 

- **`SEE_THROUGH`** (Default): Vanilla behavior; name tags remain fully visible through walls.
- **`OBSCURED`**: Dims the name tag to a specified opacity when behind walls.
- **`HIDE`**: Completely hides the name tag display when behind walls (acts as a built-in anti-wallhack).

### Interaction Matrix

| Configuration State | Rendering Behavior |
| :--- | :--- |
| **`showWhileLooking: true` + `throughWallMode: SEE_THROUGH`** | The tag is only visible when the viewer aims their crosshair directly at the target player. |
| **`showWhileLooking: false` + `throughWallMode: OBSCURED`** | The tag remains visible continuously, but dims in opacity when line of sight is blocked by solid geometry. |
| **`showWhileLooking: false` + `throughWallMode: HIDE`** | The tag remains visible when there is clear line of sight, but is completely despawned when behind walls. |
| **Both features enabled (`showWhileLooking: true` + `OBSCURED`)** | The tag only renders when targeted, and will render with reduced opacity if the sightline passes through solid blocks. |
| **Both features enabled (`showWhileLooking: true` + `HIDE`)** | The tag only renders when targeted and if there is a clear, unobstructed line of sight. |

---

## Through-Wall Settings

```yaml
visibility:
  throughWallMode: OBSCURED        # Options: SEE_THROUGH, OBSCURED, HIDE
  throughWallSettings:
    opacity: 55                    # Alpha value (0–127) when sightline is blocked
    maxDistance: 48.0              # Maximum block radius for evaluation
    checkInterval: 5               # Check frequency in ticks
```

| Option | Default | Description |
| :--- | :--- | :--- |
| **`throughWallMode`** | `SEE_THROUGH` | Direct line-of-sight mode. Set to `OBSCURED` to dim tags, or `HIDE` to completely remove them behind solid geometry. |
| **`throughWallSettings.opacity`** | `55` | The opacity value (from `0` to `127`) applied to the display when the sightline is obstructed in `OBSCURED` mode. |
| **`throughWallSettings.maxDistance`** | `48.0` | The maximum distance (in blocks) for the check. Beyond this distance, through-wall calculations are skipped. |
| **`throughWallSettings.checkInterval`** | `5` | Ticks between raycast updates. This runs synchronously on the main thread. |

> [!WARNING]
> Both `showWhileLooking` and through-wall checks execute raycast calculations on the
> **primary server thread**. Through-wall occlusion settings are applied to `TEXT` display groups
> only. On high-population servers, it is highly recommended to increase
> `throughWallSettings.checkInterval` to `10` or `20` ticks to prevent performance degradation.

---

## Performance Best Practices

- **Adjust Intervals**: Raise `behavior.taskInterval` (e.g., to `40` ticks) to reduce the
  computation frequency of `showWhileLooking`.
- **Settle Check Timings**: Set `throughWallSettings.checkInterval` between `10` and `20` ticks.
  This change is virtually unnoticeable to players but significantly reduces CPU overhead.
- **Disable Unused Toggles**: Keep `showWhileLooking` set to `false` and `throughWallMode` set to
  `SEE_THROUGH` if they are not actively utilized; they consume no resources.

For detailed performance steps, refer to the [Performance Tuning Guide](../performance.md).

---

## Common Use Cases

- **Minigames & Competitive Maps**: Displays team names only when targeted, minimizing UI clutter
  on screen.
- **Roleplay & Immersive Servers**: Hides name tags at long distances to protect immersion,
  revealing them only when focusing on a player.
- **Tactical & PvP Servers**: Through-wall dimming prevents players from tracking opponents through
  solid walls using floating name tags, without completely disabling name tag visibility.
