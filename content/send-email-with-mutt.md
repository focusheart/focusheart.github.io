title: mutt is a good email command!
date: 2016-11-14
category: development

# Mutt!

I have some thing stored on my office computer,
but it is not simple to access from outside.
The only way is using jump machine by `ssh`.
If I want to get some files out, 
I must send it to jump machine and then fetch it.

If the file is large, I can bear the inconvenience.
But if files are small and must be send a lot of times,
it really made me mad!

So, using a email client for sending small files as attachment is a good idea!
After searching, I found `mutt` is very good!

Simplly install:

    $ sudo apt install mutt msmtp
    
Simplly config:

    $ sudo vim /etc/Muttrc
    
    # add the following at the end of file
    set sendmail="/usr/bin/msmtp"
    set use_from=yes
    set realname="My Name"
    set from=xxx@163.com
    set envelope_from=yes
    
    $ vim ~/.msmtprc
    
    # add the following
    account default
    host smtp.163.com
    from xxx@163.com
    auth plain
    user xxx
    password 123456
    logfile ~/.msmtp.log
    
    $ chmod 600 ./msmtprc
    $ touch .msmtp.log

Then, send simple mail to `xxxx@qq.com`:

    $ echo "Mail Content!" | mutt -s "Mail Title!" xxxx@qq.com

Or, send mail with an attchment to `xxxx@qq.com` and cc to `yyy@qq.com`

    $ echo "Attach Mail" | mutt -s "Mail Title! Attached!" -a path-to-file -- xxxx@qq.com -c yyy@qq.com

Life become better now!