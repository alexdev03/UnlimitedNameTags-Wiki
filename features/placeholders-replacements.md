# Placeholder Replacements in UnlimitedNameTags

UnlimitedNameTags supports **placeholder replacements**, which allow you to replace a placeholder with a different value. This is useful for tasks such as:

- Changing a date placeholder to a localized version.
- Modifying biome placeholders to display text in colors.
- Using vanish placeholders to show a player's vanish status when the placeholder returns a boolean (`true/false`).

## Configuring Placeholder Replacements

Placeholder replacements are configured in the `placeholder_replacements` section of each **Tab Group**. You can specify a list of replacements for a particular placeholder, and the replacements will be applied in the order they are listed.

### Replacements Format

The replacements are defined as a list of objects, each with two properties:
- `placeholder`: The placeholder to replace.
- `replacement`: The replacement text.

### Example Configuration

```yaml
placeholder_replacements:
  '%localtime_timezone_en_US,EEEE%':
    - placeholder: Monday
      replacement: '<red>Monday</red>'
    - placeholder: Tuesday
      replacement: '<gold>Tuesday</gold>'
    - placeholder: Else
      replacement: '<green>Other day</green>'
  '%player_world_type%':
    - placeholder: Overworld
      replacement: '<aqua>Overworld</aqua>'
    - placeholder: Nether
      replacement: '<red>Nether</red>'
    - placeholder: End
      replacement: '<yellow>End</yellow>'
  '%player_biome%':
    - placeholder: PLAINS
      replacement: '<red>Plains</red>'
    - placeholder: DESERT
      replacement: '<yellow>Desert</yellow>'
    - placeholder: RIVER
      replacement: '<aqua>River</aqua>'
```

## Specified Cases

### Vanish Status

If you want to display a player's vanish status, you can use the `%advancedvanish_is_vanished%` placeholder. This placeholder returns a boolean value, so you can use it to show the player's vanish status in the tab list or any other area where placeholders are supported.

### Example Vanish Status Replacement

```yaml
placeholder_replacements:
  '%advancedvanish_is_vanished%':
    - placeholder: 'true'
      replacement: <gray>VANISHED</gray>
    - placeholder: 'false'
      replacement: <green>Visible</green>
```

This configuration allows you to display the appropriate status based on whether the player is vanished or not.

### Else Clause

The **Else clause** is used to define a default replacement when no specific placeholder matches. It’s a catch-all option for any placeholder that doesn't have a defined replacement.

### Example:

```yaml
placeholder_replacements:
  '%localtime_timezone_en_US,EEEE%':
    - placeholder: Monday
      replacement: <red>Monday</red>
    - placeholder: Tuesday
      replacement: <gold>Tuesday</gold>
    - placeholder: Else
      replacement: <green>Other day</green>
```
