# First try of the build
The first time I tried to create a Windows virtual machine with a dedicated GPU on this computer was only a 
simple test. I wanted to check how easy it is to create such a VM using Pop!_OS and what I have to expect and
take care of when I set up my daily machine. Following section explain my approach and experiences.

## Binding the GPU
I have to be honest that the very first time I tried to bind the GPU I recognized that my IOMMU groups are not
quite suitbale if I want to use the second PCIe slot for my guest GPU. A detailed version of the problem can be
found [here](../problems.md##B550-board-and-its-IOMMU-groups). Therefore I swapped my GPUs and used the guest GPU in the first slot. My
explnation will start at this point.  
So after booting into Linux I checked my IOMMU groups and was quite relieved as I saw the result of changing
the PCIe slot. I installed [all packages](prerequisites.md#Needed-Packages) that I needed and started the binding.  
Compared to other ways I tried in the past, the binding seemed relatively easy and quite forward. So at first you
will need to check your IOMMU groups since you will need to pass through everything to the VM that is in the same
group as you're GPU is. To check your IOMMU groups, you need to run following code in a terminal:
```sh
for d in /sys/kernel/iommu_groups/*/devices/*; do n=${d#*/iommu_groups/*}; n=${n%%/*}; printf 'IOMMU Group %s ' "$n"; lspci -nns "${d##*/}"; done;
```
Alternatively you can also just save it in a shell file and execute it. After executing it you'll see a longer
printed in the terminal. This text contains you're devices that are in your pc and therefore assigned to a 
specific IOMMU group. How they are assigned really depends on your chipset and your CPU. Some platforms are
more comfortable when it comes to such groups like B550 for example. Whether or not you need to find your guest
GPU in there and check its group. For a better understanding I provide my ouput. Don't nbe confused if something's missing, I shortened it a bit to keep a better overview:
```
IOMMU Group 0 00:01.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse PCIe Dummy Host Bridge [1022:1482]
IOMMU Group 0 00:01.1 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse GPP Bridge [1022:1483]
IOMMU Group 0 00:01.2 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse GPP Bridge [1022:1483]
IOMMU Group 0 01:00.0 Non-Volatile memory controller [0108]: Phison Electronics Corporation E12 NVMe Controller [1987:5012] (rev 01)
IOMMU Group 0 02:00.0 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] Device [1022:43ee]
IOMMU Group 0 02:00.1 SATA controller [0106]: Advanced Micro Devices, Inc. [AMD] Device [1022:43eb]
IOMMU Group 0 02:00.2 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:43e9]
IOMMU Group 0 03:00.0 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:43ea]
IOMMU Group 0 03:06.0 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:43ea]
IOMMU Group 0 03:07.0 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:43ea]
IOMMU Group 0 03:08.0 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:43ea]
IOMMU Group 0 03:09.0 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:43ea]
IOMMU Group 0 04:00.0 VGA compatible controller [0300]: Advanced Micro Devices, Inc. [AMD/ATI] Lexa PRO [Radeon 540/540X/550/550X / RX 540X/550/550X] [1002:699f] (rev c7)
IOMMU Group 0 04:00.1 Audio device [0403]: Advanced Micro Devices, Inc. [AMD/ATI] Baffin HDMI/DP Audio [Radeon RX 550 640SP / RX 560/560X] [1002:aae0]
IOMMU Group 0 07:00.0 Ethernet controller [0200]: Realtek Semiconductor Co., Ltd. RTL8125 2.5GbE Controller [10ec:8125] (rev 05)
IOMMU Group 11 0c:00.0 Non-Essential Instrumentation [1300]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse PCIe Dummy Function [1022:148a]
IOMMU Group 12 0d:00.0 Non-Essential Instrumentation [1300]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse Reserved SPP [1022:1485]
IOMMU Group 13 0d:00.1 Encryption controller [1080]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse Cryptographic Coprocessor PSPCPP [1022:1486]
IOMMU Group 14 0d:00.3 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] Matisse USB 3.0 Host Controller [1022:149c]
IOMMU Group 15 0d:00.4 Audio device [0403]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse HD Audio Controller [1022:1487]
IOMMU Group 1 00:02.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse PCIe Dummy Host Bridge [1022:1482]
IOMMU Group 2 00:03.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse PCIe Dummy Host Bridge [1022:1482]
IOMMU Group 2 00:03.1 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse GPP Bridge [1022:1483]
IOMMU Group 2 09:00.0 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:1470] (rev c3)
IOMMU Group 2 0a:00.0 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:1471]
IOMMU Group 2 0b:00.0 VGA compatible controller [0300]: Advanced Micro Devices, Inc. [AMD/ATI] Vega 10 XL/XT [Radeon RX Vega 56/64] [1002:687f] (rev c3)
IOMMU Group 2 0b:00.1 Audio device [0403]: Advanced Micro Devices, Inc. [AMD/ATI] Vega 10 HDMI Audio [Radeon Vega 56/64] [1002:aaf8]
```

**Update:**  
After testing around and doing more and more stuff with my VMs, I wondered why my system only has a few IOMMU groups.
Especially if I wanted to bind other devices to my guests, it didn't seem that easy because of the low amount of groups.
At the end I found out that I hadn't "ACS" enabled in my BIOS settings. Since the whole backrgound behind IOMMU is a bit
more complicated than other things, I decided to not explain everything about it. I don't even understand all of it myself. Therefore I just recommend reading [this](https://vfio.blogspot.com/2014/08/iommu-groups-inside-and-out.html)
article. It should include most details and furthermore I leave my "new" IOMMU groups just here.
```
IOMMU Group 0 00:01.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse PCIe Dummy Host Bridge [1022:1482]
IOMMU Group 10 00:08.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse PCIe Dummy Host Bridge [1022:1482]
IOMMU Group 11 00:08.1 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse Internal PCIe GPP Bridge 0 to bus[E:B] [1022:1484]
IOMMU Group 12 00:14.0 SMBus [0c05]: Advanced Micro Devices, Inc. [AMD] FCH SMBus Controller [1022:790b] (rev 61)
IOMMU Group 12 00:14.3 ISA bridge [0601]: Advanced Micro Devices, Inc. [AMD] FCH LPC Bridge [1022:790e] (rev 51)
IOMMU Group 13 00:18.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Matisse Device 24: Function 0 [1022:1440]
IOMMU Group 13 00:18.1 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Matisse Device 24: Function 1 [1022:1441]
IOMMU Group 13 00:18.2 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Matisse Device 24: Function 2 [1022:1442]
IOMMU Group 13 00:18.3 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Matisse Device 24: Function 3 [1022:1443]
IOMMU Group 13 00:18.4 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Matisse Device 24: Function 4 [1022:1444]
IOMMU Group 13 00:18.5 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Matisse Device 24: Function 5 [1022:1445]
IOMMU Group 13 00:18.6 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Matisse Device 24: Function 6 [1022:1446]
IOMMU Group 13 00:18.7 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Matisse Device 24: Function 7 [1022:1447]
IOMMU Group 14 01:00.0 Non-Volatile memory controller [0108]: Phison Electronics Corporation E12 NVMe Controller [1987:5012] (rev 01)
IOMMU Group 15 02:00.0 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] Device [1022:43ee]
IOMMU Group 15 02:00.1 SATA controller [0106]: Advanced Micro Devices, Inc. [AMD] Device [1022:43eb]
IOMMU Group 15 02:00.2 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:43e9]
IOMMU Group 15 03:00.0 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:43ea]
IOMMU Group 15 03:08.0 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:43ea]
IOMMU Group 15 04:00.0 VGA compatible controller [0300]: Advanced Micro Devices, Inc. [AMD/ATI] Lexa PRO [Radeon 540/540X/550/550X / RX 540X/550/550X] [1002:699f] (rev c7)
IOMMU Group 15 04:00.1 Audio device [0403]: Advanced Micro Devices, Inc. [AMD/ATI] Baffin HDMI/DP Audio [Radeon RX 550 640SP / RX 560/560X] [1002:aae0]
IOMMU Group 15 05:00.0 Ethernet controller [0200]: Realtek Semiconductor Co., Ltd. RTL8125 2.5GbE Controller [10ec:8125] (rev 05)
IOMMU Group 16 06:00.0 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:1470] (rev c3)
IOMMU Group 17 07:00.0 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:1471]
IOMMU Group 18 08:00.0 VGA compatible controller [0300]: Advanced Micro Devices, Inc. [AMD/ATI] Vega 10 XL/XT [Radeon RX Vega 56/64] [1002:687f] (rev c3)
IOMMU Group 19 08:00.1 Audio device [0403]: Advanced Micro Devices, Inc. [AMD/ATI] Vega 10 HDMI Audio [Radeon Vega 56/64] [1002:aaf8]
IOMMU Group 1 00:01.1 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse GPP Bridge [1022:1483]
IOMMU Group 20 09:00.0 Non-Essential Instrumentation [1300]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse PCIe Dummy Function [1022:148a]
IOMMU Group 21 0a:00.0 Non-Essential Instrumentation [1300]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse Reserved SPP [1022:1485]
IOMMU Group 22 0a:00.1 Encryption controller [1080]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse Cryptographic Coprocessor PSPCPP [1022:1486]
IOMMU Group 23 0a:00.3 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] Matisse USB 3.0 Host Controller [1022:149c]
IOMMU Group 24 0a:00.4 Audio device [0403]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse HD Audio Controller [1022:1487]
IOMMU Group 2 00:01.2 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse GPP Bridge [1022:1483]
IOMMU Group 3 00:02.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse PCIe Dummy Host Bridge [1022:1482]
IOMMU Group 4 00:03.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse PCIe Dummy Host Bridge [1022:1482]
IOMMU Group 5 00:03.1 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse GPP Bridge [1022:1483]
IOMMU Group 6 00:04.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse PCIe Dummy Host Bridge [1022:1482]
IOMMU Group 7 00:05.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse PCIe Dummy Host Bridge [1022:1482]
IOMMU Group 8 00:07.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse PCIe Dummy Host Bridge [1022:1482]
IOMMU Group 9 00:07.1 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Starship/Matisse Internal PCIe GPP Bridge 0 to bus[E:B] [1022:1484]
```

As you can see, any part has its own entry divided within three parts that you should take care of. First of all
there is the <span style="color:#5463d3">IOMMU group</span>, followed by the <span style="color:#ce4648">name of the device</span> and at the end is the <span style="color:green">so called device ID</span>. here you can see my
guest GPU:

<span style="color:#5463d3">IOMMU Group 2 0b:00.0</span> <span style="color:#ce4648">VGA compatible controller [0300]: Advanced Micro Devices, Inc. [AMD/ATI] Vega 10 XL/XT [Radeon RX Vega 56/64]</span> <span style="color:green">[1002:687f] (rev c3)</span>

As soon as you found you GPU and any other devices you want to give to the VM, you need to check the IOMMU groups.
In my case, there is only the VGA and sound controller and some PCIe bridges. Unfortunately, I don't know yet
whether the PCIe bridges will have any impact on my use case. I definitely have to look up some stuff to 
understand everything behind it. But for now, I need to pass them to my VM too otherwise I will run into an error
as soon as I want to create a VM using my guest GPU. If your IOMMU groups are fine you have to write down those
device IDs. For example the IDs I wrote down:
```
1002:687f, 1002:aaf8
```
Those devices will now be given to VFIO. This means that your guest GPU cannot be used anymore by the host OS and
therefore you will not have any output on all screens connected to that card after doing so. So don't be 
surprised. But how do we do that? That is the part where I think that Pop_OS! makes it really easy for us at least
from that what I know. Maybe it's most often that simple and I chose more advanced techniques in the past.  
Coming back to the binding, you need to open up this file:
```
/boot/efi/loader/entries/Pop_OS-current.conf
```
This contains boot parameters for your host OS. Here we have to set those parameters that will bind your devices
to VFIO at boot. To do so, you simple just have to add your device IDs and a general paramter like this:
```
options root=UUID=486bc510-1589-40ec-b4d3-f85cab587c46 ro quiet loglevel=0 systemd.show_status=false splash amd_iommu=on vfio-pci.ids="1002:aaf8,1002:687f"
```
So to make sure what I added to **options**:
```
amd_iommu=on vfio-pci.ids="1002:aaf8,1002:687f"
```
Afterwards you have to reboot your machine since the freshly added parameters are only initialized at boot. If you
did everything right, you should not have any output ont any screen that is connected to your guest GPU. Otherwise
you probably did something wrong. Of course, this explanation cannot help everyone since the whole setup really
depends on your machine and components. 
As soon as the machine rebooted, you can easily check if the binding worked by executing following command:
```
lspci -k
```
This command will print all PCI devies and buses of your system. You should look for your GPU and check the 
Kernel driver. If you did everything right, it should use the ``vfio-pci`` driver. To clarify what I mean:
```
0b:00.0 VGA compatible controller: Advanced Micro Devices, Inc. [AMD/ATI] Vega 10 XL/XT [Radeon RX Vega 56/64] (rev c3)
	Subsystem: Sapphire Technology Limited Vega 10 XL/XT [Radeon RX Vega 56/64]
	Kernel driver in use: vfio-pci
	Kernel modules: amdgpu
0b:00.1 Audio device: Advanced Micro Devices, Inc. [AMD/ATI] Vega 10 HDMI Audio [Radeon Vega 56/64]
	Subsystem: Advanced Micro Devices, Inc. [AMD/ATI] Vega 10 HDMI Audio [Radeon Vega 56/64]
	Kernel driver in use: vfio-pci
	Kernel modules: snd_hda_intel

```

## Creating the VM
### Pre-Configuration
Now I opened virt-manager and created a new VM by clicking on the top left icon.  
**(Step 1)** A new window will pop up which wants to know how the operating system needs to be installed. Simply
just select the "Local install media".  
**(Step 2)** The following step wants you to choose the image file. Select your Windows
10 ISO by browsing through
your local file and make sure that the operating system is recognized as Windows 10 and not something else. If so,
change it to Windows 10 and go forward.  
**(Step 3)** Now you have to set your allocated memory and the amoutn of threads you want to give to the VM.
Adjust it to the specs of your pc but try to keep enough resources for your host.  I chose to give 16GB and 12 threads of my host to the guest.  
**(Step 4)** The storage will be the next in the line. Depending on your system, you can either create a disk
image on your host drives, create a new partition or pass through a whole drive. For now, especially because I'm
only testing, I decided to go with a disk image on one of my host drives. I plan on passing through a whole
disk but it seems like my IOMMU groups will prevent me from doing so. I need to check this in the future.
Nonetheless, I gave about 100GB to my guest but it is going to be more as soon as I tested enough.  
**(Step 5)** Last but not least you can give your VM a name and decide whether you want to **"customize the
configuration before install"**. You need to tick this checkbox to be able to give a dedicated GPU to your guest.
The "Network selection" might be interesting but did not play any role for my machine at this time. So I finished
the pre-configuration by pressing "Finish"
### Advanced Configuration
I will only cover the important parts. So things I'm not metioning should only be changed
if you know why and how to change them. Don't be surprised if some pictures don't look
like your virt-manager window. I made these after testing the VM and therefore I can't
edit all options anymore.  
So first of all, you have to change the ``Firmware`` to an UEFI. I'm not sure what the 
different listd UEFI's do but I chose ``UEFI x86_64: /usr/share/OVMF/OVMF_CODE_4M.fd``.
Everything else is not needed to be changed.
![](/resources/config-overview.png)  
In the next step you really need to edit is the ``CPUs`` section. You're probably not 
using the same CPU and therefore you need to adjust the numbers to your CPU. Since I'm
using a 12C/24T processor, I decided to give 6C/12T to my gaming VM. I recommend to tick
the  ``Copy host CPU configuration`` and ``Manually set CPU topology`` box. ``Sockets`` should be set to your amount of CPUs you're using, ``Cores`` to the amount of your actual
cores and ``Threads`` to the amount of logical threads one of your cores has. The picture
explains most of it I think. Depending on the things you select here, you can gain some
performance but for most people this type of setting up the CPU configuration should work.
(there is stuff like CPU pinning or the option ``host-passthrough`` instead of ``Copy
host CPU configuration``).
![](/resources/config-cpus.png) 
Now you add the storage device for your booting image. Hopefully you already donwloaded
the Windows 10 ISO. To create such a device, you have to click on ``Add Hardware`` on the
bottom left. Click on a so called ``Storage`` device, choose ``Select or create custom
storage`` and find your image file. Change the ``Device type`` to ``CDROM device`` and
your good to go by clicking on ``Finish``.  
![](/resources/config-image-file.png)  
Don't forget to change the boot order under ``Boot Options``.
![](/resources/config-boot-order.png) 
You will do the exact same with the [virtio drivers](/passthrough/prerequisites.md) you
probably donwloaded previously. Simply just add another CDROM which contains the drivers.
You will need them as soon as you installed Windows. Otherwise there are going to occur some
issues.  
Last but not least, you have to add your graphics card, a mouse and a keyboard. Therefore you should
have at least 2 keyboards and mice connected to your machine. One pair for your host and the other one
for your guest. Make sure that all devices are already connected before trying to give them to the VM.
Of course, you can also just passthrough a whoel USB controller but this is another topic which could
be added to this repo in the near future.  
Just hit that ``Add Hardware`` button and find ``PCI Host Device``. You're going to see a bunch of
your PCI devices. Now you need to add all devices that we bound to vfio earlier or in other words, add
all devices that are in the same IOMMU group as your graphics card. I only needed to add two devices:
my video and audio controller. The following pictures shows them. Both of them end with ``[Radeon RX
Vega 56/64]``.  
![](/resources/config-pci-gpu.png) 
Repeat that process with your USB devices (mouse, keyboard and everything else you need). Instead of 
opening ``PCI Host Device`` in virt-manager, you'll need to selct ``USB Host Device`` and find your
devices.  
![](/resources/config-usb.png) 
Furthemore, I recommend keeping a "virtual monitor" so to say. In virt-manager it's called ``Video
QXL``. It's a simple window which will be recognized as an extra monitor and will be really helpful
if you're using and AMD card (didn't test it with an Nvidia card so far) because of the [reset bug](/problems.md#AMD's-reset-bug) and/or missing drivers. In my case, I did not get a signal on my second
monitor and therefore I needed to use this little Window until I installed Windows and the virtio
drivers.  
The only thing I changed after the creation process at my first try can be found [here](/problems.md##Upload-Speed).
It improves your upload speed quite a lot. This means that I created the VM by clicking
on ``Create`` at the bottom right afterwards and booted into it. The whole installation is covered by
the next section.  
You can find the XML file of my Windows 10 guest [here](win10.xml). It should include at least everything
what was listed within this explanation. But be aware It is **not** the VM which owns its own disk to boot into it.

## Installing Windows
The VM should start after some background processes are finished. Depending on your machine, you
either see the Windows installation within your "virtual monitor" or on your extra monitor. However,
you now have to install Windows as usual. There are more than enough guides out there which explain how
Windows can be installed and as I already said, it's not different from installing Windows directly
on your machine. When selecting your installation drive, you're also able to load the virtio drivers by
clicking on ``Load driver``. Sometimes you need to do it in order to see your empty virtio drive(s).
I'll cover the whole driver installation process later on. If you did everything right and didn't run
into any common issues you should end up in Windows like me. I checked the Taskmanager and saw my
Vega, 16GB of RAM and my 6 cores and 12 threads. Now I needed to install the virtio drivers.
Compared to all other steps I've done so far it seemed to be the easiest one.

## Configuring Windows
To ensure that basic functionalities and processes run without any problems on your guest, you have to
install the virtio drivers. I passed an extra CD drive to my guest which contained all needed files
and executables. 
![](/resources/windows_cd_drivers.png)
However you gain access to those files, you need to install the drivers. The executable to run is called
``virtio-win-get-x64``. If the name changes over time, just look for a installer package for Windows
which is made for x64 systems. After you installed all drivers, you should see your GPU, connect to
the network etc. if it not happened yet. Either way, to make sure that everything works properly, restart
your guest once. Now you can do whatever you want. It runs nearly as good as a native installtion on your
machine, depending on the amount of RAM and CPU cores you gave to the guest. You can also install
an official GPU driver. I downloaded the AMD drivers and only went into problems when I wanted to update
to a specific version. Just try out what fits best for you.