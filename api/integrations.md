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
> **`getPacketDisplayText(player)`** exposes live **`UntNametagDisplay`** instances. Use only for
> advanced cases (low-level packets, custom viewer filtering). Prefer high-level API methods or
> [Bukkit events](events.md).
