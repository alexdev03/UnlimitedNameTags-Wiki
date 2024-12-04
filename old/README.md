# 🏷️ Nametags

### Usage

In order to use this plugin you need

* [PacketEvents](https://github.com/Retrooper/PacketEvents)
* Spigot 1.20.2 or higher
* Paper 1.19.4 or higher

Then you can edit settings.yml to customize the plugin according to your needs

{% code title="settings.yml" %}
```yaml
nameTags:
  staffer:
    permission: nametag.staffer
    lineGroups:
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
  default:
    permission: nametag.default
    lineGroups:
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
disableDefaultNameTag: false
removeEmptyLines: true
# Whether to see the NameTag of a user when pointing at them.
showWhileLooking: false

# Authors: AlexDev_
```
{% endcode %}

### Lines

You can use PlaceholderAPI's placeholders wherever you want

You can use legacy formatting, Minimessage and even Minedown!

To "animate" your tags you can use Minedown or Minimessage gradient tags like&#x20;

<pre class="language-xml"><code class="lang-xml"><strong>&#x3C;!--MiniMessage https://docs.advntr.dev/minimessage/format.html#gradient -->
</strong><strong>&#x3C;gradient:#5e4fa2:#f79459:#phase-mm-g#> ||||| &#x3C;/gradient>
</strong><strong>&#x3C;rainbow:#phase-mm#>Test Rainbow&#x3C;/rainbow>
</strong></code></pre>

\#phase-md# for MineDown)

&#x20;\#phase-mm-g# for MiniMessage used by Gradient and Transition tags

\#phase-mm# for MiniMessage used by Rainbow tags



### [LinesGroups](linesgroups.md)

###

### Background

#### enable

true -> trasparent background

false -> translucent background

#### seeThrough

true if you want to see nametags even behind blocks



### Billboards

{% content-ref url="billboards.md" %}
[billboards.md](billboards.md)
{% endcontent-ref %}



### Limitations

NPC Plugins that use default nametag won't work, it's striclty advised to use a plugin that has an hologram mode for npcs:

* [Citizens ](https://wiki.citizensnpcs.co/Frequently_Asked_Questions#My_NPCs_Have_Names_Like_CIT-abc_Instead_Of_Their_Actual_Names)
* [FancyNPCS](https://fancyplugins.de/docs/fn-multiple-lines.html)
* [ZNPCs](https://github.com/gonalez/znpcs/wiki/commands-permissions#lines)
* There are problems with nametag visibility between blocks, this problem should be fixed by Mojang
