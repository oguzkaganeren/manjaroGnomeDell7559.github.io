## Dell Inspiron 7559 Manjaro Linux Guide
![image](https://github.com/oguzkaganeren/manjarodell7559.github.io/blob/master/Screenshot%20from%202018-03-23%2021-29-18.png)
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
add 
```systemd.mask=mhwd-live.service acpi_osi=! acpi_osi="Windows 2009" ``` 
at boot with press e.
Don't forget to add `acpi_osi=! acpi_osi=\"Windows 2009\" `to your GRUB_CMDLINE_LINUX_DEFAULT on` /etc/default/grub` and run `sudo update-grub`
For Fastest pacman mirror;
```
sudo pacman-mirrors -f -b stable
```
### Open Wifi Hotspot
```
sudo create_ap wlp5s0 wlp5s0 MyAccessPoint password
```
### For Nvidia
```
sudo pacman -S virtualgl lib32-virtualgl lib32-primus primus

sudo mhwd -f -i pci video-hybrid-intel-nvidia-bumblebee

sudo systemctl enable bumblebeed
sudo reboot
optirun -b none nvidia-settings -c :8
```
here[Link](https://wiki.manjaro.org/index.php?title=Configure_NVIDIA_(non-free)_settings_and_load_them_on_Startup#Bumblebee_and_Steam)

### For Extra
```
sudo pacman -S --noconfirm --needed powerpill
sudo powerpill -S xf86-video-fbdev --noconfirm --needed
sudo powerpill -S --noconfirm --needed aria2
sudo powerpill -S --noconfirm --needed cmatrix
sudo powerpill -S --noconfirm --needed variety
sudo powerpill -S --noconfirm --needed atom
sudo powerpill -S --noconfirm --needed git
sudo powerpill -S --noconfirm --needed chromium
sudo powerpill -S --noconfirm --needed deepin-movie
sudo powerpill -S --noconfirm --needed archey3
sudo powerpill -S --noconfirm --needed bleachbit
sudo powerpill -S --noconfirm --needed grsync
sudo powerpill -S --noconfirm --needed gtk-engine-murrine
sudo powerpill -S --noconfirm --needed hardinfo
sudo powerpill -S --noconfirm --needed hddtemp
sudo powerpill -S --noconfirm --needed htop
sudo powerpill -S --noconfirm --needed mlocate
sudo powerpill -S --noconfirm --needed net-tools
sudo powerpill -S --noconfirm --needed screenfetch
sudo powerpill -S --noconfirm --needed scrot
sudo powerpill -S --noconfirm --needed sysstat
sudo powerpill -S --noconfirm --needed ttf-ubuntu-font-family
sudo powerpill -S --noconfirm --needed tumbler
sudo powerpill -S --noconfirm --needed unclutter
sudo powerpill -S --noconfirm --needed rxvt-unicode
sudo powerpill -S --noconfirm --needed unace unrar zip unzip sharutils uudeview arj cabextract
sudo powerpill -S --noconfirm --needed base-devel
sudo powerpill -S speedtest-cli
sudo powerpill -S youtube-dl
sudo powerpill -S spyder3
sudo powerpill -S  xorg-xrandr
sudo powerpill -S terminator
sudo powerpill -S yaourt
sudo yaourt -S oh-my-zsh-git
```
In the terminal write this;
```
chsh -s /usr/bin/zsh
```
after that,
```
sudo powerpill -S zsh-theme-powerlevel9k 
echo 'source /usr/share/zsh-theme-powerlevel9k/powerlevel9k.zsh-theme' >> ~/.zshrc
yaourt -S zsh-autosuggestions 
echo 'source /usr/share/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh' >> ~/.zshrc

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
yaourt -S undistract-me-git
sudo hardcode-fixer
sudo powerpill -S ttf-roboto --noconfirm --needed
sudo powerpill -S adobe-source-sans-pro-fonts --noconfirm --needed
yaourt -S conky-lua-archers
yaourt -S android-studio
yaourt -S eclipse-java
yaourt -S materia-theme
yaourt -S youtube-dl-gui-git
yaourt -S gnome-shell-extension-installer
yaourt -S ungoogled-chromium
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
gsettings set org.gnome.settings-daemon.plugins.color night-light-schedule-automatic false
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
sudo powerpill -S virt-manager qemu vde2 ebtables dnsmasq bridge-utils openbsd-netcat
sudo systemctl enable libvirtd.service
sudo systemctl start libvirtd.service
```
#### For equalizer;
```
sudo powerpill -S pulseaudio-equalizer
sudo powerpill -S pavucontrol
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
You can add `sudo nano /etc/default/grub` To disable sub menus on your end please add the following line:
```
GRUB_DISABLE_SUBMENU=y
```

Not:If you get any problem on boot, you can apply https://wiki.manjaro.org/index.php/Restore_the_GRUB_Bootloader#For_UEFI_Systems
#### Install plymount (Opening screen-splash sreen);
```
yaourt -S plymouth-theme-arch-breeze-git
sudo plymouth-set-default-theme -R arch-breeze

```
After that add ```splash``` in grub with grub customizer. For silent bot(removing boot message), you should add ` loglevel=3 rd.systemd.show_status=auto rd.udev.log_priority=3` in grub after splash command.
