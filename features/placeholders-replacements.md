# Placeholder replacements

UnlimitedNameTags supports **placeholder replacements**: map raw PlaceholderAPI (or other) outputs to different text. Useful for:

- Localising or restyling values (dates, biomes, worlds).
- Turning boolean vanish placeholders into short labels.

---

## Configuration

In **`settings.yml`**, top-level key **`placeholdersReplacements`** (camelCase, ConfigLib).

For each placeholder name you list entries with `placeholder` (**exact** string to match) and `replacement` (text to show). **Order:** first match wins.

---

## Format

```yaml
placeholdersReplacements:
  '%some_placeholder%':
    - placeholder: RawValueFromPAPI
      replacement: '<green>Nicer text</green>'
    - placeholder: OtherValue
      replacement: '<red>…</red>'
```

---

## Example

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

## YAML reserved words

In YAML 1.1, `Yes` / `No` / `On` / `Off` may parse as booleans. If the placeholder output is **literally** `Yes`, use **quotes**:

```yaml
placeholder: "Yes"
```

## Vanish status

For `%advancedvanish_is_vanished%`, the default config maps `Yes` / `No` to styled text. Align `placeholder` strings with **exactly** what your vanish plugin returns.

---

## Catch-all row (`ELSE`)

If no row matches the resolved output, the plugin looks for a row whose `placeholder` is **`Else`** or **`ELSE`** (case-insensitive). Use that as a fallback for all other values.

See also [Configuration](../configuration.md).
