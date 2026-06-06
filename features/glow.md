# Display Group Glow

**UnlimitedNameTags v2** supports colored outline glow on every display row — text, items, and
blocks. Glow can be configured in `settings.yml`, applied at runtime via the API, or toggled
in-game with `/unt glow`.

---

## Global Presets (`glowAnimations`)

Reusable glow definitions live at the root of `settings.yml` under **`glowAnimations`**. Reference
them from any display group with `glow.type: reference` and `ref: <id>`.

The migrator adds three built-in presets when upgrading to **`configVersion: 6`**. On plugin
enable, **`default_gold_pulse`** is registered as the custom handler behind the **`gold_pulse`**
preset (you do not need to register it yourself).

```yaml
configVersion: 6

glowAnimations:
  rainbow:
    type: rainbow
    speed: 1.0
  gradient:
    type: gradient
    colors:
      - '#FF5555'
      - '#55FF55'
      - '#5555FF'
    refreshInterval: 10
    speed: 1.0
  gold_pulse:
    type: custom
    id: default_gold_pulse
    speed: 1.0
```

You can add your own entries and reference them from display groups or from `/unt glow animation`.

---

## Per-Row Glow (`displayGroups`)

Each display group accepts an optional **`glow`** block and optional **`glowInterval`** (tick cadence
for animated glow; defaults to `behavior.displayAnimationInterval`).

### Glow Types

| `type` | Description | Key fields |
| :--- | :--- | :--- |
| **`fixed`** | Static outline color | `color` — hex (`#RRGGBB`) or RGB (`255,0,0`) |
| **`reference`** | Reuses a `glowAnimations` preset (or API-registered preset) | `ref`, optional `speed` multiplier |
| **`rainbow`** | Cycles hue over time | `speed` |
| **`gradient`** | Steps through a color list | `colors` (≥2), `refreshInterval`, `speed` |
| **`custom`** | Delegates to a plugin handler | `id` — must match `registerNametagCustomGlowHandler` |

Common fields on all types:

| Field | Default | Description |
| :--- | :--- | :--- |
| **`enabled`** | `true` | Set to `false` to disable glow for the row. |
| **`speed`** | `1.0` | Tempo multiplier for animated types. `0` disables animation. |
| **`customProperties`** | `{}` | Optional string map readable by custom glow handlers. |

### Example: VIP row with preset rainbow glow

```yaml
nameTags:
  vip:
    permission: group.vip
    displayGroups:
      - lines:
          - text: '<gold>%player_name%</gold>'
        scale: 1.1
        glow:
          type: reference
          ref: rainbow
          speed: 1.2
        glowInterval: 2
```

### Example: Fixed staff color on an item row

```yaml
- displayType: ITEM
  itemMaterial: NETHER_STAR
  scale: 0.6
  glow:
    type: fixed
    color: '#55ffff'
```

---

## In-Game Commands (`/unt glow`)

| Command | Description |
| :--- | :--- |
| **`/unt glow fixed <player> <group> <color>`** | Sets a fixed glow color on one row (persisted). |
| **`/unt glow animation <id> [player] [group]`** | Applies a named preset to all rows or one row. |
| **`/unt glow rate <rate> <id> [player] [group]`** | Same as animation with a speed multiplier. |
| **`/unt glow rainbow <player> <group> [speed]`** | Rainbow glow on one row. |
| **`/unt glow gradient <player> <group> <colors...> [interval]`** | Gradient glow (space-separated colors). |
| **`/unt glow clear <player> [group]`** | Clears API/command glow overrides. |
| **`/unt glow get [player]`** | Lists active per-player glow overrides. |

Permissions: **`unt.glow`** (self), **`unt.glow.others`** (modify other players).

---

## Developer API

Programmatic glow overrides, preset registration, and custom handlers are documented in the
[Developer API Guide](../api.md#display-group-glow-api).

---

## Performance Notes

- Animated glow (rainbow, gradient, custom) ticks on the same schedule as display animations.
  Use **`glowInterval`** on busy rows to reduce update frequency.
- Glow is applied client-side via display entity metadata; the server only sends color updates on
  the configured interval.
