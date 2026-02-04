# MindNode Themes & Theme Creation Skill

Custom themes for [MindNode](https://www.mindnode.com/) plus an **Agent Skill** that teaches AI assistants how to create MindNode themes from any color palette.

## Quick Start

### Install a Theme

Double-click any `.mindnodedynamictheme` file to install it in MindNode.

### Install the Skill

The `mindnode-theme` skill teaches AI agents how to create MindNode themes. Install it in your AI tool of choice:

**Using [skills.sh](https://skills.sh) CLI (easiest):**
```bash
npx skills add mindnode-theme
```

**Manual installation:** Copy the `skills/mindnode-theme` folder to your AI tool's skills directory (see [Installation Paths](#installation-paths) below).

## Included Themes

| Theme | Description |
|-------|-------------|
| `Gruvbox.mindnodedynamictheme` | [Gruvbox](https://github.com/morhetz/gruvbox) color scheme with light/dark mode, per-level styling, and 7 accent colors |
| `mindmanager.mindnodedynamictheme` | MindManager-inspired style |

## The Agent Skill

The `mindnode-theme` skill enables any compatible AI assistant to:

- Convert color palettes (hex, RGB) to MindNode format
- Create dynamic themes with automatic light/dark mode switching
- Configure per-level styling (boxes vs text-only nodes)
- Set up branch color rainbows
- Package themes as `.mindnodedynamictheme` files

### Example Prompts

Once installed, try asking your AI:

- *"Create a MindNode theme using the Dracula color palette"*
- *"Make a Nord-themed mind map style with boxes only for the first two levels"*
- *"Convert my brand colors to a MindNode theme"*

### Installation Paths

| AI Tool | Skills Location |
|---------|-----------------|
| **Claude Code** | `~/.claude/skills/` (global) or `.claude/skills/` (project) |
| **Claude.ai** | Upload via skill icon in chat, or use marketplace |
| **VS Code / Copilot** | `.github/skills/` or `.claude/skills/` in workspace |
| **OpenAI Codex** | `~/.codex/skills/` (user) or `.codex/skills/` (repo) |
| **Cursor** | `.cursor/skills/` or `.claude/skills/` |

### Manual Installation

```bash
# Clone this repo
git clone https://github.com/jguice/mindnode-gruvbox.git
cd mindnode-gruvbox

# Copy to your AI tool's skills directory:

# Claude Code (global)
cp -r skills/mindnode-theme ~/.claude/skills/

# Claude Code (project-local)
cp -r skills/mindnode-theme .claude/skills/

# VS Code / GitHub Copilot
cp -r skills/mindnode-theme .github/skills/

# OpenAI Codex
cp -r skills/mindnode-theme ~/.codex/skills/

# Cursor
cp -r skills/mindnode-theme .cursor/skills/
```

## Skill Contents

```
skills/mindnode-theme/
├── SKILL.md                 # Main skill instructions
├── color-palettes.md        # Pre-converted popular palettes
└── examples/
    ├── dynamic-theme-template.json
    ├── gruvbox-dynamic.json
    └── gruvbox-dark-contents.xml
```

## Learn More

- [Agent Skills Explained](https://www.avanderlee.com/ai-development/agent-skills-replacing-agents-md-with-reusable-ai-knowledge/) - Overview of the Agent Skills standard
- [Claude Code Skills Documentation](https://docs.anthropic.com/en/docs/claude-code/skills)
- [VS Code Agent Skills](https://code.visualstudio.com/docs/copilot/customization/agent-skills)
- [awesome-agent-skills](https://github.com/heilcheng/awesome-agent-skills) - Curated list of skills for AI agents
- [MindNode Theme Guide](https://www.mindnode.com/support/guides/themes) - Official MindNode documentation

## License

MIT
