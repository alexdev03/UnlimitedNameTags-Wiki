# Glow (API)

Glow tints the outline of text, item, and block display rows. Overrides apply per display group
index (0-based). For YAML configuration and `/unt glow` commands, see the
[Glow feature guide](../features/glow.md).

| Method | Description |
| :--- | :--- |
| **`setDisplayGroupGlow(player, groupIndex, glow)`** | Applies a `GlowOverride` to one row. |
| **`setDisplayGroupGlow(player, groupIndex, glow, persist)`** | Persists the glow override across relog when `persist` is `true`. |
| **`setDisplayGroupFixedGlow(player, groupIndex, color)`** | Shortcut for a fixed hex/RGB glow color. |
| **`clearDisplayGroupGlow(player, groupIndex)`** | Removes the API glow override for one row. |
| **`getDisplayGroupGlowOverride(uuid, groupIndex)`** | Returns the active API glow override, if any (`UNTAPI` / UUID). |

Factory helpers live in **`NametagGlowOverrides`**:

```java
UNTPaperAPI api = UNTPaperAPI.getInstance();

api.setDisplayGroupFixedGlow(player, 0, "#ff0000");
api.setDisplayGroupGlow(player, 1, NametagGlowOverrides.rainbow(1.5), true);
api.setDisplayGroupGlow(player, 0, NametagGlowOverrides.reference("gold_pulse"));
```

---

## Presets and Custom Handlers

Available on **`UNTPaperAPI`**. The plugin registers built-in presets on enable (`rainbow`,
`gradient`, `gold_pulse`) and the custom handler **`default_gold_pulse`** used by the
`gold_pulse` preset.

| Method | Description |
| :--- | :--- |
| **`registerNametagGlowAnimation(id, glow)`** | Registers a reusable glow preset (referenceable via `type: reference` in YAML). |
| **`unregisterNametagGlowAnimation(id)`** | Removes an API-registered preset. |
| **`getAllKnownGlowAnimationIds()`** | Union of `settings.yml` `glowAnimations` keys and API presets. |
| **`registerNametagCustomGlowHandler(id, handler)`** | Registers a handler for `GlowOverride` with `type: custom`. |
| **`unregisterNametagCustomGlowHandler(id)`** | Removes a custom glow handler. |

```java
api.registerNametagGlowAnimation("staff_glow",
    NametagGlowOverrides.gradient(List.of("#ff5555", "#ffff55"), 8));

api.registerNametagCustomGlowHandler("my_pulse", ctx -> {
    double wave = 0.5 + 0.5 * Math.sin(ctx.scaledElapsedSeconds() * Math.PI * 2.0);
    int bright = 0xFFAA00;
    int dim = 0x553300;
    int r = (int) (((bright >> 16) & 0xFF) * wave + ((dim >> 16) & 0xFF) * (1.0 - wave));
    int g = (int) (((bright >> 8) & 0xFF) * wave + ((dim >> 8) & 0xFF) * (1.0 - wave));
    int b = (int) ((bright & 0xFF) * wave + (dim & 0xFF) * (1.0 - wave));
    return (r << 16) | (g << 8) | b;
});
```

`NametagCustomGlowContext` fields: **`glow()`**, **`scaledElapsedSeconds()`**,
**`monotonicTick()`**, **`effectiveGlowTickInterval()`**, **`ownerId()`**.

Use **`type: custom`** with matching **`id`** in YAML, or reference presets with
**`type: reference`** and **`ref: staff_glow`**.
