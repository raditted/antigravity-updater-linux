# antigravity-updater-linux

A minimal shell script to update [Antigravity Desktop](https://antigravity.google) on Linux x64 without opening a browser.

## Why

Antigravity Desktop does not auto-update on Linux. This script fetches the latest release from the official download page, compares it against the last installed version, and installs only when there is something new.

## Install

```bash
curl -fsSL https://raw.githubusercontent.com/raditted/antigravity-updater-linux/main/update-antigravity \
  -o ~/.local/bin/update-antigravity
chmod +x ~/.local/bin/update-antigravity
```

Make sure `~/.local/bin` is on your `PATH`. If not, add this to your shell profile:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

## Usage

```
update-antigravity [options]

Options:
  -n, --dry-run     Show what would happen without making changes
  -y, --yes         Skip all confirmation prompts
      --no-launch   Do not offer to relaunch after update
  -h, --help        Show this help
```

**Check for update (no changes):**
```bash
update-antigravity --dry-run
```

**Update without prompts (for cron / automation):**
```bash
update-antigravity --yes --no-launch
```

## How it works

1. Fetches the download page and extracts the latest Linux x64 `.tar.gz` URL.
2. Reads the installed version from `~/.local/share/Antigravity-x64/.installed-version` (a marker written after each successful install).
3. Skips the download if versions match.
4. If Antigravity is running, asks to stop it before installing (or stops automatically with `--yes`).
5. Downloads with a live progress bar, verifies the archive is a valid gzip file, then extracts.
6. Optionally relaunches Antigravity detached from the terminal so closing the terminal does not kill the app.

## Requirements

- `bash` 4+
- `curl`
- `tar`
- `pgrep` (part of `procps`)

## Install directory

`~/.local/share/Antigravity-x64` — the standard location used by the official installer.

## License

MIT
