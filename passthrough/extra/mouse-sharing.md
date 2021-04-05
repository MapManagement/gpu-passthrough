# Mouse Sharing
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
- it supports Linux, MacOs and Wndows (also FreeBSD and OpenBSD but these are no the OSes I'm running the most)
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

