Title: Install Jupyter Environment
Date: 2016-10-10 22:54
Category: Development
Tags: python

In order to start my own work on data analysis into EDM, 
I need a development environment of Python Notebook.
The process is quite simple:

Install the `virtualenv` and related packages:

    $ sudo apt-get install python-virtualenv python-pip python-tk

Create a virtual environment for project:
    
    $ cd ~/workspace
    $ virtualenv edm
    $ source edm/bin/activate

Update the pip source to improve the download speed, follow the instruction of 
[TUNA of THU](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)
        
Update the `pip`:

    (edm)$ pip install --upgrade pip
    
Install the jupyter, follow [official instruction](https://jupyter.readthedocs.io/en/latest/install.html):

    (edm)$ pip install jupyter
    
Install some optional packages:

    (edm)$ pip install numpy scipy scikit-learn glances matplotlib requests flask pyt

Generate a jupyter config profile:

    (edm)$ jupyter notebook --generate-config
    (edm)$ vim ~/.jupyter/jupyter_notebook_config.py
    
We can change the settings of notebook, for example:

    c.NotebookApp.port = 8765
    c.NotebookApp.ip = '0.0.0.0'
    c.NotebookApp.open_browser = False
    c.NotebookApp.allow_origin = '*'
    
Start the notebook server under any folder:

    (edm)$ cd ~/workspace
    (edm)$ jupyter notebook

    
## One more thing

In my case, the jupyter notebook server is located in inner network, 
which is not accessible from my laptop and desktop.
I setup a nginx proxy for this purpose then.

On the proxy gateway machine A, write a configuration file.
Basicly, this file specify the domain name and reverse proxy host,
and the most important config is the proxy for WebSocket:

    $ cd /etc/nginx/site-available
    $ sudo vim 009-jupyter-proxy.conf
    
    # Add the following content, any server_name is OK:
    
    server {
        listen 80;
        server_name jupyter.my.com;

        location / {
            proxy_redirect off;
            proxy_pass http://192.168.1.188:8765;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
        }
    }

Link the configuration file and restart Nginx:
    
    $ cd /etc/nginx/site-enabled
    $ sudo ln -s ../site-available/009-jupyter-proxy.conf ./
    $ sudo /etc/init.d/nginx restart

Because I configured a domain name for the reverse proxy,
I must add this domain name to DNS server or local DNS parse file on my laptop:

    $ sudo vim /etc/hosts
    
    # Add the following:
    IP-of-machine-A        jupyter.my.com
    
Then I can access this jupyter notebook on my laptop:

    $ lynx IP-of-machine-A
    # or use Firefox, Chrome, etc.