# What's New in UnlimitedNameTags v2! 🚀

Welcome to **UnlimitedNameTags v2**! This release is a major rewrite designed to bring your server's player name tags to life with advanced visual elements, modern rendering features, and highly requested customization options.

Whether you are looking to create beautiful roleplay name tags, display items/blocks, or implement clean performance-friendly indicators, v2 is built for you.

---

## 🌟 Key Features & Improvements

### 1. Stacked Display Groups (Multi-Row Layouts)
In **v1**, name tags were flat and all text shared the same layout, scaling, and background.
In **v2**, we introduced **Display Groups**. You can now stack multiple rows of name tags above players, with each row having its own:
* 📏 **Scale**: Make the player name large, and the rank or health bar smaller.
* 🎨 **Background plate**: Customize the background color, transparency, or shadow per row.
* ↕️ **Y-Offset**: Control the spacing between stacked rows down to the pixel.

> [!TIP]
> This is perfect for showing a player's **Rank**, **Name**, **Guild**, and **Health** in a clean, tiered design. For details, see the [Display Groups Guide](features/display-groups.md).

---

### 2. Render Actual Items & Blocks! ⚔️
Name tags are no longer limited to plain text! You can now display actual Minecraft items (including weapons, tools, or custom 3D models) and blocks directly above player heads.
* **Item Displays**: Render item icons (e.g., a sword, a shield, or custom textures).
* **Block Displays**: Show rotating blocks or custom models.
* **Compatibility**: Works seamlessly with modern item engines like **Nexo**, **Oraxen**, and **ItemsAdder**.

---

### 3. Smooth Physical Animations 🌀
Bring your server to life with smooth, client-side physical animations applied to text, items, or blocks! You can configure these directly in your `settings.yml` without any code:
* **Bob**: Makes the name tag float gently up and down.
* **Rotate**: Spins the text, block, or item on any axis (X, Y, Z).
* **DVD Bounce**: Bounces the display side-to-side like the classic screensaver.
* **Pulse**: Rhythmically grows and shrinks the display.
* **Orbit**: Revolves the name tag in a circle around the player's head.

> [!NOTE]
> All animations are fully client-side and optimized to ensure zero server-side lag. Learn more in the [Animations Guide](features/animations.md).

---

### 4. Per-Line Conditional Rendering (`when` rules)
You can now show or hide specific lines within a display group dynamically. Using the `when:` condition parameter (supporting PlaceholderAPI), you can show/hide details based on player stats or situations.
* *Example:* Only show a 🔴 **Low Health indicator** when the player's health drops below 5 hearts.
* *Example:* Only show a 📶 **Ping indicator** if the player's ping is higher than 150ms.

---

### 5. Smart Helmet Height Compensation 🪖
Custom cosmetics, carved pumpkins, or bulky helmets can often cover or clip into name tags.
In **v2**, the plugin automatically detects what the player is wearing on their head—including custom armor, items, or custom model data configurations—and **automatically shifts the name tag upwards** to prevent clipping.

---

### 6. Advanced Through-Wall Visibility Modes 🧱
Choose exactly how name tags behave when players walk behind walls or obstacles using `throughWallMode`:
* `SEE_THROUGH`: Name tags are fully visible through walls.
* `OBSCURED`: Name tags are visible through walls, but **automatically dim** (adjustable opacity) so you can tell the player is behind cover.
* `HIDE`: Name tags are completely hidden when behind solid blocks.

---

## 📊 Summary Comparison: v1 vs. v2

| Feature | Legacy v1.x | Modern v2.x |
| :--- | :--- | :--- |
| **Formatting Structure** | Flat (one size/style fits all) | **Stacked Display Groups** (custom size/style per row) |
| **Supported Content** | Text only | **Text, Items, and Blocks** |
| **Animations** | Basic text | **Physical motion (Bob, Rotate, Pulse, Orbit, DVD Bounce)** |
| **Through-Wall Visibility** | Basic on/off toggle | **Dimming (Obscured), See-through, or Hidden** |
| **Helmet Detection** | Static offset | **Dynamic Height Compensation** (detects helmet type/custom models) |
| **Conditional Toggling** | Limited to entire groups | **Per-row and per-line conditional visibility** (`when: '...'`) |
| **Configuration Formatting** | Harder to organize | **Clean, modular yaml structure with auto-migration** |

---

## 🛠️ Hassle-Free Migration

Worried about having to rewrite your entire configuration? **UnlimitedNameTags v2 features an automatic config migrator!**

When you start the server with the new version, the plugin will:
1. Detect your old v1 configuration.
2. Create a secure timestamped backup of your settings.
3. Automatically convert your old settings to the modern v2 structure.

> [!IMPORTANT]
> Ready to make the jump? Check out the step-by-step [Migration Guide](migration.md) to verify your settings and adjust manual expressions.
