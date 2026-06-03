# Integrations

**UnlimitedNameTags** integrates with several popular plugins to provide automatic features (such as height offsets for custom headwear) and custom visual effects.

---

## Nexo & Oraxen (Custom Helmets)

Both **Nexo** and **Oraxen** automatically adjust player name tag heights when players equip custom helmets or tall hats, preventing the name tag text from clipping into the custom models.

### Nexo Offset Rendering
With Nexo custom helmets (or custom items utilizing merged resource packs), the tag automatically tracks model height offsets.

![Nexo nametag offset](../assets/nexo-nametag-offset.gif)

### Oraxen
Similarly, Oraxen custom hats automatically offset name tag rendering heights.

> [!TIP]
> You can override or adjust these automatic height rules manually by writing custom rule sets in `advanced.yml`. (See the [advanced.yml Guide](../features/advanced-yml.md)).

---

## ItemsAdder

> [!WARNING]
> Depending on your plugin version, automatic height detection for **ItemsAdder** may not be included. Check your release notes. If automatic ItemsAdder support is unavailable in your build, you must define custom height rules in `advanced.yml` matching the specific `material`, `customModelData`, or `equippableModel` parameters.

---

## Cosmetics & Accessories (HMCCosmetics, etc.)

- **HMCCosmetics**: Height offsets are supported when used with a compatible hook source (such as
  CreativeHook or Nexo).
- **CosmeticsCore**: Direct integration is not supported. Custom helmets must be recognized as
  standard items by other supported integration systems for offsets to apply.

---

## ViaVersion

> [!WARNING]
> While **ViaVersion** allows older client versions to connect to the server and helps the plugin detect client capability profiles, it **does not** enable custom display entity rendering on Java clients older than **1.19.4**. (See the [Supported Versions Guide](../README.md)).

---

## LibsDisguises

- The plugin automatically hides custom name tags when players are disguised as other entity
  types.

---

## PlaceholderAPI

Name tags fully support standard **PlaceholderAPI** variables.

> [!IMPORTANT]
> To use viewer-dependent placeholders (such as `%rel_...%` or `%relational_...%`), you must enable `performance.enableRelationalPlaceholders: true` in `settings.yml`. (See the [Performance Tuning Guide](../performance.md)).

### Built-in `%unt_` Placeholder Expansion

| Placeholder | Description |
| :--- | :--- |
| **`%unt_phase-mm%`** | MiniMessage-style color cycling animation phase. |
| **`%unt_phase-md%`** | MineDown color cycling animation phase. |
| **`%unt_phase-mm-g%`** | MiniMessage gradient cycling animation phase. |
| **`%unt_-phase-mm%`** | Reverse MiniMessage color cycling animation phase. |
| **`%unt_-phase-md%`** | Reverse MineDown color cycling animation phase. |

> [!NOTE]
> There is no `%unt_-phase-mm-g%` placeholder. To use reverse gradient cycles, write the token **`#-phase-mm-g#`** directly within your text lines. (See the [Animations Guide](../features/animations.md)).

---

## MiniPlaceholders

- **MiniPlaceholders** is fully supported.
- Set `behavior.format` to `MINIMESSAGE` or `UNIVERSAL` for optimal formatting.
- If text components appear static or fail to update, try disabling
  `performance.componentCaching` in `settings.yml`. (See the
  [Performance Tuning Guide](../performance.md)).

---

## TypeWriter

- Custom name tags are automatically hidden during TypeWriter cinematic sequences.

---

## Floodgate

- Integrates with **Floodgate** to differentiate Bedrock Edition players from Java Edition
  players, adjusting name tag packaging logic where needed.

---

## FeatherServerAPI

- When present, the plugin disables Feather's client-side name tags on supported Java clients
  to prevent rendering conflicts.

---

## Bedrock Edition (Geyser)

> [!NOTE]
> Name tags render on Bedrock Edition clients connected via **Geyser**, but support is only partial. Rendering elements (such as background plates, drop-shadows, and multi-line stacks) may not appear identical to Java Edition clients due to Bedrock rendering limitations.
