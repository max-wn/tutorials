# Cheatsheet for some commands in Debian

---

- создать резервную копию существующего файла sources.list: in debian
`sudo cp /etc/apt/sources.list /etc/apt/sources.list_backup`

- открыть файл sources.list in debian
`sudo vim /etc/apt/sources.list`

- добавить строку в sources.list с репозиторием backports in debian
`deb http://deb.debian.org/debian buster-backports main contrib non-free`

- install deb files, example
`sudo dpkg -i ~/Downloads/slack-desktop-4.3.2-amd64.deb`

## fixes in debian
`sudo apt --fix-broken install`

## swappiness

### show swappiness
`cat /proc/sys/vm/swappiness`

### file with swappiness
`sudo nano /etc/sysctl.conf`

### set swappiness parametr in file
`vm.swappiness = 10`

## just for STEAM
You might need to specify the right command for games inside Steam.

1. Select a game - that you want to run using your discrete Nvidia card - from
the Library page of the Steam client, right-mouse-button-click, and select
Properties.
2. Click the _set launch options_ button and specify `optirun %command%` for
   the command line.
3. Save your changes.
4. If this doesn't work, try `primusrun` instead of `optirun`.

## PYTHON
`sudo apt install python3-pip` install _pip3_

## JETBRAINS
1. `sudo tar -xzf jetbrains-toolbox-1.16.6319.tar.gz -C /opt` unpack file
   `jetbrains-toolbox-1.16.6319.tar.gz`
2. `./jetbrains-toolbox` run the file `jetbrains-toolbox`

## checkio
1. `sudo pip3 install checkio_client`
2. `checkio config`

## terminal

### check for utf-8
`curl https://www.cl.cam.ac.uk/~mgk25/ucs/examples/UTF-8-demo.txt`

### check for emoji
`cat emoji_examples`

---

THE END
