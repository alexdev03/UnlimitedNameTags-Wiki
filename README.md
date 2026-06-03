# UnlimitedNameTags — Wiki

> [!NOTE]
> This is the documentation for **UnlimitedNameTags v2.x (current)**. If you are using the legacy version 1.x, please refer to the [v1 Wiki](v1/README.md).

A comprehensive guide for configuring and customizing **UnlimitedNameTags v2.x**, a premium solution for creating advanced stacked name tags (featuring plain text, items, and blocks) above player heads on Paper servers.

This wiki is the official documentation for **[UnlimitedNameTags](https://github.com/alexdev03/UnlimitedNameTags)** on GitHub.

[![Discord](https://img.shields.io/discord/1263414013040263249?label=Discord&logo=discord&color=5865F2)](https://discord.gg/W4Fu8fqCKs)
[![Release](https://img.shields.io/github/v/release/alexdev03/UnlimitedNametags?color=blue)](https://github.com/alexdev03/UnlimitedNameTags/releases/latest)

| Requirement | Details |
| :--- | :--- |
| **Server Software** | **Paper 1.21.4+** (Paper or its forks are highly recommended) |
| **Required Dependency** | **[PacketEvents](https://modrinth.com/plugin/packetevents)** (must be installed as a standard plugin) |
| **Main Configuration** | `plugins/UnlimitedNameTags/settings.yml` (automatically generated on first run) |
| **Advanced Tweaks** | `plugins/UnlimitedNameTags/advanced.yml` (optional configuration file for helmet height adjustment) |

![Name tags preview](assets/nametags-preview.gif)

---

## Where to Start

| Page | Description |
| :--- | :--- |
| **[Getting Started](getting-started.md)** | Installation process, first-run requirements, and minimal configuration examples. |
| **[Configuration](configuration.md)** | Step-by-step breakdown of the main `settings.yml` parameters. |
| **[Display Groups](features/display-groups.md)** | Stacking multiple rows, conditional visibility, and custom elements (text, items, blocks). |
| **[Performance Tuning](performance.md)** | Optimization guide for high-population servers. |
| **[Commands & Permissions](commands-permissions.md)** | Full list of `/unt` command usage and permission nodes. |
| **[Migration Guide](migration.md)** | Detailed instructions on upgrading from v1.x configuration formats. |
| **[Developer API](api.md)** | Java API integration documentation for custom developers. |
| **[Integrations](integrations/integrations.md)** | External plugin support (Nexo, Geyser, PlaceholderAPI, etc.). |
| **[FAQ](faq.md)** | Answers to common setup questions and troubleshooting steps. |

### Feature Guides

* **[Animations](features/animations.md)** — Configure movement patterns and color cycling effects.
* **[Billboard Settings](features/billboards.md)** — Control how name tags rotate relative to the player camera.
* **[Placeholder Replacements](features/placeholders-replacements.md)** — Customize raw outputs from PlaceholderAPI variables.
* **[Show While Looking](features/show-while-looking.md)** — Display name tags dynamically based on visibility line-of-sight.
* **[Advanced Configuration](features/advanced-yml.md)** — Detailed custom height adjustments using `advanced.yml`.

---

## Compatibility & Requirements

### Server Platform
* **Paper 1.21.4+** is required. Paper provides optimized packet handling and API support necessary for rendering custom displays.

### Java Clients
> [!WARNING]
> Custom displays require **Minecraft Java 1.19.4 or newer**. Older clients (pre-1.19.4) do not support the game engine rendering features required by this plugin. Using protocol support plugins like **ViaVersion** will not bypass this native client-side rendering limitation.

### Bedrock Edition (Geyser)
* **Geyser/Bedrock** players are partially supported. However, visual features (e.g., custom colors, custom backgrounds, multi-line alignments) may render differently compared to Java Edition clients due to Bedrock rendering engine limitations.

---

## Support

Need assistance? Join the community on our **[Discord Server](https://discord.gg/W4Fu8fqCKs)**. 
* To receive support, please open a ticket in the designated channel and verify your license when prompted.
