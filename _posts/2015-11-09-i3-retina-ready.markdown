---
layout: post
title:  "i3wm Retina-ready"
date:   2015-11-21 16:32
categories: linux apple macbook pro retina i3wm tiling wm tiling-wm
---

Yesterday I've installed i3wm-gaps just for fun, knowing that last time I tried it on my Retina MacBook Pro ti was completely unusable due to the Retina display.

This is changing today.

Since both GTK+ 3 and QT 5 now have good HiDPI support, setting `Xft.dpi: 192` into the `.Xresources` file makes sure all applications written using these two frameworks will scale properly on the display.

You have to set the i3 font to exactly 2 times your desidered size, for example "Droid Sans 10" will become "Droid Sans 20".

[Here](https://github.com/gsora/i3-retina) you can find my i3 configuration for this machine.

![](http://i.imgur.com/YZ6KtEf.png)
