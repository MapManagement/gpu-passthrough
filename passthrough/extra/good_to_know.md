# Good To Know
## Guest CPU
As soon as you installed any OS on your virtual machine, you probably check the resources it owns. You'll see things like the capacity and speed
of the RAM, the amount of storage you can use, the graphics card you passed-through and also the processor. If you followed my guide and also 
use and AMD processor, it will be recognized as an ``AMD EPYC-Rome Processor``.  
![](/resources/cpu_epyc_rome.png)
Eventhough my system ran perfectly fine, I was curious how some people were able to display the "real" CPU name within their guests. After a short research, I found out that it was quite simple to show the name of the actually installed CPU. You only need to make some changes within the
``<cpu/>`` element in your guest configuration. Just add or change following parameters:
```xml
<cpu mode="host-passthrough" check="none" migratable="on">
    <topology sockets="1" dies="1" cores="6" threads="2"/>
</cpu>
```
I also found some more detailed information about the whole topic [here](https://www.linux-kvm.org/images/6/65/Bonzini-Kvmforum18-cpu.pdf).  
So after changing the configuration, you should see the "real" CPU model name. In my case it's a Ryzen 9 3900X and as you can see, the guest also
recognizes the actual CPU now.
![](/resources/cpu_ryzen.png)

## Dual Boot and VM
You can also install an operating system directly on one of your disks AND use the created image for a guest. This means that
you're able to either boot into that image or start it as an virtual machine which owns a dedicated GPU. At first it seems a bit
odd, why should I boot into one of my systems if I can easily access it within a virtual machine? There are several reasons why
I decided to do that. On one hand I can play some of the games which are not playable in a VM. For example Valorant. On the
other hand I'm able to change any settings if I can not access this image anymore. To be honest, I already ran into issues
which would have been easier to fix if I had the possiblity to simply just change some stuff directly within the image. 
Furthemore you split up the two "worlds" so to say. So I would have a "work disk" and a "gaming disk" since I'm mainly playing
games in my Windows VM. But how do I add a whole disk to guest?  
First of all you need to install Windows or any other OS you want to use. I recommend to store it on it's disk. It will
simplify the configuration later on. So just install it as you would normally do. If you're finished, boot into your host OS
to configure everything else.