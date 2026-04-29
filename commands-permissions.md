# Commands & Permissions

Use **`/unt`** in chat (same idea as **`/unlimitednametags`** — either spelling works).

**Permissions** are the nodes you give with LuckPerms, PermissionsEx, or whatever you already use.

---

## Commands

### General

#### `/unt`

- **Description:** Shows the plugin version and lists subcommands.
- **Usage:** `/unt`
- **Permission:** none

---

#### `/unt reload`

- **Description:** Reloads configuration and refreshes nametag state **without** restarting the server.
- **Usage:** `/unt reload`
- **Behaviour:** Reloads `settings.yml` and, if present, `advanced.yml`; refreshes the nametag manager and placeholder manager.
- **Permission:** `unt.reload`

---

#### `/unt debugger <true|false>`

- **Description:** Turns **persistent debug mode** for the nametag system on or off.
- **Usage:** `/unt debugger true` or `/unt debugger false`
- **Permission:** `unt.debug`

---

#### `/unt debug`

- **Description:** Runs a **one-shot** debug pass for the command sender (unlike `/unt debugger`, which toggles persistent debug).
- **Usage:** `/unt debug`
- **Permission:** `unt.debug`

---

### Nametag Management

#### `/unt show <player>`

- **Description:** Shows the nametag for the given player (server / tracking side).
- **Usage:** `/unt show <player>`
- **Example:** `/unt show AlexDev`
- **Permission:** `unt.show`

---

#### `/unt hide <player>`

- **Description:** Hides the nametag for the given player.
- **Usage:** `/unt hide <player>`
- **Example:** `/unt hide AlexDev`
- **Permission:** `unt.hide`

---

#### `/unt refresh <player>`

- **Description:** Refreshes how **you** see that player's nametag (command sender only).
- **Usage:** `/unt refresh <player>` *(player sender only)*
- **Example:** `/unt refresh AlexDev`
- **Permission:** `unt.refresh`

---

### Customisation

#### `/unt billboard <type>`

- **Description:** Sets the default billboard mode (`CENTER`, `HORIZONTAL`, `VERTICAL`, `FIXED`). Persisted to `settings.yml`.
- **Usage:** `/unt billboard <type>`
- **Example:** `/unt billboard CENTER`
- **Permission:** `unt.billboard`

---

#### `/unt formatter <formatter>`

- **Description:** Sets the default text formatter (`MINIMESSAGE`, `MINEDOWN`, `LEGACY`, `UNIVERSAL`). Persisted to `settings.yml`.
- **Usage:** `/unt formatter <formatter>`
- **Example:** `/unt formatter MINIMESSAGE`
- **Permission:** `unt.formatter`

---

### Other Players' Nametags (Per Viewer)

#### `/unt hideOtherNametags [silent]`

- **Description:** Hides other players' nametags for the command sender.
- **Usage:** `/unt hideOtherNametags` or `/unt hideOtherNametags true` *(suppress confirmation message)*
- **Permission:** `unt.hideOtherNametags`

---

#### `/unt showOtherNametags [silent]`

- **Description:** Shows other players' nametags again for the command sender.
- **Usage:** `/unt showOtherNametags` or `/unt showOtherNametags true` *(suppress confirmation message)*
- **Permission:** `unt.showOtherNametags`

---

### Per-Player Preferences

Preferences are stored per-player via PersistentDataContainer and persist across restarts.

#### `/unt preferences`

- **Description:** Lists the available preference sub-commands.
- **Usage:** `/unt preferences`
- **Permission:** `unt.preferences`

---

#### `/unt preferences get [player]`

- **Description:** Shows the current nametag preferences for yourself, or for a target player.
- **Usage:** `/unt preferences get` or `/unt preferences get <player>`
- **Note:** Viewing another player's preferences requires `unt.preferences.others`.
- **Permission:** `unt.preferences` (self), `unt.preferences.others` (target)

---

#### `/unt preferences seeothers <true|false> [player]`

- **Description:** Toggles whether you (or a target player) see other players' nametags. `false` is equivalent to `/unt hideOtherNametags`; `true` is equivalent to `/unt showOtherNametags`. Preference is persisted.
- **Usage:** `/unt preferences seeothers true|false [player]`
- **Permission:** `unt.preferences` (self), `unt.preferences.others` (target)

---

#### `/unt preferences showown <true|false> [player]`

- **Description:** Toggles whether you (or a target player) see your **own** nametag above your head.
- **Usage:** `/unt preferences showown true|false [player]`
- **Note:** Requires `allowPerPlayerShowOwnWhenGlobalDisabled: true` in `settings.yml` when the global `showCurrentNameTag` is `false`.
- **Permission:** `unt.preferences` (self), `unt.preferences.others` (target)

---

#### `/unt preferences showothers <true|false> [player]`

- **Description:** Toggles whether your (or a target player's) nametag is visible to **other players**.
- **Usage:** `/unt preferences showothers true|false [player]`
- **Permission:** `unt.preferences` (self), `unt.preferences.others` (target)

---

## Default Permissions

Configure in your permissions plugin (e.g. LuckPerms).

| Permission | Default | Purpose |
|------------|---------|---------|
| `unt.shownametags` | **true** | See other players' nametags. If removed, that player will not see any custom nametags. |
| `unt.showownnametag` | **true** | See your own nametag (when `showCurrentNameTag` is enabled in config). |

---

## Command Permissions (Summary)

| Permission | Default | Commands |
|------------|---------|----------|
| `unt.reload` | op | `/unt reload` |
| `unt.debug` | op | `/unt debug`, `/unt debugger` |
| `unt.show` | op | `/unt show` |
| `unt.hide` | op | `/unt hide` |
| `unt.refresh` | op | `/unt refresh` |
| `unt.billboard` | op | `/unt billboard` |
| `unt.formatter` | op | `/unt formatter` |
| `unt.hideOtherNametags` | true | `/unt hideOtherNametags` |
| `unt.showOtherNametags` | true | `/unt showOtherNametags` |
| `unt.preferences` | true | `/unt preferences` (self) |
| `unt.preferences.others` | op | `/unt preferences … <player>` |
