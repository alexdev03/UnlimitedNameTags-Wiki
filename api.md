# Developer API

UnlimitedNameTags exposes a Java API for other plugins to control nametags at runtime — override text, inject items/blocks, register custom animations, integrate vanish systems, and more.

---

## Adding the Dependency

The API artifact is published via JitPack. Add it as a **compile-only** dependency (do not shade it — the plugin jar must already be present on the server).

**Gradle (Kotlin DSL):**
```kotlin
repositories {
    maven("https://jitpack.io")
}

dependencies {
    compileOnly("com.github.alexdev03.UnlimitedNametags:api:2.0.0")
}
```

**Maven:**
```xml
<repositories>
    <repository>
        <id>jitpack.io</id>
        <url>https://jitpack.io</url>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>com.github.alexdev03.UnlimitedNametags</groupId>
        <artifactId>api</artifactId>
        <version>2.0.0</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

Add `UnlimitedNameTags` to your `plugin.yml` so Bukkit loads it before your plugin:

```yaml
# hard dependency
depend: [UnlimitedNameTags]

# or soft dependency (handle absence gracefully)
softdepend: [UnlimitedNameTags]
```

---

## Getting the API Instance

```java
UNTAPI api = UNTAPI.getInstance();
```

> **Note:** `getInstance()` throws `IllegalStateException` if called before your plugin's `onEnable`, or if UnlimitedNameTags failed to load. For soft-depends, guard with `Bukkit.getPluginManager().isPluginEnabled("UnlimitedNameTags")` first.

---

## Core Types

| Type | Purpose |
|------|---------|
| `UNTAPI` | Entry point — all operations go through this class |
| `UnlimitedNameTagsPlugin` | Plugin interface; holds the custom animation registry |
| `Settings.NameTag` | Immutable record: permission + list of `DisplayGroup`s |
| `Settings.DisplayGroup` | One stacked row (type, lines, background, scale, offset, conditions, animation, billboard) |
| `Settings.NametagLine` | One line within a TEXT group: `text` + optional `when` condition |
| `Settings.Background` | Background panel settings (color, opacity, shadowed, seeThrough) |
| `DisplayAnimation` | Base for the 7 built-in animation types |
| `UntNametagDisplay` | Live display entity interface |
| `NametagCustomAnimationHandler` | Functional interface for custom animations |
| `VanishIntegration` | Interface to implement for custom vanish plugins |
| `HatHook` | Interface to implement for custom hat height offsets |

---

## Nametag Overrides

Every player has a **config nametag** (resolved from `settings.yml`) and optionally an **override** set by API. The override wins over the config. The override is not persisted across restarts.

| Method | Description |
|--------|-------------|
| `setNametagOverride(player, nameTag)` | Replace the player's nametag with a full override |
| `removeNametagOverride(player)` | Revert to the config nametag |
| `hasNametagOverride(player)` | `true` if an override is active |
| `getNametagOverride(player)` | `Optional<Settings.NameTag>` — the override, or empty |
| `getEffectiveNametag(player)` | Override if present, otherwise config |
| `getConfigNametag(player)` | Config nametag only (ignores any override) |
| `modifyNametagProperty(player, modifier)` | Clone effective nametag, apply function, save as override |

**Example — append an extra row to a player's nametag:**
```java
UNTAPI api = UNTAPI.getInstance();

api.modifyNametagProperty(player, current -> {
    List<Settings.DisplayGroup> groups = new ArrayList<>(current.displayGroups());
    groups.add(new Settings.DisplayGroup(
        List.of(new Settings.NametagLine("<red>Staff</red>", null)),
        null,       // background
        1.0f,       // scale
        0.0f,       // yOffset
        null,       // when condition
        false,      // relationalConditions
        Settings.DisplayType.TEXT,
        null, null, null,  // itemMaterial, blockMaterial, itemDisplayMode
        null,       // animation
        null,       // animationInterval
        null        // billboard override
    ));
    return current.withDisplayGroups(groups);
});
```

---

## Common Property Shortcuts

These methods clone the effective nametag, apply the change, and save it as an override in one call.

| Method | Description |
|--------|-------------|
| `setNametagScale(player, scale)` | Set scale on all display groups |
| `setNametagBackground(player, background)` | Set background on all display groups |
| `setNametagDisplayGroups(player, list)` | Replace the full list of display groups |
| `setNametagShadowed(player, shadowed)` | Set shadowed flag on all display groups |
| `setNametagSeeThrough(player, seeThrough)` | Set seeThrough flag on all display groups |
| `setNametagBillboard(player, billboard)` | Set billboard mode on all active display entities |

---

## Forced Nametags

A forced nametag overrides the rendered text at the packet level, bypassing config and overrides entirely. Applies only to the **first** display entity (to avoid duplicating text on stacked rows).

| Method | Scope |
|--------|-------|
| `setForcedNametag(player, component)` | All viewers see this component |
| `setForcedNametag(player, viewer, component)` | Only `viewer` sees this component |
| `clearForcedNametag(player)` | Remove forced text for all viewers |
| `clearForcedNametag(player, viewer)` | Remove forced text for one viewer |

**Example:**
```java
// Show a hidden name to all players (e.g., spectator mode)
api.setForcedNametag(target, Component.text("???", NamedTextColor.GRAY));

