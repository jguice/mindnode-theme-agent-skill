# MindNode Gruvbox Theme

Custom [Gruvbox](https://github.com/morhetz/gruvbox) color scheme theme for [MindNode](https://www.mindnode.com/).

## Installation

Double-click `Gruvbox.mindnodedynamictheme` to install. The theme will appear in MindNode's "Dynamic Themes" section.

## Features

- **Light/Dark mode support**: Automatically switches between Gruvbox light and dark palettes based on system appearance
- **Per-level styling**: Root, level 1, and level 2 nodes have rounded rectangle boxes; level 3+ are text-only with underlines
- **7 branch colors**: Full Gruvbox accent palette (red, orange, yellow, green, aqua, blue, purple) for branch rainbow effect
- **Bold borders**: 4-6pt border widths for clear visual hierarchy

## Theme Files

| File | Description |
|------|-------------|
| `Gruvbox.mindnodedynamictheme` | Main dynamic theme with light/dark support |
| `mindmanager.mindnodedynamictheme` | MindManager-style theme |

## Reference Files

| File | Description |
|------|-------------|
| `natural.mindnodetheme` | Reference static theme from MindNode |
| `iThoughtsX MindManager Style*.itmz-style` | Reference style files from iThoughtsX |

## Creating Your Own Themes

See `.claude/skills/mindnode-theme/SKILL.md` for documentation on creating MindNode themes, including:
- Dynamic theme JSON format
- Color conversion (hex to RGBA)
- Per-level styling with `levelOverrides`
- Complete property reference

Example templates are in `.claude/skills/mindnode-theme/examples/`.
