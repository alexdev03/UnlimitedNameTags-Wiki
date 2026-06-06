# API Setup

## Adding the Dependency

Add **`unlimitednametags-api-paper`** from Maven Central as **compile-only** (`provided` in Maven).
Use the same version as the **UnlimitedNameTags** plugin on your server. Do not shade or bundle the
API — the plugin JAR must be on the server at runtime.

### Gradle (Kotlin DSL)
```kotlin
repositories {
    mavenCentral()
}

dependencies {
    compileOnly("io.github.alexdev03:unlimitednametags-api-paper:2.0.0")
}
```

### Maven
```xml
<dependency>
    <groupId>io.github.alexdev03</groupId>
    <artifactId>unlimitednametags-api-paper</artifactId>
    <version>2.0.0</version>
    <scope>provided</scope>
</dependency>
```

> [!NOTE]
> For UUID-only integrations without Paper types, use artifact **`unlimitednametags-api`** instead.
> It is pulled in transitively when you depend on **`api-paper`** — you normally do not need
> **`unlimitednametags-common`** separately.

### Plugin Configuration (`plugin.yml`)

```yaml
# Hard dependency (required for startup)
depend: [UnlimitedNameTags]

# Or soft dependency (handle absence gracefully in code)
softdepend: [UnlimitedNameTags]
```

---

## Retrieving the API Instance

- **Paper/Bukkit (recommended):**
  ```java
  UNTPaperAPI api = UNTPaperAPI.getInstance();
  ```

- **Platform-neutral (UUID-based):**
  ```java
  UNTAPI api = UNTAPI.getInstance();
  ```

> [!WARNING]
> Calling `getInstance()` before your plugin's `onEnable()` or when **UnlimitedNameTags** is
> disabled throws `IllegalStateException`. For soft dependencies, guard with
> `Bukkit.getPluginManager().isPluginEnabled("UnlimitedNameTags")` first.

---

## Core Types Reference

| Type | Description |
| :--- | :--- |
| **`UNTAPI`** | Core platform-neutral entry point (UUID-based). |
| **`UNTPaperAPI`** | Paper/Bukkit entry point with `Player` overloads, custom animation/glow registration, and forced nametag helpers. |
| **`UnlimitedNameTagsInstancePaper`** | Extended plugin interface via `UNTPaperAPI.paperPlugin()`. |
| **`UntNametagManager` / `UntNametagManagerPaper`** | Override, glow, refresh, and visibility operations (`api.nametagManager()`). |
| **`Settings.NameTag`** | Permission preset and its `displayGroups` list. |
| **`Settings.DisplayGroup`** | One stacked row (text, item, or block), with optional `glow` and `animation`. |
| **`Settings.NametagLine`** | Text line with optional `when` condition. |
| **`Settings.Background`** | Background plate (color, opacity, shadow, see-through). |
| **`GlowOverride`** | Per-row glow (`fixed`, `reference`, `rainbow`, `gradient`, `custom`). |
| **`DisplayAnimation`** | Built-in and custom physical animations. |
| **`UntNametagDisplay`** | Live client-side display entity (one row). |
| **`NametagCustomAnimationHandler`** | Custom pose animation handler. |
| **`NametagCustomGlowHandler`** | Custom glow color handler (`api-paper`). |
| **`NametagCustomGlowContext`** | Tick context for custom glow handlers. |
| **`VanishIntegration`** | Third-party vanish hook. |
| **`HatHook`** | Custom helmet/cosmetic height offset hook. |
