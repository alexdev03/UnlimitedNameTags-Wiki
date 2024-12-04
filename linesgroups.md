---
description: LinesGroups are a way of displaying lines if a condition is met
---

# LinesGroups

Every nametag in the config contains a list of LineGroups. Each group is made by a list of lines and list of modifiers.

#### Modifiers

The modifier is system that let's you decide if a group can be displayed.

There are 2 modifiers:

*   GlobalModifier: Let's you choose if the group is displayed with an option\


    ```yaml
    modifiers:
    - type: global
      enabled: true
    ```
*   ConditionModifier: Let's you define a condition, if the condition is met then the group is displayed\


    ```yaml
    modifiers:
    - type: conditional
      parameter: '%vault_eco_balance%'
      condition: '>'
      value: '1000'
    ```



You can define multiple modifiers that work with an AND. So for example if you add 2 conditiona modifiers, both of them must be true in order to display the group.

**Note:** For string comparisons, use double quotes `" "` or single quotes `' '`. For numerical comparisons, quotes are not needed. Also if you use `'` for quotes, you need to escape them with `''`. The same applies for `"` and `""`. Example: `''%player_name%''` or `"'%player_name%'"`

```yaml
      - type: conditional
        parameter: '''%player_name%'''
        condition: '!='
        value: '''AlexDev_'''
```



Example:

<pre class="language-yaml"><code class="lang-yaml">nametags:
  default:
    permission: nametag.default
    lineGroups:
    - lines:
      - '%luckperms_prefix% %player_name% %luckperms_suffix%'
      modifiers:
      - type: global
        enabled: true
    - lines:
      - Rich Player and
      - In good health
      modifiers:
      - type: conditional
        parameter: '%vault_eco_balance%'
        condition: '>'
        value: '1000'
<strong>      - type: conditional
</strong>        parameter: '%player_health%'
        condition: '>='
        value: '20'        
    background:
      type: hex
      enabled: false
      opacity: 255
      shadowed: false
      seeThrough: false
      hex: '#ffffff'
</code></pre>

In this example if the balance of the player is higher than 1000 and the health of the player is higher or equal than 20, the 2 lines will be displayed.

