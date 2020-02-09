# Bridge-Pi Guide

###### A guide for creating a wireless bridge with a Raspberry Pi B+
---
Let's get started~

A Raspberry Pi wireless bridge is meant to connect devices that do not have a wireless card/adapter to a wireless network.

The set up guide is going to be foucsed on setting up the SD card for the Rasberry Pi with a Unix-Like system. 

I will be using Arch Linux. I'm sure many different Unix-Like OS's will work too.

I may expand this section in the future to give more detail for Windows in the future, but for now I recommend using Rufus for writing media on Windows.

---
1. Start by downloading your Raspberry Pi OS [here](https://www.raspberrypi.org/downloads/raspbian/) for the lastest Raspbian version.
   - I will be using "Raspbian Buster Desktop, Version: February 2020" for this installation.
   
2. Get your sd card + usb adapter together and plug them into your computer.

3. You may or many not have autorun to connect your sd card automatically on your system, if you do; you need to unmount the sd card from its mounted directory using `umount`.
   - If you do not have autorun enabled then you do not need to worry. 
   - We need the card to be unmounted for this.

4. To check the location of the drive use `fdisk -l`.
   - Look for a device section starting with `Disk /dev/sd*`.
   - In my case it is `Disk /dev/sdb`
   
   ``` 
   Disk /dev/sdb: 59.64 GiB, 64021856256 bytes, 125042688 sectors
   Disk model: Storage Device  
   Units: sectors of 1 * 512 = 512 bytes
   Sector size (logical/physical): 512 bytes / 512 bytes
   I/O size (minimum/optimal): 512 bytes / 512 bytes
   Disklabel type: dos
   Disk identifier: 0x266982c5

   Device     Boot  Start       End   Sectors  Size Id Type
   /dev/sdb1         8192    532479    524288  256M  c W95 FAT32 (LBA)
   /dev/sdb2       532480 125042687 124510208 59.4G 83 Linux
   ```
