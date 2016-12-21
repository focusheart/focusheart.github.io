Title: Amazing SSH Tunnel!
Date: 2016-12-21
Category: Development

I have been using ssh to connect to my servers for a long time.
A typical scene is ssh to a gateway machine and then ssh to another machine in ethernet.
Then, vim this, vim that, scp this, scp that, etc.
I have been used to this sort of thing, although it was really boring.
Can I make it easier? The answer is yes! I found a powerful tool! **SSH Tunnel**!

On Windows OS, putty can provide all needed tools: (download here)[http://www.putty.org/]

Let's begin with the following scene:

    +---------------+                 +-------------------+
    |  my_notebook  |     internet    |  Gateway_Server   |
    |  on internet  | ===============>|  pub:202.117.0.1  |
    +---------------+                 |  eth:192.168.1.2  |
                                      +-------------------+
                                               ||
                                               ||  ethernet
                                          +----||----+
                                          | ethernet |
                                          |  Switch  |
                                          +--||--||--+
                                             ||  ||  ethernet
                                  ++=========++  ++=========++
                                  ||                        ||
                          +------------------+     +-------------------+
                          |  Inner_Server_A  |     |   Inner_Server_B  |
                          |  192.168.1.123   |     |   192.168.1.166   |
                          +------------------+     +-------------------+

In order to do jobs on `Inner_Server_A`, I must do this on `my_notebook`:

    open Xshell (A ssh client on Windows OS)
    ssh user@202.117.0.1
    # logined to **Gateway_Server**
    ssh user@192.168.1.123

If I want to copy some files out of `Inner_Server_A`,
I have to take `Gateway_Server` as a trasit port.
`scp` files to `Gateway_Server`, then `scp` files to `my_notebook`.
It's fine if just doing this for a few times, when you have to do this often,
it's boring and I hate this!

Now, I have `SSH Tunnel`! It's quite simple!
First, prepare follwoing software if on Windows (I have tested on Win 10):

    (DokanInstall_0.6.0.exe)[https://docs.google.com/file/d/0B4VuXMcT-V28elkzejN0aDNPRFk/view]
    (win-sshfs-0.0.1.5-setup.exe)[https://github.com/apaka/win-sshfs] or (Google Code Archive)[https://code.google.com/p/win-sshfs/]
    (plink.exe)[http://www.putty.org/]

First, create a tunnel to `Inner_Server_A` on `my_notebook`:

    plink.exe -N -L 127.0.0.1:22123:192.168.1.123:22 user@202.117.0.1

Then, `plink` will create a tunnel between `my_notebook:22123` and `Inner_Server_A:22`.
on `my_notebook`, use Xshell or putty connect to `localhost:22123`, you will find yourself on `Inner_Server_A`!

Next, in order to copy files easier(Xshell provides `xftp` which is fine),
we want to setup `SSHFS` between our `my_notebook` and `Inner_Server_A`.

If `my_notebook` runs a Linux distro, it is very easy to setup `SSHFS`.
For example, on Ubunutu, once you have installed the `sshfs`, simply run:

    $ sshfs -o allow_other,transform_symlinks user@remote_ip:/path /localpath

Then, you get remote path mounted on your `/localpath`.

If `my_notebook` runs a Windows OS, it is not as easy as on Linux.
You should install `Dokan` and `win-sshfs`, then follow the GUI.
`win-sshfs` will guide you mount a remote machine to your local drive(E/F/G/H/.../X/Y/Z).

Have a try!
