# Animations (API)

## Programmatic Overrides

Apply or clear animations on specific display group rows by index:

```java
UNTPaperAPI api = UNTPaperAPI.getInstance();

api.setNametagDisplayGroupAnimation(player, 0, new DisplayAnimation.RotateDisplayAnimation(...));
api.clearNametagDisplayGroupAnimation(player, 0);
api.setNametagDisplayGroupAnimation(player, 0, animation, true); // persist across relog
```

> [!NOTE]
> The display group index is 0-based. An out-of-range index throws `IllegalArgumentException`.

---

## Custom Animations

Register a pose modifier with **`NametagCustomAnimationHandler`**. In YAML, use
`animation.type: custom` with a matching `id`.

```java
api.registerNametagCustomAnimation("my_pulse", (target, animation, scaledElapsedSeconds) -> {
    float scale = 1.0f + 0.1f * (float) Math.sin(scaledElapsedSeconds * Math.PI * 2);
    target.setAnimationScale(scale);
});
```

Handler parameters:

- **`target`** (`NametagAnimationTarget`): scale and positional offsets for the row
- **`animation`** (`DisplayAnimation.CustomDisplayAnimation`): custom config properties
- **`scaledElapsedSeconds`**: elapsed wall time since start, multiplied by row speed

```java
api.unregisterNametagCustomAnimation("my_pulse");
api.getNametagCustomAnimationHandler("my_pulse"); // null if unregistered
```

For YAML animation types (bob, rotate, orbit, …), see the [Animations feature guide](../features/animations.md).
