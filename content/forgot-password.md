Title: Password!
Date: 2016-01-18 10:17
Category: Development
Tags: Ubuntu

## When I forgot password...

Password! I have lots of machines to manage, and passwords to remember. Therefore sometimes I can not remember a password when I login to a machine. There are some simple methods to remember, for example I can write the password on a notebook. Even though that, I still forgot. As a result, I have to reset the password! On linux (Ubuntu 14.04), it's simple:

    1. Reboot to GRUB and rescure, select root( drop to shell prompt)
    2. RE-mount filesystem to r/w mode: # mount -o remount,rw /
    3. Change the password: # password username
    
    
## When I'm lazy to type password

Typing `ssh user@hostname` and then password is quite normal in my daliy life. I think it may cost at least half a hour per day to type these long, complex, hard-remember password again and again. As said that lazy is good for programmer, I decide to change something! (Also because I installed Ansible to manage lots of machines) Using `ssh` without typing password! It's simple, too:

    0. login to control machine and type following commands to generate a key and give it to other machines
    1. $ ssh-keygen
    2. $ ssh-copy-id -i ~/.ssh/id_rsa.pub user@remote_machine
    
Type the password of `user@remote_machine` last time! Then, `ssh user@remote_machine` will be simple! 

## NO password when sudo

I've already write this in the previous post, but some changed in this one. It's not safe for a normal user to do root's matters. As result, there is `sudo`. But in some cases, the password is not necessary. For example, using ansible. We can cancel the verification by doing this:

    1. change sudo setting in normal user or root: `#/$ pkexec visudo`
    # change the %sudo line into following:
    %sudo ALL=NOPASSWD: ALL

then exit and enjoy no password for `sudo xxx`.