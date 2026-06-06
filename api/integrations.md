# Integrations (API)

## Vanish

Implement **`VanishIntegration`** to hook a custom or third-party vanish plugin:

```java
api.setVanishIntegration(new VanishIntegration() {
    @Override
    public boolean canSee(Player viewer, Player other) {
        return myVanishPlugin.canSee(viewer, other);
    }

    @Override
    public boolean isVanished(Player player) {
        return myVanishPlugin.isVanished(player);
    }
});
```

Tab list / scoreboard helpers for soft vanish:

```java
api.vanishPlayer(player);
api.unVanishPlayer(player);
```

---

## Hat Offset Hooks

Implement **`HatHook`** for headwear not detected by built-in integrations:

```java
HatHook hook = player -> {
    MyCosmetic hat = myPlugin.getActiveHat(player);
    return hat != null ? hat.getHeightOffset() : 0.0;
};

api.addHatHook(hook);
api.removeHatHook(hook);
```

> [!NOTE]
> Return **`0.0`** when the hook does not apply. For file-based rules, prefer
> [`advanced.yml`](../features/advanced-yml.md).

---

## Direct Display Entity Access

> [!CAUTION]
> **`getPacketDisplayText(player)`** on **`UNTPaperAPI`** exposes live **`UntNametagDisplay`**
> instances — the supported low-level row type for addon plugins. Use only for advanced cases
> (custom viewer filtering, display metadata). Prefer high-level API methods or
> [Bukkit events](events.md).

> [!WARNING]
> **`PacketNameTag`** and other internal packet/implementation classes are **not** part of the
> public API. Do not reference them in addon code or documentation; use **`UntNametagDisplay`**
> and **`UNTPaperAPI`** instead.

If your integration touches EntityLib types (e.g. **`BillboardConstraints`**), add EntityLib as
**compile-only** — see [API Setup — EntityLib](setup.md#entitylib-optional-compile-only).
