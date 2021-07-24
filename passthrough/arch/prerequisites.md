# Prerequisites
## Linux Distro
This time I want to try Arch Linux as my host OS. On one hand to get a feeling for other distros in combination with gpu-passthrough and on the other hand
because of its Rolling Releases and increased modularity. If you need any more information about the build of the machine using Arch, visit [the guide of the
official Arch wiki](https://wiki.archlinux.org/title/PCI_passthrough_via_OVMF).

## Needed Packages
- [qemu](https://wiki.archlinux.org/title/QEMU)
- [virt-manager](https://virt-manager.org/)
- [libvirt](https://wiki.archlinux.org/title/Libvirt)
- [ovmf](https://www.ovirt.org/develop/release-management/features/virt/ovmf.html)
- [bridge-utils](https://archlinux.org/packages/extra/x86_64/bridge-utils/)  

To simplify the whole installation, I'll leave this here:
```
pacman -S qemu virt-manager ovmf bridge-utils libvirt
```

## ISO Images
Of course, the needed images for a Windows VM are still the same:
- [Windows image file](https://www.microsoft.com/en-gb/software-download/windows10ISO)
- [virtio driver ISO](https://docs.fedoraproject.org/en-US/quick-docs/creating-windows-virtual-machines-using-virtio-drivers/index.html) (I recommend the stable release)
