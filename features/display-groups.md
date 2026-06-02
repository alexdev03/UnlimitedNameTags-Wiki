# Display Groups

Each entry under **`nameTags`** has a list called **`displayGroups`**. That list is the **stack of rows** above the player's head — text, a floating item, or a block. **Order in the file = top to bottom** in-game.

---
## Row Types (`displayType`)

UnlimitedNameTags allows you to stack different types of elements to create beautiful multi-layered nametags. You can mix and match text, items, and blocks in a single configuration.

| Type | What it is | Primary Configuration | Supports PAPI |
|------|------------|-----------------------|:-------------:|
| **`TEXT`** (default) | One or more lines of high-performance text | Use **`lines`** array (supports MiniMessage & legacy) | Yes |
| **`ITEM`** | Floating 3D item models and custom textures | Use **`itemMaterial`** (and optional `itemDisplayMode`) | Yes |
| **`BLOCK`** | Floating 3D block models (vanilla or custom) | Use **`blockMaterial`** | Yes |

---

## 📝 TEXT Rows (`displayType: TEXT`)

This is the default type. It renders high-definition, customizable text lines.

### Features
* **Multi-Line Stacking**: Define one or more lines in the same display group.
* **MiniMessage Formatting**: Out-of-the-box support for gradients, hex colors, shadows, fonts, and bold/italic styles.
* **Line-Level Conditions**: Toggle visibility of individual lines using the `when` filter.

### Example
```yaml
displayGroups:
  - displayType: TEXT
    lines:
      - text: '<gradient:#ff5555:#ffaa00><b>ADMIN</b></gradient>'
      - text: '%luckperms_prefix%%player_name%'
      - text: '<gray>[Latency: <green>%player_ping%ms</green>]</gray>'
        when: '%player_ping% < 100'
      - text: '<gray>[Latency: <red>%player_ping%ms</red>]</gray>'
        when: '%player_ping% >= 100'
    scale: 1.0
    yOffset: 0.0
```

> [!TIP]
> Line-level `when` conditions are extremely efficient for toggling text layers (like latency indicators or status modes) without creating separate display groups.

---

## 👑 ITEM Rows (`displayType: ITEM`)

Allows you to render any Minecraft item or custom item model (including resource packs, Oraxen, Nexo, and ItemsAdder) floating above the player's head.

