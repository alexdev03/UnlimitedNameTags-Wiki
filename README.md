# UnlimitedNameTags — Wiki

> Stacked name tags for Paper — **text**, **items**, and **blocks**; PlaceholderAPI; display-entity animations; developer API.

Documentation for **[UnlimitedNameTags](https://github.com/alexdev03/UnlimitedNameTags)**.

[![Discord](https://img.shields.io/discord/1263414013040263249?label=Discord&logo=discord&color=5865F2)](https://discord.gg/W4Fu8fqCKs)
[![Release](https://img.shields.io/github/v/release/alexdev03/UnlimitedNametags?color=blue)](https://github.com/alexdev03/UnlimitedNameTags/releases/latest)

| | |
|:--|:--|
| **Host** | Paper **1.20.1+** (Spigot 1.20.2+ supported; Paper recommended) |
| **Depends on** | [PacketEvents](https://modrinth.com/plugin/packetevents) (required) |
| **Config** | `plugins/UnlimitedNameTags/settings.yml` (schema v4) |
| **Optional** | `advanced.yml` — helmet height rules (not auto-generated) |

![Name tags preview](https://i.imgur.com/w7zlGaO.gif)

---

## Quick links

| Page | Contents |
|------|----------|
| [Configuration](configuration.md) | `settings.yml`: `displayGroups`, globals, formatting |
| [Display groups](features/display-groups.md) | Stacked rows, `when` (JEXL), TEXT / ITEM / BLOCK |
| [Performance](performance.md) | Reducing CPU load, ticks, and network |
| [FAQ](faq.md) | Common questions and issues |
| [Commands & permissions](commands-permissions.md) | `/unt`, viewer permissions |
| [Integrations](integrations/integrations.md) | PAPI, ViaVersion, Nexo, Geyser, etc. |

### Features

- [Animations](features/animations.md) — phase placeholders (`#phase-mm#`) and display animations (`rotate`, `bob`, …)
- [Billboard](features/billboards.md) — `CENTER`, `HORIZONTAL`, `VERTICAL`, `FIXED`
- [Placeholder replacements](features/placeholders-replacements.md) — rewrite PAPI output strings
- [Show while looking](features/show-while-looking.md) — tag only when aiming at the player
- [Advanced (`advanced.yml`)](features/advanced-yml.md) — manual helmet / CMD height rules

---

## Supported versions

**Server:** Paper 1.20.1+; Spigot 1.20.2+ (Paper preferred).

**Java clients:** custom name tags (text displays) require **1.19.4 or newer**. Older clients lack protocol support; this is not a plugin bug. **ViaVersion on the server does not** make sub-1.19.4 clients supported for these tags.

**Bedrock (Geyser):** partial support; text displays do not match Java (backgrounds, shadows, multi-line may differ).

---

## Support

[Discord](https://discord.gg/W4Fu8fqCKs) — for help after purchase, open a ticket with verified license where required.
