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
1. download iso from [wiki](https://www.archlinux.org/download/ "wiki
   downloads")
2. `cd` to Download directory
3. get the checksum:

    md5 archlinux-2020.11.01-x86_64.iso

4. the command return md5 checksum and you should compare it with md5 checksum
   mentioned on [site](https://www.archlinux.org/download/ "wiki downloads")
3. Write Arch to USB.
4. Start installation as mentioned below.

## arch installation

1. Check our disks (for example it is 'nvme0n1')

    lsblk

2. Check UEFI or BIOS (this tutor for BIOS only)

    ls /sys/firmware/efi/efivars

3. Check internet connection
* check ip and ping (if no ip and ping use wifi instructions):

    ip addr show
    ping -c 3 archlinux.org

* Connect to wifi via `wpa_supplicant`

* Find device name (for example it is `wlan0`)

    iw dew

* Set rfkill off

    rfkill unblock wifi

* Set device up

    ip link set wlan0 up

* Scan networks

    iw dev wlan0 scan | grep SSID

* Generate config file

    wpa_passphrase <nameofnetwork> <passwordfornetwork> > wpa_nameofnetwork.conf

* Connect to network

    wpa_supplicant -B -i wlan0 -c wpa_nameofnetwork.conf

* Check connection

    ping -c 3 archlinux.org

4. Set time
    timedatectl set-ntp true

5. Disk partition

    fdisk /dev/nvme0n1

interface: `m` - help, `p` - disks, `d` - delete disk, `n` - new partition
hint `n` for new partition, always chose primary, start sector defoult, end
sector put the size as mentioned below:

* 1 - primary (boot): +512M
* 2 - primary (swap): +16G
* 3 - primary (root): +50G
* 4 - primary (home): hit `Enter` for all the rest space, hit `w` for write
  these partitions

6. Set file system as follows:

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

7. Mount root

    mount /dev/nvme01p3 /mnt

8. Create home directory

    mkdir /mnt/home

9. Create boot directory

    mkdir /mnt/boot

10. Mount boot

    mount /dev/nvme01p1 /mnt/boot

11. Mount home

    mount /dev/nvme01p4 /mnt/home

* Correct mirror list (put Russia first)

    sudo vim /etc/pacman.d/mirrorlist

12. Install arch

    pacstrap /mnt base base-devel linux linux-firmware linux-headers vim
    inetutils netctl dhcpcd dialog iw iwd wpa_supplicant intel-ucode man-pages
    man-db

13. Generate fstab

    genfstab /mnt

14. Force fstab to use UUID

    genfstab -U /mnt

15. Write fstab to file

    genfstab -U /mnt >> /mnt/etc/fstab

16. Transfer from USB linux to Computer linux

    arch-chroot /mnt

17. Add hosts

    vim /etc/hosts

put the followimg in file:

    127.0.0.1   localhost
    ::1         localhost
    127.0.1.1   <hostname>.localdomine <hostname>

18. Set Network Manager

    pacman -S networkmanager network-manager-applet

19. Enable Network Manager

    systemctl enable NetworkManager

===

19.1. NM tuning for wifi
    1. disable dhcpcd on ethernet
        sudo systemctl disable dhcpcd@enp8s0.service
    2. disable netctl on wifi
        sudo systemctl disable netctl-auto@wlan0.service
    2. enable NM
        sudo systemctl enable NetworkManager.service
    3. reboot
        reboot
    4.Show connections (for example it is wlan0)
        nmcli connection
    5. Connect to a wifi network
        nmcli device wifi connect wlan0 password <yourpassword>
    6. More info see in wiki
        https://wiki.archlinux.org/index.php/NetworkManager

20. Set grub
    pacman -S grub

21. Enable grub
    grub-install --target=i386-pc /dev/nvme0n1

22. Create config file for grub
    grub-mkconfig -o /boot/grub/grun.cfg

23. Set root password
    passwd <yourpassword>

24. Generate local
    24.1. uncomment langusges in:
        vim /etc/locale.gen
        en_US.UTF-8 UTF-8
        ru_RU.UTF-8 UTF-8
    24.2. enable local
        locale-gen
    24.2. Set (wright in file) language in:
        vim /etc/locale.conf
        LANG=en_US.UTF-8

25. Set time zone
   ln -sf /usr/share/zoneinfo/Europe/Moscow /etc/localtime
    25.1. Generate time
        hwclock --systohc

26. Wright your hostname in file:
    vim /etc/hostname

27. Exit from root and unmount USB
    exit
    umount -R /mnt

28. Reboot
    reboot


29.Check internet cinnection. If not - execute clause #3 above and then make
    NM turning.
    NM tuning for wifi:
        1. disable dhcpcd on ethernet
            sudo systemctl disable dhcpcd@enp8s0.service
        2. disable netctl on wifi
            sudo systemctl disable netctl-auto@wlan0.service
        2. enable NM
            sudo systemctl enable NetworkManager.service
        3. reboot
            reboot

30. Update & upgrade system
    sudo pacman -Syu

31. Users and groups.
    31.1. Add new user (-m means home dir will be created)
        useradd -m <nameofuser>
    31.2. Add password for new user
        passwd <nameofuser> <password>
    31.3. Add user to groups
        usermod -aG wheel,audio,video,optical,storage,<nameofuser>
    31.4 Check user's groups
        groups <nameofuser>
    31.5 Open sudoers file (/etc/sudoers) and uncomment wheel group
        visudo

32. Install xorg and video drivers
    sudo pacman -S xorg xorg-xinit xsorg-server xorg-drivers xorg-xmodmap
    xterm xcb-util-cursor
    32.1. show mod keys (work only if WM starts):
        xmodmap -pm
    32.2. Check your video card (here I have nvidia and I will use
    proprietary driver)
        lspci -k | grep -E "(VGA|3D)"
    32.3. Uninstal conflict drivers. IMPORTANT!
        sudo pacman -Rs xf86-video-amdgpu
        sudo pacman -Rs xf86-video-nouveau
        sudo pacman -Rs xf86-video-intel
    32.4. Ceck if you have mesa (if not, install it)
        sudo pacman -Qs mesa
    32.5. Install nvidia packages
        sudo pacman -S nvidia nvidia-utils
    32.6. Check xorg
        sudo startx
    32.7. Exit from xorg
        exit

33. Install WM (qtile and additional packages for it)
sudo pacman -S qtile alacritty dmenu python-psutil python-iwlib
python-keyring python-dateutil python-pyxdg python-mpd2
    33.1. Copy .xinitrc to home directory (see instructions in wiki:
        https://wiki.archlinux.org/index.php/Xinit)
    33.2. Put the following line in .xinitrc
        exec qtile
    33.3. Start qtile from tty by command:
        xinit

34. Install additional important packages
    sudo pacman -S git openssh gnupg pass rsync veracrypt

    packages description:
    git                       --> distributed version control system
    openssh                   --> tool for remote login with the SSH protocol
    gnupg                     --> complete and free implementation of the OpenPGP standard
    pass                      --> password manager
    rsync                     --> file copying tool for remote and local files
    veracrypt                 --> disk encryption

35. Install fonts
sudo pacman -S ttf-inconsolata ttf-jetbrains-mono
    plus: ttf-linux-libertine
    or: noto-fonts

36. Install audio packages
sudo pacman -S pulseaudio pulseaudio-alsa alsa-lib alsa-utils
XX.X Set up sound (instructions see
    at: https://wiki.archlinux.org/index.php/Advanced_Linux_Sound_Architecture)
        alsamixer

37. Install workflow packages
    sudo pacman -S figlet mutt firefox udisks2 pycharm-community-edition moc
    htop mc calcurse sxiv zathura zathura-pdf-mupdf zathura-djvu
    pacman-contrib github-cli newsboat perl-image-exiftool calibre mpv krita
    gimp tldr pandoc texlive-most texlive-lang biber

    packages description:
    figlet                    --> display large letters out of text
    mutt                      --> email client
    firefox                   --> internet browser
    udisks2                   --> to query and manipulate storage devices
    pycharm-community-edition --> python ide
    moc                       --> audio player
    htop                      --> interactive process viewer
    mc                        --> file manager
    calcurse                  --> todo manager
    sxiv                      --> lightweight and scriptable image viewer
    zathura                   --> document viewer PDF, PS, DjVu
    zathura-pdf-mupdf         --> document viewer PDF, PS, DjVu
    zathura-djvu              --> document viewer PDF, PS, DjVu
    pacman-contrib            --> contributed scripts and tools for pacman systems
    github-cli                --> cli client gor github
    newsboat                  --> rss feed
    perl-image-exiftool       --> Reader and rewriter of EXIF informations that supports raw files
    calibre                   --> e-book management application
    mpv                       --> video player
    youtube-dl                --> A command-line program to download videos from YouTube.com and a few more sites
    ffmoeg                    --> Complete solution to record, convert and stream audio and video
    krita                     --> edit and paint images
    gimp                      --> GNU Image Manipulation Program
    tldr                      --> Command line client for tldr, a collection of simplified and community-driven man pages
    pandoc                    --> Conversion between markup formats
    texlive-most              --> group contains most TeX Live packages
    texlive-lang              --> group contains packages providing character sets and features for languages with non-latin characters
    biber                     --> A Unicode-capable BibTeX replacement for biblatex users
    lynx                      --> A text browser for the World Wide Web
    stow                      --> Manage installation of multiple softwares in the same directory tree

38. Set defolt programms, put in file:
vim ~/.bash_profile
    export EDITOR="vim"
    export BROWSER="firefox"

39. Create RU keyboard layout:
    39.1. For using Shift + Ctrl for US-RU change:
        localectl --no-convert set-x11-keymap us,ru "" "" grp:ctrl_shift_toggle
    39.2. Restart xinit
    39.3 ...

40. STEAM installation.
All instructions see in: https://wiki.archlinux.org/index.php/Steam
    40.1 to enable multilib repository, uncomment the [multilib] section in
        /etc/pacman.conf as follows:
            [multilib]
            Include = /etc/pacman.d/mirrorlist
    40.2 install proprietary 32-bit version OpenGL graphics driver
        sudo pacman -S lib32-nvidia-utils
    40.3 Generated en_US.UTF-8 locale, preventing invalid pointer error
    40.4 The GUI heavily uses the Arial font. See Microsoft fonts. An alternative
        is to use ttf-liberation
            sudo pacman -S ttf-liberation
    40.5 install wqy-zenhei to add support for Asian languages
        sudo pacman -S wqy-zenhei
    40.6 install steam
        sudo pacman -S steam

Turn off PC:
    sudo shutdown -h now

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
        will refresh your databases even if they are already up to date.
        I have found this to rarely be necessary,
        even when updating my mirrorlist or adding custom repos.
        Could be used when your database was corrupted,
        and that happens exactly very rarely in Arch.

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
4 - chenge string --> xkb_symbols... as folows:

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
