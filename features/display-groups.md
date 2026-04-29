# Display Groups

Each entry under **`nameTags`** has a list called **`displayGroups`**. That list is the **stack of rows** above the player's head — text, a floating item, or a block. **Order in the file = top to bottom** in-game.

---

## Row Types (`displayType`)

| Type | What it is | How you fill it in |
|------|------------|---------------------|
| **`TEXT`** (default) | One or more lines of text | Use **`lines`** with your text / placeholders |
| **`ITEM`** | Shows a floating item model | **`itemMaterial`** (and `itemDisplayMode`) — `lines` are ignored |
| **`BLOCK`** | Shows a floating block | **`blockMaterial`** — `lines` are ignored |

---

## TEXT Row: Lines

In `configVersion: 4`, **`lines`** is a list of objects. Each entry has a **`text`** field and an optional **`when`** field:

```yaml
displayGroups:
  - lines:
      - text: '%luckperms_prefix%%player_name%%luckperms_suffix%'
      - text: '<green>%player_ping%ms</green>'
        when: '%player_ping% < 70'
      - text: '<red>%player_ping%ms</red>'
        when: '%player_ping% >= 70'
    scale: 1.0
    yOffset: 0.0
```

Line-level `when` hides only that single line. Group-level `when` (on the `displayGroup` itself) hides the whole row entity. Group condition is evaluated first.

---

## Row Visibility (`when`)

Optional **`when:`** on a row means "only show this row if this condition is true." You can use PlaceholderAPI placeholders inside the condition.

```yaml
displayGroups:
  - lines:
      - text: '%player_name%'
    scale: 1.0
    yOffset: 0.0
  - lines:
      - text: '<gold>VIP</gold>'
    when: '%vault_eco_balance% > 1000'
    scale: 0.9
    yOffset: 0.15
```

### Per-Viewer Conditions (`relationalConditions`)

Set **`relationalConditions: true`** on a row to evaluate its `when` condition per-viewer using relational PlaceholderAPI (the condition sees both the tag owner and the viewer):

```yaml
- lines:
    - text: '<aqua>Friend</aqua>'
  when: '%friendsplugin_is_friend%'
  relationalConditions: true
```

> **Note:** Relational conditions require `enableRelationalPlaceholders: true` in `settings.yml` and PlaceholderAPI. Cost increases with player count. See [Performance](../performance.md).

---

## Background (`background`)

Optional panel behind the text of a TEXT row.

```yaml
background:
  enabled: true
  color: '#1a1a1a'    # hex string — or use "R,G,B" e.g. "26,26,26"
  opacity: 200        # 0–255
  shadowed: false
  seeThrough: false
```

| Field | Description |
|-------|-------------|
| **`enabled`** | Show or hide the panel. |
| **`color`** | Hex string (`'#RRGGBB'`) or comma-separated RGB (`'R,G,B'`). |
| **`opacity`** | `0` = transparent, `255` = fully opaque. |
| **`shadowed`** | Enable drop shadow on the text. |
| **`seeThrough`** | Render text through blocks when the display entity is visible. |

> **Note:** Older configs used `type: integer` or `type: hex` background formats. These are migrated automatically — see [Migration Guide](../migration.md).

---

## Scale and Offset

- **`scale`** — size of that row (`1.0` = normal; `0.8` = 80% size).
- **`yOffset`** — vertical shift for this row relative to the base stack position (blocks).

---

## Per-Row Billboard

Optional **`billboard`**: `CENTER`, `HORIZONTAL`, `VERTICAL`, or `FIXED` — how that row turns toward the camera. If omitted, the global **`defaultBillboard`** applies. See [Billboard](billboards.md).

---

## ITEM Row

Shows a floating item model above the player. Set `displayType: ITEM` and provide `itemMaterial`:

```yaml
- displayType: ITEM
  itemMaterial: DIAMOND
  itemDisplayMode: HEAD
  scale: 0.8
  yOffset: 0.2
  animation:
    type: rotate
    axis: Y
    degreesPerSecond: 120
```

`itemMaterial` supports PlaceholderAPI — the material name is resolved per-refresh. Common values: `DIAMOND`, `PLAYER_HEAD`, `NETHERITE_SWORD`, etc.

### `itemDisplayMode`

Controls how the item model is oriented:

| Mode | Description |
|------|-------------|
| `HEAD` | Worn on head — same as an armour stand helmet |
| `GROUND` | Lying flat — like a dropped item |
| `FIXED` | Fixed orientation — like an item frame |
| `THIRDPERSON_LEFTHAND` | Third-person left-hand hold pose |
| `THIRDPERSON_RIGHTHAND` | Third-person right-hand hold pose |
| `FIRSTPERSON_LEFTHAND` | First-person left-hand pose |
| `FIRSTPERSON_RIGHTHAND` | First-person right-hand pose |
| `GUI` | GUI/inventory display orientation |

Most common for nametag use: **`HEAD`** or **`FIXED`**.

---

## BLOCK Row

Shows a floating block. Set `displayType: BLOCK` and provide `blockMaterial`:

```yaml
- displayType: BLOCK
  blockMaterial: EMERALD_BLOCK
  scale: 0.5
  yOffset: 0.4
  when: '%vault_eco_balance% > 10000'
```

`blockMaterial` supports PlaceholderAPI — the block type is resolved per-refresh.

---

## Full Multi-Type Example

```yaml
nameTags:
  vip:
    permission: 'group.vip'
    displayGroups:
      # Row 1 — text name tag
      - lines:
          - text: '<gold>%player_name%</gold>'
        background:
          enabled: true
          color: '#1a1a1a'
          opacity: 180
        scale: 1.1
        yOffset: 0.0
      # Row 2 — rotating diamond (always visible)
      - displayType: ITEM
        itemMaterial: DIAMOND
        itemDisplayMode: HEAD
        scale: 0.7
        yOffset: 0.5
        animation:
          type: rotate
          axis: Y
          degreesPerSecond: 120
          cullBeyondBlocks: 32
      # Row 3 — emerald block shown only to rich players (per-viewer condition)
      - displayType: BLOCK
        blockMaterial: EMERALD_BLOCK
        scale: 0.4
        yOffset: -0.2
        when: '%vault_eco_balance% > 10000'
        relationalConditions: true
```

---

## Permissions and Order

Each preset under **`nameTags`** can set **`permission`**. The plugin walks the list in file order and picks the **first** preset the player qualifies for. Put VIP / staff entries **above** `default`.

Keep a **`default`** entry (no `permission:`) as the catch-all for everyone else.

---

## Animations

Optional **`animation`** block on any row. See [Animations](animations.md) for all types and fields.

**`animationInterval`** (ticks) on a row overrides the global **`displayAnimationInterval`** for that row only. **`cullBeyondBlocks`** pauses pose updates when no viewer is nearby — good for decorative effects on busy servers.

See also: [Performance](../performance.md).
