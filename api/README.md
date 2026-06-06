# Developer API

**UnlimitedNameTags** exposes a Java API for other plugins to control name tags at runtime:
overrides, glow, animations, Bukkit events, vanish hooks, and more.

- **[Setup](setup.md)**: Maven dependency (`io.github.alexdev03`), optional EntityLib compile-only,
  `plugin.yml`, `getInstance()`, public vs internal types
- **[Overrides](overrides.md)**: Layout overrides, property shortcuts, persistence
- **[Events](events.md)**: Bukkit visibility and lifecycle events
- **[Glow](glow.md)**: Per-row glow overrides and custom handlers
- **[Animations](animations.md)**: Programmatic and custom pose animations
- **[Visibility](visibility.md)**: Refresh, hide/show, forced nametags, sneak opacity
- **[Integrations](integrations.md)**: Vanish, hat offsets, low-level display access

For YAML-side glow and config, see also the [Glow feature guide](../features/glow.md).
