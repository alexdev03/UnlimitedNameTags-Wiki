# Configuration (`settings.yml`)

This page covers **`plugins/UnlimitedNameTags/settings.yml`**. The current schema is **config version 4**. For optional helmet height rules see [`advanced.yml`](features/advanced-yml.md).

---

## Minimal example

```yaml
configVersion: 4

nameTags:
  default:
    displayGroups:
      - lines:
          - text: '%luckperms_prefix% %player_name% %luckperms_suffix%'
        background:
          enabled: true
          color: "255,0,0"   # R,G,B  — or use hex: "#FF0000"
          opacity: 200
          shadowed: true
        scale: 1.0
        yOffset: 0.0

behavior:
  taskInterval: 20
  format: MINIMESSAGE
  disableDefaultNameTag: true

visibility:
  sneakOpacity: 70

performance:
  placeholderCacheTime: 1

placeholdersReplacements:
  '%advancedvanish_is_vanished%':
    - placeholder: "Yes"
      replacement: ' &7[V]&r'
    - placeholder: "No"
      replacement: ''
```

Unspecified keys keep their defaults. The file is auto-generated and updated on the first run.

---

## General structure

- **`nameTags`**: map of nametag profiles (keys e.g. `default`, `staffer`).
- The **first** profile for which the player has **`permission`** (if set) wins; otherwise **`default`** is used.
- Each profile contains **`displayGroups`**: a list of stacked rows (text, item, or block).
- Settings are grouped into **`behavior`**, **`visibility`**, and **`performance`** sections.

---

## `behavior` section

Controls how nametags are refreshed and rendered.

### `taskInterval` (ticks)

Interval between nametag **refreshes** (placeholders, text, etc.). Default `20` (one second). **Higher** = less load, less real-time updates. See [Performance](performance.md).

### `displayAnimationInterval` (ticks)

Ticks between **animation pose** updates when a `displayGroup` does not set its own `animationInterval`. `0` = follow `taskInterval`.

### `yOffset`

Base vertical offset (blocks) of the nametag stack relative to the player. Default `0.3`.

### `viewDistance`

Sent as the display entity **view range** (divided by 160 internally). Not exact blocks — it affects how far the **client** renders the entity. **Lower** = visible from shorter range (less client load). See [Performance](performance.md).

### `compactDisplayGroupStack`

When `true`, rows are stacked without gaps for inactive (hidden) rows. Rows that evaluate their `when` as false don't reserve space. Default `false`.

### `displayGroupLineHeightBlocks`

Approximate height (blocks) of one text line, used for compact stack spacing. Default `0.25`. Client-rendered font height varies.

### `format`

`MINIMESSAGE` (recommended), `MINEDOWN`, `LEGACY`, or `UNIVERSAL`. `UNIVERSAL` is the most flexible but heaviest on CPU.

### `defaultBillboard`

Default billboard for rows that do not set their own: `CENTER`, `HORIZONTAL`, `VERTICAL`, `FIXED`. See [Billboard](features/billboards.md).

### `disableDefaultNameTag` / `forceDisableDefaultNameTag`

Control whether and how the vanilla nametag is suppressed. `forceDisableDefaultNameTag` helps rare cases with NPCs that share an online player's name.

### `removeEmptyLines`

Strips empty lines after parsing. Default `true`.

---

## `visibility` section

Controls when and how players see nametags.

### `sneakOpacity`

Text opacity while the player is **sneaking** (-128…127; `-1` = fully opaque / 255). Below the client threshold (~26) text may not render.

### `showWhileLooking`

If `true`, a viewer only sees the tag when **aiming** at the player (plus other checks). See [Show while looking](features/show-while-looking.md).

### `obscuredNametagThroughWalls`

If `true`, viewers without clear line-of-sight get a dimmed style where supported. **`obscuredNametagCheckInterval`** controls how often LOS is rechecked — raise it to reduce work. **`obscuredNametagMaxDistance`** caps the effect distance (blocks).

### `obscuredNametagOpacity`

Text opacity used for the through-wall dimmed style (-128–127).

### `showCurrentNameTag`

Lets the player see their own tag (Lunar-style). Often paired with permission `unt.showownnametag`.

### `allowPerPlayerShowOwnWhenGlobalDisabled`

When `showCurrentNameTag: false`, players with `unt.showownnametag` can still toggle it on via `/unt preferences showown true`.

---

## `performance` section

### `componentCaching`

Caches rendered text components to reduce formatting cost. Can interact with very dynamic placeholders (e.g. MiniPlaceholders). Default `false`.

### `placeholderCacheTime` (ticks)

Global default cache for placeholder values. Effective update rate = `max(behavior.taskInterval, placeholderCacheTime)`. Higher = staler data, lower server load.

### `placeholderUpdateRates`

Per-placeholder refresh rate in ticks. Overrides `placeholderCacheTime` for specific placeholders:

```yaml
performance:
  placeholderUpdateRates:
    "%vault_eco_balance%": 100
    "%player_ping%": 10
```

Effective rate = `max(behavior.taskInterval, rate)`. Setting a rate lower than `taskInterval` has no effect. See [Performance](performance.md#performanceplaceholderupdaterates) for full details.

### `enableRelationalPlaceholders`

Enables **relational** PlaceholderAPI placeholders (viewer + target). Default `false` — enabling it multiplies PAPI calls by the number of viewers.

---

## `placeholdersReplacements`

Top-level map from placeholder name to a list of `{ placeholder, replacement }` entries. First match wins.

Quote YAML reserved words: `"Yes"`, `"No"`, `"On"`, `"Off"` must be quoted to avoid boolean parsing.

See [Placeholder replacements](features/placeholders-replacements.md).

---

## `DisplayGroup` fields (each row)

| Field | Role |
|-------|------|
| `lines` | Only for **`displayType: TEXT`** (default). Each entry: `{ text: "..." }` or `{ text: "...", when: "expr" }`. |
| `background` | Optional. If omitted, transparent default. See [Background](features/display-groups.md#background). |
| `scale` | Entity scale; `<= 0` treated as 1. |
| `yOffset` | Vertical offset for this row in the stack. |
| `when` | Optional JEXL + PAPI expression; if false, the row is hidden. |
| `relationalConditions` | If `true`, `when` and line conditions are evaluated per-viewer (requires PAPI). |
| `displayType` | `TEXT` (default), `ITEM`, `BLOCK`. |
| `itemMaterial`, `itemDisplayMode`, `blockMaterial` | For ITEM/BLOCK rows. `lines` is ignored for these types. |
| `animation` | Animation object. See [Animations](features/animations.md). |
| `animationInterval` | Ticks between poses for **this** row (overrides `displayAnimationInterval`). |
| `billboard` | Per-row billboard override. |

More detail: [Display groups](features/display-groups.md), [Animations](features/animations.md).

---

## Migration from older versions

The plugin migrates `settings.yml` automatically on startup and creates a timestamped backup in `plugins/UnlimitedNameTags/migration-backups/` before any change.

| Version | What changed |
|---------|-------------|
| 1 | Flat `lines`/`background`/`scale` on each NameTag |
| 2 | Introduced `displayGroups` with string lines |
| 3 | Lines became structured objects `{ text, when? }` |
| **4** | Background unified: no `type:` discriminator; settings moved to `behavior`/`visibility`/`performance` sections |
