# Visibility & Refresh

## Refresh and Show/Hide

| Method | Description |
| :--- | :--- |
| **`forceRefresh(player)`** | Re-evaluates placeholders and updates display packets. |
| **`forceRefresh(player, force)`** | Manual refresh with forced packet reconstruction. |
| **`hideNametag(player)`** | Despawns name tag entities for all tracking viewers. |
| **`showNametag(player)`** | Spawns name tag entities for all tracking players. |
| **`getPacketDisplayText(player)`** | Returns active `UntNametagDisplay` row instances. |

> [!NOTE]
> Prefer **`forceRefresh`** over direct display entity manipulation.

To intercept visibility before packets are sent, use
[Bukkit events](events.md).

---

## Forced Packet-Level Name Tags

Forced nametags overwrite text at the packet level, bypassing config and layout overrides. Only the
**first** display entity in the stack is affected.

| Method | Description |
| :--- | :--- |
| **`setForcedNametag(player, component)`** | Static component for all tracked viewers. |
| **`setForcedNametag(player, viewer, component)`** | Static component for one viewer only. |
| **`clearForcedNametag(player)`** | Clears forced override for all viewers. |
| **`clearForcedNametag(player, viewer)`** | Clears forced override for one viewer. |

```java
api.setForcedNametag(target, Component.text("???", NamedTextColor.GRAY));
api.setForcedNametag(target, admin, Component.text(target.getName(), NamedTextColor.WHITE));
api.clearForcedNametag(target);
```

---

## Sneak Opacity

By default, name tag opacity drops when a player sneaks. Disable per player (e.g. in PvP arenas):

```java
api.setShiftSystemBlocked(player, true);
api.isShiftSystemBlocked(player);
api.setShiftSystemBlocked(player, false);
```
