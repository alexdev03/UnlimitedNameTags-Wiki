# ❓ Frequently Asked Questions (FAQ)

Find answers to common questions about installation, configuration, compatibility, and optimization
for **UnlimitedNameTags v2.x**.

---

## 💾 Installation & Dependencies

<details>
<summary><b>What are the plugin dependencies?</b></summary>

- **PacketEvents**: Required. Install it like any other plugin in the `plugins/` directory and restart the server.
- **Paper 1.21.4+**: Required for the core plugin to load and execute correctly.
</details>

<details>
<summary><b>Where are the configuration files located?</b></summary>

- **Primary Settings**: Managed via `plugins/UnlimitedNameTags/settings.yml`. This file is generated
  automatically upon the first successful startup.
- **Helmet Height Rules**: Managed via the optional `plugins/UnlimitedNameTags/advanced.yml`
  configuration (which is not generated automatically).
</details>

<details>
<summary><b>How do I reload the configuration?</b></summary>

- Run the command `/unt reload` (in-game or via the console) to reload both `settings.yml` and
  `advanced.yml` without restarting the server.
</details>

---

## 🔄 Version Compatibility

<details>
<summary><b>Why was a backup of my configuration created automatically?</b></summary>

- **Automatic Schema Migration**: The plugin automatically migrates `settings.yml` when upgrading to a
  version with a newer configuration schema. It saves a copy of your prior configuration as
  `settings.yml.backup-<timestamp>.yml` before writing the migrated file.
</details>

<details>
<summary><b>Is manual configuration update required after upgrading?</b></summary>

- **Usually No**: The built-in migrator automatically updates old flat settings to the modern v4 layout.
- **Exception**: If you used the legacy `modifiers:` list in v1, you must manually rewrite them using
  the new `when:` condition syntax. For details, refer to the [Migration Guide](migration.md).
</details>

<details>
<summary><b>Why are custom name tags invisible to certain players?</b></summary>

> [!WARNING]
> **Minecraft Java 1.19.4 or newer** is required to render these display entities. Clients running
> versions older than 1.19.4 are physically unable to render custom name tags due to client-side
> engine limitations.
</details>

<details>
<summary><b>Does ViaVersion allow older clients to render custom name tags?</b></summary>

- **No**: While ViaVersion allows older client versions to connect to the server, it cannot backport
  missing features (like display entities) to legacy game clients.
</details>

<details>
<summary><b>Are Bedrock Edition clients supported via Geyser?</b></summary>

> [!NOTE]
> **Bedrock/Geyser** support is only partial. Visual features such as multiple lines, custom
> backgrounds, text shadows, or opacity settings may not align perfectly with Java rendering.
</details>

---

## 📐 Layout & Conditions

<details>
<summary><b>How do I migrate legacy linesGroups or modifiers?</b></summary>

- The modern configuration format uses `displayGroups` to represent stacked rows. To conditionally show
  or hide rows, define a logical condition string in the `when:` key of each group. (See [Display
  Groups](features/display-groups.md)).
</details>

<details>
<summary><b>How do I conditionally display a name tag row?</b></summary>

- Define the `when:` option under the target display group. You can use standard comparison operators
  and placeholders (e.g., `when: '%vault_eco_balance% > 1000'`).
</details>

<details>
<summary><b>Why are lines ignored in ITEM or BLOCK display groups?</b></summary>

- If a group's `displayType` is set to `ITEM` or `BLOCK`, the visual elements are rendered using the
  `itemMaterial` or `blockMaterial` fields. The `lines` list is ignored and can be left empty.
</details>

<details>
<summary><b>What is the difference between group-level and line-level visibility conditions?</b></summary>

- **Group-level `when:`**: Hides the entire row entity when the condition is false.
- **Line-level `when:`**: Hides only that specific text line.
- If a display group is hidden by a group-level condition, its line-level conditions are not evaluated.
</details>

<details>
<summary><b>Can players customize their own name tag visibility?</b></summary>

- **Yes**: Ensure `allowPerPlayerShowOwnWhenGlobalDisabled: true` is configured under `visibility:` in
  `settings.yml` and grant players the `unt.preferences` permission. Players can then toggle their
  settings in-game using the command `/unt preferences showown <true/false>`.
