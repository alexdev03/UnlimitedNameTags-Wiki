# Getting Started

---

## Requirements

| Requirement | Notes |
|-------------|-------|
| **Paper 1.20.1+** | Spigot 1.20.2+ also works; Paper is recommended |
| **PacketEvents** | Required — download and install like any other plugin |
| **Java 21** | Required to run the plugin jar |
| **Client 1.19.4+** | Older Java clients cannot render custom nametags at all |

> **Note:** Bedrock clients (via Geyser/Floodgate) have partial support — some features like backgrounds or shadows may not match Java. See [Integrations](integrations/integrations.md).

---

## Installation

1. Download **PacketEvents** from [Modrinth](https://modrinth.com/plugin/packetevents) and drop the jar into `plugins/`.
2. Download **UnlimitedNameTags** and drop the jar into `plugins/`.
3. Start (or restart) the server. Both plugins must show a green success line in the console.
4. The plugin creates `plugins/UnlimitedNameTags/settings.yml` on first run.
5. Edit `settings.yml` to your liking, then run **`/unt reload`** to apply changes.

---

## First-Run Checklist

- [ ] Console shows `[UnlimitedNameTags] Plugin enabled` without errors
- [ ] Console shows no PacketEvents errors
- [ ] `plugins/UnlimitedNameTags/settings.yml` exists
- [ ] A Java 1.19.4+ client sees custom nametags in-game

---

## Minimal Working Config

Below is a complete, valid `settings.yml` at the current schema version. It creates one default nametag preset showing the player's name.

```yaml
configVersion: 4

behavior:
  taskInterval: 20
  displayAnimationInterval: 1
  yOffset: 0.3
  viewDistance: 60
  compactDisplayGroupStack: false
  displayGroupLineHeightBlocks: 0.25
  disableDefaultNameTag: true
  forceDisableDefaultNameTag: false
  defaultBillboard: CENTER
  format: MINIMESSAGE
  removeEmptyLines: true

visibility:
  sneakOpacity: 70
  showWhileLooking: false
  showCurrentNameTag: false
  allowPerPlayerShowOwnWhenGlobalDisabled: false
  obscuredNametagThroughWalls: false
  obscuredNametagOpacity: 55
  obscuredNametagMaxDistance: 48.0
  obscuredNametagCheckInterval: 5

performance:
  componentCaching: false
  placeholderCacheTime: 1
  enableRelationalPlaceholders: false
  placeholderUpdateRates: {}

nameTags:
  default:
    displayGroups:
      - lines:
          - text: '%player_name%'
        scale: 1.0
        yOffset: 0.0
```

> **Note:** `default` (no `permission:` field) is the fallback preset. Every player who does not match a higher-priority preset gets this one.

---

## Applying Changes

Run **`/unt reload`** in-game or from the console to reload both `settings.yml` and `advanced.yml` (if present) without restarting the server.

---

## Next Steps

| Page | What you will find |
|------|--------------------|
| [Configuration](configuration.md) | Every config option explained |
| [Display Groups](features/display-groups.md) | Text, item, and block rows |
| [Animations](features/animations.md) | Rotate, bob, pulse, and more |
| [Performance](performance.md) | Tuning for busy servers |
| [Migration Guide](migration.md) | Upgrading from an older config version |
