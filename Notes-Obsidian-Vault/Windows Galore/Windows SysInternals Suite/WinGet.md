## Table of Contents

- [Winget Cheatsheet](#winget\cheatsheet)
  - [General Syntax](#General\Syntax)
  - [Main Commands](#Main\Commands)
    - [1. `search`](#1.\`search`)
    - [2. `show`](#2.\`show`)
    - [3. `install`](#3.\`install`)
    - [4. `uninstall`](#4.\`uninstall`)
    - [5. `upgrade`](#5.\`upgrade`)
    - [6. `list`](#6.\`list`)
    - [7. `export`](#7.\`export`)
    - [8. `import`](#8.\`import`)
    - [9. `source`](#9.\`source`)
    - [10. `hash`](#10.\`hash`)
    - [11. `validate`](#11.\`validate`)
    - [12. `settings`](#12.\`settings`)
  - [Common Options](#Common\Options)
  - [Example Workflow](#Example\Workflow)
  - [Winget Command Options Table](#Winget\Command\Options\Table)

# Winget Cheatsheet

`winget` is a package manager for Windows, allowing users to discover, install, upgrade, remove, and configure applications on their Windows 10 and Windows 11 systems. This cheatsheet provides an overview of its commands, arguments, and examples.

## General Syntax
```
winget [command] [options] [parameters]
```

## Main Commands

### 1. `search`
Searches for packages.
```
winget search [query] [options]
```
- **Examples:**
  - `winget search vscode` - Searches for Visual Studio Code.
  - `winget search --name git` - Searches for packages with 'git' in their name.

### 2. `show`
Shows information about a package.
```
winget show [package] [options]
```
- **Examples:**
  - `winget show vscode` - Shows details about Visual Studio Code.
  - `winget show --id Microsoft.VisualStudioCode` - Shows details using the package ID.

### 3. `install`
Installs a package.
```
winget install [package] [options]
```
- **Examples:**
  - `winget install vscode` - Installs Visual Studio Code.
  - `winget install --id Microsoft.VisualStudioCode --silent` - Silently installs the package.

### 4. `uninstall`
Uninstalls a package.
```
winget uninstall [package] [options]
```
- **Examples:**
  - `winget uninstall vscode` - Uninstalls Visual Studio Code.
  - `winget uninstall --id Microsoft.VisualStudioCode` - Uninstalls using the package ID.

### 5. `upgrade`
Upgrades a package.
```
winget upgrade [package] [options]
```
- **Examples:**
  - `winget upgrade vscode` - Upgrades Visual Studio Code.
  - `winget upgrade --all` - Upgrades all upgradable packages.

### 6. `list`
Lists installed packages.
```
winget list [options]
```
- **Examples:**
  - `winget list` - Lists all installed packages.
  - `winget list vscode` - Lists all packages related to Visual Studio Code.

### 7. `export`
Exports a list of installed packages.
```
winget export [file] [options]
```
- **Examples:**
  - `winget export packages.json` - Exports installed packages to a JSON file.

### 8. `import`
Imports packages from a file.
```
winget import [file] [options]
```
- **Examples:**
  - `winget import packages.json` - Installs packages listed in a JSON file.

### 9. `source`
Manages sources for packages.
```
winget source [operation] [options]
```
- **Operations:** `add`, `remove`, `update`, `list`, `reset`
- **Examples:**
  - `winget source add -n MyRepo --url https://example.com/repo` - Adds a new source.

### 10. `hash`
Generates a hash for a package.
```
winget hash [installer] [options]
```
- **Examples:**
  - `winget hash .\path\to\installer.exe` - Generates a hash for the specified installer.

### 11. `validate`
Validates a manifest file.
```
winget validate [manifest] [options]
```
- **Examples:**
  - `winget validate .\path\to\manifest.yaml` - Validates a manifest file.

### 12. `settings`
Opens the winget settings file.
```
winget settings
```

## Common Options
- `-h, --help`: Display help.
- `--version`: Show version information.
- `--info`: Show additional information.
- `--verbose`: Provide additional output.
- `--silent`: Minimize output.
- `--locale <locale>`: Override system locale.

## Example Workflow
1. **Search for a Package:**
   ```
   winget search firefox
   ```
2. **Install a Package:**
   ```
   winget install firefox
   ```
3. **List Installed Packages:**
   ```
   winget list
   ```
4. **Upgrade a Package:**
   ```
   winget upgrade firefox
   ```
5. **Uninstall a Package:**
   ```
   winget uninstall firefox
   ```

## Winget Command Options Table

This table provides an overview of the options available for the `winget` command. Each command might support different subsets of these options.

| Option | Description | Applicable Commands |
| ------ | ----------- | ------------------- |
| `-h`, `--help` | Display help information about a command. | All |
| `--version` | Show the `winget` version information. | All |
| `--info` | Show additional information about a command. | All |
| `--verbose` | Provide more detailed output. | All |
| `--silent` | Minimize output, useful for scripts or automation. | Mostly `install`, `uninstall`, `upgrade` |
| `--accept-source-agreements` | Automatically accept source agreements during operation. | `install`, `upgrade` |
| `--exact` | Requires an exact match for a query. | `search`, `show`, `install`, `upgrade`, `uninstall` |
| `--id` | Specify the package identifier. | `show`, `install`, `upgrade`, `uninstall` |
| `--name` | Specify the package name. | `search`, `show`, `install`, `upgrade`, `uninstall` |
| `--moniker` | Specify the package moniker. | `search`, `show`, `install` |
| `--tag` | Specify a tag associated with the package. | `search`, `show` |
| `--command` | Specify a command associated with the package. | `search`, `show` |
| `--source` | Specify the source to use. | `search`, `show`, `install`, `upgrade`, `list`, `export`, `import` |
| `--count` | Limit the number of results shown. | `search`, `list` |
| `--include-unknown` | Include packages with unknown versions in the output. | `list` |
| `--location` | Specify the install location. | `install` |
| `--version` | Specify the version of the package to install. | `install`, `upgrade` |
| `--scope` | Specify the scope of the installation (`user` or `machine`). | `install` |
| `--override` | Provide additional installation arguments. | `install` |
| `--interactive` | Allows the installer to display a user interface. | `install`, `uninstall` |
| `--force` | Force the operation to reattempt installation or reevaluate available packages. | `install`, `upgrade`, `uninstall` |
| `--all` | Apply the operation to all applicable packages. | `upgrade`, `export` |
| `--include-versions` | Include all available versions of the package. | `show` |
| `--locale` | Override the system locale for the operation. | `install`, `upgrade`, `show` |
| `--accept-package-agreements` | Automatically accept package agreements during installation. | `install` |
| `--accept-source-agreements` | Automatically accept source agreements during the operation. | `install`, `upgrade` |
| `--file` | Specify a file for import or export. | `import`, `export` |
| `--output` | Specify the output format (e.g., `json`, `table`, `list`). | `list`, `search`, `show` |
| `--query` | Provide a search query. | `search` |
| `--required` | Show only required parameters for an operation. | Mostly `show` |
| `--ignore-versions` | Ignore available versions during export. | `export` |
| `--include-architecture` | Include the architecture in the export. | `export` |
| `--export-overwrite` | Overwrite existing files during export. | `export` |
| `--export-ignore-missing` | Ignore missing packages during export. | `export` |
| `--manifest` | Specify a manifest file for validation. | `validate` |
