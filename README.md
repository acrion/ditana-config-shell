# Ditana Config Shell

**Ditana Config Shell** is an Arch Linux package that provides a unified shell configuration for Ditana GNU/Linux. By maintaining a **clear separation between shell-specific and common configurations**, it enables shared settings between bash and zsh, resulting in a cohesive and maintainable shell environment.

## Description

Ditana Config Shell introduces a `.shellrc` file and a `.shell.d` directory to create a unified shell environment. These components allow for shared configurations and scripts that work seamlessly across both bash and zsh shells. This approach keeps shell-specific configuration files focused on truly shell-specific settings.

## Features

- **Unified Configuration File (`.shellrc`)**: Contains configurations common to both bash and zsh.
- **Dedicated Directory (`.shell.d`)**: Houses shell-agnostic scripts automatically sourced at startup.
- **Clear Separation**: Maintains a distinction between shell-specific configurations and common settings.
- **Integration with Ditana Shell Packages**: Works alongside `ditana-config-zsh` and `ditana-config-bash`.
- **Custom Aliases**: Provides helpful aliases without modifying existing terminal commands to avoid unexpected behavior.

### Common Configuration (`.shellrc`)

The `.shellrc` file includes:

1. **Environment Variables**:
   - Sets `JAVA_HOME` and `JAVA_COMPILER` based on the Arch Linux Java environment.
   - Configures `OPENMPI_DIR` if OpenMPI is installed.
   - Adds the CUDA bin directory to `PATH` if present.

2. **PATH Configuration**:
   - Adds user-specific binary directories to the `PATH` (e.g., Go binaries, local bin).

3. **Custom Functions**:
   - `log_system`: Monitors and displays system logs.
   - `safe_rm`: A wrapper around the `rm` command to prevent accidental deletion of the home directory.

4. **Alias Definitions**:
   - `rm`: Aliased to the `safe_rm` function.
   - `open`: Uses `xdg-open` for opening files.
   - `ll`: Enhanced directory listing using `exa`.
   - `rm_orphaned`: Removes orphaned packages.
   - `monitor_dir`: Monitors directory changes using `inotifywait`.
   - `addpkg`: Alias for package installation with system upgrade.
   - `rmpkg`: Alias for package removal with dependency cleanup.

5. **External Script Sourcing**:
   - Sources a system info print script.
   - Automatically sources executable scripts from the `.shell.d/` directory.

#### Custom Aliases for Package Management

This package includes aliases to help manage packages on an Arch-based system, ensuring system consistency and avoiding partial upgrades. Ditana uses [pikaur](https://github.com/actionless/pikaur) as the AUR helper, which delegates to `pacman` for native packages.

##### `addpkg` - Install or Update Packages with System Upgrade

```bash
alias addpkg='pikaur -Syu'
```

**Usage Example:**

```bash
addpkg timeshift
```

**Explanation:**

Running `pikaur -Syu <package>` updates both the package repository and AUR packages before installing or upgrading the specified package. This approach prevents **partial upgrades**, which are unsupported in Arch Linux and can lead to system instability. By using `addpkg`, users maintain a consistent system state, reducing the risk of compatibility issues. For more information, refer to [Partial upgrades are unsupported](https://wiki.archlinux.org/title/System_maintenance#Partial_upgrades_are_unsupported).

##### `rmpkg` - Remove Packages with Dependency Cleanup

```bash
alias rmpkg='pikaur -Rsu'
```

**Usage Example:**

```bash
rmpkg timeshift
```

Using `rmpkg` with the `-Rsu` options removes the specified package and its unneeded dependencies, as well as any other orphaned packages in the system. This ensures that the system remains clean by removing packages that are no longer needed.

- **-R**: Removes the specified target package.
- **-s**: Recursively removes dependencies that are not required by other installed packages.
- **-u**: Additionally removes all unneeded packages, helping to eliminate orphaned packages.

**Note:** The `-c` (cascade) option is not used here because it removes the specified package and anything that depends on it, which could lead to unintended removals. By omitting `-c`, `rmpkg` focuses solely on the target package and its unneeded dependencies without affecting other dependent packages.

## Usage

The package sets up the following structure:

- **`.shellrc`**: Main shared configuration file sourced by both bash and zsh.
- **`.shell.d/`**: Directory for additional shell-agnostic scripts.

Users can add their own shell-agnostic scripts to the `.shell.d/` directory, which will be automatically sourced when a new terminal session starts.

## Integration with Shell-Specific Configurations

This package works in tandem with:

- [ditana-config-zsh](https://github.com/acrion/ditana-config-zsh): For zsh-specific configurations.
- [ditana-config-bash](https://github.com/acrion/ditana-config-bash): For bash-specific configurations.

These packages handle shell-specific settings, while `ditana-config-shell` provides the common ground, resulting in a clean and maintainable shell environment.

## Installation

On Ditana GNU/Linux, this package is installed by default. For manual installation or updates, use:

```bash
pikaur -S ditana-config-shell
```

## Customization

1. Place shell-agnostic scripts in the `.shell.d/` directory.
2. Use the respective shell-specific configuration files for bash or zsh settings.
