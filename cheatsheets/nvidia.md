## nvidia cheatsheet

### how to use Use NVIDIA graphics only in optimus video card

[source1](https://wiki.archlinux.org/title/NVIDIA_Optimus#Checking_3D)
[source2](https://zalinux.ru/?p=3461&PageSpeed=noscript)

First, you should have installed the NVIDIA drivers `nvidia`, `nvidia-utils`
and `xorg-xrandr` and `mesa-demos`.

Then, configure file `/etc/X11/xorg.conf.d/10-nvidia-drm-outputclass.conf`
(template provided in
`/usr/share/X11/xorg.conf.d/10-nvidia-drm-outputclass.conf`) as follows:

```
Section "OutputClass"
    Identifier "intel"
    MatchDriver "i915"
    Driver "modesetting"
EndSection

Section "OutputClass"
    Identifier "nvidia"
    MatchDriver "nvidia-drm"
    Driver "nvidia"
    Option "AllowEmptyInitialConfiguration"
    Option "PrimaryGPU" "yes"
    ModulePath "/usr/lib/nvidia/xorg"
    ModulePath "/usr/lib/xorg/modules"
EndSection
```

Next, add the following two lines to the beginning of your `~/.xinitrc`:

```
xrandr --setprovideroutputsource modesetting NVIDIA-0
xrandr --auto
```

Now reboot to load the drivers, and X should start.

You can check if the NVIDIA graphics are being used by tools from `mesa-demos`
and running:

```bash
# these commands should display info about NVIDIA
glxinfo | grep NVIDIA
glxinfo|egrep "OpenGL vendor|OpenGL renderer"
glxheads

# this commant should display very big FPS (more than 1k)
glxgears

# this command should show glxheads or glxgears proceses
nvidia-smi
```

---

THE END
