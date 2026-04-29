# Optional `advanced.yml` (helmet height rules)

**UnlimitedNameTags** can read an **optional** file `advanced.yml` in the plugin data folder (`plugins/UnlimitedNameTags/advanced.yml`). It is **never created automatically**: if missing, behaviour is unchanged.

Use it for **manual** rules to raise the nametag for specific helmets (**custom model data**, **equippable model**, **material**, etc.). Useful when automatic hooks do not match your items (e.g. **ItemsAdder** not active in the current build — use `advanced.yml` or **Nexo** / **Oraxen** where supported).

---

## How it works

* Rules live under **`helmetHeightRules`** (a list).
* Each rule adds vertical offset using the same **`height`** unit as other hat hooks (Nexo, Oraxen, …).
* Rules are sorted by **`priority`** (higher first). The **first matching** rule for the player’s helmet applies; its `height` is used.
* This hook registers **before** other integrations: a matching `advanced.yml` rule **wins** over Nexo / ItemsAdder / Oraxen / HMCCosmetics when it returns height **> 0**.
* Reload with **`/unt reload`** (same as `settings.yml`).

---

## YAML format (ConfigLib)

Keys use **camelCase** (ConfigLib default), not kebab-case.

### Example

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

Full template in the main repo: [`advanced.example.yml`](https://github.com/alexdev03/UnlimitedNameTags/blob/main/advanced.example.yml).

---

## Rule fields

| Field | Required | Description |
|--------|-----------|-------------|
| `priority` | No (default `0`) | Higher values evaluated first. |
| `height` | Yes (**> 0**) | Vertical offset; same scale as other hat hooks. |
| `material` | No* | Bukkit material name, e.g. `PLAYER_HEAD`. |
| `customModelData` | No* | Exact **CustomModelData** match. |
| `customModelDataMin` / `customModelDataMax` | No* | Inclusive CMD range; **both** required. If a range is set, `customModelData` is ignored (console warning). |
| `equippableModel` | No* | `namespace:key` for equippable model (**1.21.3+**). |
| `worlds` | No | If non-empty, player must be in one of these world **names**. |
| `permission` | No | If set, player must have this permission. |

\* **At least one** of: `material`, `customModelData`, both `customModelDataMin` and `customModelDataMax`, or `equippableModel`, or the rule never applies.

---

## Matching logic

All conditions on a rule must pass:

1. `permission` (if set)
2. `worlds` (if non-empty)
3. `material` (if set) — must match helmet item type; unknown materials log a warning
4. `equippableModel` (if set) — equippable meta and same model key
5. CMD **range** (if both min and max set), or exact `customModelData` (if set and no full range)

Material-only rules (no CMD / equippable) match **any** stack of that material.

---

## Errors and reload

* **Missing** file: no extra offset from this file.
* File exists but **fails on first start**: empty rules for `advanced.yml`.
* Failure on **`/unt reload`**: **previous** successfully loaded `advanced` data is kept.
* Invalid rules (`height <= 0`, only one of min/max CMD, no item matcher): **logged**; they do not apply.

---

## See also

* [Configuration (`settings.yml`)](../configuration.md)
* [Integrations](../integrations/integrations.md) — Nexo, ItemsAdder, hats
