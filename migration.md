# Migration Guide

> [!NOTE]
> This is the documentation for **UnlimitedNameTags v2.x (current)**. If you are using the legacy version 1.x, [click here to view the v1 Wiki](v1/README.md).

This document provides step-by-step instructions for upgrading your configuration format to **UnlimitedNameTags v2.x** (`configVersion: 7`).

---

## Schema Version History

| `configVersion` | Description of Changes |
| :--- | :--- |
| **1** (Legacy) | Flat layout configuration. Properties like `lines`, `background`, `scale`, and `yOffset` were defined directly under each `nameTags` entry. |
| **2** | Introduced the `displayGroups` list. The `lines` configuration was defined as a simple string list (e.g., `- 'text'`). |
| **3** | Updated the `lines` list format to structured objects, supporting conditional parameters (e.g., `- {text: 'text', when: 'condition'}`). |
| **4** | Grouped global variables into `behavior`, `visibility`, and `performance` sections. Standardized the `background` block by unifying background parameters under a single `color` option. |
| **5** | Replaced `obscuredNametagThroughWalls` and related parameters with `throughWallMode` (`SEE_THROUGH`, `OBSCURED`, `HIDE`) and nested `throughWallSettings`. |
| **6** | Introduced `glowAnimations` presets and optional per-display-group `glow` / `glowInterval` fields. |
| **7** (Current) | Includes `performance.distanceRefreshCulling` for distance-aware refresh intervals. |

---

## Automatic Migration

The plugin executes an automatic migration routine whenever it starts or when `/unt reload` is triggered. The migration process:

1. Generates a backup copy of your configuration at `plugins/UnlimitedNameTags/settings.yml.backup-<timestamp>.yml` prior to performing any modifications.
2. Restructures flat v1 parameters into the modern `displayGroups` layout.
3. Converts flat string list entries under `lines` into standard `{text: '...'}` objects.
4. Translates old background structures (`type: integer` or `type: hex`) into the unified `color:` format.
5. Restructures top-level global settings under their respective `behavior`, `visibility`, or `performance` categories.
6. Converts `obscuredNametagThroughWalls` and related settings to `throughWallMode` and nested `throughWallSettings` (v5).
7. Adds default `glowAnimations` presets when upgrading to v6.
8. Ensures the default `performance.distanceRefreshCulling` block exists when upgrading to v7.
9. Sets `configVersion: 7` in the rewritten configuration file.

Under normal circumstances, restarting the server or executing `/unt reload` is sufficient to complete the migration.

---

## Configuration Aspects Requiring Manual Review

### Removal of `modifiers`

> [!WARNING]
> The automatic migration utility **deletes** the legacy `modifiers:` block. The old multi-condition parameter structure cannot be automatically translated due to changes in syntax. You must manually rewrite these rules using the new `when:` condition string.

{% tabs %}
{% tab title="Before (Legacy v1/v2)" %}
```yaml
modifiers:
  - type: conditional
    parameter: "%vault_eco_balance%"
    condition: ">"
    value: "1000"
```
{% endtab %}
{% tab title="After (v4+)" %}
```yaml
when: '%vault_eco_balance% > 1000'
```
{% endtab %}
{% endtabs %}

- **Group-level conditional**: Place the `when:` key directly under the display group entry to
  show/hide the entire line row.
- **Line-level conditional**: Place the `when:` key inside the `lines` list entry to show/hide
  only that specific line of text.

---

### Removal of Background `type` Field

The parameters `type: integer` and `type: hex` are no longer supported.

{% tabs %}
{% tab title="Before (integer)" %}
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
{% endtab %}
{% tab title="Before (hex)" %}
```yaml
background:
  type: hex
  enabled: true
  hex: '#ff0000'
  opacity: 200
  shadowed: false
  seeThrough: false
```
{% endtab %}
{% tab title="After (v4)" %}
```yaml
background:
  enabled: true
  color: '#ff0000'      # Defined as a hex string OR "255,0,0" for RGB format
  opacity: 200
  shadowed: false
  seeThrough: false
```
{% endtab %}
{% endtabs %}

The built-in migrator automatically standardizes these structures upon initialization.

---

### Migration of Through-Wall Settings to `throughWallMode` (v5)

The old visibility parameters `obscuredNametagThroughWalls`, `obscuredNametagOpacity`, `obscuredNametagMaxDistance`, and `obscuredNametagCheckInterval` have been unified into `throughWallMode` and a nested `throughWallSettings` map.

{% tabs %}
{% tab title="Before (v4)" %}
```yaml
visibility:
  obscuredNametagThroughWalls: false # or true
  obscuredNametagOpacity: 55
  obscuredNametagMaxDistance: 48.0
  obscuredNametagCheckInterval: 5
```
{% endtab %}
{% tab title="After (v5)" %}
```yaml
visibility:
  throughWallMode: SEE_THROUGH # Becomes OBSCURED if obscuredNametagThroughWalls was true
  throughWallSettings:
    opacity: 55
    maxDistance: 48.0
    checkInterval: 5
```
{% endtab %}
{% endtabs %}

The built-in migrator automatically performs this conversion.

---

### Conversion of `lines` to Objects

{% tabs %}
{% tab title="Before (Plain String)" %}
```yaml
lines:
  - '%luckperms_prefix% %player_name%'
  - '%player_ping%ms'
```
{% endtab %}
{% tab title="After (v4 Object)" %}
```yaml
lines:
  - text: '%luckperms_prefix% %player_name%'
  - text: '%player_ping%ms'
    when: '%player_ping% > 0'   # Optional per-line visibility check
```
{% endtab %}
{% endtabs %}

---

### Legacy `linesGroups` renamed to `displayGroups`

The legacy internal setting `linesGroups` is automatically renamed to `displayGroups` by the migrator.

---

### Migration to Glow Presets (v6)

Schema version 6 adds a root-level **`glowAnimations`** map with default presets (`rainbow`,
`gradient`, `gold_pulse`). The migrator inserts these automatically if the section is missing.

Per-row glow is optional and does not change existing layouts. To use glow, add a `glow:` block
under an **`ITEM`** or **`BLOCK`** `displayGroups` entry (glow has no effect on **`TEXT`** rows).
See the [Glow Guide](features/glow.md).

---

### API: `setNametagLines` Deprecated

> [!WARNING]
> **`UNTPaperAPI.setNametagLines(Player, List<Settings.DisplayGroup>)`** is **deprecated** (since
> 2.0.0, marked for removal). Replace all calls with
> **`setNametagDisplayGroups(Player, List<Settings.DisplayGroup>)`**. See the
> [Developer API Guide](api/README.md).

---

## Verifying the Migration

Confirm the migration completed successfully by following these steps:

1. Execute the `/unt reload` command and monitor the server console for any configuration loading or parsing errors.
2. Run `/unt debug` to inspect a snapshot of active player name tag metadata.
3. Check the `plugins/UnlimitedNameTags/` directory to ensure that a `settings.yml.backup-*.yml` file was created.
4. Join the server using a Minecraft Java 1.19.4+ client and verify that custom name tags render correctly.
