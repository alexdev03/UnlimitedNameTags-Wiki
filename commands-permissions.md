## 🛠️ **Commands**

The following commands are available for managing and customizing name tags. They are divided into sections based on their function.

---

### Main:

### <mark style="color:orange;">`/unt`</mark>
* **Description**: Displays the plugin version and a list of available commands.
* **Usage**: <mark style="color:orange;">`/unt`</mark>
* **Example**: <mark style="color:orange;">`/unt`</mark>
* **Behavior**: Displays the plugin version and a list of commands.
* **Permission**: <mark style="color:yellow;">*none*</mark>

---

### <mark style="color:orange;">`/unt reload`</mark>
* **Description**: Reloads the plugin configuration without restarting the server.
* **Usage**: <mark style="color:orange;">`/unt reload`</mark>
* **Example**: <mark style="color:orange;">`/unt reload`</mark>
* **Behavior**: Reloads the plugin settings.
* **Permission**: <mark style="color:yellow;">`unt.reload`</mark>

---

### <mark style="color:orange;">`/unt debug`</mark>
* **Description**: Performs a debug operation for troubleshooting.
* **Usage**: <mark style="color:orange;">`/unt debug`</mark>
* **Example**: <mark style="color:orange;">`/unt debug`</mark>
* **Behavior**: Initiates the debugging process for troubleshooting issues.
* **Permission**: <mark style="color:yellow;">`unt.debug`</mark>

---

### Name Tag Management:

### <mark style="color:orange;">`/unt show <player>`</mark>
* **Description**: Displays the name tag for a specific player.
* **Usage**: <mark style="color:orange;">`/unt show <player>`</mark>
    * <mark style="color:green;">`<player>`</mark>: The name of the target player.
* **Example**: <mark style="color:orange;">`/unt show AlexDev`</mark>
* **Behavior**: Displays the name tag for the specified player.
* **Permission**: <mark style="color:yellow;">`unt.show`</mark>

---

### <mark style="color:orange;">`/unt hide <player>`</mark>
* **Description**: Hides the name tag for a specific player.
* **Usage**: <mark style="color:orange;">`/unt hide <player>`</mark>
    * <mark style="color:green;">`<player>`</mark>: The name of the target player.
* **Example**: <mark style="color:orange;">`/unt hide AlexDev`</mark>
* **Behavior**: Hides the name tag for the specified player.
* **Permission**: <mark style="color:yellow;">`unt.hide`</mark>

---

### <mark style="color:orange;">`/unt refresh <player>`</mark>
* **Description**: Refreshes the name tag of a specific player for the command sender.
* **Usage**: <mark style="color:orange;">`/unt refresh <player>`</mark>
    * <mark style="color:green;">`<player>`</mark>: The name of the target player.
* **Example**: <mark style="color:orange;">`/unt refresh AlexDev`</mark>
* **Behavior**: Refreshes the name tag to reflect any recent changes.
* **Permission**: <mark style="color:yellow;">`unt.refresh`</mark>

---

### Customization and Configuration:

### <mark style="color:orange;">`/unt billboard <type>`</mark>
* **Description**: Sets the default billboard type (e.g., `CENTER`, `FIXED`, etc.).
* **Usage**: <mark style="color:orange;">`/unt billboard <type>`</mark>
    * <mark style="color:green;">`<type>`</mark>: The billboard type to be used.
* **Example**: <mark style="color:orange;">`/unt billboard CENTER`</mark>
* **Behavior**: Sets the type of billboard to display with the name tag.
* **Permission**: <mark style="color:yellow;">`unt.billboard`</mark>

---

### <mark style="color:orange;">`/unt formatter <formatter>`</mark>
* **Description**: Sets the default name tag formatter.
* **Usage**: <mark style="color:orange;">`/unt formatter <formatter>`</mark>
    * <mark style="color:green;">`<formatter>`</mark>: The format to be used (e.g., `MINIMESSAGE`, `MINEDOWN`, etc.).
* **Example**: <mark style="color:orange;">`/unt formatter MINIMESSAGE`</mark>
* **Behavior**: Sets the name tag format for displaying text.
* **Permission**: <mark style="color:yellow;">`unt.formatter`</mark>

---

### Managing Other Players' Name Tags:

### <mark style="color:orange;">`/unt hideOtherNametags [-h]`</mark>
* **Description**: Hides the name tags of other players. Use the `-h` flag to suppress the confirmation message.
* **Usage**: <mark style="color:orange;">`/unt hideOtherNametags [-h]`</mark>
    * <mark style="color:green;">`[-h]`</mark>: Flag to suppress confirmation.
* **Example**: <mark style="color:orange;">`/unt hideOtherNametags`</mark>
* **Behavior**: Hides name tags of all players except the sender.
* **Permission**: <mark style="color:yellow;">`unt.hideOtherNametags`</mark>

---

### <mark style="color:orange;">`/unt showOtherNametags [-h]`</mark>
* **Description**: Displays the name tags of other players. Use the `-h` flag to suppress the confirmation message.
* **Usage**: <mark style="color:orange;">`/unt showOtherNametags [-h]`</mark>
    * <mark style="color:green;">`[-h]`</mark>: Flag to suppress confirmation.
* **Example**: <mark style="color:orange;">`/unt showOtherNametags`</mark>
* **Behavior**: Displays name tags of all players.
* **Permission**: <mark style="color:yellow;">`unt.showOtherNametags`</mark>


## 🔒 **Default Permissions**

The following permissions control access to various features of the plugin. You can configure them in your server's permissions plugin (e.g., PermissionsEx, LuckPerms).

---

### **Core Permissions:**
- **<mark style="color:yellow;">`unt.shownametags`</mark>**: Enabled by default. This permission controls whether a player can see other players' name tags. Revoking this permission will hide all other players' name tags globally for the player.
- **<mark style="color:yellow;">`unt.showownnametag`</mark>**: Allows the player to see their own name tag. If revoked, the player will not see their own name tag, but other players can still see it.
