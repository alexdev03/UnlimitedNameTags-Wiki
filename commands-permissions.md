# Commands & permissions

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

- **Description:** Runs a **one-shot** debug for the command sender (unlike `/unt debugger`, which toggles persistent debug).
- **Usage:** `/unt debug`
- **Permission:** `unt.debug`

---

### Nametag management

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

- **Description:** Refreshes how **you** see that player’s nametag (command sender only).
- **Usage:** `/unt refresh <player>` *(player sender only)*
- **Example:** `/unt refresh AlexDev`
- **Permission:** `unt.refresh`

---

### Customization

#### `/unt billboard <type>`

- **Description:** Sets the default billboard mode (`CENTER`, `HORIZONTAL`, `VERTICAL`, `FIXED`).
- **Usage:** `/unt billboard <type>`
- **Example:** `/unt billboard CENTER`
- **Permission:** `unt.billboard`

---

#### `/unt formatter <formatter>`

- **Description:** Sets the default text formatter (`MINIMESSAGE`, `MINEDOWN`, `LEGACY`, `UNIVERSAL`).
- **Usage:** `/unt formatter <formatter>`
- **Example:** `/unt formatter MINIMESSAGE`
- **Permission:** `unt.formatter`

---

### Other players’ nametags (per viewer)

#### `/unt hideOtherNametags [-h]`

- **Description:** Hides other players’ nametags for you.
- **Usage:** `/unt hideOtherNametags` or `/unt hideOtherNametags -h` *(hide confirmation when supported)*
- **Permission:** `unt.hideOtherNametags`

---

#### `/unt showOtherNametags [-h]`

- **Description:** Shows other players’ nametags again for you.
- **Usage:** `/unt showOtherNametags` or `/unt showOtherNametags -h`
- **Permission:** `unt.showOtherNametags`

---

## Default permissions

Configure in your permissions plugin (e.g. LuckPerms).

| Permission | Purpose |
|------------|---------|
| `unt.shownametags` | **Default: yes** for normal players. If removed, that player will not see **other** players’ UnlimitedNameTags nametags. |
| `unt.showownnametag` | Allows seeing **your own** custom nametag; without it, others can still see yours (subject to `showCurrentNameTag` in config). |

---

## Command permissions (summary)

| Permission | Commands |
|------------|----------|
| `unt.reload` | `/unt reload` |
| `unt.debug` | `/unt debug`, `/unt debugger` |
| `unt.show` | `/unt show` |
| `unt.hide` | `/unt hide` |
| `unt.refresh` | `/unt refresh` |
| `unt.billboard` | `/unt billboard` |
| `unt.formatter` | `/unt formatter` |
| `unt.hideOtherNametags` | `/unt hideOtherNametags` |
| `unt.showOtherNametags` | `/unt showOtherNametags` |
