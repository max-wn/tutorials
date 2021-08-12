# arch installation tutor

## usb and iso preparation

### usb format

Find name of USB (for example it is `sda`)

```bash
sudo fdisk -l
```

Unmount USB

```bash
sudo umount /dev/sda
```

Format USB

```bash
sudo mkfs -t ext4 -L FLASH /dev/sda
```

Download latest Arch iso file and verify signature. All instructions awaliable
in [wiki](https://wiki.archlinux.org/index.php/Installation_guide "wiki
guide")

**for example on macOS:**
* download iso from [wiki](https://www.archlinux.org/download/ "wiki
   downloads")
* `cd` to Download directory
* get the checksum:

    ```bash
    md5 archlinux-2020.11.01-x86_64.iso
    ```

* the command return md5 checksum and you should compare it with md5 checksum
   mentioned on [site](https://www.archlinux.org/download/ "wiki downloads")
* Write Arch to USB.
* Start installation as mentioned below.

## arch installation

### Check our disks (for example it is 'nvme0n1')

```bash
lsblk
```

### Check UEFI or BIOS (this tutor for BIOS only)

```bash
ls /sys/firmware/efi/efivars
```

### Check internet connection

check ip and ping (if no ip and ping use wifi instructions):

```bash
ip addr show
ping -c 3 archlinux.org
```

Connect to wifi via `wpa_supplicant`

Find device name (for example it is `wlan0`)

```bash
iw dew
```

Set rfkill off

```bash
rfkill unblock wifi
```

Set device up

```bash
ip link set wlan0 up
```

Scan networks

```bash
iw dev wlan0 scan | grep SSID
```

Generate config file

```bash
wpa_passphrase <nameofnetwork> <passwordfornetwork> > wpa_nameofnetwork.conf
```

Connect to network

```bash
wpa_supplicant -B -i wlan0 -c wpa_nameofnetwork.conf
```

Check connection

```bash
ping -c 3 archlinux.org
```

### Set time

```bash
timedatectl set-ntp true
```

### Disk partition

```bash
fdisk /dev/nvme0n1
```

interface: `m` - help, `p` - disks, `d` - delete disk, `n` - new partition
hint `n` for new partition, always chose primary, start sector defoult, end
sector put the size as mentioned below:

1. primary (boot): `+512M`
2. primary (swap): `+16G`
3. primary (root): `+50G`
4. primary (home): hit `Enter` for all the rest space, hit `w` for write
  these partitions

### Set file system as follows:

for boot

```bash
mkfs.ext4 /dev/nvme0n1p1
```

for root

```bash
mkfs.ext4 /dev/nvme0n1p3
```

for home

```bash
mkfs.ext4 /dev/nvme0n1p4
```

for swap

```bash
mkswap /dev/nvme0n1p2
```

swap activation

```bash
swapon /dev/nvme0n1p2
```

### Mount root

```bash
mount /dev/nvme01p3 /mnt
```

### Create home directory

```bash
mkdir /mnt/home
```

### Create boot directory

```bash
mkdir /mnt/boot
```

### Mount boot

```bash
mount /dev/nvme01p1 /mnt/boot
```

### Mount home

```bash
mount /dev/nvme01p4 /mnt/home
```

### Correct mirror list (put Russia first)

```bash
sudo vim /etc/pacman.d/mirrorlist
```

### Install arch

```bash
pacstrap /mnt base base-devel linux linux-firmware linux-headers vim
inetutils netctl dhcpcd dialog iw iwd wpa_supplicant intel-ucode man-pages
man-db
```

### Generate fstab

```bash
genfstab /mnt
```

### Force fstab to use UUID

```bash
genfstab -U /mnt
```

### Write fstab to file

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

### Transfer from USB linux to Computer linux

```bash
arch-chroot /mnt
```

### Add hosts

```bash
vim /etc/hosts
```

put the followimg in file:

```bash
127.0.0.1   localhost
::1         localhost
127.0.1.1   <hostname>.localdomine <hostname>
```

### Set Network Manager

install NM

```bash
pacman -S networkmanager network-manager-applet
```

Enable Network Manager

```bash
systemctl enable NetworkManager
```

### NM tuning for wifi

disable dhcpcd on ethernet

```bash
sudo systemctl disable dhcpcd@enp8s0.service
```

disable netctl on wifi

```bash
sudo systemctl disable netctl-auto@wlan0.service
```

enable NM

```bash
sudo systemctl enable NetworkManager.service
```

reboot

```bash
reboot
```

Show connections (for example it is wlan0)

```bash
nmcli connection
```

Connect to a wifi network

```bash
nmcli device wifi connect wlan0 password <yourpassword>
```

More info see in [wiki](https://wiki.archlinux.org/index.php/NetworkManager "NM archwiki")

### Set grub

```bash
pacman -S grub
```

Enable grub

```bash
grub-install --target=i386-pc /dev/nvme0n1
```

Create config file for grub

```bash
grub-mkconfig -o /boot/grub/grun.cfg
```

### Set root password

```bash
passwd <yourpassword>
```

### Generate local

uncomment langusges in:

```bash
vim /etc/locale.gen
en_US.UTF-8 UTF-8
ru_RU.UTF-8 UTF-8
```

enable local

```bash
locale-gen
```

Set (wright in file) language in:

```bash
vim /etc/locale.conf
LANG=en_US.UTF-8
```

### Set time zone

```bash
ln -sf /usr/share/zoneinfo/Europe/Moscow /etc/localtime
```

### Generate time

```bash
hwclock --systohc
```

### Wright your hostname in file:

```bash
vim /etc/hostname
```

### Exit from root and unmount USB

```bash
exit
umount -R /mnt
```

### Reboot

```bash
reboot
```

### Check internet cinnection.

If not - execute clause "Check internet connection" above and then make NM tuning for wifi:

disable dhcpcd on ethernet

```bash
sudo systemctl disable dhcpcd@enp8s0.service
```

disable netctl on wifi

```bash
sudo systemctl disable netctl-auto@wlan0.service
```

enable NM

```bash
sudo systemctl enable NetworkManager.service
```

reboot

```bash
reboot
```

### Update & upgrade system

```bash
sudo pacman -Syu
```

### Users and groups.

Add new user (-m means home dir will be created)

```bash
useradd -m <nameofuser>
```

Add password for new user

```bash
passwd <nameofuser> <password>
```

Add user to groups

```bash
usermod -aG wheel,audio,video,optical,storage,<nameofuser>
```

Check user's groups

```bash
groups <nameofuser>
```

Open sudoers file (/etc/sudoers) and uncomment wheel group

```bash
visudo
```

### Install xorg and video drivers

```bash
sudo pacman -S xorg xorg-xinit xsorg-server xorg-drivers xorg-xmodmap
xterm xcb-util-cursor
```

show mod keys (work only if WM starts):

```bash
xmodmap -pm
```

Check your video card (here I have nvidia and I will use proprietary driver)

```bash
lspci -k | grep -E "(VGA|3D)"
```

Uninstal conflict drivers. IMPORTANT!

```bash
sudo pacman -Rs xf86-video-amdgpu
sudo pacman -Rs xf86-video-nouveau
sudo pacman -Rs xf86-video-intel
```

Ceck if you have mesa (if not, install it)

```bash
sudo pacman -Qs mesa
```

Install nvidia packages

```bash
sudo pacman -S nvidia nvidia-utils
```

Check xorg

```bash
sudo startx
```

Exit from xorg

```bash
exit
```

### Install WM (qtile and additional packages for it)

```bash
sudo pacman -S qtile alacritty dmenu python-psutil python-iwlib
python-keyring python-dateutil python-pyxdg python-mpd2
```

Copy `.xinitrc` to `home` directory, see instructions in
[wiki](https://wiki.archlinux.org/index.php/Xinit "xinit archwiki")

Put the following line in .xinitrc

```bash
exec qtile
```

Start qtile from tty by command:

```bash
xinit
```

### Install additional important packages

```bash
sudo pacman -S git openssh gnupg pass rsync veracrypt
```

packages description:

* git        --> distributed version control system
* openssh    --> tool for remote login with the SSH protocol
* gnupg      --> complete and free implementation of the OpenPGP standard
* pass       --> password manager
* rsync      --> file copying tool for remote and local files
* veracrypt  --> disk encryption

### Install fonts

```bash
sudo pacman -S ttf-inconsolata ttf-jetbrains-mono
```

plus `ttf-linux-libertine` or: `noto-fonts`

### Install audio packages

```bash
sudo pacman -S pulseaudio pulseaudio-alsa alsa-lib alsa-utils
```

Set up sound, instructions see in
[wiki](https://wiki.archlinux.org/index.php/Advanced_Linux_Sound_Architecture "alsamixer archwiki")

### Install workflow packages

```bash
sudo pacman -S figlet mutt firefox udisks2 pycharm-community-edition moc
htop mc calcurse sxiv zathura zathura-pdf-mupdf zathura-djvu
pacman-contrib github-cli newsboat perl-image-exiftool calibre mpv krita
gimp tldr pandoc texlive-most texlive-lang biber
```

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

```bash
export EDITOR="vim"
export BROWSER="firefox"
```

---

Create RU keyboard layout:

For using `Shift + Ctrl` for US-RU change:

```bash
localectl --no-convert set-x11-keymap us,ru "" "" grp:ctrl_shift_toggle
```

Restart xinit

---

STEAM installation.

All instructions see in [wiki](https://wiki.archlinux.org/index.php/Steam "steam archwiki")

to enable multilib repository, uncomment the `[multilib]` section in
`/etc/pacman.conf` as follows:

```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

install proprietary 32-bit version OpenGL graphics driver

```bash
sudo pacman -S lib32-nvidia-utils
```

Generated `en_US.UTF-8` locale, preventing invalid pointer error

The GUI heavily uses the Arial font. See Microsoft fonts. An alternative is to
use `ttf-liberation`

```bash
sudo pacman -S ttf-liberation
```

install `wqy-zenhei` to add support for Asian languages

```bash
sudo pacman -S wqy-zenhei
```

install steam

```bash
sudo pacman -S steam
```

---

Turn off PC:

```bash
sudo shutdown -h now
```

---

Emergency action:

`Ctrl + Alt + F2` or `F3` or `F4` etc --> Open new tty session

---

MOUNT USB

Finde a name of USB device (for example it is sda)

```bash
sudo fdisk -l
```

Create directory for USB

```bash
mkdir /mnt/usbstick
```

Mount USB

```bash
sudo mount /dev/sda /mnt/usbstick
```

Give a group to usbstick directory

```bash
sudo chown root.wheel /mnt/usbstick
```

Give a right to usbstick directory

```bash
sudo chmod g+w /mnt/usbstick
```

See what is mounted

```bash
blkid -o list -c /dev/null
```

Unmount USB

```bash
sudo umount /mnt/usbstick
```

---

PACMAN

Uncomment Color and VerbosePkgLists lines in file:

```bash
vim /etc/pacman.conf
```

Update all packages

```bash
sudo pacman -Syu
```

This will refresh your databases even if they are already up to date. I have found
this to rarely be necessary, even when updating my mirrorlist or adding custom
repos. Could be used when your database was corrupted, and that happens
exactly very rarely in Arch.

```bash
sudo pacman -Syyu
```

Install packages

```bash
sudo pacman -S <packagename>
```

Remove packages

```bash
sudo pacman -Rs <packagename>
```

Finde packages in repository

```bash
sudo pacman -Ss <packagename>
```

Finde packages on PC

```bash
sudo pacman -Qs <packagename>
```

Finde not needed package/dependancy which could be safly remove

```bash
sudo pacman -Qdt
```

Remove not needed package/dependancy which could be safly remove

```bash
sudo pacman -Rns $(pacman -Qtdq)
```

---

УСТАНОВКА РУССКОГО ЯЗЫКА (RALT --> ENG, Shift + RALT --> RUS)

Check keyboard settigs on the your computer:

```bash
setxkbmap -layout us,ru -print
```

create file config:

```bash
touch ~/.config/xkb/config
```

Write the result in the file:

```bash
setxkbmap -layout us,ru -print > ~/.config/xkb/config
```

chenge string `xkb_symbols...` as folows:

```
xkb_symbols "my" {
include "pc+us+ru:2+inet(evdev)"
key <RALT> { [ ISO_First_Group, ISO_Last_Group ] };
};
```

Load config:

```bash
xkbcomp $HOME/.config/xkb/config $DISPLAY
```

source finde [here](https://m.habr.com/ru/post/486872/ "instructions")

---

THE END
