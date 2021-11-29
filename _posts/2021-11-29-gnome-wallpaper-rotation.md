---
title:  "Rotating wallpapers in GNOME"
layout: post
author: Linus Probert
date:   2021-11-29 14:00
tags: cron gnome wallpaper
categories: 
comments: true
---

I recently found a nice stash of cool wallpapers (all starwars themed
naturally). Essentially just google `imgur starwars wallpapers` if you're
interested.  Now, I found my new stash of wallpapers very pleasing but I wasn't
about to sit and manually switch them when I felt like it. Hell no, this calls
for *automation*.

I started out with this:

```bash
#!/bin/sh

BG_DIR=/home/linus/Pictures/wallpapers
next=$(ls -1 $BG_DIR | shuf -n 1)
gsettings set org.gnome.desktop.background picture-uri "file://$BG_DIR/$next"
```

Nice. Worked like a charm. Now let's just stuff that in cron with a 10 minute
rotation and we should be golden. Like this:

```crontab
*/10 * * * * sh ~/bin/wallpaper_rotator.sh

```

Nope. Errors.

```log
((process:2356646): dconf-WARNING **: 13:51:01.194: failed to commit changes to dconf: Cannot autolaunch D-Bus without X11 $DISPLAY)
```

Turns out, gsettings needs some env variables to be set in order to know what
display to operate on etc.

This solution actually took me a bit to find so I thought I'd share it again to
help anyone in the future. Here is what you need to feed into cron:

```crontab
*/10 * * * * env DISPLAY=:0 DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus sh ~/bin/wallpaper_rotator.sh
```

Notice in particular the variables `DISPLAY` and `DBUS_SESSION_BUS_ADDRESS`.
Take special note of the uid present in the middle of the
`DBUS_SESSION_BUS_ADDRESS`. In my example it's 1000 which is my users id on
this computer. To find yours you can just `cat /etc/passwd`.

That's all.

// Liq
