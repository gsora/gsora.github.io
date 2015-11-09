---
layout: post
title:  "MacBook Pro Retina touchpad on Fedora 23"
date:   2015-11-09 21:21
categories: fedora linux apple macbook pro retina
---

Fedora 23 is working great on my 13" MacBook Pro Retina but the Synaptics's touchpad driver is lacking gestures and palm/thumb recognize support.

To enable something that can be called "gestures", the `mtrack` driver can be installed and tweaked at our desire.

Since it's not available already build, I wrote a `spec` that will download and build the `rpm` for you

    $ wget https://goo.gl/89Fu0R
    $ rpmbuild -ba xorg-x11-drv-mtrack-git.spec
    $ rpm -ivh ~/rpmbuild/RPMS/xorg-x11-drv-mtrack*

Once done, uninstall the Synaptics driver:

    $ sudo dnf remove xorg-x11-drv-synaptics

[This](https://gist.github.com/32d6de025481698c9ea8) is my configuration, place it into `/etc/X11/xorg.conf.d/20-mtrack.conf`.

Happy scrolling!
