Title: Install MySQLdb in virtualenv on Ubuntu
Date: 2016-10-12
Category: development
tags: shell

I have install the python-mysqldb on my Ubuntu server,
but a missing dependency message alerted when I run a program in virtualenv.

Then I do as [this post](http://www.saltycrane.com/blog/2010/02/install-mysqldb-virtualenv-ubuntu-karmic/) and solved my problem:

    $ sudo apt-get build-dep python-mysqldb
    $ source venv/bin/activate
    (venv)$ pip install MySQL-python
    
then everything works fine.