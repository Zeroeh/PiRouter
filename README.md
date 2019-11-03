# PiRouter
A guide for setting up a dual wifi access point/repeater system on a raspberry pi. Note that this guide is actually very specific about the way things are set up. (The setup is geared more towards offensive operations) If you don't need the exact setup as explained below, it will probably be easier to follow some other standard guide for working with an access point on a raspberry pi. This guide is mostly for my own personal reference so that I don't forget these steps in the future. :S

# The interfaces

- **eth0** -> Used as a local dhcp router so that whatever device is plugged into the ethernet will have internet capabilities which are provided as a simple iptables masquerade through wlan1. 

- **wlan0** -> The soldered antenna on the Pi 3b. This adapter will be used as the access point.

- **wlan1** -> This is an external wifi dongle, that connects to any access points specified in your wpa_supplicant file. Provides internet capabilities to wlan0 and eth0.

- **br0** -> *Not required*. I used this in the past to get internet through wlan0->eth0 where wlan0 was an AP. A bridge doesn't seem to be required for wlan0->wlan1 connections. Iptables rules are not set either. Not entirely certain *how* wlan0 gets internet. Need to look into this.

# Config 

- **/etc/rc.local** -> at the end but before ``exit 0``, add the following:
    ```
       ./root/route_scripts/setup.sh
       dmesg -D
       hostapd -B /etc/hostapd/hostapd.conf
    ```
- **/etc/network/interfaces** -> [See image](/Selection_842.png). Note that for whatever reason, wlan1 has to be defined *before* wlan0, otherwise the interfaces wont come up.

- **/etc/hostapd/hostapd.conf** -> [See image](/Selection_841.png)

- **/etc/dnsmasq.conf** -> [See image](/Selection_840.png)

- After editting the above files, you will need to tether the eth0 and wlan1 interfaces so that eth0 can get internet from wlan1.
To do this, see [this](https://github.com/Zeroeh/udp-mitm/blob/master/raspberrypi/setup.sh) script. Replace all instances of wlan0 with wlan1. Save the script to the same path as in rc.local (Or change the path to whatever you like).

- Reboot and verify that you can get internet through eth0. If you can't get internet, you'll have to search around on the internet on how to get it set up. There are lots of guides on this.

- Verify that the access point is working. Verify that a device is able to connect and get internet.

- Verify that wlan1 is connecting to your desired AP and getting internet.

- Verify that your ``ifconfig`` looks somewhat similar to [this](/Selection_843.png).

- That's about it. I could be missing a few things but as I said, this is more for personal reference rather than some easy to follow tutorial.
