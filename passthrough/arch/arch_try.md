# Arch Try
## Enabling IOMMU
If IOMMU is already enabled, it also has to be supported by the kernel. Similar to Pop_OS! and most other distributions, it can easily be enabled
by changing the kernel boot parameters. The only thing that changes from build to build is the location of the paramter file. In constrast to my 
previous Pop_OS! build, I'm now using GRUB as my bootloader. So I modified following file: ``/etc/default/grub`` and added these parameters:
```
GRUB_CMDLINE_LINUX_DEFAULT="... amd_iommu=on iommu=pt ..."
```
Machines based on an AMD CPUs need ``amd_iommu=on``, Intel machines need ``intel_iommu=on``. Keep that file in mind, it's also important when
binding devices to vfio. For now the system should support IOMMU after a quick reboot. You can use ``dmesg`` to check if IOMMU is enabled.
The output should containt something like this:
```
dmesg | grep -i -e DMAR -e IOMMU

[    0.591055] pci 0000:00:00.2: AMD-Vi: IOMMU performance counters supported
```

## Verifying IOMMU grups
Once IOMMU was enabled successfully, it's essential to verify that the IOMMU groups of the system are matching the goals. Passing a specific graphics
card to a guest isn't a problem for most builds but the more hardware, and especially specific hardware, is needed within a guest, the more difficult
it becomes. Logically I already knew my IOMMU grouping since Arch did not change anything in comparison to Pop_OS!. So I just leave my much more
detailed explanation of my [Pop_OS! documenation](../pop_os/first_try.md#Binding-the-GPU) here. It should cover all steps and does not differ
from Arch. I wrote down following device IDs of my graphics card:
```
1002:687f, 1002:aaf8
```
Don't forget, if you want to pass any more devices to a guest later on, remember their device IDs. I deciced to pass a whole SSD to one of my guest
in a further step and added its ID to my list for example.

## Binding devices
As soons as all device IDs are known the devices' drivers need to be replaced. You don't want your devices to be controlled by "standard" drivers, it's
neccessary to use s specific vfio driver. To do so, we will adjust the boot parameters of our system. They will make sure that all devices, you want to
pass to a guest, cannot be accessed by normal but by these vfio drivers. Keep in mind, all devices that use vfio drivers are not reachable by your host 
anymore. You can either bind your devices via kernel parameters (did that in my Pop_OS! build) or via a new modprobe config file. If you decide to do
it via a new modprobe configuration, don't forget to regenerate your intiramfs. Either way, add following text/lines:
- For kernel parameters, edit ``/etc/default/grub`` and add your device IDs like this:  
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash vfio-pci.ids=1002:687f, 1002:aaf8"
```
- For modprobe, create a new config like ``/etc/modprobe.d/my-vfio.conf and add your IDs:  
```
options vfio-pci ids=1002:687f, 1002:aaf8
```
To prevent any drivers to load before the virtio one's can, it is recommended to adjust the loaded modules at boot time. Just add following modules to your
mkinitcpio configuration (``/etc/mkinitcpio.conf``):
```
MODULES=(... vfio_pci vfio vfio_iommu_type1 vfio_virqfd modconf ...)
```
Now it's time to regenerate your initramfs again. You can do it by using following bash script:
```
mkinitcpio -P
```