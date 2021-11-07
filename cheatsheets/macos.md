## macos cheatsheet

```bash
diskutil list  # return names of disks (now flash is "disk2")
```

### format flash

```bash
sudo diskutil eraseDisk exfat FLASH /dev/disk2  # exfat --> new file system on flash (as example) FLASH --> new volume name
```

### to write iso on flash

```bash
sudo diskutil umountDisk /dev/disk2  # unmaunt flash before writing

# example how to write Ddebian iso
sudo dd if=/Users/Maksim/Downloads/debian-live-10.2.0-amd64-xfce.iso of=/dev/rdisk2 bs=1m
```

### to write macОС on flash

you should format flash to _Mac OS Extended_ and name it as `MyVolume` -->
important!

write macos on flash

```bash
sudo /Applications/Install\ macOS\ Mojave.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume --applicationpath /Applications/Install\ macOS\ Mojave.app`
```

### how to find apps for deleting

```bash
find . -name "*vlc*" && sudo find /Library -name "*vlc*"
```

### homebrew

```bash
brew update    # First update the formulae and Homebrew itself
brew outdated  # You can now find out what is outdated with
brew upgrade   # Upgrade everything with
brew doctor    # inspect homebrew
```

---

THE END

