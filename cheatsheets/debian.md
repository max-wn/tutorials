## debian cheatsheet

### basis

создать резервную копию существующего файла _sources.list_

```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list_backup
```

открыть файл _sources.list_

```bash
sudo vim /etc/apt/sources.list
```

добавить строку в _sources.list_ с репозиторием _backports_

```bash
deb http://deb.debian.org/debian buster-backports main contrib non-free
```

install deb files (example)

```bash
sudo dpkg -i ~/Downloads/slack-desktop-4.3.2-amd64.deb
```

fixes in debian

```bash
sudo apt --fix-broken install
```

show swappiness

```bash
cat /proc/sys/vm/swappiness
```

file with swappiness

```bash
sudo nano /etc/sysctl.con
```

set swappiness parametr in file

`vm.swappiness = 10`

### just for steam

You might need to specify the right command for games inside Steam.

1. Select a game - that you want to run using your discrete Nvidia card - from
the Library page of the Steam client, right-mouse-button-click, and select
Properties.
2. Click the _set launch options_ button and specify `optirun %command%` for
   the command line.
3. Save your changes.
4. If this doesn't work, try `primusrun` instead of `optirun`.

### python

```bash
sudo apt install python3-pip  # install pip3
```

### jetbrains

```bash
sudo tar -xzf jetbrains-toolbox-1.16.6319.tar.gz -C /opt  # unpack file "jetbrains-toolbox-1.16.6319.tar.gz"
./jetbrains-toolbox                                       # run the file "jetbrains-toolbox"
```

### checkio

```bash
sudo pip3 install checkio_client  # install checkio
checkio config                    # config checkio
```

### terminal

check for utf-8

```bash
curl https://www.cl.cam.ac.uk/~mgk25/ucs/examples/UTF-8-demo.txt
```

---

THE END
