# pi-skill-installer (Pi Coding Agent Skill)

This repository is a **Pi skill**. It adds a `pi-skill-installer` skill to Pi Coding Agent so Pi can list/install other Pi-compatible skills from GitHub.

Pi skills docs: https://pi.dev/docs/latest/skills

## Install this skill

Choose one method.

### Method 1: Install globally (recommended)

Copy this repo into Pi’s global skills directory using the folder name `pi-skill-installer`.

- macOS/Linux global dir: `~/.pi/agent/skills/`
- Windows global dir: `~/.pi/skills/`

Example (macOS/Linux):

```bash
git clone https://github.com/<owner>/pi-skill-installer.git ~/.pi/agent/skills/pi-skill-installer
```

Example (Windows PowerShell):

```powershell
git clone https://github.com/<owner>/pi-skill-installer.git $env:USERPROFILE\.pi\skills\pi-skill-installer
```

### Method 2: Install per-project

Inside your project, place the skill at:

- `.pi/skills/pi-skill-installer`

Example:

```bash
mkdir -p .pi/skills
git clone https://github.com/<owner>/pi-skill-installer.git .pi/skills/pi-skill-installer
```

### Method 3: Use `--skill` with Pi CLI

If you keep this repo anywhere, pass its path directly:

```bash
pi --skill /absolute/path/to/pi-skill-installer
```

## Verify the skill is available

Restart Pi (or reload skills), then ask Pi:

- “List available Pi skills”
- “Install `brave-search` from `badlogic/pi-skills`”

## What this skill does

The skill drives these scripts in this repo:

- `scripts/list-curated-skills.py`
- `scripts/install-skill-from-github.py`

Direct script examples:

```bash
python scripts/list-curated-skills.py
python scripts/install-skill-from-github.py --repo badlogic/pi-skills --path brave-search
```

## Notes

- Requires Python 3.10+
- Network access required
- `git` is needed for fallback install mode and private-repo workflows

## License

See [LICENSE.txt](LICENSE.txt).
