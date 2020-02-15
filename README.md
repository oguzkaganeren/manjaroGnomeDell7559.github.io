## Dell Inspiron 7559 Manjaro Linux Guide(Updated 28.11.19)
You can download it [here](https://manjaro.org/download/official/gnome/) and prepare bootable disk with [rufus](https://rufus.akeo.ie/) (with DD mode) or if you are on linux, you can use etcher, DD, imagewriter etc..

After that, reboot your computer and when you see the dell logo, you should press F2 and disable boot secure in boot section, then close the bios with F10(Yes). Again at the dell logo, you should press F12 and select your bootable disk. Then you see that Manjaro boot menu, select Boot:Manjaro Linux. 

Now, you can start the installiation. Follow the steps in the Manjaro User Guide. You can download [Manjaro User Guide](https://manjaro.org/support/userguide/) or find your on live manjaro system.
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
[Here my notes for all manjaro](https://github.com/oguzkaganeren/manjaro-notes)


### Change login screen background
Download the [script](https://github.com/oguzkaganeren/manjaroGnomeDell7559.github.io/blob/master/set-gdm-wallpaper.sh)
```
sudo bash set-gdm-wallpaper.sh /run/media/oguz/Files/Wallpapers/login.jpg
```

