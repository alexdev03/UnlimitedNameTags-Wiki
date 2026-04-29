# Optional `advanced.yml` (helmet height rules)

You can add a second file next to your main config: **`plugins/UnlimitedNameTags/advanced.yml`**.

- The plugin **never creates it for you**. If the file does not exist, nothing changes.
- Use it to **nudge the nametag up or down** when someone wears a specific helmet (custom head, leather with model data, resource-pack items, …).
- Reload with **`/unt reload`** like the main `settings.yml`.

Handy when automatic pack support (Nexo, Oraxen, …) does not match your item, or ItemsAdder auto-height is not available in your build — see [Integrations](../integrations/integrations.md).

---

## How it works

* Rules live under **`helmetHeightRules`** — a **list** you add to yourself.
* Each rule sets a **`height`** (same kind of number other hat integrations use).
* **`priority`** — higher numbers are checked **first**. The **first rule that matches** the helmet wins.
* Manual rules here can **override** automatic height from Nexo / Oraxen / ItemsAdder / HMCCosmetics when they return a height **greater than zero** (see server logs if unsure).

---

## How to write option names

Use the spellings below exactly — **no dashes** in names, and **capitals** where shown (e.g. `customModelData`, not `custom-model-data`). Copy from the sample and change the numbers.

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

Full copy-paste template: [`reference/advanced.example.yml`](../reference/advanced.example.yml).

---

## Rule fields

| Field | Required | What it means |
|--------|-----------|---------------|
| `priority` | No (treat as `0` if omitted) | Higher = checked earlier. |
| `height` | Yes, must be **&gt; 0** | How much to raise the tag. |
| `material` | No* | Helmet item type, e.g. `PLAYER_HEAD`. |
| `customModelData` | No* | Exact **custom model data** number on the item. |
| `customModelDataMin` / `customModelDataMax` | No* | Inclusive range — **both** needed. If you use a range, single `customModelData` is ignored. |
| `equippableModel` | No* | `namespace:key` style id on **1.21.3+** items. |
| `worlds` | No | If set, only applies in those world **names**. |
| `permission` | No | If set, player must have this permission. |

\* At least **one** of: `material`, `customModelData`, both min and max, or `equippableModel` — otherwise the rule never matches.

---

## Matching logic

Everything on a rule must pass:

1. Permission (if you set one)
2. World list (if you set one)
3. Material (if you set one)
4. Equippable model (if you set one)
5. Either a **range** of model data or one exact **`customModelData`**

A rule with only **`material`** matches **any** stack of that item type.

---

## If something goes wrong

* **No file** → no extra offset from this feature.
* **Bad file on first start** → rules load as empty; check console errors.
* **Bad file on reload** → **previous** good rules stay loaded until you fix YAML.
* Broken single rules are **skipped** and logged — fix the numbers and reload.

---

## See also

* [Configuration (`settings.yml`)](../configuration.md)
* [Integrations](../integrations/integrations.md) — Nexo, Oraxen, ItemsAdder, hats
