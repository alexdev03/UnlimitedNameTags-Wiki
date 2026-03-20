# Placeholder replacements

UnlimitedNameTags supports **placeholder replacements**: map raw PlaceholderAPI (or other) outputs to different text. Useful for:

- Localising or restyling values (dates, biomes, worlds).
- Turning boolean vanish placeholders into short labels.

---

## Configuration

Replacements are set in **`settings.yml`** under the top-level key **`placeholdersReplacements`** (camelCase, ConfigLib default).

You list entries per placeholder string; each entry has `placeholder` (exact output to match) and `replacement` (what to show instead). Order matters: first match wins.

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

## Vanish status

For `%advancedvanish_is_vanished%`, the default config maps `Yes` / `No` to styled text. Adjust the `placeholder` strings to match **exactly** what your vanish plugin returns.

---

## Catch-all row (`ELSE`)

If no row matches the resolved placeholder output, the plugin looks for a row whose `placeholder` is **`Else`** or **`ELSE`** (case-insensitive; same as the internal `ELSE` fallback in code). Use that as a default replacement for all other values.

See also the [Configuration](../configuration.md) page for the full `settings.yml` overview.
