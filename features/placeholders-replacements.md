# Placeholder replacements

Sometimes PlaceholderAPI returns ugly or long text — a biome id, a raw `Yes`/`No`, a timestamp you want prettier. **Placeholder replacements** let you say: “when the result is **exactly this**, show **that** instead.”

---

## Where it lives

In **`settings.yml`**, look for **`placeholdersReplacements`** near the top (same area as other global options — it may sit under a parent section depending on your plugin version).

For each placeholder you list one or more pairs:

- **`placeholder`** — the **exact** text to match (case handled as documented for your version)
- **`replacement`** — what players should see

**First match wins** if you list several rows.

---

## Format

```yaml
placeholdersReplacements:
  '%some_placeholder%':
    - placeholder: RawValueFromPlaceholder
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

## When the value is `Yes`, `No`, `On`, or `Off`

Some words are treated specially by YAML. If PlaceholderAPI literally returns **`Yes`**, write it with **quotes**:

```yaml
placeholder: "Yes"
```

Same idea for **`No`** matching a vanish placeholder, etc.

---

## Vanish status

The default config often maps **`Yes` / `No`** for vanish-style placeholders to short tags like `[V]`. Make sure the **`placeholder`** text matches **exactly** what your vanish plugin prints.

---

## Fallback row (`ELSE`)

Add a line whose **`placeholder`** is **`Else`** or **`ELSE`** to catch **every other** value that did not match above.

See also [Configuration](../configuration.md).
