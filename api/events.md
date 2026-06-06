# Bukkit Events

Events ship in **`unlimitednametags-api-paper`** (`org.alexdev.unlimitednametags.api.event`).
Register them like any other Bukkit event.

All lifecycle events extend **`PlayerNametagLifecycleEvent`**, which exposes:

| Method | Description |
| :--- | :--- |
| **`getOwner()`** | The player whose nametag row is affected. |
| **`getViewer()`** | The player receiving (or losing) the row. |
| **`getDisplay()`** | The `UntNametagDisplay` row instance. |
| **`isOwnerViewingOwnNametag()`** | `true` when owner and viewer are the same player. |

## Event Reference

| Event | When it fires |
| :--- | :--- |
| **`PlayerNametagVisibilityEvent`** | Before a row is shown or hidden for a viewer. Listeners can override the final decision with **`setVisible(boolean)`**. |
| **`PlayerNametagShowEvent`** | When a row is shown to a viewer (after visibility checks pass). |
| **`PlayerNametagHideEvent`** | When a row is hidden from a viewer. |
| **`PlayerNametagRefreshEvent`** | When an existing row is refreshed for a viewer (placeholder or layout update). |

> [!NOTE]
> **`PlayerNametagVisibilityEvent`** is the single decision point for show/hide. It runs before
> lifecycle show/hide events and receives the plugin's initial visibility (`isVisible()`), plus
> **`isViewerAlreadySeeing()`** to indicate whether the viewer already has the row spawned.

## Example: Block nametags in a custom region

```java
@EventHandler(ignoreCancelled = true)
public void onNametagVisibility(PlayerNametagVisibilityEvent event) {
    if (myRegion.contains(event.getOwner()) && !event.getViewer().hasPermission("myplugin.seetags")) {
        event.setVisible(false);
    }
}
```
