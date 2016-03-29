Title: Write more codes!
Date: 2016-03-29 10:15
Category: Development

Finally, the VNCServer works for me on my Ubuntu 14.04.
As I experienced before in Ubuntu 12.04 and other dists, the VNCServer is a good choice to control computer remotely. However, it's changed since I installed Ubuntu 14.04. I can NOT get the "Desktop" when I use the same configurations. After searching, I found the solution!

Generally, install vnc4server first:
    
    # apt-get install vnc4server
    
Install more:

    # apt-get install gnome-panel gnome-settings-daemon metacity nautilus gnome-terminal
    
First start up, I prefer use `:1`:

    # vnc4server :1
    
and then kill it:

    # vnc4server -kill :1

update the xstartup:

    # vim ~/.vnc/xstartup
    
replace the content with the following:

    #!/bin/sh

    export XKL_XMODMAP_DISABLE=1
    unset SESSION_MANAGER
    unset DBUS_SESSION_BUS_ADDRESS

    [ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
    [ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
    xsetroot -solid grey
    vncconfig -iconic &

    gnome-panel &
    gnome-settings-daemon &
    metacity &
    nautilus &
    gnome-terminal & 

then, restart vnc4server:

    # vnc4server -geometry 1280x800 :1
    
use vncviewer connect to this server ip, `xxx.xxx.xxx.xxx:1`!