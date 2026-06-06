# Player Preferences

Players can toggle name tag visibility client-side with **`/unt preferences`**. Choices are stored in
the player's persistent data and survive relog and server restarts.

For command syntax and permissions, see [Commands & Permissions](../commands-permissions.md#player-preferences).

---

## Available Preferences

| Preference | What it controls |
| :--- | :--- |
| **`seeothers`** | Whether the player sees **other** players' custom name tags. |
| **`showown`** | Whether the player sees **their own** name tag. |
| **`showothers`** | Whether **other** players can see this player's name tag. |

Setting **`seeothers`** to `false` behaves like **`/unt hideOtherNametags`**. Setting it back to
`true` restores visibility (like **`/unt showOtherNametags`**).

---

## Global Settings Interaction

These `settings.yml` keys interact with player preferences:

```yaml
visibility:
  showCurrentNameTag: false
  allowPerPlayerShowOwnWhenGlobalDisabled: false
```

| Setting | Effect |
| :--- | :--- |
| **`showCurrentNameTag`** | When `true`, players with **`unt.showownnametag`** can see their own tag by default. |
| **`allowPerPlayerShowOwnWhenGlobalDisabled`** | When `true`, players can use **`/unt preferences showown`** even if **`showCurrentNameTag`** is `false`. |

> [!IMPORTANT]
> If **`showCurrentNameTag`** is `false` and you want players to toggle their own tag, set
> **`allowPerPlayerShowOwnWhenGlobalDisabled: true`**.

---

## Permissions

| Permission | Default | Purpose |
| :--- | :--- | :--- |
| **`unt.shownametags`** | `true` | Base permission to see other players' tags |
| **`unt.showownnametag`** | `true` | See own tag when globally enabled |
| **`unt.preferences`** | `true` | Use `/unt preferences` on self |
| **`unt.preferences.others`** | `op` | View or change another player's preferences |
