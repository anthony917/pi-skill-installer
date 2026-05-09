---
name: pi-skill-installer
description: Install Pi Coding Agent skills into Pi skill locations from the Pi skills catalog or a GitHub repo path. Use when a user asks to list available Pi skills, install a Pi skill, or install an Agent Skills-compatible skill from another repository.
metadata:
  short-description: Install Pi Coding Agent skills from Pi skill repositories
---

# Pi Skill Installer

Helps install skills for Pi Coding Agent. By default these are from https://github.com/badlogic/pi-skills, but users can also provide other Agent Skills-compatible GitHub locations.

Use the helper scripts based on the task:
- List Pi skills when the user asks what is available, or if the user uses this skill without specifying what to do.
- Install from the Pi skills catalog when the user provides a skill name.
- Install from another GitHub repo/path when the user provides one, including private repos.

Install skills with the helper scripts.

## Pi Skill Locations

Pi loads skills from multiple places:
- Global:
  - Mac/Linux: `~/.pi/agent/skills`
  - Win: `$env:USERPROFILE\.pi\skills` and `$env:USERPROFILE\.agents\skills`
- Project: `.pi/skills/` and `.agents/skills/`
- Packages: `skills/` directories or `pi.skills` entries in `package.json`
- Settings: `skills` array entries in Pi settings
- CLI: repeated `--skill <path>` arguments

By default, this installer writes to Pi's primary global skills directory for the current platform. Users can override the destination with:
- `PI_SKILLS_DIR=/path/to/skills`
- `--dest /path/to/skills`

Use project destinations such as `.pi/skills` when the user asks for project-local installation.

## Communication

When listing Pi skills, output approximately as follows, depending on the context of the user's request:
"""
Skills from {repo}:
1. skill-1
2. skill-2 (already installed)
3. ...
Which ones would you like installed?
"""

After installing a skill, tell the user: "Restart Pi or reload skills to pick up new skills."

## Scripts

All of these scripts use network, so when running in the sandbox, request escalation when running them.

- `scripts/list-curated-skills.py` (prints the Pi skills catalog with installed annotations)
- `scripts/list-curated-skills.py --format json`
- `scripts/install-skill-from-github.py --repo <owner>/<repo> --path <path/to/skill> [<path/to/skill> ...]`
- `scripts/install-skill-from-github.py --url https://github.com/<owner>/<repo>/tree/<ref>/<path>`

Examples:
- `scripts/list-curated-skills.py`
- `scripts/install-skill-from-github.py --repo badlogic/pi-skills --path brave-search`
- `scripts/install-skill-from-github.py --url https://github.com/badlogic/pi-skills/tree/main/youtube-transcript`
- `scripts/install-skill-from-github.py --repo owner/repo --path skills/custom-skill --dest .pi/skills`

## Behavior and Options

- Defaults to direct download for public GitHub repos.
- If download fails with auth/permission errors, falls back to git sparse checkout.
- Aborts if the destination skill directory already exists.
- Installs into the platform default Pi global skills directory by default.
- Multiple `--path` values install multiple skills in one run, each named from the path basename unless `--name` is supplied.
- Options: `--ref <ref>` (default `main`), `--dest <path>`, `--method auto|download|git`.
- Destination skill names must follow Pi's Agent Skills name rules: lowercase letters, digits, and hyphens only; 1-64 characters; no leading, trailing, or consecutive hyphens.

## Notes

- The default skill list is fetched from `https://github.com/badlogic/pi-skills` via the GitHub API. If it is unavailable, explain the error and exit.
- Private GitHub repos can be accessed via existing git credentials or optional `GITHUB_TOKEN`/`GH_TOKEN` for download.
- Git fallback tries HTTPS first, then SSH.
- Pi can also use existing Claude Code or Codex skill directories if the user adds those directories to Pi settings.
- Installed annotations come from the configured Pi skills directory.
