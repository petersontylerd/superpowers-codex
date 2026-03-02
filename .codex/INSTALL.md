# Installing Superpowers for Codex

Enable superpowers skills in Codex via native skill discovery. Just clone and symlink.

## How It Works

Codex discovers skills by scanning `~/.agents/skills/` at startup. Superpowers is enabled by making this repo’s `skills/` directory visible there via a single symlink (or Windows junction).

## Prerequisites

- Git

## Installation

1. **Clone the superpowers repository:**
   ```bash
   git clone <YOUR_SUPERPOWERS_CODEX_REPO_URL> ~/.codex/superpowers-codex
   ```

2. **Create the skills symlink:**
   ```bash
   mkdir -p ~/.agents/skills
   ln -s ~/.codex/superpowers-codex/skills ~/.agents/skills/superpowers-codex
   ```

   **Windows (PowerShell):**
   ```powershell
   New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
   cmd /c mklink /J "$env:USERPROFILE\.agents\skills\superpowers-codex" "$env:USERPROFILE\.codex\superpowers-codex\skills"
   ```

3. **Restart Codex** (quit and relaunch the CLI) to discover the skills.

## Migrating from old bootstrap

If you installed superpowers before native skill discovery, you need to:

1. **Update the repo:**
   ```bash
   cd ~/.codex/superpowers-codex && git pull
   ```

2. **Create the skills symlink** (step 2 above) — this is the new discovery mechanism.

3. **Remove the old bootstrap block** from `~/.codex/AGENTS.md` — any block referencing `superpowers-codex bootstrap` is no longer needed.

4. **Restart Codex.**

## Verify

```bash
ls -la ~/.agents/skills/superpowers-codex
```

You should see a symlink (or junction on Windows) pointing to your superpowers skills directory.

## Updating

```bash
cd ~/.codex/superpowers-codex && git pull
```

Skills update instantly through the symlink.

## Uninstalling

```bash
rm ~/.agents/skills/superpowers-codex
```

**Windows (PowerShell):**
```powershell
Remove-Item "$env:USERPROFILE\.agents\skills\superpowers-codex"
```

Optionally delete the clone: `rm -rf ~/.codex/superpowers-codex`.

## Troubleshooting

### Skills not showing up

1. Verify the symlink/junction: `ls -la ~/.agents/skills/superpowers-codex`
2. Verify skills exist: `ls -la ~/.codex/superpowers-codex/skills`
3. Restart Codex (skills are discovered at startup)
