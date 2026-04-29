# Migration Guide

This page covers upgrading an existing UnlimitedNameTags config to v2.0.0 (schema **`configVersion: 4`**).

---

## Schema Version History

| `configVersion` | What changed |
|-----------------|-------------|
| **1** (legacy) | Flat layout — `lines`, `background`, `scale`, `yOffset` directly on each `nameTags` entry |
| **2** | `displayGroups` list introduced; `lines` values were plain strings (`- 'text'`) |
| **3** | `lines` changed to structured objects (`- {text: 'text', when: 'condition'}`) |
| **4** (current) | `behavior` / `visibility` / `performance` sections introduced; `Background` unified `color:` field (no `type:` discriminator) |

---

## Automatic Migration

The plugin runs its migrator on every load and `/unt reload`. It:

- Creates a backup at `plugins/UnlimitedNameTags/settings.yml.backup-<timestamp>.yml` **before** any rewrite.
- Promotes flat v1 keys into `displayGroups`.
- Converts string `lines` entries to `{text: '…'}` objects.
- Converts old `type: integer` / `type: hex` Background entries to the unified `color:` format.
- Moves top-level keys (`taskInterval`, `sneakOpacity`, etc.) under the correct section heading.
- Sets `configVersion: 4` in the output.

In most cases you just restart the server or run `/unt reload` — the migration is fully automatic.

---

## What Requires Manual Attention

### `modifiers:` Removed

Old configs used a `modifiers:` list with `type: conditional` entries for per-row conditions. The migrator **strips** these rather than migrating them, because the new `when:` string is not equivalent to the old multi-condition list.

**Before (v1/v2 style):**
```yaml
modifiers:
  - type: conditional
    parameter: "%vault_eco_balance%"
    condition: ">"
    value: "1000"
```

**After (v2.0.0):**
```yaml
when: '%vault_eco_balance% > 1000'
```

Place the `when:` field at the `displayGroup` level to hide the whole row, or at the `lines` entry level to hide just that line.

---

### Background `type:` Field Removed

Old backgrounds used `type: integer` (RGB components) or `type: hex` (hex string). Both are gone.

**Before (`type: integer`):**
```yaml
background:
  type: integer
  enabled: true
  red: 255
  green: 0
  blue: 0
  opacity: 200
  shadowed: false
  seeThrough: false
```

**Before (`type: hex`):**
```yaml
background:
  type: hex
  enabled: true
  hex: '#ff0000'
  opacity: 200
  shadowed: false
  seeThrough: false
```

**After (v4):**
```yaml
background:
  enabled: true
  color: '#ff0000'      # hex, OR use "255,0,0" for R,G,B
  opacity: 200
  shadowed: false
  seeThrough: false
```

The migrator converts both old forms automatically on load.

---

### `lines` is Now a List of Objects

**Before (v2 string lines):**
```yaml
lines:
  - '%luckperms_prefix% %player_name%'
  - '%player_ping%ms'
```

**After (v4):**
```yaml
lines:
  - text: '%luckperms_prefix% %player_name%'
  - text: '%player_ping%ms'
    when: '%player_ping% > 0'   # optional per-line condition
```

The migrator converts plain string entries to `{text: '…'}` objects automatically.

---

### `linesGroups` → `displayGroups`

The old internal key `linesGroups` is renamed to `displayGroups` automatically by the migrator.

---

### API: `setNametagLines` Deprecated

`UNTAPI.setNametagLines(Player, List<Settings.DisplayGroup>)` still compiles in v2.0.0 but is deprecated and will be removed in a future version. Replace all calls with `setNametagDisplayGroups(Player, List<Settings.DisplayGroup>)`. See [Developer API](api.md).

---

## Checking Migration Success

1. Run `/unt reload` and watch the console — no migration errors should appear.
2. Run `/unt debug` to print a diagnostic snapshot of the current nametag state.
3. Check `plugins/UnlimitedNameTags/` for a `settings.yml.backup-*.yml` file — its presence confirms the migrator ran.
4. Log into the server with a 1.19.4+ client and verify nametags render correctly.
