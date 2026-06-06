# API Setup

## Adding the Dependency

Add **`unlimitednametags-api-paper`** from Maven Central as **compile-only** (`provided` in Maven).
Use the same version as the **UnlimitedNameTags** plugin on your server. Do not shade or bundle the
API — the plugin JAR must be on the server at runtime.

{% tabs %}
{% tab title="Gradle (Kotlin DSL)" %}
```kotlin
repositories {
    mavenCentral()
}

dependencies {
    compileOnly("io.github.alexdev03:unlimitednametags-api-paper:2.0.0")
}
```
{% endtab %}
{% tab title="Maven" %}
```xml
<dependency>
    <groupId>io.github.alexdev03</groupId>
    <artifactId>unlimitednametags-api-paper</artifactId>
    <version>2.0.0</version>
    <scope>provided</scope>
</dependency>
```
{% endtab %}
{% endtabs %}

> [!NOTE]
> For UUID-only integrations without Paper types, use artifact **`unlimitednametags-api`** instead.
> It is pulled in transitively when you depend on **`api-paper`** — you normally do not need
> **`unlimitednametags-common`** separately.

> [!IMPORTANT]
> **Maven coordinates vs Java packages:** artifacts are published under Maven groupId
> **`io.github.alexdev03`**. Java types live under package **`org.alexdev.unlimitednametags…`**
> — that package name is correct in import statements and is not a dependency typo.

### EntityLib (optional, compile-only)

**`unlimitednametags-api-paper` does not bundle EntityLib.** The artifact exposes only the UNT
public API; EntityLib types are not on your compile classpath unless you add them yourself.

Add EntityLib as **compile-only** when your addon references EntityLib types exposed through UNT
config or low-level display APIs — for example
**`AbstractDisplayMeta.BillboardConstraints`**, display metadata, or similar.

{% tabs %}
{% tab title="Gradle (Kotlin DSL)" %}
```kotlin
repositories {
    mavenCentral()
    maven("https://maven.pvphub.me/tofaa") // required for EntityLib snapshots
}

dependencies {
    compileOnly("io.github.alexdev03:unlimitednametags-api-paper:2.0.0")
    compileOnly("io.github.tofaa2:spigot:3.0.3-SNAPSHOT") // match your UNT release
}
```
{% endtab %}
{% tab title="Maven" %}
```xml
<repositories>
    <repository>
        <id>tofaa</id>
        <url>https://maven.pvphub.me/tofaa</url>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>io.github.alexdev03</groupId>
        <artifactId>unlimitednametags-api-paper</artifactId>
        <version>2.0.0</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>io.github.tofaa2</groupId>
        <artifactId>spigot</artifactId>
        <version>3.0.3-SNAPSHOT</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```
{% endtab %}
{% endtabs %}

> [!WARNING]
> Do **not** shade or bundle EntityLib in your addon JAR. **UnlimitedNameTags** ships EntityLib at
> runtime; your dependency is only for compilation. Use the same EntityLib version as the
> **UnlimitedNameTags** build on your server.

### Plugin Configuration (`plugin.yml`)

```yaml
# Hard dependency (required for startup)
depend: [UnlimitedNameTags]

# Or soft dependency (handle absence gracefully in code)
softdepend: [UnlimitedNameTags]
```

---

## Retrieving the API Instance

{% tabs %}
{% tab title="Paper/Bukkit (recommended)" %}
```java
UNTPaperAPI api = UNTPaperAPI.getInstance();
```
{% endtab %}
{% tab title="Platform-neutral (UUID)" %}
```java
UNTAPI api = UNTAPI.getInstance();
```
{% endtab %}
{% endtabs %}

> [!WARNING]
> Calling `getInstance()` before your plugin's `onEnable()` or when **UnlimitedNameTags** is
> disabled throws `IllegalStateException`. For soft dependencies, guard with
> `Bukkit.getPluginManager().isPluginEnabled("UnlimitedNameTags")` first.

---

## Public API vs internal types

Integrate through **`UNTPaperAPI`** (Paper/Bukkit) or **`UNTAPI`** (UUID-based). For live row
instances, use **`UntNametagDisplay`** via **`getPacketDisplayText(player)`** — see
[Integrations — Direct Display Entity Access](integrations.md).

**Do not depend on internal implementation types** such as **`PacketNameTag`**. They are not part
of the supported addon surface and may change without notice.

---

## Core Types Reference

| Type | Description |
| :--- | :--- |
| **`UNTAPI`** | Core platform-neutral entry point (UUID-based). |
| **`UNTPaperAPI`** | Paper/Bukkit entry point with `Player` overloads, custom animation/glow registration, and forced nametag helpers. |
| **`UnlimitedNameTagsInstancePaper`** | Extended plugin interface via `UNTPaperAPI.paperPlugin()`. |
| **`UntNametagManager` / `UntNametagManagerPaper`** | Override, glow, refresh, and visibility operations (`api.nametagManager()`). |
| **`Settings.NameTag`** | Permission preset and its `displayGroups` list. |
| **`Settings.DisplayGroup`** | One stacked row (text, item, or block), with optional `glow` and `animation` (ITEM/BLOCK only). |
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
