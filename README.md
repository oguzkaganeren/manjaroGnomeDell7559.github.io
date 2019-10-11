## Dell Inspiron 7559 Manjaro Linux Guide(Updated 11.10.19)
You can download ise here[Link](https://manjaro.org/download/official/gnome/) and prepare bootable disk with rufus(DD mode)[Link](https://rufus.akeo.ie/) or if you are on linux, you can use etcher, DD, imagewriter etc.. After that, reboot your computer and when you see the dell logo, you should press F2 and disable boot secure in boot section, then close the bios with F10(Yes). Again at the dell logo, you should press F12 and select your bootable disk. After that you see that Manjaro boot menu, select Boot:Manjaro Linux. 

Now, you can start the installiation. Follow the steps in the Manjaro User Guide. You can download it [Here](https://manjaro.org/support/userguide/) or find your on live manjaro system.
**After the installiation**
add 
```acpi_osi=! acpi_osi="Windows 2009" ``` 
at boot with press e.
> This settings for nvidia bumblebee. If you use Nvidia prime do not apply it.
After started your system, add `acpi_osi=! acpi_osi=\"Windows 2009\" `to your GRUB_CMDLINE_LINUX_DEFAULT using`sudo nano /etc/default/grub` and run `sudo update-grub`
### Fastest pacman
```
sudo pacman-mirrors --fasttrack 5
```
### Update Your System
```
sudo pacman -Syu
```



### Packages I use
```
sudo pacman -S aria2 ttf-ubuntu-font-family speedtest-cli deepin-movie deepin-calculator telegram-desktop kdenlive inkscape create_ap gedit virtualbox fish flameshot deepin-terminal neofetch soundconverter gtop
```
### Change the bash shell to fish
```
chsh -s /usr/bin/fish
curl -L https://get.oh-my.fish | fish
omf install bobthefish
```
## Customize shell and terminal
Change `.config/fish/omf.fish` with [this](https://github.com/oguzkaganeren/manjaro-cinnamon-dell-7559/blob/master/.config/fish/omf.fish)
Set default deepin terminal then open it. Right click on the terminal and switch theme `argonaut`.
### Aur Packages I use
```
yay -S materia-theme opera chromium ttf-font-awesome ttf-font-awesome-4 ttf-roboto android-studio woeusb-git jdownloader2 ttf-ms-fonts vscodium-bin breeze-blurred-git otf-san-francisco xdman gwe svr zettlr-bin fslint waterfox-bin odio-appimage
```
### Adblock Spotify
```
yay -S --mflags --skipinteg --needed spotify spotify-adblock
```
>  :exclamation: If you have a SSD, you should enable fstrim.
```
sudo systemctl enable fstrim.timer
```
### If headphones not detected when restart (after startup not working, I am working on it )
```
sudo nano /etc/pulse/default.pa
```
at the bottom under `### Make some devices default` put
```
set-default-sink 1
```
### Installing Nvidia Drivers(bumblebee)
Use Manjaro Setting Manager > Hardware Configuration > Auto Install Proprietary Driver
After Installation,
```
sudo gpasswd -a <user> bumblebee
reboot
```
### Open gwe
```
optirun gwe --ctrl-display ":8"
```

#### Shortkey
For showing desktop, Settings > Keyboard > Hide all normal windows

You change ctrl+alt+delete default value and add that;
```
gnome-system-monitor
```
### Open Wifi Hotspot
```
sudo create_ap wlp5s0 wlp5s0 MyAccessPoint password
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
sudo pacman -S virt-manager qemu vde2 ebtables dnsmasq bridge-utils openbsd-netcat
sudo systemctl enable libvirtd.service
sudo systemctl start libvirtd.service
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
