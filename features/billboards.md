# Billboard

Display entities use Minecraft **billboard constraints** to orient the nametag relative to the viewer. Set a global default (`defaultBillboard`) and optionally a **`billboard`** on each `displayGroup`.

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

![CENTER](https://i.imgur.com/LBlke9Q.gif)

---

## HORIZONTAL

```yaml
defaultBillboard: HORIZONTAL
```

![HORIZONTAL](https://i.imgur.com/Z8lALCR.gif)

---

## VERTICAL

```yaml
defaultBillboard: VERTICAL
```

![VERTICAL](https://i.imgur.com/uNIAC4y.gif)

---

## FIXED

```yaml
defaultBillboard: FIXED
```

![FIXED](https://i.imgur.com/ukpMPq5.gif)

---

## Command vs per-row

- Global default: `defaultBillboard` in `settings.yml`.
- Per row: `billboard` on the `displayGroup`.
- In-game: **`/unt billboard <CENTER|HORIZONTAL|VERTICAL|FIXED>`** (permission `unt.billboard`) — updates the default per command implementation.

See also [Configuration](../configuration.md).
