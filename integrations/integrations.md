# Integrations

UnlimitedNameTags integrates with various plugins to extend functionality. Below are the supported integrations.

---

## Nexo and Oraxen

**Nexo** and **Oraxen** are supported for **dynamic name tag height** when players wear custom hats from those packs. The plugin derives offset from the item model / resource pack so the name tag sits more naturally above tall cosmetics.

### Nexo

When a player wears a Nexo hat (or an item using a merged resource pack), the name tag shifts vertically based on model height.

![Nexo Integration: Dynamic Name Tag Adjustment](https://i.imgur.com/ocnlz9Q.gif)

### Oraxen

The same style of **hat height** adjustment applies to **Oraxen** custom helmets, using Oraxen’s item definitions and custom model data.

For both Nexo and Oraxen, you can **override or supplement** automatic height with manual rules in [`advanced.yml`](../features/advanced-yml.md) (matching rules take precedence over automatic hooks when they apply).

---

## ItemsAdder

In the **current plugin source**, the ItemsAdder hook is **not registered** (it is disabled in code until re-enabled in a future release). Do **not** rely on automatic ItemsAdder-based hat height in production until an official release notes say otherwise.

You can still tune name tag height for ItemsAdder items using the optional [`advanced.yml`](../features/advanced-yml.md) file (e.g. `customModelData`, `equippableModel`, `material`).

---

## Hat / cosmetics plugins (HMCCosmetics, etc.)

**HMCCosmetics** is integrated for hat height when a compatible **CreativeHook** source (e.g. Nexo) is present.

There is **no dedicated hook** for **CosmeticsCore** in the plugin. Compatibility is **indirect** only: if the worn helmet is still a normal Bukkit item that Nexo or Oraxen (or manual `advanced.yml` rules) can describe, height rules may apply. Do not assume official CosmeticsCore support.

Use [`advanced.yml`](../features/advanced-yml.md) for per-item control when automatic hooks are not enough.

---

## ViaVersion

If **ViaVersion** is installed, the plugin uses it to detect whether a **viewer’s client** can handle **text display** name tags. This does **not** make **Java clients older than 1.19.4** supported: those clients **cannot** show these custom name tags. See **Supported Client Versions** on the [wiki home page](../README.md).

---

## LibsDisguises

**LibsDisguises** is supported: name tags can be hidden when a player is disguised as another entity.

---

## PlaceholderAPI

**PlaceholderAPI** is supported for placeholders in name tag lines. Enable **`enableRelationalPlaceholders`** in `settings.yml` if you need relational placeholders (viewer + target).

### Built-in expansion: `%unt_<param>%`

The plugin registers the **`unt`** expansion. Usable **outside** name tags too (e.g. other plugins that parse PAPI):

| Placeholder | Purpose |
|-------------|---------|
| `%unt_phase-mm%` | Animation phase value for MiniMessage-style rainbow / phase usage |
| `%unt_phase-md%` | Animation phase for MineDown |
| `%unt_phase-mm-g%` | Animation phase for MiniMessage gradient-style usage |
| `%unt_-phase-mm%` | Negative phase variant (MiniMessage) |
| `%unt_-phase-md%` | Negative phase variant (MineDown) |

These mirror the internal phase keys used by the [Animations](../features/animations.md) placeholders (e.g. `#phase-mm#` in lines).

---

## MiniPlaceholders

**MiniPlaceholders** is supported. Use **MINIMESSAGE** or **UNIVERSAL** as the name tag formatter for best results.

---

## TypeWriter

**TypeWriter** is supported: name tags can be disabled while a player is in cinematic mode.

---

## Floodgate

**Floodgate** is supported when present: the plugin can tell **Floodgate / Bedrock** players from Java players (e.g. for display behaviour) alongside **Geyser**.

---

## FeatherServerAPI

When **FeatherServerAPI** is present, the plugin **blocks Feather’s client-side “nametags” mod** on supported Java clients so it does not conflict with server-rendered UnlimitedNameTags.

---

## Bedrock (Geyser)

**Geyser** allows Bedrock players to see name tags, but **text displays** are not fully supported on Bedrock. Not all features (e.g. some backgrounds or shadows) match the Java client. See the note in the [wiki overview](../README.md#-supported-client-versions).

---
