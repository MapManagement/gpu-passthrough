# Mouse And File sharing
This file is not only about sharing a mouse between different machines (in my case the host and guest), it's also about
sharing a keyboard, files and some more things I'll get to know in the future. "Mouse Sharing" seemed like a good title
and therefore I decided to got with that. Following topics will be about different methods you can consider if you want
to use just one mouse and keyboard or whatever for your guest and host. [My ideas file](../../ideas.md) should've
already clarified some stuff but only in a general and not very detailed form.

## Barrier
### Overview
[Barrier](https://github.com/debauchee/barrier) is actually the software I'm running on my system. There are several
reasons for that:
- it's open source and therefore for free
- it supports Linux, MacOs and Windows (also FreeBSD and OpenBSD but these are not the OSes I'm running the most)
- it's a fork of the non-free software Synergy (basically before Synergy was commercialized)

The main purpose of Barrier is to share your keyboard and mouse between two machines very easily. Normally it's used
to share them between non-virtualized OSes but it obviously works with virtualized ones too. Since my monitors are side
by side it does not bother me at all and simplifies many tasks. Unfortunately, drag and drop does not work on Linux 
systems which means that I cannot share files using it. So I might need to find another way of doing so. Nonetheless,
I'm really happy about this open source software and can really recommend it so far.  
### Installation
Just download the corresponding program for your system, the link at the top should explain how to install Barrier on
different operating systems. If you installed them on your ``client`` and ``server``, as they call it, you can connect
them via IP-addresses. Of course, you can also configure where your displays are placed:  
![](/resources/barrier-configuration.png)


## Looking-Glass
### Overview
Also a very good piece of software but not the best for my case is [looking-glass](https://looking-glass.io/). It does not
support a mouse sharing as you would think of in general, it kinda lets you use your guest and host on one monitor and
I'm not talking about a single-gpu-passthrough. Instead of plugging in a second monitor or switching the source on one
monitor, you can use your guest in a window. You only need to configure the software, which is not that hard, and any
video input port. You can either plug it into the same monitor you're already using or you buy a dummy that will be 
recognized as a second monitor.
### Installation
THe installation is not too complex but also not as easy as installing Barrier for example. Therefore I decided to leave
you two helpful sources here. The first one is the [official wiki](https://looking-glass.io/wiki/Installation) which 
should cover all details. The seconds one is [a video](https://www.youtube.com/watch?v=wEhvQEyiOwI) by a person that
already made a bunch of videos about gpu-passthrough.


## Samba
### Overview
Samba is based on the so called SMB (**S**erver **M**essage **B**lock) protocol. It's open-source and allows you to create a file server on your system to share them with other computers within the network. If you create such a server and a client
successfully connects to it, it can easily access any file which are stored in the shared directory. To cut a long story
short, you have access to a non-local drive. Normally you use a dedicated server for this so all clients can share their own
files. In this case, you don't need an extra server since your host OS will run the Samba process to provide the communication
between the host and the guest. Thus file sharing is made very comfortable.
### Installation
Eventhough there are several tutorials to install and configure Samba, I want to include the whole process here as well.
First of all you need to install the needed package:
```
apt install samba
```
Since we're sharing files, we have to specify a directory which will be accessable by other machines. Simply just add a new
directory that provides enough capacity. For example:
```
mkdir /samba/pictures
```
Copy the exact path and paste it into the configuration file of Samba (``/etc/samba/smb.conf``) like this:
```
[pictures]
    comment = My Pictures
    path = /samba/pictures
    read only = no
    browsable = yes
```
Now you only need to restart the service. Consider to update your firewall rules. On an Ubuntu server for example, you need to
allow Samba traffic:
```
sudo service smbd restart
sudo ufw allow samba
```
If you want to, you can also secure it with a password. To do so, just use following command. ``your_user`` must be a a user
which exists on your host. If you run this, you will be prompted to enter a password:
```
sudo smbpasswd -a your_user
```

You can now add the freshly created file server to your guest just by right clicking on the "Network" tab in your file 
explorer under Windows. Next select "Map network drive..." and enter the server address, your user name and the corresponding
password.
![](/resources/samba_network_drive.png)
## SCP
**work in progress**
