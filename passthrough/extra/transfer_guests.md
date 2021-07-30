# Transfer Guests
## Why should I transfer my guests?
This topic is not only about people that plan on switching to another Linux distro, it's also a kind of securing your guests. You can always lose your data
and if so, most people can protect themselves by simply just making backups from time to time. If think that no one at all wants to restore all the work that has to
be put into creating VMs and their data. So this file will be about saving your guests properly to be able to use them as fast as possible when switching to another
machine, distribution or doing anything else.

## What should be transferred?
Ideally you transfer your guest configurations (XML files) and the corresponding image and storage files (depending on the file format you chose earlier).
Your configuration files is everything you can edit in the virt-manager. For example the amount of RAM or passed PCI devices are stored within those XML files.
Of course, you cannot use them in the way they are after exporting them to another build. Some details need to be changed but it's still much less work than trying
to replicate multiple configuration by hand. You can already see the XML file in virt-manager instead of the standard GUI:
![](/resources/xml-configuration.png)
Addtionally to your guest configuration you will probably need the actual data of your guest. In other words, you definitely want your virtual disks saved as well.
In contrast to relatively small XML files, virtual disks need much more space. So if you plan on exporting them to another machine, be aware of the amount of storage
you still have. It won't be a big deal if you only need to backup a few guests but the more VMs you create over time, the more storage will be need logically.

## How can I save them?
Saving your XML files is the quite easy. There is an interface for your terminal called
[virsh](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/virtualization/chap-virtualization-managing_guests_with_virsh). By using virsh,
you can re-adjust your guests in many different ways, including the generation of XML files of your guests. You can use following command:
```
virsh dumpxml guest-name > guest-name.xml
```
You will get your guest configuration file out of it. Store it where- and however you want but keep in mind, it's always useful to have all configuration files multiple
times.  
Virtual disks can be treated like normal files. Store them on your NAS, in a cloud or wherever you want to. It's similar to the configuration files but as I already said
they need much more space since they contain all the data of your guest. In my case, all guest images are stored on a dedicated SSD. From time to time I copy all data of
this SSD to my local server. And even if I wanted to switch to another Linux distribution (probably Arch :D), I can simply just mount the SSD and I'll be fine.


## Using on other machines
The transfer of your guest images should already be clear. The question is, how can I "import" my XML configurations via virsh. There's also a simple command:
```
virsh define guest-name.xml
```
It will create a guest for you. Check if everything worked by starting virt-manager.
