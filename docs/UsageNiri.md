# Using Flameshot on niri

Install `grim` and LayerShellQt alongside Flameshot. Flameshot calls `grim` once for each
connected output and combines the images for `flameshot full`. This also works
when outputs have different scales. It avoids niri's Screenshot portal, which
currently cannot capture more than one output at a time.

Launch Flameshot as a native Wayland application with `XDG_CURRENT_DESKTOP=niri`
and `WAYLAND_DISPLAY` set by the session. The usual commands work:

```sh
flameshot gui
flameshot screen -n 0 -p /tmp/screen.png
flameshot full -p /tmp/desktop.png
```

For a compositor shortcut, add a binding to your niri `binds` section:

```kdl
Print { spawn "flameshot" "gui"; }
```

`flameshot gui` opens the capture editor on every connected output without a
monitor picker. In Flameshot's General settings, **Capture focused niri
monitor** limits the editor to niri's focused output. The `screen` command
captures the focused output by default; use `-n` to choose a numbered output.

When built with LayerShellQt, the editor uses one overlay surface per output.
These overlays do not enter niri's scrolling window layout, so opening the
editor does not scroll existing windows. The build falls back to fullscreen
windows if LayerShellQt is unavailable.

`grim` must be available in the environment where Flameshot runs. If it is
missing, Flameshot reports the missing dependency instead of waiting for an
unusable multi-monitor portal request.
