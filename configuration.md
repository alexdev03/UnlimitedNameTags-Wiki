# Configuration (`settings.yml`)

Almost everything runs from **`plugins/UnlimitedNameTags/settings.yml`**. You do not need to create it yourself — start the server once with the plugin, then edit the file it generates and run **`/unt reload`**.

For optional helmet-height rules in a separate file, see [Advanced (`advanced.yml`)](features/advanced-yml.md).

---

## How the File Is Organised

Think of it in three layers:

1. **Global options** — grouped under **`behavior`**, **`visibility`**, and **`performance`** sections. These control timing, formatting, view distance, and so on.
2. **`nameTags`** — named **presets** (e.g. `default`, `vip`). Each player gets the **first** preset in the file they have permission for, or `default` if nothing matches.
3. **`placeholdersReplacements`** — optional rules that rewrite what PlaceholderAPI returns before it appears on the tag.

Each preset contains **`displayGroups`**: a **list of rows** stacked above the player (text lines, an item, or a block). The order in the file is top-to-bottom on screen.

> **Note:** If your config has keys at the top level (not under sections), the plugin auto-migrates them on load. See the [Migration Guide](migration.md).

---

## About "Ticks"

Many numbers in the config are in **ticks**. On a normal server, **20 ticks ≈ 1 second**. So `taskInterval: 20` means "refresh about once per second."

---

## Complete Example (`configVersion: 4`)

```yaml
configVersion: 4

behavior:
  taskInterval: 20
  displayAnimationInterval: 1
  yOffset: 0.3
  viewDistance: 60
  compactDisplayGroupStack: false
  displayGroupLineHeightBlocks: 0.25
  disableDefaultNameTag: true
  forceDisableDefaultNameTag: false
  defaultBillboard: CENTER
  format: MINIMESSAGE
  removeEmptyLines: true

visibility:
  sneakOpacity: 70
  showWhileLooking: false
  showCurrentNameTag: false
  allowPerPlayerShowOwnWhenGlobalDisabled: false
  obscuredNametagThroughWalls: false
  obscuredNametagOpacity: 55
  obscuredNametagMaxDistance: 48.0
  obscuredNametagCheckInterval: 5

performance:
  componentCaching: false
  placeholderCacheTime: 1
  enableRelationalPlaceholders: false
  placeholderUpdateRates: {}

nameTags:
  vip:
    permission: 'group.vip'
    displayGroups:
      - lines:
          - text: '<gold>%player_name%</gold>'
          - text: '<green>Rich</green>'
            when: '%vault_eco_balance% > 1000'
        background:
          enabled: true
          color: '#1a1a1a'
          opacity: 200
          shadowed: false
          seeThrough: false
        scale: 1.1
        yOffset: 0.0
  default:
    displayGroups:
      - lines:
          - text: '%luckperms_prefix%%player_name%%luckperms_suffix%'
        scale: 1.0
        yOffset: 0.0

placeholdersReplacements:
  '%advancedvanish_is_vanished%':
    - placeholder: "Yes"
      replacement: ' &7[V]&r'
    - placeholder: "No"
      replacement: ''
```

---

## Global Options

### `behavior` Section

| Option | Default | Description |
|--------|---------|-------------|
| **`taskInterval`** | `20` | Ticks between placeholder refreshes. Higher = less CPU, slower updates. |
| **`displayAnimationInterval`** | `1` | Ticks between animation pose updates. `0` = match `taskInterval`. |
| **`yOffset`** | `0.3` | Vertical shift (blocks) added to the whole stack. |
| **`viewDistance`** | `60` | Hint to clients about how far to render the nametag. Scaled internally. Lower = fewer distant renders. |
| **`compactDisplayGroupStack`** | `false` | When `true`, hidden or empty rows do not reserve vertical space in the stack. |
| **`displayGroupLineHeightBlocks`** | `0.25` | Estimated height per text line (blocks) used by compact stacking. |
| **`disableDefaultNameTag`** | `true` | Hide Minecraft's built-in name so it does not overlap the custom tag. |
| **`forceDisableDefaultNameTag`** | `false` | Force-disable via team cache. Useful when NPCs share a real player's name. |
| **`defaultBillboard`** | `CENTER` | Default camera-facing mode: `CENTER`, `HORIZONTAL`, `VERTICAL`, `FIXED`. See [Billboard](features/billboards.md). |
| **`format`** | `MINIMESSAGE` | Text parser: `MINIMESSAGE`, `LEGACY` (`&` codes), `UNIVERSAL` (both), `MINEDOWN`. |
| **`removeEmptyLines`** | `true` | Hide blank lines when a placeholder resolves to an empty string. |

### `visibility` Section

