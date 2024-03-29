# Low latency osu! guide
## The requirements

Install wine-staging. To do so follow the guide
```
https://wiki.winehq.org/Download
```

### Ubuntu
For **Ubuntu** run these commands first
```
sudo dpkg --add-architecture i386 
wget -nc https://dl.winehq.org/wine-builds/winehq.key
sudo add-apt-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ focal main'
```
Next update your packages
```
sudo apt update
```
Install Dependencies
```
sudo apt install -y libc6 libgcc1 libglib2.0-0 libgphoto2-6 libgphoto2-port12 libgstreamer-plugins-base1.0-0 libgstreamer1.0-0 liblcms2-2 libldap-2.4-2 libmpg123-0 libopenal1 libpulse0 libudev1 libx11-6 libxext6 libxml2 zlib1g libasound2-plugins libncurses5 libcapi20-3 libcups2 libdbus-1-3 libfontconfig1 libfreetype6 libglu1-mesa libgnutls30 libgsm1 libgssapi-krb5-2 libgtk-3-0 libkrb5-3 libncurses5 libodbc1 libosmesa6 libpcap0.8 libpng16-16 libsane1 libsdl2-2.0-0 libtiff5 libv4l-0 libva-drm2 libva-x11-2 libva2 libvulkan1 libxcomposite1 libxcursor1 libxfixes3 libxi6 libxinerama1 libxrandr2 libxrender1 libxslt1.1 libxxf86vm1 gnutls-bin cabextract
sudo apt install -y libgnutls30:i386 libldap-2.4-2:i386 libgpg-error0:i386 libxml2:i386 libasound2-plugins:i386 libsdl2-2.0-0:i386 libfreetype6:i386 libdbus-1-3:i386 libsqlite3-0:i386
```
Install wine-staging
```
sudo apt install --install-recommends winehq-staging
```

