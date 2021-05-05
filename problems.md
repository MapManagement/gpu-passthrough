# Problems
## Finding "perfect" GPU
### **Problem**
Most of you probably think that I'm talking about the guest GPU for my virtual machine, but I'm not. This problem is about my host GPU since my Vega 56 seems quite good for a Windows VM. So at the beginning I wanted to use a GT 710 but it only has one DVI and one HDMI port and my monitors do not support DVI. Additionally the graphics card has to handle a 3440x1440p @75Hz ultrawide display and the HDMI port of the GT 710 does not have the needed bandwidth.  
The second card I wanted to use was an older R9 290. Unfortunately that card didn't work at all. Neither Windows nor the BIOS recognized the card so I decided to buy a new one. Since I am using Linux as my host OS I bought an AMD card. To be precise, it was an RX 550 with one HDMI, DP and DVI port. That card should be enough for my purpose, especially because I won't need an extremely well performing card for Pop!_OS.
### **Solution**

---

## B550 board and its IOMMU groups
### **Problem**
**I**nput-**O**utput **M**emory **M**anagement **U**nit or in short IOMMU connects devices that are able to access the memory directly (DMA-capable) ot an address of the main memory. I really don't want to get into details and therefore I will only explain all technical background information concercing this problem.
Nonetheless, the problem has its origin in the B550 chipset of my mainboard. Unfortunately, the B550 chipset
puts all lanes connected to it into one IOMMU group. In my case it's two PCIe slots, the ethernet controller an NVMe slot and some more stuff. But when passing a device, e.g. my GPU, to a VM I have to pass everything of exact that IOMMU group also to my VM. Meaning, that if I wanted to use my guest GPU in the second or third PCIe slot, I would have to pass everything else too. Luckily, the first PCIe slot is directly connected to the CPU. That's the reason why I put my Vega into the first slot. It seemes to work for now but who knows what problems I will get into within the next weeks.
### **Solution**

---

## USB devices and audio
### **Problem**
Of course, I also need to pass through some USB devices like a keyboard or a mouse and my audio to use a microphone or
some headphones. Passing through a keyboard and a mouse is quiet easy but at first, I had some problems with my
controller and I really need to check how I can use my normal audio devices within the VM.
### **Solution**

---

## Anti Cheats
### **Problem**
The primary reason why I really want such a machine is because of the comfortable way of using Linux for daily stuff
and Windows mostly for gaming. I'm not only talking aboout single player games, no, the games I want to play also
include several mutliplayer games. But multiplayer games always bring something with them and that is anti cheat
software. The are quite important for some games and without them it would be definitely hard to play many games
without running into cheaters and so on. On the other hand they have been a problem for Linux users for a long time
and won't allow most multiplayer games to run under Linux. Eventhough I will play in a VM, some anti cheats still
kick or even ban your from a small amount of games since the developers do not really like people playing their
game in a VM. Luckily there are workarounds to bypass those anti cheats from detectin my VM and I will test them for
sure. Therefore [this file](games.md) contains games that I tested so far, how my machine performed, what problems
occured and how the anti cheat reacted to my VM.
### **Solution**

---

