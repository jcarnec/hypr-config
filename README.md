# Personal Hyprland Configuration

Personal Hyprland/Omarchy configuration overrides.

## Structure

This repository contains custom configurations that override the default Omarchy settings from `~/.local/share/omarchy/default/hypr/`.

- `hyprland.conf` - Main configuration file that sources both defaults and personal overrides
- `bindings.conf` - Custom application keybindings
- `monitors.conf` - Monitor configuration
- `input.conf` - Input device settings
- `looknfeel.conf` - Visual appearance settings
- `autostart.conf` - Autostart applications
- `hyprspace.conf` - Hyprspace plugin configuration (workspace overview)
- Other config files for hypridle, hyprlock, etc.

## Plugins

### Hyprspace
Workspace overview plugin (like macOS Mission Control). Toggle with `Super + Tab`.

The plugin is loaded directly via `plugin =` in `hyprland.conf` to avoid timing issues with `hyprpm reload`. After Hyprland updates, rebuild with:
```bash
hyprpm update
```

## Installation

1. Clone this repo to `~/.config/hypr/`
2. Adjust settings to match your setup (especially `monitors.conf`)
3. Reload Hyprland

## Based on

[Omarchy](https://github.com/basecamp/omarchy) - Arch Linux distribution by Basecamp
