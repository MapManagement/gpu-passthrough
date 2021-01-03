# Setup

## Components
**Mainboard:** Gigabyte B550 Aorus Pro  
**CPU:** Ryzen 9 3900X  
**RAM:** 32GiB 3000MHz CL16
**PSU:** BitFenix Formula Gold 750W  
**Cooler:** Arctic Liquid Freezer II 360  
**Host GPU:** PowerColor Radeon RX 550 Red Dragon  
in first PCIe slot  
**Guest GPU:** Sapphire Pulse Radeon RX Vega 56  
in second PCIe slot  
**Storage**: SSD 240GiB, 3TiB HDD, (120GiB SSD, 1TiB HDD, 256GiB NVMe)  
For testing, only the first two drives will be used. When going for an actual daily machine, I will probably use all of
them.

## BIOS Settings
None of my parts are overclocked, neither the CPU, the RAM or my GPU. Even PBO is disabled. My RAM uses its XMP-profile
(3000MHz CL16-18-18-38) and never lead into any errors or annoying bugs within this machine. Of course, because the whole
system is based on virtualization, [SVM](../explanations/glossary.md#SVM) is enabled and so is [IOMMU](../explanations/glossary.md#IOMMU) too. Because of older approaches I do know that I might have to try [CSM](../explanations/glossary.md#CSM) enabeld too. Most of the other settings are either set default or are not that important.
