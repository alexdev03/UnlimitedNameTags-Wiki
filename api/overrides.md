# Name Tag Overrides

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

## Example: Appending a Row

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

## Property Shortcuts

Convenience methods that modify the effective layout and register it as an override:

| Method | Description |
| :--- | :--- |
| **`setNametagScale(player, scale)`** | Updates scale across all display groups. |
| **`setNametagBackground(player, background)`** | Applies a background plate across all groups. |
| **`setNametagDisplayGroups(player, list)`** | Overwrites the entire display group list. |
| **`setNametagShadowed(player, shadowed)`** | Toggles drop-shadow on all text lines. |
| **`setNametagSeeThrough(player, seeThrough)`** | Toggles block-transparency on all text lines. |
| **`setNametagBillboard(player, billboard)`** | Updates billboard mode for all display groups. |
