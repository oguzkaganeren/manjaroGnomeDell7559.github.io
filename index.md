## Dell Inspiron 7559 Manjaro Linux Guide

You can download ise here[Link](https://downloads.sourceforge.net/manjarolinux/manjaro-gnome-17.1.4-stable-x86_64.iso)  and prepare bootable disk with rufus[Link](https://rufus.akeo.ie/) . After that, reboot your computer and when you see the dell logo, you should press F2 and disable boot secure in boot section, then close the bios with F10(Yes). Again at the dell logo, you should press F12 and select your bootable disk. After that you see that Manjaro boot menu, select Boot:Manjaro Linux. When your system is ready for installiation, you should apply these steps for getting any error.
```
sudo gedit /lib/calamares/modules/mhwdcfg/main.py
```
edit it with this;
```
def run():
    """ Configure the hardware """
    
    mhwd = MhwdController()
    
    # return mhwd.run()
    return None # <- Add this and comment the above line
```
Now, you can start the installiation.[Here](https://www.linuxtechi.com/manjaro-17-05-gnome-installation-guide-screenshots/)
**After the installiation**
add `systemd.mask=mhwd-live.service and acpi_osi=! acpi_osi="Windows 2009" ` at boot with press e.
Don't forget to add `acpi_osi=! acpi_osi=\"Windows 2009\" `to your GRUB_CMDLINE_LINUX_DEFAULT on` /etc/default/grub` and run `sudo update-grub`
For Fastest pacman mirror;
```
sudo pacman-mirrors -f -b stable
```
### For Nvidia
```
sudo pacman -S virtualgl lib32-virtualgl lib32-primus primus

sudo mhwd -f -i pci video-hybrid-intel-nvidia-bumblebee

sudo systemctl enable bumblebeed
sudo reboot
optirun -b none nvidia-settings -c :8
```

### For Extra
```
sudo pacman -S --noconfirm --needed  aria2
sudo pacman -S --noconfirm --needed cmatrix
sudo pacman -S --noconfirm --needed variety
sudo pacman -S --noconfirm --needed atom
sudo pacman -S --noconfirm --needed git
sudo pacman -S --noconfirm --needed chromium
sudo pacman -S --noconfirm --needed deepin-movie
sudo pacman -S --noconfirm --needed archey3
sudo pacman -S --noconfirm --needed bleachbit
sudo pacman -S --noconfirm --needed grsync
sudo pacman -S --noconfirm --needed gtk-engine-murrine
sudo pacman -S --noconfirm --needed hardinfo
sudo pacman -S --noconfirm --needed hddtemp
sudo pacman -S --noconfirm --needed htop
sudo pacman -S --noconfirm --needed mlocate
sudo pacman -S --noconfirm --needed net-tools
sudo pacman -S --noconfirm --needed screenfetch
sudo pacman -S --noconfirm --needed scrot
sudo pacman -S --noconfirm --needed sysstat
sudo pacman -S --noconfirm --needed ttf-ubuntu-font-family
sudo pacman -S --noconfirm --needed tumbler
sudo pacman -S --noconfirm --needed unclutter
sudo pacman -S --noconfirm --needed rxvt-unicode
sudo pacman -S --noconfirm --needed unace unrar zip unzip sharutils uudeview arj cabextract
sudo pacman -S --noconfirm --needed base-devel
sudo pacman -S youtube-dl
sudo pacman -S spyder3
sudo pacman -S  xorg-xrandr
sudo pacman -S yaourt
```
#### Shortkey
For showing desktop:
```
wnckprop --show-desktop
```
You change ctrl+alt+delete default value and add that;
```
gnome-system-monitor
```
#### Editing yaourt and terminal
```
sudo gedit ~/.yaourtrc
```
and add that;
```
BUILD_NOCONFIRM=1
EDITFILES=0
```
For Terminal;
```
sudo gedit ~/.bashrc
```
add that at bottom;
```
if [ -f /usr/bin/screenfetch ]; then screenfetch -D arch; fi
```
```
yaourt -S whatsapp-desktop
yaourt -S cool-retro-term
yaourt -S temps
yaourt -S vivaldi
yaourt -S insync
yaourt -S gradio peek radiotray
yaourt -S arc-gtk-theme downgrade paper-icon-theme papirus-icon-theme ttf-font-awesome
yaourt -S hardcode-fixer-git
yaourt -S uget-chrome-wrapper
yaourt -S gnome-terminal-transparency
sudo hardcode-fixer
sudo pacman -S ttf-roboto --noconfirm --needed
sudo pacman -S adobe-source-sans-pro-fonts --noconfirm --needed
yaourt -S conky-lua-archers
yaourt -S android-studio
yaourt -S eclipse-java
yaourt -S materia-theme
yaourt -S youtube-dl-gui-git
```
Select Materia-theme on tweak tools.
```
sudo cp -v /usr/share/gnome-shell/gnome-shell-theme.gresource{,~}
GTK_THEME=$(gsettings get org.gnome.desktop.interface gtk-theme | sed "s/'//g")
sudo cp -v /usr/share{/themes/$GTK_THEME,}/gnome-shell/gnome-shell-theme.gresource
```
#### For closing bluetooth and nightlight;
```
sudo machinectl shell
sudo systemctl stop bluetooth.service
sudo systemctl disable bluetooth.service
systemctl status bluetooth.service
sudo systemctl mask bluetooth.service
gsettings set org.gnome.settings-daemon.plugins.color night-light-enabled false
```
#### Libre Office icon;
```
yaourt -S papirus-libreoffice-theme
```
After that;
LibreOffice->tools->options->View->Icon Style->Papirus
---AutoMount(You should change same code to your partiton(fstab))
You can show UUID with;
```
lsblk -f
```
```
sudo gedit //etc/fstab

```
At bottom added these;

```
UUID=DAF6FE7CF6FE5869 /run/media/oguz/D ntfs auto,user,rw 0 2
UUID=C480917680917022 /run/media/oguz/E ntfs auto,user,rw 0 2
```
Save it.
#### For Android Studio(KVM);
```
sudo pacman -S virt-manager qemu vde2 ebtables dnsmasq bridge-utils openbsd-netcat
sudo systemctl enable libvirtd.service
sudo systemctl start libvirtd.service
```
#### For equalizer;
```
sudo pacman -S pulseaudio-equalizer
sudo pacman -S pavucontrol
pactl load-module module-equalizer-sink
pactl load-module module-dbus-protocol
sudo gedit /etc/pulse/default.pa
```
Add these;
```
### Load the integrated PulseAudio equalizer and D-Bus module
load-module module-equalizer-sink
load-module module-dbus-protocol
```
#### For grub theme;
https://www.gnome-look.org/p/1009236/
download it and setup with ```./Install```
#### For Changing for default kernel or OS on boot;
```
yaourt -S grub-customizer
```
Not:If you get any problem on boot, you can apply https://wiki.manjaro.org/index.php/Restore_the_GRUB_Bootloader#For_UEFI_Systems
#### Install plymount (Opening screen-splash sreen);
```
yaourt -S plymouth-theme-arch-breeze-git
sudo plymouth-set-default-theme -R arch-breeze

```
After that add ```splash``` in grub with grub customizer. For silent bot(removing boot message), you should add ` loglevel=3 rd.systemd.show_status=auto rd.udev.log_priority=3` in grub after splash command.
