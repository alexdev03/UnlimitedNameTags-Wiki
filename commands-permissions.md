## 🛠️ **Commands**

The following commands are available for managing and customizing name tags. They are divided into sections based on their function.

---

### Main:
- **`/unt`**: Displays the plugin version and a list of available commands. *(Permission: none)*
- **`/unt reload`**: Reloads the plugin configuration without restarting the server. *(Permission: `unt.reload`)*
- **`/unt debug`**: Performs a debug operation for troubleshooting. *(Permission: `unt.debug`)*

---

### Name Tag Management:
- **`/unt show <player>`**: Displays the name tag for a specific player. *(Permission: `unt.show`)*
- **`/unt hide <player>`**: Hides the name tag for a specific player. *(Permission: `unt.hide`)*
- **`/unt refresh <player>`**: Refreshes the name tag of a specific player for the command sender. *(Permission: `unt.refresh`)*

---

### Customization and Configuration:
- **`/unt billboard <type>`**: Sets the default billboard type (e.g., `CENTER`, `FIXED`, etc.). *(Permission: `unt.billboard`)*
- **`/unt formatter <formatter>`**: Sets the default name tag formatter. *(Permission: `unt.formatter`)*

---

### Managing Other Players' Name Tags:
- **`/unt hideOtherNametags [-h]`**: Hides the name tags of other players. Use the `-h` flag to suppress the confirmation message. *(Permission: `unt.hideOtherNametags`)*
- **`/unt showOtherNametags [-h]`**: Displays the name tags of other players. Use the `-h` flag to suppress the confirmation message. *(Permission: `unt.showOtherNametags`)*

---

## 🔒 **Default Permissions**

The following permissions control access to various features of the plugin. You can configure them in your server's permissions plugin (e.g., PermissionsEx, LuckPerms).

---

### **Core Permissions:**
- **`unt.shownametags`**: Enabled by default. This permission controls whether a player can see other players' name tags. Revoking this permission will hide all other players' name tags globally for the player.
- **`unt.showownnametag`**: Allows the player to see their own name tag. If revoked, the player will not see their own name tag, but other players can still see it.

---

### **Name Tag Management:**
- **`unt.show`**: Grants permission to show a specific player's name tag using the `/unt show <player>` command.
- **`unt.hide`**: Grants permission to hide a specific player's name tag using the `/unt hide <player>` command.
- **`unt.refresh`**: Allows the player to refresh a specific player's name tag using the `/unt refresh <player>` command.

---

### **Customization and Configuration:**
- **`unt.billboard`**: Allows the player to set the default billboard type with the `/unt billboard <type>` command.
- **`unt.formatter`**: Allows the player to set the default name tag formatter using the `/unt formatter <formatter>` command.

---

### **Managing Other Players' Name Tags:**
- **`unt.hideOtherNametags`**: Grants permission to hide the name tags of other players using the `/unt hideOtherNametags` command.
- **`unt.showOtherNametags`**: Grants permission to show the name tags of other players using the `/unt showOtherNametags` command.

--- 

### **Advanced Permissions:**
- **`unt.reload`**: Allows the player to reload the plugin configuration with the `/unt reload` command.
- **`unt.debug`**: Allows the player to perform a debug operation using the `/unt debug` command.