### Features
* **Custom Model Data**: Render complex cosmetic items like floating crowns, wings, or emblems.
* **`itemDisplayMode` Positioning**: Fine-tune how the item is rendered relative to the player.
* **PAPI Integration**: Resolve item names dynamically (e.g., render the player's active held item, or change icons based on rank).

### Example: Floating Crown/Badge
```yaml
- displayType: ITEM
  itemMaterial: GOLDEN_HELMET
  itemDisplayMode: HEAD  # Positions the item naturally above the head
  scale: 0.7
  yOffset: 0.4           # Raise it above the text row
  animation:
    type: rotate
    axis: Y
    degreesPerSecond: 90
```

### `itemDisplayMode` Options
| Mode | Description | Recommended Use |
|------|-------------|-----------------|
| **`HEAD`** (default) | Worn naturally — matches the orientation of a helmet on an armor stand | Floating crowns, hats, badges |
| **`FIXED`** | Fixed absolute orientation, facing flat | Standard 2D icons or custom UI badges |
| **`GROUND`** | Renders flat on the ground | Loot effects or status displays |
| **`GUI`** | Standard 2D inventory model style | Clean UI icons |

---

## 💎 BLOCK Rows (`displayType: BLOCK`)

Render actual 3D blocks (like diamonds, custom block models, or animated crystals) as part of the nametag.

### Features
* **Full 3D Block Models**: Renders block geometry accurately.
* **PAPI Support**: Change the block type dynamically based on game stats.
* **Visibility Rules**: Bind blocks to player conditions (e.g., show an emerald block when a player goes rich).

### Example: Spinning Wealth Crystal
```yaml
- displayType: BLOCK
  blockMaterial: AMETHYST_CLUSTER
  scale: 0.6
  yOffset: 0.5
  when: '%vault_eco_balance% > 50000'
  animation:
    type: rotate
    axis: Y
    degreesPerSecond: 180
```

---

## 🛡️ Row Visibility & Conditions (`when`)

You can conditionally show or hide any individual display group or single lines using powerful expressions.

### Group-Level Visibility
Hides or shows the entire row entity:
```yaml
displayGroups:
  - lines:
      - text: '%player_name%'
    scale: 1.0
  - lines:
      - text: '<gold>✪ VIP ✪</gold>'
    when: '%vault_eco_balance% > 1000'
    scale: 0.9
    yOffset: 0.15
```

### Per-Viewer Conditions (`relationalConditions`)
Evaluate the `when` condition relative to the viewer instead of just the tag owner. This lets you show unique tags (e.g. showing "Friend" only to mutual friends, or showing "Target" only to assassins):

```yaml
- lines:
    - text: '<light_purple>❤ Friend</light_purple>'
  when: '%friendsplugin_is_friend%'
  relationalConditions: true
```

> [!WARNING]
> Relational conditions require `enableRelationalPlaceholders: true` in your main `settings.yml`. They carry a performance cost proportional to the number of nearby players.

---

## 🎨 Background (`background`)

For `TEXT` rows, you can define a custom background panel behind the text lines.

```yaml
background:
  enabled: true
  color: '#1a1a1ade'    # Hex code (with optional alpha) or "R,G,B" format
  opacity: 200          # Opacity (0 = completely invisible, 255 = solid)
  shadowed: false       # Add drop shadow to text
  seeThrough: false     # Allow viewing text through blocks
```

---

## 📐 Scale and Offset

Adjust positioning and sizing per row to prevent clipping:
* **`scale`**: Multiplier for the display size (`1.0` = normal, `0.7` = 70%).
* **`yOffset`**: Vertically offsets the row in blocks relative to the base height (`0.0` = base, positive = higher, negative = lower).

---

## 🌐 Per-Row Billboard

Adjust how individual rows rotate toward the camera using **`billboard`**: `CENTER`, `HORIZONTAL`, `VERTICAL`, or `FIXED`. (See [Billboard](billboards.md) for a deep dive).

---

## 🚀 Full Stacking Example (The Ultimate Nametag)

This example combines **TEXT**, **ITEM**, and **BLOCK** types into a single premium nametag stack:

```yaml
nameTags:
  god_rank:
    permission: 'group.god'
    displayGroups:
      # Row 1 — 3D Spinning Amethyst Cluster on top
      - displayType: BLOCK
        blockMaterial: AMETHYST_CLUSTER
        scale: 0.5
        yOffset: 0.7
        animation:
          type: rotate
          axis: Y
          degreesPerSecond: 120

      # Row 2 — Floating Gold Star item in the middle
      - displayType: ITEM
        itemMaterial: NETHER_STAR
        itemDisplayMode: FIXED
        scale: 0.6
        yOffset: 0.3

      # Row 3 — Main Player Name & Rank Text at the base
      - displayType: TEXT
        lines:
          - text: '<gradient:#d4af37:#f3e5ab><b>GOD</b></gradient>'
          - text: '<white>%player_name%</white>'
        background:
          enabled: true
          color: '20,20,20'
          opacity: 180
          shadowed: true
        scale: 1.0
        yOffset: 0.0
```

---

## 🛡️ Permissions and Order

Each preset under **`nameTags`** can set **`permission`**. The plugin walks the list in file order and picks the **first** preset the player qualifies for. Put VIP / staff entries **above** `default`.

Keep a **`default`** entry (no `permission:`) as the catch-all for everyone else.


---

## Animations

Optional **`animation`** block on any row. See [Animations](animations.md) for all types and fields.

**`animationInterval`** (ticks) on a row overrides the global **`displayAnimationInterval`** for that row only. **`cullBeyondBlocks`** pauses pose updates when no viewer is nearby — good for decorative effects on busy servers.

See also: [Performance](../performance.md).
