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

## Sound
Especially if you want a "normal" experience when playing some games, sound should not be missed. No worries, passing sound from your guest
to your host is not difficult at all as long as you don't have any special demands. As always, there are multiple ways to hear everything that
your guest sends as output but for now I will only cover the easiest method.  
### Spice server
So after installing all virtio drivers in your virtual machine it is neccessary to shut it down again since we're going to change the actual config
file of the given virtual machine. Open virt-manager and select the virtual machine to view all details. Press the ``Add Hardware`` button and select
``Graphics``. You should see following options to adjust but in this case we don't have to change anything.
![](/resources/add-spice-server.png)  
Once you added a ``Spice server`` you already good to go. Don't be confused if multiple devices were added to your VM config, it will need all of
them to work properly. Now start your guest and click on the little monitor in the top left corner. Its tooltip says ``Show the graphical console``.
The windows will change from ``Details`` to ``Console``. As long as this windows is opened you should hear all sounds of your guest. You will
definitely hear some delay. While playing some games, I noticed how each sound appeared like a half to a full second later than it should. To get
rid of this dealy you have to try other methods.
![](/resources/running-spice-server.png)
### PulseAudio
I've tested this method with Arch and Pop_OS! as host operating systems and both times I was able to get sound from my guest without any
remarkable delay. I also couldn't notice any difference in the sound quality eventhough some users complained about it in some articles or
forums I visited so far. Another useful fact to know about this method, is the ability to adjust all sound within your PulseAudio manager
(whatever manager you're using). This picture shows my guest audio being adjustable just by my host PulseAudio settings.  
![](/resources/guest-pulse-audio.png)  
First of all you need to install a [PulseAudio sound server](https://www.freedesktop.org/wiki/Software/PulseAudio/). The package should be called
``pulseaudio`` not matter what distribution you're using. Installing PulseAudio itself is all we need but I also installed a GUI program to
see and control all input and output devices. The picture above is one of these programs and in my case it's called ``PulseAudio Volume Manager`` or
in short ``pavucontrol``. Feel free to use another software, there are plenty to choose from. You can find more of them
[here](https://wiki.archlinux.org/title/PulseAudio).  
Now we only need to edit our guest config. I read about multiple ways to pass audio and I'm not quite sure when to use what method. However, I only 
needed to add some arguments to the commandline of my configuration. But before adding arguments, we have to import a specific XML namespace. Open
virt-manager and select your guest. Switch to its ``Details`` view and click on ``Overview``. Here you can select the ``XML`` tab. You will see the
plain XML of your configuration which will need to be edited by hand. Your root element should be called something like this:
```xml
<domain type="kvm">
  ...
</domain>
```
In order to use specific commandline arguments, we have to add a XML namespace. So edit your root element and add follwing namespace:
```
<domain xmlns:qemu="http://libvirt.org/schemas/domain/qemu/1.0" type="kvm">
  ...
</domain>
```
I experienced a bug where namespaces could not be applied when pressing the ``Apply`` button in the bottm right corner. Unfortunately, I coulnd't 
resolve that problem yet. I'll probably add a solution as soons as I found one. Anyway, we also need to add the commandline arguments. Make sure that
no other devices are called ``hda`` otherwise you will face an error saying that commandline arguments cannot be applied. So add these arguments:
```xml
<domain xmlns:qemu="http://libvirt.org/schemas/domain/qemu/1.0" type="kvm">
  ...
  <devices>
    ...
  </devices>
  <qemu:commandline>
    <qemu:arg value="-device"/>
    <qemu:arg value="ich9-intel-hda,bus=pcie.0,addr=0x1b"/>
    <qemu:arg value="-device"/>
    <qemu:arg value="hda-micro,audiodev=hda"/>
    <qemu:arg value="-audiodev"/>
    <qemu:arg value="pa,id=hda,server=unix:/run/user/1000/pulse/native"/>
  </qemu:commandline>
</domain>
```
Now reboot your system and check if you can hear your guest's sound. If not, check the server using the ``pax11publish``. It should print the location of your server and some ID. If not, something with your PulseAudio is not configured correctly.