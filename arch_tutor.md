# arch installation tutor

## usb and iso preparation

1. USB format

Find name of USB (for example it is `sda`)

    sudo fdisk -l

Unmount USB

    sudo umount /dev/sda

Format USB

    sudo mkfs -t ext4 -L FLASH /dev/sda

2. Download latest Arch iso file  and verify signature. All instructions
   awaliable in [wiki](https://wiki.archlinux.org/index.php/Installation_guide
   "wiki guide")

**for example on macOS:**
* download iso from [wiki](https://www.archlinux.org/download/ "wiki
   downloads")
* `cd` to Download directory
* get the checksum:

    md5 archlinux-2020.11.01-x86_64.iso

* the command return md5 checksum and you should compare it with md5 checksum
   mentioned on [site](https://www.archlinux.org/download/ "wiki downloads")
* Write Arch to USB.
* Start installation as mentioned below.

## arch installation

### Check our disks (for example it is 'nvme0n1')

    lsblk

### Check UEFI or BIOS (this tutor for BIOS only)

    ls /sys/firmware/efi/efivars

### Check internet connection

check ip and ping (if no ip and ping use wifi instructions):

    ip addr show
    ping -c 3 archlinux.org

Connect to wifi via `wpa_supplicant`

Find device name (for example it is `wlan0`)

    iw dew

Set rfkill off

    rfkill unblock wifi

Set device up

    ip link set wlan0 up

Scan networks

    iw dev wlan0 scan | grep SSID

Generate config file

    wpa_passphrase <nameofnetwork> <passwordfornetwork> > wpa_nameofnetwork.conf

Connect to network

    wpa_supplicant -B -i wlan0 -c wpa_nameofnetwork.conf

Check connection

    ping -c 3 archlinux.org

### Set time

    timedatectl set-ntp true

### Disk partition

    fdisk /dev/nvme0n1

interface: `m` - help, `p` - disks, `d` - delete disk, `n` - new partition
hint `n` for new partition, always chose primary, start sector defoult, end
sector put the size as mentioned below:

1. primary (boot): +512M
2. primary (swap): +16G
3. primary (root): +50G
4. primary (home): hit `Enter` for all the rest space, hit `w` for write
  these partitions

### Set file system as follows:

for boot

    mkfs.ext4 /dev/nvme0n1p1

for root

    mkfs.ext4 /dev/nvme0n1p3

for home

    mkfs.ext4 /dev/nvme0n1p4

for swap

    mkswap /dev/nvme0n1p2

swap activation

    swapon /dev/nvme0n1p2

### Mount root

    mount /dev/nvme01p3 /mnt

### Create home directory

    mkdir /mnt/home

### Create boot directory

    mkdir /mnt/boot

### Mount boot

    mount /dev/nvme01p1 /mnt/boot

### Mount home

    mount /dev/nvme01p4 /mnt/home

### Correct mirror list (put Russia first)

    sudo vim /etc/pacman.d/mirrorlist

### Install arch

    pacstrap /mnt base base-devel linux linux-firmware linux-headers vim
    inetutils netctl dhcpcd dialog iw iwd wpa_supplicant intel-ucode man-pages
    man-db

### Generate fstab

    genfstab /mnt

### Force fstab to use UUID

    genfstab -U /mnt

### Write fstab to file

    genfstab -U /mnt >> /mnt/etc/fstab

### Transfer from USB linux to Computer linux

    arch-chroot /mnt

### Add hosts

    vim /etc/hosts

put the followimg in file:

    127.0.0.1   localhost
    ::1         localhost
    127.0.1.1   <hostname>.localdomine <hostname>

### Set Network Manager

install NM

    pacman -S networkmanager network-manager-applet

Enable Network Manager

    systemctl enable NetworkManager

### NM tuning for wifi

disable dhcpcd on ethernet

    sudo systemctl disable dhcpcd@enp8s0.service

disable netctl on wifi

    sudo systemctl disable netctl-auto@wlan0.service

enable NM

    sudo systemctl enable NetworkManager.service

reboot

    reboot

Show connections (for example it is wlan0)

    nmcli connection

Connect to a wifi network

    nmcli device wifi connect wlan0 password <yourpassword>

More info see in [wiki](https://wiki.archlinux.org/index.php/NetworkManager "NM archwiki")

### Set grub

    pacman -S grub

Enable grub

    grub-install --target=i386-pc /dev/nvme0n1

Create config file for grub

    grub-mkconfig -o /boot/grub/grun.cfg

### Set root password

    passwd <yourpassword>

### Generate local

uncomment langusges in:

    vim /etc/locale.gen
    en_US.UTF-8 UTF-8
    ru_RU.UTF-8 UTF-8

enable local

    locale-gen

Set (wright in file) language in:

    vim /etc/locale.conf
    LANG=en_US.UTF-8

### Set time zone

    ln -sf /usr/share/zoneinfo/Europe/Moscow /etc/localtime

### Generate time

    hwclock --systohc

### Wright your hostname in file:

    vim /etc/hostname

### Exit from root and unmount USB

    exit
    umount -R /mnt

### Reboot

    reboot

### Check internet cinnection.

If not - execute clause "Check internet connection" above and then make NM tuning for wifi:

1. disable dhcpcd on ethernet

    sudo systemctl disable dhcpcd@enp8s0.service

2. disable netctl on wifi

    sudo systemctl disable netctl-auto@wlan0.service

3. enable NM

    sudo systemctl enable NetworkManager.service

4. reboot

    reboot

### Update & upgrade system

    sudo pacman -Syu

### Users and groups.

Add new user (-m means home dir will be created)

    useradd -m <nameofuser>

Add password for new user

    passwd <nameofuser> <password>

Add user to groups

    usermod -aG wheel,audio,video,optical,storage,<nameofuser>

Check user's groups

    groups <nameofuser>

Open sudoers file (/etc/sudoers) and uncomment wheel group

    visudo

### Install xorg and video drivers

    sudo pacman -S xorg xorg-xinit xsorg-server xorg-drivers xorg-xmodmap
    xterm xcb-util-cursor

show mod keys (work only if WM starts):

    xmodmap -pm

Check your video card (here I have nvidia and I will use

    proprietary driver)
    lspci -k | grep -E "(VGA|3D)"

Uninstal conflict drivers. IMPORTANT!

    sudo pacman -Rs xf86-video-amdgpu
    sudo pacman -Rs xf86-video-nouveau
    sudo pacman -Rs xf86-video-intel

Ceck if you have mesa (if not, install it)

    sudo pacman -Qs mesa

Install nvidia packages

    sudo pacman -S nvidia nvidia-utils

Check xorg

    sudo startx

Exit from xorg

    exit

### Install WM (qtile and additional packages for it)

    sudo pacman -S qtile alacritty dmenu python-psutil python-iwlib
    python-keyring python-dateutil python-pyxdg python-mpd2

Copy `.xinitrc` to `home` directory, see instructions in
[wiki](https://wiki.archlinux.org/index.php/Xinit "xinit archwiki")

Put the following line in .xinitrc
    exec qtile

Start qtile from tty by command:
    xinit

### Install additional important packages

    sudo pacman -S git openssh gnupg pass rsync veracrypt

packages description:

* git        --> distributed version control system
* openssh    --> tool for remote login with the SSH protocol
* gnupg      --> complete and free implementation of the OpenPGP standard
* pass       --> password manager
* rsync      --> file copying tool for remote and local files
* veracrypt  --> disk encryption

### Install fonts

`sudo pacman -S ttf-inconsolata ttf-jetbrains-mono` plus `ttf-linux-libertine`
or: `noto-fonts`

### Install audio packages

    sudo pacman -S pulseaudio pulseaudio-alsa alsa-lib alsa-utils

Set up sound, instructions see in
[wiki](https://wiki.archlinux.org/index.php/Advanced_Linux_Sound_Architecture "alsamixer archwiki")

### Install workflow packages

    sudo pacman -S figlet mutt firefox udisks2 pycharm-community-edition moc
    htop mc calcurse sxiv zathura zathura-pdf-mupdf zathura-djvu
    pacman-contrib github-cli newsboat perl-image-exiftool calibre mpv krita
    gimp tldr pandoc texlive-most texlive-lang biber

packages description:

* figlet                    --> display large letters out of text
* mutt                      --> email client
* firefox                   --> internet browser
* udisks2                   --> to query and manipulate storage devices
* pycharm-community-edition --> python ide
* moc                       --> audio player
* htop                      --> interactive process viewer
* mc                        --> file manager
* calcurse                  --> todo manager
* sxiv                      --> lightweight and scriptable image viewer
* zathura                   --> document viewer PDF, PS, DjVu
* zathura-pdf-mupdf         --> document viewer PDF, PS, DjVu
* zathura-djvu              --> document viewer PDF, PS, DjVu
* pacman-contrib            --> contributed scripts and tools for pacman systems
* github-cli                --> cli client gor github
* newsboat                  --> rss feed
* perl-image-exiftool       --> Reader and rewriter of EXIF informations that supports raw files
* calibre                   --> e-book management application
* mpv                       --> video player
* youtube-dl                --> A command-line program to download videos from YouTube.com and a few more sites
* ffmoeg                    --> Complete solution to record, convert and stream audio and video
* krita                     --> edit and paint images
* gimp                      --> GNU Image Manipulation Program
* tldr                      --> Command line client for tldr, a collection of simplified and community-driven man pages
* pandoc                    --> Conversion between markup formats
* texlive-most              --> group contains most TeX Live packages
* texlive-lang              --> group contains packages providing character sets and features for languages with non-latin characters
* biber                     --> A Unicode-capable BibTeX replacement for biblatex users
* lynx                      --> A text browser for the World Wide Web
* stow                      --> Manage installation of multiple softwares in the same directory tree

### Set defolt programms

put in file `vim ~/.bash_profile`

    export EDITOR="vim"
    export BROWSER="firefox"

### Create RU keyboard layout:

For using `Shift + Ctrl` for US-RU change:

    localectl --no-convert set-x11-keymap us,ru "" "" grp:ctrl_shift_toggle

Restart xinit

---

STEAM installation.

All instructions see in [wiki](https://wiki.archlinux.org/index.php/Steam "steam archwiki")

1. to enable multilib repository, uncomment the `[multilib]` section in
   `/etc/pacman.conf` as follows:

    [multilib]
    Include = /etc/pacman.d/mirrorlist

2. install proprietary 32-bit version OpenGL graphics driver

    sudo pacman -S lib32-nvidia-utils

3. Generated `en_US.UTF-8` locale, preventing invalid pointer error

4. The GUI heavily uses the Arial font. See Microsoft fonts. An alternative

    is to use ttf-liberation
    sudo pacman -S ttf-liberation

5. install `wqy-zenhei` to add support for Asian languages

    sudo pacman -S wqy-zenhei

6. install steam

    sudo pacman -S steam

---

Turn off PC:

    sudo shutdown -h now

---

Emergency action:

Open new tty session

    Ctrl + Alt + F2 (or F3 or F4 etc)

---

MOUNT USB

0. Finde a name of USB device (for example it is sda)

    sudo fdisk -l

1. Create directory for USB

    mkdir /mnt/usbstick

2. Mount USB

    sudo mount /dev/sda /mnt/usbstick

3. Give a group to usbstick directory

    sudo chown root.wheel /mnt/usbstick

4. Give a right to usbstick directory

    sudo chmod g+w /mnt/usbstick

5. See what is mounted

    blkid -o list -c /dev/null

6. Unmount USB

    sudo umount /mnt/usbstick

---

PACMAN

1 - Uncomment Color and VerbosePkgLists lines in file:

    vim /etc/pacman.conf

2 - Update all packages

    sudo pacman -Syu

    sudo pacman -Syyu

will refresh your databases even if they are already up to date. I have found
this to rarely be necessary, even when updating my mirrorlist or adding custom
repos. Could be used when your database was corrupted, and that happens
exactly very rarely in Arch.

3 - Install packages

    sudo pacman -S <packagename>

4 - Remove packages

    sudo pacman -Rs <packagename>

5 - Finde packages in repository

    sudo pacman -Ss <packagename>

6 - Finde packages on PC

    sudo pacman -Qs <packagename>

7 - Finde not needed package/dependancy which could be safly remove

    sudo pacman -Qdt

8 - Remove not needed package/dependancy which could be safly remove

    sudo pacman -Rns $(pacman -Qtdq)

---

УСТАНОВКА РУССКОГО ЯЗЫКА (RALT --> ENG, Shift + RALT --> RUS)

1 - Check keyboard settigs on the your computer:

    setxkbmap -layout us,ru -print

2 - create file config:

    touch ~/.config/xkb/config

3 - Write the result in the file:

    setxkbmap -layout us,ru -print > ~/.config/xkb/config

4 - chenge string `xkb_symbols...` as folows:

    xkb_symbols "my" {
    include "pc+us+ru:2+inet(evdev)"
    key <RALT> { [ ISO_First_Group, ISO_Last_Group ] };
    };

5 - Load config:

    xkbcomp $HOME/.config/xkb/config $DISPLAY

6 - source finde here:

    https://m.habr.com/ru/post/486872/

---

THE END
