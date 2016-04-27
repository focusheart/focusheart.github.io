Title: Access mine home machine from outside by SSH tunnel!
Date: 2016-04-16 10:25
Category: development

Global IP address is limited resource in my environment.
I have some machine in my office and home which has no global IP.
If I want to access these inner machines from outside, it is quite diffcult.
Now, thanks to lots of programmers, I known how to achieve that~
Let's explain the process:

It's quite simple! There are one thing you must prepare: a machine with global IP anyway.
You can buy a VPS on Amazon EC2 or DigitalOcean or Alibaba, or any other provider.
Assume this machine is G and its IP is gg.gg.gg.gg.

My inner machine in workplace is W, which installed Ubuntu OS.
My notebook is N, I will take it when outside. 

Let's begin config!

First, generate a public key on **W**, you can simplely just press Enter and Enter again to generate a RSA key pair:

    $ ssh-keygen
    
Second, put this keyfile `id_rsa.pub` to **G**:

    $ scp ~/.ssh/id_rsa.pub user@gg.gg.gg.gg:/home/user/w_key.pub
    
Then, login to **G** and import this public key:

    $ cat ~/w_key.pub >> ~/.ssh/authorized_keys
    
Then, your machine **W** can login into **G** without typing password, it is important for the following steps.

Next, we create a piece of scripts to create ssh reverse tunnel:

    $ vim keep_ssh_tunnel.sh
    
    #!/bin/sh
    RET=`ps ax | grep "10000:localhost:22" | grep -v "grep"`
    if [ "$RET" = "" ]; then
        echo "The recent SSH failure occured at `date`" >> ~/ssh_tunnel.log
        echo "Restart reverse SSH at `date`!"
        ssh -f -R 10000:localhost:22 user@gg.gg.gg.gg "vmstat 30"
    fi;

Let's explain the above script. 
`RET` is to check whether the ssh process is running.
If ssh is running, `RET` must be a non-blank string, otherwise it is blank.
If `RET` is blank, we start a new ssh process, creating reverse tunnel and execute a command `vmstat` as frequency of one time per 30 seconds. This tunnel bind port 10000 of **G** to port 22 of **W**.

Now, we make a crontab plan to check every 3 minutes to execute the shell script.

    $ crontab -e
    */3 * * * * /path_to_script/keep_ssh_tunnel.sh

That's all preparation work. Now, you can access **W** from **N**:

    $ ssh user@gg.gg.gg.gg
    
On **G**, execute:

    $ ssh user_of_w@localhost -p 10000

Done! Then if I want to access **W**, it is simple, just do the last two lines.