</details>

---

## 🧩 PlaceholderAPI

<details>
<summary><b>How can I resolve delayed placeholder updates?</b></summary>

- Placeholders refresh periodically on the main plugin task. Increase their update rate by lowering the
  global `behavior.taskInterval` ticks in `settings.yml`, keeping in mind this increases CPU usage. (See
  [Performance Tuning](performance.md)).
</details>

<details>
<summary><b>How do I enable relational (viewer vs. target) placeholders?</b></summary>

- Set `performance.enableRelationalPlaceholders: true` in your `settings.yml` configuration.

> [!WARNING]
> Relational placeholders require per-viewer computations and will increase server CPU usage,
> especially on high-population servers.
</details>

<details>
<summary><b>How can I format or customize raw placeholder outputs?</b></summary>

- Define rules in the `placeholdersReplacements` section to translate raw placeholder returns (e.g.,
  translating `Yes`/`No` outputs to formatted indicators).

> [!IMPORTANT]
> Because YAML parses terms like `Yes`, `No`, `On`, `Off`, `True`, and `False` as booleans, **always
> enclose these placeholders in quotation marks** (e.g., `"Yes"`, `"No"`).

For details, refer to the [Placeholder Replacements Guide](features/placeholders-replacements.md).
</details>

---

## 🎨 Look & Behavior

<details>
<summary><b>How do I prevent vanilla name tags from overlapping?</b></summary>

- Set `behavior.disableDefaultNameTag: true` in `settings.yml`. If vanilla name tags remain visible on
  custom NPCs, set `behavior.forceDisableDefaultNameTag: true`.
</details>

<details>
<summary><b>How can players see their own name tags?</b></summary>

- Set `visibility.showCurrentNameTag: true` in `settings.yml` and grant the player the permission node
  `unt.showownnametag`.
</details>

<details>
<summary><b>How do I adjust name tag height for custom helmets or items?</b></summary>

- Automatic height hooks are provided for popular item plugins (such as Nexo or Oraxen). For other custom
  assets, adjust offset heights manually using rules in `advanced.yml`. (See the [Integrations
  Guide](integrations/integrations.md)).
</details>

---

## ⚡ Performance Tuning

<details>
<summary><b>How do I optimize through-wall occlusion performance?</b></summary>

> [!WARNING]
> Line-of-sight and through-wall checks require raycast calculations on the primary server thread. If
> enabling through-wall occlusion (`throughWallMode` set to `OBSCURED` or `HIDE`) impacts performance,
> increase `visibility.throughWallSettings.checkInterval` to `10` or `20` ticks in `settings.yml`. This
> decreases calculation frequency with negligible visual impact.
</details>

<details>
<summary><b>How do I optimize performance on a high-population server?</b></summary>

- Refer to the [Performance Tuning Guide](performance.md) for full details.
- Key actions include:
  1. Increasing `behavior.taskInterval` (e.g., to `20` or higher).
  2. Setting `behavior.format` to `MINIMESSAGE`.
  3. Disabling unused line-of-sight check features.
  4. Optimizing placeholder updates via `performance.placeholderUpdateRates`.
</details>

<details>
<summary><b>How do I optimize animation performance?</b></summary>

- Increase the global `behavior.displayAnimationInterval` or configure custom `animationInterval`
  values on specific rows.
- Define the `cullBeyondBlocks` parameter in your `animation:` configurations to skip animation updates
  when players are distant.
</details>

---

## 🛠️ Support & Diagnostics

<details>
<summary><b>Where can I request official support?</b></summary>

- Join our official **[Discord Server](https://discord.gg/W4Fu8fqCKs)**.
- When reporting issues, please provide your Spigot/Paper version, UnlimitedNameTags version, relevant
  configuration snippets (ensuring you omit sensitive info), and steps to reproduce.
</details>

<details>
<summary><b>What is the difference between /unt debugger and /unt debug?</b></summary>

- **`/unt debugger <true/false>`**: Toggles persistent, continuous debug logging to the server console.
- **`/unt debug`**: Executes a single diagnostic pass for the player who ran the command.
- Both commands require the staff permission node `unt.debug`.
</details>
