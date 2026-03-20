# Commands and permissions

Commands for managing name tags. Aliases: `/unt`, `/unlimitednametags`.

---

## Commands

### Main

#### `/unt`

- **Description:** Shows the plugin version and lists available subcommands.
- **Usage:** `/unt`
- **Permission:** *(none)*

---

#### `/unt reload`

- **Description:** Reloads configuration and refreshes name tag state **without** restarting the server.
- **Usage:** `/unt reload`
- **Behavior:** Reloads `settings.yml` and, if the file exists, optional `advanced.yml`. Also reloads the name tag manager and placeholder manager.
- **Permission:** `unt.reload`

---

#### `/unt debugger <true|false>`

- **Description:** Turns **debug mode** for the name tag system on or off (verbose / diagnostic behaviour in-game).
- **Usage:** `/unt debugger true` or `/unt debugger false`
- **Permission:** `unt.debug`

---

#### `/unt debug`

- **Description:** Runs a **one-shot debug** action for the executing user (inspection / troubleshooting output from the name tag manager). This is **not** the same as `/unt debugger`, which toggles persistent debug mode.
- **Usage:** `/unt debug`
- **Permission:** `unt.debug`

---

### Name tag management

#### `/unt show <player>`

- **Description:** Shows the name tag for the given player (to the server / tracking system).
- **Usage:** `/unt show <player>`
- **Example:** `/unt show AlexDev`
- **Permission:** `unt.show`

---

#### `/unt hide <player>`

- **Description:** Hides the name tag for the given player.
- **Usage:** `/unt hide <player>`
- **Example:** `/unt hide AlexDev`
- **Permission:** `unt.hide`

---

#### `/unt refresh <player>`

- **Description:** Refreshes how you see that player’s name tag (for the **command sender** only).
- **Usage:** `/unt refresh <player>` *(player-only command sender)*
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

### Other players’ name tags (per viewer)

#### `/unt hideOtherNametags [-h]`

- **Description:** Hides other players’ name tags for you.
- **Usage:** `/unt hideOtherNametags` or `/unt hideOtherNametags -h` *(hide confirmation message when supported)*
- **Permission:** `unt.hideOtherNametags`

---

#### `/unt showOtherNametags [-h]`

- **Description:** Shows other players’ name tags again for you.
- **Usage:** `/unt showOtherNametags` or `/unt showOtherNametags -h`
- **Permission:** `unt.showOtherNametags`

---

## Default permissions

Configure these in your permissions plugin (e.g. LuckPerms).

| Permission | Purpose |
|------------|---------|
| `unt.shownametags` | **Default: yes** for normal players. If removed, that player will not see **other** players’ UnlimitedNameTags name tags. |
| `unt.showownnametag` | Allows seeing **your own** custom name tag; without it, others can still see yours (subject to `showCurrentNameTag` in config). |

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
