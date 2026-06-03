# Billboard Settings

The billboard mode defines how display group entities rotate to face the viewer's camera. You can configure a global default facing mode for all name tags, and optionally override it on a per-row basis.

---

## Billboard Modes

| Mode | Camera Alignment Behavior |
| :--- | :--- |
| **`CENTER`** | Standard billboard behavior. The display faces the camera on all axes, balancing pitch and yaw (closest to Minecraft vanilla rendering). |
| **`HORIZONTAL`** | Align yaw (horizontal rotation) with the camera, while locking the pitch (vertical tilt). |
| **`VERTICAL`** | Align pitch (vertical tilt) with the camera, locking the yaw. |
| **`FIXED`** | Fixed orientation relative to the player model. The display does not rotate to track the viewer's camera. |

> [!NOTE]
> The **`FIXED`** billboard mode is highly recommended for `ITEM` or `BLOCK` display groups (such as custom wings, backpacks, or status blocks) that must remain static relative to the player's physical posture.

---

## Configuration Examples

### CENTER (Default Global Setting)
```yaml
defaultBillboard: CENTER
```
![CENTER](../assets/billboard-center.gif)

---

### HORIZONTAL
```yaml
defaultBillboard: HORIZONTAL
```
![HORIZONTAL](../assets/billboard-horizontal.gif)

---

### VERTICAL
```yaml
defaultBillboard: VERTICAL
```
![VERTICAL](../assets/billboard-vertical.gif)

---

### FIXED
```yaml
defaultBillboard: FIXED
```
![FIXED](../assets/billboard-fixed.gif)

---

## Scope & Overrides

* **Global Settings**: Defined by the `defaultBillboard` key in `settings.yml` (e.g., `defaultBillboard: CENTER`).
* **Display Group Overrides**: Can be configured on a per-row basis using the `billboard` key inside a specific `displayGroup` (e.g., `billboard: FIXED`).
* **Command Modifier**: Run the in-game command `/unt billboard <CENTER|HORIZONTAL|VERTICAL|FIXED>` to update and persist the default global billboard setting in `settings.yml` (requires the permission node `unt.billboard`).

---

## See Also

* [Configuration Guide (`settings.yml`)](../configuration.md)
* [Display Groups Guide](display-groups.md)
