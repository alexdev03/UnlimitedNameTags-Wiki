# Placeholder Replacements

Placeholder replacements allow you to map and format raw or unformatted string outputs returned by PlaceholderAPI (e.g., biome IDs, timestamps, or raw `Yes`/`No` outputs) to cleaner, styled visual tags.

---

## Configuration Layout

In your `settings.yml`, replacement rules are defined under the `placeholdersReplacements` section:

* **`placeholder`**: The exact string returned by the PlaceholderAPI variable to match.
* **`replacement`**: The formatted replacement string (supports formatting engines like MiniMessage) to display on the player name tag.

The plugin evaluates replacement entries sequentially from top to bottom. The first match that satisfies the rule is applied.

---

## Configuration Syntax

```yaml
placeholdersReplacements:
  '%some_placeholder%':
    - placeholder: RawValueFromPlaceholder
      replacement: '<green>Nicer text</green>'
    - placeholder: OtherValue
      replacement: '<red>…</red>'
```

---

## Practical Example

This example translates default world type names into styled, colored equivalents:

```yaml
placeholdersReplacements:
  '%player_world_type%':
    - placeholder: Overworld
      replacement: '<aqua>Overworld</aqua>'
    - placeholder: Nether
      replacement: '<red>Nether</red>'
    - placeholder: End
      replacement: '<yellow>End</yellow>'
```

---

## Matching YAML Keywords (Booleans)

> [!WARNING]
> The YAML 1.1 specification interprets unquoted keywords such as `Yes`, `No`, `On`, `Off`, `True`, and `False` as booleans. If a placeholder returns one of these raw strings, **you must enclose the value in quotation marks** in your configuration.

```yaml
placeholder: "Yes"
```

This applies to all boolean matching scenarios, such as mapping vanish status or toggle modes.

---

## Fallback Rule (`ELSE`)

> [!NOTE]
> You can define a fallback rule using the keyword `ELSE` (or `Else`) in the `placeholder` field. This rule acts as a catch-all for any output that does not match the prior specified rules.

```yaml
placeholdersReplacements:
  '%some_placeholder%':
    - placeholder: SpecificValue
      replacement: '<green>Matched Specific Value</green>'
    - placeholder: ELSE
      replacement: '<gray>%some_placeholder%</gray>' # Renders the raw value as fallback
```

---

## See Also

* [Configuration Guide (`settings.yml`)](../configuration.md)
* [Performance Tuning Guide](../performance.md)
