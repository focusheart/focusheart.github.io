Title: Setup a proxy for MySQL server on Ubuntu located in intranet
Date: 2016-10-14
Category: development

I have a MySQL server instance running in office's intranet,
which only can not be accessed from internet.
Meanwhile, I have a proxy server as a gateway connected to both networks,
and I therefore I could setup an MySQL proxy!

Fortunately, after searing on web, 
a program called `mysql-proxy` could solve my problem!
Also, it is included in Ubuntu's APT source.

Installation as the following steps:

    # install the program
    $ sudo apt-get install mysql-proxy
    
    # write a proxy config file
    $ vim config.conf
    
    # the content is my situation
    [mysql-proxy]  
    daemon = 1
    pid-file = /var/run/mysql-proxy.pid
    log-file = /var/log/mysql-proxy.log
    log-level = debug
    max-open-files = 4096
    plugins = admin,proxy
    user = root

    proxy-address = proxy-server-IP:4567

    proxy-backend-addresses = mysql-server-IP:3306
    admin-username = admin
    admin-password = admin
    admin-lua-script = /usr/lib/mysql-proxy/lua/admin.lua
    
    # run the proxy
    $ sudo mysql-proxy --defaults-file=config.conf
    
Then you may have a proxy running, listening port 4567.
If it doesn't work, check the logfile in `/var/log/mysql-proxy.log`.

When proxy server is ready, 
create an user which is accessible to MySQL server from proxy server.

    # on MySQL server, execute following command to add a user:
    mysql> grant all on dbname.* to username@proxy-server-IP identified by 'pw';
    
Finally, you can have access MySQL server from internet by a proxy!