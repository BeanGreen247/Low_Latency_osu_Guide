# Low latency osu! guide
## The requirements

Install wine staging. To do so follow the guide
```
https://wiki.winehq.org/Download
```
For **Arch Linux** (my case) go and open your terminal. We will need to edit the pacman.conf file
```
sudo nano /etc/pacman.conf
```
Scroll down and find where it starts saying [testing], [core], [extra], [community] and so on. Make sure to remove "#" at he beginning of the line so it looks like this.
```
# The testing repositories are disabled by default. To enable, uncomment the
# repo name header and Include lines. You can add preferred servers immediately
# after the header, and they will be used before the default mirrors.

[testing]
Include = /etc/pacman.d/mirrorlist

[core]
Include = /etc/pacman.d/mirrorlist

[extra]
Include = /etc/pacman.d/mirrorlist

[community-testing]
Include = /etc/pacman.d/mirrorlist

[community]
Include = /etc/pacman.d/mirrorlist

# If you want to run 32 bit applications on your x86_64 system,
# enable the multilib repositories as required here.

[multilib-testing]
Include = /etc/pacman.d/mirrorlist

[multilib]
Include = /etc/pacman.d/mirrorlist

# An example of a custom package repository.  See the pacman manpage for
# tips on creating your own repositories.
#[custom]
#SigLevel = Optional TrustAll
#Server = file:///home/custompkgs
```
Next update the packages
```
sudo pacman -Suy
```
And install wine-staging
```
sudo pacman -Sy wine-staging
```
And lastly install all of these packages
```
sudo pacman -Sy giflib lib32-giflib libpng lib32-libpng libldap lib32-libldap gnutls lib32-gnutls mpg123 lib32-mpg123 openal lib32-openal v4l-utils lib32-v4l-utils libpulse lib32-libpulse libgpg-error lib32-libgpg-error alsa-plugins lib32-alsa-plugins alsa-lib lib32-alsa-lib libjpeg-turbo lib32-libjpeg-turbo sqlite lib32-sqlite libxcomposite lib32-libxcomposite libxinerama lib32-libgcrypt libgcrypt lib32-libxinerama ncurses lib32-ncurses opencl-icd-loader lib32-opencl-icd-loader libxslt lib32-libxslt libva lib32-libva gtk3 lib32-gtk3 gst-plugins-base-libs lib32-gst-plugins-base-libs vulkan-icd-loader lib32-vulkan-icd-loader cups samba alsa alsa-utils alsa-tools gnutls libpng
```
With that please reboot and proceed with the guide

## The guide

Create the main directory (we will be using this directory for all the configurations and tweaks....may be different in your case)
```
sudo mkdir /mnt/84C2FF4EC2FF42CA/osu-wine-prefix
```
Create wine prefix
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/ WINEARCH=win32 winecfg
```
Install winetricks
```
wget https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks
chmod +x winetricks
sudo mv -v winetricks /usr/local/bin
```
Install .NET Framework
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/ WINEARCH=win32 winetricks -q --force dotnet462
```
Lower audio latency
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/ WINEARCH=win32 winetricks sound=alsa
cat > dsound.reg << "EOF"
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Wine\DirectSound]
"HelBuflen"="512"
"SndQueueMax"="3"
EOF
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/ WINEARCH=win32 wine regedit dsound.reg
```
Create osu directory where osu! is going to be installed
```
sudo mkdir /mnt/84C2FF4EC2FF42CA/osu-wine-prefix/drive_c/users/root/Application\ Data/osu\!
```
Download the Installer https://m1.ppy.sh/r/osu!install.exe

Copy it to your newly created folder
```
sudo cp -r ~/Downloads/osu\!install.exe /mnt/84C2FF4EC2FF42CA/osu-wine-prefix/drive_c/users/root/Application\ Data/osu\!
```
Install japanese fonts and icons
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/ WINEARCH=win32 winetricks -q cjkfonts gdiplus
```
Install dxvk 1.3.1

Download https://github.com/doitsujin/dxvk/releases/download/v1.3.1/dxvk-1.3.1.tar.gz

Extract dxvk-1.3.1.tar.gz to your home folder and copy using the following command
```
sudo cp -r dxvk-1.3.1/x32/* /mnt/84C2FF4EC2FF42CA/osu-wine-prefix/drive_c/windows/system32/
```
Open winecfg
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/ WINEARCH=win32 winecfg
- Set Windows version to Windows 7 if not done already
- Go to libraries and add
>>d3d10
>>d3d10_1
>>d3d10core
>>d3d11
>>dxgi
- Set them to native and click apply
- Next go to graphics and tick and "Allow the window manager to control the windows"
- Next go to Staging and tick the first four
```
Next install DirectX

To do so download the DirectX web installer
https://www.microsoft.com/en-us/download/details.aspx?id=8109

Then launch it using the following command
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/ WINEARCH=win32 wine ~/Downloads/directx_Jun2010_redist.exe
```
It will aske where to install. Pick C:\

Once done launch the installer
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/ WINEARCH=win32 wine /mnt/84C2FF4EC2FF42CA/osu-wine-prefix/drive_c/DXSETUP.exe
```

Once finished with the install continue.

Create a custom terminal command for launching osu!
```
cat > osu << "EOF"
#!/bin/sh
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/ WINEARCH=win32 wine /mnt/84C2FF4EC2FF42CA/osu-wine-prefix/drive_c/users/root/Application\ Data/osu\!/osu\!.exe
EOF
```
Create a custom terminal command for killing osu!
```
cat > osukill << "EOF"
#!/bin/sh
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/ WINEARCH=win32 wineserver -k
EOF
```
Make the commands executable and make them accesable from the terminal
```
sudo mv osu /usr/bin/
sudo chmod +x /usr/bin/osu
sudo mv osukill /usr/bin/
sudo chmod +x /usr/bin/osukill
```
More tweaks to get rid of vsync
```
sudo mkdir -v /etc/X11/xorg.conf.d
cat > tmp.conf << "EOF"
Section "Device"
   Identifier   "Radeon"
   Driver      "radeon"
   Option      "SwapbuffersWait"   "off"
EndSection
EOF
sudo mv tmp.conf /etc/X11/xorg.conf.d/10-radeon.conf
```
Mouse tweak
```
xset m 0 0
```
Next reboot

Launch the Installer
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/ WINEARCH=win32 wine /mnt/84C2FF4EC2FF42CA/osu-wine-prefix/drive_c/users/root/Application\ Data/osu\!/osu\!install.exe
```

After that make sure to run osu with
```
sudo osu
```
Once open make sure to let it cache for really low latency (latency decreases as you play)

To speed it up pick any really long map and enable HR,NC and AT

Let osu play the map till the end and let the leaderboards and all of the ui element enabled

Once done you can play with low latency enjoy 

I would recommend closing osu normally and then kill wine by 
```
sudo osukill
```
And reopen it to ensure that the changes applied succesfully
