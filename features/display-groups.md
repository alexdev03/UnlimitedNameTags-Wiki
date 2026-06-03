# Display Groups

The `displayGroups` list under each **`nameTags`** preset defines the layout stack rendered above a player's head. You can stack different types of elements (text lines, floating items, or blocks) in a single configuration. The order in which display groups are defined determines their vertical stack layout from top to bottom above the player.

---

## Display Group Types (`displayType`)

| Type | Description | Primary Key | PlaceholderAPI Support |
| :--- | :--- | :--- | :---: |
| **`TEXT`** (Default) | One or more lines of formatted text. | `lines` | Yes |
| **`ITEM`** | Floating 3D item models and custom textures. | `itemMaterial` | Yes |
| **`BLOCK`** | Static 3D block geometry (vanilla blocks or custom models). | `blockMaterial` | Yes |

---

## 📝 TEXT Rows (`displayType: TEXT`)

This is the default display type. It renders high-definition, customizable text lines.

### Features
* **Multi-Line Stacking**: Define one or more lines in the same display group.
* **MiniMessage Formatting**: Out-of-the-box support for gradients, hex colors, shadows, custom fonts, and styling.
* **Line-Level Conditions**: Toggle the visibility of individual text lines using the `when` condition.

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
> Line-level `when` conditions are highly efficient for toggling text layers (such as latency indicators or combat status) without creating separate display group entities.

---

## 👑 ITEM Rows (`displayType: ITEM`)

Renders a floating Minecraft item or custom item model (including resources from Nexo, Oraxen, ItemsAdder, etc.) above the player's head.

### Features
* **Custom Model Data**: Renders complex cosmetic items like floating crowns, wings, or shields.
* **Positioning**: Fine-tune item display postures using the `itemDisplayMode` key.
* **Dynamic Resolution**: Supports PlaceholderAPI to resolve item types dynamically (e.g., matching a player's active hand item).

### Example: Floating Crown
```yaml
- displayType: ITEM
  itemMaterial: GOLDEN_HELMET
  itemDisplayMode: HEAD  # Positions the item above the player head
  scale: 0.7
  yOffset: 0.4           # Vertically offsets it above the text row
  animation:
    type: rotate
    axis: Y
    degreesPerSecond: 90
```

### `itemDisplayMode` Reference
| Mode | Description | Recommended Use |
| :--- | :--- | :--- |
| **`HEAD`** (Default) | Renders the item as if equipped on an armor stand helmet slot. | Floating crowns, hats, and helmets |
| **`FIXED`** | Renders the item with a fixed flat posture facing the camera. | 2D UI emblems, badges, or flat icons |
| **`GROUND`** | Renders the item flat on the ground plane. | Drop loot icons or ground indicators |
| **`GUI`** | Renders the item in 2D inventory icon format. | Clean status indicators |

---

## 💎 BLOCK Rows (`displayType: BLOCK`)

Renders 3D blocks (such as diamond blocks, custom block geometry, or crystals) as part of the name tag stack.

### Features
* **Full 3D Block Models**: Renders actual block geometry.
* **Dynamic Material Binding**: Swap block materials dynamically based on player stats or placeholders.
* **Conditional Rendering**: Display blocks based on logical conditions (e.g., showing a status block when a player goes AFK).

### Example: Spinning Wealth Indicator
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

## 🛡️ Visibility & Logical Conditions (`when`)

Use the `when` condition parameter to conditionally display rows or specific text lines using mathematical or logical string evaluations.

### Group-Level Visibility
Controls the visibility of the entire display group entity:
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

### Per-Viewer Conditions
Evaluate conditions relative to the viewing player rather than the name tag owner. This allows you to show viewer-dependent details (e.g., showing a "Friend" tag only to players on the owner's friend list).

#### Automatic Detection of Relational Conditions
The plugin automatically detects if the `when` condition contains relational placeholders (e.g., `%rel_` or `%relational_`) and evaluates the condition per-viewer automatically. You do not need to manually configure `relationalConditions: true`, though you may declare it to explicitly force this behavior.

```yaml
- lines:
    - text: '<light_purple>❤ Friend</light_purple>'
  when: '%rel_friendsplugin_is_friend% == '\''true'\'''
```

> [!TIP]
> **Relational Performance Optimization:** Name tag rendering for relational placeholders has been heavily optimized using Adventure's `replaceText` API. The plugin compiles and parses formatting (like MiniMessage or legacy colors) **only once** for the tag owner, then performs a lightweight substitution of relational placeholders for each viewer. This drastically reduces CPU overhead compared to previous versions.

> [!WARNING]
> Relational conditions (both auto-detected and explicit) still carry a layout-calculation cost. Because display rows may be hidden for some viewers but shown to others, the plugin must compute and send stacked Y-offset packet updates individually for each viewer.

---

## 🎨 Background Layout (`background`)

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

Positioning and sizing parameters per row:
* **`scale`**: Sizing multiplier for the row display (`1.0` = default size, `0.7` = 70% scale).
* **`yOffset`**: Vertical offset (in blocks) relative to the group stack's base position. Positive values raise the row; negative values lower it.

---

## 🌐 Per-Row Billboard

Adjust how individual rows rotate toward the camera using **`billboard`**: `CENTER`, `HORIZONTAL`, `VERTICAL`, or `FIXED`. (See [Billboard Settings](billboards.md)).

---

## 🚀 Advanced Stacking Example

This example combines **TEXT**, **ITEM**, and **BLOCK** types into a single premium name tag layout:

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

## Permissions & Priority Evaluation

Each configuration preset under **`nameTags`** can define a `permission` node. The plugin evaluates these presets sequentially from top to bottom, applying the **first** preset for which the player holds the required permission. 

> [!NOTE]
> Custom group presets (e.g., Staff, VIP) must be ordered **above** the `default` preset. The `default` preset (which contains no `permission:` key) acts as the catch-all fallback and must always be positioned at the bottom of the list.

---

## Animation Overrides

You can apply an optional `animation` block on any row. (See [Animations Guide](animations.md) for full configuration options).

* **`animationInterval`** (in ticks): Overrides the global `behavior.displayAnimationInterval` for this specific row.
* **`cullBeyondBlocks`**: Pauses animation calculations when no players are within the specified block radius. (See [Performance Tuning Guide](../performance.md)).
