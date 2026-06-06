# What's New in UnlimitedNameTags v2! 🚀

Welcome to **UnlimitedNameTags v2**! This release represents a complete, ground-up rewrite of the plugin core to support modern Minecraft rendering features. We have transformed the way player name tags are configured, rendered, and animated.

Whether you're running a Survival, Roleplay, Minigames, or PvP server, v2 is designed to give you ultimate control over player displays with maximum performance.

Below is a detailed breakdown of all the new features, visual options, and quality-of-life improvements.

---

## 🎨 Beyond Text: Stacking, Items, & Blocks

### 1. Multi-Row Stacked Display Groups
In **v1**, name tags were flat and all text shared the same layout, scaling, and background.
In **v2**, we introduced **Display Groups**. You can now stack multiple rows of name tags above players, with each row having its own:
* 📏 **Scale**: Make the main player name large and tags/rank lines smaller.
* 🎨 **Background plate**: Customize the background color, transparency, shadows, or text see-through toggles per row.
* ↕️ **Y-Offset**: Control the spacing between stacked rows down to the pixel.

> [!TIP]
> This allows you to build highly structured name tags. For example, you can have a spinning item at the top, a rank badge below it, the player name in the middle, and health bar text at the bottom.
> Learn more in the [Display Groups Guide](features/display-groups.md).

### 2. Rendering Items and Blocks ⚔️
Name tags are no longer limited to plain text! You can now display actual Minecraft items (including custom weapons, tools, or custom 3D models) and blocks directly above player heads.
* **Item Displays (`displayType: ITEM`)**: Render floating item icons. Supports custom posture modes like `HEAD` (for crowns/hats), `FIXED` (for flat UI icons), `GUI` (2D inventory slot style), and `GROUND`.
* **Block Displays (`displayType: BLOCK`)**: Render full 3D block geometry (crystals, ores, etc.).
* **Dynamic Binding**: Item and block materials support placeholders, allowing you to change what's shown dynamically (e.g. showing a player's held item in their tag!).
* **Asset Integration**: Fully compatible with resource pack engines like **Nexo**, **Oraxen**, and **ItemsAdder**.

---

## 🌀 Smooth Physical Animations

