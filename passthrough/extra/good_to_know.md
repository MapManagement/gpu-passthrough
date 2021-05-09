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