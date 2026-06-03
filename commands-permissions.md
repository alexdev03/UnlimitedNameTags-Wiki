# Commands & Permissions

Manage and configure **UnlimitedNameTags** in-game or via console using the command `/unt` (alias `/unlimitednametags`). 

Permissions can be assigned using **LuckPerms** or any compatible permission management plugin.

---

## Command Reference

### General Commands

#### `/unt`
* **Description:** Displays the plugin version and lists all available subcommands.
* **Usage:** `/unt`
* **Permission:** None

---

#### `/unt reload`
* **Description:** Reloads all configuration files and refreshes active name tags without requiring a server restart.
* **Usage:** `/unt reload`
* **Behavior:** Reloads `settings.yml` and `advanced.yml` (if present), then restarts the name tag and placeholder managers.
* **Permission:** `unt.reload`

---

#### `/unt debugger <true|false>`
* **Description:** Toggles persistent debug mode for the name tag system.
* **Usage:** `/unt debugger <true|false>`
* **Permission:** `unt.debug`

---

#### `/unt debug`
* **Description:** Executes a single debug check pass relative to the command executor.
* **Usage:** `/unt debug`
* **Permission:** `unt.debug`

---

### Name Tag Management

#### `/unt show <player>`
* **Description:** Forces the server-side visibility tracker to show the target player's name tag.
* **Usage:** `/unt show <player>`
* **Example:** `/unt show AlexDev`
* **Permission:** `unt.show`

---

#### `/unt hide <player>`
* **Description:** Forces the server-side visibility tracker to hide the target player's name tag.
* **Usage:** `/unt hide <player>`
* **Example:** `/unt hide AlexDev`
* **Permission:** `unt.hide`

---

#### `/unt refresh <player>`
* **Description:** Refreshes the visual rendering of the target player's name tag specifically for the command sender.
* **Usage:** `/unt refresh <player>` *(Must be executed by a player)*
* **Example:** `/unt refresh AlexDev`
* **Permission:** `unt.refresh`

---

### Configuration Modifiers

#### `/unt billboard <type>`
* **Description:** Updates and persists the default billboard mode in `settings.yml`.
* **Accepted Values:** `CENTER`, `HORIZONTAL`, `VERTICAL`, `FIXED` (See [Billboard Settings](features/billboards.md)).
* **Usage:** `/unt billboard <type>`
* **Example:** `/unt billboard CENTER`
* **Permission:** `unt.billboard`

---

#### `/unt formatter <formatter>`
* **Description:** Updates and persists the default text formatter in `settings.yml`.
* **Accepted Values:** `MINIMESSAGE`, `MINEDOWN`, `LEGACY`, `UNIVERSAL`.
* **Usage:** `/unt formatter <formatter>`
* **Example:** `/unt formatter MINIMESSAGE`
* **Permission:** `unt.formatter`

---

### Visibility Controls (Per-Viewer)

#### `/unt hideOtherNametags [silent]`
* **Description:** Hides all other players' name tags from the executor's view.
* **Usage:** `/unt hideOtherNametags [true]` *(Pass `true` to suppress confirmation messages)*
* **Permission:** `unt.hideOtherNametags`

---

#### `/unt showOtherNametags [silent]`
* **Description:** Restores the visibility of other players' name tags for the executor.
* **Usage:** `/unt showOtherNametags [true]` *(Pass `true` to suppress confirmation messages)*
* **Permission:** `unt.showOtherNametags`

---

### Player Preferences

Player preferences are stored via the player's `PersistentDataContainer` and persist across server restarts.

#### `/unt preferences`
* **Description:** Lists all available preference subcommands.
* **Usage:** `/unt preferences`
* **Permission:** `unt.preferences`

---

#### `/unt preferences get [player]`
* **Description:** Displays active name tag preferences for yourself or a target player.
* **Usage:** `/unt preferences get [player]`
* **Permission:** `unt.preferences` (for self), `unt.preferences.others` (to view another player)

---

#### `/unt preferences seeothers <true|false> [player]`
* **Description:** Toggles whether you (or the target player) can see other players' name tags. Setting this to `false` functions identically to `/unt hideOtherNametags`.
* **Usage:** `/unt preferences seeothers <true/false> [player]`
* **Permission:** `unt.preferences` (for self), `unt.preferences.others` (to modify another player)

---

#### `/unt preferences showown <true|false> [player]`
* **Description:** Toggles whether you (or the target player) can see your own name tag.
* **Usage:** `/unt preferences showown <true/false> [player]`
* **Permission:** `unt.preferences` (for self), `unt.preferences.others` (to modify another player)

> [!IMPORTANT]
> To allow players to toggle their own name tag visibility when the global `showCurrentNameTag` setting is disabled, you must enable `allowPerPlayerShowOwnWhenGlobalDisabled: true` in `settings.yml`.

---

#### `/unt preferences showothers <true|false> [player]`
* **Description:** Toggles whether your (or the target player's) name tag is visible to other players on the server.
* **Usage:** `/unt preferences showothers <true/false> [player]`
* **Permission:** `unt.preferences` (for self), `unt.preferences.others` (to modify another player)

---

## Default Permissions

Assign these nodes within your permissions plugin (e.g., LuckPerms) to control basic visibility behaviors:

| Permission | Default Status | Purpose |
| :--- | :--- | :--- |
| **`unt.shownametags`** | `true` | Allows the player to see other players' custom name tags. If revoked, the player will not see any custom name tags. |
| **`unt.showownnametag`** | `true` | Allows the player to see their own name tag (only active if `showCurrentNameTag` is set to `true` globally). |

---

## Command Permissions Summary

| Permission | Default Group | Bound Commands |
| :--- | :--- | :--- |
| **`unt.reload`** | `op` | `/unt reload` |
| **`unt.debug`** | `op` | `/unt debug`, `/unt debugger` |
| **`unt.show`** | `op` | `/unt show` |
| **`unt.hide`** | `op` | `/unt hide` |
| **`unt.refresh`** | `op` | `/unt refresh` |
| **`unt.billboard`** | `op` | `/unt billboard` |
| **`unt.formatter`** | `op` | `/unt formatter` |
| **`unt.hideOtherNametags`** | `true` | `/unt hideOtherNametags` |
| **`unt.showOtherNametags`** | `true` | `/unt showOtherNametags` |
| **`unt.preferences`** | `true` | `/unt preferences` (self) |
| **`unt.preferences.others`** | `op` | `/unt preferences ... <player>` |
