# Installing Bitpowers for OpenCode

## Prerequisites

- [OpenCode.ai](https://opencode.ai) installed

## Installation

Add bitpowers to the `plugin` array in your `opencode.json` (global or project-level):

```json
{
  "plugin": ["bitpowers@git+https://github.com/pahrizal/bitpowers.git"]
}
```

Restart OpenCode. The plugin installs through OpenCode's plugin manager and
registers all skills.

Verify by asking: "Tell me about your bitpowers"

OpenCode uses its own plugin install. If you also use Claude Code, Codex, or
another harness, install Bitpowers separately for each one.

## Migrating from the old symlink-based install

If you previously installed bitpowers using `git clone` and symlinks, remove the old setup:

```bash
# Remove old symlinks
rm -f ~/.config/opencode/plugins/bitpowers.js
rm -rf ~/.config/opencode/skills/bitpowers

# Optionally remove the cloned repo
rm -rf ~/.config/opencode/bitpowers

# Remove skills.paths from opencode.json if you added one for bitpowers
```

Then follow the installation steps above.

## Usage

Use OpenCode's native `skill` tool:

```
use skill tool to list skills
use skill tool to load bitpowers/brainstorming
```

## Updating

OpenCode installs Bitpowers through a git-backed package spec. Some OpenCode
and Bun versions pin that resolved git dependency in a lockfile or cache, so a
restart may not pick up the newest Bitpowers commit. If updates do not appear,
clear OpenCode's package cache or reinstall the plugin.

To pin a specific version:

```json
{
  "plugin": ["bitpowers@git+https://github.com/pahrizal/bitpowers.git#v5.0.3"]
}
```

## Troubleshooting

### Plugin not loading

1. Check logs: `opencode run --print-logs "hello" 2>&1 | grep -i bitpowers`
2. Verify the plugin line in your `opencode.json`
3. Make sure you're running a recent version of OpenCode

### Windows install issues

Some Windows OpenCode builds have upstream installer issues with git-backed
plugin specs, including cache paths for `git+https` URLs and Bun not finding
`git.exe` even when it works in a normal terminal. If OpenCode cannot install
the plugin, try installing with system npm and pointing OpenCode at the local
package:

```powershell
npm install bitpowers@git+https://github.com/pahrizal/bitpowers.git --prefix "$HOME\.config\opencode"
```

Then use the installed package path in `opencode.json`:

```json
{
  "plugin": ["~/.config/opencode/node_modules/bitpowers"]
}
```

### Skills not found

1. Use `skill` tool to list what's discovered
2. Check that the plugin is loading (see above)

### Tool mapping

When skills reference Claude Code tools:
- `TodoWrite` → `todowrite`
- `Task` with subagents → `@mention` syntax
- `Skill` tool → OpenCode's native `skill` tool
- File operations → your native tools

## Getting Help

- Report issues: https://github.com/pahrizal/bitpowers/issues
- Full documentation: https://github.com/pahrizal/bitpowers/blob/main/docs/README.opencode.md
