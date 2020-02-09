# Bridge-Pi Guide

###### A guide for creating a wireless bridge with a Raspberry Pi B+
---
Let's get started~

A Raspberry Pi wireless bridge is meant to connect devices that do not have a wireless card/adapter to a wireless network.

The set up guide is going to be focused on setting up the SD card for the Rasberry Pi with a Unix-Like system. 

I will be using Arch Linux. I'm sure many different Unix-Like OS's will work too.

I may expand this section in the future to give more detail for Windows in the future, but for now I recommend using Rufus for writing media on Windows.

---
1. Start by downloading your Raspberry Pi OS. Visit [here](https://www.raspberrypi.org/downloads/raspbian/) for the lastest Raspbian version.
   - I will be using "Raspbian Buster Desktop, Version: February 2020" for this installation.
---
2. Get your sd card + usb adapter together and plug them into your computer.
   - Make sure that you do not need any data off of said sd card.
---
3. You may or many not have autorun to connect your sd card automatically on your system, if you do; you need to unmount the sd card from its mounted directory using `umount`.
   - If you do not have autorun enabled then you do not need to worry. 
   - We need the card to be unmounted for this.
---
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
---
5. Remember location very will because if you mess up you could potentially erase your main system storage units.
   - Make sure you do not need any data off of the sd card. This is the last warning before we wipe the sd card.
---
6. Next do `fdisk /dev/sdb`
   - Press `m` to see a list of your options.
   - We are going to delete all data on the drive.
   - Press `d` and `enter` until all partitions on the drive are deleted.
   - Press `o` and `enter` to give a new DOS disklabel.
   - Press `n` to create a new partition.
   - Press `Enter` for a default Partition of 1.
   - Press `Enter` to give the first sector.
   - Press `Enter` to give the last sector.
   - It should complete with...
   ```
   Created a new partition 1 of type 'Linux' and of size 59.6 GiB.
   ```
   - ...and whatever size sd card you have.
   - MAKE SURE YOU DO NOT NEED ANY DATA OFF OF THE SD CARD.
   - Press `w` and `enter` to write table to disk and exit.
   ```
   The partition table has been altered.
   Calling ioctl() to re-read partition table.
   Syncing disks.
   ```
---   
7. Now we're going to give our fresh disk a filesystem with `mkfs`
   - Enter `mkfs.vfat /dev/sdb -I`
   - If it says... 
   ```
   mkfs.fat 4.1 (2017-01-24)
   attribute "partition" not found
   ```
   - It is not a worry.
---   
8. Next we're going to write the Raspbian Image to the sd card with `dd`.
   - Hopefully by now your Raspbian download is complete.
   - Go to the download location and uncompress the folder.
   - Once it is uncompressed you will need to go to the download location in your terminal or from your home directory it will look something like this...
   - `dd if=~/Downloads/2020-02-05-raspbian-buster/2020-02-05-raspbian-buster.img of=/dev/sdb bs=4M`
   - Depending on your system and/or size of the sd card this could take a moment.
---
9. Once `dd` completes it's task we can unplug the usb adapter from the computer and take the sd card and insert it into the Raspberry Pi.
   - Once the sd card is in, you can plug in the usb power adapter to start the Pi.
   - Ideally you will have a monitor to see if the installation is successful. A headless install is beyond the scope of this guide.
   - If you land on a screen that says something along the lines of... 
      - INSERT PICTURE HERE
      - INSERT PICTURE HERE
   - ...then you have messed something above up and need to start from 2. of this guide.
   - Or your image could have been damaged during download or uncompression.
   - If you did not land on the screens above then let the Pi do it's thing for a moment. Set up take some time.
   - You should land on this page if you are sucessful...
      - INSERT PICTURE HERE
---
10. Follow the `Welcome to Raspberry Pi` window directions to set up the lanauage, keyboard, locale, password, screen settings, wifi, and software update.
    - It is recommended you update your system to the latest version.
    - Once your system update is complete if the Pi asks you to restart the system then restart.
---
11. 
---
To be continued...
   
   
   
   
   
   
   
   
   
   
