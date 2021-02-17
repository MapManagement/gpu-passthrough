# Setup

## Components
**Mainboard:** Gigabyte B550 Aorus Pro  
**CPU:** Ryzen 9 3900X  
**RAM:** 32GiB 3000MHz CL16
**PSU:** BitFenix Formula Gold 750W  
**Cooler:** Arctic Liquid Freezer II 360  
**Host GPU:** PowerColor Radeon RX 550 Red Dragon  
in second PCIe slot
**Guest GPU:** Sapphire Pulse Radeon RX Vega 56  
in first PCIe slot
**Storage**: SSD 240GiB, 3TiB HDD, (120GiB SSD, 1TiB HDD, 256GiB NVMe)  
For testing, only the first two drives will be used. When going for an actual daily machine, I will probably use all of
them.

## BIOS Settings
None of my parts are overclocked, neither the CPU, the RAM or my GPU. Even PBO is disabled. My RAM uses its XMP-profile
(3000MHz CL16-18-18-38) and never lead into any errors or annoying bugs within this machine. Of course, because the whole
system is based on virtualization, [SVM](../explanations/glossary.md#SVM) is enabled and so is [IOMMU](../explanations/glossary.md#IOMMU) too. Because of older approaches I do know that I might have to try [CSM](../explanations/glossary.md#CSM) enabeld too. Most of the other settings are either set default or are not that important.

# Displays
I will use 3 monitors for my setup. One will only be used by my Linux host distro. It's a 21:9 widescreen monitor which should be perfect for productive tasks and daily usage.
Another one is for VMs only and should provide more than enough space for a single VM although it's a 16:9 FHD display.
Last but not least, I will definitely use a WQHD 16:9 monitor which will either be used by my host or guest os depending whether a VM runs at the moment and if it's really needed for it. I can simply switch just switch the ouput signal between my
first and seconds GPU.  
Since my displays are side by side, I will probably create two places on my desk with a mouse and a keyboard. Due to the fact that I want to create a gaming Windows VM and a position in front of my gaming monitor is more
suitable than only one place in the middle of all monitors.
