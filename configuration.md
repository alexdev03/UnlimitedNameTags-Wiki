# **Configuration Guide for Unlimited Name Tags** ⚙️

This page explains the structure and options available in the `settings.yml` file. You can customize name tags for specific groups, apply conditional formatting, and control visual properties like scale, background, and more.

---

## **Default Configuration**

Below is the default configuration for `settings.yml`:

```yaml
nameTags:
  staffer:
    permission: nametag.staffer
    linesGroups:
      - lines:
          - '%luckperms_prefix% %player_name% %luckperms_suffix%'
        modifiers:
          - type: global
            enabled: true
    background:
      type: integer
      enabled: false
      opacity: 255
      shadowed: true
      seeThrough: false
      red: 255
      green: 0
      blue: 0
    scale: 1.0
  default:
    permission: nametag.default
    linesGroups:
      - lines:
          - '%luckperms_prefix% %player_name% %luckperms_suffix%'
        modifiers:
          - type: global
            enabled: true
      - lines:
          - Rich Player
        modifiers:
          - type: conditional
            parameter: '%vault_eco_balance%'
            condition: '>'
            value: '1000'
    background:
      type: hex
      enabled: false
      opacity: 255
      shadowed: false
      seeThrough: false
      hex: '#ffffff'
    scale: 1.0
# The default billboard constraints for the nametag. CENTER, HORIZONTAL, VERTICAL, FIXED)
defaultBillboard: CENTER
taskInterval: 20
# This is opacity that will be applied to the nametag when a player sneaks. So, the value is from -128 to 127. 
# Similar to the background, the text rendering is discarded when it is less than 26. Defaults to -1, which represents 255 and is completely opaque.
sneakOpacity: 70
yOffset: 0.30000001192092896
viewDistance: 60.0
# Which text formatter to use (MINIMESSAGE, MINEDOWN, LEGACY or UNIVERSAL) 
# Take note that UNIVERSAL is the most resource intensive but it supports all formatting options (except for MINEDOWN) 

# (&x&0&8&4&c&f&bc LEGACY OF LEGACY - &#084cfbc LEGACY - &#084cfbc& MINEDOWN - <color:#084cfbc> MINIMESSAGE)
format: MINIMESSAGE
# Whether to disable the default name tag or not.
disableDefaultNameTag: true
# This option works if disableDefaultNameTag is enabled. This will skip the team internal cache and will make all default nametags invisible. The use case is when there is a npc from a plugin that doesn't use display entities / armorstands for nametags, and there is an online player with the name of the npc.
forceDisableDefaultNameTag: false
removeEmptyLines: true
# Whether to see the NameTag of a user only while pointing at them
showWhileLooking: false
# Whether to see your own NameTag (Similar to nametag mod of Lunar Client)
showCurrentNameTag: false
# Whether to cache components for some time and reuse them or not.
#  This is useful for performance improvements where a lot of gradients are used, but it uses a bit more memory.
#  It could create problems with MiniPlaceholders, so it is disabled by default.
componentCaching: false
# How long to cache placeholders for (in ticks)
placeholderCacheTime: 1
enableRelationalPlaceholders: true
placeholdersReplacements:
  '%advancedvanish_is_vanished%':
    - placeholder: Yes
      replacement: ' &7[V]&r'
    - placeholder: No
      replacement: ''

# Authors: AlexDev_


```

## Configuration Options

Below is a description of key configuration options in the `settings.yml` file for **UnlimitedNameTags**.

---

### `nameTags:`
This top-level section is where you define different groups of name tags. Each group (e.g., `staffer`, `default` in the example) can have its own specific `permission` required to display it, `linesGroups` to define the content and conditional display of text lines, `background` settings, and `scale`. This allows for highly customizable name tags based on player roles, conditions, or other criteria.

---

