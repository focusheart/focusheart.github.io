Title: Ubuntu Tricks?
Date: 2016-01-14 17:06
Category: development

There are lots of tricks or tips when I use Linux. Most of time I use Ubuntu. Here, I want to record some experiences during my work on Ubuntu.

## NO password when sudo

It's not safe for a normal user to do root's matters. As result, there is `sudo`. But in some cases, the password is not necessary. For example, using ansible. We can cancel the verification by doing this:

    $ pkexec visudo

    # change the %sudo line into following:
    %sudo ALL=NOPASSWD: ALL

then exit and enjoy no password for `sudo xxx`.