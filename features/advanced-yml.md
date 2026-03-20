# 🧢 **Optional `advanced.yml` (helmet height rules)**

**UnlimitedNameTags** can read an **optional** file named `advanced.yml` in the plugin data folder (`plugins/UnlimitedNameTags/advanced.yml`). This file is **never created automatically**: if it does not exist, the plugin behaves exactly as before.

Use it when you need **manual** rules to raise the name tag for specific helmets (for example **custom model data**, **equippable model keys**, or **material**). This is especially useful when automatic hooks do not match your items (for example **ItemsAdder** is not active in the current release — use `advanced.yml` for those items, or rely on **Nexo** / **Oraxen** where supported).

---

## **How it works**

* Rules live under **`helmetHeightRules`** (a list).
* Each rule adds vertical offset using the same **`height`** unit as other hat hooks (e.g. Nexo / ItemsAdder).
* Rules are sorted by **`priority`** (higher first). The **first rule that matches** the player’s helmet applies; its `height` is used.
* This hook is registered **before** other hat integrations, so a matching `advanced.yml` rule **wins** over Nexo / ItemsAdder / Oraxen / HMCCosmetics when it returns a height **greater than 0**.
* Reload with **`/unt reload`** (same as `settings.yml` for the main config).

---

## **YAML format (ConfigLib)**

Keys use **camelCase** (ConfigLib default), not kebab-case.

### **Example**

```yaml
helmetHeightRules:
  - priority: 10
    height: 28
    material: PLAYER_HEAD
    customModelData: 12345

  - priority: 5
    height: 20
    material: LEATHER_HELMET
    customModelDataMin: 1000
    customModelDataMax: 1999

  - priority: 8
    height: 32
    equippableModel: "mypack:item/tall_hat"

  # Optional on any rule:
  # worlds: [world, world_nether]
  # permission: "myserver.bighelmet"
```

A full copy-paste template is also in the main plugin repo as [`advanced.example.yml`](https://github.com/alexdev03/UnlimitedNameTags/blob/main/advanced.example.yml).

---

## **Rule fields**

| Field | Required | Description |
|--------|-----------|-------------|
| `priority` | No (default `0`) | Higher values are evaluated first. |
| `height` | Yes (must be **> 0**) | Vertical offset; same scale as other hat hooks. |
| `material` | No* | Bukkit material name, e.g. `PLAYER_HEAD`, `LEATHER_HELMET`. |
| `customModelData` | No* | Exact **CustomModelData** match. |
| `customModelDataMin` / `customModelDataMax` | No* | Inclusive CMD range; **both** must be set. If a range is set, `customModelData` is **ignored** (the server logs a warning). |
| `equippableModel` | No* | `namespace:key` for the equippable model (**Minecraft 1.21.3+**), e.g. `mypack:item/my_hat`. |
| `worlds` | No | If non-empty, the player must be in one of these world **names**. |
| `permission` | No | If set, the player must have this permission. |

\* **At least one** of the following must be set, or the rule never applies: `material`, `customModelData`, both `customModelDataMin` and `customModelDataMax`, or `equippableModel`.

---

## **Matching logic**

All conditions you specify on a rule must pass:

1. `permission` (if set)
2. `worlds` (if non-empty)
3. `material` (if set) — must match the helmet item’s type; unknown material names are warned in the console.
4. `equippableModel` (if set) — helmet must have equippable meta and the same model key.
5. CMD **range** (if both min and max set), or **exact** `customModelData` (if set and no full range).

Material-only rules (no CMD / equippable) match **any** stack of that material.

---

## **Errors and reload**

* If `advanced.yml` is **missing**: defaults are used (no extra helmet offset from this file).
* If the file **exists** but fails to load on **first** start: the plugin uses empty rules for this file.
* If it fails on **`/unt reload`**: the **previous** successfully loaded `advanced` data is kept.
* Invalid rules (e.g. `height <= 0`, only one of min/max CMD, no item matcher) are **logged**; they will not apply.

---

## **See also**

* [Configuration (`settings.yml`)](../configuration.md)
* [Integrations](../integrations/integrations.md) — Nexo, ItemsAdder, hat-related plugins
