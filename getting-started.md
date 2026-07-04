# Getting Started

This page covers the requirements, installation steps, and basic setup needed to get **UnlimitedNameTags** running on your server.

---

## Requirements

### Server Requirements
| Requirement | Description |
| :--- | :--- |
| **Server Software** | **Paper 1.21.4+** (Highly recommended for optimal packet handling) |
| **Dependencies** | **PacketEvents** (Download and place in the `plugins/` directory) |
| **Java Version** | **Java 21** or newer (Required to execute the plugin jar) |

### Client Compatibility
> [!WARNING]
> **Minecraft Java 1.19.4 or newer** is strictly required for clients to render custom
> displays. Older game versions cannot display these custom name tags due to client-side engine
> limitations. Protocol translation tools (e.g., **ViaVersion**) do not bypass this limitation.

> [!NOTE]
> **Bedrock Edition (Geyser/Floodgate)** clients are only partially supported. Specific
> rendering options, such as custom text shadows and background plates, may not display
> accurately or at all on Bedrock clients. For details, see the
> [Integrations](integrations/integrations.md) guide.

For the complete list of known constraints, see [Limitations](limitations.md).

---

## Installation

Follow these steps to install the plugin on your server:

1. Download **PacketEvents** from [Modrinth](https://modrinth.com/plugin/packetevents) and place the `.jar` file into the `plugins/` directory.
1. Download **UnlimitedNameTags** and place the `.jar` file into the `plugins/` directory.
1. Start (or restart) the server. Verify that both plugins enable successfully without error messages in the server console.
1. The plugin will automatically generate the configuration directory and file at `plugins/UnlimitedNameTags/settings.yml` upon its initial execution.
1. Modify `settings.yml` to fit your server's needs, then execute the `/unt reload` command to apply your changes.

---

## First-Run Checklist

Confirm your setup is correct by verifying the following:
- [ ] The console outputs `[UnlimitedNameTags] Plugin enabled` without errors.
- [ ] The console reports no startup issues from **PacketEvents**.
- [ ] The file `plugins/UnlimitedNameTags/settings.yml` has been successfully created.
- [ ] A player joining from a Minecraft Java 1.19.4+ client can see the custom name tags.

---

## Minimal Working Configuration

Below is a complete, valid `settings.yml` using the current schema version. It configures a single `default` name tag preset that displays the player's username.

```yaml
configVersion: 7

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
  throughWallMode: SEE_THROUGH
  throughWallSettings:
    opacity: 55
    maxDistance: 48.0
    checkInterval: 5

performance:
  componentCaching: false
  placeholderCacheTime: 1
  enableRelationalPlaceholders: false
  placeholderUpdateRates: {}
  distanceRefreshCulling:
    enabled: true
    nearDistance: 24.0
    maxDistance: 96.0
    maxInterval: 100
    curve: 2.0

nameTags:
  default:
    displayGroups:
      - lines:
          - text: '%player_name%'
        scale: 1.0
        yOffset: 0.0
```

> [!NOTE]
> The `default` entry (which contains no `permission:` field) acts as the fallback preset. Any player who does not match a higher-priority custom preset will be assigned this layout.

---

## Applying Changes

To apply edits made to `settings.yml` or `advanced.yml` (if present) without restarting the Minecraft server, execute the following command:

- **Command:** `/unt reload` (run in-game or via the console)

---

## Next Steps

To continue configuring and tuning **UnlimitedNameTags**, consult these dedicated guides:

| Guide | Description |
| :--- | :--- |
| **[Configuration](configuration.md)** | A detailed breakdown of every main configuration option. |
| **[Display Groups](features/display-groups.md)** | Learn how to stack lines, adjust layouts, and use custom elements. |
| **[Animations](features/animations.md)** | Add movement, colors, and dynamic effects to name tags. |
| **[Performance Tuning](performance.md)** | Best practices for running the plugin on active or large servers. |
| **[Migration Guide](migration.md)** | Step-by-step instructions for upgrading from older configuration versions. |
