# Bridge-Pi Guide

#### A guide for creating a wireless bridge with a Raspberry Pi B+
---
##### WARNING: THIS DOCUMENT IS STILL BEING EDITED!
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
    - If you were not prompted to update your system you can enter the commands...
      - `sudo apt-get update`
      - `sudo apt-get upgrade`
    - Once your system update is complete if the Pi asks you to restart the system then restart.
---
11. Before we get too far ahead of ourselves, we should setup the wlan0 connection that we plan on using. If you have already setup your wireless connection then you can skip ahead to step 5. <-- CHANGE THIS
    - Otherwise open up the wpa_supplicant file by running the following command:
      - `sudo nano /etc/wpa_supplicant/wpa_supplicant.conf`
---
12. Within this file add the following, making sure you replace the ssid with the name of the network you want to connect to and replace the psk value with the password for that network.
    - ```
      network={
         ssid="networkname"
         psk="networkpassword"
      }
      ```
---
11. Now that we have completed the inital set up we are going to install some packages to get this bridge going.
    - `sudo apt-get install dnsmasq`
---
12. Before we get started with modifying dnsmasq’s configuration we will first make a backup of the original configuration by running the following command.
    - `sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig`
---
13. With the original configuration now backed up and moved out of the way we can now move on and create our new configuration file by typing the command below into the terminal.
    - `sudo nano /etc/dnsmasq.conf`
    - I prefer to use vim and tmux in my terminals but use what you are comfortable with.
---
14. Now that we have our new file created we want to add the lines below, these lines basically tell the dnsmasq package how to handle DNS and DHCP traffic.
    - ```
      interface=eth0       # Use interface eth0  
      listen-address=192.168.220.1   # Specify the address to listen on  
      bind-interfaces      # Bind to the interface
      server=8.8.8.8       # Use Google DNS  
      domain-needed        # Don't forward short names  
      bogus-priv           # Drop the non-routed address spaces.  
      dhcp-range=192.168.220.50,192.168.220.150,12h # IP range and lease time
      ```
---
15. We now need to configure the Raspberry Pi’s firewall so that it will forward all traffic from our eth0 connection over to our wlan0 connection. Before we do this we must first enable ipv4p IP Forwarding through the sysctl.conf configuration file, so let’s begin editing it with the following command:
    - `sudo nano /etc/sysctl.conf`
    - Now we can save and quit out of the file by pressing Ctrl+X then pressing Y and then Enter.
---
16. Within this file you need to find the following line, and remove the # from the beginning of it. 
    - `#net.ipv4.ip_forward=1`
    - `net.ipv4.ip_forward=1`
    - Now we can save and quit out of the file by pressing Ctrl+X then pressing Y and then Enter.
---
17. Now since we don’t want to have to wait until the next reboot before the configuration is loaded in, we can run the following command to enable it immediately.
    - `sudo sh -c "echo 1 > /proc/sys/net/ipv4/ip_forward"`
---
18. Now that IPv4 Forwarding is enabled we can reconfigure our firewall so that traffic is forwarded from our eth0 interface over to our wlan0 connection. Basically this means that anyone connecting to the ethernet will be able to utilize our wlan0 internet connection.

    - Run the following commands to add our new rules to the iptable:
       - `sudo iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE`
       - `sudo iptables -A FORWARD -i wlan0 -o eth0 -m state --state RELATED,ESTABLISHED -j ACCEPT`
       - `sudo iptables -A FORWARD -i eth0 -o wlan0 -j ACCEPT`
    - If you get errors when entering the above lines simply reboot the Pi using `sudo reboot`
---
19. Iptables are flushed on every boot of the Raspberry Pi so we will need to save our new rules somewhere so they are loaded back in on every boot. To save our new set of rules run the following command.
      - `sudo sh -c `iptables-save > /etc/iptables.ipv4.nat`
---
20. Now with our new rules safely saved somewhere we need to make this file be loaded back in on every reboot. The most simple way to handle this is to modify the rc.local file.
      - `sudo nano /etc/rc.local`
---
21. Now we are in this file, we need to add the line below. Make sure this line appears above exit 0. This line basically reads the settings out of our iptables.ipv4.nat file and loads them into the iptables.
      - Find `exit 0`
      - Add `iptables-restore < /etc/iptables.ipv4.nat` above it.
      - Now we can save and quit out of the file by pressing Ctrl+X then pressing Y and then Enter.
---
22. Finally all we need to do is start our dnsmasq service. To do this, all you need to do is run the following command:
      - `sudo service dnsmasq start`
---
23. Now you should finally have a fully operational Raspberry Pi WiFi Bridge.
      - You can ensure this is working by plugging any device into its Ethernet port 
      - The bridge should provide an internet connection to the device you plugged it into.
      - To ensure everything will run smoothly it’s best to try rebooting now. 
      - This will ensure that everything will successfully re-enable when the Raspberry Pi is started back up. 
      - Run the following command to reboot the Raspberry Pi.
         - `sudo reboot`    
---
###### fin.
   
   
   
   
   
   
   
   
   
   
