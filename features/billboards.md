# Billboard

Nametag rows can **face the player** in different ways — that facing mode is what we call the **billboard**. Set a default for all rows, and optionally override **per row** with **`billboard`**.

---

## Modes

| Value | Typical behaviour |
|-------|-------------------|
| **CENTER** | Standard billboard-style facing; balanced toward the viewer (closest to vanilla feel). |
| **HORIZONTAL** | Stronger horizontal (yaw) component; less vertical roll in many setups. |
| **VERTICAL** | Stronger pitch (vertical tilt) component. |
| **FIXED** | Fixed orientation relative to the entity (does not face the camera like CENTER). |

The GIFs below are illustrative; test in-game for your pack and scale.

---

## CENTER (default)

```yaml
defaultBillboard: CENTER
```

![CENTER](../assets/billboard-center.gif)

---

## HORIZONTAL

```yaml
defaultBillboard: HORIZONTAL
```

![HORIZONTAL](../assets/billboard-horizontal.gif)

---

## VERTICAL

```yaml
defaultBillboard: VERTICAL
```

![VERTICAL](../assets/billboard-vertical.gif)

---

## FIXED

```yaml
defaultBillboard: FIXED
```

![FIXED](../assets/billboard-fixed.gif)

---

## Command vs per-row

- Global default: `defaultBillboard` in `settings.yml`.
- Per row: `billboard` on the `displayGroup`.
- In-game: **`/unt billboard <CENTER|HORIZONTAL|VERTICAL|FIXED>`** (permission `unt.billboard`) — updates the default per command implementation.

See also [Configuration](../configuration.md).
