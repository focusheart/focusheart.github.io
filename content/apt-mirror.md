Title: You should have a local APT-mirror~
Date: 2016-03-30 10:25
Category: development, deep-learning

The network condition in our lab is very good, in general. 
However, the `apt-get` update is not that well.
Download speed often fluctuates in the daytime, maybe due to great amount of users.
The speed in late night is quite good, but I can't install softwares at that time.
Then, I found that I can install a mirror site of apt sources!
It will be very fast!

It's quite simple to achieve that~

First of all, you must install apt-mirror on your local machine as apt source.

    # apt-get install apt-mirror
    
Then update the config of apt-mirror to link to a mirror sites

    # vim /etc/apt/mirror.list
    
    set base_path /apt     # I mounted a disk on this path as storage
    set defaultarch amd64  # Only sync 64bit data, i386 is nouse for me
    set nthreads 20        # Set the threads number to speed up download
    set _tilde 0           # I forget what is this config
    
    # The mirror urls, I use tsinghua's apt mirror    
    deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty main multiverse restricted universe
    deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-backports main multiverse restricted universe
    deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-proposed main multiverse restricted universe
    deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-security main multiverse restricted universe
    deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-updates main multiverse restricted universe 
    
Then sync! This process will take much long time to finish, you can go to sleep

    # apt-mirror
    
Some posts from blogs say that you can config `crontab` to sync with apt server automatically.
I didn't config that because this is enough for me.
I can update the mirror when I want to.

Then update my Apache's config:

    # vim /etc/apache2/mods-available/alias.conf

Add the following configs in `<IfModule alias_module>`

    Alias /ubuntu /apt/mirror/mirrors.tuna.tsinghua.edu.cn/ubuntu
 
    <Directory "/apt/mirror/mirrors.tuna.tsinghua.edu.cn/ubuntu">
        Options Indexes FollowSymlinks
        AllowOverride None
        Require all granted
    </Directory>

Restart or reload config of apache2

    # /etc/init.d/apache2 reload
    
Then, the SERVER side is done! Left is config client machine.
    
Update the `sources.list` on client machine:

    # vim /etc/apt/sources.list
    
Replace the origin content with the following. 
Attention please replace `localhost` with your apt server IP or domain name

    deb [arch=amd64] http://localhost/ubuntu/ trusty main multiverse restricted universe
    deb [arch=amd64] http://localhost/ubuntu/ trusty-backports main multiverse restricted universe
    deb [arch=amd64] http://localhost/ubuntu/ trusty-proposed main multiverse restricted universe
    deb [arch=amd64] http://localhost/ubuntu/ trusty-security main multiverse restricted universe
    deb [arch=amd64] http://localhost/ubuntu/ trusty-updates main multiverse restricted universe

Then Update apt

    # apt-get update    