| Option | Default | Description |
|--------|---------|-------------|
| **`sneakOpacity`** | `70` | Opacity (0–127) while the player is sneaking. `-1` = fully opaque. |
| **`showWhileLooking`** | `false` | Only show the tag when the viewer aims at the player. See [Show While Looking](features/show-while-looking.md). |
| **`showCurrentNameTag`** | `false` | Allow players to see their own nametag. Requires permission `unt.showownnametag`. |
| **`allowPerPlayerShowOwnWhenGlobalDisabled`** | `false` | When `true`, players can toggle their own-tag visibility via `/unt preferences showown` even when `showCurrentNameTag` is `false`. |
| **`obscuredNametagThroughWalls`** | `false` | Dim the tag when there is no clear line of sight. |
| **`obscuredNametagOpacity`** | `55` | Opacity (0–127) when the viewer has no line of sight. |
| **`obscuredNametagMaxDistance`** | `48.0` | Max distance (blocks) for the through-wall check. |
| **`obscuredNametagCheckInterval`** | `5` | Ticks between line-of-sight rechecks. Runs on the main thread — raise on busy servers. |

### `performance` Section

| Option | Default | Description |
|--------|---------|-------------|
| **`componentCaching`** | `false` | Cache rendered text components to avoid repeat work. Can produce slightly stale text; may conflict with MiniPlaceholders. |
| **`placeholderCacheTime`** | `1` | Default placeholder cache time (ticks). |
| **`enableRelationalPlaceholders`** | `false` | Enable per-viewer PlaceholderAPI evaluation. Required for relational placeholders. High cost on large servers. |
| **`placeholderUpdateRates`** | `{}` | Per-placeholder refresh rate. Key = placeholder string, value = ticks. Example: `'%vault_eco_balance%': 100`. |

---

## `nameTags` Presets

Each entry under `nameTags:` is a named preset. The plugin checks them top-to-bottom and assigns the first preset whose `permission:` the player has.

```yaml
nameTags:
  staff:                       # preset name (arbitrary)
    permission: 'group.staff'  # optional; omit for a catch-all preset
    displayGroups:
      - ...
```

Place the `default` (no `permission:` field) preset last — it matches any player not covered by a higher preset.

---

## Each Row (`displayGroup`) — Field Reference

| Field | Default | Description |
|-------|---------|-------------|
| **`lines`** | `[]` | Text lines for a TEXT row. Each entry is `{text: '…', when: '…'}`. Ignored for ITEM and BLOCK rows. |
| **`background`** | null | Optional background panel. See Background below. |
| **`scale`** | `1.0` | Size multiplier for this row. |
| **`yOffset`** | `0.0` | Vertical offset (blocks) from the base stack position. |
| **`when`** | null | Condition expression; if false, the entire row entity is hidden. Supports PlaceholderAPI. |
| **`relationalConditions`** | `false` | When `true`, the `when` condition is evaluated per-viewer using relational PlaceholderAPI. |
| **`displayType`** | `TEXT` | Row type: `TEXT`, `ITEM`, or `BLOCK`. See [Display Groups](features/display-groups.md). |
| **`itemMaterial`** | `STONE` | Material for ITEM rows. Supports PlaceholderAPI. |
| **`blockMaterial`** | `STONE` | Material for BLOCK rows. Supports PlaceholderAPI. |
| **`itemDisplayMode`** | null | Item orientation: `HEAD`, `GROUND`, `FIXED`, `THIRDPERSON_LEFTHAND`, etc. |
| **`animation`** | null | Movement effect. See [Animations](features/animations.md). |
| **`animationInterval`** | null | Per-row animation tick interval override. |
| **`billboard`** | null | Per-row camera-facing override; overrides `defaultBillboard` for this row only. |

### Background

```yaml
background:
  enabled: true
  color: '#1a1a1a'    # hex string, OR use "R,G,B" e.g. "26,26,26"
  opacity: 200        # 0–255
  shadowed: false
  seeThrough: false
```

| Field | Description |
|-------|-------------|
| **`enabled`** | Show or hide the background panel. |
| **`color`** | Hex string (`'#RRGGBB'`) or RGB string (`'R,G,B'`). |
| **`opacity`** | `0` = transparent, `255` = fully opaque. |
| **`shadowed`** | Enable drop shadow on the text. |
| **`seeThrough`** | Render text through blocks when the display entity is visible. |

### Per-Line Conditions

In v4, `lines` is a list of objects. Each entry has a `text` field and an optional `when` field:

```yaml
lines:
  - text: '%player_name%'
  - text: '<green>%player_ping%ms</green>'
    when: '%player_ping% < 70'
  - text: '<red>%player_ping%ms</red>'
    when: '%player_ping% >= 70'
```

Line-level `when` hides only that single line. Group-level `when` (on the `displayGroup`) hides the whole row entity. Group condition is checked first — if the group is hidden, line conditions are not evaluated.

---

## `placeholdersReplacements`

Maps raw PlaceholderAPI output to a replacement string:

```yaml
placeholdersReplacements:
  '%advancedvanish_is_vanished%':
    - placeholder: "Yes"
      replacement: ' &7[V]&r'
    - placeholder: "No"
      replacement: ''
```

> **Note:** YAML 1.1 treats bare `Yes`, `No`, `On`, `Off`, `True`, `False` as booleans. Always quote them in `placeholder:` values.

See [Placeholder Replacements](features/placeholders-replacements.md) for more detail and the `ELSE` fallback.

---

Deeper guides: [Display Groups](features/display-groups.md), [Animations](features/animations.md), [Performance](performance.md).
