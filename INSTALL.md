# Installing Superpowers

Enable Superpowers skills in your coding agent via native skill discovery. Just clone and symlink.

## How It Works

Your coding agent discovers skills by scanning `~/.agents/skills/` at startup. Superpowers is enabled by making this repo’s `skills/` directory visible there via a single symlink (or Windows junction).

## Prerequisites

- Git

## Installation

1. **Clone the Superpowers repository:**
   ```bash
   git clone <YOUR_SUPERPOWERS_REPO_URL> <SUPERPOWERS_DIR>
   ```

2. **Create the skills symlink:**
   ```bash
   mkdir -p ~/.agents/skills
   ln -s <SUPERPOWERS_DIR>/skills ~/.agents/skills/superpowers
   ```

   **Windows (PowerShell):**
   ```powershell
   New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
   cmd /c mklink /J "$env:USERPROFILE\.agents\skills\superpowers" "<SUPERPOWERS_DIR>\\skills"
   ```

3. **Restart your coding agent** (quit and relaunch it) to discover the skills.

## Migrating from old bootstrap

If you installed Superpowers before native skill discovery, you need to:

1. **Update the repo clone:**
   ```bash
   cd <SUPERPOWERS_DIR> && git pull
   ```

2. **Create the skills symlink** (step 2 above) — this is the discovery mechanism.

3. **Remove the old bootstrap block** from your agent config (any block referencing a `bootstrap` command for Superpowers is no longer needed).

4. **Restart your coding agent.**

## Verify

```bash
ls -la ~/.agents/skills/superpowers
```

You should see a symlink (or junction on Windows) pointing to your Superpowers skills directory.

## Updating

```bash
cd <SUPERPOWERS_DIR> && git pull
```

Restart your coding agent after updating (skills are discovered at startup; a restart is the safest way to ensure changes are picked up).

## Uninstalling

```bash
rm ~/.agents/skills/superpowers
```

**Windows (PowerShell):**
```powershell
Remove-Item "$env:USERPROFILE\.agents\skills\superpowers"
```

Optionally delete the clone directory (wherever you cloned it).

## Troubleshooting

### Skills not showing up

1. Verify the symlink/junction: `ls -la ~/.agents/skills/superpowers`
2. Verify skills exist in your clone: `ls -la <SUPERPOWERS_DIR>/skills`
3. Restart your coding agent (skills are discovered at startup)
