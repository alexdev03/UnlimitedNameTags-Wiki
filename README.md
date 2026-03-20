# 🏷️ Overview

_A powerful tool to customize and manage player name tags like never before!_

Welcome to the official documentation for the **UnlimitedNameTags** plugin. This wiki provides detailed information about the plugin's features, installation, configuration, commands, and integrations.

---

[![Discord](https://img.shields.io/discord/1263414013040263249?label=Discord\&logo=discord\&color=5865F2)](https://discord.gg/W4Fu8fqCKs)\
[![CodeFactor](https://www.codefactor.io/repository/github/alexdev03/unlimitednametags/badge)](https://www.codefactor.io/repository/github/alexdev03/unlimitednametags)\
[![API Version](https://img.shields.io/github/v/release/alexdev03/UnlimitedNametags?\&color=blue)](https://github.com/alexdev03/UnlimitedNametags/releases/latest)

![Unlimited Name Tags in Action](https://i.imgur.com/w7zlGaO.gif)

## **Table of Contents**

1. [Overview](./#overview) 📝
2. [Requirements](./#requirements) 📋
3. **Features** ✨
   * [Advanced (`advanced.yml`)](features/advanced-yml.md) 🧢
   * [Animations](features/animations.md) 🎨
   * [Billboards](features/billboards.md) 🎥
   * [LineGroups](features/linesgroups.md) 🏷️
   * [Placeholder replacements](features/placeholders-replacements.md) 🔄
   * [Show while looking](features/show-while-looking.md) 👀
4. [Configuration](configuration.md) 🔧
5. [Commands and permissions](commands-permissions.md) 🖱️🔑
6. [Integrations](integrations/integrations.md) 🔗
7. [Wiki style guide](STYLE.md) ✏️
8. [Support](./#-support) 🆘

---

## **Overview**

Unlimited Name Tags is a powerful plugin for Minecraft servers that allows advanced customization and management of player name tags. It is compatible with **Paper 1.20.1+** and **Spigot 1.20.2+**, with **Paper** being highly recommended for optimal performance and compatibility with advanced features.

### Key Features:

* Customizable Name Tags (colors, formats, extra details)
* Dynamic Placeholder Support via [PlaceholderAPI](https://github.com/PlaceholderAPI/PlaceholderAPI)
* Smooth Name Tag Movement (client-side, no lag and no teleport)
* Compatibility with vanish plugins
* Customizable tag behavior based on player relationships
* Easy-to-configure settings via `settings.yml`
* Optional [`advanced.yml`](features/advanced-yml.md) for manual helmet → name tag height rules (custom CMD, equippable models, etc.)

---

## **Requirements**

* Minecraft **Paper 1.20.1+** or **Spigot 1.20.2+**
* [PacketEvents](https://www.spigotmc.org/resources/packetevents-api.80279/)

## 📜 **Supported Versions**

* **Paper**: Fully supported from **1.20.1+** _(highly recommended)_.
* **Spigot**: Supported from **1.20.2+**, but Paper is preferred for enhanced performance.

---

## 📜 **Supported Client Versions**

**Java:**

* **1.19.4 or newer** is required for **custom UnlimitedNameTags** (text display) name tags to render. Older clients lack the necessary protocol support.

> **Important:** Players using a **Java client older than 1.19.4** will **not** see custom name tags from this plugin. This is a hard limitation, not a bug.
>
> **Support policy:** Any support request related to clients **below 1.19.4** (including combinations with ViaVersion / ViaBackwards) **will be ignored**. Use a supported client version.

The plugin may use **ViaVersion** on the server to detect viewer capabilities; that does **not** make sub-1.19.4 clients supported for displaying these name tags.

**Bedrock:** ([Waiting for this pull request to be merged](https://github.com/GeyserMC/Geyser/pull/5157))

* Latest

> **Note:** Bedrock clients do not support text displays like Java; not all features match. Multi-line and RGB-style colours may work; backgrounds and shadows often will not.

---

## 💬 **Support**

Need help? Join our [Discord Server](https://discord.gg/W4Fu8fqCKs)! For **pre-sale questions**, feel free to use the **#chat** channel. If you need support, please open a ticket and ensure your license is verified to gain access to assistance.

---
