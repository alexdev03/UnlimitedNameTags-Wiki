# Commands & Permissions

Manage and configure **UnlimitedNameTags** in-game or via console using the command `/unt` (alias `/unlimitednametags`). 

Permissions can be assigned using **LuckPerms** or any compatible permission management plugin.

---

## Command Reference

Below is a quick-reference table for all available commands. Click on a command to jump to its detailed description, usage examples, and behavior details.

| Command | Permission | Description |
| :--- | :--- | :--- |
| [**`/unt`**](#cmd-unt) | None | Displays version and lists available subcommands. |
| [**`/unt reload`**](#cmd-reload) | `unt.reload` | Reloads configuration files and refreshes active tags. |
| [**`/unt debugger <true/false>`**](#cmd-debugger) | `unt.debug` | Toggles persistent console debugging mode. |
| [**`/unt debug`**](#cmd-debug) | `unt.debug` | Executes a single debugging check pass. |
| [**`/unt show <player>`**](#cmd-show) | `unt.show` | Forces a player's name tag to become visible. |
| [**`/unt hide <player>`**](#cmd-hide) | `unt.hide` | Forces a player's name tag to become hidden. |
| [**`/unt refresh <player>`**](#cmd-refresh) | `unt.refresh` | Re-renders a target player's tag for the sender. |
| [**`/unt billboard <type>`**](#cmd-billboard) | `unt.billboard` | Updates the global default billboard rotation mode. |
| [**`/unt formatter <formatter>`**](#cmd-formatter) | `unt.formatter` | Updates the global default text formatter. |
| [**`/unt hideOtherNametags [silent]`**](#cmd-hideothers) | `unt.hideOtherNametags` | Hides all other players' tags from the sender's view. |
| [**`/unt showOtherNametags [silent]`**](#cmd-showothers) | `unt.showOtherNametags` | Restores other players' tags visibility for the sender. |
| [**`/unt preferences`**](#cmd-preferences) | `unt.preferences` | Lists available player preference subcommands. |
| [**`/unt preferences get [player]`**](#cmd-preferences-get) | `unt.preferences` | Displays active preferences for yourself or another. |
| [**`/unt preferences seeothers <true/false> [player]`**](#cmd-preferences-seeothers) | `unt.preferences` | Toggles whether you can see other players' name tags. |
| [**`/unt preferences showown <true/false> [player]`**](#cmd-preferences-showown) | `unt.preferences` | Toggles whether you can see your own name tag. |
| [**`/unt preferences showothers <true/false> [player]`**](#cmd-preferences-showothers) | `unt.preferences` | Toggles whether your tag is visible to other players. |
| [**`/unt glow …`**](#cmd-glow) | `unt.glow` | Per-player display-group glow overrides. |

---

### General Commands

<a id="cmd-unt"></a>
#### `/unt`
- **Description:** Displays the plugin version and lists all available subcommands.
- **Usage:** `/unt`
- **Permission:** None

---

<a id="cmd-reload"></a>
#### `/unt reload`
- **Description:** Reloads all configuration files and refreshes active name tags without
  requiring a server restart.
- **Usage:** `/unt reload`
- **Behavior:** Reloads `settings.yml` and `advanced.yml` (if present), then restarts the name tag
  and placeholder managers.
- **Permission:** `unt.reload`

---

<a id="cmd-debugger"></a>
#### `/unt debugger <true|false>`
- **Description:** Toggles persistent debug mode for the name tag system.
- **Usage:** `/unt debugger <true|false>`
- **Permission:** `unt.debug`

---

<a id="cmd-debug"></a>
#### `/unt debug`
- **Description:** Executes a single debug check pass relative to the command executor.
- **Usage:** `/unt debug`
- **Permission:** `unt.debug`

---

### Name Tag Management

<a id="cmd-show"></a>
#### `/unt show <player>`
- **Description:** Forces the server-side visibility tracker to show the target player's name tag.
- **Usage:** `/unt show <player>`
- **Example:** `/unt show AlexDev`
- **Permission:** `unt.show`

---

<a id="cmd-hide"></a>
#### `/unt hide <player>`
- **Description:** Forces the server-side visibility tracker to hide the target player's name tag.
- **Usage:** `/unt hide <player>`
- **Example:** `/unt hide AlexDev`
- **Permission:** `unt.hide`

---

<a id="cmd-refresh"></a>
#### `/unt refresh <player>`
- **Description:** Refreshes the visual rendering of the target player's name tag specifically
  for the command sender.
- **Usage:** `/unt refresh <player>` *(Must be executed by a player)*
- **Example:** `/unt refresh AlexDev`
- **Permission:** `unt.refresh`

---

### Configuration Modifiers

<a id="cmd-billboard"></a>
#### `/unt billboard <type>`
- **Description:** Updates and persists the default billboard mode in `settings.yml`.
- **Accepted Values:** `CENTER`, `HORIZONTAL`, `VERTICAL`, `FIXED` (See
  [Billboard Settings](features/billboards.md)).
- **Usage:** `/unt billboard <type>`
- **Example:** `/unt billboard CENTER`
- **Permission:** `unt.billboard`

---

<a id="cmd-formatter"></a>
#### `/unt formatter <formatter>`
- **Description:** Updates and persists the default text formatter in `settings.yml`.
- **Accepted Values:** `MINIMESSAGE`, `MINEDOWN`, `LEGACY`, `UNIVERSAL`.
- **Usage:** `/unt formatter <formatter>`
- **Example:** `/unt formatter MINIMESSAGE`
- **Permission:** `unt.formatter`

---

### Visibility Controls (Per-Viewer)

<a id="cmd-hideothers"></a>
#### `/unt hideOtherNametags [silent]`
- **Description:** Hides all other players' name tags from the executor's view.
- **Usage:** `/unt hideOtherNametags [true]` *(Pass `true` to suppress confirmation messages)*
- **Permission:** `unt.hideOtherNametags`

---

<a id="cmd-showothers"></a>
#### `/unt showOtherNametags [silent]`
- **Description:** Restores the visibility of other players' name tags for the executor.
- **Usage:** `/unt showOtherNametags [true]` *(Pass `true` to suppress confirmation messages)*
- **Permission:** `unt.showOtherNametags`

---

### Player Preferences

Player preferences are stored via the player's `PersistentDataContainer` and persist across server
restarts.

<a id="cmd-preferences"></a>
#### `/unt preferences`
- **Description:** Lists all available preference subcommands.
- **Usage:** `/unt preferences`
- **Permission:** `unt.preferences`

---

<a id="cmd-preferences-get"></a>
#### `/unt preferences get [player]`
- **Description:** Displays active name tag preferences for yourself or a target player.
- **Usage:** `/unt preferences get [player]`
- **Permission:** `unt.preferences` (for self), `unt.preferences.others` (to view another player)

---

<a id="cmd-preferences-seeothers"></a>
#### `/unt preferences seeothers <true|false> [player]`
- **Description:** Toggles whether you (or the target player) can see other players' name tags.
  Setting this to `false` functions identically to `/unt hideOtherNametags`.
- **Usage:** `/unt preferences seeothers <true/false> [player]`
- **Permission:** `unt.preferences` (for self), `unt.preferences.others` (to modify another player)

---

<a id="cmd-preferences-showown"></a>
#### `/unt preferences showown <true|false> [player]`
- **Description:** Toggles whether you (or the target player) can see your own name tag.
- **Usage:** `/unt preferences showown <true/false> [player]`
- **Permission:** `unt.preferences` (for self), `unt.preferences.others` (to modify another player)

> [!IMPORTANT]
> To allow players to toggle their own name tag visibility when the global `showCurrentNameTag`
> setting is disabled, you must enable `allowPerPlayerShowOwnWhenGlobalDisabled: true` in
> `settings.yml`.

---

<a id="cmd-preferences-showothers"></a>
#### `/unt preferences showothers <true|false> [player]`
- **Description:** Toggles whether your (or the target player's) name tag is visible to other
  players on the server.
- **Usage:** `/unt preferences showothers <true/false> [player]`
- **Permission:** `unt.preferences` (for self), `unt.preferences.others` (to modify another player)

---

<a id="cmd-glow"></a>
#### `/unt glow …`
- **Description:** Manage per-player glow overrides on display group rows. Overrides set via these
  commands are persisted across relog. Run `/unt glow` without arguments for the full subcommand list.
- **Usage:**
  - `/unt glow fixed <player> <group> <color>`
  - `/unt glow animation <id> [player] [group]`
  - `/unt glow rate <rate> <id> [player] [group]`
  - `/unt glow rainbow <player> <group> [speed]`
  - `/unt glow gradient <player> <group> <color...> [interval]`
  - `/unt glow clear <player> [group]`
  - `/unt glow get [player]`
- **Permission:** `unt.glow` (self), `unt.glow.others` (target other players)
- **Details:** See the [Glow Guide](features/glow.md).

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
| **`unt.glow`** | `true` | `/unt glow` (self) |
| **`unt.glow.others`** | `op` | `/unt glow ... <player>` |
