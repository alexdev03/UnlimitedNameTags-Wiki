# Developer API

**UnlimitedNameTags** exposes a comprehensive Java API that allows other plugins to dynamically control player name tags at runtime. You can programmatically override name tag text, inject item/block entities, register custom animations, implement custom vanish integrations, and adjust cosmetic height offsets.

---

## Adding the Dependency

The API is published to **Maven Central** (and can also be installed locally using `gradle publishToMavenLocal`). It must be declared as a **compile-only** dependency; do not shade or bundle it within your plugin artifact, as the **UnlimitedNameTags** jar must be present on the server at runtime.

* **`unlimitednametags-api-paper`**: Recommended for Paper/Bukkit plugins. Provides helper methods utilizing the standard Bukkit `Player` object.
* **`unlimitednametags-api`**: Platform-neutral API utilizing player `UUID` identifiers.

### Gradle (Kotlin DSL)
```kotlin
repositories {
    mavenCentral()
}

dependencies {
    // For Paper/Bukkit development (recommended):
    compileOnly("org.alexdev:unlimitednametags-api-paper:2.0.0")

    // Or, for platform-neutral UUID-only development:
    // compileOnly("org.alexdev:unlimitednametags-api:2.0.0")
}
```

### Maven
```xml
<dependencies>
    <!-- For Paper/Bukkit development (recommended): -->
    <dependency>
        <groupId>org.alexdev</groupId>
        <artifactId>unlimitednametags-api-paper</artifactId>
        <version>2.0.0</version>
        <scope>provided</scope>
    </dependency>

    <!-- Or, for platform-neutral UUID-only development:
    <dependency>
        <groupId>org.alexdev</groupId>
        <artifactId>unlimitednametags-api</artifactId>
        <version>2.0.0</version>
        <scope>provided</scope>
    </dependency>
    -->
</dependencies>
```

### Plugin Configuration (`plugin.yml`)
To ensure the server loads **UnlimitedNameTags** before your plugin, declare the dependency in your `plugin.yml` file:

```yaml
# Hard dependency (required for startup)
depend: [UnlimitedNameTags]

# Or soft dependency (handle absence gracefully in code)
softdepend: [UnlimitedNameTags]
```

---

## Retrieving the API Instance

Acquire the appropriate API singleton instance depending on your platform dependency:

* **Paper/Bukkit API (Recommended):**
  ```java
  UNTPaperAPI api = UNTPaperAPI.getInstance();
  ```

* **Platform-Neutral API (UUID-based):**
  ```java
  UNTAPI api = UNTAPI.getInstance();
  ```

> [!WARNING]
> Calling `getInstance()` before your plugin's `onEnable()` execution or when **UnlimitedNameTags** is disabled will throw an `IllegalStateException`. For soft-dependent setups, always verify plugin status using `Bukkit.getPluginManager().isPluginEnabled("UnlimitedNameTags")` before retrieving the instance.

---

## Core Types Reference

| Type | Description |
| :--- | :--- |
| **`UNTAPI`** | Core platform-neutral entry point. All base API operations are defined here. |
| **`UNTPaperAPI`** | Paper-specific entry point providing `Player` object mapping overloads. |
| **`UnlimitedNameTagsPlugin`** | Internal plugin interface, managing components such as the custom animation registry. |
| **`Settings.NameTag`** | Immutable configuration record representing a permission mapping and its associated `DisplayGroup` list. |
| **`Settings.DisplayGroup`** | Immutable record representing a single display row, including its type, lines, scale, offsets, conditions, animations, and billboard overrides. |
| **`Settings.NametagLine`** | A line configuration containing formatting text and optional visibility check conditions. |
| **`Settings.Background`** | Background plate properties (color, opacity, drop-shadow, and block transparency rendering). |
| **`DisplayAnimation`** | Base class for built-in name tag animations. |
| **`UntNametagDisplay`** | Interface representing a live client-side display entity. |
| **`NametagCustomAnimationHandler`** | Functional interface for creating and registering custom animations. |
| **`VanishIntegration`** | Interface to connect custom or third-party vanish plugin systems. |
| **`HatHook`** | Interface for registering custom cosmetic offset height calculations. |

---

## Name Tag Overrides

Player name tags are resolved from `settings.yml` (the config name tag) unless an active override is registered via the API. Overrides take precedence over config settings, are stored in memory, and do not persist across server restarts.

| Method | Description |
| :--- | :--- |
| **`setNametagOverride(player, nameTag)`** | Registers a complete custom `Settings.NameTag` override for a player. |
| **`removeNametagOverride(player)`** | Clears the active override, restoring the default configuration-based layout. |
| **`hasNametagOverride(player)`** | Returns `true` if the player currently has an active override. |
| **`getNametagOverride(player)`** | Returns an `Optional<Settings.NameTag>` containing the override layout if present. |
| **`getEffectiveNametag(player)`** | Returns the active override layout if present, falling back to the configuration layout. |
| **`getConfigNametag(player)`** | Retrieves the player's default configuration layout, ignoring active overrides. |
| **`modifyNametagProperty(player, modifier)`** | Retrieves the effective layout, applies a mapping function, and registers the result as an override. |

### Example: Appending a Row to an Active Name Tag

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

## Configuration Property Shortcuts

These convenience methods retrieve the player's active layout, apply the specified modification, and register the updated state as an override in a single operation:

