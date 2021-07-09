# Kali-Parrot_Dual_Monitor
Fix the Issue (OS can not detect HDMI monitors)



To fix that Issue:

1- Download Display_on 

2- chmod +x Display_on

3- Press cltr + alt + (f1 or f2) #to run (X terminal) 

4- sudo ./Display_on

5- Reboot

--------------------------------------------------------------------

Display_on:

#!/bin/bash

sudo apt install linux-headers-$(uname -r)

sudo echo "blacklist nouveau

options nouveau modeset=0" > /etc/modprobe.d/blacklist-nouveau.conf

sudo update-initramfs -u

apt install nvidia-driver nvidia-xconfig

sudo modprobe nvidia-drm

nvidia-xconfig --query-gpu-info
 Similar --> PCI BusID: PCI:1:0:0

sudo echo 'Section "ServerLayout"
	Identifier "layout"
	Screen 0 "nvidia"
	Inactive "intel"
EndSection
 
Section "Device"
	Identifier "nvidia"
	Driver "nvidia"
	BusID "PCI:1:0:0"
EndSection
 
Section "Screen"
	Identifier "nvidia"
	Device "nvidia"
	Option "AllowEmptyInitialConfiguration"
EndSection
 
Section "Device"
	Identifier "intel"
	Driver "modesetting"
EndSection
 
Section "Screen"
	Identifier "intel"
	Device "intel"
EndSection

' > /etc/X11/xorg.conf.d/xorg.conf

sudo echo '
[Desktop Entry]
Type=Application
Name=Optimus
Exec=sh -c "xrandr --setprovideroutputsource modesetting NVIDIA-0; xrandr --auto"
NoDisplay=true
X-GNOME-Autostart-Phase=DisplayServer' > /etc/xdg/autostart/optimus.desktop 


sudo echo '
[Desktop Entry] 
Type=Application
Name=Optimus
Exec=sh -c "xrandr --setprovideroutputsource modesetting NVIDIA-0; xrandr --auto"
NoDisplay=true
X-GNOME-Autostart-Phase=DisplayServer' > /usr/share/gdm/greeter/autostart/optimus.desktop

