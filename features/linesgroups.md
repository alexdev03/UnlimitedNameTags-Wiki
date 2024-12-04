---
description: LinesGroups are a way of displaying lines if a condition is met
---

# LinesGroups

Every nametag in the config contains a list of `LineGroups`. Each group is made by a list of lines and a list of modifiers.

#### Modifiers

The modifier system lets you decide if a group can be displayed.

There are 2 types of modifiers:

- **GlobalModifier**: Lets you choose if the group is always displayed.

```yaml
  modifiers:
    - type: global
      enabled: true
```

- **ConditionModifier**: Lets you define a condition. If the condition is met, the group will be displayed.

```yaml
  modifiers:
    - type: conditional
      parameter: '%vault_eco_balance%'
      condition: '>'
      value: '1000'
```

You can define multiple modifiers that work with an **AND** condition. For example, if you add two conditional modifiers, both of them must be true for the group to be displayed.

> **Note:** For string comparisons, use double quotes `" "` or single quotes `' '`. For numerical comparisons, quotes are not needed. If you use single quotes `'`, you need to escape them with `''`. The same applies for double quotes `"`, which should be escaped with `""`.

Example for escaping quotes:
```yaml
- type: conditional
  parameter: '''%player_name%'''
  condition: '!='
  value: '''AlexDev_'''
```

---

## Example Configuration

Here is an example of how to configure `LineGroups` with modifiers in your `settings.yml` file:

```yaml
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
```

### Example with Conditional Modifiers

```yaml
  default:
    permission: nametag.default
    linesGroups:
      - lines:
          - '%player_name%'
        modifiers:
          - type: global
            enabled: true
      - lines:
          - "Town: %town%"
        modifiers:
          - type: conditional
            parameter: '%town%'
            condition: '!='
            value: ""
```

In this example, if the player has a town, the town name will be displayed, otherwise the line will be hidden.
Example with 2 players:

Steve has a town, and Alex doesn't.

Steve's nametag:

```
Steve
Town: Town1
```

Alex's nametag:

```
Alex
```