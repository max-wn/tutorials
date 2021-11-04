## homebrew cheatsheet

### Commands

| Command                    | Description                 |
| ---                        | ---                         |
| `brew install git`         | Install a package           |
| `brew uninstall git`       | Remove/Uninstall a package  |
| `brew upgrade git`         | Upgrade a package           |
| ---                        | ---                         |
| `brew unlink git`          | Unlink                      |
| `brew link git`            | Link                        |
| `brew switch git 2.5.0`    | Change versions             |
| ---                        | ---                         |
| `brew list --versions git` | See what versions you have  |

### More package commands

| Command                    | Description                 |
| ---                        | ---                         |
| `brew info git`            | List versions, caveats, etc |
| `brew cleanup git`         | Remove old versions         |
| `brew edit git`            | Edit this formula           |
| `brew cat git`             | Print this formula          |
| `brew home git`            | Open homepage               |
| `brew search git`          | Search for formulas         |

### Global commands

| Command         | Description              |
| ---             | ---                      |
| `brew update`   | Update brew and cask     |
| `brew upgrade`  | Upgrade all packages     |
| `brew list`     | List installed           |
| `brew outdated` | What's due for upgrades? |
| `brew doctor`   | Diagnose brew issues     |

### Brew Cask commands

| Command                       | Description                           |
| ---                           | ---                                   |
| `brew install --cask firefox` | Install the Firefox browser           |
| `brew list --cask`            | List installed applications           |

Cask commands are used for interacting with graphical applications.

### uninstall

Uninstall formulae that were only installed as
a dependency of another formula and are now no longer needed.

```bash
brew autoremove [options]
    -n, --dry-run  # list what would be uninstalled, but do not actually uninstall anything.
```

uninstall formula

```bash
brew uninstall <formula>

brew uninstall, rm, remove [options] formula|cask  # Uninstall a formula or cask.
    -f, --force  # Delete all installed versions of formula. Uninstall even if cask is not installed, overwrite existing files and ignore errors when removing files.
    --zap  # Remove all files associated with a cask. May remove files which are shared between applications.
    --ignore-dependencies  # Don’t fail uninstall, even if formula is a dependency of any installed formulae.
    --formula  # Treat all named arguments as formulae.
    --cask  # Treat all named arguments as casks.
```

### Also see

* [Homebrew homepage](https://brew.sh/) _brew.sh_
* [Homebrew docs](https://docs.brew.sh) _docs.brew.sh_

---

THE END
