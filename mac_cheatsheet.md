# MAC TRICKS AND COMMANDS

`diskutil list`  # return names of disks (now flash is `disk2`)

## format flash
`sudo diskutil eraseDisk exfat FLASH /dev/disk2`  # exfat --> new file system on flash (as example) FLASH --> new volume name

## to write iso on flash
1. `sudo diskutil umountDisk /dev/disk2`  # unmaunt flash before writing
2. `sudo dd if=/Users/Maksim/Downloads/debian-live-10.2.0-amd64-xfce.iso of=/dev/rdisk2 bs=1m`

## to write macОС on flash
1. you should format flash to _Mac OS Extended_
2. and name it as `MyVolume` --> important!
3. `sudo /Applications/Install\ macOS\ Mojave.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume --applicationpath /Applications/Install\ macOS\ Mojave.app`

## how to find apps for deleting
`find . -name "*vlc*" && sudo find /Library -name "*vlc*"`

## homebrew
1. `brew update`    # First update the formulae and Homebrew itself
2. `brew outdated`  # You can now find out what is outdated with
3. `brew upgrade`   # Upgrade everything with

---

THE END