### `defaultBillboard: CENTER`
This setting defines the default behavior for the name tag's positioning. The `CENTER` option places the name tag directly above the player’s head and is the most similar to the vanilla Minecraft name tag behavior. Other options include `HORIZONTAL` (follows player's head yaw), `VERTICAL` (follows player's head pitch), and `FIXED` (always faces the camera).

For more information on different billboard styles, visit the [Billboards feature documentation](features/billboards.md).

---

### `taskInterval: 20` (ticks)
This setting determines how often the plugin updates name tags, in ticks. A value of `20` means the name tags will be updated every second (since Minecraft runs at 20 ticks per second). If you want to speed up animations or make updates more frequent, you can lower this value (minimum is `1`).

---

### `sneakOpacity: 70`
The opacity level of the name tag when the player is sneaking. This value ranges from `-128` to `127`. Higher values make the name tag more opaque (visible), while lower values make it more transparent. Text rendering is discarded if the effective opacity is less than 26. A value of `-1` represents full opacity (255). The default `70` provides a semi-transparent effect while sneaking.

---

### `yOffset: 0.3`
This value adjusts the vertical offset of the name tag from the player's position. A positive value will move the name tag higher, while a negative value will move it lower. The default `0.3` (approximately) places it slightly above the player's head.

---

### `viewDistance: 60.0` (blocks)
This setting determines the maximum distance (in blocks) at which a name tag will be visible to other players. If a player moves beyond this distance, their name tag will not be displayed.

---

### `format: MINIMESSAGE`
This setting defines which text formatting system to use for name tags. The `MINIMESSAGE` format allows you to use rich text formatting features, like colors, gradients, styles, hover events, and click events. Other supported formats include:
* `MINEDOWN`: Another rich text format with its own syntax.
* `LEGACY`: Supports traditional Minecraft color codes (e.g., `&c` for red).
* `UNIVERSAL`: Attempts to support all formatting options (except for MINEDOWN's specific syntax) but is the most resource-intensive.

> **Formatting Examples:**
> * `&x&0&8&4&c&f&bcText` (LEGACY OF LEGACY - for hex colors using older ampersand style)
> * `&#084cfbcText` (LEGACY - for hex colors)
> * `&#084cfbcText` (MINEDOWN - also supports hex colors)
> * `<color:#084cfbc>Text</color>` (MINIMESSAGE - for hex colors)

---

### `disableDefaultNameTag: true`
When set to `true`, this setting disables Minecraft's default name tag display. This allows **UnlimitedNameTags** to take full control over the name tag rendering, preventing overlaps or conflicts.

---

### `forceDisableDefaultNameTag: false`
This option works in conjunction with `disableDefaultNameTag`. If set to `true` (and `disableDefaultNameTag` is also `true`), this setting will bypass Minecraft's internal team cache, forcing all default name tags to be invisible. This is particularly useful for resolving display conflicts that can occur when an NPC (Non-Player Character) from another plugin (which doesn't use display entities or armor stands for its name tags) shares the same name as an online player.

---

### `removeEmptyLines: true`
If enabled (`true`), this setting will remove any empty lines from the name tag display. This helps prevent unnecessary blank spaces and keeps the name tag looking clean, especially if some placeholder lines resolve to empty strings.

---

### `showWhileLooking: false`
This option controls whether a player's name tag will only be shown when you are looking directly at them. If set to `true`, the name tag will appear when your crosshair is on the player, potentially even if they are further away than other name tags, depending on other visibility settings.

For more details, visit the [Show While Looking feature documentation](features/show-while-looking.md).

---

### `showCurrentNameTag: false`
When set to `true`, this setting will allow a player to see their own name tag. This feature is similar to the "Show Own Name Tag" option found in clients like Lunar Client.

---

### `scale: 1.0` (within `nameTags` groups or as a global default if supported)
This setting allows you to set the scale of the name tag. This value acts as a multiplier with the player's own scale attribute (if applicable, e.g., from other plugins or effects).
**Formula**: `final_nametag_scale = player_scale_attribute * nametag_scale_setting`
For example, if you set the `scale` in a nametag group to `0.5`, and the player's scale attribute is `1.0`, the name tag will be rendered at half its normal size. If this option is also available globally (outside a `nameTags` group), it would define a default scale.

---

### `componentCaching: false`
This setting determines whether the plugin should cache formatted text components for a period and reuse them. Enabling caching (`true`) can lead to performance improvements, especially when using numerous gradients or complex formatting, but it will consume slightly more memory. It is disabled by default (`false`) as it may cause issues with dynamic placeholders, such as those provided by MiniPlaceholders, which update frequently.

---

### `placeholderCacheTime: 1` (ticks)
Specifies the duration, in ticks (where 20 ticks = 1 second), for which placeholder values are cached by the plugin. This setting is relevant if `componentCaching` is enabled or for any internal placeholder caching mechanisms the plugin employs. The default value is `1` tick, meaning placeholders are re-evaluated very frequently.

---

### `enableRelationalPlaceholders: true`
When set to `true`, this option enables the processing and use of relational placeholders (e.g., those provided by PlaceholderAPI that depend on both the viewer and the target player). Relational placeholders display information that is relative to the player viewing the name tag and the player owning the name tag (e.g., `%rel_player_health%`, distance, team relationship).

---

### `placeholdersReplacements:`
This section allows you to define custom string replacements for the output of specific placeholders. Instead of displaying the raw output of a placeholder, you can configure the plugin to show a different text or format. This is useful for shortening long outputs, adding styling, or translating boolean values into more descriptive text.

In the default configuration:
```yaml
placeholdersReplacements:
  '%advancedvanish_is_vanished%':
    - placeholder: Yes # The original string output by the placeholder
      replacement: ' &7[V]&r' # What it will be replaced with
    - placeholder: No
      replacement: '' # Replaced with an empty string (nothing)
```
Certamente. Ecco la sezione "Configuration Options" della guida, formattata in Markdown e pronta per essere copiata:

Markdown

## Configuration Options

Below is a description of key configuration options in the `settings.yml` file for **UnlimitedNameTags**.

---

### `nameTags:`
This top-level section is where you define different groups of name tags. Each group (e.g., `staffer`, `default` in the example) can have its own specific `permission` required to display it, `linesGroups` to define the content and conditional display of text lines, `background` settings, and `scale`. This allows for highly customizable name tags based on player roles, conditions, or other criteria.

---

### `defaultBillboard: CENTER`
This setting defines the default behavior for the name tag's positioning. The `CENTER` option places the name tag directly above the player’s head and is the most similar to the vanilla Minecraft name tag behavior. Other options include `HORIZONTAL` (follows player's head yaw), `VERTICAL` (follows player's head pitch), and `FIXED` (always faces the camera).

For more information on different billboard styles, visit the [Billboards feature documentation](features/billboards.md).

---

### `taskInterval: 20` (ticks)
This setting determines how often the plugin updates name tags, in ticks. A value of `20` means the name tags will be updated every second (since Minecraft runs at 20 ticks per second). If you want to speed up animations or make updates more frequent, you can lower this value (minimum is `1`).

---

### `sneakOpacity: 70`
The opacity level of the name tag when the player is sneaking. This value ranges from `-128` to `127`. Higher values make the name tag more opaque (visible), while lower values make it more transparent. Text rendering is discarded if the effective opacity is less than 26. A value of `-1` represents full opacity (255). The default `70` provides a semi-transparent effect while sneaking.

---

### `yOffset: 0.3`
This value adjusts the vertical offset of the name tag from the player's position. A positive value will move the name tag higher, while a negative value will move it lower. The default `0.3` (approximately) places it slightly above the player's head.

---

### `viewDistance: 60.0` (blocks)
This setting determines the maximum distance (in blocks) at which a name tag will be visible to other players. If a player moves beyond this distance, their name tag will not be displayed.

---

### `format: MINIMESSAGE`
This setting defines which text formatting system to use for name tags. The `MINIMESSAGE` format allows you to use rich text formatting features, like colors, gradients, styles, hover events, and click events. Other supported formats include:
* `MINEDOWN`: Another rich text format with its own syntax.
* `LEGACY`: Supports traditional Minecraft color codes (e.g., `&c` for red).
* `UNIVERSAL`: Attempts to support all formatting options (except for MINEDOWN's specific syntax) but is the most resource-intensive.

> **Formatting Examples:**
> * `&x&0&8&4&c&f&bcText` (LEGACY OF LEGACY - for hex colors using older ampersand style)
> * `&#084cfbcText` (LEGACY - for hex colors)
> * `&#084cfbcText` (MINEDOWN - also supports hex colors)
> * `<color:#084cfbc>Text</color>` (MINIMESSAGE - for hex colors)

---

### `disableDefaultNameTag: true`
When set to `true`, this setting disables Minecraft's default name tag display. This allows **UnlimitedNameTags** to take full control over the name tag rendering, preventing overlaps or conflicts.

---

### `forceDisableDefaultNameTag: false`
This option works in conjunction with `disableDefaultNameTag`. If set to `true` (and `disableDefaultNameTag` is also `true`), this setting will bypass Minecraft's internal team cache, forcing all default name tags to be invisible. This is particularly useful for resolving display conflicts that can occur when an NPC (Non-Player Character) from another plugin (which doesn't use display entities or armor stands for its name tags) shares the same name as an online player.

---

### `removeEmptyLines: true`
If enabled (`true`), this setting will remove any empty lines from the name tag display. This helps prevent unnecessary blank spaces and keeps the name tag looking clean, especially if some placeholder lines resolve to empty strings.

---

### `showWhileLooking: false`
This option controls whether a player's name tag will only be shown when you are looking directly at them. If set to `true`, the name tag will appear when your crosshair is on the player, potentially even if they are further away than other name tags, depending on other visibility settings.

For more details, visit the [Show While Looking feature documentation](features/show-while-looking.md).

---

### `showCurrentNameTag: false`
When set to `true`, this setting will allow a player to see their own name tag. This feature is similar to the "Show Own Name Tag" option found in clients like Lunar Client.

---

### `scale: 1.0` (within `nameTags` groups or as a global default if supported)
This setting allows you to set the scale of the name tag. This value acts as a multiplier with the player's own scale attribute (if applicable, e.g., from other plugins or effects).
**Formula**: `final_nametag_scale = player_scale_attribute * nametag_scale_setting`
For example, if you set the `scale` in a nametag group to `0.5`, and the player's scale attribute is `1.0`, the name tag will be rendered at half its normal size. If this option is also available globally (outside a `nameTags` group), it would define a default scale.

---

### `componentCaching: false`
This setting determines whether the plugin should cache formatted text components for a period and reuse them. Enabling caching (`true`) can lead to performance improvements, especially when using numerous gradients or complex formatting, but it will consume slightly more memory. It is disabled by default (`false`) as it may cause issues with dynamic placeholders, such as those provided by MiniPlaceholders, which update frequently.

---

### `placeholderCacheTime: 1` (ticks)
Specifies the duration, in ticks (where 20 ticks = 1 second), for which placeholder values are cached by the plugin. This setting is relevant if `componentCaching` is enabled or for any internal placeholder caching mechanisms the plugin employs. The default value is `1` tick, meaning placeholders are re-evaluated very frequently.

---

### `enableRelationalPlaceholders: true`
When set to `true`, this option enables the processing and use of relational placeholders (e.g., those provided by PlaceholderAPI that depend on both the viewer and the target player). Relational placeholders display information that is relative to the player viewing the name tag and the player owning the name tag (e.g., `%rel_player_health%`, distance, team relationship).

---

### `placeholdersReplacements:`
This section allows you to define custom string replacements for the output of specific placeholders. Instead of displaying the raw output of a placeholder, you can configure the plugin to show a different text or format. This is useful for shortening long outputs, adding styling, or translating boolean values into more descriptive text.

In the default configuration:
```yaml
placeholdersReplacements:
  '%advancedvanish_is_vanished%':
    - placeholder: Yes # The original string output by the placeholder
      replacement: ' &7[V]&r' # What it will be replaced with
    - placeholder: No
      replacement: '' # Replaced with an empty string (nothing)
```
If the placeholder %advancedvanish_is_vanished% (from the AdvancedVanish plugin, for example) returns the string "Yes", UnlimitedNameTags will replace it with " &7[V]&amp;r" (a grey [V] tag). If it returns "No", it will be replaced with an empty string, effectively hiding it. You can define multiple replacements for different placeholders.
For more information on how to use placeholders, visit the [Placeholder Replacements feature documentation](features/placeholders-replacements.md).
