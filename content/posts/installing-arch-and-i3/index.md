---
title: "Making Arch Linux & i3 WM usable"
date: 2020-08-02T21:02:18+02:00
description: "This guide helps with making Arch Linux and i3 window manager usable."
categories: ["Linux"]
tags: ["Arch Linux", "i3"]
aliases: ["/blog/linux/installing-arch-linux-and-i3-wm/"]
---

This guide helps with making Arch Linux and i3 window manager usable. In this guide I use a script to install Arch Linux. If you are installing Arch for the first time I recommend to do it manually, this will give you a better understanding of how to install an OS.

Some of the programs included in this guide
* [i3-gaps](https://github.com/Airblader/i3) - The window manager
* [polybar](https://github.com/jaagr/polybar) - The status bar
* [polybar-spotify](https://github.com/Jvanrhijn/polybar-spotify) - Polybar module for showing current playing spotify song.
* [betterlockscreen](https://github.com/pavanjadhaw/betterlockscreen) - The lockscreen used in this config
* [rofi](https://github.com/davatorium/rofi) - Program Launcher 
* [rEFInd](https://www.rodsbooks.com/refind/) + [regular theme](https://github.com/bobafetthotmail/refind-theme-regular) - Boot manager

<br />

### Dotfiles

---

Before I start, I would like to point out that all my configuration files, so called dotfiles, for my Arch Linux installation can be found on [Github](https://github.com/niekvleeuwen/dotfiles). After this configuration is applied your desktop environment will look like this
{{< figure src="/img/posts/installing-arch-and-i3/screenshot.jpg"   caption="Screenshot of my setup" >}}

### Getting started

---

Get the latest version from the [Arch website](https://www.archlinux.org/download/). Burn the ISO to a USB drive with for example [Rufus](https://rufus.ie/) and boot into the live environment.

### Connect to the internet

---

1. Turn the networking interface on `ip link set [interface] up`
2. Save the wifi configuration `wpa_passphrase [ssid] [password] > /etc/wpa_supplicant/my_essid.conf`
3. Check if the connection is working `wpa_supplicant -c /etc/wpa_supplicant/my_essid.conf -i [wireless device]`
4. Execute the program in the background
`wpa_supplicant -B -c /etc/wpa_supplicant/my_essid.conf -i [wireless device]`
5. Retrieve an IP adress `dhclient [wireless device]`

### Using archfi to install the base

---

[Archfi](https://github.com/MatMoul/archfi) is a simple bash script wizard to install Arch Linux after you have booted on the official Arch Linux install media.

Download the script

```bash
$ wget archfi.sf.net/archfi
$ wget matmoul.github.io/archfi
```

Launch the script

```bash
$ sh archfi
```

### Install i3

---

Install display server and the i3 window manager

```bash
$ sudo pacman -S i3-gaps xorg-server xorg-xinit
$ nano /etc/X11/xinit/xinitrc # or vi, vim, emacs, etc etc
```

Remove the final chunk of code containing `twm` and apps. Replace with: `exec i3`

Save, return to console, and execute:

```bash
$ startx
```

Now hit `Meta+Enter` to show a console and begin to rice starting at file `/etc/i3/config`.

### Installing programs

---

All programs to make i3 usable. 

#### Terminal

---

Install `kitty` with the following command

```bash
$ sudo pacman -S kitty
```

**TODO:** zsh, ohmyzsh en theme met altigen

#### Microcode

---

Install the [intel-ucode](https://www.archlinux.org/packages/?name=intel-ucode) package

```bash
$ sudo pacman -S intel-ucode
```

Enable early loading, this updates the microcode very early during boot, before the initramfs stage, so it is the preferred method.

**Refind:**

```bash
$ sudo nano /boot/refind_linux.conf
```

Example config with microcode and separate boot partition.

```bash
"Arch Linux         " "root=UUID=5c18e74a-3e4b-4143-9481-66db2b6da794 rw add_efi_memmap initrd=/intel-ucode.img initrd=/initramfs-linux.img"
```

#### CPU frequency scaling

---

Install the `thermald` and `cpupower` packages:

```bash
$ yay -S thermald cpupower
```

To set the maximum clock frequency (clock_freq is a clock frequency with units: GHz, MHz):

```bash
$ cpupower frequency-set -u clock_freq
```

To set the minimum clock frequency:

```bash
$ cpupower frequency-set -d clock_freq
```

To monitor cpu frequency realtime:

```bash
$ watch grep \"cpu MHz\" /proc/cpuinfo
```

#### Battery notifications

---

I would like to get a notification when my battery is below 15% and suspend when 5% battery or less is detected:

```bash
$ sudo nano /etc/udev/rules.d/99-lowbat.rules
```

Add the following to the file:

```bash
# Send notification when battery is low
SUBSYSTEM=="power_supply", ATTR{status}=="Discharging", ATTR{capacity}=="[15-6]", RUN+="/home/niek/Scripts/System/battery.sh"
# Suspend the system when battery level drops to 5% or lower
SUBSYSTEM=="power_supply", ATTR{status}=="Discharging", ATTR{capacity}=="[0-5]", RUN+="/usr/bin/systemctl suspend"
```

Make a script in the following location `/home/niek/Scripts/System/battery.sh`

```bash
#!/bin/sh
battery_level=`acpi -b | cut -d ' ' -f 4 | grep -o '[0-9]*' | head -1`
notify-send "Battery low" "Battery level is ${battery_level}%!"
```

#### File manager and appearance

---

Install `nautilus` with the following command:

```bash
$ sudo pacman -S nautilus
```

Install lxappearance and gtk-chtheme with the following command:

```bash
$ sudo pacman -S lxappearance gtk-chtheme
```

Now install a gtk theme, for example `materia-gtk-theme`. Start with lxappearance and choose the theme, then choose it in gtk-chtheme.

#### Trackpad

---

Install `xf86-input-synaptics` and `xf86-input-libinput`.

```bash
$ sudo pacman -S xf86-input-libinput xf86-input-synaptics
```

Enable touch to click

```bash
$ sudo nano /etc/X11/xorg.conf.d/30-touchpad.conf
```

Add the following to the file:

```bash
Section "InputClass"
	Indentifier "libinput touchpad catchall"
	MatchIsTouchpad "on"
	MatchDevicePath "/dev/input/event*"
	Option "Tapping" "On"
	Driver "libinput"
EndSection
```

**libinput-gestures**

Install `libinput-gestures` from the AUR.

```bash
$ yay -S libinput-gestures
```

Add user to the input group

```bash
$ sudo gpasswd -a $USER input
```

Autostart the application by adding the following line to the `.xinitrc` file

```bash
libinput-gestures &
```

Copy the configuration file to the following location

```bash
nano $HOME/.config/libinput-gestures.conf
```

#### Theme for the bootmanager

---

If all went well, rEFInd was installed during configuration with `archfi`. Now we want to install a nice theme to make it look good. I like [this](https://github.com/noraj/refind-theme-regular) one. To install the theme, execute the following command:

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/noraj/refind-theme-regular/master/install.sh)"
```

I had the problem that my windows install was not detected. I fixed this by adding a small delay in the config.

```bash
# Delay for the specified number of seconds before scanning disks.
# This can help some users who find that some of their disks
# (usually external or optical discs) aren't detected initially,
# but are detected after pressing Esc.
# The default is 0.
#
scan_delay 1
```

Also, set `use_graphics_for` to linux so we get a cleaner boot process. 

```bash
# Launch specified OSes in graphics mode. By default, rEFInd switches
# to text mode and displays basic pre-launch information when launching
# all OSes except macOS. Using graphics mode can produce a more seamless
# transition, but displays no information, which can make matters
# difficult if you must debug a problem. Also, on at least one known
# computer, using graphics mode prevents a crash when using the Linux
# kernel's EFI stub loader. You can specify an empty list to boot all
# OSes in text mode.
# Valid options:
#   osx     - macOS
#   linux   - A Linux kernel with EFI stub loader
#   elilo   - The ELILO boot loader
#   grub    - The GRUB (Legacy or 2) boot loader
#   windows - Microsoft Windows
# Default value: osx
#
use_graphics_for linux
```

#### Plymouth

---

Install `plymouth` from the AUR.

```bash
$ yay -S plymouth
```

Add plymouth to the HOOKS array in mkinitcpio.conf. It must be added after base and udev for it to work:

```bash
$ sudo nano /etc/mkinitcpio.conf

#HOOKS=(base udev plymouth ...)
```

Add the following to the kernel parameters

```bash
quiet splash loglevel=3 rd.udev.log_priority=3 vt.global_cursor_default=0
```

#### Lockscreen

---

Install `betterlockscreen` from the AUR.

```bash
$ yay -S betterlockscreen
```

Cache the desired wallpaper:

```bash
$ betterlockscreen -u Path/To/Wallpaper.jpg -b 0.5
```

**Optional:** use the alias function to improve the experience. Place the following in .zshrc

```bash
alias lock="betterlockscreen -l blur -t 'Hard work beats talent.' > /dev/null"
alias suspend="betterlockscreen -s blur -t 'Hard work beats talent.' > /dev/null"
```

#### Notification deamon

---

Install deadd with the following command:

```bash
$ yay -S deadd-notification-center
```

Copy the config in to the following file:

```bash
$ nano .config/deadd/deadd.conf
```

#### Volume and brightness indicator

---

Install `volnoti-brightness-git` from the AUR.

```bash
$ yay -S volnoti-brightness-git
```

The following section of the i3 config file is using `volnoti`

```bash
# brightness
bindsym XF86MonBrightnessUp exec   --no-startup-id "xbacklight -inc 10 && volnoti-show >
bindsym XF86MonBrightnessDown exec --no-startup-id "xbacklight -dec 10 && volnoti-show >

# volume
bindsym XF86AudioRaiseVolume exec --no-startup-id "amixer set Master 5%+ && volnoti-sho>
bindsym XF86AudioLowerVolume exec --no-startup-id "amixer set Master 5%- && volnoti-sho>
bindsym XF86AudioMute exec --no-startup-id "amixer set Master toggle && if amixer get M>
```

#### Screenshot tool

---

Install `deepin-screenshot` with pacman.

```bash
sudo pacman -S deepin-screenshot
```

Add the following line to the i3 config.

```bash
bindsym Print exec --no-startup-id "deepin-screenshot"
```

#### Autotiling

---

The [autotiling](https://aur.archlinux.org/packages/autotiling/) ****AUR package can be used for automatic switching horizontal / vertical window split orientation resulting in a similar behavior to the spiral tiling of bspwm. After installation add the following to your `~/.config/i3/config` and reload i3.

```
exec_always --no-startup-id autotiling
```

#### Media control

---

First, install the `playerctl` package:

```bash
$ sudo pacman -S playerctl
```

Since I have no dedicated media control keys, I use mod+i, mod+o and mod+p:

```bash
# media controls
bindsym $mod+i exec --no-startup-id "playerctl previous"
bindsym $mod+o exec --no-startup-id "playerctl play-pause"
bindsym $mod+p exec --no-startup-id "playerctl next"
```

#### Redshit

---

Redshift adjusts the color temperature of your screen according to your surroundings. This may help your eyes hurt less if you are working in front of the screen at night. To install redshift:

```bash
sudo pacman -S redshift
```

Copy the [sample](https://raw.githubusercontent.com/jonls/redshift/master/redshift.conf.sample) config file to the following location:

```bash
$ nano .config/redshift/redshift.conf
```

Adjust the settings like temperature and location. Now add the following line **before** `exec i3` to the `.xinitrc` file so it starts automatically:

```bash
redshift &
```

#### Useful programs

---

Some useful programs that can be installed with pacman

- Arandr - control external screens
- Evince - PDF viewer

### Conclusion
In this article we have installed Arch Linux, i3 window manager and several programs to make this setup work for a daily driver. 

Thanks for reading! If you have found a mistake, want to ask a question or have a comment, please send me an [email](mailto:ik@niekvanleeuwen.nl).