# Show while looking

With **`visibility.showWhileLooking: true`** in `settings.yml`, a viewer only sees another player's nametag when **aiming at them** (plus the usual checks: permissions, world, vanish, etc.).

```yaml
visibility:
  showWhileLooking: true
```

The plugin runs periodic checks to update visibility and packets; on very large servers consider the impact on load ([Performance](../performance.md)).

![Example](https://i.imgur.com/VHJ8IxQ.gif)
