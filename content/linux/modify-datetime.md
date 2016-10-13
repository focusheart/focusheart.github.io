Title: Modify timezone and datetime on Ubuntu
Date: 2016-10-12
Category: development
tags: shell

Some configurations commands for Ubuntu server:

    $ tzselect
    
    # Since I'm in China, I select the Asia -> China -> Beijing Time
    # The TZ='Asia/Shanghai'
    
    # Update the localtime profile
    $ sudo cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
    
    # Update datetime by time center
    $ sudo ntpdate asia.pool.ntp.org
    
    # Update the hardware time
    $ sudo hwclock --systohc
    
Done!