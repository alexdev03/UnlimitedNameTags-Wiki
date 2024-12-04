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
      enabled: true
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
defaultBillboard: CENTER
taskInterval: 20
sneakOpacity: 70
yOffset: 0.30000001192092896
viewDistance: 60.0
format: MINIMESSAGE
disableDefaultNameTag: true
removeEmptyLines: true
showWhileLooking: false
showCurrentNameTag: false
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

### `defaultBillboard: CENTER`
This setting defines the default behavior for the name tag's positioning. The `CENTER` option places the name tag directly above the player’s head and is the most similar to the vanilla Minecraft name tag behavior.

For more information on different billboard styles, visit the [Billboards feature documentation](features/billboards.md).

---

### `taskInterval: 20`
This setting determines how often the plugin updates name tags, in ticks. A value of `20` means the name tags will be updated every second (since Minecraft runs at 20 ticks per second).
If you want to speed up animations you can lower this value, min is 1.

---

### `sneakOpacity: 70`
The opacity level of the name tag when the player is sneaking. This value ranges from -128 to 127, where higher values make the name tag more visible, and lower values make it more transparent.

---

### `yOffset: 0.3`
This value adjusts the vertical offset of the name tag from the player's position. A positive value will move the name tag higher, while a negative value will move it lower.

---

### `viewDistance: 60.0`
This setting determines the maximum distance at which a name tag will be visible to other players. If a player moves beyond this distance, their name tag will not be displayed.

---

### `format: MINIMESSAGE`
This setting defines which text formatting system to use for name tags. The `MINIMESSAGE` format allows you to use rich text formatting features, like colors and styles. Other supported formats include `MINEDOWN`, `LEGACY`, and `UNIVERSAL`.
> **Note**: Take note that UNIVERSAL is the most resource intensive but it supports all formatting options (except for MINEDOWN)
> 
(&x&0&8&4&c&f&bc LEGACY OF LEGACY - &#084cfbc LEGACY - &#084cfbc& MINEDOWN - <color:#084cfbc> MINIMESSAGE)

---

### `disableDefaultNameTag: true`
When set to `true`, this setting disables Minecraft's default name tag display. This allows **UnlimitedNameTags** to take full control over the name tag rendering.

---

### `removeEmptyLines: true`
If enabled, this setting will remove any empty lines from the name tag. This helps prevent unnecessary blank lines in the name tag display.

---

### `showWhileLooking: false`
This option controls whether the name tag will be shown when you are looking directly at the player. If set to `true`, the name tag will appear while you're looking at the player, even if you are not close to them.

For more details, visit the [Show While Looking feature documentation](features/show-while-looking.md).

---

### `showCurrentNameTag: false`
When set to `true`, this setting will allow the a player to see their own name tag while they are looking at another player. This features is similar to Lunar Client's "Show Current Name Tag" feature.

---

### `scale: 1.0`
This setting allows you to set the default scale of the name tag. This works with the scale attribute of the player. 
For example, if you set the scale to `0.5`, and the scale attribute of the player is `1.0`, the name tag will be half the size of normal.

**Formula**: `scale = player_scale * nametag_scale`



