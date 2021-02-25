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
1022:1482, 1022:1483, 1022:1470, 1022:1471, 1002:687f, 1002:aaf8
```
Those devices will now be given to VFIO. This means that your guest GPU cannot be used anymore by the host OS and
therefore you will not have any output on all screens connected to that card after doing so. So don't be 
surprised. But how do we do that? That is the part where I think that Pop_OS! makes it really for us at least from
that what I know. Maybe it's most often that simple and I chose more advanced techniques in the past.  
Coming back to the binding, you need to open up this file:
```
/boot/efi/loader/entries/Pop_OS-current.conf
```
This contains boot parameters for your host OS. Here we have to set those parameters that will bind your devices
to VFIO at boot. To do so, you simple just have to add your device IDs and a general paramter like this:
```
options root=UUID=486bc510-1589-40ec-b4d3-f85cab587c46 ro quiet loglevel=0 systemd.show_status=false splash amd_iommu=on vfio-pci.ids="1002:aaf8,1002:687f,1022:1471,1022:1470,1022:1483,1022:1482"
```
So to make sure what I added to **options**:
```
amd_iommu=on vfio-pci.ids="1002:aaf8,1002:687f,1022:1471,1022:1470,1022:1483,1022:1482"
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
