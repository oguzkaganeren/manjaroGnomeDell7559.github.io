## Dell Inspiron 7559 Manjaro Linux Guide
![image](https://github.com/oguzkaganeren/manjarodell7559.github.io/blob/master/Screenshot%20from%202018-03-23%2021-29-18.png)
You can download ise here[Link](https://downloads.sourceforge.net/manjarolinux/manjaro-gnome-17.1.4-stable-x86_64.iso)  and prepare bootable disk with rufus[Link](https://rufus.akeo.ie/) . After that, reboot your computer and when you see the dell logo, you should press F2 and disable boot secure in boot section, then close the bios with F10(Yes). Again at the dell logo, you should press F12 and select your bootable disk. After that you see that Manjaro boot menu, select Boot:Manjaro Linux. 

Now, you can start the installiation.[Here](https://www.linuxtechi.com/manjaro-17-05-gnome-installation-guide-screenshots/)
**After the installiation**
add 
```systemd.mask=mhwd-live.service acpi_osi=! acpi_osi="Windows 2009" ``` 
at boot with press e.
After started your system, add `acpi_osi=! acpi_osi=\"Windows 2009\" `to your GRUB_CMDLINE_LINUX_DEFAULT using`sudo nano /etc/default/grub` and run `sudo update-grub`
### Fastest pacman
```
sudo pacman-mirrors --fasttrack
```
### Update Your System
```
sudo pacman -Syyu
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

### Packages I use
```
sudo pacman -S --noconfirm --needed git pulseaudio pulseaudio-alsa alsa-utils alsa-plugins pavucontrol aria2 screenfetch ttf-ubuntu-font-family rxvt-unicode unace unrar zip unzip sharutils uudeview arj cabextract speedtest-cli ntp deepin-movie virt-manager qemu vde2 ebtables dnsmasq bridge-utils openbsd-netcat tlp tlp-rdw iw smartmontools ethtool x86_energy_perf_policy lm_sensors yay intel-ucode xf86-video-fbdev deepin-calculator telegram-desktop gimp kdenlive inkscape terminus-font gufw firejail create_ap gedit virtualbox mtpaint
```

### INTEL - Enable Early Kernel Mode Setting for i915 module.
Edit /etc/mkinitcpio.conf file and in MODULES section add i915.
```
# MODULES
# The following modules are loaded before any boot hooks are
# run.  Advanced users may wish to specify all system modules
# in this array.  For instance:
#     MODULES=(piix ide_disk reiserfs)
MODULES=(i915)
```
Save and
```
sudo mkinitcpio -P
```
### Aur Packages I use
```
yay -S --noconfirm materia-theme opera chromium spotify ttf-font-awesome ttf-font-awesome-4 powerline-fonts ttf-roboto  adobe-source-sans-pro-fonts android-studio woeusb-git papirus-icon-theme ntfs-3g  jdownloader2 ttf-ms-fonts ephifonts otf-exo oh-my-zsh-git uGet-Integrator thermald vscodium-bin
```
### Power Settings
```
sudo timedatectl set-ntp true
sudo systemctl enable libvirtd.service
sudo systemctl start libvirtd.service
sudo systemctl mask systemd-rfkill.socket systemd-rfkill.service
sudo sensors-detect
sudo systemctl enable thermald
sudo systemctl start thermald
```
### ZSH(Optional)
In the terminal, write this;
```
chsh -s /usr/bin/zsh
```
after that,
```
sudo pacman -S zsh-theme-powerlevel9k 
echo 'source /usr/share/zsh-theme-powerlevel9k/powerlevel9k.zsh-theme' >> ~/.zshrc
yay -S zsh-autosuggestions 
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
#### Editing terminal
For Terminal;
```
sudo gedit ~/.bashrc
```
add that at bottom;
```
if [ -f /usr/bin/screenfetch ]; then screenfetch -D arch; fi
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
yay -S papirus-libreoffice-theme
```
After that;
LibreOffice->tools->options->View->Icon Style->Papirus
### For Other Partitations
If you have another partition(E, D etc.). You can mount it on the startup. Thus some applications which are using other partitions don't get an error.

```
lsblk -f
```
The command shows your disks uuid.
```
sudo gedit /etc/fstab 
```
Open your fstab config with the command. You should add codes similar to the following example. You should change UUID and /run/media/yourUserName/Partition.
```
UUID=DAF6FE7CF6FE5869 /run/media/oguz/D ntfs-3g defaults  0 0
UUID=C480917680917022 /run/media/oguz/E ntfs-3g defaults  0 0
```

>  :exclamation: If you use manjaro with dual boot, you should close fast-startup,hibarnate on your Windows, otherwise, you have not a write permission for other partitions.
#### For Android Studio(KVM);
```
sudo powerpill -S virt-manager qemu vde2 ebtables dnsmasq bridge-utils openbsd-netcat
sudo systemctl enable libvirtd.service
sudo systemctl start libvirtd.service
```

#### Install plymount (Opening screen-splash sreen)(Optional);
```
yay -S plymouth-theme-arch-breeze-git
sudo plymouth-set-default-theme -R arch-breeze

```
After that add ```splash``` in grub with grub customizer. For silent bot(removing boot message), you should add ` loglevel=3 rd.systemd.show_status=auto rd.udev.log_priority=3` in grub after splash command.
# Solving Problems
## Android Studio
If you get
`
libGL error: unable to load driver: i965_dri.so
`
Open terminal in your SDK folder `/emulator/lib64/libstdc++`
```
mv libstdc++.so.6 libstdc++.so.6.bak
ln -s /usr/lib64/libstdc++.so.6
```
## Stuck at hardware detection
>  :exclamation: When installing, It can be stuck at hardware detection. You can apply these settings. It does not appear the newer iso.
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
