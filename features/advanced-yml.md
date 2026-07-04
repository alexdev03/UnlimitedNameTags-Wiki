# Advanced (advanced.yml)

The optional file `plugins/UnlimitedNameTags/advanced.yml` allows you to define custom height offset rules for specific headwear or helmets (e.g., custom models, player heads, resource pack cosmetics).

> \[!NOTE] The plugin does not automatically generate this file. If the file is not present, no custom height rules will be applied.

Modifications to this file can be applied instantly using the `/unt reload` command. This file is useful when automatic height detection (provided by integrations like Nexo or Oraxen) is unavailable, disabled, or requires manual overrides. (See the [Integrations Guide](../integrations/integrations.md)).

***

## How Height Rules Work

* Rules are defined under the `helmetHeightRules` list.
* Each rule specifies a `height` value representing the vertical offset.
* The `priority` field determines evaluation order (higher priority rules are evaluated first). The first rule that matches the equipped helmet is applied.
* Rules defined in `advanced.yml` override heights supplied by Nexo, Oraxen, ItemsAdder, or HMCCosmetics when they return a height value greater than zero.

***

## Configuration Key Syntax

> \[!WARNING] Configuration keys in `advanced.yml` are case-sensitive and must be written in camelCase (e.g., use `customModelData`, not `custom-model-data`). Ensure you match the capitalization shown in the template exactly.

### Global Settings

The following global parameters can be configured to debug or customize scaling math:

* **`helmetRulesDebug`** (boolean, default: `false`): Enables verbose console logs when evaluating rules for players (throttled).
* **`helmetRulesDebugCooldownMs`** (number, default: `5000`): Minimum cooldown interval (in milliseconds) between debug logs per player.
* **`helmetHeightYOffsetMultiplier`** (number, default: `0.017857143`): Multiplier used to convert rule height values into Minecraft coordinate system blocks. The default conversion is `0.25 / 14`.

### Rule Example

```yaml
# Optional debugging / scaling config:
helmetRulesDebug: false
helmetRulesDebugCooldownMs: 5000
helmetHeightYOffsetMultiplier: 0.017857143

helmetHeightRules:
  - priority: 10
    height: 28
    material: PLAYER_HEAD
    customModelData: 12345

  - priority: 5
    height: 20
    material: LEATHER_HELMET
    customModelDataMin: 1000
    customModelDataMax: 1999

  - priority: 9
    height: 24
    itemModel: "nexo:my_custom_helmet"

  - priority: 8
    height: 32
    equippableModel: "mypack:item/tall_hat"

  # Optional on any rule:
  # worlds: [world, world_nether]
  # permission: "myserver.bighelmet"
```

A complete configuration template is available in [`reference/advanced.example.yml`](https://github.com/alexdev03/UnlimitedNameTags-Wiki/blob/main/reference/advanced.example.yml).

***

## Rule Fields Reference

| Field                                           | Required             | Description                                                                                                  |
| ----------------------------------------------- | -------------------- | ------------------------------------------------------------------------------------------------------------ |
| **`priority`**                                  | No (Defaults to `0`) | Higher priorities are evaluated first.                                                                       |
| **`height`**                                    | Yes (Must be `> 0`)  | The vertical offset value applied to the tag.                                                                |
| **`material`**                                  | No\*                 | The item material type (e.g., `PLAYER_HEAD`).                                                                |
| **`customModelData`**                           | No\*                 | The exact custom model data integer value.                                                                   |
| **`customModelDataMin` / `customModelDataMax`** | No\*                 | An inclusive range of custom model data integers. If used, `customModelData` is ignored.                     |
| **`itemModel`**                                 | No\*                 | The namespace ID (e.g., `nexo:my_custom_helmet`) for the Minecraft 1.20.5+ `minecraft:item_model` component. |
| **`equippableModel`**                           | No\*                 | The namespace ID for the Minecraft 1.21.3+ equippable model component.                                       |
| **`worlds`**                                    | No                   | A list of world names where the rule applies.                                                                |
| **`permission`**                                | No                   | The permission node required for the rule to apply to the player.                                            |

> \[!IMPORTANT] A rule must define at least **one** matching criteria (`material`, `customModelData`, the custom model data range, `itemModel`, or `equippableModel`) to be valid. Otherwise, the rule will never match.

***

## Matching Logic

For a rule to apply, all defined criteria must evaluate to true:

1. **Permission**: Checked if defined.
2. **Worlds**: Checked if defined.
3. **Material**: Checked if defined.
4. **Equippable Model**: Checked if defined.
5. **Item Model**: Checked if defined.
6. **Model Data**: Checks either the exact `customModelData` or the inclusive range `customModelDataMin` to `customModelDataMax`.

* A rule containing only a `material` definition matches any item stack of that material type.

***

## Error Handling & Troubleshooting

* **File Missing**: No height offsets are applied through this system.
* **Invalid File on Startup**: The plugin console logs the configuration loading error; no rules are loaded.
* **Invalid File on Reload**: The plugin logs the error, but keeps the **previously parsed valid rules** in memory until the configuration is fixed.
* **Malformed Rules**: Individual rules with syntax errors are skipped and logged; other valid rules load normally.

***

## See Also

* [Configuration Guide (`settings.yml`)](../configuration.md)
* [Integrations Guide (Nexo, Oraxen, ItemsAdder)](../integrations/integrations.md)