### Debian 10
For **Debian 10** run these commands first
```
sudo dpkg --add-architecture i386 
wget -nc https://dl.winehq.org/wine-builds/winehq.key
wget -nc https://download.opensuse.org/repositories/Emulators:/Wine:/Debian/Debian_10/Release.key
sudo apt-key add winehq.key
sudo apt-key add Release.key
sudo apt-add-repository contrib
```
Next open your /etc/apt/sources.list file and add to the end of the file
```
deb https://dl.winehq.org/wine-builds/debian/ buster main
deb-src https://dl.winehq.org/wine-builds/debian/ buster main
deb https://download.opensuse.org/repositories/Emulators:/Wine:/Debian/Debian_10 ./
```
Next update your packages
```
sudo apt update
```
Install Dependencies
```
sudo apt install -y libc6 libgcc1 libglib2.0-0 libgphoto2-6 libgphoto2-port12 libgstreamer-plugins-base1.0-0 libgstreamer1.0-0 liblcms2-2 libldap-2.4-2 libmpg123-0 libopenal1 libpulse0 libudev1 libx11-6 libxext6 libxml2 zlib1g libasound2-plugins libncurses5 libcapi20-3 libcups2 libdbus-1-3 libfontconfig1 libfreetype6 libglu1-mesa libgnutls30 libgsm1 libgssapi-krb5-2 libgtk-3-0 libkrb5-3 libncurses5 libodbc1 libosmesa6 libpcap0.8 libpng16-16 libsane1 libsdl2-2.0-0 libtiff5 libv4l-0 libva-drm2 libva-x11-2 libva2 libvulkan1 libxcomposite1 libxcursor1 libxfixes3 libxi6 libxinerama1 libxrandr2 libxrender1 libxslt1.1 libxxf86vm1 gnutls-bin cabextract
sudo apt install -y libgnutls30:i386 libldap-2.4-2:i386 libgpg-error0:i386 libxml2:i386 libasound2-plugins:i386 libsdl2-2.0-0:i386 libfreetype6:i386 libdbus-1-3:i386 libsqlite3-0:i386
```
Install wine-staging
```
sudo apt install --install-recommends winehq-staging
```
### Arch Linux
For **Arch Linux** go and open your terminal. We will need to edit the pacman.conf file
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
sudo pacman -Sy giflib lib32-giflib libpng lib32-libpng libldap lib32-libldap gnutls lib32-gnutls mpg123 lib32-mpg123 openal lib32-openal v4l-utils lib32-v4l-utils libpulse lib32-libpulse libgpg-error lib32-libgpg-error alsa-plugins lib32-alsa-plugins alsa-lib lib32-alsa-lib libjpeg-turbo lib32-libjpeg-turbo sqlite lib32-sqlite libxcomposite lib32-libxcomposite libxinerama lib32-libgcrypt libgcrypt lib32-libxinerama ncurses lib32-ncurses opencl-icd-loader lib32-opencl-icd-loader libxslt lib32-libxslt libva lib32-libva gtk3 lib32-gtk3 gst-plugins-base-libs lib32-gst-plugins-base-libs vulkan-icd-loader lib32-vulkan-icd-loader cups samba alsa alsa-utils alsa-tools gnutls libpng wine-mono lib32-libxml2 lib32-mpg123 lib32-lcms2 lib32-giflib lib32-libpng lib32-gnutls pygtk python2-dbus lib32-libpulse lib32-fontconfig lib32-libxcomposite lib32-libxrender  lib32-libxslt lib32-gnutls lib32-libxi lib32-libxrandr lib32-libxinerama lib32-libcups lib32-freetype2 lib32-libpng lib32-openal python-pyopencl lib32-v4l-utils lib32-libxcursor lib32-mpg123 lib32-sdl xf86-video-intel lib32-mesa-libgl nss-mdns --needed --noconfirm
```
With that please reboot and proceed with the guide

## The guide part 1/2

Create the main directory (we will be using this directory for all the configurations and tweaks....may be different in your case)
```
sudo mkdir /home/usernamehere/Games/osu-wine-prefix
```
Create wine prefix
```
sudo WINEPREFIX=/home/usernamehere/Games/osu-wine-prefix/ WINEARCH=win32 winecfg
```
**NOTE: It's going to ask you to install wine-mono and wine-gecko, make sure to select no.**

Install winetricks
```
wget https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks
chmod +x winetricks
sudo mv -v winetricks /usr/local/bin
```
Install .NET Framework + japenese fonts and special characters
```
sudo WINEPREFIX=/home/usernamehere/Games/osu-wine-prefix/ WINEARCH=win32 winetricks -q --force dotnet472 cjkfonts gdiplus ie8 winhttp
```
Lower audio latency (set sound to alsa if using alsamixer or pulse if using pulseaudio)
```
sudo WINEPREFIX=/home/usernamehere/Games/osu-wine-prefix/ WINEARCH=win32 winetricks sound=alsa
cat > dsound.reg << "EOF"
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Wine\DirectSound]
"HelBuflen"="512"
"SndQueueMax"="3"
EOF
sudo WINEPREFIX=/home/usernamehere/Games/osu-wine-prefix/ WINEARCH=win32 wine regedit dsound.reg
```
## The guide part 2/2
Create osu directory where osu! is going to be installed
```
sudo mkdir /home/usernamehere/Games/osu-wine-prefix/drive_c/users/root/Application\ Data/osu\!
```
If you don't want to use the installer download the preinstalled osu! folder (http://www.mediafire.com/file/omcxq8r13uyqa2q/osu.zip/file) and extract it into /home/usernamehere/Games/osu-wine-prefix/drive_c/users/root/Application\ Data/osu\!

If you want to use the installer follow these steps

Download the Installer https://m1.ppy.sh/r/osu!install.exe

Copy it to your newly created folder
```
sudo cp -r ~/Downloads/osu\!install.exe /home/usernamehere/Games/osu-wine-prefix/drive_c/users/root/Application\ Data/osu\!
```

Install dxvk 1.7.1

Download https://github.com/doitsujin/dxvk/releases/download/v1.7.1/dxvk-1.7.1.tar.gz

Extract dxvk-1.7.1.tar.gz and copy its contents
```
sudo cp -r dxvk-1.7.1/x64/* /home/usernamehere/Games/osu-wine-prefix/drive_c/windows/system32/
sudo cp -r dxvk-1.7.1/x32/* /home/usernamehere/Games/osu-wine-prefix/drive_c/windows/system/
```
Open winecfg
```
sudo WINEPREFIX=/home/usernamehere/Games/osu-wine-prefix/ WINEARCH=win32 winecfg
- Set Windows version to Windows 7 if not done already
- Go to libraries and add
>>d3d10
>>d3d10_1
>>d3d10core
>>d3d11
>>d3d12
>>d3d8
>>d3d9
>>dxgi
- Set them to native and click apply
- Next go to graphics tick "Allow the window manager to control the windows"
- Next go to Staging and tick the first four
```
Next install DirectX

To do so download the DirectX installer
https://www.microsoft.com/en-us/download/details.aspx?id=8109

Then launch it using the following command
```
sudo WINEPREFIX=/home/usernamehere/Games/osu-wine-prefix/ WINEARCH=win32 wine ~/Downloads/directx_Jun2010_redist.exe
```
It will aske where to install. Pick C:\

Once done launch the installer
```
sudo WINEPREFIX=/home/usernamehere/Games/osu-wine-prefix/ WINEARCH=win32 wine /home/usernamehere/Games/osu-wine-prefix/drive_c/DXSETUP.exe
```

Once finished with the install continue.

Create a custom terminal command for launching osu!
```
cat > osu << "EOF"
#!/bin/sh
export BROWSER='/opt/brave.com/brave/brave-browser'
export STAGING_AUDIO_DURATION=2000
export WINEPREFIX=/home/usernamehere/Games/osu-wine-prefix/
export WINEARCH=win32
wine /home/usernamehere/Games/osu-wine-prefix/drive_c/users/root/Application\ Data/osu\!/osu\!.exe
EOF
```
Create a custom terminal command for killing osu!
```
cat > osukill << "EOF"
#!/bin/sh
export WINEPREFIX=/home/usernamehere/Games/osu-wine-prefix/
export WINEARCH=win32
sudo wineserver -k
sudo killall -9 /opt/wine-staging/bin/*
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
sudo WINEPREFIX=/home/usernamehere/Games/osu-wine-prefix/ WINEARCH=win32 wine /home/usernamehere/Games/osu-wine-prefix/drive_c/users/root/Application\ Data/osu\!/osu\!install.exe
```

After that make sure to run osu with
```
sudo osu
```
**YOU HAVE TO DO THIS STEP ONLY ONCE**

Once open make sure to let it cache for really low latency (latency decreases as you play)

To speed it up pick any really long map and enable HR,NC and AT

Let osu play the map till the end and let the leaderboards and all of the ui element enabled

Once done you can play with low latency.

I would recommend closing osu normally and then kill wine by 
```
sudo osukill
```
### THE FINAL STEP
I would recommend to change "HeightFullscreen" and "WidthFullscreen" to a resolution you like playing at.
These work for me:
```
HeightFullscreen = 768
WidthFullscreen = 1366
```
And change the same for "Height" and "Width"
```
Height = 768
Width = 1366
```
For the last step open osu to ensure that the changes applied succesfully.

## EXTRA
If you want to push latency lower I would recommend overclocking yourn NVIDIA GPU by using the guide below.
https://github.com/BeanGreen247/Linux_NVIDIA_GPU_Overclocking_Guide

## Troubleshooting
If you get any error related to Windows, dlls missing or exes missing, just remove the prefix folder and start again
```
sudo rm -rf /home/usernamehere/Games/osu-wine-prefix
```
If the game refuses to run make sure to test if X11 allows you to open any app as root. 

To do so run
```
sudo gedit
```
OR
```
sudo mousepad
```
To fix run
```
xhost +si:localuser:root
```
If you get this error
```
X Error of failed request:  BadWindow (invalid Window parameter)
  Major opcode of failed request:  10 (X_UnmapWindow)
  Resource id in failed request:  0x1a00001
  Serial number of failed request:  293
  Current serial number in output stream:  296
```
Make sure to run the game in virtual desktop
