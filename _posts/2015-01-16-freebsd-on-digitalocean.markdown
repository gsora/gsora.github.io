---
layout: post
title:  "FreeBSD on DigitalOcean
date:   2015-01-16 17:13:05
categories: freebsd digitalocean vps
---

DigitalOcean has just added **FreeBSD** in the list of operating systems available on their droplets, so I immediately took this opportunity to learn more about this amazing UNIX OS and to finally ditch my old Ubuntu Server droplet: *too much* mess in that filesystem.

Unlike others OSes available on DigitalOcean, FreeBSD *forces* you to use SSH pubkeys to login.
If you haven't generated one, on your client machine execute:

    ssh-keygen -t rsa -b 4096 -C "$(whoami)@$(hostname)"

Your new key is located in `~/.ssh/` if you haven't changed any standard ssh config.

After the droplet creation, log into your new machine as the `freebsd` user.

The `freebsd` user is already configured to use `sudo` without asking for a password, since the SSH login is only possible via SSH keys.

In order to gain `su` login again,  we have to firstly set a password for the `root` and `freebsd` user

    $ sudo su
    # passwd
    # passwd freebsd

After this, I copied a *skel* file for **tcsh** in my home and then I installed `vim`.
    
    $ cp /usr/share/skel/dot.cshrc ~/.cshrc
    $ sudo pkg install vim

Now you can set `vim` as your default editor by editing line 22 in `~/.cshrc`.

Next, you surely want to edit `/etc/motd` and replace the standard message with a personalized one.

My advice is to take a look at the URLs written in the standard motd, FreeBSD Handbook and forums are a great source of knowledge.

Next, I installed PHP 5.6 with Apache 2.4 as my webserver.
The configuration procedure is always the same, the only thing that changes is the installation procedure:

    # pkg install apache24
    # pkg install php56
    # pkg install mod_php56

Then, add these lines to your `/etc/rc.conf` file to let Apache and PHP-FPM start at boot:

    apache24_enable="YES"
    php_fpm_enable="YES"

Here is it! 

Your new FreeBSD droplet is ready to rock, but first be sure to [secure it properly!](https://forums.freebsd.org/threads/unofficial-freebsd-security-checklist-links-resources.4108/)
