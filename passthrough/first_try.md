# First try of the build
The first time I tried to create a Windows virtual machine with a dedicated GPU on this computer was only a 
simple test. I wanted to check how easy it is to create such a VM using Pop!_OS and what I have to expect and
take care of when I set up my daily machine. Following section explain my approach and experiences.

# Binding the GPU
I have to be honest that the very first time I tried to bind the GPU I recognized that my IOMMU groups are not
quite suitbale if I want to use the second PCIe slot for my guest GPU. A detailed version of the problem can be
found [here](../problems.md##B550-board-and-its-IOMMU-groups). Therefore I swapped my GPUs and used the guest GPU in the first slot. My
explnation will start at this point.  
So after booting into Linux I checked my IOMMU groups and was quite relieved as I saw the result of changing
the PCIe slot. I installed [all packages](prerequisites.md#Needed-Packages) that I needed and started the binding.