// Show real name only to the admin who is spectating
api.setForcedNametag(target, admin, Component.text(target.getName(), NamedTextColor.WHITE));

// Clear when done
api.clearForcedNametag(target);
```

---

## Animations

### Per-Group Animation (Programmatic)

```java
// Set a Y-axis rotation on the first display group (index 0)
api.setNametagDisplayGroupAnimation(player, 0, new DisplayAnimation.RotateDisplayAnimation(...));

// Remove animation from group 0
api.clearNametagDisplayGroupAnimation(player, 0);
```

`displayGroupIndex` is 0-based. Throws `IllegalArgumentException` if the index is out of range.

### Custom Animations

Implement `NametagCustomAnimationHandler` and register it. In `settings.yml`, use `animation.type: custom` with the matching `id:`.

```java
api.registerNametagCustomAnimation("my_pulse", (target, animation, elapsedMs) -> {
    float scale = 1.0f + 0.1f * (float) Math.sin(elapsedMs / 500.0 * Math.PI * 2);
    target.setAnimationScale(scale);
});
```

`NametagCustomAnimationHandler` receives:
- `target` — `NametagAnimationTarget`: writable pose interface
- `animation` — the `DisplayAnimation.CustomDisplayAnimation` instance (access `customProperties` here)
- `elapsedMs` — time in milliseconds since the animation started

```java
api.unregisterNametagCustomAnimation("my_pulse");
api.getNametagCustomAnimationHandler("my_pulse"); // returns null if not registered
```

---

## Refresh & Visibility

| Method | Description |
|--------|-------------|
| `forceRefresh(player)` | Immediately refresh placeholder text and update all packets |
| `forceRefresh(player, force)` | Same with explicit force flag |
| `hideNametag(player)` | Remove nametag display entities from all viewers |
| `showNametag(player)` | Re-show nametag to all currently tracked players |
| `getPacketDisplayText(player)` | `Collection<? extends UntNametagDisplay>` — direct access to live display entities |

> **Note:** Prefer `forceRefresh` over direct display manipulation for most use cases.

---

## Sneak System (Shift Opacity)

The shift system applies reduced opacity when a player is sneaking. You can block it per-player (useful in arenas where you do not want opacity changes).

```java
api.setShiftSystemBlocked(player, true);   // disable sneak opacity for this player
api.isShiftSystemBlocked(player);          // check current state
api.setShiftSystemBlocked(player, false);  // re-enable
```

---

## Vanish Integration

Implement `VanishIntegration` to connect your own vanish plugin. The integration controls which players' nametags are visible to which viewers.

```java
api.setVanishIntegration(new VanishIntegration() {
    @Override
    public boolean canSee(Player viewer, Player other) {
        return myVanishPlugin.canSee(viewer, other);
    }

    @Override
    public boolean isVanished(Player player) {
        return myVanishPlugin.isVanished(player);
    }
});
```

Direct vanish state helpers (useful for tab/scoreboard sync):

```java
api.vanishPlayer(player);    // hide from tab + scoreboard
api.unVanishPlayer(player);  // show in tab + scoreboard
```

---

## Hat Hooks

Implement `HatHook` to provide custom nametag height offsets for cosmetic items that are not auto-detected.

```java
HatHook hook = player -> {
    MyCosmetic hat = myPlugin.getActiveHat(player);
    return hat != null ? hat.getHeightOffset() : 0.0;
};

api.addHatHook(hook);
// later:
api.removeHatHook(hook);
```

Return `0.0` (or any non-positive value) if the hook does not apply to the given player. For config-based height rules, prefer [advanced.yml](features/advanced-yml.md) instead.

---

## Direct Display Access (`UntNametagDisplay`)

`getPacketDisplayText(player)` returns the live display entity collection. Use this only for advanced scenarios (inspecting packet state, per-viewer packet manipulation). For normal control, prefer the high-level methods above.
