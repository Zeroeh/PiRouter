# PiRouter
A guide for setting up a dual wifi access point/repeater system on a raspberry pi. Note that this guide is actually very specific about the way things are set up. If you don't need the exact setup as explained below, it will probably be easier to follow some other standard guide for working with an access point on a raspberry pi.

# The interfaces

- **eth0** -> Used as a local dhcp router so that whatever device is plugged into the ethernet will have internet capabilities which are provided as a simple iptables masquerade through wlan1. 

- **wlan0** -> The soldered antenna on the Pi 3b. This adapter will be used as the access point.

- **wlan1** -> This is an external wifi dongle, that connects to any access points specified in your wpa_supplicant file. Provides internet capabilities to wlan0 and eth0.

# Config 