| Method | Description |
| :--- | :--- |
| **`setNametagScale(player, scale)`** | Updates the scale factor across all display groups. |
| **`setNametagBackground(player, background)`** | Applies a background plate style across all display groups. |
| **`setNametagDisplayGroups(player, list)`** | Overwrites the entire list of display groups for the player. |
| **`setNametagShadowed(player, shadowed)`** | Toggles drop-shadow rendering across all text lines. |
| **`setNametagSeeThrough(player, seeThrough)`** | Toggles block-transparency rendering across all text lines. |
| **`setNametagBillboard(player, billboard)`** | Updates the camera-alignment billboard mode for all player display groups. |

---

## Forced Packet-Level Name Tags

Forced name tags overwrite the text components at the packet level, bypassing configurations and registered layout overrides. A forced name tag only applies to the **first** display entity in the stack to prevent visual duplication.

| Method | Description |
| :--- | :--- |
| **`setForcedNametag(player, component)`** | Displays a static component to all tracked viewers. |
| **`setForcedNametag(player, viewer, component)`** | Displays a static component exclusively to a target viewer player. |
| **`clearForcedNametag(player)`** | Removes the forced component override for all viewers. |
| **`clearForcedNametag(player, viewer)`** | Removes the forced component override for a specific viewer. |

### Example: Conditional Visibility Toggles

```java
// Show a generic label to general spectators
api.setForcedNametag(target, Component.text("???", NamedTextColor.GRAY));

// Show the true username only to an admin spectator
api.setForcedNametag(target, admin, Component.text(target.getName(), NamedTextColor.WHITE));

// Restore default rendering when complete
api.clearForcedNametag(target);
```

---

## Animations

### Programmatic Animation Overrides
Apply or clear animations on specific display group rows by their index:

```java
// Apply a Y-axis rotation to the first display group (index 0)
api.setNametagDisplayGroupAnimation(player, 0, new DisplayAnimation.RotateDisplayAnimation(...));

// Clear active animations on the first display group
api.clearNametagDisplayGroupAnimation(player, 0);
```

> [!NOTE]
> The display group index parameter is 0-based. If the index exceeds the size of the player's active display group list, an `IllegalArgumentException` is thrown.

### Custom Animations
Register a dynamic pose modifier by implementing the functional interface `NametagCustomAnimationHandler`. To apply this animation in `settings.yml`, define `animation.type: custom` and matching `id`.

```java
api.registerNametagCustomAnimation("my_pulse", (target, animation, elapsedMs) -> {
    float scale = 1.0f + 0.1f * (float) Math.sin(elapsedMs / 500.0 * Math.PI * 2);
    target.setAnimationScale(scale);
});
```

The handler interface provides three parameters:
* **`target`** (`NametagAnimationTarget`): An interface allowing updates to the display's scale and positional offsets.
* **`animation`** (`DisplayAnimation.CustomDisplayAnimation`): The animation configuration containing custom configuration properties.
* **`elapsedMs`**: The total elapsed time in milliseconds since the animation execution was initialized.

```java
// Unregister a custom handler
api.unregisterNametagCustomAnimation("my_pulse");

// Retrieve an active handler (returns null if unregistered)
api.getNametagCustomAnimationHandler("my_pulse");
```

---

## Refresh & Visibility Controls

| Method | Description |
| :--- | :--- |
| **`forceRefresh(player)`** | Immediately evaluates placeholders and updates display packets sent to viewers. |
| **`forceRefresh(player, force)`** | Executes a manual refresh with a forced packet reconstruction override flag. |
| **`hideNametag(player)`** | Despawns name tag display entities for all tracking viewers. |
| **`showNametag(player)`** | Spawns name tag display entities for all currently tracking players. |
| **`getPacketDisplayText(player)`** | Accesses the active collection of name tag display entities. |

> [!NOTE]
> For general rendering updates, prefer calling `forceRefresh` over direct display entity manipulation.

---

## Sneak Opacity Controls

By default, the plugin reduces name tag opacity when players sneak. You can disable this behavior on a per-player basis (e.g., inside PvP arenas or mini-game zones).

```java
// Disables sneaking opacity changes for this player
api.setShiftSystemBlocked(player, true);

// Returns true if the sneaking opacity system is currently blocked
api.isShiftSystemBlocked(player);

// Restores standard sneaking opacity behavior
api.setShiftSystemBlocked(player, false);
```

---

## Vanish Integrations

Implement the `VanishIntegration` interface to hook your custom or third-party vanish plugin into the visibility engine. This regulates which players' name tag display packets are sent to viewers.

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

The API also exposes utility methods to manage player presence in the tab list and server scoreboards (useful for soft vanish implementations):

```java
// Hide the player from the tab list and scoreboard tracking
api.vanishPlayer(player);

// Restore the player's presence in the tab list and scoreboard tracking
api.unVanishPlayer(player);
```

---

## Hat Offset Hooks

Implement the functional interface `HatHook` to supply custom height offsets for headwear or cosmetics that are not automatically detected by built-in integrations.

```java
HatHook hook = player -> {
    MyCosmetic hat = myPlugin.getActiveHat(player);
    return hat != null ? hat.getHeightOffset() : 0.0;
};

// Register the custom hook
api.addHatHook(hook);

// Unregister the custom hook when disabled
api.removeHatHook(hook);
```

> [!NOTE]
> If a registered hook does not apply to the queried player, return `0.0`. For file-based helmet height adjustment rules, prefer configuring `advanced.yml` instead.

---

## Direct Display Entity Access

> [!CAUTION]
> The method `getPacketDisplayText(player)` exposes the active list of `UntNametagDisplay` entities. This method should only be used in advanced cases (e.g., low-level packet modification, custom viewer filtering). Under normal circumstances, use high-level API methods to ensure stability.
