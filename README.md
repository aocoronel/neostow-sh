# The declarative GNU stow

> [!WARNING]
> This repository has been moved to Codeberg. Find it at [aocoronel/neostow-sh](https://codeberg.org/aocoronel/neostow-sh).

> [!WARNING]
> `neostow-sh` does not have robust shell injection protections! Do never run this without reviewing the neostow file! Do use sudo with caution!

`neostow-sh` is a POSIX compliant shell script, that does only depends on common Linux utils to operate.

`neostow` is a tool that streamline the process to manage symlinks like GNU `stow`, but using a `neostow` file, instead. It allows more flexible symlink management, enabling the creation of symlinks from a relative source to anywhere on your computer.

This declarative nature allows to easily make reproducible and granular symlinking, unlike `stow`. However, this project does not aims to fully replace it, but to give a declarative feature missing from it.

This tool is useful to keep files and directories organized in a single centralized place, while also having them across the system. Differently, than `stow`, which allows you to place some files into a different target, other than the parent directory, `neostow` aims to further improve this functionality.

With `neostow` each file or directory can be symlinked to a specific part of the system, and not the project as a whole. There is not ignore file, and no need to adjust the folder layout to achieve your goals. If your `neostow` does not explicitly specify an operation, it won't touch a single file.

Other versions of Neostow:

- [aocoronel/neostow-c](https://github.com/aocoronel/neostow-c)
- [aocoronel/neostow-nim](https://github.com/aocoronel/neostow-nim): No longer maintained.
- [aocoronel/neostow-rs](https://github.com/aocoronel/neostow-rs)

## Shell Edition

The shell version is written in POSIX to allow maximum compatibility across all shells, and does not depend on extra utilities other than the common Linux utilities such as the `coreutils`.

### Bugs

Currently, there is no known bugs.

### Vulnerabilities

From all the rewrites of `neostow`, the shell version is the most vulnerable to shell injections, since the parsed data from the `.neostow` file can contain malicious data such as `sudo rm -rf --no-preserve-root /`, and many variations with `||`, `&&` or `;`.

Currently the shell version is able to block these tricks, but maybe not all possible attempts.

## Features

- **Flexible symlink creation**: Create symlinks from any relative source to any destination.
- **Per-project file**: Maintain a `.neostow` file per project.
- **Overwrite symlinks**: Optionally overwrite existing symlinks.
- **Remove symlinks**: Easily remove all created symlinks.
- **Preview operations**: Preview what operations would run.

## Installation

Copy the source:

```bash
git clone https://github.com/aocoronel/neostow-sh.git
cd neostow-sh
```

To install `neostow`:

```bash
chmod +x src/neostow
sudo cp src/neostow /usr/local/bin/
```

To install manpages:

```bash
sudo cp docs/neostow.1 /usr/share/man/man1/
```

To install completions:

```bash
# Bash
sudo cp completions/_neostow.bash /usr/share/bash-completion/completions/
# Zsh
sudo cp completions/_neostow.zsh /usr/share/zsh/site-functions
```

## Usage

`neostow` reads from a `.neostow` file in the current directory to determine which symlinks to create.

The `.neostow` file should contain lines in the following format: `source=destination`.

See the manpage(1) at `FILES` for more details. Or give at look at the [example file](https://github.com/aocoronel/neostow-c/blob/main/test/.neostow) and [Examples](#examples).

```console
neostow | the declarative GNU stow

Usage:  neostow [OPTIONS] <COMMAND>

Commands:
  delete
          Delete symlinks
  edit
          Edit the neostow file

Options:
  -D, --debug
          Enables debug verbosity
  -F, --force
          Skip prompt dialogs
  -V, --verbose
          Enable verbosity
  -f, --file <FILE>
          Load an alternative neostow file
  -d, --dry
          Describe potential operations
  -h, --help
          Displays this message and exits
  -o, --overwrite
          Overwrite existing symlinks
  -v, --version
          Displays program version
```

### Configuration File

The `.neostow` file should be placed in the root of your project directory.

#### Examples

Example `.neostow` file:

```text
config/myconfig.txt=/home/username/.config/myconfig/ # links myconfig.txt to ~/.config/myconfig/
scripts/myscript.sh=/home/username/bin/myscript/ # links myscript.sh to ~/bin/myscrypt/
myfile=$HOME/Downloads # links myfile to ~/Downloads
```

The left side paths are relative to the current directory where the `.neostow` file is found.

## Integrations

### [Just](https://github.com/casey/just)

`just` is a handy way to save and run `neostow` commands from any directory within the project.

Because, `neostow` has the limitation to only find `.neostow` files in the current directory, `just` gives an extra functionality. Integrating both, allows you to run `neostow` from the root of the project, thus finding the `.neostow` file.

In or `justfile`, you may create a recipe like this:

```just
# Neostow: Overwrite
neostow:
  neostow -o
```

Then, from any child directory where this `justfile` was placed, you can just run `just neostow`, and it will run the configured recipe.

## Notes

This program was only tested in a Linux machine.

## License

This repository is licensed under the MIT License, a very permissive license that allows you to use, modify, copy, distribute and more.
