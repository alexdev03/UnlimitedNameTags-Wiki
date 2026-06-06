# Developer API

**UnlimitedNameTags** exposes a comprehensive Java API that allows other plugins to dynamically
control player name tags at runtime. You can programmatically override name tag text, inject
item/block entities, register custom animations and glow effects, listen to Bukkit lifecycle events,
implement custom vanish integrations, and adjust cosmetic height offsets.

---

## Adding the Dependency

Add **`unlimitednametags-api-paper`** from Maven Central as **compile-only** (`provided` in Maven).
Use the same version as the **UnlimitedNameTags** plugin on your server. Do not shade or bundle the
API — the plugin JAR must be on the server at runtime.

### Gradle (Kotlin DSL)
```kotlin
repositories {
    mavenCentral()
}

dependencies {
    compileOnly("io.github.alexdev03:unlimitednametags-api-paper:2.0.0")
}
```

### Maven
```xml
<dependency>
    <groupId>io.github.alexdev03</groupId>
    <artifactId>unlimitednametags-api-paper</artifactId>
    <version>2.0.0</version>
    <scope>provided</scope>
</dependency>
```

> [!NOTE]
> For UUID-only integrations without Paper types, use artifact **`unlimitednametags-api`** instead.
> It is pulled in transitively when you depend on **`api-paper`** — you normally do not need
> **`unlimitednametags-common`** separately.

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

- **Paper/Bukkit API (Recommended):**
  ```java
  UNTPaperAPI api = UNTPaperAPI.getInstance();
  ```

- **Platform-Neutral API (UUID-based):**
  ```java
  UNTAPI api = UNTAPI.getInstance();
  ```

> [!WARNING]
> Calling `getInstance()` before your plugin's `onEnable()` execution or when
> **UnlimitedNameTags** is disabled will throw an `IllegalStateException`. For soft-dependent
> setups, always verify plugin status using
> `Bukkit.getPluginManager().isPluginEnabled("UnlimitedNameTags")` before retrieving the instance.

---

## Core Types Reference

| Type | Description |
| :--- | :--- |
| **`UNTAPI`** | Core platform-neutral entry point (UUID-based). |
| **`UNTPaperAPI`** | Paper/Bukkit entry point with `Player` overloads, custom animation/glow registration, and forced nametag helpers. |
| **`UnlimitedNameTagsInstancePaper`** | Extended plugin interface reachable via `UNTPaperAPI.paperPlugin()`. |
| **`UntNametagManager` / `UntNametagManagerPaper`** | Nametag override, glow, refresh, and visibility operations (`api.nametagManager()`). |
| **`Settings.NameTag`** | Immutable configuration record for a permission preset and its `displayGroups` list. |
| **`Settings.DisplayGroup`** | A single stacked row (text, item, or block), including optional `glow` and `animation`. |
| **`Settings.NametagLine`** | A text line with optional `when` visibility condition. |
| **`Settings.Background`** | Background plate properties (color, opacity, shadow, see-through). |
| **`GlowOverride`** | Per-row glow configuration (`fixed`, `reference`, `rainbow`, `gradient`, `custom`). |
| **`DisplayAnimation`** | Built-in and custom physical animation definitions. |
| **`UntNametagDisplay`** | A live client-side display entity (one stacked row). |
| **`NametagCustomAnimationHandler`** | Functional interface for custom pose animations. |
| **`NametagCustomGlowHandler`** | Functional interface for custom glow color animations (`api-paper`). |
| **`NametagCustomGlowContext`** | Tick context passed to custom glow handlers (elapsed time, row interval, owner UUID). |
| **`VanishIntegration`** | Hook for third-party vanish plugins. |
| **`HatHook`** | Hook for custom helmet/cosmetic height offsets. |

---

## Bukkit Events

Events ship in the **`unlimitednametags-api-paper`** module (`org.alexdev.unlimitednametags.api.event`).
Register them like any other Bukkit event.

All lifecycle events extend **`PlayerNametagLifecycleEvent`**, which exposes:

| Method | Description |
| :--- | :--- |
| **`getOwner()`** | The player whose nametag row is affected. |
| **`getViewer()`** | The player receiving (or losing) the row. |
| **`getDisplay()`** | The `UntNametagDisplay` row instance. |
| **`isOwnerViewingOwnNametag()`** | `true` when owner and viewer are the same player. |

### Event Reference

| Event | When it fires |
| :--- | :--- |
| **`PlayerNametagVisibilityEvent`** | Before a row is shown or hidden for a viewer. Listeners can override the final decision with **`setVisible(boolean)`**. |
| **`PlayerNametagShowEvent`** | When a row is shown to a viewer (after visibility checks pass). |
| **`PlayerNametagHideEvent`** | When a row is hidden from a viewer. |
| **`PlayerNametagRefreshEvent`** | When an existing row is refreshed for a viewer (placeholder or layout update). |

### Example: Block nametags in a custom region

```java
@EventHandler(ignoreCancelled = true)
public void onNametagVisibility(PlayerNametagVisibilityEvent event) {
    if (myRegion.contains(event.getOwner()) && !event.getViewer().hasPermission("myplugin.seetags")) {
        event.setVisible(false);
    }
}
```

> [!NOTE]
> **`PlayerNametagVisibilityEvent`** is the single decision point for show/hide. It runs before
> lifecycle show/hide events and receives the plugin's initial visibility (`isVisible()`), plus
> **`isViewerAlreadySeeing()`** to indicate whether the viewer already has the row spawned.

