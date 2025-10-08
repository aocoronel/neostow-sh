# The declarative GNU stow

`neostow` is a tool that streamline the process to manage symlinks like GNU `stow`, but using a `neostow` file, instead. It allows more flexible symlink management, enabling the creation of symlinks from a relative source to anywhere on your computer.

This declarative nature allows to easily make reproducible and granular symlinking, unlike `stow`. However, this project does not aims to fully replace it, but to give a declarative feature missing from it.

Other versions of Neostow:

- [aocoronel/neostow-c](https://github.com/aocoronel/neostow-c)
- [aocoronel/neostow-nim](https://github.com/aocoronel/neostow-nim)
- [aocoronel/neostow-rs](https://github.com/aocoronel/neostow-rs)

## Features

- **Flexible symlink creation**: Create symlinks from any relative source to any destination.
- **Per-project file**: Maintain a `.neostow` file per project.
- **Overwrite symlinks**: Optionally overwrite existing symlinks.
- **Remove symlinks**: Easily remove all created symlinks.

## Installation

```bash
git clone https://github.com/aocoronel/neostow-sh.git
chmod +x neostow/src/neostow
sudo cp neostow/src/neostow /usr/local/bin/
```

## Usage

`neostow` reads from a `.neostow` file in the current directory to determine which symlinks to create. The `.neostow` file should contain lines in the following format: `source=destination`.

See the manpage(1) at `FILES` for more details.

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
config/myconfig=/home/username/.config/myconfig/
scripts/myscript.sh=/home/username/bin/myscript/
myfile=$HOME/Downloads
```

## Integrations

### [Just](https://github.com/casey/just)

`just` is a handy way to save and run `neostow` commands from any directory within the project.

In or `justfile`, you may create a recipe like this:

```just
# Neostow: Overwrite
neostow:
  neostow -o
```

Then, from any child directory where this `justfile` was placed, you can just run `just neostow`, and it will run the configured recipe.

## License

This repository is licensed under the MIT License, a very permissive license that allows you to use, modify, copy, distribute and more.