Bring your server to life with smooth, client-side physical animations applied to **items** and **blocks**! You can configure these directly in your `settings.yml` without any coding. For animated text colors, use [phase placeholders](features/animations.md#text-phase-placeholders).

1. **Bob**: Makes the display row float gently up and down in a sine wave.
2. **Rotate**: Spins the item or block on any axis (X, Y, Z) or all of them.
3. **DVD Bounce**: Bounces the display side-to-side like the classic DVD screensaver.
4. **Pulse**: Rhythmically grows and shrinks the display row.
5. **Wiggle**: Tilts the display row back and forth.
6. **Orbit**: Revolves the display row in a circular orbit around the player's head.

> [!NOTE]
> **Performance Optimization (Animation Culling):** All physics calculations are client-side. The server only updates coordinates, and you can configure `cullBeyondBlocks` to pause animation updates when no players are nearby to watch them!
> Learn more in the [Animations Guide](features/animations.md).

---

## 👁️ Tactical Visibility & Anti-Wallhack

### 1. Show While Looking 🎯
When enabled, name tags only appear when you look directly at a player with your crosshair. The moment you look away, the name tag disappears.
* Perfect for immersive roleplay, tactical minigames, or clean server screenshots.
* Learn more in the [Show While Looking & Through-Wall Guide](features/show-while-looking.md).

### 2. Through-Wall Occlusion Modes (`throughWallMode`)
Customize exactly what happens to name tags when players walk behind walls or solid geometry:
* **`SEE_THROUGH`** (Default): Vanilla behavior; name tags remain visible through walls.
* **`OBSCURED`**: Visible through walls, but **dims the opacity** to a custom level (e.g., 50%) so players know their target is behind cover.
* **`HIDE`**: Completely hides the name tag behind solid blocks. **Acts as a native anti-wallhack system!**
* Configured via **`throughWallMode`** in `settings.yml` — see [Show While Looking & Through-Wall](features/show-while-looking.md#companion-feature-through-wall-occlusion-throughwallmode).

---

## 💡 Dynamic Conditions & Placeholder Mapping

### 1. Per-Line and Per-Row Conditions (`when` Rules)
In v2, you can hide or show specific lines or entire display groups dynamically using `when:` condition checks (supporting PlaceholderAPI comparisons).
* *Example:* Only show a 📶 **Ping Indicator** if player ping is above 150ms.
* *Example:* Only show a 💬 **"AFK" badge** when `%essentials_afk%` equals `yes`.

### 2. Placeholder Replacements
You can map and format raw strings returned by PlaceholderAPI (e.g., world names, booleans, vanish statuses) into styled visual tags under the `placeholdersReplacements` section of your configuration.
* *Example:* Map raw world names like `world_nether` to a styled `<red>Nether</red>` tag.
* *Example:* Includes fallback (`ELSE`) rules to catch-all unmatched outputs.
* Learn more in the [Placeholder Replacements Guide](features/placeholders-replacements.md).

### 3. Optimized Relational Conditions
Relational conditions (comparing viewer properties with target player properties, e.g. "is player a friend of viewer?") are now evaluated per-viewer. This is heavily optimized to parse colors and layouts once, keeping CPU overhead minimal on active servers.

---

## 🪖 Smart Headwear & Helmet Height Rules

To prevent name tags from clipping into or overlapping custom helmets, carved pumpkins, or player heads:
1. **Dynamic Detection**: The plugin automatically detects equipped helmets, blocks, and custom model data headwear and shifts the name tag stack upward accordingly.
2. **`advanced.yml` Custom Height Rules**: You can now define explicit height offsets for custom item model data, equippable models (Minecraft 1.21.3+ component), or item namespaces (`itemModel` in Minecraft 1.20.5+).
* Learn more in the [Advanced Configuration Guide](features/advanced-yml.md).

---

## 🛠️ Command & Control Upgrades

### 1. Brigadier Command Engine
All commands (`/unt`) have been moved to the modern Brigadier engine. This gives server admins:
* Real-time, responsive **tab completion** for all subcommands and parameters.
* Clear visual errors in the chat box if a command is typed incorrectly.

### 2. Per-Player Preferences
Players can now toggle custom name tag visibility on/off using **`/unt preferences`**, which respect permissions and default rules. See the [Player Preferences Guide](features/player-preferences.md).

### 3. Display Group Glow ✨
Each **`ITEM`** or **`BLOCK`** row can carry a colored outline glow:
* **Fixed colors**, **rainbow**, **gradient**, and **custom** animated glow types.
* Reusable presets under `glowAnimations` in `settings.yml`.
* Runtime overrides via `/unt glow` and the Java API (with optional persistence across relog).
* Learn more in the [Glow Guide](features/glow.md).

### 4. Developer Bukkit Events
Addon plugins can listen to nametag lifecycle events:
* **`PlayerNametagVisibilityEvent`** — intercept show/hide decisions before packets are sent.
* **`PlayerNametagShowEvent`**, **`PlayerNametagHideEvent`**, **`PlayerNametagRefreshEvent`** — observe row lifecycle per viewer.
* Documented in the [Developer API — Events](api/events.md).

### 5. Maven Central API Artifacts
The multi-module API is published to **Maven Central** under **`io.github.alexdev03`** on release tags. See [API Setup](api/setup.md).

---

## 📊 Summary Comparison: v1 vs. v2

| Feature | Legacy v1.x | Modern v2.x |
| :--- | :--- | :--- |
| **Formatting Structure** | Flat (one size/style fits all) | **Stacked Display Groups** (custom size/style/background per row) |
| **Supported Content** | Text only | **Text, Items, and Blocks** |
| **Physical Animations** | None | **Bob, Rotate, Pulse, Orbit, Wiggle, DVD Bounce** |
| **Animation Culling** | N/A | **Yes** (stops updating when no players are nearby to save TPS) |
| **Through-Wall Visibility** | Basic on/off toggle | **Dimming (Obscured), See-through, or Hidden (Anti-Wallhack)** |
| **Helmet Detection** | Static offset | **Dynamic Height Compensation** (detects helmet type/custom models) |
| **Advanced Height Override** | Basic coordinates | **Support for itemModel and equippableModel components** |
| **Conditional Toggling** | Limited to entire groups | **Per-row and per-line conditional visibility** (`when: '...'`) |
| **Placeholder Formatting** | Raw values only | **Placeholder Replacements engine** (map raw values to styled text) |
| **Command System** | Legacy parser | **Brigadier Engine** (tab-completion & error highlighting) |
| **Row Glow** | None | **Fixed, rainbow, gradient, custom, and preset reference glow** |
| **Developer Events** | None | **Bukkit visibility and lifecycle events** |
| **API Distribution** | JitPack / shaded | **Maven Central** (`io.github.alexdev03`) |
| **Migration** | Manual rewrite | **Automatic Config Migrator** (safe auto-upgrade from v1 configuration) |

---

## 🚀 Hassle-Free Migration

Worried about having to rewrite your entire configuration? **UnlimitedNameTags v2 features an automatic config migrator!**

When you start the server with the new version, the plugin will:
1. Detect your old v1 configuration.
2. Create a secure timestamped backup of your settings.
3. Automatically convert your old settings to the modern v2 structure.

> [!IMPORTANT]
> Ready to make the jump? Check out the step-by-step [Migration Guide](migration.md) to verify your settings and adjust manual expressions.
