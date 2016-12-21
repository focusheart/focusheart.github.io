title: Install x11vnc on deepin Linux
date: 2016-11-26
category: development

First of all, the `deepin` distribution of Linux is very pretty!
I like the color scheme, icon design, control panel design, etc.
Since the main feature of `deepin` is desktop design,
I really want to use its desktop from my remote machines if bandwidth is fine.

I used to access my Linux machine by basic `vnc`,
which creates a very simple desktop session, and different from the desktop.

In `deepin`, I've no idea how to create a session which is as the same looking.
After searching, I found the `x11vnc`, which was created in 2002,
but still works perfect! [home page](http://www.karlrunge.com/x11vnc/).

Install `x11vnc` on Debian-like system is very easy:

    $ sudo apt install x11vnc

Then, create a password:

    $ x11vnc -storepasswd

Start the server:

    $ x11vnc -display :0 -usepw -forever -bg

And then, I can enjoy the exactly same desktop on Windows by vncviewer.
