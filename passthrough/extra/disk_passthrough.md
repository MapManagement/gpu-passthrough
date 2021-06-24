# Disk Passthrough
I already listed the topic of passing a whole disk to one of my guests in [ideas](../../ideas.md). I didn't
need the increased speed and performance of the disk but wanted to create a dual boot **and** a VM at once. This
means that I should be able to access my guest within Linux through the virt-manager and also by directly booting
into it. After a long time of researching and testing around, I could reach my goal without any annoying
problems. Following text will explain how to pass a whole disk to a guest, how to configure the guest to be able
to boot into it and how to maintain your installation properly.
## Binding the disk
Binding a disk does not mean that we are binding a real disk. I will bind a SATA or NVMe controller to vfio like
I did with my second GPU. Eventhough the procedure should be known, I repeat it once more for binding the
controller. Check your IOMMU groups with following script. Your controller has to be in its own group.
```sh
for d in /sys/kernel/iommu_groups/*/devices/*; do n=${d#*/iommu_groups/*}; n=${n%%/*}; printf 'IOMMU Group %s ' "$n"; lspci -nns "${d##*/}"; done;
```
If I do this on my machine, I recognize that my NVMe controller has its own group and therefore can easily be
passed to a guest. Its device ID is ``1987:5012``.
```
IOMMU Group 14 01:00.0 Non-Volatile memory controller [0108]: Phison Electronics Corporation E12 NVMe Controller
[1987:5012] (rev 01)
```
Just add the device ID to your boot parameters to make sure that the disk is bound to vfio a boot. Open this file:
```
/boot/efi/loader/entries/Pop_OS-current.conf
```
And add your device ID at the very end:
```
options root=UUID=486bc510-1589-40ec-b4d3-f85cab587c46 ro quiet loglevel=0 systemd.show_status=false splash amd_iommu=on vfio-pci.ids="1002:aaf8,1002:687f,1022:1471,1022:1470,1022:1483,1022:1482,1987:5012"
```
Reboot the system and verify that the controller is used by vfio by using:
```
lspci -k
```

## Creating the VM
As always you need to create a VM. Just do it as if you would do it for a normal Windows guest which has access to
one of your GPUs. You can check every step [here](../first_try.md). But instead of creating an extra disk device
for your guest, you will need to pass your whole SATA/NVMe controller. Now set it as the first boot option and fire
up your system. Install Windows and the virtio drivers. 

## UUID
Eventhough I could boot Windows natively and use it as I normally would, I went into some issues as soon as I 
wanted to start the guest under Linux. My installation was completely resetted and I didn't know what to do.
Thus I looked for a solution and I found one. When you're booting into Windows natively, you need to save its UUID.
You can get it by running this short command in the command prompt:
```
WMIC csproduct list /format
```
Save the UUID, you will need it for your VM XML file later on. So get back to Windows, start virt-manager and create
a new VM. I just adjusted everything like the old "BareMetal" VM, how I called it, but changed one little thing.
Instead of using a random generated UUID for my guest, I entered that one, which I just got from the native
Windows boot. Start the installation and you will probably end in your Windows installation without any problems. 
The XML file you created before can be deleted. You will not need it anymore.