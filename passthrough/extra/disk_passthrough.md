# Disk Passthrough
I already listed the topic of passing a whole disk to one of my guests in [ideas](../../ideas.md). I didn't
need the increased speed and performance of the disk but wanted to create a dual boot **and** a VM at once. This
means that I should be able to access my guest within Linux through the virt-manager and also by directly booting
into it. After a long time of researching and testing around, I could reach my goal without any annoying
problems. Following text will explain how to pass a whole disk to a guest, how to configure the guest to be able
to boot into it and how to maintain your installation properly.