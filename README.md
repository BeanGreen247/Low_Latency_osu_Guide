# Low latency osu! guide
## The requirements

Install wine-staging. To do so follow the guide
```
https://wiki.winehq.org/Download
```
### Debian 10
For **Debian 10** run these commands first
```
sudo dpkg --add-architecture i386 
wget -nc https://dl.winehq.org/wine-builds/winehq.key
wget -nc https://download.opensuse.org/repositories/Emulators:/Wine:/Debian/Debian_10/Release.key
sudo apt-key add winehq.key
sudo apt-key add Release.key
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
sudo pacman -Sy giflib lib32-giflib libpng lib32-libpng libldap lib32-libldap gnutls lib32-gnutls mpg123 lib32-mpg123 openal lib32-openal v4l-utils lib32-v4l-utils libpulse lib32-libpulse libgpg-error lib32-libgpg-error alsa-plugins lib32-alsa-plugins alsa-lib lib32-alsa-lib libjpeg-turbo lib32-libjpeg-turbo sqlite lib32-sqlite libxcomposite lib32-libxcomposite libxinerama lib32-libgcrypt libgcrypt lib32-libxinerama ncurses lib32-ncurses opencl-icd-loader lib32-opencl-icd-loader libxslt lib32-libxslt libva lib32-libva gtk3 lib32-gtk3 gst-plugins-base-libs lib32-gst-plugins-base-libs vulkan-icd-loader lib32-vulkan-icd-loader cups samba alsa alsa-utils alsa-tools gnutls libpng wine-mono lib32-libxml2 lib32-mpg123 lib32-lcms2 lib32-giflib lib32-libpng lib32-gnutls pygtk python2-dbus lib32-libpulse lib32-fontconfig lib32-libxcomposite lib32-libxrender  lib32-libxslt lib32-gnutls lib32-libxi lib32-libxrandr lib32-libxinerama lib32-libcups lib32-freetype2 lib32-libpng lib32-openal python-pyopencl lib32-v4l-utils lib32-libxcursor lib32-mpg123 lib32-sdl xf86-video-intel lib32-mesa-libgl          nss-mdns --needed --noconfirm
```
With that please reboot and proceed with the guide

## The guide part 1/2

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
Install .NET Framework + japenese fonts and special characters
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/ WINEARCH=win32 winetricks -q --force dotnet472 cjkfonts gdiplus ie8 winhttp
```
Lower audio latency
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/ WINEARCH=win32 winetricks sound=alsa
cat > dsound.reg << "EOF"
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Wine\DirectSound]
"HelBuflen"="128"
"SndQueueMax"="1"
EOF
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/ WINEARCH=win32 wine regedit dsound.reg
```
## Audio Latency
### Results of my Progress
It is now time for the results. Testing was done with the patch I described along with a few common PulseAudio configuration adjustments (that I’ll give later in the article).

My testing method to measure audio latency is the following:

* edit the osu!.<user>.cfg file and set CustomFrameLimit to an unrealistically high number (such as 10000) to achieve the highest possible refresh rate on main menu
* start a microphone recording on Audacity
* set the system, osu! global and music volume to 100% (or whatever is high enough to hear music decently in your mic)
* set osu! effects volume to 0%
* put the microphone in between the speakers and the mouse
* pause/unpause the music and measure the audio latency by calculating the difference between when you hear the click and the music pausing/unpausing.

Let’s begin with Windows 10.
I’m using the default Microsoft drivers for my motherboard’s built-in sound card with all enhancements disabled.

This is about 74ms of latency! This is high. Unfortunately, the Windows audio subsystem is hardly configurable so the only way to optimize latency is probably either in osu! or the audio drivers.

We can surely do better…

Let’s see what osu! on Wine Staging 4.14 (without my patch) gives.

Unfortunately, despite tweaks to my PulseAudio install, we’re getting 104ms of latency. This is way higher than what we experience on Windows, and unplayable to my standards.
The most common and recent workaround for this was to use the Wine ALSA drivers instead (which works on PulseAudio using default configuration), which somehow provide lower latency despite being theoretically less optimal.

Time for my patch to get in the way.

Now we’re talking: 15ms! This is the result I get with my patch and STAGING_AUDIO_DURATION set to 5000 and the PulseAudio settings given below. I’m sure I could actually lower it down to 10ms (with more extreme PulseAudio settings; I can also set STAGING_AUDIO_DURATION as low as 2000). But this is stable and I don’t really need to push it down further; I’m very happy with this right now!

In order to get the lowest latency possible, we will need to lower wine's audio latency.

To do so we will be required to replace the 32-bit and 64-bit version of the winepulse.drv.so file.
```
sudo wget -O /opt/wine-staging/lib/wine/winepulse.drv.so https://github.com/BeanGreen247/Low_Latency_osu_Guide/blob/master/winepulse.drv.so?raw=true
sudo wget -O /opt/wine-staging/lib64/wine/winepulse.drv.so https://github.com/BeanGreen247/Low_Latency_osu_Guide/blob/master/winepulse-64.drv.so?raw=true
```
## The guide part 2/2
Create osu directory where osu! is going to be installed
```
sudo mkdir /mnt/84C2FF4EC2FF42CA/osu-wine-prefix/drive_c/users/root/Application\ Data/osu\!
```
If you don't want to use the installer download the preinstalled osu! folder (http://www.mediafire.com/file/omcxq8r13uyqa2q/osu.zip/file) and extract it into /mnt/84C2FF4EC2FF42CA/osu-wine-prefix/drive_c/users/root/Application\ Data/osu\!

If you want to use the installer follow these steps

Download the Installer https://m1.ppy.sh/r/osu!install.exe

Copy it to your newly created folder
```
sudo cp -r ~/Downloads/osu\!install.exe /mnt/84C2FF4EC2FF42CA/osu-wine-prefix/drive_c/users/root/Application\ Data/osu\!
```

Install dxvk 1.3.2

Download https://github.com/doitsujin/dxvk/releases/download/v1.3.2/dxvk-1.3.2.tar.gz

Extract dxvk-1.3.2.tar.gz to your home folder and copy using the following command
```
sudo cp -r dxvk-1.3.2/x32/* /mnt/84C2FF4EC2FF42CA/osu-wine-prefix/drive_c/windows/system32/
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
- Next go to Staging and tick Hide Wine version from applications
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
export STAGING_AUDIO_DURATION=2000 #recommend starting at 5000 and work your way down till osu starts crashing
export WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/
export WINEARCH=win32
wine /mnt/84C2FF4EC2FF42CA/osu-wine-prefix/drive_c/users/root/Application\ Data/osu\!/osu\!.exe
EOF
```
Create a custom terminal command for killing osu!
```
cat > osukill << "EOF"
#!/bin/sh
export WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/
export WINEARCH=win32
wineserver -k
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
WidthFullscreen = 1024
```
And change the same for "Height" and "Width"
```
Height = 768
Width = 1024
```
For the last step open osu to ensure that the changes applied succesfully.

## EXTRA
If you want to push latency lower I would recommend overclocking yourn NVIDIA GPU by using the guide below.
https://github.com/BeanGreen247/Linux_NVIDIA_GPU_Overclocking_Guide

## Troubleshooting
If you get rundll32 error run
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/ WINEARCH=win32 winetricks -q --force dotnet472
```
If that did not work try
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/osu-wine-prefix/ WINEARCH=win32 wine cmd
sfc /scannow
```
