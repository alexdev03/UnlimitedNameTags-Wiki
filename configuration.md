# Configuration (`settings.yml`)

Almost everything runs from **`plugins/UnlimitedNameTags/settings.yml`**. You do not need to create it yourself — start the server once with the plugin, then edit the file it generates and run **`/unt reload`**.

For optional helmet-height rules in a separate file, see [Advanced (`advanced.yml`)](features/advanced-yml.md).

---

## How the file is organised

Think of it in three layers:

1. **Global options** — timing, how far tags can be seen, formatting style, and so on. In recent plugin versions these are grouped under headings like **`behavior`**, **`visibility`**, and **`performance`** instead of one long flat list. If your file still has settings at the top level, the plugin will migrate them when possible.
2. **`nameTags`** — named **presets** (e.g. `default`, `vip`). Each player gets **one** preset: the first in the file they have permission for, or `default` if nothing else matches.
3. **`placeholdersReplacements`** — optional rules that rewrite what PlaceholderAPI returns before it appears on the tag.

Each preset contains **`displayGroups`**: a **list of rows** stacked above the player (text lines, an item, or a block). The order in the file is top-to-bottom on screen.

---

## About “ticks”

Many numbers in the config are in **ticks**. On a normal server, **20 ticks ≈ 1 second** (20 updates per second). So `taskInterval: 20` usually means “refresh about once per second.”

---

## Example (older flat layout)

Your generated file might look different (newer backgrounds, or settings under `behavior:` / `visibility:`). This sample shows the idea: two stacked text rows, the second only when a balance check passes.

```yaml
configVersion: 2
defaultBillboard: CENTER
taskInterval: 20
displayAnimationInterval: 1
sneakOpacity: 70
yOffset: 0.3
viewDistance: 60
format: MINIMESSAGE
disableDefaultNameTag: true
forceDisableDefaultNameTag: false
removeEmptyLines: true
showWhileLooking: false
obscuredNametagThroughWalls: false
obscuredNametagOpacity: 55
obscuredNametagMaxDistance: 48.0
obscuredNametagCheckInterval: 5
showCurrentNameTag: false
componentCaching: false
placeholderCacheTime: 1
enableRelationalPlaceholders: false

nameTags:
  default:
    permission: nametag.default
    displayGroups:
      - lines:
          - '%luckperms_prefix% %player_name% %luckperms_suffix%'
        background:
          type: integer
          enabled: true
          red: 255
          green: 0
          blue: 0
          opacity: 255
          shadowed: true
          seeThrough: false
        scale: 1.0
        yOffset: 0.0
      - lines:
          - 'Rich rank line'
        background:
          type: hex
          enabled: true
          hex: '#ffffff'
          opacity: 255
          shadowed: false
          seeThrough: false
        scale: 1.0
        yOffset: 0.0
        when: '%vault_eco_balance% > 1000'

placeholdersReplacements:
  '%advancedvanish_is_vanished%':
    - placeholder: "Yes"
      replacement: ' &7[V]&r'
    - placeholder: "No"
      replacement: ''
```

---

## Global options (what each idea does)

### `configVersion`

Internal version number for the layout of this file. **Do not lower it** manually — let the plugin update it when you upgrade.

### `defaultBillboard`

How rows face the camera by default: `CENTER`, `HORIZONTAL`, `VERTICAL`, or `FIXED`. See [Billboard](features/billboards.md).

### `taskInterval` (ticks)

How often nametag text and placeholders are **refreshed**. Default is often `20` (~1 second). **Higher** = easier on the server, but money/ping-style text updates less often. See [Performance](performance.md).

### `displayAnimationInterval` (ticks)

How often **moving** tag animations update their pose if a row does not set its own speed. `0` means “match `taskInterval`.”

### `sneakOpacity`

How faint the tag looks while the player is **sneaking**.

### `yOffset`

Small vertical shift of the whole stack above the player.

### `viewDistance`

How far away clients are told **they might still draw** the tag (not exact block metres — the plugin scales this number internally). **Lower** often means less work for distant players’ games. See [Performance](performance.md).

### `format`

How chat formatting in lines is read: `MINIMESSAGE`, `MINEDOWN`, `LEGACY` (`&` colour codes), or `UNIVERSAL` (heaviest on the CPU).

### `disableDefaultNameTag` / `forceDisableDefaultNameTag`

Hide Minecraft’s built-in name so it does not sit on top of your custom tag. The **force** option helps odd cases where an NPC shares a real player’s name.

### `removeEmptyLines`

Hide blank lines when a placeholder resolves to nothing useful.

### `showWhileLooking`

Only show the tag when the **viewer is looking at** that player. See [Show while looking](features/show-while-looking.md).

### `obscuredNametagThroughWalls`

Dim or style tags when there is no clear line of sight. **`obscuredNametagCheckInterval`** controls how often that is rechecked — higher = less frequent checks, easier on the server.

### `showCurrentNameTag`

Whether someone can see **their own** tag above their head (like some client mods). Often used with permission `unt.showownnametag`.

### `componentCaching` / `placeholderCacheTime`

Optional caching to avoid redoing the same work every refresh. Can make very “live” placeholders look slightly out of date — see [Performance](performance.md).

### `enableRelationalPlaceholders`

If **on**, some PlaceholderAPI placeholders that need **who is looking at whom** are supported; cost can rise on big servers. Default is usually **off**.

### `placeholdersReplacements`

“When PlaceholderAPI returns exactly **this** text, show **that** instead.” If you need to match the word `Yes` or `No`, put it in **quotes** in YAML so the file does not treat it as true/false. See [Placeholder replacements](features/placeholders-replacements.md).

---

## Each row (`displayGroup`) — quick field list

| Field | Role |
|-------|------|
| `lines` | Text lines for a **normal text** row (default type). |
| `background` | Optional panel behind the text (colour from RGB numbers or hex). |
| `scale`, `yOffset` | Size and vertical position of that row in the stack. |
| `when` | Optional **condition**: if it is not true, the row is hidden. You can use placeholders inside the condition (same idea as “balance &gt; 1000” in the example above). |
| `displayType` | `TEXT` (default), `ITEM`, or `BLOCK`. |
| `itemMaterial`, `itemDisplayMode`, `blockMaterial` | For **item** or **block** rows instead of text. |
| `animation` | Movement effect (`rotate`, `bob`, …). |
| `animationInterval` | How often that row’s animation updates (ticks). |
| `billboard` | Per-row facing mode; overrides default if set. |

Deeper guides: [Display groups](features/display-groups.md), [Animations](features/animations.md).
