# Integrations

UnlimitedNameTags integrates with several plugins. Below: known behaviour and limits.

---

## Nexo & Oraxen

**Nexo** and **Oraxen** support **dynamic nametag height** when players wear hats from those packs: the tag shifts vertically based on model height.

### Nexo

With Nexo helmets (or merged resource-pack items), vertical offset follows model height.

![Nexo nametag offset](https://i.imgur.com/ocnlz9Q.gif)

### Oraxen

Same approach for **Oraxen** helmets (custom model data and item definitions).

For both you can **add or override** automatic rules with [`advanced.yml`](../features/advanced-yml.md) (manual rules take precedence where the code specifies).

---

## ItemsAdder

In current source the **ItemsAdder** hook may be **disabled** (`false &&` in onEnable). Do not rely on automatic integration until a release notes it explicitly.

You can still tune height with [`advanced.yml`](../features/advanced-yml.md) (`customModelData`, `equippableModel`, `material`).

---

## Hats / cosmetics (HMCCosmetics, etc.)

**HMCCosmetics** is integrated for height when a compatible source is present (e.g. CreativeHook / Nexo).

**CosmeticsCore** has no dedicated hook: compatibility is **indirect** only if the helmet is still a normal Bukkit item describable by Nexo, Oraxen, or `advanced.yml`.

---

## ViaVersion

With **ViaVersion** installed, the plugin checks whether a viewer’s client can show **text displays**. This does **not** make Java clients **below 1.19.4** supported. See [Supported versions](../README.md#supported-versions).

---

## LibsDisguises

Supported: nametags can be hidden while the player is disguised as another entity.

---

## PlaceholderAPI

Lines use **PlaceholderAPI**. Enable **`enableRelationalPlaceholders`** if you need **relational** placeholders (viewer + target).

### Built-in `%unt_<param>%` expansion

| Placeholder | Purpose |
|-------------|---------|
| `%unt_phase-mm%` | MiniMessage-style animation phase |
| `%unt_phase-md%` | MineDown phase |
| `%unt_phase-mm-g%` | MiniMessage gradient phase |
| `%unt_-phase-mm%` | Negative phase (MiniMessage) |
| `%unt_-phase-md%` | Negative phase (MineDown) |

There is **no** `%unt_-phase-mm-g%` in the expansion — use the inline token **`#-phase-mm-g#`** in nametag lines (see [Animations](../features/animations.md)).

Same values as inline placeholders where both exist (`#phase-mm#`, …).

---

## MiniPlaceholders

Supported. Prefer **`MINIMESSAGE`** or **`UNIVERSAL`** formatters. Watch interactions with **`componentCaching`**.

---

## TypeWriter

Supported: nametags can be disabled while a player is in cinematic mode.

---

## Floodgate

With **Floodgate** / **Geyser**, the plugin can distinguish Bedrock from Java players where needed.

---

## FeatherServerAPI

When present, the plugin can **block** Feather’s client nametag mod on supported Java clients so it does not conflict with server-rendered UnlimitedNameTags.

---

## Bedrock (Geyser)

**Geyser** lets Bedrock players see nametags, but **text displays** are not equivalent to Java. Backgrounds, shadows, and multi-line may differ. See [Supported versions / Bedrock](../README.md#supported-versions).