---

## Name Tag Overrides

Player name tags are resolved from `settings.yml` unless an active override is registered via the
API. Overrides take precedence over config settings.

By default, overrides are **in-memory only** and are cleared on disconnect or restart.

On **`UNTAPI`**, pass **`persist = true`** on supported methods to store overrides in the player's
persistent data (survives relog). On **`UNTPaperAPI`**, the `persist` flag is available for glow
and animation overrides; for full nametag layout overrides with persistence, use the UUID overloads
on **`UNTAPI`**.

| Method | Description |
| :--- | :--- |
| **`setNametagOverride(player, nameTag)`** | Registers a complete custom `Settings.NameTag` override. |
| **`setNametagOverride(uuid, nameTag, persist)`** | UUID overload; persists across relog when `persist` is `true`. |
| **`removeNametagOverride(player)`** | Clears the active override, restoring the configuration layout. |
| **`removeNametagOverride(uuid, persist)`** | UUID overload; also removes a stored persistent override when `persist` is `true`. |
| **`hasNametagOverride(player)`** | Returns `true` if the player currently has an active override. |
| **`getNametagOverride(player)`** | Returns an `Optional<Settings.NameTag>` containing the override layout if present. |
| **`getEffectiveNametag(player)`** | Returns the active override layout if present, falling back to the configuration layout. |
| **`getConfigNametag(player)`** | Retrieves the player's default configuration layout, ignoring active overrides. |
| **`modifyNametagProperty(player, modifier)`** | Applies a mapping function to the effective layout and registers the result as an override. |

> [!WARNING]
> **`setNametagLines(Player, List)`** on **`UNTPaperAPI`** is **deprecated** (since 2.0.0). Use
> **`setNametagDisplayGroups(Player, List)`** instead.

### Example: Appending a Row to an Active Name Tag

```java
UNTPaperAPI api = UNTPaperAPI.getInstance();

api.modifyNametagProperty(player, current -> {
    List<Settings.DisplayGroup> groups = new ArrayList<>(current.displayGroups());
    groups.add(Settings.DisplayGroup.builder()
        .line("<red>Staff</red>")
        .scale(1.0f)
        .build());
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

## Display Group Glow (API)

Glow tints the outline of text, item, and block display rows. Overrides can be applied per display
group index (0-based). See the [Glow Guide](features/glow.md) for YAML configuration.

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

// Fixed red glow on row 0
api.setDisplayGroupFixedGlow(player, 0, "#ff0000");

// Rainbow glow on row 1 (persisted)
api.setDisplayGroupGlow(player, 1, NametagGlowOverrides.rainbow(1.5), true);

// Reference the built-in gold_pulse preset (handler default_gold_pulse is registered by the plugin)
api.setDisplayGroupGlow(player, 0, NametagGlowOverrides.reference("gold_pulse"));
```

### Registering Glow Presets and Custom Handlers

Available on **`UNTPaperAPI`** (and **`UnlimitedNameTagsInstancePaper`**). The plugin registers
built-in presets on enable (`rainbow`, `gradient`, `gold_pulse`) and the custom handler
**`default_gold_pulse`** used by the `gold_pulse` preset.

| Method | Description |
| :--- | :--- |
| **`registerNametagGlowAnimation(id, glow)`** | Registers a reusable glow preset (referenceable via `type: reference` in YAML). |
| **`unregisterNametagGlowAnimation(id)`** | Removes an API-registered preset. |
| **`getAllKnownGlowAnimationIds()`** | Union of `settings.yml` `glowAnimations` keys and API presets. |
| **`registerNametagCustomGlowHandler(id, handler)`** | Registers a handler for `GlowOverride` with `type: custom`. |
| **`unregisterNametagCustomGlowHandler(id)`** | Removes a custom glow handler. |

```java
// Register a reusable gradient preset
api.registerNametagGlowAnimation("staff_glow",
    NametagGlowOverrides.gradient(List.of("#ff5555", "#ffff55"), 8));

// Custom animated glow — return 24-bit RGB (0xRRGGBB) or null to clear glow for this tick
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

// Persist the animation override across relog
api.setNametagDisplayGroupAnimation(player, 0, animation, true);
```

> [!NOTE]
> The display group index parameter is 0-based. If the index exceeds the size of the player's active display group list, an `IllegalArgumentException` is thrown.

### Custom Animations
Register a dynamic pose modifier by implementing the functional interface `NametagCustomAnimationHandler`. To apply this animation in `settings.yml`, define `animation.type: custom` and matching `id`.

```java
api.registerNametagCustomAnimation("my_pulse", (target, animation, scaledElapsedSeconds) -> {
    float scale = 1.0f + 0.1f * (float) Math.sin(scaledElapsedSeconds * Math.PI * 2);
    target.setAnimationScale(scale);
});
```

The handler interface provides three parameters:
- **`target`** (`NametagAnimationTarget`): An interface allowing updates to the display's scale
  and positional offsets.
- **`animation`** (`DisplayAnimation.CustomDisplayAnimation`): The animation configuration containing
  custom configuration properties.
- **`scaledElapsedSeconds`**: Elapsed wall time since the animation started, multiplied by the row's
  configured speed.

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
> The method `getPacketDisplayText(player)` exposes the active list of `UntNametagDisplay` entities. This method should only be used in advanced cases (e.g., low-level packet modification, custom viewer filtering). Under normal circumstances, use high-level API methods or Bukkit events to ensure stability.
