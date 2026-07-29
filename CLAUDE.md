# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a Scoop bucket for Windows package management. The bucket contains application manifests (JSON/YAML files) that define how to install, update, and manage Windows applications.

## Bucket Installation

```powershell
scoop bucket add ajqk https://github.com/jqk/scoopbucket
```

## Architecture

### Script Wrapper Pattern

The `bin/` directory contains wrapper scripts that delegate to Scoop's core scripts in `$env:SCOOP_HOME\bin/`. These wrappers add bucket-specific functionality:

- `Helpers.ps1` provides `Get-RecursiveFolder` for multi-directory manifest processing
- All wrappers auto-detect `$SCOOP_HOME` if not set (via `scoop prefix scoop`)
- Scripts support both single manifest and wildcard operations
- Recursive mode (`-Recurse`) processes manifests across all subdirectories (excluding `.vscode` and `bin`)

### Directory Organization

- `bucket/` - Active application manifests
- `bucket/deprecated/` - Deprecated manifests (moved here instead of deleted)
- `bucket/*` - Other subdirectories supported (e.g., TODO directories)
- Both JSON and YAML manifest formats are supported

### Local Apps (lc_ prefix)

Apps prefixed with `lc_` install from local disk instead of downloading. These require:

- Installer files manually placed in `\Scoop\local_installers` (or use `mklink` to link to correct location)
- Execute commands from the same disk partition as installers
- Install: `scoop install lc_appname.json`
- Uninstall: `scoop uninstall lc_appname`

See `README_LOCAL.md` for complete local app documentation.

### Manifest Structure

Common manifest fields beyond the basic schema:

- `extract_to` - Extract archive to a subdirectory instead of root
- `shortcuts` - Array of `[executable_path, shortcut_name]` pairs for start menu
- `installer` - Object with `script` or `file` keys for custom installation logic
- `post_install` / `pre_install` - PowerShell scripts to run during install
- `checkver` - Version detection configuration (supports `github`, URLs, JSONPath, regex)
- `autoupdate` - Template for updating manifests with `$version` variable

Hash format: `algorithm:hash` where algorithm is `md5`, `sha256`, `sha512`, etc.

For JSON API version detection, use `jsonpath` and named capture groups in regex:

```json
"checkver": {
    "url": "https://api.example.com/versions",
    "jsonpath": "$.versions",
    "regex": "(?sm)Windows.*?https://(?<path>[\\w.-/].*?)-(?<version>[\\d+\\.]+\\d+)(?<suffix>[\\w.-].*?).exe"
}
```

## Essential Commands

### Testing

```powershell
.\bin\test.ps1           # Run Pester tests on all manifests
```

### Version Management

```powershell
.\bin\checkver.ps1 APP_NAME       # Check version
.\bin\checkver.ps1 APP_NAME -u    # Check and update if newer
.\bin\checkver.ps1 APP_NAME -f    # Force update
.\bin\checkver.ps1 -Recurse       # Check all manifests recursively
```

### Hash Verification

```powershell
.\bin\checkhashes.ps1 APP_NAME    # Check hashes
.\bin\checkhashes.ps1 APP_NAME -u # Update mismatched hashes
```

### URL Validation

```powershell
.\bin\checkurls.ps1 APP_NAME      # Check if download URLs are valid
```

### Combined Workflow (Check → Update → Commit → Push)

```powershell
.\bin\checkAndPush.ps1 APP_NAME.json
.\bin\checkAndPush.ps1 APP_NAME.json -ForceUpdate -Hashes
```

### Bulk Operations

```powershell
.\bin\auto-pr.ps1 -Push           # Check all manifests and push updates
.\bin\missing-checkver.ps1        # Find manifests without checkver/autoupdate
```

## Auto-Update Configuration

Manifests should include `checkver` and `autoupdate` properties for automatic version detection:

- `checkver: "github"` - Auto-detect from GitHub releases
- `checkver: "https://example.com/version.txt"` - Custom URL for version detection
- `autoupdate` - Template for updating URLs with `$version` variable

## Schema Validation

JSON manifests are validated against Scoop's schema (configured in `.vscode/settings.json`):

- Schema URL: `https://raw.githubusercontent.com/lukesampson/scoop/master/schema.json`
- Applied to: `bucket/*.json` files
- VSCode extension `EditorConfig.EditorConfig` and `ms-vscode.PowerShell` recommended

## CI/CD

- **AppVeyor** (`.appveyor.yml`) - Tests manifests with PowerShell 5 and 7, skips changes to `.md` files and `.vscode/` directory
- **GitHub Actions** - Automated issue verification, PR validation, and scheduled checks

## Commit Message Format

When using `checkAndPush.ps1` or manual commits:

- Version updates: `APP_NAME: Bumped to X.X.X`
- Hash fixes: `APP_NAME: Fixed hashes`

## Token Optimization with RTK

This repository uses PowerShell extensively. Consider using RTK (Rust Token Killer) prefix for token optimization:

- `rtk .\bin\test.ps1` instead of `.\bin\test.ps1`
- `rtk .\bin\checkver.ps1 APP_NAME` instead of `.\bin\checkver.ps1 APP_NAME`
- RTK works with all PowerShell commands and provides 60-90% token reduction on output
- See global CLAUDE.md for complete RTK command reference
