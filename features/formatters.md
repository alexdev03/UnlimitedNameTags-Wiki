# Text Formatters

The global **`behavior.format`** setting in `settings.yml` chooses how display text is parsed before
it is sent to clients.

```yaml
behavior:
  format: MINIMESSAGE
```

Change at runtime with **`/unt formatter <type>`** (requires **`unt.formatter`**).

---

## Supported Formatters

| Value | Description |
| :--- | :--- |
| **`MINIMESSAGE`** (default) | [MiniMessage](https://docs.advntr.dev/minimessage/) — hex colors, gradients, `<bold>`, hover/click events. |
| **`LEGACY`** | Classic **`&`** color codes and formatting (`&a`, `&l`, …). |
| **`UNIVERSAL`** | Accepts both MiniMessage tags and legacy **`&`** codes in the same string. |
| **`MINEDOWN`** | [MineDown](https://github.com/Phoenix616/MineDown) formatting syntax. |

---

## Tips

- Prefer **MiniMessage** for new configs — it supports modern hex and gradient syntax used throughout
  this wiki.
- [Phase placeholders](animations.md#text-phase-placeholders) in animations depend on the active
  formatter (`#phase-mm#` vs `#phase-md#`).
- Enabling **`performance.componentCaching`** can delay text updates when placeholders change; see
  the [Performance Guide](../performance.md).
