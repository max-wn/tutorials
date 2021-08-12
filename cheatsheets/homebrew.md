---
title: homebrew
category: macos
layout:
updated:
---

Display brief statistics for your Homebrew installation.

`brew info`

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

to update and upgrade

```bash
brew update
brew outdated
brew upgrade
```

---

THE END
