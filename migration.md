# Migration Guide

> [!NOTE]
> This is the documentation for **UnlimitedNameTags v2.x (current)**. If you are using the legacy version 1.x, [click here to view the v1 Wiki](v1/README.md).

This document provides step-by-step instructions for upgrading your configuration format to **UnlimitedNameTags v2.x** (`configVersion: 5`).

---

## Schema Version History

| `configVersion` | Description of Changes |
| :--- | :--- |
| **1** (Legacy) | Flat layout configuration. Properties like `lines`, `background`, `scale`, and `yOffset` were defined directly under each `nameTags` entry. |
| **2** | Introduced the `displayGroups` list. The `lines` configuration was defined as a simple string list (e.g., `- 'text'`). |
| **3** | Updated the `lines` list format to structured objects, supporting conditional parameters (e.g., `- {text: 'text', when: 'condition'}`). |
| **4** | Grouped global variables into `behavior`, `visibility`, and `performance` sections. Standardized the `background` block by unifying background parameters under a single `color` option. |
| **5** (Current) | Replaced `obscuredNametagThroughWalls` and related parameters with `throughWallMode` (`SEE_THROUGH`, `OBSCURED`, `HIDE`) and nested `throughWallSettings`. |

---

## Automatic Migration

The plugin executes an automatic migration routine whenever it starts or when `/unt reload` is triggered. The migration process:

1. Generates a backup copy of your configuration at `plugins/UnlimitedNameTags/settings.yml.backup-<timestamp>.yml` prior to performing any modifications.
2. Restructures flat v1 parameters into the modern `displayGroups` layout.
3. Converts flat string list entries under `lines` into standard `{text: '...'}` objects.
4. Translates old background structures (`type: integer` or `type: hex`) into the unified `color:` format.
5. Restructures top-level global settings under their respective `behavior`, `visibility`, or `performance` categories.
6. Converts `obscuredNametagThroughWalls` and related settings to the unified `throughWallMode` and nested `throughWallSettings` layout.
7. Sets `configVersion: 5` in the rewritten configuration file.

Under normal circumstances, restarting the server or executing `/unt reload` is sufficient to complete the migration.

---

## Configuration Aspects Requiring Manual Review

### Removal of `modifiers`

> [!WARNING]
> The automatic migration utility **deletes** the legacy `modifiers:` block. The old multi-condition parameter structure cannot be automatically translated due to changes in syntax. You must manually rewrite these rules using the new `when:` condition string.

**Before (Legacy v1/v2 format):**
```yaml
modifiers:
  - type: conditional
    parameter: "%vault_eco_balance%"
    condition: ">"
    value: "1000"
```

**After (v2.0.0+ / configVersion 4+ format):**
```yaml
when: '%vault_eco_balance% > 1000'
```

- **Group-level conditional**: Place the `when:` key directly under the display group entry to
  show/hide the entire line row.
- **Line-level conditional**: Place the `when:` key inside the `lines` list entry to show/hide
  only that specific line of text.

---

### Removal of Background `type` Field

The parameters `type: integer` and `type: hex` are no longer supported.

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
  color: '#ff0000'      # Defined as a hex string OR "255,0,0" for RGB format
  opacity: 200
  shadowed: false
  seeThrough: false
```

The built-in migrator automatically standardizes these structures upon initialization.

---

### Migration of Through-Wall Settings to `throughWallMode` (v5)

The old visibility parameters `obscuredNametagThroughWalls`, `obscuredNametagOpacity`, `obscuredNametagMaxDistance`, and `obscuredNametagCheckInterval` have been unified into `throughWallMode` and a nested `throughWallSettings` map.

**Before (v4):**
```yaml
visibility:
  obscuredNametagThroughWalls: false # or true
  obscuredNametagOpacity: 55
  obscuredNametagMaxDistance: 48.0
  obscuredNametagCheckInterval: 5
```

**After (v5):**
```yaml
visibility:
  throughWallMode: SEE_THROUGH # Becomes OBSCURED if obscuredNametagThroughWalls was true
  throughWallSettings:
    opacity: 55
    maxDistance: 48.0
    checkInterval: 5
```

The built-in migrator automatically performs this conversion.

---

### Conversion of `lines` to Objects

**Before (Plain String Format):**
```yaml
lines:
  - '%luckperms_prefix% %player_name%'
  - '%player_ping%ms'
```

**After (v4 Object Format):**
```yaml
lines:
  - text: '%luckperms_prefix% %player_name%'
  - text: '%player_ping%ms'
    when: '%player_ping% > 0'   # Optional per-line visibility check
```

---

### Legacy `linesGroups` renamed to `displayGroups`

The legacy internal setting `linesGroups` is automatically renamed to `displayGroups` by the migrator.

---

### API Method Removal: `setNametagLines`

> [!WARNING]
> The deprecated API method `UNTAPI.setNametagLines(Player, List<Settings.DisplayGroup>)` has been **completely removed** as of configuration schema version 5 (v2.1.0+).
> 
> Developers must replace all instances of this call with `setNametagDisplayGroups(Player, List<Settings.DisplayGroup>)`. For more information, refer to the [Developer API Guide](api.md).

---

## Verifying the Migration

Confirm the migration completed successfully by following these steps:

1. Execute the `/unt reload` command and monitor the server console for any configuration loading or parsing errors.
2. Run `/unt debug` to inspect a snapshot of active player name tag metadata.
3. Check the `plugins/UnlimitedNameTags/` directory to ensure that a `settings.yml.backup-*.yml` file was created.
4. Join the server using a Minecraft Java 1.19.4+ client and verify that custom name tags render correctly.
