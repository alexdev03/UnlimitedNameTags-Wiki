# Performance

Nametags redraw and ask PlaceholderAPI for fresh text on a schedule. When lots of players are online, or lines use heavy placeholders, small changes in **`settings.yml`** can keep things smooth.

**Ticks:** on most servers **20 ticks ≈ 1 second**. Settings often say “every N ticks.”

---

## Where to look in the file (newer layouts)

Recent plugin versions group “global” switches under three **headings** in `settings.yml`:

- **`behavior:`** — how often things update, how far tags are sent, text format, default facing mode, etc.
- **`visibility:`** — sneaking fade, “only while looking,” seeing through walls, seeing your own tag, …
- **`performance:`** — optional caching, slowing down specific placeholders, relational PlaceholderAPI mode

If your file is older, some keys may sit at the **top level** until you upgrade and the plugin reorganises them. The **names** of the options are the same — they just move under the right heading.

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
  # your presets…
```

Per-option explanations: [Configuration](configuration.md).

---

## What usually costs the most

| Area | Plain-English effect |
|------|----------------------|
| **PlaceholderAPI** | Every refresh asks placeholders again — slow or fancy expansions add up with many players and many lines. |
| **Refresh speed** | **`behavior.taskInterval`** — lower = snappier tags, busier server. |
| **Moving tag animations** | Pose updates on a timer; you can slow them or stop updating when nobody is near. |
| **“Relational” placeholders** | Some placeholders need “viewer + target”; turning that **on** can multiply work. |
| **Text format** | **`UNIVERSAL`** is the richest and **heaviest**; **`MINIMESSAGE`** is the usual sweet spot. |
| **Special visibility modes** | “Only while looking” and “through walls” run extra checks. |
| **View distance** | **`behavior.viewDistance`** — how far the game **bothers** drawing the tag; lower often means less work for people far away. |

---

## `behavior.taskInterval`

Seconds-per-refresh is roughly **`taskInterval / 20`**.

- **Bigger number** (e.g. `40`): updates less often, lighter server, leaderboards / money text feel slower.
- **Smaller number** (e.g. `10`): livelier, heavier.

For many servers **`20`** (about once per second) is a good starting point.

---

## `behavior.displayAnimationInterval` and per-row `animationInterval`

Controls how often **moving** parts of the tag update. If a row sets its own **`animationInterval`**, that wins; otherwise the global **`displayAnimationInterval`** applies. **`0`** means “match `taskInterval`.”

Rainbow-style **text** tokens like `#phase-mm#` still follow the main placeholder refresh.

More: [Animations](features/animations.md).

---

## `behavior.format`

| Value | When to use |
|-------|-------------|
| `MINIMESSAGE` | Default recommendation — modern `<…>` style colours. |
| `MINEDOWN` | If you write lines in MineDown syntax. |
| `LEGACY` | Classic `&` colour codes. |
| `UNIVERSAL` | Supports almost everything — **most CPU**. |

---

## `behavior.viewDistance`

The number in the file is **scaled** before it goes to players’ clients — it is not “exact blocks.” **Lower** usually means the tag stops being drawn sooner as you walk away, which can help on huge worlds.

---

## `behavior.compactDisplayGroupStack` and `displayGroupLineHeightBlocks`

When **`compactDisplayGroupStack: true`**, empty or hidden rows do not reserve vertical space — the stack packs tighter. Mostly a visual choice; it does not replace tuning placeholders.

**`displayGroupLineHeightBlocks`** (default `0.25`) is the estimated height per resolved text line used to calculate compact spacing. Adjust if your scale or font size differs significantly from the default.

---

## `behavior.removeEmptyLines`

Hides blank lines when placeholders come back empty — less clutter, tiny bit less work.

---

## `visibility.showWhileLooking`

Tag only if the viewer is **aiming at** that player. Can feel lighter for some setups but adds its own checks. [Show while looking](features/show-while-looking.md).

## `visibility.obscuredNametagThroughWalls`

Fades tags when there is no clear line of sight. Applies to TEXT rows only. See `obscuredNametagCheckInterval` below.

## `visibility.obscuredNametagCheckInterval`

How often (ticks) the plugin rechecks line of sight for through-wall dimming. Default is `5`. The raycast runs on the **main thread** — raising this to `10`–`20` on busy servers reduces cost significantly. The visual delay before a tag dims equals this interval, which is usually imperceptible to players.

Only relevant when `obscuredNametagThroughWalls: true`.

---

## PlaceholderAPI extras

### `performance.placeholderCacheTime`

How long (in ticks) a placeholder result may be **remembered** for a player before asking PlaceholderAPI again. Higher = fewer repeats, possibly “older” numbers on screen.

Placeholders listed under **`placeholderUpdateRates`** use their own timing instead (see below).

### `performance.componentCaching`

Can speed up repeating fancy formatting (gradients) but may act odd with **very** live placeholders — leave **off** unless you know you need it. Try on a test server first.

---

<a id="performanceplaceholderupdaterates"></a>

## `performance.placeholderUpdateRates`

A small list of **“this placeholder may wait X ticks between updates”** — great for expensive values that rarely change (balance, rank from a slow plugin, …).

```yaml
performance:
  placeholderUpdateRates:
    "%vault_eco_balance%": 100
    "%some_heavy_placeholder%": 80
```

The server will **not** refresh that placeholder faster than **`behavior.taskInterval`**, even if you type a smaller number. So if the global refresh is every `20` ticks, `5` here still waits at least `20`.

Leave combat / health-style placeholders **out** of this list so they stay responsive.

---

## `performance.enableRelationalPlaceholders`

**Off** by default. Turn **on** only if you use placeholders that **compare viewer and target**. Costs more on large servers.

---

## Animation distance culling: `cullBeyondBlocks`

On each **`animation:`** block you can set **`cullBeyondBlocks`**. If nobody is within that many blocks, the server may **skip** pose updates for that animation — handy for sparkly tags nobody is looking at.

---

## Fewer lines = less work

- Each **`displayGroup`** row has a cost — keep only what you need.
- Use **`when:`** so expensive lines are **off** when they do not matter (see [Display groups](features/display-groups.md)).

---

## Busy-Server Checklist

1. **`behavior.taskInterval`** at least **20** unless you truly need faster text.
2. **`behavior.format: MINIMESSAGE`** unless you need `UNIVERSAL`.
3. **`performance.enableRelationalPlaceholders: false`** if you do not use relational placeholders.
4. Slow down animations or add **`cullBeyondBlocks`** on decorative motion.
5. Turn off wall / aim-based extras (`showWhileLooking`, `obscuredNametagThroughWalls`) if unused.
6. Lower **`behavior.viewDistance`** if tags only need to read up close.
7. Add **`performance.placeholderUpdateRates`** for slow-changing heavy placeholders.
8. Raise **`visibility.obscuredNametagCheckInterval`** to `10`–`20` if through-wall dimming is on with many players.
9. Enable **`behavior.compactDisplayGroupStack`** if many rows are conditionally hidden to reduce unused packet space.

---

## See also

- [Configuration](configuration.md)
- [Animations](features/animations.md)
- [Display groups](features/display-groups.md)
