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

 ## How can I save them?