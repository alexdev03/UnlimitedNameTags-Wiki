# FAQ

---

## Installation & dependencies

### What else do I need besides UnlimitedNameTags?

**PacketEvents** is required — download it like a normal plugin and restart. **Paper 1.20.1+** (or **Spigot 1.20.2+**) is required for the main plugin.

### Where do I change settings?

Almost everything is in **`plugins/UnlimitedNameTags/settings.yml`**. It appears after the first successful start.

Optional helmet tuning: **`advanced.yml`** in the same folder — only if you need it; the plugin does **not** create this file for you.

### How do I apply config changes without restarting?

**`/unt reload`** reloads `settings.yml` and `advanced.yml` (if present) and refreshes nametags.

---

## Versions & Compatibility

### My config got backed up automatically after an upgrade — is that normal?

**Yes.** The plugin auto-migrates `settings.yml` when the schema version changes and creates a backup (`settings.yml.backup-<timestamp>.yml`) before rewriting anything. Your old file is safe. See [Migration Guide](migration.md).

### Do I need to hand-edit my config when upgrading from an older version?

**Usually no.** The plugin migrates v1 flat nametags to the current v4 sectioned format automatically on first load. The only case requiring manual work is the old **`modifiers:`** list — the migrator removes it and you need to replace it with a `when:` string. See [Migration Guide](migration.md).

### Why doesn’t someone see the custom nametag?

**Minecraft Java older than 1.19.4** cannot show this kind of nametag at all. Updating the game fixes it; config cannot bypass that.

### Does ViaVersion fix old clients?

**No** for actually **showing** these tags. Via may help the server **detect** what a client can do, but it does not add missing features to ancient clients.

### Is Bedrock the same as Java here?

**No.** With **Geyser** / Bedrock, things can look different: several lines, shadows, or backgrounds may not match Java. Expect **partial** support.

---

## Layout & Conditions

### I used `linesGroups` / `modifiers` on an old config — what now?

New configs use **`displayGroups`**: each entry is one stacked row (text, item, or block). **One `when:` line per row** replaces old modifier lists for “only show if…”.

### How do I show a line only sometimes?

Add **`when:`** under that row with a condition — for example “only if balance is above 1000” using placeholders. Examples: [Display groups](features/display-groups.md).

### I set up an ITEM or BLOCK row but `lines` seems ignored?

For **item** or **block** rows, the visual comes from **`itemMaterial`** / **`blockMaterial`** (and related fields), not from `lines`. You can leave `lines` empty.

### What is the difference between group-level `when:` and line-level `when:`?

**Group-level `when:`** (on a `displayGroup`) hides the **entire row entity** when false. **Line-level `when:`** (inside a `lines` entry) hides only **that single line of text** when false. Both can be used together. The group condition is evaluated first — if the group is hidden, line conditions are not checked.

### Can players toggle their own nametag visibility preferences?

**Yes.** Enable **`allowPerPlayerShowOwnWhenGlobalDisabled: true`** under `visibility:` and give players the `unt.preferences` permission. They can then use `/unt preferences showown true|false`. See [Commands & Permissions](commands-permissions.md).

---

## PlaceholderAPI

### Placeholders feel slow to update

The server only refreshes tags every **`taskInterval`** ticks. **Lowering** that number updates more often but costs more CPU — see [Performance](performance.md).

### Relational placeholders (viewer vs target) don’t work

Turn **`enableRelationalPlaceholders`** **on** in your settings. That can cost more on busy networks because more combinations are evaluated.

### How do I change the raw text PlaceholderAPI returns?

Use **`placeholdersReplacements`**: match the exact output and substitute nicer text. If the output is literally `Yes` or `No`, wrap it in **quotes** in YAML. [Placeholder replacements](features/placeholders-replacements.md).

---

## Look & behaviour

### Default Minecraft nametag still shows / overlaps

Set **`disableDefaultNameTag: true`**. If NPCs share a real player’s name, try **`forceDisableDefaultNameTag: true`**.

### I want to see my own nametag

**`showCurrentNameTag: true`** and give the player permission **`unt.showownnametag`** if your setup uses it.

### Tag sits too high / low with a custom hat

Prefer automatic pack hooks (Nexo, Oraxen, …) where they exist. Otherwise use **`advanced.yml`** rules. Some builds do not auto-detect every item plugin — see [Integrations](integrations/integrations.md).

---

## Performance

### Through-wall dimming is on but feels laggy

Raise **`visibility.obscuredNametagCheckInterval`** — it controls how often the line-of-sight raycast runs. The check runs on the **main thread** (required for an accurate ray trace), so raising it to `10`–`20` ticks significantly reduces cost. The visual delay before a tag dims equals this interval, which is usually unnoticeable. See [Performance](performance.md).

### Server struggles with many players

Start with [Performance](performance.md). In short: **raise `taskInterval`**, avoid `UNIVERSAL` formatting unless you need it, turn off wall / “only while looking” extras if unused, slow down heavy animations, and trim down placeholder-heavy lines.

### Animations are smooth but CPU is high

Increase **`displayAnimationInterval`** or the per-row **`animationInterval`**, and use **`cullBeyondBlocks`** on decorative motion so far-away players do not keep animations running.

---

## Support & bugs

### Where is help?

[Discord](https://discord.gg/W4Fu8fqCKs). For bugs, say your Paper/Spigot version, UnlimitedNameTags version, paste **relevant** config (no secrets), and what you did vs what you expected.

### `/unt debugger` vs `/unt debug`

**`/unt debugger true|false`** toggles **ongoing** debug output for the nametag system. **`/unt debug`** runs **one** debug pass for whoever ran the command. Both need staff-style permissions (`unt.debug`).
