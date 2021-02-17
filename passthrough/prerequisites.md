# Prerequisites
## Linux Distro
I decided to go with [Pop!_OS](https://pop.system76.com/) for this build but it might still change if I recognize any problems or things I just do not like. So why not choosing the distro that matches best for your use case but for the beginning it will be Pop!_OS. But why, you might ask. I simply just used too Debian/Ubuntu based systems and don't want to make it unnecessary difficult for me. Furthermore did I test Linux Mint and Pop!_OS the longest time yet when it comes to desktop machines. That's really much it what I can tell you about my decision so far. 
So after installing the AMD ISO of the distro, I configured the GUI (using GNOME by the way) and some system settings. I'd say that it's time to start the actual passthrough part.

## Needed Packages
- [qemu-kvm](https://launchpad.net/qemu-kvm)
- [virt-manager](https://virt-manager.org/)
- [libvirt-clients](https://packages.debian.org/sid/libvirt-clients)
- [libvirt-daemon-system](https://packages.debian.org/sid/libvirt-daemon-system)
- [ovmf](https://www.ovirt.org/develop/release-management/features/virt/ovmf.html)
- [bridge-utils](https://launchpad.net/bridge-utils)
To simplify the whole installation, I'll leave this here:
```
sudo apt install emu-kvm qemu-utils libvirt-daemon-system libvirt-clients virt-manager ovmf
```