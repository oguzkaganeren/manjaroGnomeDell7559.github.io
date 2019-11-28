## Dell Inspiron 7559 Manjaro Linux Guide(Updated 28.11.19)
You can download ise here[Link](https://manjaro.org/download/official/gnome/) and prepare bootable disk with rufus(DD mode)[Link](https://rufus.akeo.ie/) or if you are on linux, you can use etcher, DD, imagewriter etc..

After that, reboot your computer and when you see the dell logo, you should press F2 and disable boot secure in boot section, then close the bios with F10(Yes). Again at the dell logo, you should press F12 and select your bootable disk. Then you see that Manjaro boot menu, select Boot:Manjaro Linux. 

Now, you can start the installiation. Follow the steps in the Manjaro User Guide. You can download it [Here](https://manjaro.org/support/userguide/) or find your on live manjaro system.
**After the installation, reboot your system. Press e when you see Manjaro Grub Screen(It appears if your system is dual-boot otherwise you should keep pressing shift button to show grub screen)**
then add the line after `quiet` word.
```acpi_osi=! acpi_osi="Windows 2009" ``` 

```
sudo nano /etc/default/grub 
```
> GRUB_CMDLINE_LINUX_DEFAULT="quiet acpi_osi=! acpi_osi=\"Windows 2009\" acpi_backlight=vendor"

```
sudo update-grub
```

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
sudo pacman -S yay aria2 speedtest-cli telegram-desktop kdenlive inkscape create_ap virtualbox fish flameshot deepin-terminal neofetch gtop kolourpaint autoconf binutils make gcc pkg-config fakeroot
```
### Change the bash shell to fish
```
chsh -s /usr/bin/fish
curl -L https://get.oh-my.fish | fish
omf install bobthefish
```
## Customize shell and terminal
Change `.config/fish/conf.d/omf.fish` with [this](https://github.com/oguzkaganeren/manjaro-cinnamon-dell-7559/blob/master/.config/fish/omf.fish)
Set default deepin terminal then open it. Right click on the terminal and switch theme `argonaut`.
### Aur Packages I use
```
yay -S materia-theme opera ungoogled-chromium-bin ttf-font-awesome ttf-font-awesome-4 ttf-roboto android-studio woeusb-git jdownloader2 ttf-ms-fonts vscodium-bin breeze-blurred-git otf-san-francisco xdman gwe svr zettlr-bin fslint waterfox-bin odio-appimage skypeforlinux-stable-bin posy-cursors all-repository-fonts
```
### Adblock Spotify
```
yay -S --mflags --skipinteg --needed spotify spotify-adblock
```
>  :exclamation: If you have a SSD, you should enable fstrim.
```
sudo systemctl enable fstrim.timer
```
### If skype not run
```
sudo sysctl kernel.unprivileged_userns_clone=1
```
### If headphones not detected when restart (after startup not working, I am working on it )
```
sudo nano /etc/pulse/default.pa
```
at the bottom under `### Make some devices default` put
```
set-default-sink 1
```
## Nvidia Options
### Installing Nvidia Drivers(bumblebee)
Use Manjaro Setting Manager > Hardware Configuration > Auto Install Proprietary Driver
After Installation,
```
sudo gpasswd -a <user> bumblebee
reboot
```
### Set up Nvidia Prime
* Step 1: remove bumblebee

If you installed with the non-free driver option mhwd will have set up bumblebee for you. This will get in the way so the first step is to remove it. Use the mhwd command-line or simply remove it via Manjaro Settings Manager.

* Step 2: install the NVIDIA driver

Use mhwd or MSM to install the nvidia driver in the normal way.

* Step 3: fix mhwd's configuration

mhwd does the sensible thing and puts configuration in place as though the NVIDIA GPU was the only device available. We need to change this setup so PRIME will work.
* Step 3.1: set up a new Xorg configuration

Firstly, remove `/etc/X11/xorg.conf.d/90-mhwd.conf` and replace it with:
`/etc/X11/xorg.conf.d/optimus.conf`

```
Section "Module"
    Load "modesetting"
EndSection

Section "Device"
    Identifier "nvidia"
    Driver "nvidia"
    BusID "PCI:1:0:0"
    Option "AllowEmptyInitialConfiguration"
EndSection
```

While the BusID value above should be correct for most Optimus laptops you should check your values with `lspci | grep -E "VGA|3D"`.
* Step 3.2: Refine blacklisting

PRIME relies on nvidia-drm and mhwd puts it in a blacklist by default, but to ensure the nvidia kernel module will load we still need to blacklist certain other modules. Therefore, you'll have to do some editing of the files in `/etc/modprobe.d`.

To remove the existing blacklist, edit, move or remove any related `mhwd-*` files in `/etc/modprobe.d/`, e.g.
```
ls /etc/modprobe.d/mhwd*
sudo rm /etc/modprobe.d/mhwd-gpu.conf
sudo rm /etc/modprobe.d/mhwd-nvidia.conf
```
The end result must include a blacklist of the following modules, e.g. in `/etc/modprobe.d/nvidia.conf`:
```
blacklist nouveau
blacklist nvidiafb
blacklist rivafb
```

* Step 4: enable nvidia-drm.modeset

Create a new file,
`/etc/modprobe.d/nvidia-drm.conf`
```
options nvidia_drm modeset=1
```
* Step 5: Set the output source for your DM.

* For GDM

e.g. GNOME edition

Create a new file,
`/usr/local/share/optimus.desktop`

```
[Desktop Entry]
Type=Application
Name=Optimus
Exec=sh -c "xrandr --setprovideroutputsource modesetting NVIDIA-0; xrandr --auto"
NoDisplay=true
X-GNOME-Autostart-Phase=DisplayServer
```
and link it into place so it starts with GDM and on login
```
sudo ln -s /usr/local/share/optimus.desktop /usr/share/gdm/greeter/autostart/optimus.desktop
sudo ln -s /usr/local/share/optimus.desktop /etc/xdg/autostart/optimus.desktop
```
Then `reboot`.
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
## Fix ERROR: Cannot find the strip binary required for object file stripping.
```
sudo sudo pacman -S binutils make gcc pkg-config fakeroot
```
