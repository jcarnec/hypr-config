# Hyprland Configuration for Omarchy

Personal Hyprland configuration override repository that extends the Omarchy Linux distribution with custom keybindings and settings.

## Overview

This repository customizes Hyprland (a dynamic tiling Wayland compositor) while maintaining Omarchy's upstream configuration. All personal customizations are isolated in `~/.config/hypr/` and don't interfere with Omarchy updates from `~/.local/share/omarchy/default/hypr/`.

## Key Features

### Vim-Style Window Navigation
- `SUPER + h/j/k/l` - Move focus left/down/up/right
- `SUPER + SHIFT + h/j/k/l` - Move window left/down/up/right
- `SUPER + v` - Toggle window split (rebinded from SUPER+j to avoid conflicts)

### Workspace Overview (Hyprspace)
- `SUPER + TAB` - Toggle Hyprspace workspace overview (like macOS Mission Control)

### Application Launchers
Quick launch keybindings for common applications:
- `SUPER + RETURN` - Terminal
- `SUPER + SHIFT + F` - File Manager
- `SUPER + SHIFT + B` - Browser
- `SUPER + SHIFT + M` - Music (Spotify)
- `SUPER + SHIFT + N` - Editor
- `SUPER + SHIFT + O` - Obsidian
- `SUPER + SHIFT + D` - Docker (lazydocker)
- `SUPER + SHIFT + T` - Activity Monitor (btop)

### Web Apps & Services
- `SUPER + SHIFT + A` - ChatGPT
- `SUPER + SHIFT + E` - Email (Hey.com)
- `SUPER + SHIFT + Y` - YouTube
- `SUPER + SHIFT + P` - Google Photos
- `SUPER + SHIFT + X` - X/Twitter
- Other services: Signal, WhatsApp, Calendar, 1Password, etc.

## Configuration Structure

| File | Purpose |
|------|---------|
| `bindings.conf` | Custom keybindings (vim-style, app launchers) |
| `monitors.conf` | Display/monitor configuration |
| `input.conf` | Input device settings (keyboard, mouse) |
| `looknfeel.conf` | Visual appearance (colors, fonts, theme) |
| `autostart.conf` | Applications to launch at startup |
| `hypridle.conf` | Idle behavior and locking settings |
| `hyprlock.conf` | Lock screen configuration |
| `hyprsunset.conf` | Nightlight settings |
| `hyprspace.conf` | Hyprspace plugin settings (overview) |
| `xdph.conf` | XWayland settings |
| `shaders/` | GLSL shader files for visual effects |

## How It Works

The configuration uses Hyprland's source system to layer configurations:

1. **Omarchy Defaults** (sourced first, don't edit):
   - `~/.local/share/omarchy/default/hypr/` - Base configuration
   - Includes media keys, clipboard, tiling, utilities, etc.

2. **Personal Overrides** (sourced second, edit these):
   - `~/.config/hypr/` - Your customizations
   - Automatically override any Omarchy defaults with the same binding

## Configuration Details

### Vim-Style Keybindings
This config prioritizes vim-style navigation over Omarchy defaults:

```conf
# Window focus
bind = SUPER, h, movefocus, l        # Focus left
bind = SUPER, j, movefocus, d        # Focus down
bind = SUPER, k, movefocus, u        # Focus up
bind = SUPER, l, movefocus, r        # Focus right

# Window movement
bind = SUPER SHIFT, h, movewindow, l
bind = SUPER SHIFT, j, movewindow, d
bind = SUPER SHIFT, k, movewindow, u
bind = SUPER SHIFT, l, movewindow, r
```

### Keybinding Menu Rebinding
The Omarchy default binds `SUPER + K` to show the keybindings menu. This conflicts with vim-style 'k' (focus up). Solution:

```conf
unbind = SUPER, K
bindd = SUPER ALT, K, Show key bindings, exec, omarchy-menu-keybindings
```

Now:
- `SUPER + K` = Move focus up (vim-style)
- `SUPER + ALT + K` = Show keybindings menu (moved from SUPER + K)

### Adding New Bindings

To add or modify keybindings:

1. Add to `bindings.conf` (not the Omarchy defaults)
2. Use `unbind` first if overriding an existing Omarchy binding
3. Use `bindd` for documented bindings (shows in the menu)
4. Use `bind` for non-documented bindings

Example:
```conf
unbind = SUPER ALT, SPACE              # Remove Omarchy menu
bindd = SUPER ALT, SPACE, My Menu, exec, my-custom-menu
```

### Web App URLs
If your web app URL contains `#`, use `##` to prevent Hyprland treating it as a comment:
```conf
bindd = SUPER SHIFT, X, My App, exec, omarchy-launch-webapp "https://example.com/app##feature"
```

## Sourcing Order

Config files are sourced in this order (later overrides earlier):
1. `~/.local/share/omarchy/default/hypr/` (Omarchy defaults)
2. `~/.config/omarchy/current/theme/hyprland.conf` (Theme)
3. `~/.config/hypr/` (Personal overrides - THIS REPO)

This ensures your customizations take precedence over both Omarchy defaults and theme configurations.

## Common Tasks

### Check Available Keybindings
Press `SUPER + ALT + K` to open the interactive keybindings menu (moved from SUPER + K).

### View System Menu
`SUPER + ESCAPE` opens the Omarchy system menu (lock, logout, shutdown, etc.)

### Take Screenshots
- `PRINT` - Screenshot with editing
- `SHIFT + PRINT` - Screenshot to clipboard
- `SUPER + PRINT` - Color picker

### Toggle Features
- `SUPER + BACKSPACE` - Toggle window transparency
- `SUPER + SHIFT + SPACE` - Toggle top bar
- `SUPER + CTRL + SPACE` - Next background in theme

## Plugins

### Hyprspace
Workspace overview plugin providing a bird's-eye view of all workspaces (similar to macOS Mission Control).

**Loading mechanism**: The plugin is loaded dynamically via `autostart.conf` rather than static config directives. This is required because:
- Hyprland parses all config files before executing `exec-once` commands
- Bindings referencing plugin dispatchers fail if parsed before the plugin loads
- `plugin =` directive and `hyprpm reload` both have timing issues
- Solution: Load plugin via `hyprctl plugin load`, then add binding via `hyprctl keyword`

**Plugin location**: `/var/cache/hyprpm/josephcarnec/Hyprspace/Hyprspace.so`

**After Hyprland updates**: Run `hyprpm update` to rebuild plugins against the new Hyprland version.

**Configuration**: See `hyprspace.conf` for plugin-specific settings (animation speed, blur, workspace layout, etc.)

## Dependencies

- Hyprland
- Omarchy Linux distribution
- uwsm (for app launching)
- hyprpicker (color picker)
- nautilus (file manager, configurable)
- Hyprspace plugin (via hyprpm)
- Various omarchy-* utilities

## Git Repository

- **Remote**: `git@github.com:jcarnec/hypr-config.git`
- **Branch**: master
