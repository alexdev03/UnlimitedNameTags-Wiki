# Show while looking

With **`showWhileLooking`** turned **on**, someone only sees another player’s nametag when they are **aiming at** them (usual permission / world / vanish rules still apply).

In newer config layouts this sits under **`visibility:`**; in older files it may be a single line near the top.

```yaml
visibility:
  showWhileLooking: true
```

```yaml
# older flat file
showWhileLooking: true
```

This mode runs extra checks on a timer — on huge servers you may want to read [Performance](../performance.md) if TPS dips.

![Example](../assets/show-while-looking.gif)
