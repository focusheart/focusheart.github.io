Title: Reverse Proxy by Apache
Date: 2016-04-27 17:27
Category: development

I use Apache to proxy service located in private network.
But it's not easy to map all services which listened to different ports.
If a service use absolute path in it's web page, it is more diffcult.

Luckily, I found a simple solution by a modle called proxy_html!
It's simple on Ubuntu Server:

First, install this module:

    $ sudo apt-get install libapache2-mod-proxy-html
    
Second, enable this module and config in my config:

    $ vim /etc/apache/sites-enabled/000-defaults

Add the following in <VirtualHost> tag:
    
    ProxyHTMLExtended On
    ProxyPass /nn/ http://zstorage01:50070/
    <Location /nn/>
        ProxyPassReverse /
        ProxyHTMLURLMap http://zstorage01:50070/ /nn/
        SetOutputFilter proxy-html
        ProxyHTMLURLMap / /nn/
    </Location>

    ProxyPass /dn/ http://zstorage01:50075/
    <Location /dn/>
        ProxyPassReverse /
        ProxyHTMLURLMap http://zstorage01:50075/ /dn/
        SetOutputFilter proxy-html
        ProxyHTMLURLMap / /dn/
    </Location>
    
Finally, reload apache:

    $ sudo service apache2 reload
    
In the configs list above, I create two reverse proxies to map Hadoop's page.
The `/nn/` is NameNode, and we can access the `dfshealth.jsp` from proxy server's port 80, by `http://proxyserver/nn/.
The `/dn/` is DataNode, and also can be used to access DataNode's `browseDirectory.jsp`.

Then I can access the page of other ports from port 80 and use a relative path.