# install-pxe-server
setup a Raspberry Pi as an PXE-Server.



## requirements
### hardware:
Raspberry Pi (with LAN)
SD card (big enough to hold entire ISO images of desired Live DVDs)
USB-Stick (for preloaded iso images)
working network environment
### software
Raspbian Jessie (2016-02-09, https://www.raspberrypi.org/downloads/raspbian/)

## installation:
assuming, your Raspberry Pi is running Raspbian Jessie (2016-02-09),
and has a proper connection to the internet via LAN.
and your SD card can hold all the iso images,
and you have plugged an USB-memory-stick that has the has a label **PXE-Server**
and the folowing folder structure
```
/tftp
/tftp/iso
```

optional for win-pe pxe boot
```
/tftp/boot
/tftp/sources
```

1. run **bash install-pxe-server_pass1.sh** to install necessary packages
2. reboot your RPi
3. run **bash install-pxe-server_pass2.sh** to copy/download iso images of LiveDVDs, mount and export them and setup PXE menu according installed images.
4. reboot your RPi

done.
## note:
the script will copy/download/mount following ISOs:
```
win-pe-x86.iso        # Microsoft Windows PE, can not be downloaded, you have to create by yourself
ubuntu-x64.iso        # Ubuntu
ubuntu-x86.iso
ubuntu-lts-x64.iso    # Ubuntu LTS
ubuntu-lts-x86.iso
ubuntu-nopae.iso      # an old Ubuntu with non-PAE for old PCs
debian-x64.iso        # Debian
debian-x86.iso
gnuradio-x64.iso      # GNU Radio
deft-x64.iso          # DEFT
kali-x64.iso          # Kali Linux
pentoo-x64.iso        # Pentoo Linux
systemrescue-x86.iso  # System Rescue
bankix-x86.iso        # c't Bankix
desinfect-x86.iso     # c't desinfect, is not downloadable, you have to get by yourself
```

those url files will contain the url of the iso image, where to download, to compare if you have the requested iso already downloaded, to prevent downloading an iso newly, when it is done already.
```
win-pe-x86.url
ubuntu-x64.url
ubuntu-x86.url
ubuntu-lts-x64.url
ubuntu-lts-x86.url
ubuntu-nopae.url
debian-x64.url
debian-x86.url
gnuradio-x64.url
deft-x64.url
kali-x64.url
pentoo-x64.url
systemrescue-x86.url
bankix-x86.url
desinfect-x86.url
```