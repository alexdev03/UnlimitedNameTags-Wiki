# Show While Looking

---

## What It Does

When **`showWhileLooking: true`** is set, a viewer only sees another player's nametag while they are **aiming their crosshair at that player**. The moment they look away, the tag disappears. All other rules still apply on top (permissions, vanish, world filters, etc.).

![Example](../assets/show-while-looking.gif)

---

## Enabling It

```yaml
visibility:
  showWhileLooking: true
```

---

## How the Check Works

The plugin performs a raytrace-style check on each **`taskInterval`** tick for every viewer/player pair within view distance. This check runs on the main thread.

On large servers with many players in the same area, this adds meaningful work per tick. If TPS dips after enabling this feature, see [Performance](../performance.md) — raising `taskInterval` is the primary lever.

---

## Companion Feature — Through-Wall Dimming

**`obscuredNametagThroughWalls`** is a related but distinct option. Instead of hiding the tag when not looking, it **dims** the tag when there is no clear line of sight (e.g. the player is behind a wall). The two options can be used independently or together.

| Config | Effect |
|--------|--------|
| `showWhileLooking: true` only | Tag only visible when viewer aims directly at the player |
| `obscuredNametagThroughWalls: true` only | Tag always visible, but faded when no clear line of sight |
| Both enabled | Tag visible while looking; faded if the sightline passes through a block |

---

## Through-Wall Dimming Settings

```yaml
visibility:
  obscuredNametagThroughWalls: true
  obscuredNametagOpacity: 55       # 0–127; opacity when no clear sightline
  obscuredNametagMaxDistance: 48.0 # blocks; players beyond this are not checked
  obscuredNametagCheckInterval: 5  # ticks between sightline rechecks
```

| Option | Default | Description |
|--------|---------|-------------|
| **`obscuredNametagOpacity`** | `55` | Opacity (0–127) when the sightline is blocked. |
| **`obscuredNametagMaxDistance`** | `48.0` | Max distance (blocks) for the check. Players beyond this are shown at full opacity. |
| **`obscuredNametagCheckInterval`** | `5` | Ticks between sightline rechecks. Runs on the main thread — raise to `10`–`20` on busy servers. The visual delay before a tag dims equals this interval. |

> **Note:** Both `showWhileLooking` and through-wall dimming run sightline checks on the main thread. Through-wall dimming applies to TEXT rows only.

---

## Performance Guidance

- `showWhileLooking` adds a check per-viewer per-nearby-player every `taskInterval` ticks. Raise `taskInterval` (e.g. from `20` to `40`) to halve the cost.
- `obscuredNametagCheckInterval` controls how often the through-wall raycast runs. Setting it to `10`–`20` is usually imperceptible to players.
- Disable either feature when not needed — they add no cost when off.

See [Performance](../performance.md) for a full tuning guide.

---

## Use Cases

- **Minigames** — show teammate names only when directly aimed at, reducing visual clutter.
- **Immersive / roleplay servers** — floating names at range break atmosphere; hide them until the viewer focuses.
- **PvP / dungeon servers** — through-wall dimming prevents nametag-based wall-hacking without fully removing tags.
