# Reset Bug
## Overview
My Vega 56 suffered from the so called "reset bug" which basically means that I could only
start a guest once. If I tried to start a guest that used my Vega atfer rebooting it, it
wouldn't work. I got following error message:
```
vfio Unknown PCI header type '127' for device
```
It's a common bug of AMD cards. The device won't reset into a state that would allow the system
to pass it to any guest again. You had to reset the card by restarting your machine for example.
However, some users developed a fix so that these cards could be passed multiple times to either
the exact same or different guests. You can find it [here](https://github.com/gnif/vendor-reset).
As you might guess, I used this fix and also described my problem in [this file](../../problems.md).
Reportedly AMD fixed this issue with the 6000 gen. So cards like the 6900XT, 6800XT or 6700XT should
not need any special workaround anymore and should reset properly after turning off a guest.

## Upgrade to 6700XT
Atfer upgrading from my Vega to an RX 6700XT PowerColor Red Devil I thought that I would no longer
need the vendor-reset workaround. I installed the card, changed the PCI device IDs in my kernel
parameters and booted my machine. That card was bound to vfio-drivers and I was able to start my
guest as usual. The guest used the 6700XT and everything worked flawlessly - at least I thought so.
After playing some games and testing around, I remembered to remove the vendor-reset. I did so
and tried to restart my guests and I encountered the typical reset bug. I could only start a guest
once. The second start ended in a blank screen. Trying to start it a third time ended in this error:
```
vfio Unknown PCI header type '127' for device
```
Seems odd, right?

## Solution
I checked following stuff wihtout any success:
- kernel parameters
- mkinitcpio settings
- BIOS settings
- BIOS update
- fresh installation of Linux (Manjaro)
- different guests