## AMD's reset bug
### **Problem**
As you should already know, I'm using a Vega 56 for my guest OS. Most things should work fine and the performance is 
not that worse compared to a normal Windows machine. But there's one problem with AMD GPUs when it comes to the vfio
binding. It's called the "reset bug" because of following problem: Most non current gen GPUs do not reset once you
shutdown the guest. This means that you cannot use the card again if you want to boot the guest again. You simply just
won't have any output from it and therefore the whole reason of using a separate graphics card for your VMs becomes
obsolet. Then you have to restart your whole machine and to be honest, if you would keep it like that you could also
set up a dual boot. It would probably be the same at the end. Some cards, really depending on the generation and the
BIOS of the card, can be passed through multiple times without rebooting if you do some workarounds. My Vega 56 suffers from that bug and before I'm not able to buy a new GPU, I have to check if I can find a fix for it.
### **Solution**
Surprisingly the bug just went away without doing something against it. I'm not sure what fixed it since I did not
change anything that would be able to reset the GPU after closing the VM. Maybe my dual boot with Pop!_OS and
Windows 10 has some impact on the graphics card. It's also possible that the vfio driver that I recently
installed in my Windows VM changed some stuff but as I already said, I cannot tell what really solved the
problem.  
**Update:** I found out that my graphics card is affected too but I probably missunderstood the bug a little bit.
If I turn my VM off properly, either by pressing "Shut down" in Windows or by using the "Shut down" button in
virt-manager, I can restart the guest as often as I want to. If I use the "Force off" button in virt-manager, the
VM will not start again. My Vega cannot reset itself and therefore it won't useable at a later point. But should 
not be such a big problem if my guest runs stable enough. It's only annoying at the beginning and while testing 
some stuff.  
**Update:** After testing around a longer time, I noticed another interesting bug that is connected to AMD's reset bug.
No matter how I shutdown my VM, after about 15min the fans of my GPU will spin at their maximum speed (you cannot not
hear it :D) and I'm not able to start any VM which uses my guest GPU. If I try to, I get an error which says:  
```
Unknown PCI header type '127'
```
Unlike the "normal" reset bug it can be fixed by using a specific BIOS version. Therefore I already updated my BIOS to
the most recent version. I have to admit that I did use the oldest available version of it before the update.  
**Update:** I found a really helpful source to solve the reset bug. It does not work for all
AMD cards but the chance of having success is not that low. It worked for my Vega 56
flawlessly already a multiple times. So I recommend to give following repo a try:  
[Vendor Reset](https://github.com/gnif/vendor-reset)

---

## Upload Speed
### **Problem**
After I tested some general things I wanted to download some games and play them. So I would have some comparisons
to my actual Windows system. Therefore I made a speedtest to measure my download and upload speed but I realized
that my upload was only about 1 Mbit/s eventough it should be about 40 Mbit/s. I wondered what caused that
especially because of my download being as fast as always. Even my host OS was able to upload data at a speed
of 40 Mbit/s, so I was sure that my VM must have a problem.
### **Solution**
I found out that I need to change something in my VM setting in the virt-manager. The network interface controller
(NIC) has to be set on ``virtio`` and not ``e1000e`` or anything else.

![](resources/upload_speed.png)

---

## Weird Resets and Restarts
### **Problem**
I encountered a problem while testing around with one of my Windows VM. So if I boot up a virtual machine which
owns some USB devices, they will be resetted after some time. This means, that my host will regain full access
over this device. I don't know why and what happens but ``dmesg`` shows following processes:  
```
[ 3839.874525] vendor-reset-drm: atomfirmware: bios_scratch_reg_offset initialized to 4c
[ 3840.118706] vfio-pci 0000:08:00.0: AMD_VEGA10: bus reset disabled? yes
[ 3840.118713] vfio-pci 0000:08:00.0: AMD_VEGA10: SMU response reg: ffffffff, sol reg: 0, mp1 intr enabled? no, bl ready? no, baco? off
[ 3840.118716] vfio-pci 0000:08:00.0: AMD_VEGA10: performing post-reset
[ 3840.157584] vfio-pci 0000:08:00.0: AMD_VEGA10: reset result = 0
[ 3850.276088] usb 1-6.2: reset full-speed USB device number 4 using xhci_hcd
[ 3850.623792] usb 1-6.3: reset full-speed USB device number 7 using xhci_hcd
[ 4406.233603] usb 1-6.3: reset full-speed USB device number 7 using xhci_hcd
[ 4406.601910] usb 1-6.2: reset full-speed USB device number 4 using xhci_hcd
[ 4406.949872] usb 1-6.2: reset full-speed USB device number 4 using xhci_hcd
[ 4407.259143] input: USB Gaming Mouse as /devices/pci0000:00/0000:00:01.2/0000:02:00.0/usb1/1-6/1-6.2/1-6.2:1.0/0003:04D9:AD50.0012/input/input47
[ 4407.259304] hid-generic 0003:04D9:AD50.0012: input,hidraw4: USB HID v1.10 Mouse [USB Gaming Mouse] on usb-0000:02:00.0-6.2/input0
[ 4407.471271] input: USB Gaming Mouse Keyboard as /devices/pci0000:00/0000:00:01.2/0000:02:00.0/usb1/1-6/1-6.2/1-6.2:1.1/0003:04D9:AD50.0013/input/input48
[ 4407.531319] input: USB Gaming Mouse System Control as /devices/pci0000:00/0000:00:01.2/0000:02:00.0/usb1/1-6/1-6.2/1-6.2:1.1/0003:04D9:AD50.0013/input/input49
[ 4407.531398] input: USB Gaming Mouse Consumer Control as /devices/pci0000:00/0000:00:01.2/0000:02:00.0/usb1/1-6/1-6.2/1-6.2:1.1/0003:04D9:AD50.0013/input/input50
[ 4407.531456] input: USB Gaming Mouse as /devices/pci0000:00/0000:00:01.2/0000:02:00.0/usb1/1-6/1-6.2/1-6.2:1.1/0003:04D9:AD50.0013/input/input51
[ 4407.531538] input: USB Gaming Mouse as /devices/pci0000:00/0000:00:01.2/0000:02:00.0/usb1/1-6/1-6.2/1-6.2:1.1/0003:04D9:AD50.0013/input/input52
[ 4407.531609] hid-generic 0003:04D9:AD50.0013: input,hiddev2,hidraw5: USB HID v1.10 Keyboard [USB Gaming Mouse] on usb-0000:02:00.0-6.2/input1
[ 4407.561120] hid-generic 0003:04D9:AD50.0014: hiddev3,hidraw6: USB HID v1.10 Device [USB Gaming Mouse] on usb-0000:02:00.0-6.2/input2
[ 4477.788893] usb 1-6.3: reset full-speed USB device number 7 using xhci_hcd
[ 4490.704108] usb 1-6.3: reset full-speed USB device number 7 using xhci_hcd
jan@jan-lin:~$ 
```
Obviously one of my USB devices is resetted. In this case it's my mouse (``USB Gaming Mouse``). If I restart the VM, I can
use its mouse and keyboard flawlessly as soon as the reset starts again. Additionally I epxerienced some more issues. If I want to update
my video drivers, the VM will freeze and restart. Same happens if the Windows guest enters the standby mode.
### **Solution**
I expected that the patch for the reset bug caused all of this but I found out that it actually was not the case. First I tested various
other VMs which run operating systems like Kali Linux, Android, Elementary or also Windows. None of these seemed to have any similar issues
so I checked the Windows VM which behaved kinda weird. At the end it was Corsair's iCUE software. Unsurprisingly, that such a software can
lead to problems like that. So after uninstalling it, the guest ran stable again. Probably it's not only iCUE, other driver software likewise
cause such unwanted behaviors.  
**Update:** Eventhough removing iCUE already fixed some bugs, others didn't disappear. My USB devices kept resetting and made it nearly
impossible to use my Windows VMs. After testing around I found out that switching the USB ports from 2.0 to 3.X fixed the issues. At this point
I cannot explain why I need to use some of my 3.X USB ports instead of lower ones. Linux based VMs do not suffer from such problems and work
fine without changing anything. Maybe the future will bring light into the darkness :D