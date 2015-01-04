---
layout: post
title:  "Jolla USB tethering hacks"
date:   2015-01-04 22:40:37
categories: jolla sailfishos tethering
---

Run these commands in a terminal, or make a bash script if you often need them!

To turn off simply reboot your Jolla, of course the *Developer Mode must be activated before*.

### 3G connection —> USB tethering

    # echo 1 > /proc/sys/net/ipv4/ip_forward
    # /sbin/iptables -t nat -A POSTROUTING -o rmnet0 -j MASQUERADE
    # /sbin/iptables -A FORWARD -i rmnet0 -o rndis0 -m state --state \ RELATED,ESTABLISHED -j ACCEPT
    # /sbin/iptables -A FORWARD -i rndis0 -o rmnet0 -j ACCEPT

### WiFi connection —> USB tethering

It might seem strange but sometimes when I'm in class I need to route the WiFi to my MacBook via USB, specially during some awkward Linux setup.

    # echo 1 > /proc/sys/net/ipv4/ip_forward
    # /sbin/iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
    # /sbin/iptables -A FORWARD -i wlan0 -o rndis0 -m state --state \ RELATED,ESTABLISHED -j ACCEPT
    # /sbin/iptables -A FORWARD -i rndis0 -o wlan0 -j ACCEPT

After this, you'll need to set the new interface that popped out on your network manager with a static IP address, because by now Jolla **doesn't** ship with a DHCP server configured to serve addresses to `rndis0`.

Basically, you need to set an IP in the same area of the one reported under **Settings > System > Developer Mode > USB IP address**.

For example if your Jolla has `192.168.2.15` reported there, set it as the router, set the machine IP to `192.168.2.17`, set an appropriated subnet mask which in this case is `255.255.0.0` and set a DNS server like OpenDNS, `208.67.222$

If for some (even more) strange situation you'll need to do this on OSX, install the latest version of [HoRNDIS](http://joshuawise.com/horndis#available_versions), reboot and configure as explained before.
