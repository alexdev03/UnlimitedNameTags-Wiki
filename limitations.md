# Limitations

This page lists known compatibility constraints, unsupported client setups, plugin conflicts, and
upstream bugs that affect **UnlimitedNameTags v2**. For installation requirements, see
[Getting Started](getting-started.md). For common troubleshooting questions, see the
[FAQ](faq.md).

---

## Server platform

| Requirement | Details |
| :--- | :--- |
| **Server software** | **Paper 1.21.4+** (Paper or compatible forks) |
| **Dependency** | **[PacketEvents](https://modrinth.com/plugin/packetevents)** must be installed in `plugins/` |

Spigot-only servers and versions below Paper 1.21.4 are not supported.

---

## Client compatibility

> [!WARNING]
> **Minecraft Java 1.19.4 or newer** is required for clients to render custom display entities.
> Older game versions cannot display these name tags due to client-side engine limitations.

- **ViaBackwards is not supported.** Support requests for clients below 1.19.4 (including setups
  that rely on ViaBackwards) are out of scope. Use a supported client version.
- **ViaVersion** allows older clients to connect to the server and helps the plugin detect viewer
  capabilities, but it cannot backport missing client features such as display entities.

The plugin may use ViaVersion on the server to detect viewer capabilities; that does **not** make
sub-1.19.4 clients supported for displaying these name tags.

---

## Bedrock Edition (Geyser / Floodgate)

> [!NOTE]
> **Bedrock/Geyser** support is only partial. Visual features such as background plates, text
> shadows, multi-line stacks, and opacity settings may not match Java Edition rendering.

For integration details, see [Integrations — Bedrock Edition (Geyser)](integrations/integrations.md#bedrock-edition-geyser).

---

## NPC plugins

Some NPC plugins display the NPC name using the vanilla player name tag instead of a dedicated
nameplate, hologram, or display entity.

When an NPC shares the same username as an online player, the vanilla name tag can conflict with
**UnlimitedNameTags** rendering.

**Workaround:**

1. Set `behavior.disableDefaultNameTag: true` in `settings.yml`.
2. If vanilla name tags remain visible on those NPCs, also set
   `behavior.forceDisableDefaultNameTag: true` to bypass Minecraft's internal team cache.

See [Configuration — `behavior`](configuration.md#behavior-section) and the
[FAQ](faq.md) (vanilla name tag overlap).

---

## Simple Voice Chat

**Simple Voice Chat** is not natively supported. The plugin does not replicate Simple Voice Chat's
built-in voice icons on name tags.

**Optional workaround:**

1. Install [VoiceChatPlaceholders](https://hangar.papermc.io/Tommm/VoiceChatPlaceholders).
2. Use its PlaceholderAPI placeholders in your name tag lines (for example, a speaking indicator or
   microphone state).

> [!NOTE]
> Placeholder-based indicators **will not look identical** to Simple Voice Chat's native icons. To
> match the original icons visually, you need a **custom resource pack** (outside the scope of this
> plugin).

---

## Background see-through

The per-row `background.seeThrough` option renders the text background through opaque blocks when
the display group is visible. This is **not** the same setting as `visibility.throughWallMode`
(which controls whole-tag occlusion: see-through, dimmed, or hidden behind walls). See
[Show While Looking & Through-Wall](features/show-while-looking.md).

When `background.seeThrough` is enabled, behavior may be unreliable due to known upstream issues:

- **[Iris mod](https://github.com/IrisShaders/Iris/issues/2415)**: A bug is currently waiting for an
  upstream fix; it can make the see-through option behave incorrectly on Iris clients.
- **Vanilla client**: A separate vanilla client bug is also waiting for a fix and can affect
  see-through rendering.

These are not plugin bugs. Expect inconsistent results until Mojang and/or Iris address them.

---

## Passenger and riding plugins

Plugins that attach **passengers** to players, or make players **ride** blocks, stairs, or other
entities, can cause name tag stack positioning or visibility issues.

There is no configuration workaround documented for this class of conflict. Treat it as a known
incompatibility when combining those plugins with custom display-based name tags.

---

## Display entity collisions

Collisions on display entities **do not work** as expected. This is a vanilla Minecraft bug:
[MC-110748](https://bugs.mojang.com/browse/MC-110748).

This is not a bug in **UnlimitedNameTags**.
