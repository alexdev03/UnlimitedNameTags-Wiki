# 🎨 **Creating Animations with Built-in Placeholders**

In **UnlimitedNameTags**, you can create animations by using built-in placeholders that return integer or double values. These placeholders help animate properties like color transitions, size, and other aspects of name tags in a smooth, dynamic way.

Here are the placeholders designed for animating colors and effects, which can be used with the **MINIMESSAGE** and **MINEDOWN** formats.

## 🌈 **Available Placeholders for Animations**

### 1. **`#phase-mm#` (MINIMESSAGE for Rainbow tags)**
- **Description**: This placeholder is used for for creating **rainbow** in **MINIMESSAGE** format.
- **Range**: The value ranges from `10` (full offset) to `1` (minimal offset).
- **Usage**: It is used for animating color transitions.
- **Example**:
  ```yaml
  "<rainbow:#phase-mm#>MiniMessage rainbow animation with UnlimitedNameTags %player_name%</rainbow>"
  ```
    - The color in the name tag changes smoothly from red to yellow to green to blue, and so on with `#phase-mm#`.

<figure style="text-align: center;">
  <img src="https://i.imgur.com/nFFL0T6.gif" alt="Rainbow Animation" />
  <figcaption>Rainbow Animation</figcaption>
</figure>

### 2. **`#phase-mm-g#` (MINIMESSAGE for Gradient tags)**
- **Description**: This placeholder is used for creating **gradient** animations in **MINIMESSAGE** format.
- **Range**: The value ranges from `-1.0` (full gradient transition) to `1.0` (no transition), with increments of `0.1`.
- **Usage**: It is used for creating smooth color gradients based on the specified `phase` value.
- **Example**:
  ```yaml
  "<gradient:#5e4fa2:#f79459:red:#phase-mm-g#>MiniMessage gradient animation
        with UnlimitedNameTags %player_name%</gradient> "
  ```
    - In this example, the color in the name tag changes from 5e4fa2 to f79459 with a smooth transition, where `#phase-mm-g#` controls the gradient.

<figure style="text-align: center;">
  <img src="https://i.imgur.com/fZEDmDC.gif" alt="Gradient Animation" />
  <figcaption>Gradient Animation</figcaption>
</figure>

### 3. **`#phase-md#` (MINEDOWN)**
- **Description**: This placeholder is used for animating values in **MINEDOWN** format.
- **Range**: The value ranges from `16777215` (white) to `0` (black).
- **Usage**: Use this placeholder when you want to animate between colors in **MINEDOWN** format.
- **Example**:
  ```yaml
  "&rainbow:#phase-md#&Minedown animation with UnlimitedNameTags %player_name%"
  ```
  - As the value of `#phase-md#` changes, the color in the name tag animates.
<figure style="text-align: center;">
  <img src="https://i.imgur.com/CL4oliC.gif" alt="Mindown Animation" />
  <figcaption>Mindown Animation</figcaption>
</figure>

## Changing the Animation Speed

You can change the speed of the animation by modifying the `taskInterval` setting in the `settings.yml` file. The default value is `20`, which means the animation will run every 20 ticks.
If you want to reduce the animation speed, you can set a lower value, minimum is `1`.

in the `settings.yml` file:
```yaml
taskInterval: 5
```
