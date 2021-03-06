# install-pxe-server
setup a Raspberry Pi as an PXE-Server.

it is a private project i have made for myself.

i did not keeped an eye on network security.

USE IT AT YOUR OWN RISK.

## what is it good for?
the scripts installs necessary packages to let your RPi act as a DHCP, TFTP, Samba, NFS, PXE server.
and it will download LiveDVD ISOs you can boot your PXE client (Desktop PC) to.

the script can easely be modified to add additional ISOs or update ISOs if updated ones are available.

it also is able to act as server for NETWORK BOOTING for a Raspberry Pi 3 (see **note4**)

### overview schematic:
```
      ╔══════════╗   ╔═══╗       ╔══════╗╔═════════╗
WAN───╢DSL router╟───╢ s ║       ║PXE-  ╠╣USB-stick║
      ╚══════════╝   ║ w ║       ║server║╚═════════╝
                     ║ i ║       ║      ║
       ╔══════╗      ║ t ╟───eth0╢      ║
       ║ RPi3 ╟──────╢ c ║       ║      ║
       ╚══════╝   ┌──╢ h ╟──┐    ║      ║
                  │  ╚═══╝  │    ╚══════╝
               ╔══╧══╗   ╔══╧══╗
               ║ PC1 ║   ║ PC2 ║
               ╚═════╝   ╚═════╝
```

## requirements
### hardware:
- Raspberry Pi (with LAN)
- SD card (big enough to hold entire ISO images of desired Live DVDs), (e.g. 64GByte)
- USB memory stick (for preloaded iso images), (e.g. 64GByte)
- working network environment with a connection to internet

optional, if your SD card is too small or you dont want to have all the server content on the SD card, you can use the USB memory stick to hold all content. for that you have to do small tiny changes on the scripts.

### software:
- **Raspbian Jessie** or **Raspbina Jessie Lite** (2017-07-05), https://www.raspberrypi.org/downloads/raspbian/)

## installation:
assuming,
- your Raspberry Pi is running Raspbian Jessie (or Lite) from 2017-07-05,
- and has a proper connection to the internet via LAN.
- and your SD card can hold all the iso images,
- and you have plugged an USB-memory-stick that has the has a label **PXE-Server**
- and the folowing folder structure on the USB memory stick:
```
/tftp
/tftp/iso
```

optional structure for win-pe pxe boot
```
/tftp/bootmgr.exe
/tftp/boot/
/tftp/efi/
```

1. run `bash install-pxe-server_pass1.sh` to install necessary packages
2. reboot your RPi with `sudo reboot`
3. run `bash install-pxe-server_pass2.sh` to copy/download iso images of LiveDVDs, mount and export them and setup PXE menu according installed images.
4. reboot your RPi with `sudo reboot`

done.

## update:
to update your images, update the url in the **install-pxe-server_pass2.sh** file and re-run `bash install-pxe-server_pass2.sh`.
this will download all updated iso files.

## note:
the script will copy/download/mount following ISOs:
```
win-pe-x86.iso        # Microsoft Windows PE, can not be downloaded, you have to create by yourself
ubuntu-lts-x64.iso    # Ubuntu LTS
ubuntu-lts-x86.iso
ubuntu-x64.iso        # Ubuntu
ubuntu-x86.iso
ubuntu-nopae.iso      # an old Ubuntu with non-PAE for old PCs
debian-x64.iso        # Debian
debian-x86.iso
gnuradio-x64.iso      # GNU Radio
deft-x64.iso          # DEFT
kali-x64.iso          # Kali Linux
pentoo-x64.iso        # Pentoo Linux
systemrescue-x86.iso  # System Rescue
tails-x64             # Tails (not working yet)
bankix-x86.iso        # c't Bankix, is not downloadable anymore, you have to get by yourself
desinfect-x86.iso     # c't desinfect, is not downloadable, you have to get by yourself
rpdesktop-x86.iso     # Raspberry Pi Desktop for x86 PC
```

the following url files will contain the url of the iso image, where to download, to compare if you have the requested iso already downloaded, to prevent downloading an iso newly, when it is done already.
```
win-pe-x86.url
ubuntu-lts-x64.url
ubuntu-lts-x86.url
ubuntu-x64.url
ubuntu-x86.url
ubuntu-nopae.url
debian-x64.url
debian-x86.url
gnuradio-x64.url
deft-x64.url
kali-x64.url
pentoo-x64.url
systemrescue-x86.url
tails-x64.url
bankix-x86.url
desinfect-x86.url
rpdesktop-x86.url
```
## note2:
some of the PXE-menu entries has additional parameters, that lets the Live systems boot with german language (keyboard layout).
if you dont like or want, remove those additional parameters just behind the ' --' in the menu entries

## note3:
it is prepared for BIOS, UEFI 32bit and UEFI 64bit boot, but UEFI is not tested yet, because of lack of hardware for UEFI boot

## note4: NETWORK BOOTING for Raspberry Pi 3
the server is prepared for to boot a Raspberry Pi 3 via network.
but you have to do some additional work beside of the installation script.
you have to add a bootcode.bin to the tftp folder and other files/folders.
as template, the script will copy the PXE-Servers ```/boot``` files to ```/srv/tftp/rpi-boot``` and creates a ```/srv/tftp/rpi-boot/cmdline.txt``` for network booting.

in the script ```install-pxe-server_pass2.sh```, there is a ```RPI_SN=12345678``` line, change the ```12345678``` to the serial number of the RPi3, that will boot from network. if you have more than one RPi3 for network booting you have to add them by hand to the ```/srv/tftp``` folder.

a folder ```/srv/tftp/nfs/rpi-client``` will be created, but keept emtpy. you have to fill that folder with the operating system for the RPi3.

by default, a RPi3 is not enabled for network booting. you have to enable it once.

for more information,

see: [Network Booting](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bootmodes/net.md)<br/>
see: [Network Boot Your Raspberry Pi](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bootmodes/net_tutorial.md)
