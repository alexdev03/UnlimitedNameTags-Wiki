# Sneak Opacity & Own Nametag

Two global visibility options in `settings.yml` that are separate from
[Show While Looking](show-while-looking.md) and [through-wall modes](show-while-looking.md#companion-feature-through-wall-occlusion-throughwallmode).

---

## Sneak Opacity

When a player sneaks, name tag text can render at reduced opacity (vanilla-like behaviour).

```yaml
visibility:
  sneakOpacity: 70
```

| Value | Effect |
| :--- | :--- |
| **`0`–`127`** | Opacity while sneaking (`127` = fully opaque, lower = more transparent). |
| **`-1`** | Disable sneak dimming — tags stay fully opaque while crouching. |

Addon plugins can block sneak dimming per player via the API — see
[Visibility (API)](../api/visibility.md#sneak-opacity).

---

## Own Nametag (See Yourself)

By default, players do **not** see their own custom name tag above their head (same as vanilla).

```yaml
visibility:
  showCurrentNameTag: false
  allowPerPlayerShowOwnWhenGlobalDisabled: false
```

| Setting | Effect |
| :--- | :--- |
| **`showCurrentNameTag: true`** | Players with **`unt.showownnametag`** see their own stacked tag (Lunar-style). |
| **`allowPerPlayerShowOwnWhenGlobalDisabled: true`** | Lets players toggle own-tag visibility with **`/unt preferences showown`** even when the global setting is off. |

See [Player Preferences](player-preferences.md) for per-player toggles.
