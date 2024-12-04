# **UnlimitedNameTags Wiki** 📚
*A powerful tool to customize and manage player name tags like never before!*


Welcome to the official documentation for the **UnlimitedNameTags** plugin. This wiki provides detailed information about the plugin's features, installation, configuration, commands, and integrations.

---

[![Discord](https://img.shields.io/discord/1263414013040263249?label=Discord&logo=discord&color=5865F2)](https://discord.gg/W4Fu8fqCKs)  
[![CodeFactor](https://www.codefactor.io/repository/github/alexdev03/unlimitednametags/badge)](https://www.codefactor.io/repository/github/alexdev03/unlimitednametags)  
[![API Version](https://img.shields.io/github/v/release/alexdev03/UnlimitedNametags?&color=blue)](https://github.com/alexdev03/UnlimitedNametags/releases/latest)

![Unlimited Name Tags in Action](https://i.imgur.com/w7zlGaO.gif)

## **Table of Contents**
1. [Overview](#overview) 📝
2. [Requirements](#requirements) 📋
3. **Features** ✨
    - [Billboards](features/billboards.md) 🎥
    - [LineGroups](features/linesgroups.md) 🏷️
    - [Placeholder Replacements](features/placeholders-replacements.md) 🔄
    - [Show While Looking](features/show-while-looking.md) 👀
4. [Configuration](configuration) 🔧
5. [Commands and Permissions](commands-permissions) 🖱️🔑
6. [Integrations](integrations/integrations.md) 🔗
7. [Support](#-support) 🆘

---

## **Overview**

Unlimited Name Tags is a powerful plugin for Minecraft servers that allows advanced customization and management of player name tags. It is compatible with **Paper 1.20.1+** and **Spigot 1.20.2+**, with **Paper** being highly recommended for optimal performance and compatibility with advanced features.

### Key Features:
- Customizable Name Tags (colors, formats, extra details)
- Dynamic Placeholder Support via [PlaceholderAPI](https://github.com/PlaceholderAPI/PlaceholderAPI)
- Smooth Name Tag Movement (client-side, no lag)
- Compatibility with vanish plugins
- Customizable tag behavior based on player relationships
- Easy-to-configure settings via `settings.yml`

---

## **Requirements**

- Minecraft **Paper 1.20.1+** or **Spigot 1.20.2+**
- [PacketEvents](https://www.spigotmc.org/resources/packetevents-api.80279/)


## 📜 **Supported Versions**
- **Paper**: Fully supported from **1.20.1+** *(highly recommended)*.
- **Spigot**: Supported from **1.20.2+**, but Paper is preferred for enhanced performance.

---

## 📜 **Supported Client Versions**
**Java:**
- 1.19.4+

> **Note**: Versions below **1.19.4** do not support text displays because the required packet functionality does not exist. Additionally, clients connecting with **ViaBackwards** are not compatible and will not display name tags correctly. For the best experience, ensure both the server and clients are using compatible versions.
> You may connect with 1.19.4 client using **ViaBackwards** and **ViaVersion** and a server on one of the supported versions.


**Bedrock:** ([Waiting for this pull request to be merged](https://github.com/GeyserMC/Geyser/pull/5157))
- Latest 

> **Note**: Bedrock clients do not support text displays so not all features are supported. Only multi-line and rgb colors are supported.
> Features like backgrounds and shadows are not supported.
---

## 💬 **Support**

Need help? Join our [Discord Server](https://discord.gg/W4Fu8fqCKs)! For **pre-sale questions**, feel free to use the **#chat** channel. If you need support, please open a ticket and ensure your license is verified to gain access to assistance.

---